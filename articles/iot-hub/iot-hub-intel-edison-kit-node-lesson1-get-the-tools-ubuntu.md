---
title: "Conectar o Intel Edison (Nó) ao IoT do Azure - Lição 1: obter ferramentas (Ubuntu) | Microsoft Docs"
description: "Baixe e instale as ferramentas e softwares necessários para o primeiro aplicativo de exemplo do Edison no Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: ferramentas de desenvolvimento arduino, desenvolvimento de iot, software de iot, software de Internet das coisas, instalar o git no ubuntu, instalar node js no ubuntu
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 9ab5b161-7ec5-41a6-9c5f-4456e4882752
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: adf5b10721a28432e6b37ef73c6a7e7ec9f93cdd
ms.openlocfilehash: c56699b81d83119ca1822c2efa6b2380b3619290
ms.lasthandoff: 01/25/2017


---
# <a name="get-the-tools-ubuntu-1604"></a>Obter as ferramentas (Ubuntu 16.04)

> [!div class="op_single_selector"]
> * [Windows 7 ou posterior][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>O que você fará
Baixar as ferramentas de desenvolvimento e o software para o primeiro aplicativo de exemplo do Intel Edison. Se você tiver problemas, procure por soluções na [página de solução de problemas][troubleshooting].

## <a name="what-you-will-learn"></a>O que você aprenderá
Neste artigo, você aprenderá:

* Como instalar o Git e o Node.js
  * [Git](https://git-scm.com) é um software livre de sistema de controle de versão distribuído. O aplicativo de exemplo para este artigo está armazenado em Git.
  * O [Node.js](https://nodejs.org/en/) é um tempo de execução de JavaScript com um avançado ecossistema de pacote.
* Como usar o NPM (gerenciador de pacotes Node.js) para instalar ferramentas de desenvolvimento adicionais do Node.js.
  * A versão mínima necessária do Node.js é a 4.5 LTS.
  * O [NPM](https://www.npmjs.com) é um dos gerenciadores de pacote para o Node.js.

## <a name="what-you-need"></a>O que você precisa
Para concluir esta operação, você precisará de:
* Uma conexão com a Internet para baixar as ferramentas de desenvolvimento e o software.
* Um computador que esteja executando o Ubuntu 16.04 ou posterior.

## <a name="install-git-nodejs-and-npm"></a>Instale o Git, Node.js e o NPM
Use o atalho de teclado `Ctrl + Alt + T` para abrir um terminal e execute os seguintes comandos:

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a>Instalar ferramentas adicionais de desenvolvimento do Node.js
Use [gulp.js](http://gulpjs.com) para automatizar a implantação do aplicativo de exemplo no Edison.

Instale `gulp` executando o seguinte comando no terminal:

```bash
sudo npm install -g gulp
```

Se você tiver problemas ao instalar o Node.js e essas ferramentas adicionais de desenvolvimento no Ubuntu, consulte o [guia de solução de problemas][troubleshooting] para soluções de problemas comuns.

## <a name="install-visual-studio-code"></a>Instalar o Visual Studio Code
[Baixe](https://code.visualstudio.com/docs/setup/linux) e instale o Visual Studio Code. O Visual Studio Code é um editor de código-fonte leve mas poderoso para Windows, Linux e macOS. Use este editor posteriormente no tutorial para editar o código de exemplo.

## <a name="summary"></a>Resumo
Você instalou as ferramentas de desenvolvimento e software necessários para o primeiro aplicativo de exemplo. A tarefa seguinte é criar, implantar e executar o aplicativo de exemplo no Edison.

## <a name="next-steps"></a>Próximas etapas
[Criar e implantar o aplicativo de exemplo de piscar][create-and-deploy-the-blink-application]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md

