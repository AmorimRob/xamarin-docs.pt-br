---
title: BoxView
description: "Use um retângulo colorido para decoração, gráficos e interação."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2017
ms.openlocfilehash: 4d50ea5c3db0f5a141f1b48cf0a948c10b63f7f0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/09/2018
---
# <a name="boxview"></a>BoxView

[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) renderiza um retângulo simple de cor, altura e largura especificada. Você pode usar `BoxView` para a decoração, rudimentares gráficos e para interação com o usuário por meio de toque.

Porque xamarin. Forms não tem um sistema de gráficos vetoriais interna, o `BoxView` ajuda para compensar. Alguns dos programas de exemplo descritos neste artigo usam `BoxView` para processamento de gráficos. O `BoxView` pode ser dimensionado para se parecer com uma linha de uma largura específica e a espessura e girado usando qualquer ângulo de `Rotation` propriedade.

Embora `BoxView` pode imitar a elementos gráficos simples, talvez você queira investigar [usando SkiaSharp em xamarin. Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) para requisitos de gráficos mais sofisticados.

Este artigo aborda os seguintes tópicos:

- **[Configuração BoxView cor e tamanho](#colorandsize)**  &ndash; definir o `BoxView` propriedades.
- **[Decoração de texto de renderização](#textdecorations)**  &ndash; usar um `BoxView` para linhas de processamento.
- **[Listando as cores com BoxView](#listingcolors)**  &ndash; exibir todas as cores do sistema em um `ListView`.
- **[Executando o jogo da vida útil por subclassificação BoxView](#subclassing)**  &ndash; implementar uma automação famosa de celular.
- **[Criando um relógio Digital](#digitalclock)**  &ndash; simular uma exibição de matriz de ponto.
- **[Criando um relógio analógico](#analogclock)**  &ndash; transformar e animar `BoxView` elementos.

<a name="colorandsize" />

## <a name="setting-boxview-color-and-size"></a>Configuração BoxView cor e tamanho

Com muita frequência, você definirá as seguintes três propriedades de `BoxView`:

- [`Color`](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) Para definir sua cor.
- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) Para definir a largura do `BoxView` em unidades independentes de dispositivo.
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) Para definir a altura do `BoxView`.

O `Color` é de propriedade do tipo `Color`; a propriedade pode ser definida para qualquer `Color` valor, incluindo os 141 campos estáticos de somente leitura de chamado cores variando em ordem alfabética de `AliceBlue` para `YellowGreen`.

O `WidthRequest` e `HeightRequest` propriedades desempenham uma função somente se o `BoxView` é *irrestrita* no layout. Este é o caso quando o contêiner de layout precisa saber o filho do tamanho, por exemplo, quando o `BoxView` é um filho de uma célula de dimensionamento automático no `Grid` layout. Um `BoxView` também é irrestrita quando seu `HorizontalOptions` e `VerticalOptions` propriedades são definidas como valores diferentes de `LayoutOptions.Fill`. Se o `BoxView` irrestrita, mas o `WidthRequest` e `HeightRequest` propriedades não estão definidas e, em seguida, a largura ou altura são definidas com valores padrão de 40 unidades, ou cerca de 1/4 polegadas em dispositivos móveis.

O `WidthRequest` e `HeightRequest` propriedades são ignoradas se o `BoxView` é *restrita* no layout, nesse caso o contêiner de layout impõe seu próprio tamanho de `BoxView`. 

Um `BoxView` pode ser restrita em uma dimensão e irrestrita no outro. Por exemplo, se o `BoxView` é um filho de um vertical `StackLayout`, a dimensão vertical do `BoxView` é irrestrita e sua dimensão horizontal geralmente é restrito. Mas há exceções para a dimensão horizontal: se o `BoxView` tem seu `HorizontalOptions` propriedade definida como algo diferente de `LayoutOptions.Fill`, em seguida, a dimensão horizontal também é irrestrita. Também é possível que o `StackLayout` para ter uma dimensão horizontal irrestrita, caso em que o `BoxView` também será irrestrita horizontalmente.

O [ **BasicBoxView** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView) exemplo exibe um uma polegada-quadrada irrestrita `BoxView` no centro da sua página:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BasicBoxView"
             x:Class="BasicBoxView.MainPage">

    <BoxView Color="CornflowerBlue"
             WidthRequest="160"
             HeightRequest="160"
             VerticalOptions="Center" 
             HorizontalOptions="Center" />

</ContentPage>
```

Aqui está o resultado:

[![Básico BoxView](boxview-images/basicboxview-small.png "BoxView básica")](boxview-images/basicboxview-large.png#lightbox "BasicBoxView")

Se o `VerticalOptions` e `HorizontalOptions` propriedades são removidas do `BoxView` marca ou são definidas com `Fill`, em seguida, o `BoxView` ficar restritas pelo tamanho da página e se expande para preencher a página.

Um `BoxView` também pode ser um filho de um `AbsoluteLayout`. Nesse caso, o local e o tamanho do `BoxView` são definidos usando o `LayoutBounds` propriedade associável anexada. O `AbsoluteLayout` é discutida no artigo [ **AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolute-layout.md).

Você verá exemplos de todos esses casos em que os programas de exemplo que segue.

<a name="textdecorations" />

## <a name="rendering-text-decorations"></a>Renderização de decoração de texto

Você pode usar o `BoxView` para adicionar alguns decorações simples em suas páginas na forma de linhas horizontais e verticais. O [ **TextDecoration** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration) exemplo demonstra isso. Todos os visuais do programa são definidos no **MainPage. XAML** arquivo, que contém vários `Label` e `BoxView` elementos o `StackLayout` mostrado aqui:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:TextDecoration"
             x:Class="TextDecoration.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Black" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ScrollView Margin="15">
        <StackLayout>

            ···

        </StackLayout>
    </ScrollView>
</ContentPage>
```

A marcação que segue todas são filhos do `StackLayout`. Essa marcação consiste em vários tipos de decorativo `BoxView` elementos usados com o `Label` elemento:

[![Decoração de texto](boxview-images/textdecoration-small.png "decoração de texto")](boxview-images/textdecoration-large.png#lightbox "decoração de texto")

O estilo cabeçalho na parte superior da página é obtido com um `AbsoluteLayout` cujos filhos são os quatro `BoxView` elementos e um `Label`, todos os que são atribuídos tamanhos e locais específicos:

```xaml
<AbsoluteLayout>
    <BoxView AbsoluteLayout.LayoutBounds="0, 10, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="0, 20, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="10, 0, 5, 65" />
    <BoxView AbsoluteLayout.LayoutBounds="20, 0, 5, 65" />
    <Label Text="Stylish Header"
           FontSize="24"
           AbsoluteLayout.LayoutBounds="30, 25, AutoSize, AutoSize"/>
</AbsoluteLayout>
```

No arquivo XAML, o `AbsoluteLayout` é seguido por um `Label` com texto formatado que descreve o `AbsoluteLayout`.

Uma cadeia de caracteres de texto pode ser sublinhado colocando ambos os `Label` e `BoxView` em uma `StackLayout` que tem seu `HorizontalOptions` valor definido para algo diferente de `Fill`. A largura do `StackLayout` , em seguida, é governada pela largura do `Label`, que impõe essa largura de `BoxView`. O `BoxView` é atribuído a apenas uma altura explícita:

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

Você não pode usar essa técnica para sublinhado palavras individuais dentro de cadeias de caracteres de texto mais longas ou um parágrafo.

Também é possível usar um `BoxView` se pareça com um HTML `hr` elemento (regra horizontal). Simplesmente deixar que a largura do `BoxView` ser determinado por seu contêiner pai, que nesse caso é o `StackLayout`:

```xaml
<BoxView HeightRequest="3" />
```

Por fim, você pode desenhar uma linha vertical em um lado de um parágrafo de texto colocando ambos os `BoxView` e `Label` em um horizontal `StackLayout`. Nesse caso, a altura do `BoxView` é o mesmo que a altura de `StackLayout`, que é controlado pela altura do `Label`: 

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
/StackLayout>
```
<a name="listingcolors" />

## <a name="listing-colors-with-boxview"></a>Listando as cores com BoxView

O `BoxView` é conveniente para a exibição de cores. Esse programa usa uma `ListView` para listar todos os públicos estáticos somente leitura campos do xamarin. Forms `Color` estrutura:

[![Cores de ListView](boxview-images/listviewcolors-small.png "cores ListView")](boxview-images/listviewcolors-large.png#lightbox "cores de ListView")

O [ **ListViewColors** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ListViewColors/) programa inclui uma classe chamada `NamedColor`. O construtor estático usa reflexão para acessar todos os campos do `Color` estrutura e crie um `NamedColor` objeto para cada um. Eles são armazenados no estático `All` propriedade:

```csharp
public class NamedColor
{
    // Instance members.
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    // Static members.
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields ())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof (Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                               (int)(255 * color.R),
                                               (int)(255 * color.G),
                                               (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }
}
```

Os visuais de programa são descritos no arquivo XAML. O `ItemsSource` propriedade o `ListView` é definido como estático `NamedColor.All` propriedade, o que significa que o `ListView` exibe todos os individuais `NamedColor` objetos: 

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ListViewColors"
             x:Class="ListViewColors.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="10, 20, 10, 0" />
            <On Platform="Android, UWP" Value="10, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ListView SeparatorVisibility="None"
              ItemsSource="{x:Static local:NamedColor.All}">
        <ListView.RowHeight>
            <OnPlatform x:TypeArguments="x:Int32">
                <On Platform="iOS, Android" Value="80" />
                <On Platform="UWP" Value="90" />
            </OnPlatform>
        </ListView.RowHeight>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <ContentView Padding="5">
                        <Frame OutlineColor="Accent"
                               Padding="10">
                            <StackLayout Orientation="Horizontal">
                                <BoxView Color="{Binding Color}"
                                         WidthRequest="50"
                                         HeightRequest="50" />
                                <StackLayout>
                                    <Label Text="{Binding FriendlyName}"
                                           FontSize="22"
                                           VerticalOptions="StartAndExpand" />
                                    <Label Text="{Binding RgbDisplay, StringFormat='RGB = {0}'}"
                                           FontSize="16"
                                           VerticalOptions="CenterAndExpand" />
                                </StackLayout>
                            </StackLayout>
                        </Frame>
                    </ContentView>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage> 
```

O `NamedColor` objetos são formatados pelo `ViewCell` objeto é definido como o modelo de dados do `ListView`. Esse modelo inclui uma `BoxView` cujo `Color` propriedade está vinculada a `Color` propriedade do `NamedColor` objeto.

<a name="subclassing" />

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>Executando o jogo da vida útil por subclassificação BoxView

O jogo de vida é uma automação celular criado pelo matemático John Conway e popularizada nas páginas do *científicos American* no 1970s. Uma boa introdução é fornecida no artigo da Wikipedia [jogo de vida de Conway](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life).

O xamarin. Forms [ **GameOfLife** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/) programa define uma classe denominada `LifeCell` que deriva de `BoxView`. Essa classe encapsula a lógica de uma célula individual na jogo de vida:

```csharp
class LifeCell : BoxView
{
    bool isAlive;

    public event EventHandler Tapped;

    public LifeCell()
    {
        BackgroundColor = Color.White;

        TapGestureRecognizer tapGesture = new TapGestureRecognizer();
        tapGesture.Tapped += (sender, args) =>
        {
            Tapped?.Invoke(this, EventArgs.Empty);
        };
        GestureRecognizers.Add(tapGesture);
    }

    public int Col { set; get; }

    public int Row { set; get; }

    public bool IsAlive
    {
        set
        {
            if (isAlive != value)
            {
                isAlive = value;
                BackgroundColor = isAlive ? Color.Black : Color.White;
            }
        }
        get
        {
            return isAlive;
        }
    }
}
```

`LifeCell` adiciona três propriedades para `BoxView`: o `Col` e `Row` propriedades armazenam a posição da célula dentro da grade e o `IsAlive` propriedade indica seu estado. O `IsAlive` propriedade também define o `Color` propriedade do `BoxView` para preto se a célula estiver ativo e branco se a célula não está ativa.

`LifeCell` também instala um `TapGestureRecognizer` para permitir que o usuário alterne o estado de células tocando-los. A classe converte a `Tapped` o reconhecedor de gestos em seu próprio evento `Tapped` evento.

O **GameOfLife** programa também inclui um `LifeGrid` classe que encapsula a maior parte da lógica do jogo, e um `MainPage` classe que controla os visuais do programa. Esses incluem uma sobreposição que descreve as regras do jogo. Aqui está o programa em ação mostrando algumas centenas `LifeCell` objetos na página:

[![Jogo da vida útil](boxview-images/gameoflife-small.png "jogo da vida útil")](boxview-images/gameoflife-large.png#lightbox "jogo da vida útil")

<a name="digitalclock" />

## <a name="creating-a-digital-clock"></a>Criando um relógio Digital

O [ **DotMatrixClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock/) programa cria 210 `BoxView` elementos para simular os pontos de uma exibição de 5 por 7 matricial antigo. Você pode ler o tempo no modo paisagem ou retrato, mas ele é maior em paisagem:

[![Relógio matricial](boxview-images/dotmatrixclock-small.png "matricial relógio")](boxview-images/dotmatrixclock-large.png#lightbox "matricial de relógio")

O arquivo XAML de pouco mais do que criar uma instância de `AbsoluteLayout` usado para o relógio:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DotMatrixClock"
             x:Class="DotMatrixClock.MainPage"
             Padding="10"
             SizeChanged="OnPageSizeChanged">

    <AbsoluteLayout x:Name="absoluteLayout"
                    VerticalOptions="Center" />
</ContentPage>
```

Tudo ocorre no arquivo code-behind. A lógica de exibição matricial é bastante simplificada pela definição de várias matrizes que descrevem os pontos que correspondem a cada 10 dígitos e dois-pontos:

```csharp
public partial class MainPage : ContentPage
{
    // Total dots horizontally and vertically.
    const int horzDots = 41;
    const int vertDots = 7;

    // 5 x 7 dot matrix patterns for 0 through 9.
    static readonly int[, ,] numberPatterns = new int[10, 7, 5]
    {
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 1, 1}, { 1, 0, 1, 0, 1}, 
            { 1, 1, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 0, 0}, { 0, 1, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0}, 
            { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, 
            { 0, 0, 1, 0, 0}, { 0, 1, 0, 0, 0}, { 1, 1, 1, 1, 1}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 0, 1, 0},
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 0, 1, 0}, { 0, 0, 1, 1, 0}, { 0, 1, 0, 1, 0}, { 1, 0, 0, 1, 0}, 
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 0, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0}, { 0, 0, 0, 0, 1}, 
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 1, 0}, { 0, 1, 0, 0, 0}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0}, 
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0}, 
            { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}, 
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 1}, 
            { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 1, 1, 0, 0}
        },
    };

    // Dot matrix pattern for a colon.
    static readonly int[,] colonPattern = new int[7, 2]
    {
        { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }
    };

    // BoxView colors for on and off.
    static readonly Color colorOn = Color.Red;
    static readonly Color colorOff = new Color(0.5, 0.5, 0.5, 0.25);

    // Box views for 6 digits, 7 rows, 5 columns.
    BoxView[, ,] digitBoxViews = new BoxView[6, 7, 5];

    ···

}
```

Esses campos terminam com uma matriz tridimensional do `BoxView` elementos para armazenar os padrões de ponto de seis dígitos.

O construtor cria todos os o `BoxView` elementos para os dígitos e dois-pontos e também inicializa o `Color` propriedade o `BoxView` elementos para os dois-pontos:

```csharp
public partial class MainPage : ContentPage
{

    ···

    public MainPage()
    {
        InitializeComponent();

        // BoxView dot dimensions.
        double height = 0.85 / vertDots;
        double width = 0.85 / horzDots;

        // Create and assemble the BoxViews.
        double xIncrement = 1.0 / (horzDots - 1);
        double yIncrement = 1.0 / (vertDots - 1);
        double x = 0;

        for (int digit = 0; digit < 6; digit++)
        {
            for (int col = 0; col < 5; col++)
            {
                double y = 0;

                for (int row = 0; row < 7; row++)
                {
                    // Create the digit BoxView and add to layout.
                    BoxView boxView = new BoxView();
                    digitBoxViews[digit, row, col] = boxView;
                    absoluteLayout.Children.Add(boxView,
                                                new Rectangle(x, y, width, height),
                                                AbsoluteLayoutFlags.All);
                    y += yIncrement;
                }
                x += xIncrement;
            }
            x += xIncrement;

            // Colons between the hours, minutes, and seconds.
            if (digit == 1 || digit == 3)
            {
                int colon = digit / 2;

                for (int col = 0; col < 2; col++)
                {
                    double y = 0;

                    for (int row = 0; row < 7; row++)
                    {
                        // Create the BoxView and set the color.
                        BoxView boxView = new BoxView
                            {
                                Color = colonPattern[row, col] == 1 ?
                                            colorOn : colorOff
                            };
                        absoluteLayout.Children.Add(boxView,
                                                    new Rectangle(x, y, width, height),
                                                    AbsoluteLayoutFlags.All);
                        y += yIncrement;
                    }
                    x += xIncrement;
                }
                x += xIncrement;
            }
        }

        // Set the timer and initialize with a manual call.
        Device.StartTimer(TimeSpan.FromSeconds(1), OnTimer);
        OnTimer();
    }

    ···

}
```

Este programa usa o posicionamento relativo e o recurso de dimensionamento de `AbsoluteLayout`. A largura e altura de cada `BoxView` são definidos como valores fracionários, especificamente 85% 1 dividido pelo número de pontos horizontais e verticais. As posições também são definidas para valores fracionários. 

Porque todas as posições e os tamanhos são relativas ao tamanho total do `AbsoluteLayout`, o `SizeChanged` manipulador para a página precisa configurar um `HeightRequest` do `AbsoluteLayout`:

```csharp
public partial class MainPage : ContentPage
{
    
    ···
    
    void OnPageSizeChanged(object sender, EventArgs args)
    {
        // No chance a display will have an aspect ratio > 41:7
        absoluteLayout.HeightRequest = vertDots * Width / horzDots;
    }
    
    ···
    
}
```

A largura do `AbsoluteLayout` é definida automaticamente como ele se estende até a largura total da página.

O código final o `MainPage` cores dos pontos de cada dígito e processa o retorno de chamada do timer de classe. A definição das matrizes multidimensionais no início do arquivo code-behind ajuda a tornar essa lógica a parte mais simples do programa:

```csharp
public partial class MainPage : ContentPage
{
   
    ···
 
    bool OnTimer()
    {
        DateTime dateTime = DateTime.Now;

        // Convert 24-hour clock to 12-hour clock.
        int hour = (dateTime.Hour + 11) % 12 + 1;

        // Set the dot colors for each digit separately.
        SetDotMatrix(0, hour / 10);
        SetDotMatrix(1, hour % 10);
        SetDotMatrix(2, dateTime.Minute / 10);
        SetDotMatrix(3, dateTime.Minute % 10);
        SetDotMatrix(4, dateTime.Second / 10);
        SetDotMatrix(5, dateTime.Second % 10);
        return true;
    }

    void SetDotMatrix(int index, int digit)
    {
        for (int row = 0; row < 7; row++)
            for (int col = 0; col < 5; col++)
            {
                bool isOn = numberPatterns[digit, row, col] == 1;
                Color color = isOn ? colorOn : colorOff;
                digitBoxViews[index, row, col].Color = color;
            }
    }
}
```
<a name="analogclock" />

## <a name="creating-an-analog-clock"></a>Criando um relógio analógico

Um relógio de matriz de pontos pode parecer ser um aplicativo óbvio de `BoxView`, mas `BoxView` elementos também são capazes de perceber um relógio analógico:

[![O relógio de BoxView](boxview-images/boxviewclock-small.png "BoxView relógio")](boxview-images/boxviewclock-large.png#lightbox "BoxView relógio")

Todos os elementos visuais a [ **BoxViewClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/) programa são filhos de um `AbsoluteLayout`. Esses elementos são dimensionados usando o `LayoutBounds` propriedade anexada e girado usando o `Rotation` propriedade. 

Os três `BoxView` elementos para os ponteiros do relógio são instanciados no arquivo XAML, mas não posicionados ou tamanho:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BoxViewClock"
             x:Class="BoxViewClock.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <AbsoluteLayout x:Name="absoluteLayout"
                    SizeChanged="OnAbsoluteLayoutSizeChanged">
        
        <BoxView x:Name="hourHand"
                 Color="Black" />
        
        <BoxView x:Name="minuteHand"
                 Color="Black" />
        
        <BoxView x:Name="secondHand"
                 Color="Black" />
    </AbsoluteLayout>
</ContentPage>
```

O construtor do arquivo code-behind instancia o 60 `BoxView` elementos para as marcas de escala a circunferência do relógio:

```csharp
public partial class MainPage : ContentPage
{
      
    ···
 
    BoxView[] tickMarks = new BoxView[60];

    public MainPage()
    {
        InitializeComponent();

        // Create the tick marks (to be sized and positioned later).
        for (int i = 0; i < tickMarks.Length; i++)
        {
            tickMarks[i] = new BoxView { Color = Color.Black };
            absoluteLayout.Children.Add(tickMarks[i]);
        }

        Device.StartTimer(TimeSpan.FromSeconds(1.0 / 60), OnTimerTick);
    }
  
    ···
 
}
```

O tamanho e o posicionamento de todos os a `BoxView` elementos ocorre no `SizeChanged` manipulador para o `AbsoluteLayout`. Chamado uma pequena estrutura interna para a classe `HandParams` descreve o tamanho de cada um dos três mãos em relação ao tamanho total do relógio:

```csharp
public partial class MainPage : ContentPage
{
    // Structure for storing information about the three hands.
    struct HandParams
    {
        public HandParams(double width, double height, double offset) : this()
        {
            Width = width;
            Height = height;
            Offset = offset;
        }

        public double Width { private set; get; }   // fraction of radius
        public double Height { private set; get; }  // ditto
        public double Offset { private set; get; }  // relative to center pivot
    }

    static readonly HandParams secondParams = new HandParams(0.02, 1.1, 0.85);
    static readonly HandParams minuteParams = new HandParams(0.05, 0.8, 0.9);
    static readonly HandParams hourParams = new HandParams(0.125, 0.65, 0.9);
 
    ···
 
 }
```

O `SizeChanged` manipulador determina o centro e o raio do `AbsoluteLayout`e, em seguida, tamanhos e posiciona o 60 `BoxView` elementos usados como as marcas de escala. O `for` loop é concluído, definindo o `Rotation` propriedade de cada uma dessas `BoxView` elementos. No final do `SizeChanged` manipulador, o `LayoutHand` método é chamado para dimensionar e posicionar os três ponteiros do relógio:

```csharp
public partial class MainPage : ContentPage
{
 
    ···
 
    void OnAbsoluteLayoutSizeChanged(object sender, EventArgs args)
    {
        // Get the center and radius of the AbsoluteLayout.
        Point center = new Point(absoluteLayout.Width / 2, absoluteLayout.Height / 2);
        double radius = 0.45 * Math.Min(absoluteLayout.Width, absoluteLayout.Height);

        // Position, size, and rotate the 60 tick marks.
        for (int index = 0; index < tickMarks.Length; index++)
        {
            double size = radius / (index % 5 == 0 ? 15 : 30);
            double radians = index * 2 * Math.PI / tickMarks.Length;
            double x = center.X + radius * Math.Sin(radians) - size / 2;
            double y = center.Y - radius * Math.Cos(radians) - size / 2;
            AbsoluteLayout.SetLayoutBounds(tickMarks[index], new Rectangle(x, y, size, size));
            tickMarks[index].Rotation = 180 * radians / Math.PI;
        }

        // Position and size the three hands.
        LayoutHand(secondHand, secondParams, center, radius);
        LayoutHand(minuteHand, minuteParams, center, radius);
        LayoutHand(hourHand, hourParams, center, radius);
    }

    void LayoutHand(BoxView boxView, HandParams handParams, Point center, double radius)
    {
        double width = handParams.Width * radius;
        double height = handParams.Height * radius;
        double offset = handParams.Offset;

        AbsoluteLayout.SetLayoutBounds(boxView,
            new Rectangle(center.X - 0.5 * width,
                          center.Y - offset * height,
                          width, height));

        // Set the AnchorY property for rotations.
        boxView.AnchorY = handParams.Offset;
    }
 
    ···
 
}
```

O `LayoutHand` método tamanhos e coloca cada ponteiro para apontar diretamente até a posição de 12:00. No final do método, o `AnchorY` propriedade é definida para uma posição correspondente para o centro do relógio. Isso indica que o Centro de rotação.

Os ponteiros são girados na função de retorno de chamada timer:

```csharp
public partial class MainPage : ContentPage
{
 
    ···
     
    bool OnTimerTick()
    {
        // Set rotation angles for hour and minute hands.
        DateTime dateTime = DateTime.Now;
        hourHand.Rotation = 30 * (dateTime.Hour % 12) + 0.5 * dateTime.Minute;
        minuteHand.Rotation = 6 * dateTime.Minute + 0.1 * dateTime.Second;

        // Do an animation for the second hand.
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        secondHand.Rotation = 6 * (dateTime.Second + t);
        return true;
    }
}
```

O segundo disponível é tratado de forma diferente: uma animação facilitando a função é aplicada para fazer com que o movimento parecer mecânico, em vez de smooth. Em cada escala segundos extrai um pouco e, em seguida, overshoots seu destino. Essa parte do código adiciona muito realismo do movimento.

## <a name="conclusion"></a>Conclusão

O `BoxView` pode parecer simples em primeiro lugar, mas como você viu, ele pode ser bastante versáteis e podem quase reproduzir visuais que são normalmente possíveis apenas com gráficos vetoriais. Para gráficos mais sofisticados, consulte [usando SkiaSharp em xamarin. Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md).


## <a name="related-links"></a>Links relacionados

- [BoxView básica (exemplo)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)
- [Decoração de texto (exemplo)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)
- [Caixa de listagem de cor (exemplo)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox)
- [Jogo da vida útil (exemplo)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife)
- [Relógio de matriz de ponto (exemplo)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock)
- [Relógio de BoxView (exemplo)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock)
- [BoxView](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)