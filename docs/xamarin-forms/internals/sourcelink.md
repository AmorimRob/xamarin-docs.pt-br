---
title: Link de origem com Xamarin. Forms
description: Este artigo explica como usar o link de origem para depurar no Xamarin. Forms.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetId: 1E13FCD9-5607-46E8-80E4-87A58B389BEB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 09/26/2019
ms.openlocfilehash: 7a3fe70c8ac29f9e84b5d071a0ba1ef73afbbe11
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697496"
---
# <a name="source-link-with-xamarinforms"></a>Link de origem com Xamarin. Forms

Os pacotes do NuGet do Xamarin. Forms incluem mapeamentos de link de origem. O link de origem mapeia as bibliotecas compiladas, contidas em um pacote NuGet, para um repositório de código-fonte. O Visual Studio baixará os arquivos de código-fonte durante a depuração e permitirá que os desenvolvedores percorram o código, habilitando a depuração de pacotes sem compilar da origem.

Para obter mais informações sobre como usar o link de origem, consulte [documentação do link de origem](/dotnet/standard/library-guidance/sourcelink).

::: zone pivot="windows"

> [!WARNING]
> O Visual Studio 2019 dá suporte ao link de origem para o **depurador .net** , mas atualmente não dá suporte ao link de origem para o **depurador mono**. Portanto, você pode usar o link de origem para depurar aplicativos UWP, mas não o aplicativo Android ou iOS. Ao depurar aplicativos UWP, você deve garantir que os arquivos PDB das bibliotecas que você deseja depurar sejam copiados para a pasta **Appx** no diretório **bin** em que seu aplicativo é compilado.

## <a name="enable-source-link"></a>Habilitar link de origem

O uso do link de origem requer a habilitação da depuração para o código externo, caso contrário, o depurador passará por chamadas para o código não contido na solução atual. No Visual Studio 2019, isso pode ser encontrado no menu de **Opções** na seção **depuração** :

[Link de origem do ![Enable no Visual Studio 2019](sourcelink-images/sourcelink-enable-pc-cropped.png)](sourcelink-images/sourcelink-enable-pc.png#lightbox)

Verifique se **habilitar apenas meu código** está desabilitado e se **habilitar suporte ao link de origem** está habilitado.

::: zone-end
::: zone pivot="macos"

## <a name="enable-source-link"></a>Habilitar link de origem

O uso do link de origem requer a habilitação da depuração para o código externo, caso contrário, o depurador passará por chamadas para o código não contido na solução atual. Essa opção pode ser encontrada na janela **preferências** na seção **depurador** :

[![Enable link de origem no Visual Studio para Mac](sourcelink-images/sourcelink-enable-mac-cropped.png)](sourcelink-images/sourcelink-enable-mac.png#lightbox)

Verifique se a **etapa no código externo** está habilitada.

::: zone-end

## <a name="debug-xamarinforms-using-source-link"></a>Depurar o Xamarin. Forms usando o link de origem

Se a depuração de pacotes externos estiver habilitada, o Visual Studio usará os mapeamentos de link de origem contidos no pacote NuGet para baixar e percorrer o código-fonte externo. Isso pode ser testado definindo um ponto de interrupção em uma chamada para um método fornecido pelo Xamarin. Forms:

[![Breakpoint definido no método Xamarin. Forms](sourcelink-images/breakpoint-cropped.png)](sourcelink-images/external-code-available.png#lightbox)

Dependendo das configurações especificadas nas opções do **depurador** , o Visual Studio avisará que ele está baixando arquivos de origem:

[aviso de código externo do ![Visual Studio](sourcelink-images/external-code-cropped.png)](sourcelink-images/external-code-available.png#lightbox)

Depois que você permitir que o Visual Studio Baixe os arquivos, o depurador entrará no código externo.

::: zone pivot="windows"

## <a name="source-link-caching"></a>Cache de link de origem

O link de origem usa o cache para desempenho. O diretório de cache para o link de origem é definido no menu **Opções** , em **depuração** na seção **símbolos** :

[cache de link de origem do ![Visual Studio](sourcelink-images/sourcelink-caching-pc-cropped.png)](sourcelink-images/sourcelink-caching-pc.png#lightbox)

Esse menu permite especificar o diretório de cache para todos os símbolos de depuração, bem como limpar o cache se você encontrar problemas com os símbolos armazenados em cache.

::: zone-end
::: zone pivot="macos"

## <a name="source-link-caching"></a>Cache de link de origem

O link de origem usa o cache para desempenho. O diretório de cache para o link de origem no MacOS é `/Users/<username>/Library/Caches/VisualStudio/8.0/Symbols`. Esta pasta contém subpastas que armazenam o repositório usado para baixar arquivos de origem. Se o repositório de backup de um pacote NuGet tiver sido alterado, talvez seja necessário excluir manualmente essas pastas para atualizar o cache.

::: zone-end

## <a name="related-links"></a>Links relacionados

- [Documentação do link de origem](/dotnet/standard/library-guidance/sourcelink)
- [Link de origem no GitHub](https://github.com/dotnet/sourcelink)