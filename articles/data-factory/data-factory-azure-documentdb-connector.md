---
title: Mover dados de e para o DocumentDB | Microsoft Docs
description: "Saiba como mover dados para/da coleção Azure DocumentDB usando o Azure Data Factory"
services: data-factory, documentdb
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c9297b71-1bb4-4b29-ba3c-4cf1f5575fac
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: d5e13e6a96828e7c303e4d870ee170b90a0c4308
ms.lasthandoff: 04/20/2017


---
# <a name="move-data-to-and-from-documentdb-using-azure-data-factory"></a>Mover dados para e do DocumentDB usando o Azure Data Factory
Este artigo explica como usar a Atividade de Cópia no Azure Data Factory para mover dados bidirecionalmente no Azure DocumentDB. Ele se baseia no artigo [Atividades de movimentação de dados](data-factory-data-movement-activities.md), que apresenta uma visão geral da movimentação de dados com a atividade de cópia. 

Você pode copiar dados de qualquer repositório de dados de origem com suporte para o Azure DocumentDB ou do Azure DocumentDB para qualquer repositório de dados de coletor com suporte. Para obter uma lista de repositórios de dados com suporte como fontes ou coletores da atividade de cópia, confira a tabela [Repositórios de dados com suporte](data-factory-data-movement-activities.md#supported-data-stores-and-formats). 

> [!NOTE]
> A ação de copiar dados de repositórios de dados locais/IaaS do Azure para o Azure DocumentDB, e vice-versa, é permitida com a versão 2.1 ou superior do Gateway de Gerenciamento de Dados.

## <a name="supported-versions"></a>Versões com suporte
Esse conector do DocumentDB dá suporte à cópia de dados bidirecional na coleção de partição única e coleção particionada do DocumentDB. [DocDB para MongoDB](../documentdb/documentdb-protocol-mongodb.md) não tem suporte. Para copiar os dados como estão de/para arquivos JSON ou outra coleção do DocumentDB, confira [Importar/exportar documentos JSON](#importexport-json-documents).

## <a name="getting-started"></a>Introdução
Você pode criar um pipeline com uma atividade de cópia que mova dados bidirecionalmente no Azure DocumentDB usando diferentes ferramentas/APIs.

A maneira mais fácil de criar um pipeline é usar o **Assistente de Cópia**. Confira [Tutorial: Criar um pipeline usando o Assistente de Cópia](data-factory-copy-data-wizard-tutorial.md) para ver um breve passo a passo sobre como criar um pipeline usando o Assistente de cópia de dados.

Você também pode usar as seguintes ferramentas para criar um pipeline: **Portal do Azure**, **Visual Studio**, **Azure PowerShell**, **modelo do Azure Resource Manager**, **API .NET** e **API REST**. Confira o [Tutorial de atividade de cópia](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) para obter instruções passo a passo sobre a criação de um pipeline com uma atividade de cópia. 

Ao usar as ferramentas ou APIs, você executa as seguintes etapas para criar um pipeline que move dados de um armazenamento de dados de origem para um armazenamento de dados de coletor: 

1. Criar **serviços vinculados** para vincular repositórios de dados de entrada e saída ao seu data factory.
2. Criar **conjuntos de dados** para representar dados de entrada e saída para a operação de cópia. 
3. Criar um **pipeline** com uma atividade de cópia que usa um conjunto de dados como uma entrada e um conjunto de dados como uma saída. 

Ao usar o assistente, as definições de JSON para essas entidades do Data Factory (serviços vinculados, conjuntos de dados e o pipeline) são automaticamente criadas para você. Ao usar ferramentas/APIs (exceto a API .NET), você define essas entidades do Data Factory usando o formato JSON.  Para obter exemplos com definições de JSON das entidades do Data Factory usadas para copiar dados bidirecionalmente no DocumentDB, confira a seção [Exemplos de JSON](#json-examples) neste artigo. 

As seções a seguir fornecem detalhes sobre as propriedades JSON que são usadas para definir entidades do Data Factory específicas ao DocumentDB: 

## <a name="linked-service-properties"></a>Propriedades do serviço vinculado
A tabela a seguir fornece a descrição para elementos JSON específicas para o serviço de vinculado do Azure DocumentDB.

| **Propriedade** | **Descrição** | **Obrigatório** |
| --- | --- | --- |
| type |A propriedade type deve ser definida como: **DocumentDb** |Sim |
| connectionString |Especifique as informações necessárias para se conectar ao banco de dados do Azure DocumentDB. |Sim |

## <a name="dataset-properties"></a>Propriedades do conjunto de dados
Para obter uma lista completa das seções e propriedades disponíveis para definição de conjuntos de dados, consulte o artigo [Criando conjuntos de dados](data-factory-create-datasets.md). Seções como structure, availability e policy de um conjunto de dados JSON são similares para todos os tipos de conjunto de dados (SQL Azure, Blob do Azure, Tabela do Azure etc.).

A seção typeProperties é diferente para cada tipo de conjunto de dados e fornece informações sobre o local dos dados no armazenamento de dados. A seção typeProperties para o conjunto de dados do tipo **DocumentDbCollection** tem as propriedades a seguir.

| **Propriedade** | **Descrição** | **Obrigatório** |
| --- | --- | --- |
| collectionName |Nome da coleção de documentos do DocumentDB. |Sim |

Exemplo:

```JSON
{
  "name": "PersonDocumentDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "DocumentDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
### <a name="schema-by-data-factory"></a>Esquema do Data Factory
Para armazenamentos de dados sem esquema, como o DocumentDB, o serviço do Data Factory infere o esquema usando uma das seguintes maneiras:  

1. Se você especificar a estrutura dos dados usando a propriedade **structure** na definição de conjunto de dados, o serviço do Data Factory respeitará essa estrutura do esquema. Nesse caso, se uma linha não contiver um valor de uma coluna, um valor nulo será fornecido para ele.
2. Se você não especificar a estrutura dos dados usando a propriedade **structure** na definição de conjunto de dados, o serviço do Data Factory inferirá o esquema usando a primeira linha dos dados. Nesse caso, se a primeira linha não contiver o esquema completo, algumas colunas estarão ausentes no resultado da operação de cópia.

Portanto, para fontes de dados sem esquema, a prática recomendada é especificar a estrutura de dados usando a propriedade **structure** .

## <a name="copy-activity-properties"></a>Propriedades da atividade de cópia
Para obter uma lista completa das seções e propriedades disponíveis para definir atividades, veja o artigo [Criando pipelines](data-factory-create-pipelines.md). As propriedades, como nome, descrição, tabelas de entrada e saída, e política, estão disponíveis para todos os tipos de atividades.

> [!NOTE]
> A Atividade de cópia usa apenas uma entrada e produz apenas uma saída.

As propriedades disponíveis na seção typeProperties da atividade, por outro lado, variam de acordo com cada tipo de atividade e, no caso de Atividade de cópia, variam dependendo dos tipos de fontes e coletores.

No caso da Atividade de cópia, quando a fonte é do tipo **DocumentDbCollectionSource**, as seguintes propriedades estão disponíveis na seção **typeProperties**:

| **Propriedade** | **Descrição** | **Valores permitidos** | **Obrigatório** |
| --- | --- | --- | --- |
| query |Especifique a consulta para ler dados. |Cadeia de caracteres de consulta com suporte pelo DocumentDB. <br/><br/>Exemplo: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Não <br/><br/>Se não for especificada, a instrução SQL executada será: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Caractere especial para indicar que o documento está aninhado |Qualquer caractere. <br/><br/>O DocumentDB é um repositório NoSQL para documentos JSON, em que estruturas aninhadas são permitidas. O Azure Data Factory permite que o usuário indique a hierarquia via nestingSeparator, que é "." nos exemplos acima. Com o separador, a atividade de cópia vai gerar o objeto "Name" com três elementos filhos First, Middle e Last, de acordo com "Name.First", "Name.Middle" e "Name.Last" na definição da tabela. |Não |

**DocumentDbCollectionSink** dá suporte às seguintes propriedades:

| **Propriedade** | **Descrição** | **Valores permitidos** | **Obrigatório** |
| --- | --- | --- | --- |
| nestingSeparator |Um caractere especial no nome da coluna de fonte para indicar que esse documento aninhado é necessário. <br/><br/>No exemplo acima: `Name.First` na tabela de saída produz a seguinte estrutura JSON no documento do DocumentDB:<br/><br/>"Name": {<br/>    "First": "John"<br/>}, |Caractere que é usado para separar os níveis de aninhamento.<br/><br/>O valor padrão é `.` (ponto). |Caractere que é usado para separar os níveis de aninhamento. <br/><br/>O valor padrão é `.` (ponto). |
| writeBatchSize |Número de solicitações paralelas ao serviço do DocumentDB para a criação de documentos.<br/><br/>Você pode ajustar o desempenho copiando dados de/para o DocumentDB usando essa propriedade. Você pode esperar um melhor desempenho quando você aumenta writeBatchSize, pois mais solicitações paralelas para DocumentDB são enviadas. No entanto, será necessário evitar a limitação que pode gerar a mensagem de erro: "A taxa de solicitação é grande".<br/><br/>A limitação é decida por uma série de fatores, incluindo o tamanho dos documentos, o número de termos incluídos, a política de indexação da coleção de destino, etc. Para operações de cópia, você pode usar uma coleção melhor (por exemplo, S3) para ter mais taxa de transferência disponível (solicitação de 2.500 unidades/segundo). |Número inteiro |Não (padrão: 5) |
| writeBatchTimeout |Tempo de espera para a operação ser concluída antes de atingir o tempo limite. |timespan<br/><br/> Exemplo: "00:30:00" (30 minutos). |Não |

## <a name="importexport-json-documents"></a>Importar/Exportar documentos JSON
Usando esse conector do DocumentDB, você pode facilmente

* Importar documentos JSON de várias fontes para o DocumentDB, incluindo Blobs do Azure, Azure Data Lake, Sistema de Arquivos local ou outros repositórios de arquivo com suporte do Azure Data Factory.
* Exportar documentos JSON da coleção do DocumentDB para vários repositórios com base em arquivo.
* Migrar dados entre duas coleções do DocumentDB no estado em que se encontram.

Para atingir essa cópia independente de esquema, 
* Ao usar o assistente para copiar, marque a opção **"Exportar como estão para arquivos JSON ou uma coleção DocumentDB"**.
* Ao usar a edição de JSON, não especifique a seção "estrutura" nos conjuntos de dados do DocumentDB nem a propriedade "nestingSeparator" na fonte/coletor do DocumentDB na atividade de cópia. Para importar/exportar para arquivos JSON, no conjunto de dados do armazenamento de arquivo especifique o tipo de formato, como "JsonFormat," configure "filePattern" e ignore as configurações de formato restantes, consulte a seção [Formato JSON](data-factory-supported-file-and-compression-formats.md#json-format) para obter detalhes.

## <a name="json-examples"></a>Exemplos de JSON
Os exemplos a seguir fornecem amostras de definições de JSON que você pode usar para criar um pipeline usando o [Portal do Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), o [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ou o [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Tais exemplos mostram como copiar dados bidirecionalmente no Azure DocumentDB e no Armazenamento de Blobs do Azure. No entanto, os dados podem ser copiados **diretamente** de qualquer uma das fontes para qualquer um dos coletores declarados [aqui](data-factory-data-movement-activities.md#supported-data-stores-and-formats) usando a Atividade de Cópia no Azure Data Factory.

## <a name="example-copy-data-from-documentdb-to-azure-blob"></a>Exemplo: Copiar dados do DocumentDB para o Blob do Azure
O exemplo a seguir mostra:

1. Um serviço vinculado do tipo [DocumentDB](#linked-service-properties).
2. Um serviço vinculado do tipo [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Um [conjunto de dados](data-factory-create-datasets.md) de entrada do tipo [DocumentDbCollection](#dataset-properties).
4. Um [conjunto de dados](data-factory-create-datasets.md) de saída do tipo [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Um [pipeline](data-factory-create-pipelines.md) com Atividade de cópia que usa [DocumentDbCollectionSource](#copy-activity-properties) e [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

O exemplo copia dados do Azure DocumentDB para Blobs do Azure. As propriedades JSON usadas nesses exemplos são descritas nas seções após os exemplos.

**Serviço vinculado do Azure DocumentDB:**

```JSON
{
  "name": "DocumentDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
**Serviço vinculado do armazenamento de Blob do Azure:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Conjunto de dados de entrada do Azure DocumentDB:**

O exemplo pressupõe que você tenha uma coleção chamada **Person** em um banco de dados do Azure DocumentDB.

Definir “external”: ”true” e especificar a política externalData informa o serviço Azure Data Factory que essa é uma tabela externa à fábrica de dados e não é produzida por uma atividade nessa fábrica de dados.

```JSON
{
  "name": "PersonDocumentDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "DocumentDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Conjunto de dados de saída de Blob do Azure:**

Dados são copiados para um novo blob a cada hora com o caminho para o blob que reflete a data e hora específicas com granularidade de hora.

```JSON
{
  "name": "PersonBlobTableOut",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
Documento JSON de exemplo na coleção Person em um banco de dados de DocumentDB:

```JSON
{
  "PersonId": 2,
  "Name": {
    "First": "Jane",
    "Middle": "",
    "Last": "Doe"
  }
}
```
O DocumentDB dá suporte a consultas de documentos usando um SQL como sintaxe nos documentos JSON hierárquicos.

Exemplo: 

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

O pipeline a seguir copia dados da coleção Person no banco de dados DocumentDB para um blob do Azure. Como parte da atividade de cópia, os conjuntos de dados de entrada e saída foram especificados.  

```JSON
{
  "name": "DocDbToBlobPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "DocumentDbCollectionSource",
            "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
            "nestingSeparator": "."
          },
          "sink": {
            "type": "BlobSink",
            "blobWriterAddHeader": true,
            "writeBatchSize": 1000,
            "writeBatchTimeout": "00:00:59"
          }
        },
        "inputs": [
          {
            "name": "PersonDocumentDbTable"
          }
        ],
        "outputs": [
          {
            "name": "PersonBlobTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromDocDbToBlob"
      }
    ],
    "start": "2015-04-01T00:00:00Z",
    "end": "2015-04-02T00:00:00Z"
  }
}
```
## <a name="example-copy-data-from-azure-blob-to-azure-documentdb"></a>Exemplo: Copiar dados do Blob do Azure para o Azure DocumentDB
O exemplo a seguir mostra:

1. Um serviço vinculado do tipo [DocumentDB](#azure-documentdb-linked-service-properties).
2. Um serviço vinculado do tipo [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Um [conjunto de dados](data-factory-create-datasets.md) de entrada do tipo [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Um [conjunto de dados](data-factory-create-datasets.md) de saída do tipo [DocumentDbCollection](#azure-documentdb-dataset-type-properties).
5. O [pipeline](data-factory-create-pipelines.md) com a Atividade de cópia que usa [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) e [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).

O exemplo copia dados do blob do Azure para o Azure DocumentDB. As propriedades JSON usadas nesses exemplos são descritas nas seções após os exemplos.

**Serviço vinculado do armazenamento de Blob do Azure:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Serviço vinculado do DocumentDB do Azure:**

```JSON
{
  "name": "DocumentDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
**Conjunto de dados de entrada de Blob do Azure:**

```JSON
{
  "name": "PersonBlobTableIn",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "MiddleName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "fileName": "input.csv",
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
**Conjunto de dados de saída do Azure DocumentDB:**

O exemplo copia dados para uma coleção chamada "Person".

```JSON
{
  "name": "PersonDocumentDbTableOut",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "Name.First",
        "type": "String"
      },
      {
        "name": "Name.Middle",
        "type": "String"
      },
      {
        "name": "Name.Last",
        "type": "String"
      }
    ],
    "type": "DocumentDbCollection",
    "linkedServiceName": "DocumentDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
O pipeline a seguir copia dados do Blobo do Azure para a coleção Person no DocumentDB. Como parte da atividade de cópia, os conjuntos de dados de entrada e saída foram especificados.

```JSON
{
  "name": "BlobToDocDbPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "DocumentDbCollectionSink",
            "nestingSeparator": ".",
            "writeBatchSize": 2,
            "writeBatchTimeout": "00:00:00"
          }
          "translator": {
              "type": "TabularTranslator",
              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
          }
        },
        "inputs": [
          {
            "name": "PersonBlobTableIn"
          }
        ],
        "outputs": [
          {
            "name": "PersonDocumentDbTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromBlobToDocDb"
      }
    ],
    "start": "2015-04-14T00:00:00Z",
    "end": "2015-04-15T00:00:00Z"
  }
}
```
Se a entrada de blob de exemplo for

```
1,John,,Doe
```
Então, a saída JSON no DocumentDB será:

```JSON
{
  "Id": 1,
  "Name": {
    "First": "John",
    "Middle": null,
    "Last": "Doe"
  },
  "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
}
```
O DocumentDB é um repositório NoSQL para documentos JSON, em que estruturas aninhadas são permitidas. O Azure Data Factory permite que o usuário indique a hierarquia via **nestingSeparator**, que é "." neste exemplo. Com o separador, a atividade de cópia vai gerar o objeto "Name" com três elementos filhos First, Middle e Last, de acordo com "Name.First", "Name.Middle" e "Name.Last" na definição da tabela.

## <a name="appendix"></a>Apêndice
1. **Pergunta:**
    a atividade de cópia dá suporte à atualização de registros existentes?

    **Resposta**
   : não.
2. **Pergunta:**
    como uma nova tentativa de uma cópia para o DocumentDB lida com os registro já copiados?

    **Resposta:**
    se os registros têm um campo "ID" e a operação de cópia tenta inserir um registro com a mesma ID, a operação de cópia gerará um erro.  
3. **Pergunta:**
    o Data Factory oferece suporte a [intervalo ou particionamento de dados com base em hash](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/)?

    **Resposta**
   : não.
4. **Pergunta:**
    posso especificar mais de uma coleção de DocumentDB para uma tabela?

    **Resposta**
   : não. Somente uma coleção pode ser especificada no momento.

## <a name="performance-and-tuning"></a>Desempenho e Ajuste
Veja o [Guia de desempenho e ajuste da Atividade de Cópia](data-factory-copy-activity-performance.md) para saber mais sobre os principais fatores que afetam o desempenho da movimentação de dados (Atividade de Cópia) no Azure Data Factory, além de várias maneiras de otimizar esse processo.

