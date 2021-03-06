---
title: Adatok importálása a Machine Learning Studióban |} A Microsoft Docs
description: Adatok importálása az Azure Machine Learning Studióba különböző adatforrásokból származó adatokat hogyan. Ismerje meg, milyen típusú adatokat és adatformátumok a célnyelven támogatottak.
keywords: adatok, az adatok formátuma, adattípusok, adatforrások, betanítási adatok importálása
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: c194ee3b-838c-4efe-bb2a-c1d052326216
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.openlocfilehash: a5750555802489b41b007831164767beb953ebc4
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/10/2018
ms.locfileid: "34837463"
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>A betanítási adatok importálása az Azure Machine Learning Studióba különböző adatforrásokból
A Machine Learning Studio a saját adatok használatával fejlesztése és betanítunk egy prediktív elemzési megoldás, a következőket teheti: 

* az adatok feltöltése egy **helyi fájl** előre a merevlemezen a munkaterületen lévő adatkészlet modul létrehozása
* érheti el adatait több egyik **online adatforrás** kísérletét használatával futása közben a [adatok importálása] [ import-data] modul 
* adatok forrása egy másik Azure Machine learning **kísérletezhet** adatkészletként mentve
* adatok forrása egy helyszíni **SQL Server-adatbázis**

Ezek a beállítások leírását a témakörök egyikében az alábbi menü. Ezek a témakörök bemutatják, hogyan importálhat adatokat a különböző adatforrásokból származó adatokat a Machine Learning Studio használatához. 

[!INCLUDE [import-data-into-aml-studio-selector](../../../includes/machine-learning-import-data-into-aml-studio.md)]

> [!NOTE]
> Nincsenek mintaadatkészletek számos Machine Learning studióban, amely a betanítási adatok is használhat. Ezek a további információkért lásd: [az Azure Machine Learning Studio mintaadatkészleteinek használata](use-sample-datasets.md)).
> 
> 

Ez a témakör bevezető is ismerteti, hogyan olvashat be adatokat használatra kész a Machine Learning Studióban, és ismerteti, hogy mely adatformátumok a célnyelven és adattípusok használata támogatott. 

> [!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]
> 
> 

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a>Az Azure Machine Learning Studióban használatra kész adatok lekérése
A Machine Learning Studio téglalap alakú vagy táblázatos adatok, például a strukturált adatok egy adatbázisból, azonban bizonyos körülmények között nem téglalap adatok használhatók vagy tagolt szöveges adat tervezték.

A legjobb, ha az adatok viszonylag tiszta. Ez azt jelenti, hogy szeretné problémákat, például nem jegyzett karakterláncok gondoskodik az adatok feltöltése a kísérlet előtt.

Azonban nincsenek modulok elérhető a Machine Learning Studióban, amelyek lehetővé teszik az adatok a kísérleten belülről néhány kezeléséhez. Attól függően, gépi tanulási algoritmusok fogja használni, szükség lehet annak eldöntése, hogyan fogja kezelni adatok szerkezeti problémák, például a hiányzó értékeket és a ritka adatokhoz, és vannak, amelyek segíthetnek, hogy a modulok. Tekintse meg a **adatátalakítás** a modulpaletta végre ezeket a funkciókat modulokba vonatkozó szakaszában.

A kísérlet során bármikor megtekintheti, és töltse le a kimeneti portra kattintva egy modul által előállított adatok. Attól függően, a modul lehet különböző letöltési beállítások használható, vagy előfordulhat, hogy a jeleníthetik meg az adatokat a Machine Learning Studióban webböngészőből.

## <a name="data-formats-and-data-types-supported"></a>Támogatott formátumok és adattípusok
Különféle típusú adatokat importálhat is futtathatja a kísérletet, attól függően, hogy milyen mechanizmussal importálni az adatokat, és azt forrását használja:

* Egyszerű szövegfájlt (.txt)
* Vesszővel tagolt (CSV) egy fejlécet (tartalmazó.csv) vagy anélkül (. nh.csv)
* Tabulátorral tagolt értékeket (TSV) fejléc (.tsv) vagy anélkül (. nh.tsv)
* Excel-fájl
* Azure-tábla
* Hive-tábla
* SQL Database tábla
* OData-értékek
* SVMLight adatok (.svmlight) (lásd a [SVMLight definíció](http://svmlight.joachims.org/) formátum információ)
* Attribútum-relációs fájlformátumra (ARFF) adatokat (.arff) (lásd a [ARFF definíció](http://weka.wikispaces.com/ARFF) formátum információ)
* Zip-fájl (.zip)
* R-objektum vagy a munkaterület fájlt (. Rekordadat)

Ha például a metaadatokat tartalmazó ARFF formátumú adatokat importál, a Machine Learning Studio ezeket a metaadatokat használja meghatározásához a fejlécet és az egyes oszlopok adattípusát.

Ha importálja az adatokat, például a TSV- vagy CSV formátum, amely nem tartalmazza ezeket a metaadatokat, a Machine Learning Studio minden oszlop adattípusát kikövetkezteti által az adatok mintavételezésének. Ha az adatok nem rendelkezik oszlopfejlécekkel, a Machine Learning Studio alapértelmezett nevek biztosítja.

Explicit módon adja meg vagy módosítsa a fejlécek és adattípusok használatával oszlopok esetében a [metaadatainak szerkesztése][edit-metadata].

A következő **adattípusok** felismeri a Machine Learning Studio:

* Sztring
* Egész szám
* Dupla
* Logikai
* DateTime
* Időtartam

A Machine Learning Studio nevű belső adattípust használ ***adattábla*** adatok átadására a modulok között. Adattábla formátum használatával az explicit módon átalakíthatja az adatokat a [adatkészlet átalakítása] [ convert-to-dataset] modul.

Adattábla eltérő formátumokban fogad modulok konvertálja az adatok adattábla csendes mielőtt továbbítaná a következő modul számára.

Szükség esetén átválthat adatok táblázatos formában visszaimportálni CSV, TSV, ARFF vagy más átalakítási modulok SVMLight formátumú.
Tekintse meg a **formátum Adatátalakítókat** a modulpaletta végre ezeket a funkciókat modulokba vonatkozó szakaszában.

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
