---
title: Tamanhos da VM Linux no Azure | Microsoft Docs
description: "Lista os tamanhos diferentes disponíveis de máquinas virtuais do Linux no Azure."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: d068cf9b6b2fd9084ffc60cac4837aa378a09c79
ms.lasthandoff: 04/12/2017

---

# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Tamanhos das máquinas virtuais do Linux no Azure
Este artigo descreve os tamanhos e as opções disponíveis de máquinas virtuais do Azure que você pode usar para executar seus aplicativos Linux e cargas de trabalho. Ele também fornece considerações de implantação a serem observadas ao planejar o uso desses recursos. Este artigo também está disponível para [máquinas virtuais do Windows](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!IMPORTANT]
> * Para obter informações sobre os preços dos vários tamanhos, consulte [Preços de máquinas virtuais](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux). 
> * Para ver a disponibilidade de tamanhos de VM nas regiões do Azure, confira [Produtos disponíveis por região](https://azure.microsoft.com/regions/services/).
> * Para ver os limites gerais em VMs do Azure, consulte [Limites de assinatura e serviços do Azure, cotas e restrições](../../azure-subscription-service-limits.md).
> * Saiba mais sobre como as [ACUs (unidade de computação do Azure)](../windows/acu.md) podem ajudar você a comparar o desempenho de computação entre SKUs do Azure.
> 
> 

<br>   


| Tipo                     | Tamanhos           |    Descrição       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Propósito geral](sizes-general.md)          | DSv2, Dv2, DS, D, Av2, A0-7,  | Relação equilibrada de CPU/memória. Ideal para teste e desenvolvimento, bancos de dados pequenos a médios e servidores Web de tráfego baixo a médio. |
| [Computação otimizada](sizes-compute.md)        | Fs, F             | Alta relação de CPU/memória. Boa para servidores Web de tráfego médio, dispositivos de rede, processos de lote e servidores de aplicativo.        |
| [Memória otimizada](sizes-memory.md)         | GS, G, DSv2, DS, Dv2, D   | Alta relação de memória/núcleo. Ótima para servidores de banco de dados relacionais, caches médios a grandes e análises na memória.                 |
| [Armazenamento otimizado](sizes-storage.md)        | Ls                | Alta taxa de transferência de disco e de E/S. Ideal para Big Data, SQL e bancos de dados NoSQL.                                                         |
| [GPU](sizes-gpu.md)            | NV, NC            | Máquinas virtuais especializadas, destinadas para renderização gráfica e edição de vídeo pesadas. Disponível com uma ou várias GPUs.       |
| [Computação de alto desempenho](sizes-hpc.md) | H, A8-11          | Nossas máquinas virtuais de CPU mais rápidas e potentes com adaptadores de rede de alta taxa de transferência (RDMA) opcionais. 

<br>

Saiba mais sobre como as [ACUs (unidade de computação do Azure)](../windows/acu.md) podem ajudar você a comparar o desempenho de computação entre SKUs do Azure.

Saiba mais sobre os diferentes tamanhos de VM que estão disponíveis:
- [Propósito geral](sizes-general.md)
- [Computação otimizada](sizes-compute.md)
- [Memória otimizada](sizes-memory.md)
- [Armazenamento otimizado](sizes-storage.md)
- [GPU](sizes-gpu.md)
- [Computação de alto desempenho](sizes-hpc.md)




