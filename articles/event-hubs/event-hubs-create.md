---
title: Criar um hub de eventos do Azure | Microsoft Docs
description: Criar um namespace de Hubs de Eventos do Azure e um hub de eventos usando o Portal do Azure
services: event-hubs
documentationcenter: na
author: jtaubensee
manager: timlt
editor: 
ms.assetid: ff99e327-c8db-4354-9040-9c60c51a2191
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/25/2017
ms.author: jotaub
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 23df3a3a11d8f065d6ce2a4f14ba175d6c781ee9
ms.lasthandoff: 04/18/2017

---

# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a>Criar um namespace de Hubs de Eventos e um hub de eventos usando o Portal do Azure

## <a name="create-an-event-hubs-namespace"></a>Criar um namespace de Hubs de Eventos
1. Faça logon no [portal do Azure][Azure portal] e clique em **Novo** na parte superior esquerda da tela.
1. Clique em **Internet das Coisas** e, em seguida, clique em **Hubs de Eventos**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. Na folha **Criar um namespace** , insira um nome de namespace. O sistema imediatamente verifica para ver se o nome está disponível.
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. Depois de verificar se o nome do namespace está disponível, escolha o tipo de preço (Básico ou Standard). Além disso, escolha uma assinatura do Azure, o grupo de recursos e o local no qual o recurso será criado. 
1. Clique em **Criar** para criar o namespace.
1. Na lista de namespaces do Hubs de Eventos, clique no namespace recém-criado.      
   
    ![](./media/event-hubs-create/create-event-hub2.png)
1. Na folha do namespace, clique em **Hubs de Eventos**.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

## <a name="create-an-event-hub"></a>Criar um Hub de Evento

1. Na parte superior da folha, clique em **Adicionar Hub de Eventos**.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. Digite um nome para seu hub de eventos e clique em **Criar**.
   
    ![](./media/event-hubs-create/create-event-hub5.png)
1. Na lista de hubs de eventos, clique no nome do hub de eventos recém-criado. 
    
    ![](./media/event-hubs-create/create-event-hub6.png)
1. De volta na folha do namespace (não na folha do hub de eventos específico), clique em **Políticas de acesso compartilhado** e, em seguida, clique em **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)
1. Clique no botão de cópia para copiar a cadeia de conexão **RootManageSharedAccessKey** na área de transferência. Salve essa cadeia de conexão para usá-la posteriormente no tutorial.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

Seu hub de eventos foi criado, e você tem as cadeias de conexão que precisa para enviar e receber eventos.

## <a name="next-steps"></a>Próximas etapas
Para saber mais sobre os Hubs de Eventos, consulte estes links:

* [Visão Geral dos Hubs de Eventos](event-hubs-overview.md)
* [Visão geral de API de Hubs de Eventos](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/
