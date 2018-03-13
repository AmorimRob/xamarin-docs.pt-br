---
title: "Erro IBTool: Não foi possível concluir a operação."
ms.topic: article
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: dd668859428da1abfa3a8e46a0810b2de6645fe2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>Erro IBTool: Não foi possível concluir a operação.

## <a name="fixed-in-xcode-611"></a>Corrigido no Xcode 6.1.1

Apple [fixa](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) isso `ibtool` bug no Xcode 6.1.1, para atualizar para o Xcode 6.1.1 ou superior é a mais fácil.

* * *

## <a name="description-of-the-problem"></a>Descrição do problema

O `ibtool` comando no Xcode 6.0 tinha um bug nos X 10.10 Yosemite. Xamarin usa do Xcode `ibtool` para compilar storyboards e `XIB` arquivos.

Para obter mais informações sobre o erro em relação ao Xcode podem ser encontradas no seguinte postagem de estouro de pilha: [http://stackoverflow.com/questions/25754763/cant-open-storyboard](http://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>Mensagem de erro

> Não foi possível abrir o documento "MainStoryboard.storyboard". Não foi possível concluir a operação. (erro de com.apple.InterfaceBuilder -1).

## <a name="workarounds-for-xcode-60"></a>Soluções alternativas (para Xcode 6.0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>Opção 1: Gerenciar todos os `UIImageView.Image` propriedades em código

Em vez de configuração o `Image` propriedade de um `UIImageView` no storyboard ou `.xib` arquivo, você pode definir a propriedade de uma exibição de métodos de substituição de ciclo de vida no controlador de exibição (por exemplo, em `ViewDidLoad()`). Consulte também [trabalhando com imagens](~/ios/app-fundamentals/images-icons/index.md) para obter dicas sobre como usar `UIImage.FromBundle()` versus `UIImage.FromFile()`.

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>Opção 2: Mover todos os recursos de imagem para o nível superior `Resources` pasta

Depois de mover as imagens para o nível superior `Resources` pasta, você precisará atualizar o storyboard e `.xib` arquivos para usar os novos caminhos de imagem.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Opção 3: Definir o `LogicalName` para qualquer ativos de imagem problemático para serem copiados para o nível superior do`.app` pacote

Por exemplo, digamos que seu original `.csproj` arquivo contém a seguinte entrada:

`<BundleResource Include="Resources\Images\image.png" />`

Você pode alterar esse elemento e adicionar um `LogicalName` para que a imagem em vez disso, será copiada para o nível superior do `.app `pacote:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

No Visual Studio para Mac os `LogicalName` também podem ser definidas usando o `Resource ID` campo para a imagem em **exibição > preenche > propriedades**. (Consulte também: [http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545](http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

Após essa alteração, você precisará atualizar o storyboard e `.xib` arquivos para usar os novos caminhos de imagem de nível superior. O Visual Studio para Mac atualizará automaticamente a lista de autocompletions para o `Image` propriedade no Designer de iOS. No Visual Studio, você precisará editar o caminho manualmente. O Designer do iOS, em seguida, exibirá isso como uma imagem ausente, mas o projeto irá criar e executar corretamente.

### <a name="next-steps"></a>Próximas etapas

Para obter mais assistência, entre em contato conosco ou se esse problema permanece mesmo após utilizando as informações acima, consulte [quais opções de suporte estão disponíveis para Xamarin?](~/cross-platform/troubleshooting/support-options.md) para obter informações sobre opções de contato, sugestões, bem como a registrar um bug de novo, se necessário. 
