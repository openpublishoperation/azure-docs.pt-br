---
title: Associar um certificado SSL personalizado existente a aplicativos Web do Azure | Microsoft Docs
description: "Saiba como associar um certificado SSL personalizado a seu aplicativo Web, back-end do aplicativo móvel ou aplicativo de API no Serviço de Aplicativo do Azure."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 04/21/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 5bbdd1db655c080b4372f6728bb47207757209e4
ms.lasthandoff: 04/27/2017


---
# <a name="bind-an-existing-custom-ssl-certificate-to-azure-web-apps"></a>Associar um certificado SSL personalizado existente a aplicativos Web do Azure

Este tutorial mostra como associar um certificado SSL personalizado que você adquiriu de uma autoridade de certificação confiável para [aplicativos Web do Azure](app-service-web-overview.md). Quando você terminar, você poderá acessar seu aplicativo Web no ponto de extremidade HTTPS do seu domínio DNS personalizado.

![Aplicativo Web com certificado SSL personalizado](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!TIP]
> Se você precisar obter um certificado SSL personalizado, você poderá obtê-lo diretamente no Portal do Azure e associá-lo ao seu aplicativo Web. Siga o [tutorial de Certificados do Serviço de Aplicativo](web-sites-purchase-ssl-web-site.md). 
>
> 

## <a name="before-you-begin"></a>Antes de começar
Antes de seguir esse tutorial, certifique-se de ter feito o seguinte:

- [Crie um aplicativo do Serviço de Aplicativo](/azure/app-service/)
- [Mapear um nome DNS personalizado para o aplicativo Web](web-sites-custom-domain-name.md)
- Adquirir um certificado SSL de uma autoridade de certificado confiável

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a>Requisitos para o certificado SSL

Para usar o certificado no Serviço de Aplicativo, o certificado deve atender a todos os requisitos a seguir:

* Assinado por uma autoridade de certificado confiável
* Exportado como um arquivo PFX protegido por senha
* Conter chave privada com pelo menos 2.048 bits de extensão
* Conter todos os certificados intermediários na cadeia de certificados

> [!NOTE]
> **Certificados ECC (Criptografia de Curva Elíptica)** podem funcionar com o Serviço de Aplicativo, mas fora do escopo deste artigo. Trabalhe com sua autoridade de certificação seguindo exatamente estas etapas para criar certificados ECC.
> 
>

## <a name="prepare-your-web-app"></a>Preparar o aplicativo Web
Para associar um certificado SSL personalizado ao aplicativo Web, o [Plano do Serviço de Aplicativo](https://azure.microsoft.com/pricing/details/app-service/) deve estar na camada **Básica**, **Standard** ou **Premium**. Nesta etapa, você precisa ter certeza de que seu aplicativo Web está no tipo de preço com suporte.

### <a name="log-in-to-azure"></a>Fazer logon no Azure

Abra o portal do Azure. Para fazer isso, entre em [https://portal.azure.com](https://portal.azure.com) usando sua conta do Azure.

### <a name="navigate-to-your-web-app"></a>Navegue até seu aplicativo Web
No menu à esquerda, clique em **Serviços de Aplicativos** e, em seguida, clique no nome do seu aplicativo Web.

![Selecionar aplicativo Web](./media/app-service-web-tutorial-custom-ssl/select-app.png)

Você foi para a folha de gerenciamento de seu aplicativo Web (_folha_: uma página do portal que abre horizontalmente).  

### <a name="check-the-pricing-tier"></a>Verifique o tipo de preço

No painel de navegação à esquerda da folha do aplicativo Web, role até a seção **Configurações** e selecione **Escalar verticalmente (Plano do Serviço de Aplicativo)**.

![Menu Escalar verticalmente](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

Certifique-se de que o aplicativo Web não esteja na camada **Gratuita** ou **Compartilhada**. A camada atual do seu aplicativo Web é realçada por uma caixa azul escuro. 

![Verificar tipo de preço](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

Não há suporte para SSL personalizado nas camadas **Gratuita** e **Compartilhada**. Se você precisar escalar verticalmente, siga a próxima seção. Caso contrário, feche a folha **Escolha seu tipo de preço** e pule para [Carregar e associar o certificado SSL](#upload).

### <a name="scale-up-your-app-service-plan"></a>Escalar verticalmente seu Plano do Serviço de Aplicativo

Selecione uma das camadas **Básico**, **Standard** ou **Premium**. 

Clique em **Selecionar**.

![Escolha um tipo de preço](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

Quando você vir a notificação abaixo, a operação de escala terá sido concluída.

![Escalar verticalmente a notificação](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a>Associar o certificado SSL

Você está pronto para carregar seu certificado SSL para o aplicativo Web. 

### <a name="export-certificate-to-pfx"></a>Exportar o certificado para PFX

Você deve exportar o certificado SSL personalizado com a chave privada com a qual a solicitação de certificado foi gerada.

Se você gerou sua solicitação de certificado usando OpenSSL, você criou uma chave privada. Para exportar o certificado para PFX, execute o seguinte comando:

```bash
openssl pkcs12 -export -out myserver.pfx -inkey myserver.key -in myserver.crt
```

Se você usou o IIS ou `Certreq.exe` para gerar a solicitação de certificado, primeiro instale o certificado em seu computador local, então exporte-o para PFX seguindo as etapas em [Exportar um certificado com a chave privada](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

### <a name="upload-your-ssl-certificate"></a>Carregar o certificado SSL

Para carregar o certificado SSL, clique em **Certificados SSL** no painel de navegação esquerdo do seu aplicativo Web.

Clique em **Carregar Certificado**.

Em **Arquivo de Certificado PFX**, selecione o arquivo PFX. Em **Senha do certificado**, digite a senha que você criou ao exportar o arquivo PFX.

Clique em **Carregar**.

![Carregar um certificado](./media/app-service-web-tutorial-custom-ssl/upload-certificate.png)

Quando o Serviço de Aplicativo terminar de carregar o certificado, ele aparecerá na página **Certificados SSL**.

![Certificado carregado](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a>Associar o certificado SSL

Agora, você deverá ver seu certificado carregado de volta na página **Certificado SSL** .

Na seção **Associações SSL**, clique em **Adicionar associação**.

Na folha **Adicionar Associação SSL**, use os menus suspensos para selecionar o nome de domínio a ser protegido e o certificado a ser usado. 

Em **Tipo SSL**, selecione se deseja usar **[SNI (Indicação de Nome de Servidor)](http://en.wikipedia.org/wiki/Server_Name_Indication)** ou SSL baseado em IP.
   
- **SSL baseado em SNI** – várias associações SSL baseadas em SNI podem ser adicionadas. Esta opção permite que vários certificados SSL protejam vários domínios no mesmo endereço IP. Navegadores mais modernos (incluindo Internet Explorer, Chrome, Firefox e Opera) dão suporte ao SNI (encontre informações de suporte ao navegador mais abrangentes em [Indicação de Nome de Servidor](http://wikipedia.org/wiki/Server_Name_Indication)).
- **SSL baseado em IP** – apenas uma associação SSL de IP pode ser adicionada. Esta opção permite apenas um certificado SSL para proteger um endereço IP público dedicado. Para proteger vários domínios, você deve protegê-los todos usando o mesmo certificado SSL. Essa é a opção tradicional para associação de SSL. 

Clique em **Adicionar Associação**.

![Associar Certificado SSL](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

Quando o Serviço de Aplicativo terminar de carregar o certificado, ele aparecerá nas seções de **associações SSL**.

![Certificado associado ao aplicativo Web](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a>Remapear um registro para IP SSL

Se você não usar SSL com base em IP em seu aplicativo Web, pule para [Testar o HTTPS para seu domínio personalizado](#test). 

Por padrão, o seu aplicativo Web usa um endereço IP público compartilhado. Assim que você associar um certificado com SSL baseado em IP, o Serviço de Aplicativo criará um novo endereço IP dedicado para seu aplicativo Web.

Se você tiver mapeado um registro A para seu aplicativo Web, atualize o Registro do domínio com esse novo endereço IP dedicado.

A página **domínio personalizado** de seu aplicativo Web é atualizada com o novo endereço IP dedicado. [Copie esse endereço IP](app-service-web-tutorial-custom-domain.md#info) e, em seguida, [remapeie o registro](app-service-web-tutorial-custom-domain.md#create-the-a-record) para esse novo endereço IP.

<a name="test"></a>

## <a name="test-https"></a>Testar HTTPS
Agora, tudo o que resta fazer é certificar-se de que o HTTPS funcione com seu domínio personalizado. Em vários navegadores, navegue até `https://<your.custom.domain>` para ver se ele leva até seu aplicativo Web.

![Navegação no Portal para o aplicativo do Azure](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Se o seu aplicativo Web retorna erros de validação de certificado, você provavelmente está usando um certificado autoassinado.
>
> Se não for o caso, talvez você tenha deixado de fora certificados intermediários quando exportou o certificado para o arquivo PFX. 
>
>

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a>Impor HTTPS
Se ainda quiser permitir acesso HTTP ao seu aplicativo Web, ignore esta etapa. 

O Serviço de Aplicativo *não* impõe o HTTPS, de modo que qualquer um ainda pode acessar seu aplicativo Web usando HTTP. Para impor HTTPS para seu aplicativo Web, você poderá definir uma regra de reescrita no arquivo `web.config` de seu aplicativo Web. Todo aplicativo do Serviço de Aplicativo usa esse arquivo, independentemente da estrutura de linguagem do seu aplicativo Web.

> [!NOTE]
> Há um redirecionamento de solicitações específico a um idioma. ASP.NET MVC pode usar o filtro [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) em vez da regra de reescrita em `web.config` (consulte [Implantar um aplicativo ASP.NET MVC 5 seguro em um aplicativo Web](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)).
> 
> 

Se você é um desenvolvedor do .NET, você deve estar relativamente familiarizado com esse arquivo. Ele está na raiz de sua solução.

Como alternativa, se você desenvolve com PHP, Node.js, Python ou Java, há uma chance de termos gerado esse arquivo em seu nome no Serviço de Aplicativo.

Conecte-se ao ponto de extremidade FTP do seu aplicativo Web, seguindo as instruções em [Implantar seu aplicativo no Serviço de Aplicativo do Azure usando FTP/S](app-service-deploy-ftp.md). 

Esse arquivo deve estar localizado em `/home/site/wwwroot`. Caso contrário, crie um `web.config` nesta pasta com o seguinte XML:

```xml   
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <!-- BEGIN rule TAG FOR HTTPS REDIRECT -->
        <rule name="Force HTTPS" enabled="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{HTTPS}" pattern="off" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
        </rule>
        <!-- END rule TAG FOR HTTPS REDIRECT -->
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

Para um `web.config` existente, basta copiar toda a marcação `<rule>` no elemento `configuration/system.webServer/rewrite/rules` do seu `web.config`. Se houver outras marcações `<rule>` em seu `web.config`, coloque a marcação `<rule>` copiada antes das outras marcações `<rule>`.

Essa regra retorna um HTTP 301 (redirecionamento permanente) para o protocolo HTTPS sempre que o usuário fizer uma solicitação HTTP ao aplicativo Web. Por exemplo, ele redireciona de `http://contoso.com` para `https://contoso.com`.

Para obter mais informações sobre o Módulo de Reescrita de URL do IIS, consulte a documentação [Reescrita de URL](http://www.iis.net/downloads/microsoft/url-rewrite) .

## <a name="automate-with-scripts"></a>Automatizar com scripts

Você pode automatizar associações SSL para seu aplicativo Web com scripts, usando a [CLI do Azure](/cli/azure/install-azure-cli) ou o [Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>CLI do Azure

O comando a seguir carrega um arquivo PFX exportado e obtém a impressão digital. 

```bash
thumprint=$(az appservice web config ssl upload --certificate-file <path_to_PFX_file> \
--certificate-password <PFX_password> --name <app_name> --resource-group <resource_group_name> \
--query thumbprint --output tsv)
```

O comando a seguir adiciona uma associação de SSL baseado em SNI usando a impressão digital do comando anterior.

```bash
az appservice web config ssl bind --certificate-thumbprint $thumbprint --ssl-type SNI \
--name <app_name> --resource-group <resource_group_name>
```

### <a name="azure-powershell"></a>Azure PowerShell

O comando a seguir carrega um arquivo PFX exportado e adiciona uma associação de SSL baseado em SNI.

```PowerShell
New-AzureRmWebAppSSLBinding -WebAppName <app_name> -ResourceGroupName <resource_group_name> -Name <dns_name> `
-CertificateFilePath <path_to_PFX_file> -CertificatePassword <PFX_password> -SslState SniEnabled
```
## <a name="more-resources"></a>Mais Recursos
* [Central de confiabilidade do Microsoft Azure](/support/trust-center/security/)
* [Opções de configuração desbloqueadas nos Sites do Azure](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/)
* [Habilitar registro em log de diagnóstico](web-sites-enable-diagnostic-log.md)
* [Configurar aplicativos Web no Serviço de Aplicativo do Azure](web-sites-configure.md)

