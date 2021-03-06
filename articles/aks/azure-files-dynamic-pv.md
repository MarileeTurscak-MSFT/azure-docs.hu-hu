---
title: Az Azure használata az aks-sel
description: Azure-lemezek használata az aks-sel
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/15/2018
ms.author: iainfou
ms.openlocfilehash: dfc9171f54effe3da7a0f13695ab233d561357d4
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/30/2018
ms.locfileid: "43285685"
---
# <a name="persistent-volumes-with-azure-files"></a>Az Azure-fájlok állandó kötetek

Tartós kötet olyan tárolási megoldás, amely létre lett hozva egy Kubernetes-fürt használatát. Tartós kötet segítségével egy vagy több podok és statikusan vagy dinamikusan létrehozhatók. Ez a dokumentum részletesen **dinamikus létrehozása** az Azure-fájlmegosztás kötetként állandó.

További információk a Kubernetes szolgáltatásban állandó kötetek, statikus létrehozását, beleértve: [Kubernetes állandó kötetek][kubernetes-volumes].

## <a name="create-a-storage-account"></a>Tárfiók létrehozása

Amikor dinamikusan hoz létre egy Azure-fájlmegosztást, egy Kubernetes-kötet, minden olyan storage-fiók is használható, mindaddig, amíg az AKS van **csomópont** erőforráscsoportot. Ez a csoport nincs a *MC_* előtagot, amely hozta létre az AKS-fürt erőforrásainak üzembe helyezése. Az erőforráscsoport nevét a [az aks show] [az-aks-show] paranccsal kaphat.

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Használja a [az tárfiók létrehozása] [ az-storage-account-create] parancsot a tárfiók létrehozásához.

Frissítés `--resource-group` az előző lépésben gyűjtött az erőforráscsoport nevére és `--name` egy Ön által választott nevére. Adja meg a saját egyedi tárfióknév:

```azurecli
az storage account create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name mystorageaccount --sku Standard_LRS
```

> [!NOTE]
> Azure Files jelenleg csak a standard szintű storage használata. Ha Premium storage szolgáltatást használja, a kötet nem tudja üzembe helyezése.

## <a name="create-a-storage-class"></a>Hozzon létre egy storage-osztály

Tárolási osztály hogyan jön létre egy Azure-fájlmegosztás meghatározására szolgál. Storage-fiók a osztály adható meg. Ha nincs megadva tárfiók, egy *skuName* és *hely* meg kell adni, és a társított erőforráscsoportokhoz összes storage-fiókok értékeli ki az egyezést. Az Azure Files számára a Kubernetes storage osztályai további információkért lásd: [Kubernetes Storage osztályai][kubernetes-storage-classes].

Hozzon létre egy fájlt `azure-file-sc.yaml` , és másolja a következő példa jegyzékfájlban. Frissítés a *storageAccount* értékét az előző lépésben létrehozott storage-fiókja nevére. További információ a *mountOptions*, tekintse meg a [csatlakoztatási beállítások] [ mount-options] szakaszban.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
  storageAccount: mystorageaccount
```

Hozzon létre a storage osztályt a [a kubectl a alkalmazni] [ kubectl-apply] parancsot:

```console
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-a-cluster-role-and-binding"></a>Fürtszerepkört és kötés létrehozása

AKS-fürtök használata a Kubernetes szerepköralapú hozzáférés-vezérlés (RBAC) korlát is elvégezhető műveletekre. *Szerepkörök* határozza meg az engedélyeket, és *kötések* azokat alkalmazni a kívánt felhasználókat. Ezeket a hozzárendeléseket is alkalmazható, egy adott névtérhez vagy az egész fürt között. További információkért lásd: [használatával RBAC-hitelesítés][kubernetes-rbac].

Ahhoz, hogy a szükséges tárolási erőforrások létrehozása az Azure platformon, hozzon létre egy *ClusterRole* és *ClusterRoleBinding*. Hozzon létre egy fájlt `azure-pvc-roles.yaml` másolja be a következő yaml-kódot:

```yaml
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  name: system:azure-cloud-provider
rules:
- apiGroups: ['']
  resources: ['secrets']
  verbs:     ['get','create']
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: system:azure-cloud-provider
roleRef:
  kind: ClusterRole
  apiGroup: rbac.authorization.k8s.io
  name: system:azure-cloud-provider
subjects:
- kind: ServiceAccount
  name: persistent-volume-binder
  namespace: kube-system
```

Az engedélyek hozzárendelése a [a kubectl a alkalmazni] [ kubectl-apply] parancsot:

```console
kubectl apply -f azure-pvc-roles.yaml
```

## <a name="create-a-persistent-volume-claim"></a>Tartós kötet jogcím létrehozása

Tartós kötet jogcím (PVC) a tárolási osztály objektum használatával dinamikusan telepíthet az Azure-fájlmegosztások. A következő yaml-kódot segítségével hozzon létre egy tartós kötet jogcím *5 GB-os* méretű *ReadWriteMany* hozzáférést. Hozzáférési mód további információkért lásd: a [Kubernetes tartós kötet] [ access-modes] dokumentációját.

Most hozzon létre egy fájlt `azure-file-pvc.yaml` másolja be a következő yaml-kódot. Győződjön meg arról, hogy a *storageClassName* megegyezik az előző lépésben létrehozott storage osztály:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

Hozzon létre a tartós kötet jogcímet a [a kubectl a alkalmazni] [ kubectl-apply] parancsot:

```console
kubectl apply -f azure-file-pvc.yaml
```

Ha befejeződött, a fájlmegosztást hoz létre. Kubernetes titkos kulcs is létrehoz, amely tartalmazza a kapcsolati adatokat és hitelesítő adatokat. Használhatja a [kubectl get] [ kubectl-get] parancsot a virtuális kapcsolat állapotának megtekintése:

```
$ kubectl get pvc azurefile

NAME        STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
azurefile   Bound     pvc-8436e62e-a0d9-11e5-8521-5a8664dc0477   5Gi        RWX            azurefile      5m
```

## <a name="use-the-persistent-volume"></a>Használja a tartós kötet

A következő yaml-kódot hoz létre, amely a tartós kötet jogcím podot *azurefile* , az Azure-fájlmegosztást, csatlakoztathatja a */mnt/azure* elérési útja.

Hozzon létre egy fájlt `azure-pvc-files.yaml`, és másolja be a következő yaml-kódot. Győződjön meg arról, hogy a *claimName* megegyezik a virtuális az előző lépésben létrehozott kapcsolat.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azurefile
```

A pod-létrehozása a [a kubectl a alkalmazni] [ kubectl-apply] parancsot.

```console
kubectl apply -f azure-pvc-files.yaml
```

Az Azure csatlakoztatott lemezzel most már rendelkezik egy futó pod a */mnt/azure* könyvtár. Ez a konfiguráció látható, amikor a pod-n keresztül vizsgálatával `kubectl describe pod mypod`. A következő sűrített példához kimenetet jeleníti meg a kötet csatlakoztatva van a tárolóban:

```
Containers:
  myfrontend:
    Container ID:   docker://053bc9c0df72232d755aa040bfba8b533fa696b123876108dec400e364d2523e
    Image:          nginx
    Image ID:       docker-pullable://nginx@sha256:d85914d547a6c92faa39ce7058bd7529baacab7e0cd4255442b04577c4d1f424
    State:          Running
      Started:      Wed, 15 Aug 2018 22:22:27 +0000
    Ready:          True
    Mounts:
      /mnt/azure from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-8rv4z (ro)
[...]
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  azurefile2
    ReadOnly:   false
[...]
```

## <a name="mount-options"></a>Csatlakoztatási beállítások

Alapértelmezett *fileMode* és *dirMode* érték azonos a szerepkörtárolók Kubernetes-verzió az alábbi táblázatban leírtak szerint.

| verzió: | érték |
| ---- | ---- |
| V1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| V1.8.6 vagy újabb | 0755 |
| v1.9.0 | 0700 |
| V1.9.1 vagy újabb | 0755 |

Ha használja a fürt verziójának megfelelő 1.8.5 vagy nagyobb, és dinamikusan hozza létre a tartós kötet egy olyan tárolási osztállyal, csatlakoztatási beállítások is megadhatók a tárolási osztály objektum. A következő példa készletek *0777*:

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
```

Ha használja a fürt verziójának megfelelő 1.8.5 vagy nagyobb és a tartós kötet objektum statikusan létrehozása csatlakoztatási lehetőségek meg kell adni a a *PersistentVolume* objektum. tartós kötet statikusan létrehozásával kapcsolatos további információkért lásd: [statikus állandó kötetek][pv-static].

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: azurefile
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  azureFile:
    secretName: azure-secret
    shareName: azurefile
    readOnly: false
  mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
  ```

Ha használja a fürt verziójának 1.8.0-as - 1.8.4-es verzióra, a biztonsági környezet adható a *felhasználó* értékre állítva *0*. A Pod biztonsági környezet további információkért lásd: [konfigurálása a biztonsági környezet][kubernetes-security-context].

## <a name="next-steps"></a>További lépések

További tudnivalók a Kubernetes Azure Files használatával állandó köteteket.

> [!div class="nextstepaction"]
> [Kubernetes-beépülő modul az Azure Files számára][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[pv-static]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/aks#az-aks-show
[az-storage-account-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[mount-options]: #mount-options
