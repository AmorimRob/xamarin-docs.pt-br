---
title: Conectar-se aos Serviços Web Locais em simuladores do iOS e emuladores do Android
description: Este artigo explica como um aplicativo móvel do Xamarin em execução no simulador do iOS ou emulador do Android pode consumir um serviço Web do ASP.NET Core que está sendo executado localmente.
ms.prod: xamarin
ms.assetid: FD8FE199-898B-4841-8041-CC9CA1A00917
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2019
ms.openlocfilehash: bc88a5eb977ea49b761df22407329dfaf20fa122
ms.sourcegitcommit: 086edd9c44dfc0e77412e1ed5eda7318bbd1ce7c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58477565"
---
# <a name="connect-to-local-web-services-from-ios-simulators-and-android-emulators"></a>Conectar-se aos Serviços Web Locais em simuladores do iOS e emuladores do Android

[![Baixar Exemplo](~/media/shared/download.png) Baixar o exemplo](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST)

Muitos aplicativos móveis consomem serviços Web. Durante a fase de desenvolvimento, é comum implantar um serviço Web localmente e consumi-lo de um aplicativo móvel em execução no simulador do iOS ou no emulador do Android. Isso evita a necessidade de implantar o serviço Web em um ponto de extremidade hospedado e possibilita uma experiência de depuração simples, pois o aplicativo móvel e o serviço Web estão sendo executados localmente.

Os aplicativos móveis em execução no simulador do iOS ou no emulador do Android podem consumir serviços Web do ASP.NET Core que estão sendo executados localmente e serem expostos via HTTP, conforme a seguir:

- Os aplicativos em execução no simulador do iOS podem se conectar aos serviços web HTTP locais por meio do endereço IP do computador ou do nome do host `localhost`. Por exemplo, considerando um serviço Web HTTP local que expõe uma operação GET por meio do Uniform Resource Identifier (URI) relativo `/api/todoitems/`, o aplicativo em execução no simulador do iOS pode consumir a operação enviando uma solicitação GET ao `http://localhost:<port>/api/todoitems/`.
- Os aplicativos em execução no emulador do Android podem se conectar aos serviços Web HTTP locais por meio do endereço `10.0.2.2`, que é um alias para a sua interface de loopback do host (`127.0.0.1` no computador de desenvolvimento). Por exemplo, considerando um serviço Web HTTP local que expõe uma operação GET por meio do URI relativo `/api/todoitems/`, o aplicativo em execução no emulador do Android pode consumir a operação enviando uma solicitação GET ao `http://10.0.2.2:<port>/api/todoitems/`.

No entanto, é necessário um trabalho adicional para que o aplicativo em execução no simulador do iOS ou no emulador do Android possam consumir um serviço Web local que esteja exposto via HTTPS. Para este cenário, o processo é da seguinte maneira:

1. Crie um certificado autoassinado de desenvolvimento em seu computador. Para saber mais, confira as informações sobre como [criar um certificado de desenvolvimento](#create-a-development-certificate).
1. Configure o projeto para usar a pilha de rede `HttpClient` gerenciada para o build de depuração. Para saber mais, confira as informações sobre como [configurar o projeto](#configure-your-project).
1. Especifique o endereço do computador local. Para saber mais, confira as informações sobre como [especificar o endereço do computador local](#specify-the-local-machine-address).
1. Ignore a verificação de segurança do certificado de desenvolvimento local. Para saber mais, veja [Ignorar a verificação de segurança do certificado](#bypass-the-certificate-security-check).

Cada item será apresentado separadamente.

## <a name="create-a-development-certificate"></a>Criar um certificado de desenvolvimento

Instalar o SDK do .NET Core instala o certificado de desenvolvimento HTTPS do ASP.NET Core no repositório de certificados do usuário local. No entanto, embora o certificado tenha sido instalado, ele não é confiável. Para confiar no certificado, realize a única etapa a seguir para executar a ferramenta do dotnet `dev-certs`:

```console
dotnet dev-certs https --trust
```

O comando a seguir fornece ajuda para a ferramenta `dev-certs`:

```console
dotnet dev-certs https --help
```

Como alternativa, quando você executa um projeto do ASP.NET Core 2.1 (ou superior), que usa HTTPS, o Visual Studio detectará se o certificado de desenvolvimento está ausente e oferecerá a possibilidade de instalá-lo e de confiar nele.

> [!NOTE]
> O certificado de desenvolvimento HTTPS do ASP.NET Core é autoassinado.

Para saber mais sobre como habilitar o HTTPS local em seu computador, confira o artigo [Habilitar o HTTPS local](/aspnet/core/getting-started#enable-local-https).

## <a name="configure-your-project"></a>Configurar seu projeto

Os aplicativos do Xamarin em execução no iOS e no Android podem especificar qual pilha de rede é usada pela classe `HttpClient`; sendo as opções uma pilha de rede gerenciada ou pilhas de rede nativa. A pilha gerenciada fornece um alto nível de compatibilidade com o código existente do .NET, mas é limitada a TLS 1.0 e pode ser mais lenta e resultar em um executável de tamanho maior. As pilhas nativas podem ser mais rápidas e oferecem uma segurança melhor, mas podem não fornecer toda a funcionalidade da classe `HttpClient`.

### <a name="ios"></a>iOS

Os aplicativos do Xamarin em execução no iOS podem usar a pilha de rede gerenciada ou as pilhas de rede nativa `CFNetwork` ou `NSUrlSession`. Por padrão, a nova plataforma de projetos do iOS usa a pilha de rede `NSUrlSession`, para dar suporte ao TLS 1.2, e usa as APIs nativas para um desempenho melhor e um executável de tamanho menor.

No entanto, quando o aplicativo precisa se conectar a um serviço Web seguro que é executado localmente, para testes de desenvolvimento, é mais fácil usar a pilha de rede gerenciada. Portanto, é recomendável definir perfis de compilação do simulador de depuração para usar a pilha de rede gerenciada, e perfis de compilação de versão para usar a pilha de rede `NSUrlSession`. Cada pilha de rede pode ser definida de forma programática ou por meio de um seletor nas opções do projeto. Para saber mais, confira [Seletor de implementação de HttpClient e SSL/TLS para iOS/macOS](~/cross-platform/macios/http-stack.md).

### <a name="android"></a>Android

Os aplicativos do Xamarin em execução no Android podem usar a pilha de rede gerenciada `HttpClientHandler` ou a pilha de rede nativa `AndroidClientHandler`. Por padrão, a nova plataforma de projetos do Android usa a pilha de rede `AndroidClientHandler`, para dar suporte ao TLS 1.2, e usa as APIs nativas para um desempenho melhor e um executável de tamanho menor.

No entanto, quando o aplicativo precisa se conectar a um serviço Web seguro que é executado localmente, para testes de desenvolvimento, é mais fácil usar a pilha de rede gerenciada. Portanto, é recomendável definir perfis de compilação do emulador de depuração para usar a pilha de rede gerenciada, e perfis de compilação de versão para usar a pilha de rede nativa. Cada pilha de rede pode ser definida de forma programática ou por meio de um seletor nas opções do projeto. Para saber mais, confira [Seletor de implementação de SSL/TLS e da pilha HttpClient para o Android](~/android/app-fundamentals/http-stack.md).

## <a name="specify-the-local-machine-address"></a>Especificar o endereço do computador local

O simulador do iOS e o emulador do Android fornecem acesso para proteger os serviços Web em execução no computador local. No entanto, o endereço do computador local é diferente para cada um deles.

### <a name="ios"></a>iOS

O simulador do iOS usa a rede do computador host. Portanto, os aplicativos em execução no simulador podem se conectar aos serviços Web em execução no computador local por meio do endereço IP dos computadores ou por meio do nome do host `localhost`. Por exemplo, considerando um serviço Web local seguro que expõe uma operação GET por meio do URI relativo `/api/todoitems/`, o aplicativo em execução no simulador do iOS pode consumir a operação enviando uma solicitação GET ao `https://localhost:<port>/api/todoitems/`.

> [!NOTE]
> Ao executar um aplicativo móvel no simulador do iOS a partir do Windows, o aplicativo é exibido no [simulador do iOS remoto para Windows](~/tools/ios-simulator/index.md). No entanto, o aplicativo está sendo executado no Mac emparelhado. Portanto, não há nenhum acesso de localhost a um serviço Web em execução no Windows para um aplicativo iOS em execução no Mac.

### <a name="android"></a>Android

Cada instância do emulador do Android é isolada das interfaces de rede de seu computador de desenvolvimento e é executado atrás de um roteador virtual. Portanto, um dispositivo emulado não pode ver seu computador de desenvolvimento ou outras instâncias do emulador na rede.

No entanto, o roteador virtual para cada emulador gerencia um espaço de rede especial que inclui os endereços alocados previamente, sendo que o endereço `10.0.2.2` é um alias para a sua interface host de loopback (127.0.0.1 em seu computador de desenvolvimento). Portanto, considerando um serviço Web local seguro que expõe uma operação GET por meio do URI relativo `/api/todoitems/`, o aplicativo em execução no emulador do Android pode consumir a operação enviando uma solicitação GET ao `https://10.0.2.2:<port>/api/todoitems/`.

### <a name="xamarinforms-example"></a>Exemplo do Xamarin.Forms

Em um aplicativo Xamarin.Forms, a classe [`Device`](xref:Xamarin.Forms.Device) pode ser usada para detectar a plataforma em que o aplicativo está sendo executado. O nome do host apropriado, que permite o acesso a serviços Web locais seguros, pode ser definido da seguinte maneira:

```csharp
public static string BaseAddress =
    Device.RuntimePlatform == Device.Android ? "https://10.0.2.2:5001" : "https://localhost:5001";
public static string TodoItemsUrl = $"{BaseAddress}/api/todoitems/";
```

## <a name="bypass-the-certificate-security-check"></a>Ignorar a verificação de segurança do certificado

A tentativa de invocar um serviço Web local seguro de um aplicativo em execução no simulador do iOS ou no emulador Android resultará em uma `HttpRequestException` sendo acionada, mesmo ao usar a pilha de rede gerenciada em cada plataforma. Isso ocorre porque o certificado de desenvolvimento HTTPS local é autoassinado, e certificados autoassinados não são confiados pelo iOS nem pelo Android.

Portanto, é necessário ignorar os erros de SSL quando um aplicativo consome um serviço Web local seguro. Isso pode ser feito ao usar a pilha de rede gerenciada, definindo a propriedade `ServicePointManager.ServerCertificateValidationCallback` para um retorno de chamada que ignore o resultado da verificação de segurança do certificado, para o certificado de desenvolvimento HTTPS local:

```csharp
#if DEBUG
    System.Net.ServicePointManager.ServerCertificateValidationCallback += (sender, certificate, chain, sslPolicyErrors) =>
    {
        if (certificate.Issuer.Equals("CN=localhost"))
            return true;
        return sslPolicyErrors == System.Net.Security.SslPolicyErrors.None;
    };
#endif
```

Nesse exemplo de código, o resultado de validação de certificado do servidor é retornado quando o certificado que passar por validação não for o certificado `localhost`. Para esse certificado, o resultado da validação é ignorado e `true` é retornado, indicando que o certificado é válido. Esse código deve ser adicionado ao método `AppDelegate.FinishedLaunching` no iOS e ao método `MainActivity.OnCreate` no Android, antes da chamada de método `LoadApplication(new App())`.

> [!NOTE]
> As pilhas de rede nativa no iOS e no Android não se conectam ao `ServerCertificateValidationCallback`.

## <a name="related-links"></a>Links relacionados

- [TodoREST (amostra)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST)
- [Habilitar HTTPS local](/aspnet/core/getting-started#enable-local-https)
- [Seletor de implementação de HttpClient e SSL/TLS para iOS/macOS](~/cross-platform/macios/http-stack.md)
- [Seletor de implementação de SSL/TLS e da pilha HttpClient para Android](~/android/app-fundamentals/http-stack.md)