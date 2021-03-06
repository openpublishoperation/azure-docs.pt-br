﻿---
title: Escolher um plano de hospedagem do Azure Functions | Microsoft Docs
description: "Entenda como os Azure Functions escalam para atender às necessidades das cargas de trabalho orientadas por eventos."
services: functions
documentationcenter: na
author: dariagrigoriu
manager: erikre
editor: 
tags: 
keywords: "azure functions, functions, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor"
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/04/2017
ms.author: dariagrigoriu, glenga
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: cea92fe434288012a398f6821bc9cd7ab85b7d3e
ms.lasthandoff: 04/06/2017


---
# <a name="choose-the-correct-service-plan-for-azure-functions"></a>Escolher o plano de serviço correto para o Azure Functions

## <a name="introduction"></a>Introdução

O Azure Functions tem dois planos de serviço diferentes: o plano de Consumo e o plano do Serviço de Aplicativo. O plano de Consumo aloca automaticamente potência de computação quando seu código está em execução, escala horizontalmente conforme a necessidade para tratar da carga e reduz horizontalmente quando o código não está em execução. Isso significa que você não paga por VMs ociosas nem precisa reservar capacidade antes de precisar. Este artigo se concentra no plano de serviço de Consumo. Para obter detalhes sobre como o plano do Serviço de Aplicativo funciona, consulte [Visão geral detalhada de planos de Serviço de Aplicativo do Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Se você ainda não está familiarizado com o Azure Functions, veja o artigo [Visão geral do Azure Functions](functions-overview.md).

## <a name="service-plan-options"></a>Opções de plano de serviço

Ao criar um aplicativo de funções, você deve configurar um plano de hospedagem para funções contidas no aplicativo. Os planos de hospedagem disponíveis são: o **Plano de Consumo** e o [Plano do Serviço de Aplicativo](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). Atualmente, essa opção deve ser feita durante a criação do aplicativo de funções. Você não pode alterar entre essas duas opções após a criação. É possível escalar entre camadas no [Plano do Serviço de Aplicativo](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). No momento, não é permitido fazer alterações no Plano de consumo, uma vez que o escalonamento é dinâmico.

### <a name="consumption-plan"></a>Plano de consumo

No **Plano de consumo**, seus aplicativos de funções são atribuídos a uma instância de processamento de computação. Quando necessário, mais instâncias são adicionadas ou removidas dinamicamente. Além disso, as funções são executadas em paralelo, minimizando o tempo total necessário para processar solicitações. O tempo de execução para cada função é agregado pelo aplicativo de funções que a contém. O custo é orientado pelo tamanho da memória e pelo tempo de execução total de todas as funções em um aplicativo de funções. Use um plano de consumo quando suas necessidades de computação forem intermitentes ou quando os tempos de execução de trabalhos forem curtos. Esse plano permite que você pague apenas pelos recursos de computação quando eles estiverem sendo usados. A próxima seção fornece detalhes sobre como funciona o plano de consumo.

### <a name="app-service-plan"></a>Plano do Serviço de Aplicativo

No **Plano do Serviço de Aplicativo**, seus aplicativos de funções são executados em VMs dedicadas, assim como os Aplicativos Web funcionam hoje para SKUs Básico, Padrão ou Premium. Essas VMs dedicadas são alocadas para os aplicativos do Serviço de Aplicativo e aplicativos de funções, além de estarem sempre disponíveis, não importando se o código está sendo ativamente executado ou não. Essa é uma boa opção se você possui VMs subutilizadas que já estão executando outro código, ou se pretende executar funções continuamente ou quase continuamente. Uma VM separa o custo do tempo de execução e do tamanho da memória. Consequentemente, você pode limitar o custo de muitas funções de execução longa para o custo das VMs nas quais elas são executadas. Para obter detalhes sobre como o plano do Serviço de Aplicativo funciona, consulte [Visão geral detalhada de planos de Serviço de Aplicativo do Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Com um plano do Serviço de Aplicativo, você pode escalar horizontalmente manualmente adicionando mais instâncias de VM de núcleo único ou você pode habilitar o dimensionamento automático. Para obter mais informações, consulte [Dimensionar a contagem de instâncias manual ou automaticamente](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json). Você também pode escalar verticalmente escolhendo um plano do Serviço de Aplicativo diferente. Para obter mais informações, consulte [Escalar verticalmente um aplicativo no Azure](../app-service-web/web-sites-scale.md). Se você estiver planejando executar funções do JavaScript em um plano do Serviço de Aplicativo, deverá escolher um plano com menos núcleos. Para obter mais informações, consulte a [Referência do JavaScript para funções](functions-reference-node.md#choose-single-core-app-service-plans).  

## <a name="how-the-consumption-plan-works"></a>Como funciona o plano de consumo

O plano de Consumo dimensiona automaticamente os recursos da CPU e de memória adicionando outras instâncias de processamento, de acordo com as necessidades das funções em execução no aplicativo de funções. Cada instância de processamento do aplicativo de funções pode alocar até 1,5 GB de recursos de memória.

Durante a execução em um plano de Consumo, se uma Função de Aplicativo ficar ociosa, poderá haver um atraso de até 10 minutos para o processamento de novos blobs. Quando a Função de Aplicativo está em execução, os blobs são processados mais rapidamente. Para evitar esse atraso inicial, use um Plano de Serviço de Aplicativo regular com Always On habilitado ou use outro mecanismo para disparar o processamento de blob, como uma mensagem da fila que contém o nome do blob. 

Ao criar um Aplicativo de funções, é necessário criar ou vincular uma conta de armazenamento do Azure de uso geral que dá suporte ao armazenamento de Tabelas, Blobs e Filas. Internamente, o Azure Functions usa o Armazenamento do Azure para operações como gerenciamento de gatilhos e log de execuções de função. Algumas contas de armazenamento não dão suporte a filas e tabelas, como contas de armazenamento somente blob (incluindo o armazenamento premium) e contas de armazenamento de uso geral com a replicação ZRS. Essas contas são filtradas na folha Conta de Armazenamento durante a criação de um Aplicativo de Funções.

Ao usar o plano de hospedagem de Consumo, o conteúdo do Aplicativo de Funções (como arquivos de código de função e configuração de associação) é armazenado em compartilhamentos dos Arquivos do Azure na conta de armazenamento principal. Ao excluir a conta de armazenamento principal, esse conteúdo será excluído e não poderá ser recuperado.

Para saber mais sobre tipos de conta de armazenamento, consulte [Introduzindo os Serviços de Armazenamento do Azure] (../storage/storage-introduction.md#introducing-the-azure-storage-services).

### <a name="runtime-scaling"></a>Escalonamento de tempo de execução

O Functions usa um controlador de escala para avaliar as necessidades de computação com base nos gatilhos configurados e para decidir quando escalar e reduzir horizontalmente. O controlador de escala processa dicas continuamente para requisitos de memória e pontos de dados específicos de gatilho. Por exemplo, ao usar um gatilho do Armazenamento de Filas do Azure, os pontos de dados incluem o tamanho da fila e o tempo de fila da entrada mais antiga.

![](./media/functions-scale/central-listener.png)

A unidade de escalonamento é o aplicativo de funções. Escalar horizontalmente, nesse caso, significa adicionar mais instâncias de um aplicativo de funções. Inversamente, quando a demanda de computação é reduzida, as instâncias do aplicativo de função são removidas. O número de instâncias é eventualmente reduzido a zero quando nenhuma função está em execução. 

### <a name="billing-model"></a>Modelo de cobrança

A cobrança do plano de Consumo é descrita detalhadamente na [página de preços do Azure Functions](https://azure.microsoft.com/pricing/details/functions). O uso é relatado por aplicativo de funções, somente pelo tempo que o código está sendo executado. Veja a seguir as unidades de cobrança: 
* **Consumo de recursos em GB-s (gigabyte-segundos)** calculado como uma combinação do tamanho da memória e tempo de execução para todas as funções em execução em um Aplicativo de Funções. 
* **Execuções** contadas toda vez que uma função é executada em resposta a um evento, disparado por uma associação.

