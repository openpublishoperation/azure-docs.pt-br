---
title: "Enviar notificações entre plataformas a usuários com Hubs de Notificação (ASP.NET)"
description: "Saiba como usar modelos de Hubs de notificação para enviar, em uma única solicitação, uma notificação independente de plataforma que se destina a todas as plataformas."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 11d2131b-f683-47fd-a691-4cdfc696f62b
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: multiple
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0cc620324c75ed6ffee26fe442014d61673ba1e6


---
# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Enviar notificações entre plataformas a usuários com Hubs de Notificação
No tutorial anterior [Notificar usuários com Hubs de Notificação], você aprendeu a enviar notificações por push a todos os dispositivos registrados por um usuário autenticado específico. Nesse tutorial, várias solicitações eram necessárias para enviar uma notificação a cada plataforma de cliente com suporte. Os Hubs de Notificação oferecem suporte a modelos que permitem especificar como um dispositivo específico deseja receber notificações. Isso simplifica o envio de notificações entre plataformas. Este tópico demonstra como tirar proveito de modelos para enviar, em uma única solicitação, uma notificação independente de plataforma que se destina a todas as plataformas. Para obter informações detalhadas sobre modelos, confira [Visão geral de Hubs de Notificação do Azure][Modelos].

> [!NOTE]
> Os Hubs de Notificação permitem que um dispositivo registre vários modelos com a mesma marca. Nesse caso, uma mensagem de entrada destinada a essa marca resulta em várias notificações entregues ao dispositivo, uma para cada modelo. Isso permite que você exiba a mesma mensagem em várias notificações visuais, como uma notificação e como uma notificação do sistema em um aplicativo da Windows Store.
> 
> 

Conclua as etapas a seguir para enviar notificações entre plataformas usando modelos:

1. No Gerenciador de Soluções do Visual Studio, expanda a pasta **Controllers** e, em seguida, abra o arquivo RegisterController.cs.
2. Localize o bloco do código no método **Post** que cria um novo registro e substitui o conteúdo `switch` pelo seguinte código:
   
        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }
   
    Esse código chama o método da plataforma específica para criar um registro de modelo em vez de um registro nativo. Os registros existentes não precisam ser modificados porque os registros de modelo derivam de registros nativos.
3. No controlador **Notificações**, substituir o método **sendNotification** pelo seguinte código:
   
        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;
   
            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
    Esse código envia uma notificação a todas as plataformas ao mesmo tempo e sem precisar especificar uma carga nativa. Os Hubs de Notificação desenvolve e fornece a carga correta para cada dispositivo com o valor *marca* fornecido, conforme especificado nos modelos registrados.
4. Publicar novamente seu projeto back-end do WebApi.
5. Execute o aplicativo cliente novamente e verifique se o registro teve êxito.
6. (Opcional) Implante o aplicativo cliente em um segundo dispositivo e execute-o.
   
    Observe que uma notificação é exibida em cada dispositivo.

## <a name="next-steps"></a>Próximas etapas
Agora que você concluiu este tutorial, saiba mais sobre Hubs de Notificação e modelos nestes tópicos:

* **[Usar Hubs de Notificação para enviar as notícias mais recentes]** <br/>Demonstra outro cenário para a utilização de modelos
* **[Visão geral de Hubs de notificação do Azure][Modelos]**<br/>O tópico Visão Geral tem informações mais detalhadas sobre modelos.

<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Enviar ASP.NET por push aos usuários]: /manage/services/notification-hubs/notify-users-aspnet
[Enviar Serviços Móveis aos usuários]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express para Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Usar Hubs de Notificação para enviar as notícias mais recentes]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Hubs de Notificação do Azure]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Notificar usuários com Hubs de Notificação]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Modelos]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Instruções de Hub de Notificação para a Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx



<!--HONumber=Nov16_HO3-->


