---
title: Azure Backup használatával titkosított virtuális gépekhez tartozó Key Vault-kulcs és titkos kulcs visszaállítása
description: Ismerje meg, hogyan lehet visszaállítani a Key Vault-kulcs és titkos kulcsot az Azure Backup PowerShell-lel
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 08/28/2017
ms.author: sogup
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6ac3c3d8f2a5ae37f1d32f9781f0cdbec0b293e8
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/11/2018
ms.locfileid: "42054070"
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Azure Backup használatával titkosított virtuális gépekhez tartozó Key Vault-kulcs és titkos kulcs visszaállítása
Ez a cikk ismerteti visszaállításhoz, a titkosított Azure virtuális gépek Azure VM Backup használatával, ha a kulcs és titkos kulcs nem léteznek a key vaultban. Ezeket a lépéseket is használható, ha meg szeretné tartani a kulcs (kulcstitkosítási kulcs) és a titkos kulcs (BitLocker titkosítási kulcsot) egy külön példányát a visszaállított virtuális gép számára.

## <a name="prerequisites"></a>Előfeltételek
* **A titkosított virtuális gépek biztonsági mentése** – titkosított Azure virtuális gépek biztonsági mentése volt az Azure Backup használatából. Tekintse meg a cikk [kezelheti a biztonsági mentés és visszaállítás Azure virtuális gépek PowerShell-lel](backup-azure-vms-automation.md) részletei titkosított Azure virtuális gépek biztonsági mentését.
* **Az Azure Key Vault konfigurálásához** – győződjön meg arról, hogy, amelyhez kulcsok és titkos kulcsok kell vissza állítani őket a key vault már jelen. Tekintse meg a cikk [első lépései az Azure Key Vault](../key-vault/key-vault-get-started.md) key vault-felügyeleti részleteit.
* **Lemez visszaállítása** – győződjön meg arról, hogy a titkosított virtuális gépet a lemezek visszaállítását a visszaállítási feladat elindította [PowerShell-lépések](backup-azure-vms-automation.md#restore-an-azure-vm). Ennek oka az, ez a feladat a kulcsok és titkos kulcsok vissza kell állítani a titkosított virtuális gép tartalmazó tárfiókot hoz létre egy JSON-fájlt.

## <a name="get-key-and-secret-from-azure-backup"></a>Az Azure Backup kulcs és titkos kulcs lekérése

> [!NOTE]
> Ha a lemez vissza lett állítva a titkosított virtuális gép számára, győződjön meg arról, hogy:
> 1. $details fel van töltve a visszaállítási lemez feladat részletei között ismertetett módon [a visszaállítási lépések PowerShell a lemez szakasz](backup-azure-vms-automation.md#restore-an-azure-vm)
> 2. Virtuális gép helyreállított lemezekből csak kell létrehozni **kulcs és titkos kulcs visszaállítása a key vaulttal után**.
>
>

A lekérdezés a visszaállított lemez tulajdonságait a feladat részleteit.

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

Állítsa be az Azure storage-környezetet, és állítsa vissza a titkosított virtuális gép kulcs és titkos részleteit tartalmazó JSON-konfigurációs fájlt.

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a>Kulcs visszaállítása
A JSON-fájl jön létre a fent említett elérési utat, miután kulcsának blob-fájl létrehozása a JSON-ból, és hírcsatorna, helyezze vissza a kulcs-(KEK) a key vault-kulcs parancsmagot visszaállításához.

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a>Titkos kulcs visszaállítása
A titkos kód nevét és értékét, és továbbíthatja azt a fent létrehozott JSON-fájl használatával állítsa vissza a titok (rendelkeznek BEk-KEL) elhelyezése a key vault titkos parancsmag. **Ezeket a parancsmagokat használja, ha a virtuális gép titkosítását a rendszer rendelkeznek BEk-KEL és kek-KEL.**

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

Ha a virtuális gép **rendelkeznek BEk-KEL használatával csak titkosított**, hozzon létre a titkos kód fájlját a JSON-ból, és hírcsatorna, hogy a titkos kulcsot (rendelkeznek BEk-KEL) helyezi a key vaultban titkos parancsmag visszaállítása.

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
```

> [!NOTE]
> 1. Érték $secretname lekérheti hivatkozó $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl kimenetét, és a szöveg után titkos kulcsok használatával, vagy például kimeneti titkos URL-címe van https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 B3284AAA-DAAA-4AAA-B393-60CAA848AAAA pedig a titkos kód neve
> 2. A címke DiskEncryptionKeyFileName értéke ugyanaz, mint a titkos kód neve.
>
>

## <a name="create-virtual-machine-from-restored-disk"></a>Virtuális gép létrehozása a visszaállított lemezről
Ha készített biztonsági másolatot az Azure VM Backup használatával titkosított virtuális gép, a PowerShell-parancsmagok fent említett súgó-kulcs és titkos vissza visszaállítása a key vaulthoz. Után állítja vissza őket, tekintse meg a cikk [kezelheti a biztonsági mentés és visszaállítás Azure virtuális gépek PowerShell-lel](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) titkosított virtuális gépek létrehozása a visszaállított lemez, a kulcs és titkos kulcs.

## <a name="legacy-approach"></a>Hagyományos megközelítés
A fent említett módszer a helyreállítási pontok működne. Azonban a régebbi kulcs és titkos információk lekérésének helyreállítási pontból megközelítés lesz érvényes a 2017. július 11. rendelkeznek BEk-KEL és kek-KEL használatával titkosított virtuális gépeket a helyreállítási pontokat. Visszaállítási lemez feladat titkosított virtuális gépek használatának befejezése után [PowerShell-lépések](backup-azure-vms-automation.md#restore-an-azure-vm), győződjön meg arról, hogy $rp megjelenik egy érvényes értéket.

### <a name="restore-key"></a>Kulcs visszaállítása
Használja a következő parancsmagokat a kulcsadatokat (KEK) helyreállítási pontról, és hírcsatorna, helyezze vissza a key vault-kulcs parancsmagot visszaállításához.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a>Titkos kulcs visszaállítása
Az alábbi parancsmagok segítségével (rendelkeznek BEk-KEL) titkos információk beolvasása a helyreállítási pontról, és szeretné beállítani a titkos parancsmag segítségével helyezze vissza a key vault-hírcsatorna.

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> 1. $Secretname értéket lekérheti hivatkozó $rp1 kimenetét. KeyAndSecretDetails.SecretUrl és titkos kulcsok után pedig SMS használatával például kimeneti titkos kulcs URL-címe van https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 B3284AAA-DAAA-4AAA-B393-60CAA848AAAA pedig a titkos kód neve
> 2. A címke DiskEncryptionKeyFileName értéke ugyanaz, mint a titkos kód neve.
> 3. DiskEncryptionKeyEncryptionKeyURL érték utáni helyreállítása vissza a kulcsokat, és használatával szerezhető be a key vault [Get-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultkey) parancsmag
>
>

## <a name="next-steps"></a>További lépések
Kulcs és titkos vissza visszaállítása a key vault, után tekintse meg a cikk [kezelheti a biztonsági mentés és visszaállítás Azure virtuális gépek PowerShell-lel](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) titkosított virtuális gépek létrehozása a visszaállított lemez, a kulcs és titkos kulcs.
