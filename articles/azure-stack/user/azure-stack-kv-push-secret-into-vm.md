---
title: Virtuális gép üzembe helyezése az Azure Stacken biztonságosan tárolt tanúsítvánnyal |} A Microsoft Docs
description: Ismerje meg, hogyan helyezhet üzembe egy virtuális gépet, és küldje le azt a tanúsítványt a key vault használatával az Azure Stackben
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 46590eb1-1746-4ecf-a9e5-41609fde8e89
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/15/2018
ms.author: sethm
ms.openlocfilehash: aef706d18d558f5fe321735c7f93361a5ef50606
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/16/2018
ms.locfileid: "42139319"
---
# <a name="create-a-virtual-machine-and-install-a-certificate-retrieved-from-an-azure-stack-key-vault"></a>Hozzon létre egy virtuális gépet, és a egy Azure Stack key vault lekért tanúsítvány telepítése

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

Ismerje meg, hogyan hozhat létre egy Azure Stack virtuális gépet (VM) telepítve van a key vault-tanúsítvánnyal.

## <a name="overview"></a>Áttekintés

A tanúsítványokat helyzetekben, például az Active Directory hitelesítő vagy a webes forgalom titkosításához használja. Tanúsítványok biztonságosan tárolhatja az Azure Stack a key vault titkos kódként. Az Azure Stack Key Vault használatának előnyei a következők:

* Egy parancsfájl, a parancssori előzményekben vagy a sablon a tanúsítványok nem érhetőek el.
* A tanúsítványkezelési folyamatot zökkenőmentesen történik.
* A tanúsítványok hozzáférő kulcsok hozzáférése van.

### <a name="process-description"></a>Folyamat – leírás

A push-tanúsítványt a virtuális gép szükséges folyamat a következő lépésekből áll:

1. Hozzon létre egy Key Vault titkos.
2. Frissítse az azuredeploy.parameters.json fájlra.
3. A sablon üzembe helyezése

> [!NOTE]
> Ezeket a lépéseket az Azure Stack fejlesztői készletet, vagy egy külső ügyfél is használhatja, ha VPN-kapcsolaton keresztül kapcsolódik.

## <a name="prerequisites"></a>Előfeltételek

* Az ajánlat, amely tartalmazza a Key Vault szolgáltatás elő kell fizetnie.
* [Az Azure Stack PowerShell telepítése.](azure-stack-powershell-install.md)
* [Az Azure Stack felhasználói PowerShell-környezet konfigurálása](azure-stack-powershell-configure-user.md)

## <a name="create-a-key-vault-secret"></a>A Key Vault titkos kód létrehozása

A következő szkriptet a .pfx formátumú tanúsítványt hoz létre egy kulcstartót hoz létre és tárolja a tanúsítványt a key vault titkos kulcs.

> [!IMPORTANT]
> Kell használnia a `-EnabledForDeployment` paraméter a key vault létrehozása során. Ez a paraméter gondoskodik arról, hogy a key vault az Azure Resource Manager-sablonok lehet hivatkozni.

```powershell

# Create a certificate in the .pfx format
New-SelfSignedCertificate `
  -certstorelocation cert:\LocalMachine\My `
  -dnsname contoso.microsoft.com

$pwd = ConvertTo-SecureString `
  -String "<Password used to export the certificate>" `
  -Force `
  -AsPlainText

Export-PfxCertificate `
  -cert "cert:\localMachine\my\<Certificate Thumbprint that was created in the previous step>" `
  -FilePath "<Fully qualified path where the exported certificate can be stored>" `
  -Password $pwd

# Create a key vault and upload the certificate into the key vault as a secret
$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "servicecert"
$fileName = "<Fully qualified path where the exported certificate can be stored>"
$certPassword = "<Password used to export the certificate>"

$fileContentBytes = get-content $fileName `
  -Encoding Byte

$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
$jsonObject = @"
{
"data": "$filecontentencoded",
"dataType" :"pfx",
"password": "$certPassword"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -sku standard `
  -EnabledForDeployment

$secret = ConvertTo-SecureString `
  -String $jsonEncoded `
  -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
   -SecretValue $secret

```

Amikor az előző parancsfájlt futtat, akkor a kimenete a titkos URI tartalmazza. Jegyezze fel ezt az URI. Arra mutató hivatkozás a rendelkezik a [Push-tanúsítvány a Windows Resource Manager-sablon](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate). Töltse le a [vm-push-tanúsítvány-windows sablon](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate) mappát a fejlesztői számítógépre. Ebben a mappában találhatók a `azuredeploy.json` és `azuredeploy.parameters.json` fájlokat, amelyek a következő lépésben szüksége lesz.

Módosítsa a `azuredeploy.parameters.json` fájl a környezet értékeknek megfelelően. Érdeklik paraméterei a következők: a tároló nevére, a tár erőforráscsoportja és a titkos kulcs URI-t (mivel az előző parancsfájl által létrehozott). A következő fájl egy példát:

## <a name="update-the-azuredeployparametersjson-file"></a>Frissítés a azuredeploy.parameters.json fájlhoz

Frissítse az azuredeploy.parameters.json fájlra a vaultName, titkos URI, VmName és más értékek alapján a környezetben. A következő JSON-fájlt a sablon paraméterfájljának egy példát mutat be:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "newStorageAccountName": {
      "value": "kvstorage01"
    },
    "vmName": {
      "value": "VM1"
    },
    "vmSize": {
      "value": "Standard_D1_v2"
    },
    "adminUserName": {
      "value": "demouser"
    },
    "adminPassword": {
      "value": "demouser@123"
    },
    "vaultName": {
      "value": "contosovault"
    },
    "vaultResourceGroup": {
      "value": "contosovaultrg"
    },
    "secretUrlWithVersion": {
      "value": "https://testkv001.vault.local.azurestack.external/secrets/testcert002/82afeeb84f4442329ce06593502e7840"
    }
  }
}
```

## <a name="deploy-the-template"></a>A sablon üzembe helyezése

A sablon üzembe helyezése a következő PowerShell-parancsfájl használatával:

```powershell
# Deploy a Resource Manager template to create a VM and push the secret onto it
New-AzureRmResourceGroupDeployment `
  -Name KVDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Ha a sablon üzembe helyezése sikeresen befejeződött, a következő kimenetet eredményezi:

![Sablon üzembe helyezés eredményei](media/azure-stack-kv-push-secret-into-vm/deployment-output.png)

Az Azure Stack leküldi a tanúsítványt a virtuális gép üzembe helyezése során. A tanúsítvány helye attól függ, hogy a virtuális gép operációs rendszer:

* A Windows a helyi gépen lévő tanúsítvány helyre, a tanúsítvány áruházhoz, a felhasználó által biztosított hozzáadta a tanúsítványt.
* A Linux, a tanúsítványt a /var/lib/waagent tartományhoz, a fájl neve alá kerül &lt;UppercaseThumbprint&gt;.crt a X509 a tanúsítványfájl és &lt;UppercaseThumbprint&gt;.prv a titkos kulcs .

## <a name="retire-certificates"></a>Tanúsítványok visszavonása

Tanúsítványok eltávolítása a felügyeleti folyamat részét képezi. Nem lehet törölni a tanúsítványt a régebbi verzióját, de a segítségével letilthatja a `Set-AzureKeyVaultSecretAttribute` parancsmagot.

Az alábbi példa bemutatja, hogyan tilthatja le egy tanúsítvány. A saját értékeit használja a **VaultName**, **neve**, és **verzió** paramétereket.

```powershell
Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0
```

## <a name="next-steps"></a>További lépések

* [Virtuális gép üzembe helyezése Key Vault-jelszóval](azure-stack-kv-deploy-vm-with-secret.md)
* [Key Vault elérése érdekében alkalmazás engedélyezése](azure-stack-kv-sample-app.md)
