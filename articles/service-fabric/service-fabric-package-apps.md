---
title: Empacotar um aplicativo do Azure Service Fabric | Microsoft Docs
description: Como empacotar um aplicativo do Service Fabric antes de implantar em um cluster.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/24/2017
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 15b6f6c85c5a5accbd31225c277de87346a2e16f
ms.lasthandoff: 04/27/2017


---
# <a name="package-an-application"></a>Preparar um aplicativo
Este artigo descreve como empacotar um aplicativo do Service Fabric e deixá-lo pronto para implantação.

## <a name="package-layout"></a>Layout do pacote
O manifesto do aplicativo, os manifestos do serviço e outros arquivos de pacote necessários devem ser organizados em um layout específico para a implantação em um cluster Malha do Serviço. Os manifestos de exemplo neste artigo precisariam estar organizados na seguinte estrutura de diretórios:

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

As pastas são nomeadas para corresponder a atributos **Name** de cada elemento correspondente. Por exemplo, se o manifesto do serviço contiver dois pacotes de códigos com os nomes **MyCodeA** e **MyCodeB**, então, duas pastas com os mesmos nomes conteriam os binários necessários para cada pacote de códigos.

## <a name="use-setupentrypoint"></a>Use o SetupEntryPoint
Cenários típicos de uso do **SetupEntryPoint** quando você precisa executar um executável antes do início do serviço ou você precisa executar uma operação com privilégios elevados. Por exemplo:

* Configurar e inicializar as variáveis de ambiente que o serviço executável precisa. Isso não é limitado a apenas executáveis gravados usando os modelos de programação do Service Fabric. Por exemplo, npm.exe precisa de algumas variáveis de ambiente configurados para implantar um aplicativo node.js.
* Configurando o controle de acesso, instalando certificados de segurança.

Para obter mais detalhes sobre como configurar o **SetupEntryPoint**, confira [Configurar a política para um ponto de entrada de instalação do serviço](service-fabric-application-runas-security.md)  

## <a name="configure"></a>Configurar 
### <a name="build-a-package-by-using-visual-studio"></a>Compilar um pacote usando o Visual Studio
Se você usar o Visual Studio 2015 para criar o seu aplicativo, pode usar o comando Package para criar automaticamente um pacote que corresponda ao layout descrito acima.

Para criar um pacote, clique com o botão direito do mouse no projeto de aplicativo no Gerenciador de Soluções e escolha o comando de Pacote, conforme mostrado abaixo:

![Empacotando um aplicativo no Visual Studio][vs-package-command]

Quando o empacotamento estiver concluído, você poderá encontrar o local do pacote na janela **Saída**. A etapa de empacotamento ocorre automaticamente quando você implanta ou depura seu aplicativo no Visual Studio.

### <a name="build-a-package-by-command-line"></a>Criar um pacote pela linha de comando
Também é possível empacote seu aplicativo de modo programático usando `msbuild.exe`. Em segundo plano, é isso que o Visual Studio está executando, de modo que a saída é a mesma.

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-the-package"></a>Teste o pacote
Você pode verificar a estrutura do pacote localmente por meio do PowerShell usando o comando [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) .
Esse comando verifica se há problemas de análise de manifesto e também todas as referências. Observe que esse comando só verifica a correção estrutural de diretórios e arquivos no pacote.
Ele não verificará nenhum código ou conteúdo do pacote de dados além de verificar se todos os arquivos necessários estão presentes.

```
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

Esse erro mostra que o arquivo *MySetup.bat* referenciado no manifesto do serviço **SetupEntryPoint** está ausente do pacote de código. Depois de adicionar o arquivo que está faltando, a verificação do aplicativo é aprovada:

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
```

Se seu aplicativo tiver [parâmetros do aplicativo](service-fabric-manage-multiple-environment-app-configuration.md) definidos, você poderá passá-los em [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) para a validação correta.

Se você souber o cluster em que o aplicativo será implantado, recomenda-se que você passe a cadeia de conexão do repositório de imagens. Nesse caso, o pacote também é validado para as versões anteriores do aplicativo que já estão em execução no cluster. Por exemplo, a validação pode detectar se um pacote com a mesma versão mas conteúdo diferente já foi implantado.  

Depois que o aplicativo for empacotado corretamente for validado com êxito, avalie com base no tamanho e no número de arquivos se há necessidade de compactação. 

## <a name="compress-a-package"></a>Compactar um pacote
Quando um pacote é grande ou tem vários arquivos, você pode compactá-lo para uma implantação mais rápida. A compactação reduz o número de arquivos e o tamanho do pacote.
[Carregar o pacote de aplicativos](service-fabric-deploy-remove-applications.md#upload-the-application-package) pode demorar mais do que carregar o pacote não compactado, mas o [registro](service-fabric-deploy-remove-applications.md#register-the-application-package) e o [cancelamento do registro do tipo de aplicativo](service-fabric-deploy-remove-applications.md#unregister-an-application-type) é mais rápido.

O mecanismo de implantação é o mesmo para pacotes compactados e não compactados. Se o pacote for compactado ele será armazenado como tal no repositório de imagens do cluster e será descompactado no nó antes que o aplicativo seja executado.
A compactação substitui o pacote do Service Fabric válido pela versão compactada. A pasta deve aceitar permissões de gravação. A execução da compactação em um pacote já compactado não produz nenhuma alteração. 

Você pode compactar um pacote executando o comando do PowerShell [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) com a opção `CompressPackage`. Você pode descompactar o pacote com o mesmo comando, usando a opção `UncompressPackage`.

O comando a seguir compacta o pacote sem copiá-lo para o repositório de imagens. Você pode copiar um pacote compactado para um ou mais clusters do Service Fabric, conforme necessário, usando [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) sem o sinalizador `SkipCopy`. O pacote agora inclui arquivos compactados para os pacotes `code`, `config` e `data`. O manifesto do aplicativo e os manifestos do serviço não são compactados porque eles são necessários para várias operações internas (como compartilhamento do pacote, extração da versão e nome do tipo de aplicativo para determinadas validações).
Compactar os manifestos tornaria essas operações ineficientes.

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -CompressPackage -SkipCopy

PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
       ServiceManifest.xml
       MyCode.zip
       MyConfig.zip
       MyData.zip

```

Como alternativa, você pode compactar e copiar o pacote com [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) em uma única etapa.
Se o pacote for grande, forneça um tempo limite suficientemente longo para que haja tempo tanto para a compactação do pacote quanto para o upload para o cluster.
```
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

Internamente, o Service Fabric computa somas de verificação para os pacotes de aplicativos para validação. Ao usar a compactação, as somas de verificação são calculadas nas versões compactadas de cada pacote.
Se você copiou uma versão não compactada do seu pacote de aplicativos e deseja usar compactação para o mesmo pacote, é necessário alterar a versão dos pacotes `code`, `config` e `data` para evitar a incompatibilidade de soma de verificação. Se os pacotes não forem alterados, em vez de alterar a versão, você poderá usar o [provisionamento de comparação](service-fabric-application-upgrade-advanced.md). Com essa opção, não inclua o pacote inalterado, mas apenas faça referência a ele no manifesto do serviço.

Da mesma forma, se você carregou uma versão compactada do pacote e deseja usar um pacote descompactado, é necessário atualizar as versões para evitar a incompatibilidade da soma de verificação.

O pacote agora é empacotado corretamente, validado e compactado (se necessário), para que esteja pronto para [implantação](service-fabric-deploy-remove-applications.md) em um ou mais clusters do Service Fabric.

## <a name="next-steps"></a>Próximas etapas
[Implantar e remover aplicativos][10] descreve como usar o PowerShell para gerenciar instâncias do aplicativo

[Gerenciamento de parâmetros do aplicativo para vários ambientes][11] descreve como configurar os parâmetros e variáveis de ambiente para diferentes instâncias do aplicativo.

[Configurar políticas de segurança para seu aplicativo][12] descreve como executar serviços sob políticas de segurança para restringir o acesso.

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md

