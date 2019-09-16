---
title: Introdução ao CarouselView do Xamarin. Forms
description: O CarouselView é uma exibição para apresentar dados em um layout de tipo de carrossel.
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
ms.openlocfilehash: 7979f6ed362c580d9cf80f19b3bc0ea7550ca70c
ms.sourcegitcommit: cf56d2bae34dc0f8e94c2d3d28d5f460d59807bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985969"
---
# <a name="xamarinforms-carouselview-introduction"></a>Introdução ao CarouselView do Xamarin. Forms

![](~/media/shared/preview.png "Esta API está atualmente em pré-lançamento")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)é uma exibição para apresentar informações de uma maneira do tipo carrossel.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)está disponível no Xamarin. Forms 4,3. No entanto, ele é experimental e só pode ser usado adicionando a linha de código a seguir à `AppDelegate` sua classe no Ios ou à sua `MainActivity` classe no Android, antes de `Forms.Init`chamar:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

_Observação: No momento da redação deste artigo, o CarouselView ainda usa o sinalizador CollectionView para habilitar sua funcionalidade._

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)está disponível no iOS e no Android, mas algumas funcionalidades só podem estar parcialmente disponíveis no Plataforma Universal do Windows.

## <a name="when-to-use-carouselview"></a>Quando usar o CarouselView

Enquanto o CarouselView baseia grande parte de sua implementação de CollectionView, sua finalidade é mais focada. Embora o uso de CollectionView e CarouselView seja seu critério, os carrossel em aplicativos normalmente são usados para realçar informações e são limitados no número total de itens usados.