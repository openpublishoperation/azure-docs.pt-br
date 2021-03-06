---
title: Usar servidores NPS existentes para fornecer recursos ao Azure MFA| Microsoft Docs
description: "A extensão do Servidor de Políticas de Rede para a Autenticação Multifator do Azure é uma solução simples para adicionar recursos de verificação em duas etapas baseados em nuvem à sua infraestrutura de autenticação existente."
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
ms.date: 04/03/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 3dba9ebc8eb23be00f9b52907ba4bc565eeb5688
ms.lasthandoff: 04/15/2017

---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication---public-preview"></a>Integrar sua infraestrutura existente do NPS à Autenticação Multifator do Azure - Visualização pública

A extensão do Servidor de Políticas de Rede (NPS) para o Azure MFA adiciona recursos MFA baseados em nuvem à sua infraestrutura de autenticação usando os seus servidores existentes. Com a extensão do NPS, você pode adicionar verificação por chamada telefônica, SMS ou aplicativo ao fluxo de autenticação existente sem a necessidade de instalar, configurar e manter novos servidores. 
 
Ao usar a extensão do NPS para o Azure MFA, o fluxo de autenticação inclui os seguintes componentes: 

1. **Servidor VPN/NAS** recebe solicitações de clientes VPN e converte-os em solicitações RADIUS para servidores NPS. 
2. **Servidor NPS** conecta-se ao Active Directory para executar a autenticação primária das solicitações RADIUS e, ao obter êxito, passa a solicitação para quaisquer extensões instaladas.  
3. **Extensão do NPS** dispara uma solicitação ao Azure MFA para a autenticação secundária. Quando a extensão receber a resposta, e se o desafio de MFA for bem-sucedido, ela concluirá a solicitação de autenticação, fornecendo ao servidor NPS os tokens de segurança que incluem uma declaração MFA, emitida pelo STS do Azure.  
4. O **Azure MFA** comunica-se com o Azure Active Directory para recuperar os detalhes do usuário e executa a autenticação secundária usando um método de verificação configurado para o usuário.

O diagrama a seguir ilustra esse fluxo de solicitação de autenticação de alto nível: 

![Diagrama de fluxo de autenticação](./media/multi-factor-authentication-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a>Planejar sua implantação

A extensão NPS controla automaticamente a redundância, de maneira que você não precisa de uma configuração especial. 

Você pode criar tantos servidores NPS habilitados para a Autenticação Multifator do Azure quanto necessário. Se você instalar vários servidores, use um certificado de cliente diferente para cada um deles. Ao criar um certificado para cada servidor, você pode atualizar cada certificado individualmente e não se preocupar com tempo de inatividade em todos eles. 

Os servidores VPN encaminham as solicitações de autenticação, portanto precisam estar cientes dos novos servidores NPS habilitados para a MFA do Azure. 

## <a name="prerequisites"></a>Pré-requisitos

A extensão do NPS deve trabalhar com sua infraestrutura existente. Verifique se você cumpre os seguintes pré-requisitos antes de iniciar.

### <a name="licenses"></a>Licenças

A extensão do NPS para o Azure MFA está disponível para clientes com [licenças para Autenticação Multifator do Azure](multi-factor-authentication.md) (incluído no Azure AD Premium, EMS ou uma assinatura de MFA).

### <a name="software"></a>Software

Windows Server 2008 R2 SP1 ou superior.

### <a name="libraries"></a>Bibliotecas

Essas bibliotecas são instaladas automaticamente com a extensão. 
-    [Pacotes redistribuíveis do Visual C++ para Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
-    [Módulo Microsoft Azure Active Directory para Windows PowerShell versão 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

### <a name="azure-active-directory"></a>Azure Active Directory

Todos que usam a extensão do NPS devem estar sincronizados com o Azure Active Directory usando o Azure AD Connect e devem estar habilitados para MFA.

Ao instalar a extensão, você precisa das credenciais de administrador e a ID de diretório para seu locatário do Azure AD. Você pode encontrar a ID de diretório no [Portal do Azure](https://portal.azure.com). Entre como administrador, selecione o ícone **Azure Active Directory** à esquerda e selecione **Propriedades**. Copie o GUID na caixa **ID do diretório** e salve-o. Você usará esse GUID como a ID de locatário ao instalar a extensão NPS.

![Localize sua ID de diretório em Propriedades do Azure Active Directory](./media/multi-factor-authentication-nps-extension/find-directory-id.png)

## <a name="prepare-your-environment"></a>Prepare o seu ambiente

Antes de instalar a extensão NPS, convém preparar o ambiente para manipular o tráfego de autenticação. 

### <a name="enable-the-nps-role-on-a-domain-joined-server"></a>Habilitar a função NPS em um servidor ingressado no domínio

O servidor NPS se conecta ao Azure Active Directory e autentica as solicitações da MFA. Escolha um servidor para essa função. É recomendável escolher um servidor que não manipule solicitações de outros serviços, porque a extensão NPS gerará erros para solicitações que não sejam RADIUS.

1. No servidor, abra o **Assistente de Adição de Funções e Recursos** no menu de Início rápido do Gerenciador do Servidor.
2. Escolha **Instalação baseada em função ou recurso** para o tipo de instalação.
3. Selecione a função de servidor **Serviços de Acesso e Política de Rede**. Uma janela poderá ser exibida para informá-lo dos recursos necessários para executar essa função.
4. Siga as etapas do assistente até a página de Confirmação. Selecione **Instalar**. 

Agora que você tem um servidor designado para o NPS, você também deve configurar esse servidor para manipular as solicitações RADIUS de entrada da solução VPN. 

### <a name="configure-your-vpn-solution-to-communicate-with-the-nps-server"></a>Configurar a solução VPN para se comunicar com o servidor NPS

Dependendo da solução VPN que você usa, as etapas para configurar a política de autenticação RADIUS variam. Configure essa política para apontar para o servidor NPS de RADIUS. 

### <a name="sync-domain-users-to-the-cloud"></a>Sincronizar usuários de domínio com a nuvem

Esta etapa pode já estar concluída no seu locatário, mas é bom verificar se Azure AD Connect sincronizou recentemente os seus bancos de dados. 

1. Entre no [Portal do Azure](https://portal.azure.com) como administrador.
2. Selecione **Azure Active Directory** > **Azure AD Connect**
3. Verifique se o status de sincronização está **Habilitado** e se a última sincronização foi há menos de uma hora.

Se você precisar disparar uma nova rodada de sincronização, use as instruções em [Sincronização do Azure AD Connect: agendador](../active-directory/connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler).

### <a name="enable-users-for-mfa"></a>Habilitar usuários para a MFA

Antes de implantar a extensão NPS completa, você precisa habilitar a MFA para os usuários que você deseja que executem a verificação em duas etapas. Mas antes disso, a fim de testar a extensão ao implantá-la, você precisa de pelo menos uma conta de teste que seja totalmente registrada para a Autenticação Multifator. 

Use estas etapas para iniciar uma conta de teste:
1. [Habilitar uma conta para a MFA](multi-factor-authentication-get-started-user-states.md).
2. Acesse qualquer site que inicie uma autenticação do Azure AD, como o https://portal.azure.com.
3. [Registre-se para a verificação em duas etapas](./end-user/multi-factor-authentication-end-user-first-time.md).

## <a name="install-the-nps-extension"></a>Instalar a extensão NPS

> [!IMPORTANT]
> Instale a extensão do NPS em um servidor diferente do ponto de acesso VPN. 

### <a name="download-and-install-the-nps-extension-for-azure-mfa"></a>Baixar e instalar a extensão NPS para a MFA do Azure 

1.    [Baixe a extensão NPS](https://aka.ms/npsmfa) do Centro de Download da Microsoft.
2.    Copie o binário para o Servidor de Políticas de Rede que você deseja configurar.
3.    Execute o arquivo *setup.exe* e siga as instruções de instalação. Se você encontrar erros, verifique se as duas bibliotecas, da seção de pré-requisitos, foram instaladas com êxito.

### <a name="run-the-powershell-script"></a>Executar o script do PowerShell

O instalador cria um script do PowerShell neste local: `C:\Program Files\Microsoft\AzureMfa\Config` (em que C:\ é a sua unidade de instalação). O script do PowerShell executa as ações a seguir:

-    Crie um certificado autoassinado.
-    Associa a chave pública do certificado à entidade de serviço no Azure AD.
-    Armazena o certificado no repositório de certificados do computador local.
-    Concede acesso à chave privada do certificado ao usuário de rede.
-    Reinicia o NPS.

A menos que você deseje usar seus próprios certificados (em vez dos certificados autoassinados gerados pelo script do PowerShell), execute o Script do PowerShell para concluir a instalação. Se você instalar a extensão em vários servidores, cada um deverá ter seu próprio certificado.

1. Execute o Windows PowerShell como um administrador.
2. Altere os diretórios.

   `cd "C:\Program Files\Microsoft\AzureMfa\Config"`

3. Execute o script do PowerShell criado pelo instalador.

   `.\AzureMfaNpsExtnConfigSetup.ps1`

4. O PowerShell solicitará a sua ID de locatário. Use o GUID da ID de diretório que você copiou do Portal do Azure na seção de pré-requisitos. 
5. Entre no Azure AD como administrador.
6. O PowerShell mostra uma mensagem de êxito quando o script é concluído.  

## <a name="configure-your-nps-extension"></a>Configurar sua extensão do NPS

Esta seção inclui considerações sobre o design e sugestões para implantações bem-sucedidas da extensão do NPS.

### <a name="configurations-limitations"></a>Limitações de configurações

- A extensão NPS para a MFA do Azure não inclui ferramentas para migrar usuários e configurações do Servidor MFA para a nuvem. Por esse motivo, é aconselhável usar a extensão para novas implantações, em vez da implantação existente. Se você usar a extensão em uma implantação existente, os usuários terão que executar a verificação novamente para preencher os detalhes de MFA na nuvem.  
- A extensão do NPS usa o UPN do Active Directory local para identificar o usuário no Azure MFA para realizar a autenticação secundária. A extensão não pode ser configurada para usar um identificador diferente, como a ID de logon alternativa ou campo personalizado do AD que não seja o UPN.  

### <a name="control-radius-clients-that-require-mfa"></a>Clientes RADIUS de controle que exigem MFA

Depois que você habilita a MFA para um cliente RADIUS usando a extensão do NPS, todas as autenticações desse cliente são necessárias para executar a MFA. Se você deseja habilitar a MFA para alguns clientes RADIUS, mas outros não, você pode configurar dois servidores NPS e instalar a extensão em apenas um deles. Configure clientes RADIUS para os quais você deseja que a MFA envie solicitações ao servidor NPS configurado com a extensão e outros clientes RADIUS para o servidor NPS não configurado com a extensão.

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a>Preparar usuários que não são registrados na MFA

Se você tiver usuários que não são registrados na MFA, determine o que acontece quando eles tentam fazer a autenticação. Use a configuração de Registro *REQUIRE_USER_MATCH* no caminho do Registro *HKLM\Software\Microsoft\AzureMFA* para controlar o comportamento do recurso. Essa configuração tem uma opção de configuração única:

| Chave | Valor | Padrão |
| --- | ----- | ------- |
| REQUIRE_USER_MATCH | TRUE/FALSE | Não definido (equivalente a TRUE) |

A finalidade dessa configuração é determinar o que fazer quando um usuário não estiver inscrito na MFA. Quando a chave não existir, não estiver definida ou estiver definida como TRUE e o usuário não estiver registrado, a extensão falha no desafio da MFA. Quando a chave estiver definida como FALSE e o usuário não estiver registrado, a autenticação continuará sem executar a MFA.

Você pode optar por criar essa chave e defini-la como FALSE durante a integração do usuário. Como definir a chave permite que os usuários que não são registrados na MFA se conectem, você deve remover essa chave antes de ir para a produção.

## <a name="troubleshooting"></a>Solucionar problemas

### <a name="how-do-i-verify-that-the-client-cert-is-installed-as-expected"></a>Como verificar se o certificado do cliente está instalado conforme o esperado?

Procure o certificado autoassinado criado pelo instalador no repositório de certificados e verifique se a chave privada tem permissões concedidas ao usuário **NETWORK SERVICE**. O certificado tem um nome de entidade igual a **CN \<tenantid\>, OU = extensão NPS da Microsoft**

-------------------------------------------------------------

### <a name="how-can-i-verify-that-my-client-cert-is-associated-to-my-tenant-in-azure-active-directory"></a>Como verificar se o certificado do cliente está associado ao meu locatário no Azure Active Directory?

Abra um prompt de comando no PowerShell e execute os seguintes comandos:

```
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 
```

Esses comandos imprimem todos os certificados associados ao seu locatário com a instância da extensão do NPS em sua sessão do PowerShell. Procure seu certificado exportando o certificado do cliente como um arquivo "codificado em Base 64 X.509(.cer)" sem a chave particular e compare-a com a lista do PowerShell.

Os carimbos de data/hora Válido-de e Válido-até, que estão em formato legível, poderão ser usados para filtrar desvios óbvios se o comando retornar mais de um certificado.

-------------------------------------------------------------

### <a name="why-are-my-requests-failing-with-adal-token-error"></a>Por que minhas solicitações estão falhando, com erro de token ADAL?

Esse erro pode ser causado por vários motivos. Use estas etapas para solucionar o problema:

1. Reinicie o servidor NPS.
2. Verifique se o certificado do cliente está instalado conforme o esperado.
3. Verifique se o certificado está associado ao seu locatário no Azure AD.
4. Verifique se o link https://login.windows.net/ pode ser acessado no servidor que está executando a extensão.

-------------------------------------------------------------

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-the-user-is-not-found"></a>Por que a autenticação falha, com um erro nos logs de HTTP, informando que o usuário não foi encontrado?

Verifique se o AD Connect está em execução e se o usuário está presente no Windows Active Directory e no Azure Active Directory.

------------------------------------------------------------

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a>Por que vejo erros de conexão HTTP nos logs, com todas as minhas autenticações falhando?

Verifique se o link https://adnotifications.windowsazure.com pode ser acessado por meio do servidor que executa a extensão do NPS.

## <a name="next-steps"></a>Próximas etapas

Consulte como integrar o Azure MFA com o [Active Directory](multi-factor-authentication-get-started-server-dirint.md), a [autenticação RADIUS](multi-factor-authentication-get-started-server-radius.md) e a [autenticação LDAP](multi-factor-authentication-get-started-server-ldap.md).

