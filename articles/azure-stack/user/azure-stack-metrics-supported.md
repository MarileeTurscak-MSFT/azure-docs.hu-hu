---
title: Támogatott mérőszámok az Azure Monitor szolgáltatással az Azure Stackben |} A Microsoft Docs
description: További információk a támogatott mérőszámok az Azure Monitor az Azure Stacken.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: mabrigg
ms.openlocfilehash: 0cd8d309cfbf72a05c83c2a536d754e9cbc6e008
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/06/2018
ms.locfileid: "44022659"
---
# <a name="supported-metrics-with-azure-monitor-on-azure-stack"></a>A támogatott mérőszámok az Azure monitorral, az Azure Stackben

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

A metrikák kérdezhet le Azure monitor az Azure Stacken ugyanaz, mint az Azure globális. A mértékek is a portált, beszerezheti őket a REST API, vagy lekérdezheti, ha a PowerShell vagy parancssori felület.

Az alábbi táblázatok sorolják fel a metrikák elérhető az Azure Monitor metrika folyamatot az Azure Stacken. Lekérdezése, és hozzáférhetnek ezekhez a metrikákhoz, kell a **2018-01-01** api-version verzióját az API-profil. API-profilok és az Azure Stack kapcsolatos további információkért lásd: [kezelése API-verzióprofilok az Azure Stackben](azure-stack-version-profiles.md).

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

| Metrika | Metrika megjelenített neve | Unit (Egység) | Aggregáció típusa | Leírás | Dimenziók |
|----------------|---------------------|---------|------------------|-----------------------------------------------------------------------------------------------|---------------|
| Százalékos processzorhasználat | Százalékos processzorhasználat | Százalék | Átlag | A virtuális gép(ek) által jelenleg használt lefoglalt számítási egységek százalékos aránya | Nincs dimenzió |

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

| Metrika | Metrika megjelenített neve | Unit (Egység) | Aggregáció típusa | Leírás | Dimenziók |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| UsedCapacity | Használt kapacitás | Bájt | Átlag | A fiók felhasznált kapacitása | Nincs dimenzió |
| Tranzakciók | Tranzakciók | Darabszám | Összes | Tárolási szolgáltatás vagy a megadott API-művelet számára elküldött kérések száma. Ez a szám a sikeres és sikertelen kérelmeket, valamint a hibák kéréseket tartalmazza. Különböző típusú válaszok számának használja a ResponseType dimenziót. | ResponseType, GeoType, ApiName |
| Belépő | Belépő | Bájt | Összes | A bejövő adatok (bájt) mennyisége. Ez a szám a külső ügyfél Azure Storage-ba irányuló bejövő adatait és az Azure-on belüli bejövő adatokat egyaránt magában foglalja. | GeoType, ApiName |
| Kimenő forgalom | Kimenő forgalom | Bájt | Összes | A kimenő adatok (bájt) mennyisége. Ez a szám a külső ügyfél Azure Storage-ba irányuló kimenő adatait és az Azure-on belüli kimenő adatokat egyaránt magában foglalja. Az eredményül kapott szám nem tükrözi a számlázható kimenő forgalmat. | GeoType, ApiName |
| SuccessServerLatency | Sikeres kiszolgálói kérések késése | Ezredmásodperc | Átlag | Az átlagos várakozási használt Azure Storage által feldolgozott sikeres kérések, ezredmásodpercben. Ez az érték nem tartalmazza a hálózati késés megadott AverageE2ELatency. | GeoType, ApiName |
| SuccessE2ELatency | Sikeres kérések végpontok közötti késése | Ezredmásodperc | Átlag | A társzolgáltatásnak vagy a megadott API-művelet (MS) számára elküldött sikeres kérések átlagos végpontok közötti késését. Ez az érték magában foglalja a kérelem elolvasásához, a válasz elküldéséhez és a válasz visszaigazolásának fogadásához az Azure Storage számára szükséges feldolgozási időt. | GeoType, ApiName |
| Rendelkezésre állás | Rendelkezésre állás | Százalék | Átlag | A társzolgáltatás vagy a megadott API-művelet rendelkezésre állási százaléka. Rendelkezésre állási a TotalBillableRequests értékét és értékkel való osztásának alkalmazható kérések, ki nem várt hibák száma alapján számítjuk. Minden nem várt hiba a társzolgáltatás vagy a megadott API-művelet romlik a rendelkezésre állás eredményez. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

| Metrika | Metrika megjelenített neve | Unit (Egység) | Aggregáció típusa | Leírás | Dimenziók |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| BlobCapacity | Blob-kapacitása | Bájt | Összes | A tárfiók Blob Service-példánya által felhasznált tárterület mérete bájtban megadva. | BlobType |
| BlobCount | Blobok száma | Darabszám | Összes | A tárfiók Blob Service-példányában található blobok száma. | BlobType |
| ContainerCount | Blobtárolók száma | Darabszám | Átlag | A tárfiók Blob Service-példányában található tárolók száma. | Nincs dimenzió |
| Tranzakciók | Tranzakciók | Darabszám | Összes | Tárolási szolgáltatás vagy a megadott API-művelet számára elküldött kérések száma. Ez a szám a sikeres és sikertelen kérelmeket, valamint a hibák kéréseket tartalmazza. Különböző típusú válaszok számának használja a ResponseType dimenziót. | ResponseType, GeoType, ApiName |
| Belépő | Belépő | Bájt | Összes | A bejövő adatok (bájt) mennyisége. Ez a szám a külső ügyfél Azure Storage-ba irányuló bejövő adatait és az Azure-on belüli bejövő adatokat egyaránt magában foglalja. | GeoType, ApiName |
| Kimenő forgalom | Kimenő forgalom | Bájt | Összes | A kimenő adatok (bájt) mennyisége. Ez a szám a külső ügyfél Azure Storage-ba irányuló kimenő adatait és az Azure-on belüli kimenő adatokat egyaránt magában foglalja. Az eredményül kapott szám nem tükrözi a számlázható kimenő forgalmat. | GeoType, ApiName |
| SuccessServerLatency | Sikeres kiszolgálói kérések késése | Ezredmásodperc | Átlag | Az átlagos várakozási használt Azure Storage által feldolgozott sikeres kérések, ezredmásodpercben. Ez az érték nem tartalmazza a hálózati késés megadott AverageE2ELatency. | GeoType, ApiName |
| SuccessE2ELatency | Sikeres kérések végpontok közötti késése | Ezredmásodperc | Átlag | A társzolgáltatásnak vagy a megadott API-művelet (MS) számára elküldött sikeres kérések átlagos végpontok közötti késését. Ez az érték magában foglalja a kérelem elolvasásához, a válasz elküldéséhez és a válasz visszaigazolásának fogadásához az Azure Storage számára szükséges feldolgozási időt. | GeoType, ApiName |
| Rendelkezésre állás | Rendelkezésre állás | Százalék | Átlag | A társzolgáltatás vagy a megadott API-művelet rendelkezésre állási százaléka. Rendelkezésre állási a TotalBillableRequests értékét és értékkel való osztásának alkalmazható kérések, ki nem várt hibák száma alapján számítjuk. Minden nem várt hiba a társzolgáltatás vagy a megadott API-művelet romlik a rendelkezésre állás eredményez. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

| Metrika | Metrika megjelenített neve | Unit (Egység) | Aggregáció típusa | Leírás | Dimenziók |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| TableCapacity | Table Storage kapacitása | Bájt | Átlag | A tárfiók Table Storage-szolgáltatás-példánya által felhasznált tárterület mérete bájtban megadva. | Nincs dimenzió |
| TableCount | Táblák száma | Darabszám | Átlag | A tárfiók Table Storage-szolgáltatás-példányában található táblák száma. | Nincs dimenzió |
| TableEntityCount | Táblaentitások száma | Darabszám | Átlag | A tárfiók Table Storage-szolgáltatás-példányában található táblaentitások száma. | Nincs dimenzió |
| Tranzakciók | Tranzakciók | Darabszám | Összes | Tárolási szolgáltatás vagy a megadott API-művelet számára elküldött kérések száma. Ez a szám a sikeres és sikertelen kérelmeket, valamint a hibák kéréseket tartalmazza. Különböző típusú válaszok számának használja a ResponseType dimenziót. | ResponseType, GeoType, ApiName |
| Belépő | Belépő | Bájt | Összes | A bejövő adatok (bájt) mennyisége. Ez a szám a külső ügyfél Azure Storage-ba irányuló bejövő adatait és az Azure-on belüli bejövő adatokat egyaránt magában foglalja. | GeoType, ApiName |
| Kimenő forgalom | Kimenő forgalom | Bájt | Összes | A kimenő adatok (bájt) mennyisége. Ez a szám a külső ügyfél Azure Storage-ba irányuló kimenő adatait és az Azure-on belüli kimenő adatokat egyaránt magában foglalja. Az eredményül kapott szám nem tükrözi a számlázható kimenő forgalmat. | GeoType, ApiName |
| SuccessServerLatency | Sikeres kiszolgálói kérések késése | Ezredmásodperc | Átlag | Az átlagos várakozási használt Azure Storage által feldolgozott sikeres kérések, ezredmásodpercben. Ez az érték nem tartalmazza a hálózati késés megadott AverageE2ELatency. | GeoType, ApiName |
| SuccessE2ELatency | Sikeres kérések végpontok közötti késése | Ezredmásodperc | Átlag | A társzolgáltatásnak vagy a megadott API-művelet (MS) számára elküldött sikeres kérések átlagos végpontok közötti késését. Ez az érték magában foglalja a kérelem elolvasásához, a válasz elküldéséhez és a válasz visszaigazolásának fogadásához az Azure Storage számára szükséges feldolgozási időt. | GeoType, ApiName |
| Rendelkezésre állás | Rendelkezésre állás | Százalék | Átlag | A társzolgáltatás vagy a megadott API-művelet rendelkezésre állási százaléka. Rendelkezésre állási a TotalBillableRequests értékét és értékkel való osztásának alkalmazható kérések, ki nem várt hibák száma alapján számítjuk. Minden nem várt hiba a társzolgáltatás vagy a megadott API-művelet romlik a rendelkezésre állás eredményez. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

| Metrika | Metrika megjelenített neve | Unit (Egység) | Aggregáció típusa | Leírás | Dimenziók |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| QueueCapacity | Queue Storage kapacitása | Bájt | Átlag | A tárfiók Queue Storage-szolgáltatás-példánya által felhasznált tárterület mérete bájtban megadva. | Nincs dimenzió |
| QueueCount | Üzenetsorok száma | Darabszám | Átlag | A tárfiók Queue-szolgáltatás-példányában található üzenetsorok száma. | Nincs dimenzió |
| QueueMessageCount | Üzenetsorbeli üzenetek száma | Darabszám | Átlag | A tárfiók Queue Storage-szolgáltatás-példányában található üzenetsorbeli üzenetek hozzávetőleges száma. | Nincs dimenzió |
| Tranzakciók | Tranzakciók | Darabszám | Összes | Tárolási szolgáltatás vagy a megadott API-művelet számára elküldött kérések száma. Ez a szám a sikeres és sikertelen kérelmeket, valamint a hibák kéréseket tartalmazza. Különböző típusú válaszok számának használja a ResponseType dimenziót. | ResponseType, GeoType, ApiName |
| Belépő | Belépő | Bájt | Összes | A bejövő adatok (bájt) mennyisége. Ez a szám a külső ügyfél Azure Storage-ba irányuló bejövő adatait és az Azure-on belüli bejövő adatokat egyaránt magában foglalja. | GeoType, ApiName |
| Kimenő forgalom | Kimenő forgalom | Bájt | Összes | A kimenő adatok (bájt) mennyisége. Ez a szám a külső ügyfél Azure Storage-ba irányuló kimenő adatait és az Azure-on belüli kimenő adatokat egyaránt magában foglalja. Az eredményül kapott szám nem tükrözi a számlázható kimenő forgalmat. | GeoType, ApiName |
| SuccessServerLatency | Sikeres kiszolgálói kérések késése | Ezredmásodperc | Átlag | Az átlagos várakozási használt Azure Storage által feldolgozott sikeres kérések, ezredmásodpercben. Ez az érték nem tartalmazza a hálózati késés megadott AverageE2ELatency. | GeoType, ApiName |
| SuccessE2ELatency | Sikeres kérések végpontok közötti késése | Ezredmásodperc | Átlag | A társzolgáltatásnak vagy a megadott API-művelet (MS) számára elküldött sikeres kérések átlagos végpontok közötti késését. Ez az érték magában foglalja a kérelem elolvasásához, a válasz elküldéséhez és a válasz visszaigazolásának fogadásához az Azure Storage számára szükséges feldolgozási időt. | GeoType, ApiName |
| Rendelkezésre állás | Rendelkezésre állás | Százalék | Átlag | A társzolgáltatás vagy a megadott API-művelet rendelkezésre állási százaléka. Rendelkezésre állási a TotalBillableRequests értékét és értékkel való osztásának alkalmazható kérések, ki nem várt hibák száma alapján számítjuk. Minden nem várt hiba a társzolgáltatás vagy a megadott API-művelet romlik a rendelkezésre állás eredményez. | GeoType, ApiName |

## <a name="next-steps"></a>További lépések

Tudjon meg többet [Azure monitorozása az Azure Stacken](azure-stack-metrics-azure-data.md).