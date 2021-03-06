---
title: "Referência do Protocolo SAML do Azure AD | Microsoft Docs"
description: "Este artigo fornece uma visão geral dos perfis SAML de Logon Único e Logout Único no Azure Active Directory."
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 88125cfc-45c1-448b-9903-a629d8f31b01
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: priyamo
translationtype: Human Translation
ms.sourcegitcommit: 146d1377a017becdcdcd7fed7b97f07c2cb2bb39
ms.openlocfilehash: dceaab3f89675ae8e046419aaa5173c2733b0660
ms.lasthandoff: 03/29/2017


---
# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Como o Azure Active Directory usa o protocolo SAML
O Azure AD (Azure Active Directory) usa o protocolo SAML 2.0 para permitir que os aplicativos forneçam uma experiência de logon único para os usuários. O perfis SAML do [Logon Único](active-directory-single-sign-on-protocol-reference.md) e do [Logout Único](active-directory-single-sign-out-protocol-reference.md) do Azure AD explicam como as declarações SAML, protocolos e vinculações são usados no serviço do provedor de identidade.

O protocolo SAML requer que o provedor de identidade (Azure AD) e o provedor de serviço (o aplicativo) troquem informações sobre si mesmos.

Quando um aplicativo é registrado no Azure AD, o desenvolvedor do aplicativo registra informações relacionadas à federação no Azure AD. Isso inclui o **URI de Redirecionamento** e o **URI de Metadata** do aplicativo.

O Azure AD usa o **URI de metadados** do serviço de nuvem para recuperar a chave de assinatura e o URI de logout do serviço de nuvem. Se o aplicativo não dá suporte a um URI de metadados, o desenvolvedor deve contatar o suporte da Microsoft para fornecer o URI de logout e a chave de assinatura.

O Azure Active Directory expõe pontos de extremidade de logon único e logout único comuns e específicos de locatário (independente do locatário). Essas URLs representam os locais endereçáveis (não são apenas identificadores) para que você possa ir ao ponto de extremidade ler os metadados.

* O ponto de extremidade específico do locatário está localizado em `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  O espaço reservado <TenantDomainName> representa um nome de domínio registrado ou o GUID TenantID de um locatário do Azure AD. Por exemplo, os metadados de federação do locatário contoso.com estão em: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

* O ponto de extremidade independente do locatário está localizado em `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. Nesse endereço do ponto de extremidade, **comum** é exibido, em vez de um nome de domínio do locatário ou ID.

Para obter informações sobre os documentos de metadados federados que o Azure AD publica, confira [Metadados Federados](active-directory-federation-metadata.md).

