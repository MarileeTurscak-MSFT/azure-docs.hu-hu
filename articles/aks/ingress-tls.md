---
title: Azure Kubernetes Service (AKS)-fürt létrehozása egy HTTPS-bejövő
description: Megtudhatja, hogyan telepítheti és konfigurálhatja egy nginx-et bejövőforgalom-vezérlőt, amely most titkosításához használja automatikus SSL-tanúsítvány létrehozása Azure Kubernetes Service (AKS)-fürtben.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/30/2018
ms.author: iainfou
ms.openlocfilehash: 87ea88ad84114c4059e9a461beedb656c1d66bf5
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356651"
---
# <a name="create-an-https-ingress-controller-on-azure-kubernetes-service-aks"></a>Hozzon létre egy HTTPS bejövőforgalom-vezérlőt az Azure Kubernetes Service (AKS)

Bejövőforgalom-vezérlőjéhez olyan szoftver, amely biztosítja a fordított proxy, konfigurálható forgalom-útválasztást és a TLS-lezárást biztosít Kubernetes-szolgáltatás. Kubernetes bejövő erőforrások segítségével konfigurálhatja a bejövő szabályok és útvonalak a Kubernetes-szolgáltatás. A bejövőforgalom-vezérlőt, és a bejövő szabályok használatával az egyetlen IP-cím irányíthatja a forgalmat több szolgáltatást a Kubernetes-fürtben használható.

Ez a cikk bemutatja, hogyan helyezhet üzembe a [NGINX bejövőforgalom-vezérlőjéhez] [ nginx-ingress] Azure Kubernetes Service (AKS)-fürtben. A [cert-kezelő] [ cert-manager] projekt automatikus létrehozásához és konfigurálásához használatos [hozzunk titkosítása] [ lets-encrypt] tanúsítványokat. Végül a két alkalmazás az AKS-fürtöt, amelyek mindegyike érhető el egyetlen IP-címen keresztül a futnak.

További lehetőségek:

- [Hozzon létre egy alapszintű bejövőforgalom-vezérlőjéhez külső hálózatok közötti kapcsolatokkal][aks-ingress-basic]
- [A HTTP-kérelem útválasztási bővítmény engedélyezése][aks-http-app-routing]
- [Hozzon létre egy bejövőforgalom-vezérlőt, amely egy belső, saját hálózat és IP-cím][aks-ingress-internal]
- [Hozzon létre bejövőforgalom-vezérlőjéhez statikus nyilvános IP-címet, és nézzük titkosítása automatikusan létrehozni a TLS-tanúsítványok konfigurálása][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Előkészületek

Ez a cikk a Helm használatával az NGINX bejövőforgalom-vezérlőt, a tanúsítvány-kezelő és a egy mintául szolgáló webalkalmazás telepítése. Szüksége lesz a Helm belül az AKS-fürt inicializálva, és a tiller valóban szolgáltatásfiók használatával. Győződjön meg arról, hogy a Helm legújabb kiadását használja. Frissítési utasításokért lásd: a [Helm telepítése docs][helm-install]. Konfigurálása és a Helm használatával további információkért lásd: [telepíthet alkalmazásokat a Helm használatával az Azure Kubernetes Service (AKS)][use-helm].

Ez a cikk is szükséges, hogy futnak-e az Azure CLI 2.0.41 verzió vagy újabb. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI telepítése][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Hozzon létre egy bejövőforgalom-vezérlőt

A bejövőforgalom-vezérlőjéhez létrehozásához használja `Helm` telepítéséhez *nginx-belépő*.

> [!TIP]
> A következő példa telepíti a bejövőforgalom-vezérlőt az `kube-system` névtér. Megadhat egy másik névtér a saját környezetben, ha szükséges. Ha az AKS-fürt nem RBAC engedélyezve, vegye fel `--set rbac.create=false` parancsok.

```console
helm install stable/nginx-ingress --namespace kube-system
```

A telepítés során az Azure nyilvános IP-cím a bejövőforgalom-vezérlőjéhez jön létre. A nyilvános IP-cím statikus-élettartamát a bejövőforgalom-vezérlőjéhez tartozó. Ha törli a bejövőforgalom-vezérlőt, a nyilvános IP-cím hozzárendelése elvész. Ezután hozzon létre egy további bejövőforgalom-vezérlőt, ha egy új nyilvános IP-cím van hozzárendelve. Ha szeretné megőrizni a nyilvános IP-cím használatát, akkor ehelyett [hozzon létre egy statikus nyilvános IP-cím bejövőforgalom-vezérlőjéhez][aks-ingress-static-tls].

Nyilvános IP-címének lekéréséhez használja a `kubectl get service` parancsot. Az IP-cím hozzá kell rendelni a szolgáltatás pár percet vesz igénybe.

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                       TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
eager-crab-nginx-ingress-controller        LoadBalancer   10.0.182.160   51.145.155.210  80:30920/TCP,443:30426/TCP   20m
eager-crab-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Bejövő szabályok még hozták létre. Keresse meg a nyilvános IP-címre, ha az NGINX bejövőforgalom-vezérlőjéhez tartozó alapértelmezett 404-es oldal jelenik meg.

## <a name="configure-a-dns-name"></a>Konfigurálja a DNS-név

A HTTPS-tanúsítványok megfelelően működjön konfigurálja a bejövő forgalom vezérlő IP-cím teljes Tartománynevet. Frissítse a bejövőforgalom-vezérlőt, és a egy egyedi nevet, amely a teljes Tartománynevet használni kívánt IP-címét az alábbi parancsfájlt:

```console
#!/bin/bash

# Public IP address of your ingress controller
IP="51.145.155.210"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get the resource-id of the public ip
PUBLICIPID=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv)

# Update public ip address with DNS name
az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
```

A bejövőforgalom-vezérlőjéhez most már a teljes tartománynév keresztül érhető el.

## <a name="install-cert-manager"></a>Tanúsítvány-kezelőjének telepítése

Az NGINX bejövőforgalom-vezérlőjéhez támogatja a TLS megszüntetése. Számos módon lekérésére és tanúsítványok konfigurálása HTTPS használatára. Használatát mutatja be ez a cikk [cert-kezelő][cert-manager], amely biztosítja az automatikus [titkosítása lehetővé teszi, hogy] [ lets-encrypt] tanúsítvány generálása és felügyeleti funkciókat.

> [!NOTE]
> Ez a cikk a `staging` környezetet hozzunk titkosítása. Használja az éles környezetekben, `letsencrypt-prod` és `https://acme-v02.api.letsencrypt.org/directory` az erőforrás-definíciókban, és a Helm-diagram telepítésekor.

Az RBAC-t fürtben a tanúsítvány-kezelő tartományvezérlő telepítéséhez használja a következő `helm install` parancsot:

```console
helm install stable/cert-manager --set ingressShim.defaultIssuerName=letsencrypt-staging --set ingressShim.defaultIssuerKind=ClusterIssuer
```

Ha a fürt nem engedélyezett az RBAC, ehelyett a következő paranccsal:

```console
helm install stable/cert-manager \
  --set ingressShim.defaultIssuerName=letsencrypt-staging \
  --set ingressShim.defaultIssuerKind=ClusterIssuer \
  --set rbac.create=false \
  --set serviceAccount.create=false
```

A tanúsítvány-kezelő konfigurációjának további információkért lásd: a [cert-kezelő projekt][cert-manager].

## <a name="create-a-ca-cluster-issuer"></a>Hozzon létre egy fürtöt hitelesítésszolgáltató-kiállítói

Mielőtt adhatók ki tanúsítványok, tanúsítvány-manager csak egy [kibocsátó] [ cert-manager-issuer] vagy [ClusterIssuer] [ cert-manager-cluster-issuer] erőforrás. Ezek a Kubernetes-erőforrást megegyeznek a funkciót, azonban `Issuer` működik egy egységes névtérben, és `ClusterIssuer` összes névtér működik. További információkért lásd: a [cert-kezelő kibocsátó] [ cert-manager-issuer] dokumentációját.

Például hozzon létre egy fürtöt az issuer `cluster-issuer.yaml`, a következő példa jegyzék használatával. Frissítse az e-mail-cím az érvényes cím a szervezet:

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-staging
    http01: {}
```

A kibocsátó létrehozásához használja a `kubectl apply -f cluster-issuer.yaml` parancsot.

```
$ kubectl apply -f cluster-issuer.yaml

clusterissuer.certmanager.k8s.io/letsencrypt-staging created
```

## <a name="create-a-certificate-object"></a>Tanúsítvány-objektum létrehozása

Ezután egy tanúsítvány erőforrást kell létrehozni. A tanúsítvány erőforrás határozza meg a kívánt X.509-tanúsítvány. További információkért lásd: [cert-kezelő tanúsítványok][cert-manager-certificates].

Hozzon létre például a tanúsítvány erőforrás `certificates.yaml`, az a következő példa jegyzékfájlt. Frissítés a *dnsNames* és *tartományok* az előző lépésben létrehozott DNS-nevével. Ha egy belső bejövőforgalom-vezérlőt használ, adja meg a szolgáltatás belső DNS-nevét.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: Certificate
metadata:
  name: tls-secret
spec:
  secretName: tls-secret
  dnsNames:
  - demo-aks-ingress.eastus.cloudapp.azure.com
  acme:
    config:
    - http01:
        ingressClass: nginx
      domains:
      - demo-aks-ingress.eastus.cloudapp.azure.com
  issuerRef:
    name: letsencrypt-staging
    kind: ClusterIssuer
```

A tanúsítvány-erőforrás létrehozásához használja a `kubectl apply -f certificates.yaml` parancsot.

```
$ kubectl apply -f certificates.yaml

certificate.certmanager.k8s.io/tls-secret created
```

## <a name="run-demo-applications"></a>Bemutató alkalmazások futtatása

Bejövőforgalom-vezérlőt, és a egy tanúsítvány-kezelési megoldás konfigurálva. Most tegyük a futtatási két bemutató az AKS-fürt az alkalmazásokat. Ebben a példában a Helm szolgál egy egyszerű "Hello world" alkalmazás két példány üzembe helyezéséhez.

A minta Helm-diagramok telepítése előtt vegye fel az Azure-minták tárhelyet a Helm-környezetben a következő:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Az első bemutató alkalmazás létrehozása egy Helm-diagramot a következő paranccsal:

```console
helm install azure-samples/aks-helloworld
```

Egy második példányt a bemutató alkalmazás telepítése. A második példány, adja meg az új címet, hogy a két alkalmazás vizuálisan elkülönülnek. Is adjon meg egy egyedi szolgáltatásnevet:

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Bejövő útvonal létrehozása

Mindkét alkalmazás most már futtat egy Kubernetes-fürtöt, azonban egy szolgáltatással típusú konfigurálják `ClusterIP`. Mint ilyen az alkalmazás nem az interneten. Ahhoz, hogy azok nyilvánosan elérhető, hozzon létre egy Kubernetes bejövő erőforrást. A bejövő forgalom erőforrás konfigurálja a szabályokat, amelyek a két alkalmazás egyik irányíthatja a forgalmat.

A következő példában a forgalmat a címre `https://demo-aks-ingress.eastus.cloudapp.azure.com/` irányítja a rendszer a szolgáltatás nevű `aks-helloworld`. A cím forgalmát `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` irányítja a rendszer a `ingress-demo` szolgáltatás. Frissítés a *gazdagépek* és *gazdagép* az előző lépésben létrehozott DNS-nevével.

Hozzon létre egy fájlt `hello-world-ingress.yaml` , és másolja az alábbi példában YAML.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    certmanager.k8s.io/cluster-issuer: letsencrypt-staging
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Létrehozhatja a bejövő forgalom erőforrás a `kubectl apply -f hello-world-ingress.yaml` parancsot.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-configuration"></a>A konfiguráció teszteléséhez a bejövő forgalom

Például a Kubernetes bejövő tartományvezérlő teljes Tartománynevét egy webböngészőben nyissa meg *https://demo-aks-ingress.eastus.cloudapp.azure.com*.

Mivel ezek a példák a `letsencrypt-staging`, a böngésző által kiállított SSL-tanúsítvány nem megbízható. Fogadja el a figyelmeztetést, az alkalmazás továbbra is. Ez a tanúsítvány adatait jeleníti meg *hamis LE köztes X1* tanúsítványt most titkosítása. Ez a tanúsítvány hamis azt jelzi, hogy `cert-manager` képesek megfelelően feldolgozni a kérelmet, és a egy tanúsítványt kapott a szolgáltató:

![Most a átmeneti tanúsítvány titkosítása](media/ingress/staging-certificate.png)

Ha módosítja a hozzunk titkosítására használandó `prod` helyett `staging`, megbízható által kibocsátott tanúsítványt most titkosítására szolgál, az alábbi példában látható módon:

![Most a tanúsítvány titkosítása](media/ingress/certificate.png)

A webböngésző a bemutató alkalmazás látható:

![Egy példa az alkalmazások](media/ingress/app-one.png)

Most adja hozzá a */hello-world-two* elérési útját a teljes Tartománynevét, például *https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two*. A második bemutató alkalmazás és az egyéni cím jelenik meg:

![Példa az alkalmazások két](media/ingress/app-two.png)

## <a name="next-steps"></a>További lépések

Ez a cikk tartalmaz néhány külső összetevők az aks-ben. Ezek az összetevők kapcsolatos további információkért tekintse meg a következő projekt oldalakat:

- [Helm CLI][helm-cli]
- [Az NGINX bejövőforgalom-vezérlőt][nginx-ingress]
- [tanúsítvány-kezelő][cert-manager]

További lehetőségek:

- [Hozzon létre egy alapszintű bejövőforgalom-vezérlőjéhez külső hálózatok közötti kapcsolatokkal][aks-ingress-basic]
- [A HTTP-kérelem útválasztási bővítmény engedélyezése][aks-http-app-routing]
- [Hozzon létre egy bejövőforgalom-vezérlőt, amely egy belső, saját hálózat és IP-cím][aks-ingress-internal]
- [Hozzon létre bejövőforgalom-vezérlőjéhez statikus nyilvános IP-címet, és nézzük titkosítása automatikusan létrehozni a TLS-tanúsítványok konfigurálása][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-http-app-routing]: http-application-routing.md
