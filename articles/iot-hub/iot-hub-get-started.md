---
title: Az Azure IoT Hub – Ismerkedés az IoT-eszközök csatlakoztatása a felhőhöz |} A Microsoft Docs
description: Ismerje meg, hogyan csatlakozhat az Azure IoT Hub az IoT-kártyák és a starter Kit. Az eszközök küldhet telemetriát az IoT Hub és az IoT Hub monitorozni és kezelni az eszközöket.
author: dominicbetts
manager: timlt
keywords: az Azure iot hub-oktatóanyag
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: 77abe7e2187a3cb28b326ffa833a856625d6c33d
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125192"
---
# <a name="azure-iot-hub-get-started-with-real-devices"></a>Az Azure IoT Hub – első lépések valódi eszközön

Ezek az útmutatók ismerkedjenek meg az Azure IoT Hub és az eszköz SDK-k valódi eszközökön futó.

## <a name="set-up-your-device"></a>Az eszköz beállítása

Csatlakozás az IoT-eszköz vagy az Azure IoT Hub-átjáró:

| IoT-eszköz                       | Programozási nyelv |
|----------------------------------|----------------------|
| Raspberry Pi                     | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| IoT fejlesztői készlet                       | [Arduino a vscode-ban][DevKit]     |
| Intel Edison                     | [NODE.js][Ed_Nd], [C][Ed_C]    |
| Adafruit Feather HUZZAH ESP8266  | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 Thing Dev       | [Arduino][Th_Ard]              |
| Adafruit Feather M0              | [Arduino][M0_Ard]              |
| Online készülékszimulátort         | [Raspberry Pi (Node.js)][Ol_Sim] |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md
