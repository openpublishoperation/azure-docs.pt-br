---
title: "Tutorial: Integração do Azure Active Directory ao Clever | Microsoft Docs"
description: "Saiba como usar o Clever com o Active Directory do Azure para habilitar o logon único, provisionamento automatizado e muito mais!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 069ff13a-310e-4366-a147-d6ec5cca12a5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: ee53124019cbee9c42f94cde411a9b653413011a
ms.openlocfilehash: a219a63f4de17c18d58bfd529e8372b99ccb4e41
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-clever"></a>Tutorial: integração do Active Directory do Azure ao Clever
O objetivo deste tutorial é mostrar a integração do Azure ao Clever. O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

* Uma assinatura válida do Azure
* Um locatário do Clever

Depois de concluir este tutorial, os usuários do Azure AD atribuídos ao Clever poderão fazer logon único no aplicativo em seu site de empresa do Clever (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

* Habilitando a integração de aplicativos para o Clever
* Configurando o logon único
* Configurando o provisionamento de usuários
* Atribuindo usuários

![Cenário](./media/active-directory-saas-clever-tutorial/IC798977.png "Cenário")

## <a name="enable-the-application-integration-for-clever"></a>Habilitar a integração de aplicativos para o Clever
O objetivo desta seção é descrever como habilitar a integração de aplicativos para o Clever.

**Para habilitar a integração de aplicativos para o Clever, execute as seguintes etapas:**

1. No Portal clássico do Azure, no painel de navegação à esquerda, clique em **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-clever-tutorial/IC700993.png "Active Directory")
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
   ![Aplicativos](./media/active-directory-saas-clever-tutorial/IC700994.png "Aplicativos")
4. Clique em **Adicionar** na parte inferior da página.
   
   ![Adicionar aplicativo](./media/active-directory-saas-clever-tutorial/IC749321.png "Adicionar aplicativo")
5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
   ![Adicionar um aplicativo da galeria](./media/active-directory-saas-clever-tutorial/IC749322.png "Adicionar um aplicativo da galeria")
6. Na **caixa de pesquisa**, digite **Clever**.
   
   ![Galeria de Aplicativos](./media/active-directory-saas-clever-tutorial/IC798978.png "Galeria de Aplicativos")
7. No painel de resultados, selecione **Clever** e clique em **Concluir** para adicionar o aplicativo.
   
   ![Clever](./media/active-directory-saas-clever-tutorial/IC798979.png "Clever")
   
## <a name="configure-single-sign-on"></a>Configurar o logon único

O objetivo desta seção é descrever como permitir que os usuários autentiquem no Clever com a própria conta no Azure AD usando federação baseada no protocolo SAML.  

Seu aplicativo Clever espera as declarações do SAML em um formato específico, o que exige que você adicione mapeamentos de atributo personalizados de acordo com a sua configuração de **atributos do token SAML** .  

A captura de tela a seguir mostra um exemplo disso.

![Atributos](./media/active-directory-saas-clever-tutorial/IC798980.png "Atributos")

**Para configurar o logon único, execute as seguintes etapas:**

1. No Portal Clássico do Azure, na página de integração do aplicativo **Clever**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.
   
   ![Configurar Logon Único](./media/active-directory-saas-clever-tutorial/IC784682.png "Configurar Logon Único")
2. Na página **Como você deseja que os usuários façam logon no Clever**, selecione **Logon Único do Microsoft Azure AD** e clique em **Avançar**.
   
   ![Configurar Logon Único](./media/active-directory-saas-clever-tutorial/IC798981.png "Configurar Logon Único")
3. Na página **Configurar URL do Aplicativo**, na caixa de texto **URL de Logon do Clever**, digite a URL usada pelos seus usuários para fazer logon no seu aplicativo Clever (por exemplo: *https://clever.com/in/azsandbox*) e clique em **Avançar**.
   
   ![Configurar URL do Aplicativo](./media/active-directory-saas-clever-tutorial/IC798982.png "Configurar URL do Aplicativo")
4. Na página **Configurar logon único no Clever**, para baixar seus metadados, clique em **Baixar metadados** e salve o arquivo de metadados localmente no computador.
   
   ![Configurar Logon Único](./media/active-directory-saas-clever-tutorial/IC798983.png "Configurar Logon Único")
5. Em outra janela do navegador da Web, faça logon em seu site de empresa do Clever como um administrador.
6. Na barra de ferramentas, clique em **Logon Instantâneo**.
   
   ![Logon Instantâneo](./media/active-directory-saas-clever-tutorial/IC798984.png "Logon Instantâneo")
7. Na página **Logon Instantâneo** , execute as seguintes etapas:
   
   ![Logon Instantâneo](./media/active-directory-saas-clever-tutorial/IC798985.png "Logon Instantâneo")
   
   1. Digite a **URL de Logon**.  
   
      >[!NOTE]
      >A **URL de Logon** é um valor personalizado. Você pode obter o valor real com a equipe de suporte do Clever.
      > 
   2. Para **Sistema de Identidade**, selecione **ADFS**.
   3. Clique em **Salvar**.
8. No portal clássico do Azure, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar logon único**.
   
   ![Configurar Logon Único](./media/active-directory-saas-clever-tutorial/IC798986.png "Configurar Logon Único")
9. Na parte superior do menu, clique em **Atributos** to open the **SAML Token Atributos** .
   
   ![Atributos](./media/active-directory-saas-clever-tutorial/IC795920.png "Atributos")
10. Para adicionar os mapeamentos de atributo necessários, execute as seguintes etapas:
    
    ![Atributos de Token SAML](./media/active-directory-saas-clever-tutorial/IC795921.png "Atributos de Token SAML")
    
    | Nome do atributo | Valor do atributo |
    | --- | --- |
    | clever.student.credentials.district\_nome de usuário |User.userprincipalname |
    
    1. Para cada linha de dados na tabela acima, clique em **adicionar atributo do usuário**.
    2. Na caixa de texto **Nome do Atributo** , digite o nome do atributo mostrado para essa linha.
    3. Na caixa de texto **Valor do Atributo** , selecione o valor do atributo mostrado para essa linha.
    4. Clique em **Concluído**.
11. Clique em **Aplicar alterações**.

## <a name="configure-user-provisioning"></a>Configurar provisionamento do usuário
Para permitir que os usuários do Azure AD façam logon no Clever, eles devem ser provisionados no Clever.

No caso do Clever, o provisionamento é uma tarefa manual que precisa ser realizada por sua equipe de suporte do Clever.

>[!NOTE]
>É possível usar qualquer outra ferramenta de criação da conta de usuário do Clever ou as APIs fornecidas pelo Clever para provisionar as contas de usuário do AAD.
>  

## <a name="assign-users"></a>Atribuir usuários
Para testar sua configuração, é necessário conceder acesso ao aplicativo aos usuários do Azure AD que você deseja que usem seu aplicativo.

**Para atribuir usuários ao Clever, execute as etapas a seguir:**

1. No Portal clássico do Azure, crie uma conta de teste.
2. Na página de integração do aplicativo **Clever**, clique em **Atribuir usuários**.
   
   ![Atribuir Usuários](./media/active-directory-saas-clever-tutorial/IC798987.png "Atribuir Usuários")
3. Selecione seu usuário de teste, clique em **Atribuir** e, em seguida, clique em **Sim** para confirmar a atribuição.
   
   ![Sim](./media/active-directory-saas-clever-tutorial/IC767830.png "Sim")

Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).


