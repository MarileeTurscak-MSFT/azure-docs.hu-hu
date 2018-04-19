---
title: "Kapcsolódás Azure Cosmos adatforrásként adatbázis Azure Machine Learning-munkaterület |} Microsoft Docs"
description: "Ez a dokumentum példaként szolgál a Azure Cosmos DB keresztül Azure Machine Learning-munkaterület összekapcsolása"
services: machine-learning
author: cforbe
ms.author: cforbe
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: d36b394a528dc4bc1b6e0a9e0e5dbde728cbee1b
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/05/2018
---
# <a name="connecting-to-azure-cosmos-db-as-a-data-source"></a>Csatlakozás Azure Cosmos DB adatforrásként
Ez a cikk tartalmaz egy python minta lehetővé teszi, hogy csatlakozzon az Azure Machine Learning munkaterület Cosmos-Adatbázishoz.

## <a name="load-azure-cosmos-db-data-into-data-preparation"></a>Azure Cosmos DB adatok betöltése az adatok előkészítése

Hozzon létre egy új, parancsprogram-alapú adatfolyam, és a következő parancsfájl segítségével az adatok betöltése az Azure Cosmos DB. 

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

import pandas as pd

config = { 
    'ENDPOINT': '<Endpoint>',
    'MASTERKEY': '<Key>',
    'DOCUMENTDB_DATABASE': '<DBName>',
    'DOCUMENTDB_COLLECTION': '<collectionname>'
};

# Initialize the Python DocumentDB client.
client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})

# Read databases and take first since id should not be duplicated.
db = next((data for data in client.ReadDatabases() if data['id'] == config['DOCUMENTDB_DATABASE']))

# Read collections and take first since id should not be duplicated.
coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config['DOCUMENTDB_COLLECTION']))

docs = client.ReadDocuments(coll['_self'])

df = pd.DataFrame(list(docs))
```

## <a name="other-data-source-connections"></a>Más adatforrás-kapcsolatok
Más minták, olvassa el a [példa további adatok kapcsolatok](data-prep-appendix8-sample-source-connections-python.md)
