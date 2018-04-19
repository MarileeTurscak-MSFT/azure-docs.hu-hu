---
title: "Példa további Forráshelyek adatokhoz való kapcsolódást lehetővé az Azure Machine Learning adatok előkészítése |} Microsoft Docs"
description: "Ez a dokumentum biztosít forrás adatok kapcsolat esetén lehetséges az Azure Machine Learning adatok előkészítése"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 27d72f1b49a89929098c0cc35f92d3915fc8b0b9
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/05/2018
---
# <a name="sample-of-custom-source-connections-python"></a>Egyéni adatforrás-kapcsolatok (Python) minta 
Ahhoz, hogy olvassa el a jelen függelék, olvassa el a [Python bővítési áttekintése](data-prep-python-extensibility-overview.md).

## <a name="load-data-from-dataworld"></a>Adatok betöltése az data.world

### <a name="prerequisites"></a>Előfeltételek

#### <a name="register-yourself-at-dataworld"></a>Regisztrálja magát data.world:
Szüksége van egy API-jogkivonatot az data.world webhelyről.

#### <a name="install-dataworld-library"></a>Telepítse a data.world könyvtár

Nyissa meg az Azure Machine Learning-munkaterület parancssori felület kiválasztásával **fájl** > **nyissa meg a parancssori felület**.

```console
pip install git+git://github.com/datadotworld/data.world-py.git
```

Ezt követően futtassa `dw configure` a parancssorban, amely bekéri a jogkivonatot. Ha, írja be a jogkivonatot, egy .dw / könyvtár jön létre a kezdőkönyvtár és a token nem tárolja.

```
API token (obtained at: https://data.world/settings/advanced): <enter API token here>
```
Most meg kell tudni data.world könyvtárak importálása.

#### <a name="load-data-into-data-preparation"></a>Adatok betöltése az adatok előkészítése

Hozzon létre egy átalakítási Data Flow (parancsfájl) átalakítási. A következő parancsfájl segítségével az adatok betöltése az data.world.

```python
#paths = df['Path'].tolist()

import datadotworld as dw

#Load the dataset.
lds = dw.load_dataset('data-society/the-simpsons-by-the-data')

#Load specific data frame from the dataset.
df = lds.dataframes['simpsons_episodes']

```
## <a name="azure-cosmos-db-as-a-data-source-connection"></a>Az Azure Cosmos DB adatforrás-kapcsolat
Például egy Azure Cosmos DB adatok kapcsolatként, olvassa el a [betöltése Azure Cosmos DB adatforrás adatok kapcsolatként](data-prep-load-azure-cosmos-db.md)
