---
title: "Parte 1 – Criando uma MonoGame de plataforma cruzada"
description: "Este passo a passo mostra como criar um novo projeto para iOS e Android usando MonoGame. O resultado é um Visual Studio para solução de Mac com um projeto de código compartilhado da plataforma cruzada, bem como um projeto para cada plataforma. Este projeto exibirá uma tela azul vazia quando executado."
ms.topic: article
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 0d1352b4129dc1cf8be42e813787b9b73f80cd3e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>Parte 1 – Criando uma MonoGame de plataforma cruzada

_Este passo a passo mostra como criar um novo projeto para iOS e Android usando MonoGame. O resultado é um Visual Studio para solução de Mac com um projeto de código compartilhado da plataforma cruzada, bem como um projeto para cada plataforma. Este projeto exibirá uma tela azul vazia quando executado._

MonoGame permite o desenvolvimento de jogos de plataforma cruzada com grande parte da reutilização de código. Este passo a passo se concentrará na configuração de uma solução que contém projetos para iOS e Android, bem como um projeto de código compartilhado para o código de plataforma cruzada.

Quando terminamos, vamos ter um projeto que tem a estrutura apropriada para a execução de lógica de atualização de jogo e lógica de desenho de 30 quadros por segundo do jogo. Ele pode ser usado como o projeto de base para qualquer projeto MonoGame. Nosso projeto será assim quando executado:

![](part1-images/image1.png "O projeto terá esta aparência quando executado")


# <a name="adding-monogame-to-visual-studio-for-mac"></a>Adicionando MonoGame para o Visual Studio para Mac

MonoGame pode ser adicionado como um suplemento para o Visual Studio para Mac. No Mac, selecione **Visual Studio para Mac** > **Gerenciador de suplementos...**  . No Windows, ferramentas de select * * * * > **Gerenciador de suplementos...**  . Selecione o **galeria** guia, expanda o **desenvolvimento de jogos** categoria e selecione **MonoGame Addin**, em seguida, clique em **instalar**:

![](part1-images/image2.png "Selecione a guia galeria, expanda a categoria de desenvolvimento de jogos e selecione MonoGame suplemento e clique em instalar")

> [!IMPORTANT]
> **Observação**: se o **desenvolvimento de jogos** seção não aparece no Gerenciador de suplementos, manualmente, você pode baixar e instalar a versão mais recente aqui: http://www.monogame.net/downloads/. Talvez seja necessário reiniciar o Visual Studio para Mac para que os modelos sejam exibidos.



Uma vez instalado, MonoGame modelos aparecerão no Visual Studio para Mac, conforme veremos na próxima seção.


# <a name="creating-a-new-solution"></a>Criar uma nova solução

No Visual Studio para selecionar Mac **arquivo > nova solução**. No **novo projeto** caixa de diálogo, clique em **diversos**, role até a **geral** seção, selecione o * * aplicativo Universal Mobile de MonoGame * * opção e clique em Avançar.

![](part1-images/image3.png "Na caixa de diálogo Novo projeto, clique em diversos, role até a seção geral, selecione a opção do aplicativo móvel do Universal MonoGame e clique em Avançar")

Nomeie o projeto WalkingGame e clique em criar:

![](part1-images/image4.png "Nomeie o projeto WalkingGame e clique em criar")

Agora, nosso projeto será executado assim como qualquer outra iOS ou Android projeto. O projeto deve ser executado exibindo um plano de fundo espinheiro azul:

![](part1-images/image5.png "O projeto deve ser executado exibindo um plano de fundo espinheiro azul")


# <a name="fixing-android-compile-errors"></a>Corrigindo erros de compilação Android

A versão atual dos modelos do MonoGame inclui alguns erros de sintaxe em do Android `Activity1.cs` arquivo. Para corrigir esses problemas, substitua o `OnCreate` função com o seguinte:


```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```


# <a name="summary"></a>Resumo

Este passo a passo abordou como criar um projeto de MonoGame de plataforma cruzada usando o Visual Studio para Mac. O resultado é uma tela azul vazia. Este projeto pode ser usado como o ponto de partida para qualquer jogo Android e iOS.

## <a name="related-links"></a>Links relacionados

- [NuGet MonoGame Android](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [IOS MonoGame NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [Documentação da API do MonoGame](http://www.monogame.net/documentation/?page=main)