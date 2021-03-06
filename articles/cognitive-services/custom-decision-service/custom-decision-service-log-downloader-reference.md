---
title: LogDownloader – Custom Decision Service
titlesuffix: Azure Cognitive Services
description: Töltse le a naplófájlok Azure Custom Decision Service által készített.
services: cognitive-services
author: marco-rossi29
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-decision-service
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: marossi
ms.openlocfilehash: 8c5ab0e297690f1fbdb41a2627dd63c3ea522d1b
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/19/2018
ms.locfileid: "46366812"
---
# <a name="logdownloader"></a>LogDownloader

Letölti a naplófájlokat, amelyek Azure Custom Decision Service által készített, és hozzon létre a *.gz* Kísérletezési által használt fájlokat.

## <a name="prerequisites"></a>Előfeltételek

- Python 3: Telepítve van, és az elérési útra. Azt javasoljuk, hogy a 64 bites verziója, nagy méretű fájlok kezeléséhez.
- A *Microsoft/MOLWT-ds* tárházat: [klónozza az adattárat](https://github.com/Microsoft/mwt-ds).
- A *azure-storage-blob* csomag: telepítési részletekért nyissa meg [a Microsoft Azure Storage kódtára a Pythonhoz](https://github.com/Azure/azure-storage-python#option-1-via-pypi).
- Adja meg az Azure storage kapcsolati karakterláncát az *mwt-ds/DataScience/ds.config*: kövesse a *my_app_id: my_connectionString* sablont. Megadhatja, hogy több `app_id`. Futtatásakor `LogDownloader.py`, ha a bemeneti `app_id` nem található a `ds.config`, `LogDownloader.py` használja a `$Default` kapcsolati karakterláncot.

## <a name="usage"></a>Használat

Lépjen a `mwt-ds/DataScience` , és futtassa `LogDownloader.py` megfelelő argumentumokkal, ahogy az az alábbi kódot:

```cmd
python LogDownloader.py [-h] -a APP_ID -l LOG_DIR [-s START_DATE]
                        [-e END_DATE] [-o OVERWRITE_MODE] [--dry_run]
                        [--create_gzip] [--delta_mod_t DELTA_MOD_T]
                        [--verbose] [-v VERSION]
```

### <a name="parameters"></a>Paraméterek

| Input (Bemenet) | Leírás | Alapértelmezett |
| --- | --- | --- |
| `-h`, `--help` | A súgóüzenetet és kilépési megjelenítése. | |
| `-a APP_ID`, `--app_id APP_ID` | Az alkalmazás azonosítója (azaz a Azure blob tároló nevét). | Szükséges |
| `-l LOG_DIR`, `--log_dir LOG_DIR` | Az alap könyvtár (almappa létrehozott) adatokat tölti le.  | Szükséges |
| `-s START_DATE`, `--start_date START_DATE` | A letöltés kezdő dátuma (tartalmazza), a *éééé-hh-nn* formátumban. | `None` |
| `-e END_DATE`, `--end_date END_DATE` | A letöltés záró dátum (tartalmazza), *éééé-hh-nn* formátumban. | `None` |
| `-o OVERWRITE_MODE`, `--overwrite_mode OVERWRITE_MODE` | A felülírás módot használja. | |
| | `0`: Írja felül soha nem; Kérje meg a felhasználót, hogy blobok jelenleg használatban van. | Alapértelmezett | |
| | `1`: Kérje meg a folytatásról, ha a fájlok a különböző méretű, vagy ha a blobokat éppen használatban vannak a felhasználó. | |
| | `2`: Írja felül mindig; Töltse le a jelenleg használt blobokat. | |
| | `3`: Soha ne írja felül, és ha nélkül megkérdezi; a mérete nagyobb, hozzáfűzése Töltse le a jelenleg használt blobokat. | |
| | `4`: Soha ne írja felül, és ha nélkül megkérdezi; a mérete nagyobb, hozzáfűzése hagyja ki a jelenleg használt blobokat. | |
| `--dry_run` | A nyomtatási melyik blobok lenne le vannak töltve, hogy nem tölti le. | `False` |
| `--create_gzip` | Hozzon létre egy *gzip* Vowpal Wabbit fájlt. | `False` |
| `--delta_mod_t DELTA_MOD_T` | Időablak (másodpercben), az azt észlelte, hogy a fájl jelenleg használatban van. | `3600` mp-ben (`1` óra) |
| `--verbose` | Nyomtatási további részleteket. | `False` |
| `-v VERSION`, `--version VERSION` | Napló letöltési segédprogramja verziót kell használni. | |
| | `1`: A nyers naplók (csak a visszamenőleges kompatibilitás érdekében). | Elavult |
| | `2`: A főtt naplókhoz. | Alapértelmezett |

### <a name="examples"></a>Példák

Egy tesztfuttatás letöltés, az Azure Storage blob-tároló lévő összes adatot használja a következő kódot:
```cmd
python LogDownloader.py -a your_app_id -l d:\data --dry_run
```

Csak létre, mert a 2018. január 1. a naplók letöltéséhez `overwrite_mode=4`, a következő kóddal:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4
```

Hozhat létre egy *gzip* fájlt a letöltött fájlok egyesítését, a következő kóddal:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4 --create_gzip
```