---
title: Az Azure Import/Export-feladatok diagnosztizálása és hibajavítása |} A Microsoft Docs
description: Ismerje meg a részletes naplózás a Microsoft Azure Import/Export szolgáltatás feladatok engedélyezése.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 2a54752f933b91265d0aa8add61ca0707615931b
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526327"
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a>Az Azure Import/Export-feladatok diagnosztizálása és hibajavítása
Minden meghajtó feldolgozott az Azure Import/Export szolgáltatás hibanaplót a társított storage-fiókot hoz létre. Részletes naplózás engedélyezése beállításával a `LogLevel` tulajdonságot `Verbose` hívásakor a [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) vagy [frissítési feladat tulajdonságai](/rest/api/storageimportexport/jobs#Jobs_Update) műveleteket.

 Alapértelmezés szerint a naplók nevű tárolóba írták `waimportexport`. Úgy is megadhat egy másik nevet a `DiagnosticsPath` tulajdonság hívásakor a `Put Job` vagy `Update Job Properties` műveleteket. Az alábbi elnevezési szabályt követik a blokkblobként tárolja a naplókat: `waies/jobname_driveid_timestamp_logtype.xml`.

 URI-ját a naplók egy feladat meghívásával lehet lekérdezni a [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) művelet. A részletes napló URI Azonosítóját adja vissza a `VerboseLogUri` tulajdonság minden meghajtó, míg a hibanapló URI Azonosítóját adja vissza a `ErrorLogUri` tulajdonság.

A naplózási adatok segítségével a következő problémák azonosításához.

## <a name="drive-errors"></a>Meghajtó hibák

A következő elemek besorolt meghajtó hibák:

-   Hibák a fér hozzá, vagy az Alkalmazásjegyzék-fájl olvasása

-   Helytelen a BitLocker-kulcsok

-   Meghajtó olvasási/írási hibák

## <a name="blob-errors"></a>A BLOB-hibák

A következő elemek besorolt blob hibák:

-   Hibás vagy ütköző blob vagy -nevek

-   Hiányzó fájlok

-   A BLOB nem található

-   (A fájlok a lemezen olyan kisebb, mint a jegyzékfájlban megadott) csonkolt fájlok

-   (Az importálási feladat által észlelt egy MD5-tel ellenőrzőösszege nem egyezik) tartalom sérült fájl

-   Sérült blob metaadatait és tulajdonság fájlok (egy MD5-tel ellenőrzőösszege nem egyezik az észlelt)

-   Helytelen a séma a blob tulajdonságainak és/vagy metaadat-fájlok

Előfordulhatnak olyan esetekben, ahol egyes részei egy importálási vagy exportálási feladat nem fejeződik be, a teljes feladat továbbra is elvégzése közben. Ebben az esetben feltölthet vagy töltse le a hiányzó rész az adatok hálózaton keresztül, vagy létrehozhat egy új feladatot az adatok átvitelét. Tekintse meg a [Azure Import/Export eszköz referencia](storage-import-export-tool-how-to-v1.md) hogyan javítsa ki az adatok hálózaton keresztül.

## <a name="next-steps"></a>További lépések

* [Az Import/Export szolgáltatás REST API használatával](storage-import-export-using-the-rest-api.md)
