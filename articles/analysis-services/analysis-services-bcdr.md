---
title: Alta disponibilidade do Azure Analysis Services | Microsoft Docs
description: Garantindo a alta disponibilidade do Azure Analysis Services.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: c4eb1162edc42baafe96e6c33699805ffc121204
ms.lasthandoff: 04/20/2017


---

# <a name="analysis-services-high-availability"></a>Alta disponibilidade do Analysis Services
Este artigo descreve como garantir alta disponibilidade para servidores do Azure Analysis Services. 


## <a name="assuring-high-availability-during-a-service-disruption"></a>Garantindo alta disponibilidade durante uma interrupção do serviço
Embora seja raro, um data center do Azure pode sofrer uma interrupção. Quando uma interrupção ocorre, ela causa uma parada nos negócios que pode durar alguns minutos ou até mesmo horas. A alta disponibilidade geralmente é obtida com redundância do servidor. Com o Azure Analysis Services, você pode obter redundância criando servidores secundários adicionais em uma ou mais regiões. Ao criar servidores redundantes, para garantir que os dados e metadados nesses servidores estejam em sincronia com o servidor em uma região que ficou offline, você pode:

* Implantar modelos em servidores redundantes em outras regiões. Esse método requer processamento de dados no servidor primário e servidores redundantes em paralelo, garantindo que todos os servidores estejam em sincronia.

* Bancos de dados de backup do seu servidor primário e restauração em servidores redundantes. Por exemplo, você pode automatizar backups noturnos no Armazenamento do Azure e restaurar para outros servidores redundantes em outras regiões. 

Em ambos os casos, se o servidor primário sofre uma interrupção, você deve alterar as cadeias de conexão em clientes de relatório para se conectar ao servidor em um datacenter regional diferente. Essa alteração deve ser considerada um último recurso e apenas caso ocorra uma interrupção catastrófica de data center regional. É mais provável que, após uma interrupção, o data center que hospeda o servidor primário fique online novamente antes de você poder atualizar conexões em todos os clientes. 

Ao determinar como a sua organização lida com uma interrupção do serviço, considere o modo como você pode garantir que seus dados sejam mantidos atualizados e protegidos. 


## <a name="related-information"></a>Informações relacionadas
[Backup e restauração](analysis-services-backup.md) 
[Gerenciar o Azure Analysis Services](analysis-services-manage.md) 


