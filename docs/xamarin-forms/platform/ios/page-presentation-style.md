---
title: Estilo de apresentação de página modal no iOS
description: Especificidades da plataforma permitem que você consumir funcionalidade só está disponível em uma plataforma específica, sem implementar renderizadores personalizados ou efeitos. Este artigo explica como consumir os conjuntos específicos da plataforma iOS, o estilo de apresentação de uma página modal.
ms.prod: xamarin
ms.assetid: C791F7CF-330A-44BA-987A-4CFCCBB9278B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 3b1a88968334bed42be53119c26de43ef9cd1419
ms.sourcegitcommit: eb23b7d745d1090376f9def07e0f11cb089494d0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72171049"
---
# <a name="modal-page-presentation-style-on-ios"></a>Estilo de apresentação de página modal no iOS

[![Baixar exemplo](~/media/shared/download.png) baixar o exemplo](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Essa plataforma específica do iOS é usada para definir o estilo de apresentação de uma página modal. Ele é consumido em XAML, definindo o `Page.ModalPresentationStyle` propriedade associável a uma `UIModalPresentationStyle` valor de enumeração:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="FormSheet">
    ...
</ContentPage>
```

Como alternativa, ele pode ser consumido de c# usando a API fluente:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSModalFormSheetPageCS : ContentPage
{
    public iOSModalFormSheetPageCS()
    {
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.FormSheet);
        ...
    }
}
```

O `Page.On<iOS>` método Especifica que este específicos da plataforma serão executado apenas no iOS. O `Page.SetModalPresentationStyle` método, no [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace, é usado para definir o estilo de apresentação modal em um [ `Page` ](xref:Xamarin.Forms.Page) especificando um dos seguintes `UIModalPresentationStyle` enumeração valores:

- `FullScreen`, que define o estilo de apresentação modal para abranger a tela inteira. Por padrão, as páginas modais são exibidas usando esse estilo de apresentação.
- `FormSheet`, que define o estilo de apresentação modal seja menor do que a tela e centralizada no.

Além disso, o `GetModalPresentationStyle` método pode ser usado para recuperar o valor atual do `UIModalPresentationStyle` enumeração é aplicada para o [ `Page` ](xref:Xamarin.Forms.Page).

O resultado é que o estilo de apresentação modal em uma [ `Page` ](xref:Xamarin.Forms.Page) podem ser definidos:

[![](page-presentation-style-images/modal-presentation-style-small.png "Estilos de apresentação modal em um iPad")](page-presentation-style-images/modal-presentation-style-large.png#lightbox "Estilos de apresentação modal em um iPad")

> [!NOTE]
> As páginas que usam esse específicos da plataforma para definir o estilo de apresentação modal devem usar a navegação modal. Para obter mais informações, consulte [páginas modais do xamarin. Forms](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## <a name="related-links"></a>Links relacionados

- [PlatformSpecifics (amostra)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Criação de itens específicos à plataforma](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API iOSSpecific](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)