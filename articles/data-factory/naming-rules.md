---
title: Az Azure Data Factory-entitások elnevezési szabályai |} A Microsoft Docs
description: Az entitások Data Factory elnevezési szabályait ismerteti.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: shlo
ms.openlocfilehash: cfab1a82c7da0ad596c9989e5a9f3ed800c58e4a
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389310"
---
# <a name="azure-data-factory---naming-rules"></a>Az Azure Data Factory – elnevezési szabályok
Az alábbi táblázat a Data Factory-összetevők elnevezési szabályait biztosít.

| Name (Név) | Név egyedi-e | Érvényesség-ellenőrzések |
|:--- |:--- |:--- |
| Data Factory |Egyedi Microsoft Azure-ban. Nevek és nagybetűk nincsenek megkülönböztetve, azaz `MyDF` és `mydf` tekintse meg az azonos adat-előállítóban. |<ul><li>Minden adat-előállító pontosan egy Azure-előfizetéssel van kötve.</li><li>Az objektum nevének betűvel vagy számmal kell kezdődnie, és csak betűket, számokat és a kötőjel (-) karaktert tartalmazhat.</li><li>Minden kötőjel (-) karaktert kell előtt és egy betű vagy szám követ. Egymást követő kötőjelek használata nem engedélyezett a tároló nevében.</li><li>Neve 3 – 63 karakter hosszú lehet.</li></ul> |
| Társított szolgáltatások/adatkészletek és folyamatok |Egyedi az adat-előállítóban. Nevek és nagybetűk nincsenek megkülönböztetve. |<ul><li>Objektum nevének betűvel, számmal vagy aláhúzásjellel (_) kell kezdődnie.</li><li>A következő karakterek nem engedélyezettek: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</li><li>Szaggatott vonal ("-") a társított szolgáltatásokat és az adatkészletek csak a nevek nem engedélyezettek.</li></ul>  |
| Erőforráscsoport |Egyedi Microsoft Azure-ban. Nevek és nagybetűk nincsenek megkülönböztetve. | További információ: [Azure elnevezési szabályokat és korlátozásokat](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions). |

## <a name="next-steps"></a>További lépések
Ismerje meg, hogyan hozhat létre adat-előállítók részletes utasításait követve [a rövid útmutató: adat-előállító létrehozása](quickstart-create-data-factory-powershell.md) cikk. 
