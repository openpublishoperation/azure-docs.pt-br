---
title: "Versões e planos de consumo do Azure MFA | Microsoft Docs"
description: "Informações sobre o cliente de autenticação multifator e os diferentes métodos e as versões disponíveis. Detalhes sobre cada plano de consumo"
keywords: 
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 875b4e3cab6eb4b45ed2d4b6160c60f320703dd8
ms.openlocfilehash: 1c8ebf2e1dd9400ad153f5b42e7b72f36d28feac
ms.lasthandoff: 02/23/2017

---
# <a name="how-to-get-azure-multi-factor-authentication"></a>Como obter o Azure Multi-Factor Authentication

Quando se trata de proteger suas contas, verificação em duas etapas deve ser padrão em sua organização. Esse recurso é especialmente importante para contas administrativas que tem acesso aos recursos privilegiado. Por esse motivo, a Microsoft oferece recursos de verificação em duas etapas básicas para administradores do Office 365 e o Azure. Se você quiser atualizar os recursos para seus administradores ou estender a verificação em duas etapas para o restante de seus usuários, você pode adquirir a autenticação multifator do Azure. 

Este artigo aborda explica a diferença entre as versões oferecidas aos administradores e a versão completa do Azure MFA e especifica quais recursos estão disponíveis em cada um. Se você estiver pronto para implantar o Azure MFA completa oferta, as seções posteriores aborda como Microsoft calcula o consumo e opções de implementação.

>[!IMPORTANT]
>Este artigo deve ser um guia para ajudá-lo a entender as diferentes maneiras de comprar autenticação multifator do Azure. Para obter detalhes específicos sobre preços e cobrança, você deve sempre consultar a [página de preços da Autenticação Multifator](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Versões disponíveis do Azure Multi-Factor Authentication

A tabela a seguir descreve as diferenças entre as três versões de autenticação multifator:

| Versão | Descrição |
| --- | --- |
| Autenticação Multifator para Office 365 |Esta versão funciona exclusivamente com os aplicativos do Office 365 e é gerenciada a partir do portal do Office 365. Os administradores podem [proteger recursos do Office 365 com a verificação de duas etapas](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6). Esta versão faz parte de uma assinatura do Office 365. |
| Autenticação Multifator para administradores do Azure | Administradores do Azure podem habilitar a verificação em duas etapas para suas contas de administrador sem custo adicional.|
| Autenticação Multifator do Azure | Geralmente conhecida como a versão "completa", a Autenticação Multifator do Azure oferece o conjunto mais avançado de recursos. Ela fornece opções de configuração adicionais por meio do [portal clássico do Azure](https://manage.windowsazure.com), relatórios avançados e suporte para uma variedade de aplicativos locais e na nuvem. A Autenticação Multifator do Azure e está incluída no Azure Active Directory Premium (planos P1 e P2) e Enterprise Mobility + Security (planos E3 e E5) e pode ser implantado [na nuvem ou no local](multi-factor-authentication-get-started.md). |

## <a name="feature-comparison-of-versions"></a>Comparação de recursos dasversões
A tabela a seguir fornece uma lista dos recursos que estão disponíveis em várias versões do Autenticação Multifator do Azure.

> [!NOTE]
> Esta tabela de comparação aborda os recursos que fazem parte de cada versão da Autenticação Multifator. Se você tiver o serviço completo da Autenticação Multifator do Azure, alguns recursos podem não estar disponíveis caso você use [MFA na nuvem ou MFA no local](multi-factor-authentication-get-started.md).


| Recurso | Autenticação Multifator para Office 365 | Autenticação Multifator para administradores do Azure | Autenticação Multifator do Azure |
| --- |:---:|:---:|:---:|
| Proteger contas de administrador com MFA |● |● (Disponível apenas para contas de Administrador do Azure) |● |
| Aplicativos móveis como um fator secundário |● |● |● |
| Chamada telefônica como um fator secundário |● |● |● |
| SMS como um fator secundário |● |● |● |
| Senhas de aplicativos para clientes que não oferecem suporte a MFA |● |● |● |
| Controle do administrador sobre métodos de verificação |● |● |● |
| Modo PIN | | |● |
| Alerta de fraude | | |● |
| Relatórios de MFA | | |● |
| Desvio único | | |● |
| Saudações personalizadas para chamadas telefônicas | | |● |
| ID do chamador personalizado para chamadas telefônicas | | |● |
| IPs confiáveis | | |● |
| Lembrar MFA para dispositivos confiáveis |● |● |● |
| SDK de MFA | | |● (Exige o provedor de Autenticação Multifator e assinatura completa do Azure) |
| MFA para aplicativos locais | | |● |

## <a name="how-to-get-azure-multi-factor-authentication"></a>Como obter o Azure Multi-Factor Authentication
Se você quiser a funcionalidade completa oferecida pela autenticação multifator do Azure, há várias opções:

### <a name="option-1---mfa-licenses"></a>Opção 1 - Licenças MFA

Comprar licenças da Autenticação Multifator do Azure e atribuí-las a seus usuários no Azure Active Directory. 

Se você usar essa opção, só deverá criar um Provedor de Autenticação Multifator do Azure se também precisar fornecer verificação em duas etapas para alguns usuários que não tenham licenças. Caso contrário, você poderá ser cobrado duas vezes.

### <a name="option-2---bundled-licenses-that-include-mfa"></a>Opção 2 - Licenças agrupadas que incluem MFA

Adquirir licenças que incluem a Autenticação Multifator do Azure, como o Active Directory Premium do Azure (P1 ou P2) ou Enterprise Mobility + Security (E3 ou E5) e atribuí-los para os usuários no Azure Active Directory. 

Se você usar essa opção, só deverá criar um Provedor de Autenticação Multifator do Azure se também precisar fornecer verificação em duas etapas para alguns usuários que não tenham licenças. Caso contrário, você poderá ser cobrado duas vezes. 

### <a name="option-3---mfa-consumption-based-model"></a>Opção 3 - modelo baseado no consumo de MFA

Crie um Provedor do Azure Multi-Factor Authentication em uma assinatura do Azure. Provedores de MFA do Azure são recursos do Azure que são cobrados em relação a seu Enterprise Agreement, Azure compromisso monetário ou cartão de crédito, assim como todos os outros recursos do Azure. Esses provedores podem ser criados somente em assinaturas do Azure completo, sem limitação, assinaturas do Azure que tenha um&0; $ limite de gastos. As assinaturas limitadas são criadas quando você ativa licenças, como nas opções 1 e 2. 

Ao usar um Provedor de Autenticação Multifator do Azure, há dois modelos de uso disponíveis que são cobrados por meio de sua assinatura do Azure:  

1. **Por Usuário** - para as empresas que desejam habilitar a verificação em duas etapas para um número fixo de funcionários que precisam regularmente de autenticação. Cobrança por usuário é baseada no número de usuários habilitados para MFA em seu locatário do Azure AD e/ou o servidor Azure MFA. Se os usuários estão habilitados para MFA no Azure AD e o Azure MFA Server e sincronização de domínio (Azure AD Connect) estiver habilitada, podemos contar o maior conjunto de usuários. Se a sincronização de domínio não estiver habilitada, então podemos contar a soma de todos os usuários habilitados para MFA no Azure AD e o Azure MFA Server. O faturamento é rateado e registrado no sistema Comércio diariamente. 

  > [!NOTE]
  > Exemplo de cobrança 1: você tem 5.000 usuários habilitados para MFA hoje. O sistema MFA divide esse número por 31 e relatórios 161.29 usuários para esse dia. No dia seguinte, você habilita mais 15 usuários e, portanto, o sistema MFA relata 161,77 usuários para esse dia. No final do ciclo de cobrança, o número total de usuários cobrado em sua assinatura do Azure até adiciona em torno de 5.000. 
  >
  > Exemplo de cobrança 2: tiver uma mistura de usuários com licenças e os usuários sem, para que você tenha um provedor de MFA do Azure por usuário para fazer a diferença. Há 4.500 licenças do Enterprise Mobility + Security em seu locatário, mas 5.000 usuários habilitados para MFA. Sua assinatura do Azure é cobrada por 500 usuários, rateada e considerada 16.13 usuários diariamente. 

2. **Por Autenticação** - para empresas que querem habilitar a verificação em duas etapas para um grupo grande de usuários que raramente precisam de autenticação. Cobrança é baseada no número de solicitações de verificação em duas etapas recebidas pelo serviço de nuvem do Azure MFA, independentemente dessas verificações de êxito ou são negadas. Essa cobrança aparece em sua instrução de uso do Azure nos pacotes de 10 autenticações e é relatada para o sistema de Comércio diariamente. 

  > [!NOTE]
  > Exemplo de cobrança 3: atualmente, o serviço do Azure MFA recebido 3,105 solicitações de verificação em duas etapas. Sua assinatura do Azure é cobrada por 310,5 pacotes de autenticação. 

É importante observar que você pode ter licenças do Azure MFA, mas ainda obter cobrado pela configuração com base no consumo. Se você configurar um provedor de MFA por autenticação do Azure, você será cobrado para cada solicitação de verificação em duas etapas, mesmo aquelas feitas por usuários com licenças. Se você configurar um provedor de MFA do Azure por usuário em um domínio que não está vinculado a seu locatário do Azure AD, você será cobrado por usuário habilitado, mesmo se os usuários possuem licenças no Azure AD. 

## <a name="next-steps"></a>Próximas etapas

- Para obter mais detalhes sobre preços, veja [Preços do Azure MFA](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

- Escolha se deseja implantar o Azure MFA [na nuvem ou no local](multi-factor-authentication-get-started.md)
