---
title: "Conteúdo do SDK de aplicativos do Windows Universal"
description: "Saiba mais sobre o conteúdo do SDK dos aplicativos do Windows Universal para o Mobile Engagement do Azure"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8fa1b701-1c2b-4aec-940c-06c974ef5405
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ea47cba55c9689e23eb3592ae2559649d3377055
ms.lasthandoff: 11/17/2016


---
# <a name="windows-universal-apps-sdk-content"></a>Conteúdo do SDK de aplicativos do Windows Universal
Este documento lista e descreve o conteúdo implantado pelo SDK em seu aplicativo.

## <a name="the-resources-folder"></a>A pasta `/Resources`
Essa pasta contém todos os recursos que o Engagement precisa. Você também pode personalizá-los para se adequarem ao seu aplicativo.

* `EngagementConfiguration.xml` : Arquivo de configuração do Mobile Engagement, é onde você pode personalizar configurações do Mobile Engagement (cadeia de conexão do Mobile Engagement, relatar falhas, etc.).

### <a name="html-folder"></a>/pasta html
* `EngagementNotification.html` : O design HTLM do modo de exibição Web `Notification` para faixas no aplicativo.
* `EngagementAnnouncement.html` : O design HTLM do modo de exibição Web `Announcement` para exibições intersticiais no aplicativo.

### <a name="images-folder"></a>/pasta imagens
* `EngagementIconNotification.png` : O ícone de marca exibido à esquerda de uma notificação, substitua-o pelo ícone da sua marca.
* `EngagementIconOk.png` : O ícone `Ok` das páginas de conteúdo do reach para o botão de ação ou de validação.
* `EngagementIconNOK.png` : O ícone `NOK` usado quando o botão de validação das páginas de conteúdo do reach é desabilitado.
* `EngagementIconClose.png` : O ícone `Close` de notificações e conteúdos do reach para o botão descartar.

### <a name="overlay-folder"></a>/pasta sobreposição
* `EngagementPageOverlay.cs` : A página de sobreposição responsável por adicionar a interface do usuário no aplicativo de alcance do Engagement ao seu filho.


