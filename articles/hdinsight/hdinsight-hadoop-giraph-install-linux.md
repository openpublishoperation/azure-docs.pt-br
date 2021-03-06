---
title: Instalar e usar o Giraph em clusters HDInsight (Hadoop) baseados em Linux | Microsoft Docs
description: "Saiba como instalar o Giraph em clusters HDInsight baseados em Linux usando as Ações de Script. Ações de script permitem que você personalize o cluster durante a criação, alterando a configuração do cluster ou instalando serviços e utilitários."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9fcac906-8f06-4002-9fe8-473e42f8fd0f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 98008946357e3c4f15f43f7901ac5ffa2dab0b60
ms.lasthandoff: 04/12/2017


---
# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>Instalar o Giraph nos clusters Hadoop do HDInsight e usar o Giraph para processar gráficos em grande escala

É possível instalar o Giraph em qualquer tipo de cluster no Hadoop, no Azure HDInsight, usando a **Ação de Script** para personalizar um cluster.

Neste tópico, você aprenderá a instalar o Giraph usando a Ação de Script. Depois de instalar o Giraph, você também aprenderá a usar o Giraph em suas aplicações mais comuns: processar gráficos em grande escala.

> [!IMPORTANT]
> As etapas deste documento exigem um cluster HDInsight que usa Linux. O Linux é o único sistema operacional usado no HDInsight versão 3.4 ou superior. Para saber mais, veja [Substituição do HDInsight no Windows](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

## <a name="whatis"></a>O que é o Giraph?

[Apache Giraph](http://giraph.apache.org/) permite executar processamento de gráficos usando o Hadoop e pode ser usado com o Azure HDInsight. Os gráficos modelam as relações entre objetos, como as conexões entre roteadores em uma rede grande, como a Internet, ou as relações entre as pessoas nas redes sociais (às vezes chamados de gráficos sociais). O processamento de tabelas permite que você faça a análise das relações entre objetos em uma tabela, como:

* Identificar possíveis amigos com base em suas relações atuais.

* Identificar a menor rota entre dois computadores em uma rede.

* Calcular a ordem de classificação de página da Web.

> [!WARNING]
> Há suporte total a componentes fornecidos com o cluster do HDInsight e o Suporte da Microsoft ajudará a isolar e resolver problemas relacionados a esses componentes.
>
> Componentes personalizados, como o Giraph, recebem suporte comercialmente razoável para ajudá-lo a solucionar o problema. Isso pode resultar na resolução do problema ou na solicitação de você buscar nos canais disponíveis as tecnologias de código-fonte aberto, onde é possível encontrar conhecimento aprofundado sobre essa tecnologia. Por exemplo, há muitos sites de comunidades que podem ser usados, como o [Fórum do MSDN para o HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Além disso, os projetos do Apache têm sites do projeto em [http://apache.org](http://apache.org), por exemplo: [Hadoop](http://hadoop.apache.org/).


## <a name="what-the-script-does"></a>O que o script faz

O script executa as ações a seguir:

* Instala o Giraph no `/usr/hdp/current/giraph`

* Copia o arquivo `giraph-examples.jar` para o armazenamento padrão (WASB) para o seu cluster: `/example/jars/giraph-examples.jar`

## <a name="install"></a>Instalar o Giraph usando Ações de Script

Um script de exemplo para instalar o Giraph em um cluster HDInsight está disponível no seguinte local.

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Esta seção fornece instruções sobre como usar o script de exemplo durante a criação do cluster usando o Portal do Azure.

> [!NOTE]
> Modelos do Azure PowerShell, da CLI do Azure, do SDK do .NET do HDInsight ou do Azure Resource Manager também podem ser usados para aplicar ações de script. Também é possível aplicar ações de script a clusters que já estão em execução. Para saber mais, veja [Personalizar clusters HDInsight com as Ações de Script](hdinsight-hadoop-customize-cluster-linux.md).

1. Inicie a criação de um cluster usando as etapas em [Criar clusters de HDInsight baseados em Linux](hdinsight-hadoop-create-linux-clusters-portal.md), mas não conclua a criação.

2. Na folha **Configuração Opcional**, selecione **Ações de Script** e forneça as informações a seguir:

   * **NOME**: insira um nome amigável para a ação de script.

   * **URI do SCRIPT**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

   * **CABEÇALHO**: marque esta opção

   * **TRABALHO**: deixe esta desmarcada

   * **ZOOKEEPER**: deixe esta desmarcada

   * **PARÂMETROS**: deixe este campo em branco

3. Na parte inferior das **Ações de Script**, use o botão **Selecionar** para salvar a configuração. Por fim, use o botão **Selecionar** na parte inferior da folha **Configuração opcional** para salvar as informações de configuração opcional.

4. Continue a criação do cluster conforme descrito em [Criar clusters HDInsight baseados em Linux](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="usegiraph"></a>Como usar o Giraph no HDInsight?

Quando a criação do cluster tiver terminado, use as etapas a seguir para executar o exemplo SimpleShortestPathsComputation incluído com o Giraph. Isso usa a implementação do [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) básico para encontrar o caminho mais curto entre objetos em um gráfico.

1. Conecte-se ao cluster HDInsight usando SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Para saber mais, confira [Usar SSH com HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Use o seguinte para criar um novo arquivo chamado **tiny_graph.txt**:

    ```
    nano tiny_graph.txt
    ```

    Use o seguinte como conteúdo deste arquivo:

    ```
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```

    Esses dados descrevem uma relação entre objetos em uma tabela direcionada, usando o formato `[source_id, source_value,[[dest_id], [edge_value],...]]`. Cada linha representa uma relação entre um objeto `source_id` e um ou mais objetos `dest_id`. O `edge_value` (ou peso) pode ser considerado a força ou a distância da conexão entre `source_id` e `dest\_id`.

    Desenhado e utilizando o valor (ou peso) como distância entre os objetos, os dados acima podem se parecer com os demonstrados aqui:

    ![tiny_graph.txt Desenhado como círculos com linhas de distância variável entre](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. Para salvar o arquivo, use **Ctrl + X**, em seguida, **Y** e, finalmente, **Enter** para aceitar o nome de arquivo.

4. Use o seguinte para armazenar os dados no armazenamento primário para o cluster HDInsight:

    ```
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. Execute o exemplo SimpleShortestPathsComputation usando o comando a seguir.

    ```
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```

    Os parâmetros usados com este comando são descritos na tabela a seguir.

   | Parâmetro | O que faz |
   | --- | --- |
   | `jar /usr/hdp/current/giraph/giraph-examples.jar` |O arquivo jar que contém os exemplos. |
   | `org.apache.giraph.GiraphRunner` |A classe usada para iniciar os exemplos. |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |O exemplo usado. Nesse caso, ele calcula o caminho mais curto entre a ID 1 e todas as outras IDs no gráfico. |
   | `-ca mapred.job.tracker=headnodehost:9010` |O nó de cabeçalho do cluster. |
   | `-vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFromat` |O formato de entrada a ser usado para os dados de entrada. |
   | `-vip /example/data/tiny_graph.txt` |O arquivo de dados de entrada. |
   | `-vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat` |O formato de saída. Nesse caso, a ID e o valor como texto sem formatação. |
   | `-op /example/output/shortestpaths` |O local de saída. |
   | `-w 2` |O número de trabalhos a usar. Nesse caso, 2. |

    Para obter mais informações sobre esses e outros parâmetros usados com exemplos do Giraph, consulte o [Guia de início rápido do Giraph](http://giraph.apache.org/quick_start.html).

6. Assim que o trabalho tiver sido concluído, os resultados serão armazenados no diretório **/example/out/shotestpaths**. Os nomes de arquivo saída começam com **part-m-** e terminam com um número indicando o primeiro, segundo etc. arquivo. Para exibir a saída, use o que é mostrado a seguir:

    ```
    hdfs dfs -text /example/output/shortestpaths/*
    ```

    A saída deve ter aparência similar à exibida a seguir:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    O exemplo SimpleShortestPathComputation está codificado para começar com a ID do objeto 1 e localizar o caminho mais curto para os outros objetos. Logo, a saída deve ser lida como `destination_id distance`, em que a distância é o valor (ou peso) dos perímetros percorridos entre a ID do objeto 1 e a ID de destino.

    Visualizando isso, você pode verificar os resultados percorrendo os caminhos mais curtos entre a ID 1 e todos os outros objetos. Observe que o caminho mais curto entre a ID 1 e a ID 4 é 5. Essa é a distância total entre <span style="color:orange">ID 1 e 3</span> e, em seguida, entre <span style="color:red">ID 3 e 4</span>.

    ![Desenho dos objetos como círculos com os percursos mais curtos entre](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a>Próximas etapas

* [Instalar e usar o Hue em clusters HDInsight](hdinsight-hadoop-hue-linux.md). A Matiz é uma interface da Web que torna mais fácil criar, executar e salvar trabalhos Pig e Hive, bem como procurar o armazenamento padrão do cluster do HDInsight.

* [Instalar o R em clusters HDInsight](hdinsight-hadoop-r-scripts-linux.md): instruções sobre como usar personalização do cluster para instalar e usar o R em clusters Hadoop HDInsight. R é uma linguagem e ambiente de software livre para computação estatística. Ele fornece centenas de funções estatísticas internas e sua própria linguagem de programação, que combina aspectos de programação funcional e de programação orientada a objetos. Ele também fornece recursos abrangentes de gráficos.

* [Instalar o Solr em clusters HDInsight](hdinsight-hadoop-solr-install-linux.md). Use a personalização do cluster para instalar o Solr em clusters de Hadoop do HDInsight. O Solr permite executar operações de pesquisa avançadas nos dados armazenados.

