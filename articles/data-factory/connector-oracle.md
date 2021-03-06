---
title: Adatok másolása és Oracle az Azure Data Factory használatával |} A Microsoft Docs
description: Ismerje meg, hogyan másolhat adatokat egy Oracle Database-adatbázishoz a támogatott forrás áruházakból származó, vagy támogatott fogadó-áruházak Oracle Data Factory használatával.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/14/2018
ms.author: jingwang
ms.openlocfilehash: ec0fc11ac2caf421f331a8fe72f1dacdf6b8a702
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/22/2018
ms.locfileid: "42312163"
---
# <a name="copy-data-from-and-to-oracle-by-using-azure-data-factory"></a>Adatok másolása az és Oracle az Azure Data Factory használatával
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [1-es verzió](v1/data-factory-onprem-oracle-connector.md)
> * [Aktuális verzió](connector-oracle.md)

Ez a cikk ismerteti, hogyan használja a másolási tevékenység az Azure Data Factoryban másolhat adatokat a kezdő és a egy Oracle-adatbázishoz. Épül a [másolási tevékenység áttekintése](copy-activity-overview.md) cikket, amely megadja a másolási tevékenység általános áttekintést.

## <a name="supported-capabilities"></a>Támogatott képességek

Oracle-adatbázisból adatokat másolhatja bármely támogatott fogadó adattárba. Is másolhatja adatokat bármely támogatott forrásadattárból Oracle-adatbázishoz. A másolási tevékenység által források vagy fogadóként támogatott adattárak listáját lásd: a [támogatott adattárak](copy-activity-overview.md#supported-data-stores-and-formats) tábla.

Pontosabban az Oracle-összekötő az Oracle-adatbázis következő verzióit támogatja. Támogatja az alap- vagy Objektumazonosító hitelesítések is:

- Oracle 12c R1 (12.1)
- Oracle 11g R1 vagy R2 (11.1, 11.2)
- Oracle 10g R1 vagy R2 (10.1, 10,2)
- Oracle 9i R1 vagy R2 (9.0.1, 9.2)
- Oracle-8i R3 (8.1.7-es)

> [!Note]
> Oracle-proxy kiszolgáló nem támogatott.

## <a name="prerequisites"></a>Előfeltételek

Adatok másolása a kezdő és a egy Oracle-adatbázis, amely nem a nyilvánosan elérhető-e, meg kell egy helyi Integration Runtime beállítása. Az integration runtime kapcsolatos további információkért lásd: [helyi Integration Runtime](create-self-hosted-integration-runtime.md). Az integrációs modul egy beépített Oracle-illesztőprogram biztosít. Így nem kell manuálisan a Oracle és az adatok másolása az illesztő telepítése.

## <a name="get-started"></a>Bevezetés

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

A következő szakaszok segítségével határozhatók meg a Data Factory-entitások adott és az Oracle-összekötő-tulajdonságokkal kapcsolatos részletekért.

## <a name="linked-service-properties"></a>Társított szolgáltatás tulajdonságai

Az Oracle-beli társított szolgáltatás a következő tulajdonságok támogatottak.

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type | A type tulajdonságot állítsa **Oracle**. | Igen |
| kapcsolati Sztringje | Itt adható meg az Oracle Database-példányhoz való kapcsolódáshoz szükséges információkat. Ez a mező megjelölése tárolja biztonságos helyen a Data Factory, a SecureString vagy [hivatkozik az Azure Key Vaultban tárolt titkos](store-credentials-in-key-vault.md).<br><br>**Kapcsolat típusa támogatott**: használhatja **Oracle biztonsági azonosító** vagy **Oracle-szolgáltatás neve** azonosításához az adatbázis:<br>-Ha biztonsági azonosító: `Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;`<br>-Ha a szolgáltatás a nevet használja: `Host=<host>;Port=<port>;ServiceName=<servicename>;User Id=<username>;Password=<password>;` | Igen |
| connectVia | A [integrációs modul](concepts-integration-runtime.md) az adattárban való kapcsolódáshoz használandó. (Ha az adattár nyilvánosan hozzáférhető) használhatja a helyi Integration Runtime vagy az Azure integrációs modul. Ha nincs megadva, az alapértelmezett Azure integrációs modult használja. |Nem |

>[!TIP]
>Ha eléri hiba üzenettel "ORA-01025: UPI paraméter engedélyezett tartományon kívül esik" és az Oracle-verzió 8i, adjon hozzá `WireProtocolMode=1` a kapcsolati karakterláncot, és próbálkozzon újra.

Ahhoz, hogy az Oracle kapcsolati titkosítást, két lehetősége van:

1.  Oracle-kiszolgáló oldalán, nyissa meg az Oracle speciális biztonsági (OAS), és adja meg a titkosítási beállításokat, támogatja a háromszoros DES-titkosítás (3DES) és Advanced Encryption Standard (AES), tekintse meg a részletek [Itt](https://docs.oracle.com/cd/E11882_01/network.112/e40393/asointro.htm#i1008759). Az ADF Oracle-összekötő automatikusan egyezteti OAS Oracle-kapcsolat létesítéséhez konfigurálja egy használandó titkosítási módszer.

2.  Ügyféloldalon, hozzáadhat `EncryptionMethod=1` a kapcsolati karakterláncban. Ez a beállítás használja az SSL/TLS titkosítási módszert. Ennek kell az SSL titkosítási beállításainak letiltása OAS Oracle kiszolgálóoldali titkosítás ütközés elkerülése érdekében.

**Példa**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai

Szakaszok és adatkészletek definiálását tulajdonságainak teljes listáját lásd: a [adatkészletek](concepts-datasets-linked-services.md) cikk. Ez a szakasz az Oracle-adatkészlet által támogatott tulajdonságok listáját tartalmazza.

Adatok másolása az és az Oracle, állítsa be a type tulajdonság, az adatkészlet **OracleTable**. A következő tulajdonságok támogatottak.

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type | A type tulajdonságot az adatkészlet értékre kell állítani **OracleTable**. | Igen |
| tableName |Az Oracle-adatbázis, amelyre a társított szolgáltatás hivatkozik a tábla neve. | Igen |

**Példa**

```json
{
    "name": "OracleDataset",
    "properties":
    {
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "<Oracle linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai

Szakaszok és tulajdonságok definiálását tevékenységek teljes listáját lásd: a [folyamatok](concepts-pipelines-activities.md) cikk. Ez a szakasz az Oracle-forrás és fogadó által támogatott tulajdonságok listáját tartalmazza.

### <a name="oracle-as-a-source-type"></a>Oracle-forrás típusa

Adatok másolása az Oracle, állítsa be a forrás típusaként a másolási tevékenység **OracleSource**. A következő tulajdonságok támogatottak a másolási tevékenység **forrás** szakaszban.

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type | A másolási tevékenység forrása típusa tulajdonságát állítsa **OracleSource**. | Igen |
| oracleReaderQuery | Az egyéni SQL-lekérdezés segítségével olvassa el az adatokat. Például: `"SELECT * FROM MyTable"`. | Nem |

"OracleReaderQuery" nem ad meg, ha a az adatkészlet "struktúra" szakaszban megadott oszlopok segítségével lekérdezést (`select column1, column2 from mytable`) az Oracle-adatbázis futtatásához. Ha az adatkészlet-definícióban nem tartalmaz "szerkezet", az összes oszlop ki van jelölve, a táblából.

**Példa**

```json
"activities":[
    {
        "name": "CopyFromOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Oracle input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="oracle-as-a-sink-type"></a>Oracle, a fogadó típusa

Adatok másolása az Oracle, állítsa a fogadó típusa a másolási tevékenység **Oraclesinkben**. A következő tulajdonságok támogatottak a másolási tevékenység **fogadó** szakaszban.

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| type | A másolási tevékenység fogadó típusa tulajdonságát állítsa **Oraclesinkben**. | Igen |
| WriteBatchSize | Amikor a puffer mérete eléri a writeBatchSize adatok beszúrása SQL-táblát.<br/>Megengedett értékek: egész szám (sorok száma). |Nem (az alapértelmezett érték 10 000) |
| writeBatchTimeout | Várjon, amíg a kötegelt insert művelet befejezését, mielőtt azt az időkorlátot.<br/>Megengedett értékek: időtartam. Ilyen például, 00:30:00 (30 perc). | Nem |
| preCopyScript | Adjon meg egy SQL-lekérdezést az adatok írása az Oracle-be való minden egyes futtatás előtt hajtsa végre a másolási tevékenység. Ez a tulajdonság segítségével törölje az előre betöltött adatokat. | Nem |

**Példa**

```json
"activities":[
    {
        "name": "CopyToOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Oracle output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OracleSink"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-oracle"></a>Adattípus-leképezés Oracle

Oracle és a másolt adatok, Data Factory-közbenső adattípusok a következő hozzárendeléseket használtak Oracle adattípusokat. Hogyan a másolási tevékenység leképezi a forrás séma és adatok típusa a fogadó kapcsolatos további információkért lásd: [séma és adatok írja be a hozzárendelések](copy-activity-schema-and-type-mapping.md).

| Oracle-adattípus | Data Factory közbenső adattípus |
|:--- |:--- |
| BFILE |Byte] |
| BLOB |Byte]<br/>(csak támogatott Oracle 10g vagy újabb) |
| CHAR |Sztring |
| CLOB |Sztring |
| DATE |DateTime |
| LEBEGŐPONTOS |Tizedes tört, karakterlánc (Ha a pontosság > 28) |
| EGÉSZ SZÁM |Tizedes tört, karakterlánc (Ha a pontosság > 28) |
| HOSSZÚ |Sztring |
| MENNYI IDEIG NYERS |Byte] |
| NCHAR |Sztring |
| NCLOB |Sztring |
| SZÁM |Tizedes tört, karakterlánc (Ha a pontosság > 28) |
| NVARCHAR2 |Sztring |
| RAW |Byte] |
| }, ROWID |Sztring |
| IDŐBÉLYEG |DateTime |
| A HELYI IDŐZÓNA IDŐBÉLYEG |Sztring |
| AZ IDŐZÓNA IDŐBÉLYEG |Sztring |
| ELŐJEL NÉLKÜLI EGÉSZ SZÁM |Szám |
| VARCHAR2 |Sztring |
| XML |Sztring |

> [!NOTE]
> Az adattípusok IDŐKÖZ év TO hónap és nap TO IDŐKÖZ második nem támogatottak.


## <a name="next-steps"></a>További lépések
A másolási tevékenység, Data Factory által forrásként és fogadóként támogatott adattárak listáját lásd: [támogatott adattárak](copy-activity-overview.md##supported-data-stores-and-formats).
