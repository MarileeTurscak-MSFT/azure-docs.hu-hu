---
title: Hozzon létre egy belső hálózattal bejövőforgalom-vezérlőt az Azure Kubernetes Service (AKS)
description: Ismerje meg, hogyan telepítse és konfigurálja az NGINX bejövőforgalom-vezérlőjéhez belső, saját hálózat Azure Kubernetes Service (AKS)-fürtben.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/30/2018
ms.author: iainfou
ms.openlocfilehash: 76ad9d21f7b328e7f201d227cdd9ace51c62a3fd
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/11/2018
ms.locfileid: "44357277"
---
# <a name="create-an-ingress-controller-to-an-internal-virtual-network-in-azure-kubernetes-service-aks"></a>Hozzon létre egy bejövőforgalom-vezérlőt, a belső virtuális hálózathoz az Azure Kubernetes Service (AKS)

Bejövőforgalom-vezérlőjéhez olyan szoftver, amely biztosítja a fordított proxy, konfigurálható forgalom-útválasztást és a TLS-lezárást biztosít Kubernetes-szolgáltatás. Kubernetes bejövő erőforrások segítségével konfigurálhatja a bejövő szabályok és útvonalak a Kubernetes-szolgáltatás. A bejövőforgalom-vezérlőt, és a bejövő szabályok használatával az egyetlen IP-cím irányíthatja a forgalmat több szolgáltatást a Kubernetes-fürtben használható.

Ez a cikk bemutatja, hogyan helyezhet üzembe a [NGINX bejövőforgalom-vezérlőjéhez] [ nginx-ingress] Azure Kubernetes Service (AKS)-fürtben. A bejövőforgalom-vezérlőt egy belső, privát virtuális hálózatot és IP-cím van konfigurálva. Külső hozzáférés nem engedélyezett. Két alkalmazás futtatása a az AKS-fürtöt, amelyek mindegyike érhető el az egyetlen IP-címen keresztül.

További lehetőségek:

- [Hozzon létre egy alapszintű bejövőforgalom-vezérlőjéhez külső hálózatok közötti kapcsolatokkal][aks-ingress-basic]
- [A HTTP-kérelem útválasztási bővítmény engedélyezése][aks-http-app-routing]
- [Hozzon létre egy bejövőforgalom-vezérlőjéhez dinamikus nyilvános IP-cím, és nézzük titkosítása automatikusan létrehozni a TLS-tanúsítványok konfigurálása][aks-ingress-tls]
- [Hozzon létre bejövőforgalom-vezérlőjéhez statikus nyilvános IP-címet, és nézzük titkosítása automatikusan létrehozni a TLS-tanúsítványok konfigurálása][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Előkészületek

Ez a cikk a Helm használatával az NGINX bejövőforgalom-vezérlőt, a tanúsítvány-kezelő és a egy mintául szolgáló webalkalmazás telepítése. Szüksége lesz a Helm belül az AKS-fürt inicializálva, és a tiller valóban szolgáltatásfiók használatával. Konfigurálása és a Helm használatával további információkért lásd: [telepíthet alkalmazásokat a Helm használatával az Azure Kubernetes Service (AKS)][use-helm].

Ez a cikk is szükséges, hogy futnak-e az Azure CLI 2.0.41 verzió vagy újabb. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI telepítése][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Hozzon létre egy bejövőforgalom-vezérlőt

Alapértelmezés szerint létrejön egy nginx-et bejövőforgalom-vezérlőt egy dinamikus nyilvános IP-cím hozzárendelése. Általános konfigurációs követelmény, hogy egy belső, saját hálózat és IP-címet. Ez a megközelítés lehetővé teszi a belső felhasználók, sem külső hozzáféréssel rendelkező szolgáltatások elérésének korlátozása.

Hozzon létre egy fájlt *belső ingress.yaml* használatával a következő példa jegyzékfájl. Ez a példa hozzárendeli *10.240.0.42* , a *loadBalancerIP* erőforrás. Adja meg a saját belső IP-címet a bejövőforgalom-vezérlőjéhez való használatra. Győződjön meg arról, hogy az IP-címet nem már használatban van a virtuális hálózaton belül.

```yaml
controller:
  service:
    loadBalancerIP: 10.240.0.42
    annotations:
      service.beta.kubernetes.io/azure-load-balancer-internal: "true"
```

Már üzembe helyezheti a *nginx-belépő* Helm-diagramot. Az előző lépésben létrehozott jegyzékfájlt használ, adja hozzá a `-f internal-ingress.yaml` paramétert:

> [!TIP]
> A következő példa telepíti a bejövőforgalom-vezérlőt az `kube-system` névtér. Megadhat egy másik névtér a saját környezetben, ha szükséges. Ha az AKS-fürt nem RBAC engedélyezve, vegye fel `--set rbac.create=false` parancsok.

```console
helm install stable/nginx-ingress --namespace kube-system -f internal-ingress.yaml
```

A terheléselosztó Kubernetes szolgáltatás az NGINX bejövőforgalom-vezérlőjéhez hoz létre, ha a belső IP-cím van hozzárendelve, az alábbi példa kimenetében látható módon:

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                              TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
alternating-coral-nginx-ingress-controller        LoadBalancer   10.0.97.109   10.240.0.42   80:31507/TCP,443:30707/TCP   1m
alternating-coral-nginx-ingress-default-backend   ClusterIP      10.0.134.66   <none>        80/TCP                       1m
```

Bejövő szabályok már hozott létre, így az NGINX bejövőforgalom-vezérlőjéhez tartozó alapértelmezett 404-es lap is megjelenik, ha a belső IP-címe. Bejövő szabályok a következő lépések vannak konfigurálva.

## <a name="run-demo-applications"></a>Bemutató alkalmazások futtatása

Szeretné látni működés közben bejövőforgalom-vezérlőt, futtassunk két bemutató alkalmazás az AKS-fürt található. Ebben a példában a Helm szolgál egy egyszerű "Hello world" alkalmazás két példány üzembe helyezéséhez.

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

Mindkét alkalmazás most már a Kubernetes-fürtöt futtat. Irányíthatja a forgalmat mindegyik alkalmazás, hozzon létre egy Kubernetes bejövő erőforrást. A bejövő forgalom erőforrás konfigurálja a szabályokat, amelyek a két alkalmazás egyik irányíthatja a forgalmat.

A következő példában a forgalmat a címre `http://10.240.0.42/` irányítja a rendszer a szolgáltatás nevű `aks-helloworld`. A cím forgalmát `http://10.240.0.42/hello-world-two` irányítja a rendszer a `ingress-demo` szolgáltatás.

Hozzon létre egy fájlt `hello-world-ingress.yaml` , és másolja az alábbi példában YAML.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
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

## <a name="test-the-ingress-controller"></a>A bejövőforgalom-vezérlőjéhez tesztelése

Tesztelheti a útvonalait a bejövőforgalom-vezérlőt, keresse meg a két alkalmazás egy webes ügyféllel. Szükség esetén gyorsan tesztelheti az csak belső funkciói az AKS-fürtöt podján. Hozzon létre egy teszt pod, és egy terminál-munkamenetben csatlakoztatása:

```console
kubectl run -it --rm aks-ingress-test --image=debian
```

Telepítés `curl` be a pod `apt-get`:

```console
apt-get update && apt-get install -y curl
```

Most már elérhető a cím a Kubernetes bejövő vezérlő használatával `curl`, mint például *http://10.240.0.42*. Adja meg, amikor központilag telepítette a bejövőforgalom-vezérlőt, ez a cikk az első lépésben meg a saját belső IP-cím.

```console
curl -L http://10.240.0.42
```

Nincsenek további elérési út lett megadva a címét, ezért a bejövőforgalom-vezérlőt az alapértelmezett a */* útvonalat. Az első bemutató alkalmazás adja vissza, a következő sűrített példához kimenetben látható módon:

```
$ curl -L 10.240.0.42

<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>Welcome to Azure Kubernetes Service (AKS)</title>
[...]
```

Adjon hozzá */hello-world-two* elérési útját a címet, például *http://10.240.0.42/hello-world-two*. A második bemutató alkalmazás és az egyéni cím ad vissza, a következő sűrített példához kimenetben látható módon:

```
$ curl -L -k http://10.240.0.42/hello-world-two

<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>AKS Ingress Demo</title>
[...]
```

## <a name="next-steps"></a>További lépések

Ez a cikk tartalmaz néhány külső összetevők az aks-ben. Ezek az összetevők kapcsolatos további információkért tekintse meg a következő projekt oldalakat:

- [Helm CLI][helm-cli]
- [Az NGINX bejövőforgalom-vezérlőt][nginx-ingress]

További lehetőségek:

- [Hozzon létre egy alapszintű bejövőforgalom-vezérlőjéhez külső hálózatok közötti kapcsolatokkal][aks-ingress-basic]
- [A HTTP-kérelem útválasztási bővítmény engedélyezése][aks-http-app-routing]
- [Hozzon létre egy bejövőforgalom-vezérlőjéhez dinamikus nyilvános IP-cím, és nézzük titkosítása automatikusan létrehozni a TLS-tanúsítványok konfigurálása][aks-ingress-tls]
- [Hozzon létre bejövőforgalom-vezérlőjéhez statikus nyilvános IP-címet, és nézzük titkosítása automatikusan létrehozni a TLS-tanúsítványok konfigurálása][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
