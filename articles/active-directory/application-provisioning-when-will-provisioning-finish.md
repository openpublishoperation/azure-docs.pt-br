---
title: "O provisionamento do usuário para um aplicativo da Galeria do Azure AD está levando horas ou mais | Microsoft Docs"
description: "Como descobrir por que o provisionamento para seu aplicativo está demorando mais do que o esperado"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: asteen
translationtype: Human Translation
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: a0d2909ca69f06b6d08deaa25f35d55d9f9828fe
ms.lasthandoff: 04/11/2017


---

# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>O provisionamento do usuário para um aplicativo da Galeria do Azure AD está levando horas ou mais

Ao habilitar pela primeira vez o provisionamento automático para um aplicativo, a sincronização inicial pode levar de 20 minutos até várias horas, dependendo do tamanho do diretório do Azure AD e do número de usuários no escopo para provisionamento. 

Sincronizações subsequentes após a sincronização inicial são mais rápidas, uma vez que o serviço de provisionamento armazena as marcas d'água que representam o estado dos dois sistemas após a sincronização inicial, melhorando o desempenho dessas sincronizações subsequentes.

## <a name="how-to-improve-provisioning-performance"></a>Como melhorar o desempenho do provisionamento

Se a sincronização inicial estiver demorando mais do que apenas algumas horas, há algo que você pode fazer para melhorar o desempenho:

-   **Filtros de escopo de usuário.** Filtros de escopo permitem ajustar os dados que o serviço de provisionamento extrai do Azure AD filtrando usuários com base em valores de atributo específicos. Para obter mais informações sobre filtros de escopo, consulte [Provisionamento de aplicativos com base em atributo com filtros de escopo](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

## <a name="next-steps"></a>Próximas etapas
[Automatizar o provisionamento e desprovisionamento de usuários para aplicativos SaaS com o Azure Active Directory](active-directory-saas-app-provisioning.md)


