---
title: Modellek üzembe helyezése IoT Edge-eszközök az Azure Machine Learning szolgáltatásból származó |} A Microsoft Docs
description: 'Útmutató: a modell üzembe helyezése az Azure Machine Learning szolgáltatásból származó Azure IoT Edge-eszközökön. Az edge-eszköz üzembe helyezéséhez lehetővé teszi adatok pontozása helyett a felhő felé, és a választ vár az eszközön.'
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: shipatel
author: shivanipatel
manager: cgronlun
ms.reviewer: larryfr
ms.date: 09/24/2018
ms.openlocfilehash: 03d692ddfd6f41fd559e9b921f0214a9cd2ada22
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225225"
---
# <a name="prepare-to-deploy-models-on-iot-edge"></a>Az IoT Edge-ben a modellek üzembe helyezés előkészítése

Ebből a dokumentumból megtudhatja, hogyan készítse elő a betanított modell egy Azure IoT Edge-eszközön való üzembe helyezéshez az Azure Machine Learning szolgáltatás használatával.

Az Azure IoT Edge-eszköz, Linux vagy Windows-alapú eszköz, amely az Azure IoT Edge-futtatókörnyezet. Machine learning-modellek IoT Edge-modulok ezekre az eszközökre telepíthető. Az IoT Edge-eszköz üzembe helyezéséhez lehetővé teszi, hogy az eszköz a modell használatának közvetlenül, ahelyett, hogy adatokat küldeni a felhőbe feldolgozásra. Gyorsabb válaszidőt és alacsonyabb az adatforgalom kap.

Mielőtt üzembe helyezéséhez egy edge-eszközön, a jelen dokumentumban leírt lépések használatával a modell és az eszköz előkészítése.

## <a name="prerequisites"></a>Előfeltételek

* Azure-előfizetés. Ha még nincs előfizetése, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), mielőtt hozzákezd.

* Az Azure Machine Learning szolgáltatás munkaterületén. Szereplő lépések segítségével hozzon létre egyet, a [Ismerkedés az Azure Machine Learning szolgáltatás](quickstart-get-started.md) dokumentumot.

* A fejlesztési környezet. További információkért lásd: a [a fejlesztési környezet konfigurálása](how-to-configure-environment.md) dokumentumot.

* Egy [Azure IoT Hub](../../iot-hub/iot-hub-create-through-portal.md) az Azure-előfizetésében. 

* Betanított modell. Találhat egy példát a modell betanítására, a [betanításához egy kép osztályozási modell az Azure Machine Learning](tutorial-train-models-with-aml.md) dokumentumot.

## <a name="prepare-the-iot-device"></a>Az IoT-eszköz előkészítése

Lépéseit követve megtudhatja, hogyan regisztrálja az eszközt, és az IoT-modul telepítése a [a rövid útmutató: Linux x64 eszközre az első IoT Edge-modul telepítéséhez](../../iot-edge/quickstart-linux.md) dokumentumot.

## <a name="register-the-model"></a>Regisztrálja a modellt

Az Azure IoT Edge-modulok tárolórendszerképek alapulnak. IoT Edge-eszköz helyezheti üzembe a modellt, regisztrálja a modellt az Azure Machine Learning-munkaterület és a Docker-rendszerkép létrehozásához használja az alábbi lépéseket. 

> [!IMPORTANT]
> Ha használta az Azure Machine Learning, előfordulhat, hogy már regisztrálva van a munkaterület modellje betanításához, ebben az esetben ugorjon a 3. lépés.

1. A munkaterület inicializálása, és a config.json fájl tölthető be:

    ```python
    from azureml.core  import Workspace

    #Load existing workspace from the config file info.
    ws  = Workspace.from_config()
    ```    

1. Regisztrálja a modellt egyszerűen a munkaterületre. Cserélje le az alapértelmezett szöveget a modell elérési útja, neve, a címkék és leírása:

    ```python
    from azureml.core.model import Model
    model = Model.register(model_path = "model.pkl", # this path points to the local file
                        model_name = "best_model", # the model gets registered as this name
                        tags = {'attribute': "myattribute", 'classification': "myclassification"},
                        description = "My awesome model",
                        workspace = ws)
    ```    

1. Kérje le a regisztrált modell: 

    ```python
    from azureml.core.model import Model

    model_name = "best_model"
    model = Model(ws, model_name)                     
    ```    

## <a name="create-a-docker-image"></a>Hozzon létre egy Docker-rendszerképet.

1. Hozzon létre egy **pontozó szkript** nevű `score.py`. Ez a fájl a lemezkép lévő modell futtatására szolgál. A következő feladatokat kell tartalmaznia:

    * A `init()` függvény, amely általában egy globális objektum betölti a modellt. Ez a függvény fut, csak egyszer, amikor a Docker-tároló elindult. 

    * A `run(input_data)` függvény egy értéket a bemeneti adatok alapján előre jelezni a modellt használja. Bemenetek és kimenetek a futtató általában használjon JSON-szerializálás és deszerializálás, de más formátum támogatott.

    Egy vonatkozó példáért tekintse meg a [kép besorolási oktatóanyag](tutorial-deploy-models-with-aml.md#make-script).

1. Hozzon létre egy **környezet fájl** nevű `myenv.yml`. Ez a fájl egy Conda-környezet specifikációt, és felsorolja az összes, a függőségeket, a parancsfájl és a modell által igényelt. Egy vonatkozó példáért tekintse meg a [kép besorolási oktatóanyag](tutorial-deploy-models-with-aml.md#make-myenv).

1. A Docker rendszerkép használatával konfigurálja a `score.py` és `myenv.yml` fájlok:
    
    ```python
    from azureml.core.image import Image, ContainerImage
    
    #Image configuration
    image_config = ContainerImage.image_configuration( runtime = "python", 
                           execution_script = "score.py",
                           conda_file = "myenv.yml", 
                           tags = {"attributes", "calssification"},
                           description = "Image that contains my model",
                           
                        )
    ```    

1. A modell- és képfájlok konfiguráció használatával létrehozni:

    ```python
    image = ContainerImage.create (name = "myimage", 
                           models = [model], #this is the model object
                           image_config = image_config,
                           workspace = ws
                        )
    ```     

    A lemezkép létrehozása körülbelül 5 percet vesz igénybe.

## <a name="get-the-container-registry-credentials"></a>A container registry hitelesítő adatainak lekérése

Az Azure IoT a tárolóregisztrációs adatbázisra, amely az Azure Machine Learning szolgáltatás a docker-rendszerképek tárolja a hitelesítő adatokat kell. Az alábbi lépések segítségével hitelesítő adatainak lekérése:

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com/signin/index).

1. Nyissa meg az Azure Machine Learning-munkaterületet, és válassza ki __áttekintése__. Nyissa meg a tároló beállításjegyzék-beállításokat, jelölje be a __beállításjegyzék__ hivatkozásra.

    ![A tároló beállításjegyzék-bejegyzés képe](./media/how-to-deploy-to-iot/findregisteredcontainer.png)

1. Egyszer a container registry esetében válassza **Tárelérési kulcsok** , majd engedélyezze a rendszergazdai felhasználót.

    ![A hozzáférési kulcsok képernyő képe](./media/how-to-deploy-to-iot/findaccesskey.png)

1. Mentse az értékeket, a bejelentkezési kiszolgáló, a felhasználónevet és jelszót. 

   Ezek a hitelesítő adatok szükségesek a privát tárolójegyzékben található rendszerképek az IoT edge elérése.

## <a name="next-steps"></a>További lépések

Felkészülés az üzembe helyezés sikeresen befejeződött. Most már szereplő lépések segítségével a [üzembe helyezése az Azure IoT Edge-modulok az Azure Portalról](../../iot-edge/how-to-deploy-modules-portal.md) a dokumentumot, az edge-eszköz üzembe helyezése. Amikor konfigurálja a __beállításjegyzék-beállítások__ az eszközhöz, használja a korábban konfigurált hitelesítő adatokat.
