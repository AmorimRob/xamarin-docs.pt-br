---
title: "Ícone de loja de aplicativos"
description: "Cobre este artigo incluindo e gerenciando um ativo de imagem em um aplicativo xamarin a ser usado como um ícone de armazenamento do aplicativo."
ms.topic: article
ms.prod: xamarin
ms.assetid: BFB5665A-F557-46E1-B35E-870CC2026AD9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/26/2017
ms.openlocfilehash: 6ec4328f08859c5331a6250bf44ee705a7fd0744
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="app-store-icon"></a>Ícone de loja de aplicativos

_Cobre este artigo incluindo e gerenciando um ativo de imagem em um aplicativo xamarin a ser usado como um ícone de armazenamento do aplicativo._

Antes de 9 Xcode todos os ícones de loja de aplicativos foram adicionados por meio de conectar-se de iTunes. No entanto, isso não é mais o caso. Ícones da App Store agora devem ser incluídos como parte do seu pacote do projeto e adicionadas em um catálogo de ativos. Aplicativos que não contêm um ícone de loja de aplicativos serão rejeitados pela Apple.

O ícone de armazenamento de aplicativos é a aparência do seu aplicativo para usuários, portanto ele deve ser fácil de lembrar e exibição bem nesse tamanho pequeno. Ícones memoráveis são limpos, simples e imediatamente reconhecíveis.

A Apple sugere as seguintes diretrizes ao criar o ícone do seu aplicativo:

- Torne o ícone apropriado para seu aplicativo.
- Crie um ícone simples, consistente com o design do seu aplicativo.
- Evite usar palavras em seu ícone.
- Pense globalmente: um único ícone do aplicativo é usado em todos os territórios de repositório.

Uma imagem de 1.024 x 1.024 pixels é necessária para o ícone do aplicativo que será exibido na App Store.  A Apple declarou que o ícone da App Store no catálogo de ativos não pode ser transparente nem conter um canal alfa.

Para obter mais informações, consulte da Apple [iOS diretrizes de Interface Humana](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/image-size-and-resolution/).

## <a name="adding-an-app-store-icon"></a>Adicionar um ícone de loja de aplicativos

Os ícones de Lojas de Aplicativos devem ser entregues agora por um catálogo de ativos. 

Para adicionar um ícone de armazenamento de aplicativo faça o seguinte:

1. Localize o **AppIcon** imagem definida **Assets.xcassets** arquivo do projeto. 
    - Todos os projetos novos devem ser acompanhadas de uma uma **Assets.xcassets** arquivo que contém um conjunto de imagem AppIcon.
    - Para adicionar um novo catálogo de ativos, clique com botão direito no projeto e selecione **Adicionar > novo arquivo > Catálogo de ativos**.
    - Para adicionar um novo um conjunto de imagem do ícone de aplicativo, clique no ícone conjunto área e selecione **ícones de aplicativo & imagens de inicialização > novo ícone de aplicativo**:
    
    ![Adicionar nova opção de conjunto de imagem](app-store-icon-images/image1.png)

2. Role até a **App Store** ícone na lista:

    ![Ícone de loja de aplicativos](app-store-icon-images/image2.png)

3. Clique no ícone e navegue para a imagem de pixel de 1024 x 1024. Salve o catálogo de ativos.




## <a name="related-links"></a>Links relacionados

- [Gerenciando ícones com catálogos de ativo](~/ios/app-fundamentals/images-icons/app-icons.md#managing)