---
title: Criar um Aplicativo PHP no Aplicativo Web | Microsoft Docs
description: "Implante seu primeiro Hello World em PHP no aplicativo Web do Serviço de Aplicativo em minutos."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/31/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: d5126f3b9fa92ff95eaa8bc06554c49f9836bab9
ms.lasthandoff: 04/25/2017


---
# <a name="create-a-php-application-on-web-app"></a>Criar um aplicativo PHP no aplicativo Web

Este tutorial de início rápido orienta como desenvolver e implantar um aplicativo PHP no Azure. Executaremos o aplicativo usando um Serviço de Aplicativo do Azure baseado no Linux, e iremos criar e configurar um novo Aplicativo Web nele usando a CLI do Azure. Em seguida, usaremos o git para implantar nosso aplicativo PHP no Azure.

![hello-world-in-browser](media/app-service-web-get-started-php/hello-world-in-browser.png)

Você pode seguir as etapas abaixo usando um computador Mac, Windows ou Linux. Deve levar apenas cerca de cinco minutos para concluir todas as etapas abaixo.

## <a name="before-you-begin"></a>Antes de começar

Antes de executar este exemplo, instale localmente os seguintes pré-requisitos:

1. [Baixe e instale o git](https://git-scm.com/)
1. [Baixe e instale o PHP](https://php.net)
1. Baixe e instale a [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Baixar o exemplo

Clone o repositório de aplicativos de exemplo Hello World em seu computador local.

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
```

> [!TIP]
> Como alternativa, você pode [baixar o exemplo](https://github.com/Azure-Samples/php-docs-hello-world/archive/master.zip) como um arquivo zip e extraí-lo.

Altere para o diretório que contém o código de exemplo.

```bash
cd php-docs-hello-world
```

## <a name="run-the-app-locally"></a>Executar o aplicativo localmente

Execute o aplicativo localmente abrindo uma janela do terminal e usando a linha de comando `php` do exemplo para iniciar o servidor interno da Web do PHP.

```bash
php -S localhost:8080
```

Abra um navegador da Web e navegue até o exemplo.

```bash
http://localhost:8080
```

Você poderá ver a mensagem **Hello World** no aplicativo de exemplo exibido na página.

![localhost-hello-world-in-browser](media/app-service-web-get-started-php/localhost-hello-world-in-browser.png)

Na janela do terminal, pressione **Ctrl+C** para sair do servidor Web.

## <a name="log-in-to-azure"></a>Fazer logon no Azure

Agora, usaremos a CLI 2.0 do Azure em uma janela do terminal para criar os recursos necessários para hospedar nosso aplicativo PHP no Azure. Faça logon na sua assinatura do Azure com o comando [az login](/cli/azure/#login) e siga as instruções na tela.

```azurecli
az login
```

## <a name="configure-a-deployment-user"></a>Configurar um Usuário de Implantação

Para o FTP e o Git local, é necessário ter um usuário de implantação configurado no servidor para autenticar sua implantação. Criar um usuário de implantação é uma configuração única. Anote o nome de usuário e a senha que serão usados em uma etapa abaixo.

> [!NOTE]
> Um usuário de implantação é necessário para a implantação do FTP e do Git Local em um Aplicativo Web.
> O `username` e `password` estão no nível da conta e como tais, são diferentes de suas credenciais de Assinatura do Azure. **Essas credenciais precisam ser criadas apenas uma vez**.
>

Use o comando [az appservice web deployment user set](/cli/azure/appservice/web/deployment/user#set) para criar suas credenciais no nível da conta.

```azurecli
az appservice web deployment user set --user-name <username> --password <password>
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com [az group create](/cli/azure/group#create). Um grupo de recursos do Azure é um contêiner lógico no qual os recursos do Azure, como os aplicativos Web, bancos de dados e contas de armazenamento, são implantados e gerenciados.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-an-azure-app-service"></a>Criar um Serviço de Aplicativo do Azure

Criar um Plano do Serviço de Aplicativo baseado no Linux com o comando [az appservice plan create](/cli/azure/appservice/plan#create).

> [!NOTE]
> Um plano do Serviço de Aplicativo representa a coleção de recursos físicos usados para hospedar seus aplicativos. Todos os aplicativos atribuídos a um plano do Serviço de Aplicativo compartilham os recursos definidos por ele, permitindo que você economize ao hospedar vários aplicativos.
>
> Os Planos do Serviço de Aplicativo definem:
> * Região (Europa Setentrional, Leste dos EUA, Sudeste Asiático)
> * Tamanha da Instância (Pequena, Média, Grande)
> * Contagem da Escala (uma, duas, três instâncias etc.)
> * SKU (Gratuito, Compartilhado, Básico, Standard, Premium)
>

O exemplo a seguir cria um Plano do Serviço de Aplicativo nos Trabalhos do Linux denominado `quickStartPlan` usando o tipo de preços **Standard**.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku S1 --is-linux
```

Quando o Plano do Serviço de Aplicativo for criado, a CLI do Azure mostrará informações semelhantes ao exemplo a seguir.

```json
{
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "kind": "linux",
    "location": "West Europe",
    "sku": {
    "capacity": 1,
    "family": "S",
    "name": "S1",
    "tier": "Standard"
    },
    "status": "Ready",
    "type": "Microsoft.Web/serverfarms"
}
```

## <a name="create-a-web-app"></a>Criar um aplicativo Web

Agora que um plano do Serviço de Aplicativo foi criado, crie um Aplicativo Web no `quickStartPlan` plano do Serviço de Aplicativo. O aplicativo Web nos dá um espaço de hospedagem para implantar nosso código, bem como fornece uma URL para exibir o aplicativo implantado. Use o comando [az appservice web create](/cli/azure/appservice/web#create) para criar o Aplicativo Web.

No comando abaixo, substitua seu próprio nome exclusivo do aplicativo onde você vê o espaço reservado <nome_aplicativo>. O <nome_aplicativo> será usado como o site DNS padrão para o aplicativo Web, portanto, o nome deve ser exclusivo entre todos os aplicativos no Azure. Posteriormente, você poderá mapear qualquer entrada DNS personalizada para o aplicativo Web antes de expor para seus usuários.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan quickStartPlan
```

Quando o aplicativo Web tiver sido criado, a CLI do Azure mostrará informações semelhantes ao exemplo a seguir.

```json
{
    "clientAffinityEnabled": true,
    "defaultHostName": "<app_name>.azurewebsites.net",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>",
    "isDefaultContainer": null,
    "kind": "app",
    "location": "West Europe",
    "name": "<app_name>",
    "repositorySiteName": "<app_name>",
    "reserved": true,
    "resourceGroup": "myResourceGroup",
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "state": "Running",
    "type": "Microsoft.Web/sites",
}
```

Navegue até o site para ver seu Aplicativo Web recém-criado.

```bash
http://<app_name>.azurewebsites.net
```

![app-service-web-service-created](media/app-service-web-get-started-php/app-service-web-service-created.png)

Agora, criamos um novo Aplicativo Web vazio no Azure. Agora, configuraremos nosso Aplicativo Web para usar o PHP e implantar nosso aplicativo nele.

## <a name="configure-to-use-php"></a>Configurar para usar o PHP

Use o comando [az appservice web config update](/cli/azure/app-service/web/config#update) para configurar o Aplicativo Web para usar a versão do PHP `7.0.x`.

> [!TIP]
> Definir a versão do PHP dessa maneira usa um contêiner padrão fornecido pela plataforma. Se você quiser usar seu próprio contêiner, consulte a referência da CLI para o comando [az appservice web config container update](https://docs.microsoft.com/cli/azure/appservice/web/config/container#update).

```azurecli
az appservice web config update --linux-fx-version "PHP|7.0" --name <app_name> --resource-group myResourceGroup
```

## <a name="configure-local-git-deployment"></a>Configurar a implantação do git local

Você pode implantar seu Aplicativo Web de várias maneiras, incluindo o FTP, Git local, bem como o GitHub, Visual Studio Team Services e Bitbucket.

Use o comando [az appservice web source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git) para configurar o acesso do git local para o Aplicativo Web.

```azurecli
az appservice web source-control config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Copie a saída do terminal, pois ela será usada na próxima etapa.

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

## <a name="push-to-azure-from-git"></a>Enviar do Git para o Azure

Adicione um Azure remoto ao seu repositório Git local.

```bash
git remote add azure <paste-previous-command-output-here>
```

Envie para o Azure remoto para implantar seu aplicativo. Será solicitada a senha que você forneceu anteriormente como parte da criação do usuário de implantação.

```azurecli
git push azure master
```

Durante a implantação, o Serviço de Aplicativo do Azure comunicará seu andamento com o Git.

```bash
Counting objects: 2, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 352 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '25f18051e9'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.php'
remote: Ignoring: .git
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
   cc39b1e..25f1805  master -> master
```

## <a name="browse-to-the-app"></a>Navegar até o aplicativo

Navegue até o aplicativo implantado usando o navegador da Web.

```bash
http://<app_name>.azurewebsites.net
```

Neste momento, a página que exibe a mensagem Hello World está em execução usando nosso código PHP executado como um aplicativo Web do Serviço de Aplicativo do Azure.



## <a name="updating-and-deploying-the-code"></a>Atualizando e Implantando o Código

Usando um editor de texto local, abra o arquivo `index.php` no aplicativo do PHP e faça uma pequena alteração no texto dentro da cadeia de caracteres para `echo`:

```php
echo "Hello Azure!";
```

Confirme suas alterações no git, em seguida, envie as alterações do código para o Azure.

```bash
git commit -am "updated output"
git push azure master
```

Depois que a implantação for concluída, troque para a janela do navegador aberta na etapa Navegar até o aplicativo e clique em Atualizar.

![hello-world-in-browser](media/app-service-web-get-started-php/hello-world-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Gerenciar seu novo aplicativo Web do Azure

Vá para o portal do Azure para examinar o aplicativo Web que você acabou de criar.

Para fazer isso, entre em [https://portal.azure.com](https://portal.azure.com).

No menu à esquerda, clique em **Serviço de Aplicativos** e, em seguida, clique no nome do seu aplicativo Web do Azure.

![Navegação do portal para o aplicativo Web do Azure](./media/app-service-web-get-started-php/php-docs-hello-world-app-service-list.png)

Você foi para a _folha_ de seu aplicativo Web (uma página do portal que abre horizontalmente).

Por padrão, a folha de seu aplicativo Web mostra a página **Visão Geral**. Esta página fornece uma visão de como está seu aplicativo. Aqui, você também pode executar tarefas básicas de gerenciamento como procurar, parar, iniciar, reiniciar e excluir. As guias no lado esquerdo da folha mostram as páginas de configuração diferentes que você pode abrir.

![Folha Serviço de Aplicativo no portal do Azure](media/app-service-web-get-started-php/php-docs-hello-world-app-service-detail.png)

Essas guias na folha mostram muitos recursos excelentes que você pode adicionar ao seu aplicativo Web. A lista a seguir fornece algumas possibilidades:

* mapear um nome DNS personalizado
* associar um certificado SSL personalizado
* configurar uma implantação contínua
* Escalar vertical e horizontalmente
* adicionar a autenticação do usuário

**Parabéns!** Você implantou seu primeiro aplicativo PHP no Serviço de Aplicativo.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Próximas etapas

Explore os [scripts da CLI de aplicativos Web](app-service-cli-samples.md) pré-criados.
