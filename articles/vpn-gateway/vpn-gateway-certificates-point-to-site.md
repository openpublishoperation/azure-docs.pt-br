---
title: 'Criar certificados autoassinados para ponto a site: PowerShell: Azure | Microsoft Docs'
description: "Este artigo contém etapas para criar um certificado raiz autoassinado, exportar a chave pública e gerar os certificados do cliente usando o PowerShell no Windows 10."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 27b99f7c-50dc-4f88-8a6e-d60080819a43
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/24/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: 72fc6eb93c77dd5a0a7ce55897f4c06fb98c0721
ms.lasthandoff: 04/25/2017


---
# <a name="create-a-self-signed-root-certificate-for-point-to-site-connections-using-powershell"></a>Criar um certificado raiz autoassinado para conexões ponto a site usando o PowerShell

Conexões ponto a site usam certificados para se autenticar. Quando você configura uma conexão ponto a site, você precisa carregar a chave pública (arquivo .cer) de um certificado raiz no Azure. Certificados de cliente devem ser gerados a partir do certificado raiz e instalados em cada computador cliente que se conecta à rede virtual. O certificado de cliente permite que o cliente seja autenticado. Este artigo mostra como criar um certificado autoassinado, exportar a chave pública e gerar certificados do cliente. Este artigo contém instruções de configuração do Site de ponto ou as perguntas frequentes sobre o Point-to-Site. Você pode encontrar essas informações, selecionando um dos artigos da lista a seguir:

> [!div class="op_single_selector"]
> * [Criar certificados autoassinados](vpn-gateway-certificates-point-to-site.md)
> * [Configurar o Point-to-Site - Gerenciador de recursos - Portal do Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Configurar o Point-to-Site - Gerenciador de recursos - Portal do Azure](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Configurar o Point-to-Site - Gerenciador de recursos - Portal do Azure](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

## <a name="rootcert"></a>Criar um certificado raiz autoassinado

As etapas a seguir orientam para criar um certificado autoassinado usando o PowerShell no Windows 10. Os cmdlets e parâmetros que são usados nessas etapas fazem parte do sistema operacional Windows 10, não são parte de uma versão do PowerShell. Isso não significa que os certificados criados por você só podem ser instalados no Windows 10. Para obter informações sobre clientes com suporte, consulte [Point-to-Site perguntas frequentes sobre](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq).

1. Em um computador com Windows 10, abra um console do Windows PowerShell com privilégios elevados.
2. Use o exemplo a seguir para criar o certificado raiz autoassinado. O exemplo a seguir cria um certificado raiz autoassinado chamado 'P2SRootCert' que é instalado automaticamente em 'Certificates-Current User\Personal\Certificates'. Você pode exibir o certificado abrindo *certmgr.msc*.

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

### <a name="cer"></a>Para obter a chave pública

Conexões ponto a site exigem a chave pública (.cer) carregadas no Azure. As etapas a seguir ajudarão você a exportar o arquivo .cer para seu certificado raiz autoassinado:

1. Para obter um arquivo .cer do certificado, abra **Gerenciar certificados de usuário**. Localize o certificado raiz autoassinado, normalmente em 'Certificados – Usuário Atual\Pessoal\Certificados', então clique nele com o botão direito do mouse. Clique em **Todas as Tarefas** e, em seguida, em **Exportar**. Isso abre o **Assistente para Exportação de Certificados**.
2. No Assistente, clique em **Avançar**. Selecione **Não exportar a chave privada** e clique em **Avançar**.
3. Na página **Exportar Formato de Arquivo**, selecione **X.509 codificado em Base 64 (.CER).** e clique em **Avançar**. 
4. Em **Arquivo a ser Exportado**, use **Procurar** para encontrar a localização para a qual você deseja exportar o certificado. Em **Nome do arquivo**, dê um nome ao arquivo de certificado. Em seguida, clique em **Avançar**.
5. Clique em **Concluir** para exportar o certificado. Você verá a mensagem **A exportação foi bem-sucedida**. Clique em **OK** para fechar o assistente.

### <a name="to-export-a-self-signed-root-certificate-optional"></a>Para exportar um certificado raiz autoassinado (opcional)

Convém exportar o certificado raiz autoassinado e armazená-lo com segurança. Se precisar, você pode instalá-lo depois em outro computador e gerar mais certificados de cliente, ou exportar outro arquivo .cer. Para exportar o certificado raiz autoassinado como um .pfx, selecione o certificado raiz e use as mesmas etapas, conforme descrito em [Exportar um certificado do cliente](#clientexport) para exportar.

## <a name="clientcert"></a>Gerar um certificado do cliente

Cada computador cliente que se conecta a uma rede virtual usando ponto a site deve ter um certificado do cliente instalado. Você gera um certificado do cliente com base no certificado raiz autoassinado e, em seguida, a exporta e instala o certificado do cliente. Se o certificado do cliente não estiver instalado, a autenticação falhará. 

As etapas abaixo lhe guiarão pela geração de um certificado do cliente por meio de um certificado raiz autoassinado. Você pode gerar vários certificados de cliente a partir do mesmo certificado raiz. Quando você gerar certificados do cliente usando as etapas a seguir, o certificado do cliente será instalado automaticamente no computador que você tiver usado para gerar o certificado. Se você quiser instalar um certificado do cliente em outro computador cliente, você poderá exportar o certificado.

Windows 10 é necessária para gerar certificados de cliente usando as seguintes etapas do PowerShell. Os cmdlets e parâmetros que são usados nessas etapas fazem parte do sistema operacional Windows 10, não são parte de uma versão do PowerShell. Isso não significa que os certificados criados por você só podem ser instalados no Windows 10. Para obter informações sobre clientes com suporte, consulte [Point-to-Site perguntas frequentes sobre](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq).

### <a name="example-1"></a>Exemplo 1

Este exemplo usa a variável '$cert' declarada da seção anterior. Se você fechou o console do PowerShell após criar o certificado raiz autoassinado ou está criando certificados do cliente adicionais em uma nova sessão de console do PowerShell, use as etapas no [Examplo 2](#ex2).

Modifique e execute o exemplo para gerar um certificado do cliente. Se você executar o exemplo a seguir sem modificá-lo, o resultado será um certificado do cliente chamado 'P2SChildCert'.  Se você deseja nomear o certificado filho diferentemente, modifique o valor de CN. Não altere a TextExtension ao executar esse exemplo. O certificado do cliente que você gera é instalado automaticamente em 'Certificates - Current User\Personal\Certificates' em seu computador.

```powershell
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <a name="ex2"></a>Exemplo 2

Se você estiver criando certificados do cliente adicionais ou não estiver usando a mesma sessão do PowerShell usada para criar o certificado raiz autoassinado, use as seguintes etapas:

1. Identifique o certificado raiz autoassinado instalado no computador. Esse cmdlet retorna uma lista de certificados instalados no seu computador.

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. Localize o nome da entidade da lista retornada e copie a impressão digital localizada ao lado dela para um arquivo de texto. No exemplo a seguir, há dois certificados. O nome CN é o nome do certificado raiz autoassinado do qual você deseja gerar um certificado filho. Nesse caso, 'P2SRootCert'.

        Thumbprint                                Subject

        AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
        7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert


3. Declare uma variável para o certificado raiz usando a impressão digital da etapa anterior. Substitua THUMBPRINT pela impressão digital do certificado raiz do qual você deseja gerar um certificado filho.

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  Por exemplo, usando a impressão digital para P2SRootCert na etapa anterior, a variável teria esta aparência:

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```    

4.  Modifique e execute o exemplo para gerar um certificado do cliente. Se você executar o exemplo a seguir sem modificá-lo, o resultado será um certificado do cliente chamado 'P2SChildCert'. Se você deseja nomear o certificado filho diferentemente, modifique o valor de CN. Não altere a TextExtension ao executar esse exemplo. O certificado do cliente que você gera é instalado automaticamente em 'Certificates - Current User\Personal\Certificates' em seu computador.

  ```powershell
  New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <a name="clientexport"></a>Exportar um certificado do cliente   

Quando você gerar um certificado do cliente, ele será instalado automaticamente no computador que você usou para gerá-lo. Se você quiser instalar um certificado do cliente em outro computador cliente, será necessário exportar o certificado do cliente que você gerou.                              

1. Para exportar um certificado de cliente, abra **Gerenciar certificados de usuário**. Os certificados do cliente que você gerou são, por padrão, localizados em 'Certificates - Current User\Personal\Certificates'. Clique com o botão direito do mouse no certificado do cliente que você deseja exportar, clique em **todas as tarefas** e clique em **exportar** para abrir o **Assistente para Exportação de Certificados**.
2. No Assistente, clique em **Avançar**, escolha **Sim, exportar a chave privada** e clique em **Avançar**.
3. Na página **Formato do Arquivo de Exportação**, deixe os padrões selecionados. Verifique se a opção **Incluir todos os certificados no caminho de certificação, se possível** está selecionada. Selecionar essa opção também exporta as informações de certificado raiz que são necessárias para uma autenticação bem-sucedida. Em seguida, clique em **Avançar**.
4. Na página **Segurança** , você deve proteger a chave privada. Se você optar por usar uma senha, não deixe de anotar ou lembrar da senha definida para esse certificado. Em seguida, clique em **Avançar**.
5. Em **Arquivo a ser Exportado**, use **Procurar** para encontrar a localização para a qual você deseja exportar o certificado. Em **Nome do arquivo**, dê um nome ao arquivo de certificado. Em seguida, clique em **Avançar**.
6. Clique em **Concluir** para exportar o certificado.    

## <a name="install"></a>Instalar um certificado do cliente exportado

Se você quiser criar uma conexão P2S de um computador cliente diferente daquele usada para gerar os certificados cliente, instale um certificado de cliente. Ao instalar um certificado do cliente, você precisará da senha criada durante a exportação do certificado do cliente.

1. Localize e copie o arquivo *.pfx* no computador cliente. No computador cliente, clique duas vezes no arquivo *.pfx* para instalá-lo. Deixe o **Local do Repositório** como **Usuário Atual** e clique em **Avançar**.
2. Na página **Arquivo** a importar, não faça nenhuma alteração. Clique em **Próximo**.
3. Na página **Proteção da chave privada**, insira a senha do certificado ou verifique se a entidade de segurança está correta e clique em **Avançar**.
4. Na página **Repositório de Certificados**, deixe a localização padrão e clique em **Avançar**.
5. Clique em **Concluir**. No **Aviso de Segurança** da instalação do certificado, clique em **Sim**. Clique em 'Sim' sem medo, pois você gerou o certificado. O certificado foi importado com êxito.

## <a name="next-steps"></a>Próximas etapas

Continue com a configuração de Ponto a Site. 

* Para as etapas do modelo de implantação do **Resource Manager**, veja [Configurar uma conexão Ponto a Site a uma VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md). 
* Para as etapas do modelo de implantação **clássico**, veja [Configurar uma conexão VPN Ponto a Site para uma VNet (clássico)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).
