---
title: "Como depurar o logon único baseado em SAML nos aplicativos no Azure Active Directory | Microsoft Docs"
description: "Saiba como depurar o logon único baseado em SAML em aplicativos no Active Directory do Azure  "
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/09/2016
ms.author: asmalser
translationtype: Human Translation
ms.sourcegitcommit: c579135f798ea0c2a5461fdd7c88244d2d6d78c6
ms.openlocfilehash: 245c2de65ca771cdc0e6371010246725c407ae11
ms.lasthandoff: 01/18/2017


---
# <a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Como depurar o logon único baseado em SAML em aplicativos no Active Directory do Azure
Ao depurar uma integração de aplicativo baseada em SAML, geralmente é útil usar uma ferramenta como o [Fiddler](http://www.telerik.com/fiddler) para ver a solicitação SAML, a resposta SAML e o token SAML real que é emitido para o aplicativo. Examinando o token SAML, você pode garantir que todos os atributos necessários, o nome de usuário no assunto SAML e o URI do emissor estejam chegando conforme o esperado.

![][1]

A resposta do Azure AD que contém o token SAML normalmente é aquela que ocorre depois de um redirecionamento HTTP 302 de https://login.windows.net e é enviada para a **URL de Resposta** do aplicativo configurado. 

Você pode exibir o token SAML selecionando essa linha e, em seguida, selecionando a guia **Inspetores > WebForms** no painel à direita. Nela, clique com botão direito no valor **SAMLResponse** e selecione **Enviar para TextWizard**. Selecione **De Base64** no menu **Transformar** para decodificar o token e ver seu conteúdo.

**Observação**: para ver o conteúdo dessa solicitação HTTP, o Fiddler pode solicitar que você configure a descriptografia de tráfego HTTPS, o que você precisará fazer.

## <a name="related-articles"></a>Artigos relacionados
* [Índice de artigos para Gerenciamento de Aplicativos no Active Directory do Azure](../active-directory-apps-index.md)
* [Configurando logon único para aplicativos que não estão na galeria de aplicativo do Active Directory do Azure](../active-directory-saas-custom-apps.md)
* [Como personalizar declarações emitidas no token SAML para aplicativos pré-integrados](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png
