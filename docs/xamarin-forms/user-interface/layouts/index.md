---
title: Layouts
description: "Dispor os modos de exibição na tela."
ms.topic: article
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 0ede9bbb47f398a82d6eae5d827122f469ad6ea4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/09/2018
---
# <a name="layouts"></a>Layouts

Xamarin. Forms tem vários layouts e recursos para organizar o conteúdo na tela. 

> [!VIDEO https://youtube.com/embed/4HlLjTZQzjM]

**Layouts de xamarin. Forms, pelo [University Xamarin](https://university.xamarin.com/)**

Cada controle de layout é descrito abaixo, bem como detalhes sobre como lidar com as alterações de orientação de tela:

* **[StackLayout](stack-layout.md)**  &ndash; usado para organizar exibições linearmente, horizontal ou verticalmente. Modos de exibição em um StackLayout podem ser alinhados ao centro, esquerdo ou direito do layout.
* **[AbsoluteLayout](absolute-layout.md)**  &ndash; usado para organizar exibições definindo coordenadas & tamanho em termos de valores absolutos ou taxas. AbsoluteLayout pode ser usado para modos de exibição da camada, bem como ancorá-los para a esquerda, direita ou centralizado.
* **[RelativeLayout](relative-layout.md)**  &ndash; usado para organizar exibições definindo restrições em relação a dimensões e a posição de seu pai.
* **[Grade](grid.md)**  &ndash; usado para organizar exibições em uma grade. Linhas e colunas podem ser especificadas em termos de valores absolutos ou taxas.
* **[ScrollView](scroll-view.md)**  &ndash; usado para fornecer a rolagem quando um modo de exibição não couber completamente dentro dos limites da tela.
* **[LayoutOptions](layout-options.md)**  &ndash; definir o alinhamento e a expansão de um modo de exibição, em relação ao seu pai.
* **[Entrada transparência](#input_transparency)**  &ndash; Especifica se um elemento recebe entrada.
* **[Margem e preenchimento](margin-and-padding.md)**  &ndash; demonstra como controlar o comportamento de layout quando um elemento é renderizado na interface do usuário.
* **[Orientação do dispositivo](device-orientation.md)**  &ndash; explica como manipular as alterações de orientação do dispositivo.
* **[Layout em dispositivos tablet e área de trabalho](tablet.md)**  &ndash; mostra como otimizar para telas maiores em cada plataforma.
* **[Criar um Layout personalizado](custom.md)**  &ndash; explica como criar uma classe de layout personalizado.
* **[Compactação de layout](layout-compression.md)**  &ndash; remove especificado layout da árvore visual em uma tentativa de melhorar o desempenho de renderização da página.

Controles de plataforma também podem ser usados diretamente em layouts de xamarin. Forms com [ **inserindo nativo** ](~/xamarin-forms/platform/native-views/index.md) (novo no xamarin. Forms 2.2), e você pode [ **criar layouts personalizados** ](custom.md) para atender às necessidades específicas.

O gráfico a seguir visualiza os controles de layout:

[![](images/layouts-sml.png "Layouts de xamarin. Forms")](images/layouts.png#lightbox "xamarin. Forms Layouts")

## <a name="choosing-the-right-layout"></a>Escolher o Layout da direita

Os layouts de que escolha no seu aplicativo podem ajudar ou prejudicar o que você está criando um aplicativo xamarin. Forms atraente e utilizável. Dedicar algum tempo para considerar como funciona cada layout pode ajudá-lo a escrever código de interface do usuário mais escalonável e de limpeza. Uma tela pode ter uma combinação de layouts diferentes para alcançar um design específico.

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

O `StackLayout` é usado para exibir os modos de exibição ao longo da linha horizontal ou vertical. Posição e o tamanho do layout do é determinado com base em uma exibição `HeightRequest`, `WidthRequest`, `HorizontalOptions` e `VerticalOptions`. `StackLayout` geralmente é usada como o layout de base, organizando outros layouts na tela.

Para obter um exemplo de quando `StackLayout` seria uma boa opção, considere um aplicativo que precisa para exibir um botão e um rótulo, com o rótulo alinhado à esquerda e o botão alinhado à direita.

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

O `AbsoluteLayout` é usado para exibir os modos de exibição, com tamanho e posição sendo especificados como valores explícitos ou em relação ao tamanho do layout. Ao contrário de `StackLayout` e `Grid`, `AbsoluteLayout` permite que o filho exibições se sobreponham. Ao contrário de `RelativeLayout`, `AbsoluteLayout` não permite que você coloque elementos fora da tela.

Para obter um exemplo de quando `AbsoluteLayout` seria uma boa opção, considere um aplicativo que precisa apresentar coleções de objetos, como pilhas. Isso é geralmente visto ao apresentar álbuns de fotos ou músicas. O código a seguir fornece a aparência de uma pilha com elementos girados para dica o conteúdo da pilha:

Em XAML:

```xaml
<AbsoluteLayout Padding="15">
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="30"
    Source="bottom.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="60"
    Source="middle.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
    Source="cover.png" />
</AbsoluteLayout>
```

Observe os seguintes aspectos de código acima:

- Cada `Image` é exibida na mesma posição (no meio do espaço horizontal)
- O `Padding` é considerado por `AbsoluteLayout`, diferentemente `RelativeLayout`, que ignora a ele.
- `AbsoluteLayout.LayoutFlags` Especifica como os limites de layout serão interpretados. Nesse caso `PositionProportional`, significa que as coordenadas será uma proporção do tamanho do layout, enquanto o tamanho será interpretado como um tamanho explícito.
- `AbsoluteLayout.Layoutbounds` Especifica a posição horizontal, a posição vertical, a largura e a altura em ordem.

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

O `RelativeLayout` é usado para exibir os modos de exibição, com tamanho e posição especificada como valores relativos aos valores de layout ou de outro modo de exibição. Valores relativos não precisa corresponder a ele corresponde o valor no modo de exibição relacionado. Por exemplo, é possível definir uma exibição `Width` propriedade seja proporcional à outra exibição `X` propriedade.

RelativeLayout pode ser usado para criar interfaces do usuário que escala proporcionalmente em tamanhos de dispositivo. O XAML a seguir implementa um design com caixas nos cantos superiores, com um flagpole com sinalizador no centro:

```xaml
<RelativeLayout HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand">
  <BoxView Color="Blue" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = 0}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Red" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .9}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Gray" WidthRequest="15" x:Name="pole"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.75}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .45}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = .25}" />
  <BoxView Color="Green"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.10, Constant=10}"
    RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.2, Constant=20}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=X, Constant=15}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=Y, Constant=0}" />
</RelativeLayout>
```

Observe os seguintes aspectos de código acima:

- Posições e tamanhos são especificados como restrições.
- O flagpole é chamado para que o sinalizador (verde da caixa) posição pode ser definida em relação a flagpole.
- As expressões de restrição tem `Factor` e `Constant` propriedades, que podem ser usadas para definir as posições e os tamanhos como múltiplos (ou frações) de propriedades de outros objetos, além de uma constante. Constantes podem ser negativas.

### <a name="gridgridmd"></a>[Grade](grid.md)

O `Grid` é usado para exibir elementos em linhas e colunas. Observe que a grade não é uma tabela, para que ele não tem o conceito de células, linhas de cabeçalho e rodapé ou as bordas entre linhas e colunas. Em geral, a grade não é apropriada para exibir dados tabulares. Para que usam, considere um [ListView](~/xamarin-forms/user-interface/listview/index.md) ou [modo de tabela](~/xamarin-forms/user-interface/tableview.md).

Para obter um exemplo de quando um `Grid` é o layout da direita para usar, considere uma entrada numérica para uma calculadora. Uma entrada numérica para uma calculadora pode consistir em quatro linhas e três colunas, cada um com um botão. O código a seguir implementa esse design:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>

  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Button Text="1" Grid.Row="0" Grid.Column="0" />
  <Button Text="2" Grid.Row="0" Grid.Column="1" />
  <Button Text="3" Grid.Row="0" Grid.Column="2" />
  <Button Text="4" Grid.Row="1" Grid.Column="0" />
  <Button Text="5" Grid.Row="1" Grid.Column="1" />
  <Button Text="6" Grid.Row="1" Grid.Column="2" />
  <Button Text="7" Grid.Row="2" Grid.Column="0" />
  <Button Text="8" Grid.Row="2" Grid.Column="1" />
  <Button Text="9" Grid.Row="2" Grid.Column="2" />
  <Button Text="0" Grid.Row="3" Grid.Column="1" />
  <Button Text="&lt;-" Grid.Row="3" Grid.Column="2" />
</Grid>
```

Observe os seguintes aspectos de código acima:

- Grade e colunas são especificadas explicitamente, não é inferido do conteúdo.
- `Height` e `Width` valores podem ser definidos em estrela, o que significa que a grade definirá esses valores para preencher o espaço disponível.
- Posição de cada botão é especificada por `Grid.Row`  &  `Grid.Column` propriedades.

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

O [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) estrutura pode ser usada para definir o alinhamento e a expansão de um modo de exibição, em relação ao seu pai.

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[Margem e preenchimento](margin-and-padding.md)

O [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) e [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) propriedades controlam o comportamento de layout quando um elemento é renderizado na interface do usuário.

<a name="input_transparency" />

### <a name="input-transparency"></a>Entrada de transparência

Cada elemento tem um [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) propriedade que é usada para definir se o elemento recebe entrada. O valor padrão é `false`, garantindo que o elemento recebe entrada.

Quando essa propriedade é definida em uma classe de contêiner, como uma classe de layout, suas transferências de valor para elementos filho. Portanto, definir o [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) propriedade `true` em um layout de classe resultará em elementos dentro do layout não receber entrada.

### <a name="device-orientationdevice-orientationmd"></a>[Orientação do dispositivo](device-orientation.md)

Xamarin. Forms e seus layouts internos são capazes de lidar com as alterações na orientação do dispositivo. Considere quais orientações seu aplicativo dará suporte, bem como você fará o uso de espaço fornecido nos modos retrato e paisagem.

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[Layout para aplicativos de Desktop e Tablet](tablet.md)

iOS, Android e Windows plataformas todos os maiores tamanhos de telas oferecem suporte no tablet dispositivos (assim como laptops e estações de trabalho Windows). Xamarin. Forms permite otimizar seu aplicativo para telas maiores detectar o tipo de dispositivo e a ajustar o layout de página ou usando uma página totalmente diferente completamente para telas maiores.

### <a name="creating-a-custom-layoutcustommd"></a>[Criar um layout personalizado](custom.md)

Xamarin. Forms define quatro classes de layout - [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/), [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/), e [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), e cada organiza seus filhos de forma diferente. No entanto, às vezes, o necessário para organizar o conteúdo de página usando um layout não fornecida pelo xamarin. Forms. Este artigo explica como escrever uma classe de layout personalizado e demonstra uma orientação diferencia `WrapLayout` classe organiza seus filhos horizontalmente na página e, em seguida, ajusta a exibição de filhos subsequentes para linhas adicionais.

### <a name="layout-compressionlayout-compressionmd"></a>[Compactação de Layout](layout-compression.md)

Compactação de layout remove especificados layouts da árvore visual em uma tentativa de melhorar o desempenho de renderização da página. O benefício de desempenho que isso oferece varia dependendo da complexidade de uma página, da versão do sistema operacional que está sendo usado e do dispositivo no qual o aplicativo está sendo executado. No entanto, os maiores ganhos de desempenho serão observados em versões mais antigas.

## <a name="making-your-choice"></a>Fazer sua escolha

Lembre-se de que na maioria dos casos, mais de uma opção de layout pode ser usada para implementar o design desejado. Quando há várias opções válidas, considere qual abordagem será mais fácil para a sua situação.
Não não possível realizar a maioria dos designs com apenas um layout, para que aninhar layouts como necessárias para criar mais designs complexos.


## <a name="related-links"></a>Links relacionados

- [Diretrizes de Interface humana da Apple](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Site de Design do Android](https://developer.android.com/design/index.html)
- [Layout (exemplo)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Exemplo de BusinessTumble (exemplo)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)