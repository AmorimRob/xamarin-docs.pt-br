---
title: Consumo de um banco de dados de documento de banco de dados do Cosmos do Azure
description: "Um banco de dados de documento de banco de dados do Azure Cosmos é um banco de dados NoSQL que fornece acesso de baixa latência para documentos JSON, oferecendo um serviço de banco de dados rápida, altamente disponível e dimensionável para aplicativos que necessitam de replicação global e escala contínua. Este artigo explica como usar a biblioteca de cliente de documentos do Microsoft Azure para integrar um banco de dados de documento de banco de dados do Azure Cosmos em um aplicativo xamarin. Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C0605D9-9B7F-4002-9B60-2B5DAA3EA30C
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 5013b35828cecc2e38600839f306f3c0fc1366b9
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/09/2018
---
# <a name="consuming-an-azure-cosmos-db-document-database"></a>Consumo de um banco de dados de documento de banco de dados do Cosmos do Azure

_Um banco de dados de documento de banco de dados do Azure Cosmos é um banco de dados NoSQL que fornece acesso de baixa latência para documentos JSON, oferecendo um serviço de banco de dados rápida, altamente disponível e dimensionável para aplicativos que necessitam de replicação global e escala contínua. Este artigo explica como usar a biblioteca de cliente de documentos do Microsoft Azure para integrar um banco de dados de documento de banco de dados do Azure Cosmos em um aplicativo xamarin. Forms._

> [!VIDEO https://youtube.com/embed/BoVH12igmbg]

**Microsoft Azure Cosmos banco de dados, por [University Xamarin](https://university.xamarin.com/)**

Uma conta de banco de dados de documento de banco de dados do Azure Cosmos pode ser provisionada usando uma assinatura do Azure. Cada conta de banco de dados pode ter zero ou mais bancos de dados. Um banco de dados de documento no banco de dados do Azure Cosmos é um contêiner lógico para os usuários e coleções de documento.

Um banco de dados de documento de banco de dados do Azure Cosmos pode conter zero ou mais coleções de documento. Cada coleção de documento pode ter um nível de desempenho diferente, permitindo que a taxa de transferência mais deve ser especificado para coleções acessadas com frequência e menos throughput para coleções acessadas com pouca frequência.

Cada coleção de documento consiste em zero ou mais documentos JSON. Documentos em uma coleção são independentes de esquema e portanto não precisam compartilhar a mesma estrutura ou campos. Como documentos são adicionados a uma coleção de documentos, Cosmos DB indexa automaticamente-los e elas se tornam disponíveis para serem consultados.

Para fins de desenvolvimento, um banco de dados de documento também pode ser consumido por meio de um emulador. Usando o emulador, aplicativos podem ser desenvolvidos e testados localmente, sem criar uma assinatura do Azure ou incorrer em todos os custos. Para obter mais informações sobre o emulador, consulte [desenvolvendo localmente com o emulador de banco de dados do Azure Cosmos](/azure/cosmos-db/local-emulator/).

Neste artigo e que acompanha o aplicativo de exemplo demonstra um aplicativo de lista de tarefas onde as tarefas são armazenadas em um banco de dados de documento de banco de dados do Azure Cosmos. Para obter mais informações sobre o aplicativo de exemplo, consulte [Noções básicas sobre o exemplo](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> Atualmente, a biblioteca de documentos do cliente não é compatível com aplicativos do Windows UWP (plataforma Universal). No entanto, um banco de dados de documento de banco de dados do Azure Cosmos pode ser usado em um aplicativo de UWP criando um serviço web de camada intermediária que usa a biblioteca de cliente de documentos e invocar esse serviço do aplicativo de UWP.

Para obter mais informações sobre o banco de dados do Azure Cosmos, consulte o [documentação de banco de dados do Azure Cosmos](/azure/cosmos-db/).

## <a name="setup"></a>Configuração

O processo para a integração de um banco de dados de documento de banco de dados do Azure Cosmos em um aplicativo xamarin. Forms é da seguinte maneira:

1. Crie uma conta de banco de dados do Cosmos. Para obter mais informações, consulte [criar uma conta de banco de dados do Cosmos](/azure/cosmos-db/documentdb-dotnetcore-get-started#step-1-create-a-documentdb-account).
1. Adicionar o [biblioteca de cliente de documentos](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core) pacote NuGet para os projetos de plataforma da solução xamarin. Forms.
1. Adicionar `using` diretivas para o `Microsoft.Azure.Documents`, `Microsoft.Azure.Documents.Client`, e `Microsoft.Azure.Documents.Linq` namespaces para classes que irá acessar a conta de banco de dados do Cosmos.

Depois de executar essas etapas, a biblioteca de cliente de documentos pode ser usada para configurar e executar solicitações no banco de dados de documento.

> [!NOTE]
> A biblioteca de cliente de documentos do Azure pode ser instalada apenas em projetos de plataforma e não em um projeto de biblioteca de classe portátil (PCL). Portanto, o aplicativo de exemplo é um projeto de acesso compartilhado (SAP) para evitar a duplicação de código. No entanto, a `DependencyService` classe pode ser usada em um projeto PCL para invocar o código de biblioteca de cliente de documentos do Azure contido em projetos de plataforma específica.

## <a name="consuming-the-azure-cosmos-db-account"></a>Consumindo a conta de banco de dados do Azure Cosmos

O `DocumentClient` tipo encapsula o ponto de extremidade, credenciais e política de conexão usada para acessar a conta de banco de dados do Azure Cosmos e é usado para configurar e executar solicitações em relação à conta. O exemplo de código a seguir demonstra como criar uma instância desta classe:

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

O Uri do Cosmos banco de dados e a chave primária devem ser fornecidas para o `DocumentClient` construtor. Eles podem ser obtidos do Portal do Azure. Para obter mais informações, consulte [conectar-se a uma conta de banco de dados do Azure Cosmos](/azure/cosmos-db/documentdb-dotnetcore-get-started#a-idconnectastep-3-connect-to-an-azure-cosmos-db-account).

### <a name="creating-a-database"></a>Criando um banco de dados

Um banco de dados de documento é um contêiner lógico para os usuários e coleções de documento e pode ser criado no Portal do Azure, ou programaticamente usando o `DocumentClient.CreateDatabaseIfNotExistsAsync` método:

```csharp
public async Task CreateDatabase(string databaseName)
{
  ...
  await client.CreateDatabaseIfNotExistsAsync(new Database
  {
    Id = databaseName
  });
  ...
}
```

O `CreateDatabaseIfNotExistsAsync` método Especifica um `Database` de objeto como um argumento, com o `Database` objeto que especifica o nome do banco de dados como seu `Id` propriedade. O `CreateDatabaseIfNotExistsAsync` método cria o banco de dados se ele não existe ou retorna o banco de dados se ele já existe. No entanto, o aplicativo de exemplo ignora quaisquer dados retornados pelo `CreateDatabaseIfNotExistsAsync` método.

> [!NOTE]
> O `CreateDatabaseIfNotExistsAsync` método retorna um `Task<ResourceResponse<Database>>` objeto e o código de status da resposta podem ser verificada para determinar se um banco de dados foi criado ou um banco de dados existente foi retornado.

### <a name="creating-a-document-collection"></a>Criando uma coleção de documentos

Uma coleção de documentos é um contêiner para documentos JSON e pode ser criado no Portal do Azure, ou programaticamente usando o `DocumentClient.CreateDocumentCollectionIfNotExistsAsync` método:

```csharp
public async Task CreateDocumentCollection(string databaseName, string collectionName)
{
  ...
  // Create collection with 400 RU/s
  await client.CreateDocumentCollectionIfNotExistsAsync(
    UriFactory.CreateDatabaseUri(databaseName),
    new DocumentCollection
    {
      Id = collectionName
    },
    new RequestOptions
    {
      OfferThroughput = 400
    });
  ...
}
```

O `CreateDocumentCollectionIfNotExistsAsync` método requer dois argumentos compulsórios – um nome de banco de dados especificado como um `Uri`e um `DocumentCollection` objeto. O `DocumentCollection` objeto representa uma coleção de documentos cujo nome é especificado com o `Id` propriedade. O `CreateDocumentCollectionIfNotExistsAsync` método cria o conjunto de documentos se ele não existe ou retorna o conjunto de documentos, se ele já existe. No entanto, o aplicativo de exemplo ignora quaisquer dados retornados pelo `CreateDocumentCollectionIfNotExistsAsync` método.

> [!NOTE]
> O `CreateDocumentCollectionIfNotExistsAsync` método retorna um `Task<ResourceResponse<DocumentCollection>>` objeto e o código de status da resposta podem ser verificada para determinar se uma coleção de documento foi criada, ou uma coleção de documento existente foi retornada.

Opcionalmente, o `CreateDocumentCollectionIfNotExistsAsync` método também pode especificar um `RequestOptions` objeto, que encapsula as opções que podem ser especificadas para solicitações emitidas para a conta de banco de dados do Cosmos. O `RequestOptions.OfferThroughput` propriedade é usada para definir o nível de desempenho da coleção de documento e o exemplo de aplicativo, é definido como 400 unidades de solicitação por segundo. Esse valor deve ser aumentado ou diminuído dependendo se a coleção será frequentemente ou raramente acessada.

> [!IMPORTANT]
> Observe que o `CreateDocumentCollectionIfNotExistsAsync` método criará uma nova coleção com uma taxa de transferência reservada, que tem implicações de preços.

<a name="document_query" />

### <a name="retrieving-document-collection-documents"></a>Recuperar a coleção de documentos

O conteúdo de uma coleção de documentos pode ser recuperado criando e executando uma consulta de documento. Uma consulta de documento é criada com o `DocumentClient.CreateDocumentQuery` método:

```csharp
public async Task<List<TodoItem>> GetTodoItemsAsync()
{
  ...
  var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
            .AsDocumentQuery();
  while (query.HasMoreResults)
  {
    Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
  }
  ...
}
```

Essa consulta recupera todos os documentos da coleção especificada e de forma assíncrona coloca os documentos em um `List<TodoItem>` coleção para exibição.

O `CreateDocumentQuery<T>` método Especifica um `Uri` argumento que representa a coleção deve ser consultada para documentos. Neste exemplo, o `collectionLink` variável é um campo de nível de classe que especifica o `Uri` que representa a coleção de documento para recuperar documentos de:

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

O `CreateDocumentQuery<T>` método cria uma consulta que é executada de forma síncrona e retorna um `IQueryable<T>` objeto. No entanto, o `AsDocumentQuery` método converte o `IQueryable<T>` o objeto para um `IDocumentQuery<T>` objeto que pode ser executado de forma assíncrona. A consulta assíncrona é executada com o `IDocumentQuery<T>.ExecuteNextAsync` método, que recupera a próxima página de resultados do banco de dados de documento, com o `IDocumentQuery<T>.HasMoreResults` propriedade que indica se há resultados adicionais a serem retornados da consulta.

Os documentos podem ser filtrados do lado do servidor, incluindo um `Where` cláusula na consulta, que aplica um predicado de filtragem para a consulta em relação à coleção de documento:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

Essa consulta recupera todos os documentos da coleção cuja `Done` propriedade é igual a `false`.

<a name="inserting_document" />

### <a name="inserting-a-document-into-a-document-collection"></a>Inserindo um documento em uma coleção de documentos

Documentos são o conteúdo JSON definidos pelo usuário e podem ser inseridos em uma coleção de documentos com o `DocumentClient.CreateDocumentAsync` método:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

O `CreateDocumentAsync` método Especifica um `Uri` argumento que representa a coleção de documento deve ser inserido, e um `object` argumento que representa o documento a ser inserido.

### <a name="replacing-a-document-in-a-document-collection"></a>Substituindo um documento em uma coleção de documentos

Documentos podem ser substituídos em uma coleção de documento com o `DocumentClient.ReplaceDocumentAsync` método:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

O `ReplaceDocumentAsync` método Especifica um `Uri` argumento que representa o documento na coleção que deve ser substituído, e um `object` argumento que representa os dados de documento atualizado.

<a name="deleting_document" />

### <a name="deleting-a-document-from-a-document-collection"></a>Exclusão de um documento de um conjunto de documentos

Um documento pode ser excluído de uma coleção de documento com o `DocumentClient.DeleteDocumentAsync` método:

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

O `DeleteDocumentAsync` método Especifica um `Uri` argumento que representa o documento na coleção que deve ser excluído.

### <a name="deleting-a-document-collection"></a>Excluir um conjunto de documentos

Uma coleção de documentos pode ser excluída do banco de dados com o `DocumentClient.DeleteDocumentCollectionAsync` método:

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

O `DeleteDocumentCollectionAsync` método Especifica um `Uri` argumento que representa a coleção de documento a ser excluído. Observe que a invocar esse método também excluirá os documentos armazenados na coleção.

### <a name="deleting-a-database"></a>Excluir um banco de dados

Um banco de dados pode ser excluído de uma conta de banco de dados do banco de dados do Cosmos com o `DocumentClient.DeleteDatabaesAsync` método:

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

O `DeleteDatabaseAsync` método Especifica um `Uri` argumento que representa o banco de dados a ser excluído. Observe que invocar esse método também excluirá as coleções de documento armazenadas no banco de dados e os documentos armazenados nas coleções de documento.

## <a name="summary"></a>Resumo

Este artigo explicou como usar a biblioteca de cliente de documentos do Microsoft Azure para integrar um banco de dados de documento de banco de dados do Azure Cosmos em um aplicativo xamarin. Forms. Um banco de dados de documento de banco de dados do Azure Cosmos é um banco de dados NoSQL que fornece acesso de baixa latência para documentos JSON, oferecendo um serviço de banco de dados rápida, altamente disponível e dimensionável para aplicativos que necessitam de replicação global e escala contínua.


## <a name="related-links"></a>Links relacionados

- [TodoDocumentDB (exemplo)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDB/)
- [Documentação do cosmos DB](/azure/cosmos-db/)
- [Biblioteca de cliente de documentos](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [API do Azure Cosmos DB](https://msdn.microsoft.com/library/azure/dn948556.aspx)