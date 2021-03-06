---
title: "Tutorial: integração do Azure Active Directory com o Cisco Spark | Microsoft Docs"
description: "Saiba como configurar o logon único entre o Azure Active Directory e o Cisco Spark."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 931004d458fd0cbf3e002805ca701d777ddc57c2
ms.openlocfilehash: f0f12b1667b7f45fd164fac658502c93e8afb855
ms.lasthandoff: 03/01/2017


---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Tutorial: Integração do Azure Active Directory ao Cisco Spark
Neste tutorial, você aprenderá como integrar o Cisco Spark ao Azure AD (Azure Active Directory).

A integração do Cisco Spark ao Azure AD oferece os seguintes benefícios:

* Você pode controlar no Azure AD quem tem acesso ao Cisco Spark
* Você pode permitir que seus usuários entrem automaticamente no Cisco Spark por meio do logon único (SSO) usando suas contas do Azure AD
* Gerenciar suas contas em um único local: o Portal clássico do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos
Para configurar a integração do Azure AD ao Cisco Spark, você precisará dos seguintes itens:

* Uma assinatura do AD do Azure
* Uma assinatura habilitada para logon único (SSO) do **Cisco Spark**

>[!NOTE]
>Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção. 
> 

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

* Não use o ambiente de produção, a menos que seja necessário.
* Se não tiver um ambiente de avaliação do Azure AD, você pode obter uma [versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Cisco Spark da galeria
2. Configurar e testar o SSO do Azure AD

## <a name="add-cisco-spark-from-the-gallery"></a>Adicionar o Cisco Spark da galeria
Para configurar a integração do Cisco Spark ao Azure AD, você precisa adicionar o Cisco Spark da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Cisco Spark da galeria, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**. 
   
    ![Active Directory][1]
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
    ![Aplicativos][2]
4. Clique em **Adicionar** na parte inferior da página.
   
    ![Aplicativos][3]
5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
    ![Aplicativos][4]
6. Na caixa de pesquisa, digite **Cisco Spark**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_01.png)
7. No painel de resultados, escolha **Cisco Spark** e clique em **Concluir** para adicionar o aplicativo.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_02.png)

## <a name="configure-and-test-azure-ad-sso"></a>Configurar e testar SSO do Azure AD
Nesta seção, você vai configurar e testar o logon único (SSO) do Azure AD com o Cisco Spark, com base em um usuário de teste chamado "Brenda Fernandes".

Para que o SSO funcione, o Azure AD precisa saber qual usuário do Cisco Spark é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vinculação entre um usuário do Azure AD e o usuário relacionado do Cisco Spark.

Essa relação de vinculação é estabelecida atribuindo o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** no Cisco Spark. Para configurar e testar o logon único do Azure AD com o Cisco Spark, você precisa concluir os seguintes blocos de construção:

1. **[Configurar logon único do Azure AD](#configuring-azure-ad-single-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** - para testar logon único do Azure AD com Britta Simon.
3. **[Criando um usuário de teste do Cisco Spark](#creating-a-cisco-spark-test-user)** — para ter um equivalente de Brenda Fernandes no Cisco Spark que esteja vinculado à representação dela no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** - para habilitar Britta Simon a usar o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD
O objetivo desta seção é habilitar o SSO do Azure AD no Portal Clássico do Azure e configurar o logon único em seu aplicativo do Cisco Spark.

O aplicativo Cisco Spark espera que as asserções SAML contenham atributos específicos. Configure as atribuições a seguir para o aplicativo. Você pode gerenciar os valores dos atributos na guia **"Atributos"** do aplicativo. A captura de tela a seguir mostra um exemplo disso.

![Configurar o logon único](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_03.png) 

**Para configurar o logon único (SSO) do Azure AD com o Cisco Spark, execute as seguintes etapas:**

1. No portal clássico do Azure, na página de integração de aplicativos do **Cisco Spark**, no menu superior, clique em **Atributos**.
   
    ![Configurar Logon Único][5]
2. Na caixa de diálogo **Atributos de token SAML** , execute as seguintes etapas:
  1. Clique em **adicionar atributo de usuário** para abrir a caixa de diálogo **Adicionar Atributo de Usuário**.
   
    ![Configurar o logon único](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_05.png)
  2. Na caixa de texto **Nome do Atributo**, digite **uid**.
  3. Na lista **Valor do Atributo**, escolha **user.userprincipal**.
  4. Clique em **Concluído**. Clique em **Aplicar Alterações** na parte inferior da página.
3. No menu na parte superior, clique em **Início Rápido**.
   
    ![Configurar Logon Único][6]
4. No portal clássico, na página de integração de aplicativos do **Cisco Spark**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar Logon Único**.
   
    ![Configurar Logon Único][7] 
5. Na página **Como você deseja que os usuários façam logon no Cisco Spark**, escolha **Logon Único do Azure AD** e clique em **Avançar**.
   
    ![Configurar o logon único](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_06.png)
6. Na página de diálogo **Definir Configurações de Aplicativo** , execute as seguintes etapas:
   
    ![Configurar Logon Único](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_07.png)
  1. Na caixa de texto URL de Entrada, digite uma URL usando o seguinte padrão: `https://web.ciscospark.com/#/signin`.
  2. Clique em **Próximo**.
7. Na página **Configurar o logon único no Cisco Spark**, clique em **Baixar metadados** e salve o arquivo de metadados no computador.
   
    ![Configurar Logon Único](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_09.png)
8. Entre no [Gerenciamento de Colaboração de Nuvem da Cisco](https://admin.ciscospark.com/) com suas credenciais completas de administrador.
9. Escolha **Configurações** e, na seção **Autenticação**, clique em **Modificar**.
   
    ![Configurar Logon Único](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)
9. Escolha **Integrar um provedor de identidade de terceiros. (Avançado)** e vá para a próxima tela.
10. Clique em **Baixar Arquivo de Metadados** e salve o arquivo no computador.
11. Na página **Importar Metadados Idp**, arrastre e solte o arquivo de metadados do Azure AD na página ou use a opção de navegador de arquivos para localizar e carregar o arquivo de metadados do Azure AD. Em seguida, escolha **Exigir certificado assinado por uma autoridade de certificação em Metadados (mais seguro)** e clique em **Avançar**. 
   
   ![Configurar Logon Único](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)
12. Escolha **Testar Conexão SSO** e, quando uma nova guia do navegador for aberta, autentique-se com o Azure AD conectando-se.
13. Volte para a guia do navegador **Gerenciamento de Colaboração de Nuvem da Cisco**. Se o teste tiver sido bem-sucedido, escolha a opção **Este teste foi executado com êxito. Habilitar Logon Único** e clique em **Avançar**.
14. No portal clássico do Azure AD, selecione a confirmação de configuração do logon único e clique em **Avançar**.
   
    ![Logon Único do AD do Azure][10]
15. Na página **Confirmação de logon único**, clique em **Concluir**.  
    
     ![Logon Único do AD do Azure][11]

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD
Nesta seção, você criará uma usuária de teste no portal clássico chamada Brenda Fernandes.

![Criar um usuário do AD do Azure][20]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_09.png) 
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para exibir a lista de usuários, no menu na parte superior, clique em **Usuários**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 
4. Para abrir a caixa de diálogo **Adicionar Usuário**, na barra de ferramentas na parte inferior, clique em **Adicionar Usuário**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 
5. Na página do diálogo **Conte-nos sobre este usuário** , realize as seguintes etapas:
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_05.png) 
  1. Em Tipo de Usuário, selecione Novo usuário na organização.
  2. Na **caixa de texto** Nome do Usuário, digite **BrendaFernandes**.
  3. Clique em **Próximo**.
6. Na página do diálogo **Perfil do Usuário** , realize as seguintes etapas:
   
   ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_06.png) 
  1. Na caixa de texto **Nome**, digite **Brenda**.  
  2. Na caixa de texto **Sobrenome**, digite **Fernandes**.
  3. Na caixa de texto **Nome de Exibição**, digite **Brenda Fernandes**.
  4. Na lista **Função**, selecione **Usuário**.
  5. Clique em **Próximo**.
7. Na página de diálogo **Obter senha temporária**, clique em **criar**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_07.png) 
8. Na página de caixa de diálogo **Obter senha temporária** , execute as seguintes etapas:
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_08.png) 
  1. Anote o valor da **Nova Senha**.
  2. Clique em **Concluído**.   

### <a name="create-a-cisco-spark-test-user"></a>Criar um usuário de teste do Cisco Spark
Nesta seção, você criará um usuário chamado Brenda Fernandes no Cisco Spark. Nesta seção, você criará um usuário chamado Brenda Fernandes no Cisco Spark.

1. Acesse o [Gerenciamento de Colaboração de Nuvem da Cisco](https://admin.ciscospark.com/) com suas credenciais completas de administrador.
2. Clique em **Usuários** e em **Gerenciar Usuários**.
   
    ![Configurar Logon Único](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 
3. Na janela **Gerenciar Usuário**, escolha **Adicionar ou modificar usuários manualmente** e clique em **Avançar**.
4. Escolha **Nomes e Endereço de email**. Em seguida, preencha a caixa de texto como se segue:
   
    ![Configurar Logon Único](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 
  1. Na caixa de texto **Nome**, digite **Brenda**. 
  2. Na caixa de texto **Sobrenome**, digite **Fernandes**.
  3. Na caixa de texto **Endereço de email**, digite **britta.simon@contoso.com**.
5. Clique no sinal de mais para adicionar Brenda Fernandes. Em seguida, clique em **Avançar**.
6. Na janela **Adicionar Serviços para Usuários**, clique em **Salvar** e em **Concluir**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD
Nesta seção, você permitirá que Brenda Fernandes use o logon único (SSO) do Azure concedendo-lhe acesso ao Cisco Spark.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao Cisco Spark, execute as seguintes etapas:**

1. No portal clássico, para abrir o modo de exibição de aplicativos, no modo de exibição de diretório, clique em **Aplicativos** no menu superior.
   
    ![Atribuir usuário][201] 
2. Na lista de aplicativos, escolha **Cisco Spark**.
   
    ![Configurar o logon único](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_14.png) 
3. No menu na parte superior, clique em **Usuários**.
   
    ![Atribuir usuário][203] 
4. Na lista Todos os Usuários, escolha **Brenda Fernandes**.
5. Na barra de ferramentas na parte inferior, clique em **Atribuir**.
   
    ![Atribuir usuário][205]

### <a name="test-single-sign-on"></a>Testar logon único
O objetivo desta seção é testar sua configuração de SSO do Azure AD usando o Painel de Acesso.

Ao clicar no bloco Cisco Spark no Painel de Acesso, você deverá ser conectado automaticamente ao aplicativo Cisco Spark.

## <a name="additional-resources"></a>Recursos adicionais
* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_205.png

