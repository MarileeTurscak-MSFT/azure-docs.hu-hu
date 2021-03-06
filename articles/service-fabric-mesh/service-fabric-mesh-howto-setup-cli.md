---
title: Az Azure Service Fabric Mesh parancssori felületének beállítása | Microsoft Docs
description: Ez a dokumentum az Azure Service Fabric Mesh parancssori felület beállítását ismerteti.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/26/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: c0e2aefe1222263b169e21490da079b165a57321
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/22/2018
ms.locfileid: "42108478"
---
# <a name="set-up-the-service-fabric-mesh-cli"></a>A Service Fabric Mesh parancssori felületének beállítása
A Service Fabric Mesh parancssori felülete az erőforrások a Service Fabric Meshben való üzembe helyezéséhez és felügyeletéhez szükséges. 

Az előzetes verzióban az Azure Service Fabric Mesh parancssori felülete az Azure CLI bővítményeként van megírva. Telepítheti az Azure Cloud Shellbe vagy az Azure CLI helyileg telepített példányába. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Ha a parancssori felület helyi telepítését és használatát választja, az Azure CLI 2.0.43-as vagy újabb verzióját kell telepítenie. A verzió azonosításához futtassa a következőt: `az --version`. A parancssori felület (CLI) telepítéséhez vagy legújabb verzióra történő frissítéséhez lásd: [Az Azure CLI 2.0 telepítése][azure-cli-install].

Az Azure Service Fabric Mesh parancssori felület bővítménymodulját az alábbi paranccsal telepítheti. 

```azurecli-interactive
az extension add --name mesh
```

Egy létező Azure Service Fabric Mesh parancssori felületi modult az alábbi paranccsal frissíthet.

```azurecli-interactive
az extension update --name mesh
```

A [Windows fejlesztési környezetet](service-fabric-mesh-howto-setup-developer-environment-sdk.md) is beállíthatja.

[azure-cli-install]: /cli/azure/install-azure-cli