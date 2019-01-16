---
title: Indexação de aplicativo e vinculação profunda
description: Este artigo explica como usar a indexação de aplicativo e a vinculação profunda para fazer com que o Xamarin.Forms tenha conteúdo pesquisável em dispositivos iOS e Android.
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/28/2018
ms.openlocfilehash: c4e634ce51080ad38b093e1355767c73c72e837a
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54208052"
---
# <a name="application-indexing-and-deep-linking"></a>Indexação de aplicativo e vinculação profunda

[![Baixar Exemplo](~/media/shared/download.png) Baixar o exemplo](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)

_A indexação de aplicativo permite que os aplicativos que seriam esquecidos após alguns usos permaneçam relevantes fazendo com que eles apareçam nos resultados da pesquisa. A vinculação profunda permite que os aplicativos respondam a um resultado de pesquisa que contém dados de aplicativo, normalmente navegando até uma página referenciada de um link profundo. Este artigo explica como usar a indexação de aplicativo e a vinculação profunda para fazer com que o Xamarin.Forms tenha conteúdo pesquisável em dispositivos iOS e Android._

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**Vinculação profunda com o Xamarin.Forms e o Azure, por [Xamarin University](https://university.xamarin.com/)**


A indexação de aplicativo e a vinculação profunda do Xamarin.Forms fornecem uma API para publicar metadados para indexação de aplicativo à medida que os usuários navegam pelos aplicativos. O conteúdo indexado pode ser pesquisado no Spotlight Search, na Pesquisa Google ou em uma pesquisa na Web. Tocar em um resultado de pesquisa que contém um link profundo acionará um evento que poderá ser tratado por um aplicativo. Isso normalmente é usado para navegar até a página referenciada do link profundo.

Este aplicativo de exemplo demonstra um aplicativo de Lista de tarefas em que os dados são armazenados em um banco de dados local do SQLite, como mostrado nas seguintes capturas de tela:

![](deep-linking-images/screenshots.png "Aplicativo TodoList")

Cada instância `TodoItem` criada pelo usuário é indexada. Então, a pesquisa específica da plataforma pode ser usada para localizar dados indexados do aplicativo. Quando o usuário toca em um item de resultado de pesquisa para o aplicativo, o aplicativo é iniciado, o `TodoItemPage` é navegada e o `TodoItem` referenciado do link profundo é exibido.

Para obter mais informações sobre como usar um banco de dados do SQLite, consulte [Bancos de Dados Locais do Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md).

> [!NOTE]
> A funcionalidade de indexação de aplicativo e vinculação profunda do Xamarin.Forms somente está disponível nas plataformas Android e iOS e requer um mínimo de iOS 9 e API 23, respectivamente.

## <a name="setup"></a>Configuração

As seções a seguir fornecem instruções de configuração adicionais para usar esse recurso nas plataformas Android e iOS.

### <a name="ios"></a>iOS

Na plataforma iOS, certifique-se de que seu projeto da plataforma iOS defina o arquivo **Entitlements.plist** como o arquivo de direitos personalizados para assinar o pacote.

Para usar Links Universais do iOS:

1. Adicione um direito de Domínios Associados ao seu aplicativo, com a chave `applinks`, incluindo todos os domínios que seu aplicativo dará suporte.
1. Adicione um arquivo de Associação de Site do Aplicativo da Apple ao seu site.
1. Adicione a chave `applinks` ao arquivo de Associação de Site do Aplicativo da Apple.

Para obter mais informações, consulte [Permitir que aplicativos e sites se vinculem ao seu conteúdo](https://developer.apple.com/documentation/uikit/core_app/allowing_apps_and_websites_to_link_to_your_content) em developer.apple.com.

### <a name="android"></a>Android

Na plataforma Android, há uma série de pré-requisitos que devem ser atendidos para usar a funcionalidade de indexação de aplicativo e vinculação profunda:

1. Uma versão do seu aplicativo deve estar em tempo real no Google Play.
1. Um site complementar deve ser registrado com o aplicativo no Console do Desenvolvedor do Google. Depois que o aplicativo é associado a um site, as URLs podem ser indexadas e funcionam para o site e o aplicativo, que pode ser atendido nos resultados da pesquisa. Para obter mais informações, consulte [Indexação de apps na Pesquisa Google](https://support.google.com/googleplay/android-developer/answer/6041489) no site do Google.
1. Seu aplicativo deve dar suporte a intenções de URL HTTP na classe `MainActivity`, que informa à indexação de aplicativo quais tipos de esquemas de dados de URL o aplicativo pode responder. Para obter mais informações, consulte [Configurar o Filtro de Intenção](~/android/platform/app-linking.md#configure-intent-filter).

Depois que esses pré-requisitos forem atendidos, a seguinte configuração adicional será necessária para usar a indexação de aplicativo e a vinculação profunda do Xamarin.Forms na plataforma Android:

1. Instale o pacote do NuGet do [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) no projeto de aplicativo do Android.
1. No arquivo **MainActivity.cs**, adicione uma declaração para usar o namespace `Xamarin.Forms.Platform.Android.AppLinks`.
1. No arquivo **MainActivity.cs**, adicione uma declaração para usar o namespace `Firebase`.
1. Em um navegador da Web, crie um projeto por meio do [Console do Firebase](https://console.firebase.google.com/).
1. No Console do Firebase, adicione o Firebase ao seu aplicativo Android e insira os dados necessários.
1. Baixe o arquivo **google-services.json** resultante.
1. Adicione o arquivo **google-services.json** ao diretório raiz do projeto do Android e defina sua **Ação de Build** como **GoogleServicesJson**.
1. Na substituição de `MainActivity.OnCreate`, adicione a seguinte linha de código abaixo de `Forms.Init(this, bundle)`:

```csharp
FirebaseApp.InitializeApp(this);
AndroidAppLinks.Init(this);
```

Quando **google-services.json** é adicionado ao projeto (e a ação de build *GoogleServicesJson** é definida), o processo de build extrai a chave de API e a ID do cliente e, em seguida, adiciona essas credenciais ao arquivo de manifesto gerado.

Para obter mais informações, consulte [Conteúdo de link profundo com navegação de URL do Xamarin.Forms](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) no blog do Xamarin.

## <a name="indexing-a-page"></a>Indexação de uma página

O processo de indexação e exposição de uma página na pesquisa do Google e do Spotlight é da seguinte maneira:

1. Crie um [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) que contenha os metadados necessários para indexar a página juntamente com um link profundo para retornar à página quando o usuário selecionar o conteúdo indexado nos resultados da pesquisa.
1. Registre a instância [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) para indexá-la para a pesquisa.

O exemplo de código a seguir demonstra como criar uma instância [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry):

```csharp
AppLinkEntry GetAppLink(TodoItem item)
{
    var pageType = GetType().ToString();
    var pageLink = new AppLinkEntry
    {
        Title = item.Name,
        Description = item.Notes,
        AppLinkUri = new Uri($"http://{App.AppName}/{pageType}?id={item.ID}", UriKind.RelativeOrAbsolute),
        IsLinkActive = true,
        Thumbnail = ImageSource.FromFile("monkey.png")
    };

    pageLink.KeyValues.Add("contentType", "TodoItemPage");
    pageLink.KeyValues.Add("appName", App.AppName);
    pageLink.KeyValues.Add("companyName", "Xamarin");

    return pageLink;
}
```

A instância [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) contém uma série de propriedades cujos valores são necessários para indexar a página e criar um link profundo. As propriedades [`Title`](xref:Xamarin.Forms.IAppLinkEntry.Title), [`Description`](xref:Xamarin.Forms.IAppLinkEntry.Description) e [`Thumbnail`](xref:Xamarin.Forms.IAppLinkEntry.Thumbnail) são usadas para identificar o conteúdo indexado quando ele for exibido nos resultados da pesquisa. A propriedade [`IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) é definida como `true` para indicar que o conteúdo indexado está sendo exibido atualmente. A propriedade [`AppLinkUri`](xref:Xamarin.Forms.IAppLinkEntry.AppLinkUri) é uma `Uri` que contém as informações necessárias para retornar à página atual e exibir o `TodoItem` atual. O exemplo a seguir mostra um `Uri` de exemplo para o aplicativo de exemplo:

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=2
```

Esse `Uri` contém todas as informações necessárias para iniciar o aplicativo `deeplinking`. Navegue até `DeepLinking.TodoItemPage`e exiba o `TodoItem` que tem um `ID` de 2.

## <a name="registering-content-for-indexing"></a>Registro do conteúdo para indexação

Assim que a instância [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) tiver sido criada, ela deverá ser registrada para a indexação aparecer nos resultados da pesquisa. Isso é feito com o método [`RegisterLink`](xref:Xamarin.Forms.IAppLinks.RegisterLink(Xamarin.Forms.IAppLinkEntry)), conforme demonstrado no exemplo de código a seguir:

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

Isso adiciona a instância [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) à coleção [`AppLinks`](xref:Xamarin.Forms.Application.AppLinks) do aplicativo.

> [!NOTE]
> O método `RegisterLink` também pode ser usado para atualizar o conteúdo que foi indexado para uma página.

Assim que uma instância [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) tiver sido registrada para indexação, ela poderá aparecer nos resultados da pesquisa. A captura de tela a seguir mostra o conteúdo indexado que aparece nos resultados da pesquisa na plataforma iOS:

![](deep-linking-images/ios-search.png "Conteúdo indexado nos resultados da pesquisa no iOS")

## <a name="de-registering-indexed-content"></a>Cancelar o registro do conteúdo indexado

O método [`DeregisterLink`](xref:Xamarin.Forms.IAppLinks.DeregisterLink(Xamarin.Forms.IAppLinkEntry)) é usado para remover o conteúdo indexado dos resultados da pesquisa, conforme demonstrado no exemplo de código a seguir:

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

Isso remove a instância [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) da coleção [`AppLinks`](xref:Xamarin.Forms.Application.AppLinks) do aplicativo.

> [!NOTE]
> No Android, não é possível remover o conteúdo indexado dos resultados da pesquisa.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>Responder a um Link Profundo

Quando o conteúdo indexado for exibido nos resultados da pesquisa e for selecionado por um usuário, a classe `App` para o aplicativo receberá uma solicitação para lidar com o `Uri` contido no conteúdo indexado. Essa solicitação pode ser processada na substituição de [`OnAppLinkRequestReceived`](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)), conforme demonstrado no exemplo de código a seguir:

```csharp
public class App : Application
{
    ...
    protected override async void OnAppLinkRequestReceived(Uri uri)
    {
        string appDomain = "http://" + App.AppName.ToLowerInvariant() + "/";
        if (!uri.ToString().ToLowerInvariant().StartsWith(appDomain, StringComparison.Ordinal))
            return;

        string pageUrl = uri.ToString().Replace(appDomain, string.Empty).Trim();
        var parts = pageUrl.Split('?');
        string page = parts[0];
        string pageParameter = parts[1].Replace("id=", string.Empty);

        var formsPage = Activator.CreateInstance(Type.GetType(page));
        var todoItemPage = formsPage as TodoItemPage;
        if (todoItemPage != null)
        {
            var todoItem = await App.Database.GetItemAsync(int.Parse(pageParameter));
            todoItemPage.BindingContext = todoItem;
            await MainPage.Navigation.PushAsync(formsPage as Page);
        }
        base.OnAppLinkRequestReceived(uri);
    }
}
```

O método [`OnAppLinkRequestReceived`](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) verifica se o `Uri` recebido destina-se ao aplicativo antes da análise de `Uri` em uma página que será navegada e do parâmetro que será passado para a página. Uma instância da página que será navegada é criada e o `TodoItem` representado pelo parâmetro da página é recuperado. O [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) da página que será navegada é definido como `TodoItem`. Isso garante que, quando o `TodoItemPage` for exibido pelo método [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page)), ele estará mostrando o `TodoItem` cujo `ID` está contido no link profundo.

## <a name="making-content-available-for-search-indexing"></a>Disponibilizar o conteúdo disponível para indexação de pesquisa

Sempre que a página representada por um link profundo for exibida, a propriedade [`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) poderá ser definida como `true`. No iOS e no Android, isso torna a instância [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) disponível para a indexação de pesquisa. No iOS, isso também torna a instância `AppLinkEntry` disponível para entrega. Para obter mais informações sobre a entrega, consulte [Introdução à entrega](~/ios/platform/handoff.md).

O exemplo de código a seguir demonstra a configuração da propriedade [`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) como `true` na substituição de [`Page.OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing):

```csharp
protected override void OnAppearing()
{
    appLink = GetAppLink(BindingContext as TodoItem);
    if (appLink != null)
    {
        appLink.IsLinkActive = true;
    }
}
```

Da mesma forma, quando a página representada por um link profundo for retirada da navegação, a propriedade [`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) poderá ser definida como `false`. No iOS e no Android, isso interrompe a instância [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) que está sendo anunciada para indexação de pesquisa. No iOS, isso também interrompe o anúncio da instância `AppLinkEntry` para entrega. Isso pode ser feito na substituição de [`Page.OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing), conforme demonstrado no exemplo de código a seguir:

```csharp
protected override void OnDisappearing()
{
    if (appLink != null)
    {
        appLink.IsLinkActive = false;
    }
}
```

## <a name="providing-data-to-handoff"></a>Fornecer dados para entrega

No iOS, os dados específicos do aplicativo podem ser armazenados durante a indexação de página. Isso é feito pela adição de dados para à coleção [`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues), que é um `Dictionary<string, string>` para armazenar pares chave-valor que são usados na entrega. A entrega é uma maneira para o usuário iniciar uma atividade em um dos seus dispositivos e continuar essa atividade em outro de seus dispositivos (conforme identificado pela conta do iCloud do usuário). O código a seguir mostra um exemplo de armazenamento de pares chave-valor específicos do aplicativo:

```csharp
var pageLink = new AppLinkEntry
{
    ...
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

Os valores armazenados na coleção [`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) serão armazenados nos metadados para a página indexada e serão restaurados quando o usuário tocar em um resultado de pesquisa que contém um link profundo (ou quando a entrega for usada para exibir o conteúdo em outro dispositivo conectado).

Além disso, os valores para as seguintes chaves podem ser especificados:

- `contentType` – um `string` que especifica o identificador de tipo uniforme do conteúdo indexado. A convenção recomendada para uso para esse valor é o nome do tipo da página com o conteúdo indexado.
- `associatedWebPage` – um `string` que representa a página da Web a ser visitada se o conteúdo indexado também puder ser exibido na Web ou se o aplicativo der suporte a links profundos do Safari.
- `shouldAddToPublicIndex` – um `string` de `true` ou `false` que controla a necessidade de adição de conteúdo indexado ao índice de nuvem pública da Apple, que, em seguida, pode ser apresentado aos usuários que ainda não instalaram o aplicativo em seu dispositivo iOS. No entanto, o conteúdo ter sido definido para indexação pública não significa que ele será automaticamente adicionado ao índice de nuvem pública da Apple. Para obter mais informações, consulte [Indexação de pesquisa pública](~/ios/platform/search/nsuseractivity.md). Observe que essa chave deve ser definida como `false` ao adicionar dados pessoais à coleção [`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues).

> [!NOTE]
> A coleção `KeyValues` não é usada na plataforma Android.

Para obter mais informações sobre a entrega, consulte [Introdução à entrega](~/ios/platform/handoff.md).

## <a name="summary"></a>Resumo

Este artigo explicou como usar a indexação de aplicativo e a vinculação profunda para tornar o conteúdo do aplicativo Xamarin.Forms pesquisável em dispositivos iOS e Android. A indexação de aplicativo permite que os aplicativos que seriam esquecidos após alguns usos permaneçam relevantes fazendo com que eles apareçam nos resultados da pesquisa.

## <a name="related-links"></a>Links relacionados

- [Vinculação profunda (amostra)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [APIs de pesquisa do iOS](~/ios/platform/search/index.md)
- [Vinculação de aplicativo no Android 6.0](~/android/platform/app-linking.md)
- [AppLinkEntry](xref:Xamarin.Forms.AppLinkEntry)
- [IAppLinkEntry](xref:Xamarin.Forms.IAppLinkEntry)
- [IAppLinks](xref:Xamarin.Forms.IAppLinks)