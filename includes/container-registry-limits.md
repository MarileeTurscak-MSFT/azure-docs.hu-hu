---
title: fájl belefoglalása
description: fájl belefoglalása
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 08/30/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: bddb17a5163333a5aeb86b296a21867f0611d12f
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/30/2018
ms.locfileid: "43302529"
---
| Erőforrás | Alapszintű | Standard | Prémium |
|---|---|---|---|---|
| Tárolási<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| Maximális képméret réteg | 20 giB | 20 giB | 50 GB |
| Percenkénti ReadOps<sup>2, 3</sup> | 1,000 | 3,000 | 10,000 |
| Írási műveletek száma percenként<sup>2, 4</sup> | 100 | 500 | 2,000 |
| Töltse le a sávszélesség MB/s<sup>2</sup> | 30 | 60 | 100 |
| Töltse fel a sávszélesség MB/s<sup>2</sup> | 10 | 20 | 50 |
| Webhookok | 2 | 10 | 100 |
| Georeplikáció | – | – | [Támogatott][geo-replication] |
| Tartalom-megbízhatóság (előzetes verzió) | – | – | [Támogatott][content-trust] |

<sup>1</sup> a megadott tárolási korlátok a következők mennyisége *foglalt* storage az egyes rétegekhez. Egy további napi díj / GIB-ra a fenti ezeket a korlátokat képtárolás díjkötelesek. Forgalmi információkért lásd: [Container Registry díjszabás][pricing].

<sup>2</sup> *ReadOps*, *írási műveletek*, és *sávszélesség* minimális becslések. ACR nagy hangsúlyt fektet a teljesítmény javítása, a használatához.

<sup>3</sup> [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) a rendszer lefordítja arra több olvasási műveletek a lemezképet, valamint a manifest lekérés a rétegek száma alapján.

<sup>4</sup> [docker leküldéses](https://docs.docker.com/registry/spec/api/#pushing-an-image) a rendszer lefordítja arra, hogy kell lehet leküldeni a rétegek száma alapján, több írási műveleteket. A `docker push` tartalmaz *ReadOps* beolvasni a meglévő rendszerképet jegyzékfájl.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md