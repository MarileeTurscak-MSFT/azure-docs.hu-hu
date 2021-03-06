---
title: VPN gateway beállításairól az Azure Stackhez |} A Microsoft Docs
description: A VPN-átjárók használhatja az Azure Stack beállítások ismertetése.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: fa8d3adc-8f5a-4b4f-8227-4381cf952c56
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/14/2018
ms.author: brenduns
ms.openlocfilehash: 6380936766bb0f3848811be305783c274867b0fc
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/01/2018
ms.locfileid: "43381867"
---
# <a name="vpn-gateway-configuration-settings-for-azure-stack"></a>VPN gateway konfigurációs beállításairól az Azure Stackhez

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

VPN-átjáró olyan virtuális hálózati átjáró, amely a virtuális hálózat az Azure Stackben és a távoli VPN gateway titkosított adatforgalmat továbbít. A távoli VPN-átjáró az Azure-ban, egy a helyi adatközpontban vagy egy másik helyen található eszköz lehet.  Ha a két végpontok közötti hálózati kapcsolat, a két hálózat közötti biztonságos Site-to-Site (S2S) VPN-kapcsolatot is létrehozhat.

VPN gateway-kapcsolat támaszkodik a konfiguráció több erőforrást, amelyek mindegyike tartalmazza a konfigurálható beállítások. Ez a cikk ismerteti az erőforrások és a Resource Manager-alapú üzemi modellben létrehozott virtuális hálózat VPN-átjáró szoftverközponthoz kapcsolódó beállításokat. Leírások és topológia-diagramok megtalálhatja az összes kapcsolat-megoldás [információk a VPN Gateway az Azure Stack](azure-stack-vpn-gateway-about-vpn-gateways.md).

## <a name="vpn-gateway-settings"></a>A VPN gateway beállításairól

### <a name="gateway-types"></a>Átjárótípusok

Minden egyes Azure Stack virtuális hálózat egyetlen virtuális hálózati átjáró, amely a típusúnak kell lennie támogatja **Vpn**.  Ez a támogatás az Azure-ban, amely támogatja a további eltér.  

A virtuális hálózati átjáró létrehozásakor, győződjön meg arról, hogy az átjáró típusa megfeleljen a konfigurációnak. VPN-átjáró szükséges a `-GatewayType Vpn`, például:

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn
-VpnType RouteBased
```

### <a name="gateway-skus"></a>Gateway termékváltozatok

Egy virtuális hálózati átjáró létrehozásakor meg kell adni a használni kívánt termékváltozatot. Válassza ki a számítási feladatok, a teljesítmény, a funkciók és a szolgáltatói szerződés igényeinek megfelelő termékváltozatokat.

Az Azure stackhez VPN-átjáró SKU-k az alábbi táblázatban látható.

|   | VPN Gateway teljesítménye |Maximális VPN Gateway IPsec-alagutak |
|-------|-------|-------|
|**Alapszintű Termékváltozat**  | 100 Mbps  | 10    |
|**A standard Termékváltozat**           | 100 Mbps  | 10    |
|**Nagy teljesítményű Termékváltozat** | 200 Mbps    | 5 |

### <a name="resizing-gateway-skus"></a>Az átjárók Termékváltozatainak átméretezése

Az Azure Stack egy, a támogatott örökölt termékváltozatok között termékváltozatok átméretezése nem támogatja.

Ehhez hasonlóan az Azure Stack nem támogatja az egy átméretezése egy újabb termékváltozatra (VpnGw1, VpnGw2 és VpnGw3.) az Azure által támogatott a támogatott régi Termékváltozat (alapszintű, Standard és HighPerformance)

### <a name="configure-the-gateway-sku"></a>Az átjáró-Termékváltozatot konfigurálása

#### <a name="azure-stack-portal"></a>Az Azure Stack portálon

Ha az Azure Stack portálon használatával a Resource Manager virtuális hálózati átjáró létrehozása, az átjáró-Termékváltozatot is választhat a legördülő lista használatával. Megjelenik a beállításokat az átjáró típusa és a VPN-típust választja felelnek meg.

#### <a name="powershell"></a>PowerShell

Az alábbi PowerShell-példa meghatározza az **- GatewaySku** VpnGw1.

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1
-GatewayType Vpn -VpnType RouteBased
```

### <a name="connection-types"></a>Kapcsolattípusok

A Resource Manager-alapú üzemi modellben az egyes konfigurációkhoz egy adott virtuális hálózati átjárójának kapcsolattípusa van szükség. A rendelkezésre álló Resource Manager PowerShell-értékei **- ConnectionType** vannak:

* IPsec

A következő PowerShell-példa az IPsec kapcsolati típust igénylő S2S kapcsolat jön létre.

```PowerShell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

### <a name="vpn-types"></a>VPN-típusok

A VPN-átjáró konfigurálása a virtuális hálózati átjáró létrehozásakor meg kell adnia egy VPN-típussal. A VPN-típust választja a létrehozni kívánt kapcsolat topológia függ.  Egy VPN-típussal is függ a hardvert használ. S2S-konfigurációk VPN-eszközre van szükség. VPN-eszközök csak egy meghatározott VPN-típust támogatja.

> [!IMPORTANT]  
> Jelenleg az Azure Stack csak az útvonal-alapú VPN-típust támogatja. Ha az eszköz csak a házirend-alapú VPN-eket támogatja, az Azure Stack az adott eszközökre való kapcsolatok nem támogatottak.  
>
> Emellett az Azure Stack nem támogatja a házirend alapú Forgalomválasztóinak útvonal-alapú átjárók használata jelenleg, mert az egyéni IPSec/IKE-házirend konfigurációk nem támogatottak.

* **Házirendalapú**: Csoportházirend-alapú VPN-eket titkosítják és irányítják a csomagokat a helyszíni hálózat és az Azure Stack VNet közötti címelőtag-kombinációkkal konfigurált IPsec-házirendek alapján IPsec-alagutakon keresztül. A házirend vagy forgalomválasztó, az általában a VPN-eszköz konfigurálása a hozzáférés-lista.

  >[!NOTE]
  >Házirendalapú támogatott az Azure-ban, de nem az Azure Stackben.

* **Útvonalalapú**: az IP-cím továbbítási vagy útvonalválasztási táblán irányítják a csomagokat a megfelelő alagútkapcsolatokhoz irányítsák a konfigurált Útvonalalapú VPN-ekre használható útvonalak. Az alagútkapcsolatok ezután titkosítják vagy visszafejtik az alagutakba bemenő vagy onnan kijövő csomagokat. A házirend vagy forgalomválasztó, Útvonalalapú VPN-EK megtörténik-bármely, (vagy helyettesítő karakterek használata.) Alapértelmezés szerint ezek nem lehet módosítani. Útvonalalapú VPN-típus értéke Útvonalalapú.

Az alábbi PowerShell-példa meghatározza az **- VpnType** , Útvonalalapú. Az átjáró létrehozásakor meg kell győződnie arról, hogy a **- VpnType** megfeleljen a konfigurációnak.

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig
-GatewayType Vpn -VpnType RouteBased
```

### <a name="gateway-requirements"></a>Az átjáróra vonatkozó követelmények

A következő táblázat felsorolja a VPN-átjárók követelményei.

| |Házirendalapú alapszintű VPN Gateway | Alapszintű Útvonalalapú VPN-átjáró | Standard Útvonalalapú VPN-átjáró | Útvonalalapú nagy teljesítményű VPN Gateway|
|--|--|--|--|--|
| **Helyek közötti kapcsolat (S2S-kapcsolatokhoz)** | Nem támogatott | Útvonalalapú VPN-konfiguráció | Útvonalalapú VPN-konfiguráció | Útvonalalapú VPN-konfiguráció |
| **Hitelesítési módszer**  | Nem támogatott | Előre megosztott kulcs S2S-kapcsolatokhoz  | Előre megosztott kulcs S2S-kapcsolatokhoz  | Előre megosztott kulcs S2S-kapcsolatokhoz  |   
| **S2S-kapcsolatok maximális száma**  | Nem támogatott | 10 | 10| 5|
|**Aktív útválasztás-támogatás (BGP)** | Nem támogatott | Nem támogatott | Támogatott | Támogatott |

### <a name="gateway-subnet"></a>Átjáró alhálózata

Mielőtt létrehozna egy VPN-átjárót, létre kell hoznia egy átjáró-alhálózatot. Az átjáró-alhálózat tartalmaz IP-címek a virtuális hálózati átjáró virtuális gépek és szolgáltatások. A virtuális hálózati átjáró létrehozásakor átjáróként működő virtuális gépekhez az átjáró-alhálózat telepítve és konfigurálva a VPN-átjáró szükséges beállításokat. **Nem** bármi más (például további VM-EK) üzembe helyezése az átjáró-alhálózat.

>[!IMPORTANT]
>A megfelelő működéshez az átjáró-alhálózat neve **GatewaySubnet** kell legyen. Az Azure Stack azonosításához, azt az alhálózatot a virtuális hálózati átjáró virtuális gépek és szolgáltatások telepítése, ezt a nevet használja.

Az átjáróalhálózat létrehozásakor meg kell adnia, hogy hány IP-címet tartalmaz az alhálózat. Az átjáró-alhálózat IP-címekkel kiosztott az átjáróként működő virtuális gépekhez és átjárószolgáltatásokat. Egyes konfigurációknak a többinél nagyobb számú IP-címre van szükségük. Tekintse meg a konfigurációt, amelyet szeretne létrehozni, és győződjön meg arról, hogy szeretne létrehozni az átjáró-alhálózat megfelel ezeknek a követelményeknek megfelelő utasításokat.

Ezenkívül győződjön meg arról, hogy az átjáróalhálózat elég IP-cím további jövőbeni konfigurációk kezelésére van. Is hozhat létre egy átjáró-alhálózat kisebb, akár/29 méretű, azt javasoljuk, hogy hozzon létre egy átjáróalhálózatot/28-as vagy nagyobb (/ 28, / 27, / 26 stb.) Így ha funkciókat adhat a jövőben nem kell az átjáró üzemen, majd törölje és hozza létre újra az átjáró-alhálózat, hogy a további IP-címek.

A következő Resource Manager PowerShell-példa bemutatja egy GatewaySubnet nevű átjáró-alhálózatot. Láthatja, hogy a CIDR-jelölésrendszer/27-es, amely lehetővé teszi, hogy elegendő IP-címet, amely a jelenleg létező legtöbb konfiguráció esetében.

```PowerShell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

> [!IMPORTANT]
> Amikor átjáró-alhálózatokkal dolgozik, kerülje a hálózati biztonsági csoportok (NSG) társítását az átjáró-alhálózathoz. A hálózati biztonsági csoportok és az alhálózat társítása esetén előfordulhat, hogy a VPN Gateway nem fog a várt módon működni. Hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [Mi az a hálózati biztonsági csoport?](/azure/virtual-network/virtual-networks-nsg).

### <a name="local-network-gateways"></a>Helyi hálózati átjárók

Egy VPN-átjáró konfigurálása az Azure-beli létrehozásakor az a helyi hálózati átjáró gyakran a helyszíni helyet jelöli. Az Azure Stackben minden olyan távoli VPN-eszköz, amely az Azure Stack kívül jelöli. Ez lehet az Adatközpont (vagy egy távoli adatközpontban) VPN-eszközt, vagy a VPN-átjáró, az Azure-ban.

Nevezze el a helyi hálózati átjárót, a VPN-eszköz nyilvános IP-címét, és adja meg a címelőtagokat a helyszíni helyen található. Az Azure megvizsgálja a hálózati forgalmat a cél-címelőtagjainak, consults szintűre frissül, hogy a helyi hálózati átjáró a megadott konfigurációval, és ennek megfelelően irányítja a csomagokat.

A következő PowerShell-példa egy új helyi hálózati átjárót hoz létre:

```PowerShell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Néha szüksége a helyi hálózati átjáró beállításainak módosítására. Például ha felvétele vagy módosítása a címtartomány, vagy ha a VPN-eszköz IP-címe megváltozik. Lásd: [PowerShell-lel a helyi hálózati átjáró beállításainak módosítása](/azure/vpn-gateway/vpn-gateway-modify-local-network-gateway).

## <a name="ipsecike-parameters"></a>IPsec/IKE-paraméterek

Egy VPN-kapcsolatot az Azure Stack üzembe helyezésekor kell konfigurálni a kapcsolat mindkét végén.  Ha konfigurál egy VPN-kapcsolatot az Azure Stack és a egy hardvereszközhöz, például egy kapcsoló vagy egy VPN-átjáróként működő útválasztó között, a eszköz kérheti a további beállításokat.

Eltérően az Azure-ban, amely támogatja a több ajánlattal is egy kezdeményező és egy válaszadó, az Azure Stack is támogatja a csak egyszer veheti igénybe.

### <a name="ike-phase-1-main-mode-parameters"></a>Az IKE 1. fázis (Elsődleges mód) paraméterei

| Tulajdonság              | Érték|
|-|-|
| IKE verziószám           | IKEv2 |
|Diffie-Hellman Group   | 2. csoport (1024 bites) |
| Hitelesítési módszer | Előre megosztott kulcs |
|Titkosító és kivonatoló algoritmus | AES256, SHA256 |
|SA élettartama (Idő)     | 28 800 másodperc|

### <a name="ike-phase-2-quick-mode-parameters"></a>Az IKE 2. fázis (Gyors mód) paraméterei

| Tulajdonság| Érték|
|-|-|
|IKE verziószám |IKEv2 |
|Titkosító és kivonatoló algoritmus (titkosítás)     | GCMAES256|
|Titkosító és kivonatoló algoritmus (hitelesítés) | GCMAES256|
|SA élettartama (Idő)  | 27 000 másodperc  |
|SA élettartama (bájt) | 33,553,408     |
|Sérülés utáni titkosságvédelem (PFS) |Nincs<sup>lásd: 1. megjegyzést:</sup> |
|Kapcsolat megszakadásának észlelése | Támogatott|  

* *1. Megjegyzés:* 1807 verziónál régebbi, az Azure Stack használ PFS2048 értéke az a teljes előre sérülés utáni Titkosságvédelmi (PFS).

## <a name="next-steps"></a>További lépések

[Csatlakozás ExpressRoute használatával](azure-stack-connect-expressroute.md)
