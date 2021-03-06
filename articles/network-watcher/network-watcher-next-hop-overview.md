---
title: "Introdução ao próximo salto no Observador de Rede do Azure | Microsoft Docs"
description: "Esta página fornece uma visão geral da capacidade do próximo salto do Observador de Rede"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: 864185e62fb6c3cef4116824b36ee7e5d3447662
ms.lasthandoff: 03/04/2017

---

# <a name="introduction-to-next-hop-in-azure-network-watcher"></a>Introdução ao próximo salto no Observador de Rede do Azure

O tráfego de uma VM é enviado para um destino com base nas rotas efetivas associadas a uma NIC. O próximo salto obtém o tipo do próximo salto e o endereço IP de um pacote em uma máquina virtual específica e na NIC. Isso ajuda a determinar se o pacote está sendo direcionado para o destino ou se o tráfego está tornando-se um buraco negro. Uma configuração incorreta das rotas feita pelo usuário, onde um tráfego é direcionado para um caminho local ou um dispositivo virtual, pode levar a problemas de conectividade. O próximo salto também retorna a tabela de rotas associada ao próximo salto. Ao consultar um próximo salto, se a rota for especificada como sendo definida pelo usuário, ela será retornada. Caso contrário, o Próximo salto retornará a "Rota do Sistema".

![visão geral do próximo salto][1]

A seguir está uma lista dos tipos de próximos salto que podem ser retornados ao consultar o Próximo salto.

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Nenhum

### <a name="next-steps"></a>Próximas etapas

Saiba como usar o próximo salto para localizar problemas de conectividade de rede visitando [Verificar o próximo salto em uma VM](network-watcher-check-next-hop-portal.md)

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png














