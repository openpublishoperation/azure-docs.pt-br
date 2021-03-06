---
title: 'Locais e provedores de conectividade: Azure ExpressRoute | Microsoft Docs'
description: "Este artigo fornece uma visão geral detalhada dos locais onde os serviços são oferecidos e de como se conectar a regiões do Azure. Classificado por local."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
ms.assetid: feb67da3-5abc-4acb-bad4-f78e3c541ded
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/14/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 32b8cd51c983b2a66f3631ec399aa88e34555196
ms.lasthandoff: 04/17/2017

---
# <a name="expressroute-partners-and-peering-locations"></a>Locais de emparelhamento e parceiros da Rota Expressa

> [!div class="op_single_selector"]
> * [Locais por provedor](expressroute-locations.md)
> * [Provedores por local](expressroute-locations-providers.md)


As tabelas neste artigo fornecem informações sobre provedores de conectividade da Rota Expressa, a cobertura geográfica da Rota Expressa, serviços em nuvem da Microsoft com suporte ao longo da Rota Expressa e SIs (Integradores de Serviço) da Rota Expressa.

## <a name="partners"></a>Provedores de conectividade da Rota Expressa
Há suporte para Rota Expressa em todos os locais e regiões do Azure. O mapa a seguir fornece uma lista de locais de Rota Expressa e regiões do Azure. Os locais de Rota Expressa são aqueles onde a Microsoft emparelha-se a vários provedores de serviços.

![Mapa de localização][0]

Você terá acesso aos serviços do Azure em todas as regiões dentro de uma região geopolítica se estiver conectado a pelo menos um local de Rota Expressa dentro da região geopolítica. 

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Regiões do Azure para locais do ExpressRoute em uma região geopolítica
A tabela a seguir fornece um mapa das regiões do Azure para locais de Rota Expressa em uma região geopolítica.

| **Região Geopolítica** | **Regiões do Azure** | **Locais de Rota Expressa** |
| --- | --- | --- |
| **América do Norte** |Leste dos EUA, Oeste dos EUA, Leste dos EUA 2, Oeste dos EUA 2, Centro dos EUA, Centro-Sul dos EUA, Centro-Norte dos EUA, Centro-Oeste dos EUA, Centro do Canadá, Leste do Canadá |Atlanta, Chicago, Dallas, Las Vegas, Los Angeles, Nova York, Seattle, Vale do Silício, Washington, D.C., Montreal, Cidade de Quebec, Toronto |
| **América do Sul** |Sul do Brasil |São Paulo |
| **Europa** |Europa Setentrional, Europa Ocidental, Oeste do Reino Unido, Sul do Reino Unido |Amsterdã, Dublin, Londres, Newport(Gales), Paris |
| **Ásia** |Ásia Oriental, Sudeste Asiático |Hong Kong, Cingapura |
| **Japão** |Oeste do Japão, Leste do Japão |Osaka, Tóquio |
| **Austrália** |Sudeste da Austrália, Leste da Austrália |Melbourne, Sydney |
| **Índia** |Oeste da Índia, Índia Central, Sul da Índia |Chennai, Mumbai |
| **Coreia do Sul** |Coreia Central, Sul da Coreia |Busan, Seul |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Regiões e limites geopolíticos para nuvens nacionais
A tabela a seguir fornece informações sobre regiões e limites geopolíticos para nuvens nacionais.

| **Região Geopolítica** | **Regiões do Azure** | **Locais de Rota Expressa** |
| --- | --- | --- | --- |
| **Nuvem do Governo dos EUA** |Gov. EUA - Iowa, Gov. EUA - Virgínia, US DoD Central, US DoD Leste  |Chicago, Los Angeles, Nova York, Vale do Silício, Washington, D.C. |
| **China** |Norte da China, Leste da China |Pequim, Xangai |
| **Alemanha** |Alemanha Central, Alemanha Oriental |Berlim+, Frankfurt |

Não há suporte para conectividade entre regiões geopolíticas no SKU de Rota Expressa padrão. Você precisará habilitar o complemento premium de Rota Expressa para dar suporte a conectividade global. Não há suporte a conectividade para ambientes de nuvem nacionais. Você pode trabalhar com seu provedor de conectividade se surgir necessidade de fazê-lo.

## <a name="locations"></a>Locais de provedor de conectividade

A tabela a seguir mostra os locais de conectividade e os provedores de serviço para cada local. Se você quiser exibir provedores de serviços e os locais para os quais podem fornecer serviço, confira [Locais pelo provedor de serviços](expressroute-locations.md#locations). 


### <a name="production-azure"></a>Produção do Azure
| **Localidade** | **Provedores de Serviço** |
| --- | --- |
| **Amsterdã** |Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Internet Solutions - Cloud Connect, Interxion, KPN, Level 3 Communications, Orange, Tata Communications, TeleCity Group, Telefonica+, Telenor, Verizon |
| **Atlanta** |Equinix |
| **Busan** |LG CNS |
| **Chennai** |Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chicago** |AT&T NetBond, Comcast, Equinix, Level 3 Communications, Verizon, Zayo Group |
| **Dallas** |Aryaka Networks, AT&T NetBond, Cologix, Equinix, Level 3 Communications, Megaport, Verizon, Zayo Group+ |
| **Dublim** |Colt, TeleCity Group |
| **Hong Kong** |Aryaka Networks, British Telecom, China Telecom Global, Equinix, Megaport, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Las Vegas** |Level 3 Communications+, Megaport+ |
| **Londres** |AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, MTN, NTT Communications, Orange, Tata Communications, Telecity Group, Telehouse - KDDI, Telenor, Verizon, Vodafone, Zayo Group+ |
| **Los Angeles** |CoreSite, Equinix, Megaport, NTT, Zayo Group |
| **Melbourne** |AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **Nova Iorque** |Coresite, Equinix, Megaport, Zayo Group |
| **Newport (País de Gales)** |Dados da Próxima Geração |
| **Montreal** |Bell Canada, Cologix |
| **Mumbai** |Airtel+, Tata Communications |
| **Osaka** |Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank |
| **Paris** |Interxion, Equinix+ |
| **Cidade de Quebec** | Megaport |
| **São Paulo** |Equinix, Telefonica |
| **Seattle** |Equinix, Level 3 Communications, Megaport |
| **Seul** |KINX, Sejong Telecom |
| **Vale do Silício** |Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink+, Comcast, Console, Equinix, Level 3 Communications, Orange, Tata Communications, Verizon, Zayo Group |
| **Cingapura** |Aryaka Networks, AT&T NetBond, British Telecom, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Verizon |
| **Sydney** |AARNet, AT&T NetBond, British Telecom, Equinix, Megaport, NEXTDC, Orange, Telstra Corporation, Verizon |
| **Tóquio** |Aryaka Networks, British Telecom, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank, Verizon |
| **Toronto** |Bell Canada, Cologix, Console, Equinix, Megaport, Zayo Group |
| **Washington, D.C.** |Aryaka Networks, AT&T NetBond, British Telecom, Comcast, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, Verizon, Zayo Group |

 **+** indica que haverá em breve

### <a name="national-cloud-environments"></a>Ambientes de nuvem nacionais

### <a name="us-government-cloud"></a>Nuvem do Governo dos EUA
| **Localidade** | **Provedores de Serviço** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |Equinix, Megaport, Verizon |
| **Nova Iorque** |Equinix, Level 3 Communications+, Verizon |
| **Vale do Silício** | Equinix |
| **Seattle** | Equinix+ |
| **Washington, D.C.** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |

### <a name="china"></a>China
| **Localidade** | **Provedores de Serviço** |
| --- | --- |
| **Pequim** |China Telecom |
| **Xangai** |China Telecom |

Para saber mais, consulte [Rota Expressa na China](http://www.windowsazure.cn/home/features/expressroute/)

### <a name="germany"></a>Alemanha
| **Localidade** | **Provedores de Serviço** |
| --- | --- |
| **Berlim** |Colt+, e-shelter, Megaport+ |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="c1partners"></a>Conectividade por meio de provedores de serviços não listados
Se seu provedor de conectividade não estiver listado em seções anteriores, você ainda pode criar uma conexão.

* Verifique com seu provedor de conectividade para ver se eles estão conectados a qualquer um dos Exchanges na tabela acima. Você pode verificar os links a seguir para obter mais informações sobre os serviços oferecidos por provedores do Exchange. Vários provedores de conectividade já estão conectados a Exchanges de Ethernet.
  * [Cologix](http://www.cologix.com/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](http://www.interxion.com/)
  * [NextDC](http://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Faça com que seu provedor de conectividade estenda sua rede para o local de emparelhamento de sua escolha.
  * Certifique-se de que seu provedor de conectividade estenda sua conectividade de maneira altamente disponível para que não exista nenhum ponto de falha.
* Solicite um circuito da Rota Expressa com o Exchange como o provedor de conectividade para conectar-se à Microsoft.
  * Siga as etapas em [Criar um circuito da Rota Expressa](expressroute-howto-circuit-classic.md) para configurar a conectividade.

| **Localidade** | **Exchange** | **Provedores de conectividade** |
| --- | --- | --- |
| **Amsterdã** | Equinix, Telecity | Eurofiber , Fastweb S.p.A, Nianet |
| **Chicago** | Equinix | Windstream |
| **Dallas** | Equinix, Megaport | C3ntro, Data Foundry, Transtelco |
| **Frankfurt** | Telecity | Nianet, QSC AG |
| **Londres** | Equinix, euNetworks, Telecity | Bezeq International Ltd., Exponential E, HSO, NexGen Networks, Tamares Telecom |
| **Los Angeles** | Equinix |Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix, Equinix | Airgate Technologies. Inc, Cogeco Peer 1 |
| **Nova Iorque** |Equinix | Lightower |
| **Seattle** |Equinix | Alaska Communications |
| **Vale do Silício** |Equinix | Windstream |
| **Cingapura** |Equinix |1CLOUDSTAR, Epsilon Telecommunications Limited |
| **Slough** | Equinix | HSO|
| **Sydney** | Megaport | Macquarie Telecom Group|
| **Tóquio** | Equinix | ARTERIA Networks Corporation |
| **Toronto** | Equinix | Airgate Technologies. Inc, Cogeco Peer 1 |
| **Washington, D.C.** |Equinix | Lightower, Masergy, Windstream |

## <a name="expressroute-system-integrators"></a>Integradores de sistema de Rota Expressa
Habilitar a conectividade privada para atender às suas necessidades pode ser desafiador, dependendo da escala de sua rede. Você pode trabalhar com qualquer dos integradores de sistema listados na tabela a seguir para ajudá-lo com integração à Rota Expressa.

| **Continent** | **Integradores do sistema** |
| --- | --- |
| **Ásia** |Avanade Inc., OneAs1a |
| **Austrália** | IT Consultancy, Vigilant.IT |
| **Europa** |Avanade Inc., Altogee, Bright Skies GmbH, Dotnet Solutions, MSG Services, Nelite, Orange Networks, sol-tec |
| **América do Norte** |Avanade Inc., Equinix Professional Services, Perficient, Presidio, Project Leadership |
| **América do Sul** |Avanade Inc. |
## <a name="next-steps"></a>Próximas etapas
* Para obter mais informações sobre a Rota Expressa, consulte [Perguntas Frequentes sobre Rota Expressa](expressroute-faqs.md).
* Certifique-se que todos os pré-requisitos foram atendidos. Consulte [Pré-requisitos da Rota Expressa](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa de localização"

