---
title: Xamarin para Desenvolvedores Java
description: "Caso seja um desenvolvedor de Java, você está no caminho certo para aproveitar as suas habilidades e código existente na plataforma Xamarin, enquanto obtém os benefícios da reutilização de código do C#. Você descobrirá que a sintaxe do C# é muito semelhante à sintaxe do Java e que ambas as linguagens fornecem recursos muito semelhantes. Além disso, você descobrirá recursos exclusivos do C# que facilitarão suas tarefas de desenvolvimento."
ms.topic: article
ms.prod: xamarin
ms.assetid: A3B6C041-4052-4E7D-999C-C4FA10BE3D67
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 240fc917a154085a459c6612f68c8b71e3ef01bb
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/12/2018
---
# <a name="xamarin-for-java-developers"></a>Xamarin para Desenvolvedores Java

_Caso seja um desenvolvedor de Java, você está no caminho certo para aproveitar as suas habilidades e código existente na plataforma Xamarin, enquanto obtém os benefícios da reutilização de código do C#. Você descobrirá que a sintaxe do C# é muito semelhante à sintaxe do Java e que ambas as linguagens fornecem recursos muito semelhantes. Além disso, você descobrirá recursos exclusivos do C# que facilitarão suas tarefas de desenvolvimento._


## <a name="overview"></a>Visão geral

Este artigo oferece uma introdução à programação C# para desenvolvedores Java, com foco nos recursos de linguagem C# que você encontrará ao desenvolver aplicativos do Xamarin.Android. Além disso, este artigo explica como esses recursos diferem das respectivas contrapartes em Java e apresenta importantes recursos de C# (relevantes para Xamarin.Android) que não estão disponíveis em Java. Links para material de referência adicional são incluídos, então você pode usar este artigo como um ponto de partida para estudar o C# e o .NET em mais detalhes.

Se você estiver familiarizado com Java, você se sentirá instantaneamente em casa com a sintaxe do C#. A sintaxe do C# é muito semelhante à sintaxe de Java &ndash; o C# é uma linguagem de "chave" como o Java, o C e o C++. Em muitos aspectos, sintaxe do C# é percebida como um subconjunto da sintaxe de Java, mas com algumas palavras-chave renomeadas e adicionadas.

Muitas das principais características do Java podem ser encontradas em C#:

-   Programação orientada a objeto baseada em classe

-   Tipagem forte

-   Suporte para interfaces

-   Genéricos

-   Coleta de lixo

-   Compilação de tempo de execução

Java e C# são compilados em uma linguagem intermediária que é executada em um ambiente de execução gerenciado. C# e Java são estaticamente tipados e ambas as linguagens tratam cadeias de caracteres como tipos imutáveis.
As duas linguagens usam uma hierarquia de classes de raiz única. Assim como o Java, o C# é compatível somente com a herança única e não permite métodos globais.
Em ambas as linguagens, os objetos são criados no heap usando a palavra-chave `new` e são descartados quando eles não são mais usados. Ambas as linguagens dão suporte a tratamento de exceções com semântica de `try`/`catch`. Ambas dão suporte ao gerenciamento e à sincronização de thread.

No entanto, há várias diferenças entre Java e C#. Por exemplo:

-   Java não é compatível com a variáveis locais de tipo implícito (C# é compatível com a palavra-chave `var`).

-   Em Java, você pode passar parâmetros somente por valor, enquanto, em C#, você pode passá-los por referência e também por valor. (C# fornece as palavras-chave `ref` e `out` para passar parâmetros por referência; não há nenhum equivalente a elas em Java).

-   O Java não é compatível com diretivas de pré-processador como `#define`.

-   O Java não é compatível com tipos de inteiro sem sinal, enquanto o C oferece tipos de inteiro sem sinal como `ulong`, `uint`, `ushort` e `byte`.

-   O Java não é compatível com a sobrecarga de operador; no C#, você pode sobrecarregar operadores e conversões.

-   Em uma instrução `switch` do Java, o código pode passar para a próxima seção da opção, mas em C#, o final de cada seção `switch` deve terminar a opção (o final de cada seção deve ser fechado com uma instrução `break`).

-   Em Java, você especifica as exceções geradas por um método com a palavra-chave `throws`, mas o C# não tem nenhum conceito de exceções verificadas &ndash; a palavra-chave `throws` não é compatível com o C#.

-   O C# é compatível com LINQ (consulta integrada à linguagem), que permite que você use as palavras reservadas `from`, `select` e `where` para gravar consultas a coleções de uma maneira semelhante às consultas de banco de dados.


É claro, há muitas outras diferenças entre C# e Java que podem ser abordadas neste artigo. Além disso, Java e C# continuam a evoluir (por exemplo, Java 8, que ainda não está na cadeia de ferramentas do Android, é compatível com expressões lambda no estilo C#), então essas diferenças sofrerão alterações ao longo do tempo. São descritas aqui apenas as diferenças mais importantes encontradas atualmente por desenvolvedores Java não familiarizados com o Xamarin.Android.

-   [Desde Java até desenvolvimento em C#](#fundamentals) fornece uma introdução às diferenças fundamentais entre C# e Java.

-   [Recursos de programação orientada a objeto](#oopfeatures) descreve as diferenças mais importantes nos recursos orientados a objeto entre as duas linguagens.

-   [Diferenças de palavra-chave](#keywords) fornece uma tabela de equivalentes de palavra-chave úteis, palavras-chave somente para C# e links para definições de palavra-chave do C#.

O C# oferece muitos recursos principais para Xamarin.Android que não estão atualmente disponíveis para desenvolvedores de Java no Android. Esses recursos podem ajudar a escrever código melhor em menos tempo:

-   [Propriedades](#properties) &ndash; com o sistema de propriedades do C#, você pode acessar variáveis de membro com segurança e de modo direto, sem precisar escrever os métodos setter e getter.

-   [Expressões Lambda](#lambdas) &ndash; no C# você pode usar métodos anônimos (também chamado de *lambdas*) para expressar sua funcionalidade de forma mais sucinta e com mais eficiência. Você pode evitar a sobrecarga de precisar gravar objetos que serão usados apenas uma vez, além de poder passar o estado local para um método sem a necessidade de adicionar parâmetros.

-   [Manipulação de eventos](#events) &ndash; o C# oferece suporte no nível de linguagem para *programação controlada por evento*, em que um objeto pode se registrar para ser notificado quando ocorre um evento de interesse. A palavra-chave `event` define um mecanismo de distribuição multicast que uma classe publicadora pode usar para notificar assinantes do evento.

-   [Programação assíncrona](#async) &ndash; os recursos de programação assíncrona do C# (`async`/`await`) mantêm os aplicativos responsivos.
    O suporte no nível de linguagem desse recurso torna a programação assíncrona fácil de implementar e menos propensa a erros.


Além disso, o Xamarin permite [aproveitar ativos existentes do Java](#interop) por meio de uma tecnologia conhecida como *associação*. Você pode chamar seu código, estruturas e bibliotecas do Java existentes do C#, fazendo uso de geradores de associação automática do Xamarin. Para fazer isso, basta criar uma biblioteca estática em Java e expô-la ao C# por meio de uma associação.

<a name="fundamentals" />

## <a name="going-from-java-to-c-development"></a>Passando do Java para o desenvolvimento em C#

As seções a seguir explicam as diferenças de "introdução" básica entre C# e Java; uma seção posterior descreve as diferenças orientadas a objeto entre essas linguagens.



### <a name="libraries-vs-assemblies"></a>Bibliotecas vs. Assemblies

O Java normalmente empacota classes relacionadas em arquivos **.jar**. Em C# e .NET, no entanto, pedaços reutilizáveis de código pré-compilado são empacotados em *assemblies*, que normalmente são empacotados como arquivos *.dll*. Um assembly é uma unidade de implantação para código C#/.NET, sendo que cada assembly é tipicamente associado um projeto C#. Assemblies contêm código intermediário (IL) que é compilado por Just-In-Time no tempo de execução.

Para obter mais informações sobre assemblies, veja o tópico [Assemblies e o cache de assembly global](https://msdn.microsoft.com/en-us/library/ms173099.aspx) do MSDN.


### <a name="packages-vs-namespaces"></a>Pacotes vs. Namespaces

O C# usa a palavra-chave `namespace` para agrupar os tipos relacionados; isso é semelhante à palavra-chave `package` do Java. Normalmente, um aplicativo Xamarin.Android reside em um namespace criado para esse aplicativo. Por exemplo, o código C# a seguir declara o wrapper do namespace `WeatherApp` para um aplicativo de relatório meteorológico:

```csharp
namespace WeatherApp
{
    ...
```


### <a name="importing-types"></a>Importando tipos

Quando você faz uso de tipos definidos em namespaces externos, você importa esses tipos com uma instrução `using` (que é muito semelhante à instrução `import` do Java). Em Java, você pode importar um único tipo com uma instrução semelhante à seguinte:

```java
import javax.swing.JButton
```

Você pode importar um pacote inteiro do Java com uma instrução como esta:

```java
import javax.swing.*
```

A instrução `using` do C# funciona de maneira muito semelhante, mas permite que você importe um pacote inteiro sem especificar um caractere curinga. Por exemplo, você geralmente verá uma série de instruções `using` no início de arquivos de origem do Xamarin.Android, conforme mostrado neste exemplo:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Net;
using System.IO;
using System.Json;
using System.Threading.Tasks;
```

Essas instruções importam a funcionalidade dos namespaces `System`, `Android.App`, `Android.Content`, etc.



### <a name="generics"></a>Genéricos

Tanto Java quanto C# dão suporte a *genéricos*, que são espaços reservados que permitem que você conecte diferentes tipos em tempo de compilação. No entanto, genéricos funcionam de forma ligeiramente diferente em C#. No Java, a [eliminação de tipo](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html) disponibiliza informações de tipo somente em tempo de compilação, mas não em tempo de execução. Por outro lado, o .NET Common Language Runtime (CLR) dá suporte explícito a tipos genéricos, o que significa que o C# tem acesso a informações de tipo em tempo de execução. No desenvolvimento do Xamarin.Android diário, a importância dessa distinção não é aparente na maior parte do tempo, mas se estiver usando [reflexão](https://msdn.microsoft.com/en-us/library/ms173183.aspx), você dependerá desse recurso para acessar as informações de tipo em tempo de execução.

No Xamarin.Android, você geralmente verá o método genérico `FindViewById` usado para obter uma referência a um controle de layout. Esse método aceita um parâmetro de tipo genérico que especifica o tipo de controle para pesquisar. Por exemplo:

```csharp
TextView label = FindViewById<TextView> (Resource.Id.Label);
```

Neste exemplo de código, `FindViewById` obtém uma referência para o controle `TextView`, que é definido no layout como **Label**, depois retorna-o como um tipo `TextView`.

Para obter mais informações sobre os genéricos, veja o tópico [Genéricos](https://msdn.microsoft.com/en-us/library/512aeb7t.aspx) do MSDN.
Observe que há algumas limitações no suporte de Xamarin.Android para classes C# genéricas; Para obter mais informações, consulte [Limitações](~/android/internals/limitations.md).


<a name="oopfeatures" />

## <a name="object-oriented-programming-features"></a>Recursos de programação orientada a objeto

Java e C# usam linguagens de programação orientada a objeto muito semelhantes:

-   Todas as classes são, em última análise, derivadas de um objeto de raiz única &ndash; todos os objetos Java derivam de `java.lang.Object`, enquanto todos os objetos C# derivam de `System.Object`.

-   Instâncias de classes são tipos de referência.

-   Quando você acessa as propriedades e métodos de uma instância, você usa o operador "<code>.</code>".

-   Todas as instâncias de classe são criadas no heap por meio do operador `new`.

-   Já que ambas as linguagens usam a coleta de lixo, é possível liberar explicitamente objetos não utilizados (ou seja, não há uma palavra-chave `delete`, assim como há em C++).

-   Você pode estender as classes por meio de herança, sendo que ambas as linguagens só permitem uma única classe base por tipo.

-   Você pode definir as interfaces e uma classe pode herdar de (ou seja, implementar) várias definições de interface.

No entanto, também há algumas diferenças importantes:

-   O Java tem dois recursos avançados com os quais o C# não é compatível com: classes anônimas e internas. (No entanto, o C# permite o aninhamento de definições de classe &ndash; as classes aninhadas do C# são como classes aninhadas estáticas do Java.)

-   O C# é compatível com tipos de estrutura de estilo C (`struct`), enquanto o Java não.

-   No C#, você pode implementar uma definição de classe em arquivos de origem separados por meio da palavra-chave `partial`.

-   Interfaces C# não podem declarar campos.

-   O C# usa a sintaxe do destruidor de estilo C++ para expressar finalizadores. A sintaxe é diferente do método `finalize` do Java, mas a semântica é praticamente a mesma. (Observe que em C#, os destruidores chamam automaticamente o destruidor da classe base &ndash;, enquanto no Java, utiliza-se uma chamada explícita para `super.finalize`.)



### <a name="class-inheritance"></a>Herança da Classe

Para estender uma classe em Java, você deve usar a palavra-chave `extends`. Para estender uma classe em C#, você usa dois-pontos (`:`) para indicar derivação. Por exemplo, em aplicativos Xamarin.Android, você geralmente verá derivações da classe parecidas com o fragmento de código a seguir:

```csharp
public class MainActivity : Activity
{
    ...
```

Neste exemplo, `MainActivity` herda da classe `Activity`.

Para declarar suporte para uma interface em Java, você usa a palavra-chave `implements`. No entanto, em C#, você simplesmente adiciona nomes de interface à lista de classes da qual herdar, conforme mostrado neste fragmento de código:

```csharp
public class SensorsActivity : Activity, ISensorEventListener
{
    ...
```

Neste exemplo, `SensorsActivity` herda de `Activity` e implementa a funcionalidade declarada na interface `ISensorEventListener`. Observe que a lista de interfaces deve vir após a classe base (ou você obterá um erro em tempo de compilação). Por convenção, os nomes de interface C# são prefixados com letras maiúsculas "I". Com isso, é possível determinar quais classes são interfaces sem a necessidade de uma palavra-chave `implements`.

Quando você quiser impedir que uma classe seja dividida em mais subclasses em C#, você precederá o nome de classe com `sealed` &ndash; em Java, você precederá o nome da classe com `final`.

Para mais informações sobre definições de classe do C#, veja os tópicos [Classes e estruturas](https://msdn.microsoft.com/en-us/library/x9afc042.aspx) e [Herança](https://msdn.microsoft.com/en-us/library/x9afc042.aspx) do MSDN.


<a name="properties" />

### <a name="properties"></a>Propriedades

Em Java, métodos modificadores (setters) e inspetores (getters) geralmente são usados para controlar como as alterações são feitas em membros de classe, simultaneamente ocultando e protegendo esses membros do código externo. Por exemplo, a classe `TextView` do Android fornece os métodos `getText` e `setText`. O C# fornece um mecanismo semelhante, mas mais direto, conhecido como *propriedades*.
Os usuários de uma classe C# podem acessar uma propriedade da mesma maneira como eles acessariam um campo, mas cada acesso na verdade resulta em uma chamada de método que é transparente para o chamador. Esse método "nos bastidores" pode implementar efeitos colaterais, tais como a configuração de outros valores, a execução de conversões ou a alteração do estado do objeto.

Propriedades geralmente são usadas para acessar e modificar os membros do objeto de IU (interface do usuário). Por exemplo:

```csharp
int width = rulerView.MeasuredWidth;
int height = rulerView.MeasuredHeight;
...
rulerView.DrawingCacheEnabled = true;
```

Neste exemplo, valores de largura e altura são lidos do objeto `rulerView`, acessando suas propriedades `MeasuredWidth` e `MeasuredHeight`. Quando essas propriedades são lidas, os valores de seus valores de campo associados (mas ocultos) são buscados em segundo plano e retornados ao chamador. O objeto `rulerView` pode armazenar valores de largura e altura em uma unidade de medida (digamos, pixels) e converter esses valores rapidamente para outra unidade de medida (digamos, milímetros) quando as propriedades `MeasuredWidth` e `MeasuredHeight` são acessadas.

O objeto `rulerView` também tem uma propriedade chamada `DrawingCacheEnabled` &ndash; o código de exemplo define essa propriedade como `true` para habilitar o cache de desenho no `rulerView`. Nos bastidores, um campo oculto associado é atualizado com o novo valor e possivelmente outros aspectos do estado `rulerView` são modificados. Por exemplo, quando `DrawingCacheEnabled` é definido como `false`, `rulerView` também pode apagar qualquer informação de cache desenho já acumulada no objeto.

O acesso às propriedades pode ser leitura/gravação, somente leitura ou somente gravação. Além disso, você pode usar modificadores de acesso diferentes para leitura e gravação. Por exemplo, você pode definir uma propriedade que tem acesso de leitura público, mas acesso de gravação privado.

Para obter mais informações sobre propriedades do C#, veja o tópico [Propriedades](https://msdn.microsoft.com/en-us/library/x9fsa0sw.aspx) do MSDN.



### <a name="calling-base-class-methods"></a>Chamar métodos de classe base

Para chamar um construtor de classe base em C#, você usa dois-pontos (`:`), seguidos da palavra-chave `base` e de uma lista de inicializadores; essa chamada de construtor `base` é posicionada imediatamente após a lista de parâmetros do construtor derivado. O construtor de classe base é chamado na entrada para o construtor derivado; o compilador insere a chamada para o construtor base no início do corpo do método. O fragmento de código a seguir ilustra um construtor base chamado de um construtor derivado em um aplicativo Xamarin.Android:

```csharp
public class PictureLayout : ViewGroup
{
    ...
    public class PictureLayout (Context context)
           : base (context)
    {
        ...
    }
    ...
}

```

Nesse exemplo, a classe `PictureLayout` é derivada da classe `ViewGroup`. O construtor `PictureLayout` mostrado neste exemplo aceita um argumento `context` e transmite-o para o construtor `ViewGroup` por meio da chamada `base(context)`.

Para chamar um método de classe base em C#, use a palavra-chave `base`. Por exemplo, aplicativos do Xamarin.Android geralmente fazem chamadas para métodos de base, conforme mostrado aqui:

```csharp
public class MainActivity : Activity
{
    ...
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
```

Nesse caso, o método `OnCreate` definido pela classe derivada (`MainActivity`) chama o método `OnCreate` da classe base (`Activity`).



### <a name="access-modifiers"></a>Modificadores de acesso

Java e C# dão suporte aos modificadores de acesso `public`, `private` e `protected`. No entanto, o C# é compatível com dois modificadores de acesso adicionais:

-   **`internal`** &ndash; O membro de classe é acessível somente dentro do assembly atual.

-   **`protected internal`** &ndash; O membro de classe é acessível dentro do assembly de definição, da classe de definição e de classes derivadas (tanto as classes derivadas dentro do assembly quanto as derivadas fora dele têm acesso).

Para obter mais informações sobre os modificadores de acesso do C#, veja o tópico [Modificadores de acesso](https://msdn.microsoft.com/en-us/library/ms173121.aspx) do MSDN.



### <a name="virtual-and-override-methods"></a>Métodos de substituição e virtuais

Tanto Java quanto C# dão suporte a *polimorfismo*, a capacidade de tratar objetos relacionados da mesma maneira. Em ambos os idiomas, você pode usar uma referência de classe base para se referir a um objeto de classe derivada, e os métodos de uma classe derivada podem substituir os métodos das classes base desse objeto. Ambas as linguagens têm o conceito de um método *virtual*, um método em uma classe base que é projetado para ser substituído por um método em uma classe derivada.
Assim como o Java, o C# é compatível com classes e métodos `abstract`.

No entanto, há algumas diferenças entre o Java e o C# no modo de declarar métodos virtuais e substituí-los:

-   No C#, os métodos são não virtuais por padrão. Classes pai devem identificar explicitamente quais métodos devem ser substituídos usando a palavra-chave `virtual`. Por outro lado, todos os métodos no Java são métodos virtuais por padrão.

-   Para impedir que um método que seja substituído no C#, você simplesmente omite a palavra-chave `virtual`. Por outro lado, o Java usa a palavra-chave `final` para marcar um método com "substituição não permitida".

-   Classes derivadas de C# devem usar a palavra-chave `override` para indicar explicitamente que um método de classe base virtual está sendo substituído.

Para obter mais informações sobre o suporte do C# para polimorfismo, veja o tópico [Polimorfismo](https://msdn.microsoft.com/en-us/library/ms173152.aspx) do MSDN.


<a name="lambdas" />

## <a name="lambda-expressions"></a>Expressões lambda

O C# possibilita criar *fechamentos*: métodos anônimos embutidos que podem acessar o estado do método no qual eles estão embutidos.
Usando expressões lambda, você pode escrever menos linhas de código para implementar a mesma funcionalidade que você poderia ter implementado em Java com muito mais linhas de código.

Expressões lambda possibilitam que você ignore a cerimônia extra envolvida na criação de uma classe de uso único ou classe anônima, que você usaria no Java &ndash; em vez disso, você pode apenas gravar a lógica de negócios de seu código de método embutida. Além disso, já que lambdas têm acesso às variáveis no método ao redor, você não precisa criar uma longa lista de parâmetros para passar o estado para o código do método.

Em C#, expressões lambda são criadas com o operador `=>`, conforme mostrado aqui:

```csharp
(arg1, arg2, ...) => {
    // implementation code
};
```

No Xamarin.Android, expressões lambda geralmente são usadas para definir manipuladores de eventos. Por exemplo:

```csharp
button.Click += (sender, args) => {
    clickCount += 1;    // access variable in surrounding code
    button.Text = string.Format ("Clicked {0} times.", clickCount);
};
```

Neste exemplo, o código de expressão lambda (o código entre chaves) incrementa a contagem de cliques e atualiza o texto `button` para exibir a contagem de cliques. Essa expressão lambda é registrada com o objeto `button` como um manipulador de eventos a ser chamado sempre que o botão é tocado. (Manipuladores de eventos são explicados mais detalhadamente abaixo.) Neste exemplo simples, os parâmetros `sender` e `args` não são usados pelo código de expressão lambda, mas eles são necessários na expressão lambda para atender aos requisitos de assinatura de método para registro de evento. Nos bastidores, o compilador de C# converte a expressão lambda em um método anônimo que é chamado sempre eventos de clique em botão ocorrem.

Para obter mais informações sobre C# e expressões lambda, veja o tópico [Expressões lambda](https://msdn.microsoft.com/en-us/library/bb397687.aspx).


<a name="events" />

## <a name="event-handling"></a>Tratamento de Evento

Um *evento* é uma forma de um objeto notificar assinantes registrados quando algo interessante acontece para esse objeto. Ao contrário do Java, em que um assinante normalmente implementa uma interface `Listener` que contém um método de retorno de chamada, o C# é compatível, no nível de linguagem, com a manipulação de eventos por meio de *delegados*. Um *delegado* é como um ponteiro de função fortemente tipado e orientado a objeto &ndash; ele encapsula uma referência de objeto e um token de método. Se um objeto cliente deseja assinar um evento, ele cria um delegado e passa-o para o objeto que realiza a notificação.
Quando o evento ocorre, o objeto que realiza a notificação invoca o método representado pelo objeto delegado, fornecendo uma notificação sobre o evento ao objeto cliente de assinatura. No C#, os manipuladores de eventos nada mais são do que métodos chamados por meio de delegados.

Para obter mais informações sobre delegados, consulte o tópico [Delegados](https://msdn.microsoft.com/en-us/library/ms173171.aspx) do MSDN.

Em C#, os eventos são *multicast*; ou seja, mais de um ouvinte pode ser notificado quando um evento ocorre. Essa diferença é observada ao considerar as diferenças sintáticas entre o registro de evento do Java e do C#. Em Java, você chama `SetXXXListener` para se registrar para notificações de eventos; no C#, você usa o operador `+=` para se registrar para notificações de eventos adicionando o delegado à lista de ouvintes de eventos.
Em Java, você chama `SetXXXListener` para cancelar o registro, enquanto no C# você usa o `-=` para remover seu delegado da lista de ouvintes.

No Xamarin.Android, os eventos são geralmente usados para notificar os objetos quando um usuário faz algo para um controle de interface do usuário. Normalmente, um controle de interface do usuário tem membros definidos usando a palavra-chave `event`; você anexa seus delegados a esses membros para assinar eventos desse controle da interface do usuário.

Para assinar um evento:

1.  Crie um objeto delegado que se refere ao método que você deseja que seja invocado quando o evento ocorre.

2.  Use o operador `+=` para anexar o delegado ao evento que você está assinando.

O exemplo a seguir define um delegado (com um uso explícito da palavra-chave `delegate`) para assinar para cliques de botão.
Este manipulador de cliques de botão inicia uma nova atividade:

```csharp
startActivityButton.Click += delegate {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

No entanto, você também pode usar uma expressão lambda para registrar eventos, ignorando a palavra-chave `delegate` por completo. Por exemplo:

```csharp
startActivityButton.Click += (sender, e) => {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

Neste exemplo, o objeto `startActivityButton` tem um evento que espera um delegado com uma determinada assinatura de método: uma que aceita argumentos de evento e de remetente e retorna nulo. No entanto, já que não queremos passar pelo incômodo de definir explicitamente um delegado desse tipo ou o respectivo método, podemos declarar a assinatura do método com `(sender, e)` e usar uma expressão lambda para implementar o corpo do manipulador de eventos.
Observe que teremos para declarar essa lista de parâmetros, mesmo se nós não estivermos usando os parâmetros `sender` e `e`.

É importante lembrar que você pode cancelar a assinatura de um delegado (por meio do operador `-=`), mas você não pode cancelar a assinatura de uma expressão lambda &ndash; tentar fazer isso pode causar vazamentos de memória. Use o formulário lambda de registro de eventos somente quando constatar que o manipulador não cancela a assinatura do evento.

Normalmente, as expressões lambda são usadas para declarar manipuladores de eventos no código do Xamarin.Android. Essa forma abreviada de declarar manipuladores de eventos pode inicialmente parecer incompreensível, mas economiza uma enorme quantidade de tempo quando você está lendo e gravando código. Com o aumento da familiaridade, você se acostuma a reconhecer esse padrão (o que ocorre com frequência no código do Xamarin.Android) e você gasta mais tempo pensando sobre a lógica de negócios do seu aplicativo e menos tempo com a dificuldade de lidar com sobrecarga sintática.


<a name="async" />

## <a name="asynchronous-programming"></a>Programação assíncrona

*Programação assíncrona* é uma maneira de melhorar a capacidade de resposta geral do seu aplicativo. Recursos de programação assíncrona possibilitam que o restante do código do aplicativo continue em execução enquanto alguma parte do seu aplicativo está bloqueada por uma operação demorada.
Acessando a web, processar imagens e ler/gravar arquivos são exemplos de operações que podem fazer com que um aplicativo inteiro pareça congelado se não é escrito de forma assíncrona.

O C# inclui suporte no nível de linguagem para programação assíncrona por meio das palavras-chave `async` e `await`. Esses recursos de idioma facilitam muito a escrita de código que executa tarefas de execução longa, sem bloquear o thread principal de seu aplicativo. Em resumo, você usa a palavra-chave `async` em um método para indicar que o código no método é executado de forma assíncrona e não bloquear o thread do chamador. Você usa a palavra-chave `await` quando você chama métodos marcados com `async`. O compilador interpreta o `await` como o ponto em que a execução do método deve ser movida para um thread em segundo plano (uma tarefa é retornada ao chamador). Quando essa tarefa é concluída, a execução do código retoma no thread do chamador no ponto `await` no seu código, retornando os resultados da chamada `async`. Por convenção, os métodos que são executados de forma assíncrona têm o sufixo `Async` em seus nomes.

Em aplicativos Xamarin.Android, `async` e `await` normalmente são usados para liberar o thread de interface do usuário para que ele possa responder à entrada do usuário (como o toque em um botão **Cancelar**) enquanto uma operação de execução longa ocorre em uma tarefa em segundo plano.

No exemplo a seguir, um manipulador de eventos de clique de botão faz com que uma operação assíncrona baixe uma imagem da Web:


```csharp
downloadButton.Click += downloadAsync;
...
async void downloadAsync(object sender, System.EventArgs e)
{
    webClient = new WebClient ();
    var url = new Uri ("http://photojournal.jpl.nasa.gov/jpeg/PIA15416.jpg");
    byte[] bytes = null;

    bytes = await webClient.DownloadDataTaskAsync(url);

    // display the downloaded image ...
```

Neste exemplo, quando o usuário clica no controle `downloadButton`, o manipulador de eventos `downloadAsync` cria um objeto `WebClient` e um objeto `Uri` para buscar uma imagem da URL especificada. Em seguida, ele chama o método `DownloadDataTaskAsync` do objeto `WebClient` com essa URL para recuperar a imagem.

Observe que a declaração de método de `downloadAsync` é precedida pela palavra-chave `async` para indicar que ele será executado de forma assíncrona e retornará uma tarefa. Observe também que a chamada para `DownloadDataTaskAsync` é precedida pela palavra-chave `await`. O aplicativo move a execução do manipulador de eventos (começando no ponto em que `await` aparece) para um thread em segundo plano até `DownloadDataTaskAsync` ser concluído e retornar.
Enquanto isso, o thread de interface do usuário do aplicativo ainda pode responder à entrada do usuário e acionar manipuladores de eventos para os outros controles. Quando `DownloadDataTaskAsync` é concluído (o que pode levar vários segundos), a execução continua até a variável `bytes` ser definida como o resultado da chamada para `DownloadDataTaskAsync`, e o restante do código de manipulador de eventos exibe a imagem baixada no thread do chamador (interface do usuário).

Para uma introdução a `async`/`await` em C#, veja o tópico [Programação assíncrona com async e await](https://msdn.microsoft.com/en-us/library/hh191443.aspx) do MSDN.
Para obter mais informações sobre o suporte de Xamarin de recursos de programação assíncrona, consulte [visão geral de suporte assíncrono](~/cross-platform/platform/async.md).


<a name="keywords" />

## <a name="keyword-differences"></a>Diferenças em palavras-chave

Muitas palavras-chave usadas em Java também são usadas em C#. Também há um número de palavras-chave do Java que têm uma contraparte equivalente, mas de nome diferente em C#, conforme listado nesta tabela:

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
    <th>
        <strong>Java</strong>
    </th>
    <th>
        <strong>C#</strong>
    </th>
    <th>
        <strong>Descrição</strong>
    </th>
  </thead>
  <tbody>
    <tr>
      <td valign="top">
        <code>boolean</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/c8f5xwh7.aspx">bool</a>
      </td>
      <td valign="top">
Usado para declarar os valores boolianos <code>true</code> e <code>false</code>.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>extends</code>
      </td>
      <td valign="top">
        <code>:</code>
      </td>
      <td valign="top">
Precede a classe e as interfaces das quais herdar.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>implements</code>
      </td>
      <td valign="top">
        <code>:</code>
      </td>
      <td valign="top">
Precede a classe e as interfaces das quais herdar.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>import</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/zhdeatwt.aspx">usando</a>
      </td>
      <td valign="top">
Importa tipos de um namespace, também é usado para criar um alias de namespace.
      </td>
    </tr>
    <tr>
      <td valign="final">
        <code>final</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/88c54tsw.aspx">sealed</a>
      </td>
      <td valign="top">
Impede a derivação de classe; impede que métodos e propriedades sejam substituídos em classes derivadas.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>instanceof</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/scekt9xw.aspx">is</a>
      </td>
      <td valign="top">
Avalia se um objeto é compatível com um determinado tipo.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>native</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/e59b22c5.aspx">extern</a>
      </td>
      <td valign="top">
Declara um método que é implementado externamente.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>package</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/z2kcy19k.aspx">namespace</a>
      </td>
      <td valign="top">
Declara um escopo para um conjunto relacionado de objetos.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>T...</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/w5zay9db.aspx">params T[]</a>
      </td>
      <td valign="top">
Especifica um parâmetro do método que aceita um número variável de argumentos.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <code>super</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/hfw7t1ce.aspx">base</a>
      </td>
      <td valign="top">
Usado para acessar membros da classe pai de dentro de uma classe derivada.
      </td>
    </tr>
    <tr>
      <td valign="synchronized">
        <code>synchronized</code>
      </td>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/c5kehkcz.aspx">lock</a>
      </td>
      <td valign="top">
Encapsula uma seção crítica do código com a aquisição e a liberação de bloqueio.
      </td>
    </tr>
  </tbody>
</table>


Além disso, há muitas palavras-chave que são exclusivas do C# e não têm uma correspondência no Java. O código do Xamarin.Android geralmente usa as seguintes palavras-chave do C# (esta tabela é útil para referência ao ler o [exemplo de código](https://developer.xamarin.com/samples/android/all/) do Xamarin.Android):


<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
    <th>
        <strong>C#</strong>
    </th>
    <th>
        <strong>Descrição</strong>
    </th>
  </thead>
  <tbody>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/cscsdfbt.aspx">as</a>
      </td>
      <td valign="top">
Executa conversões entre tipos de referência compatíveis ou tipos anuláveis.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/hh156513.aspx">async</a>
      </td>
      <td valign="top">
Especifica que uma expressão lambda ou método é assíncrono.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/hh156528.aspx">await</a>
      </td>
      <td valign="top">
Suspende a execução de um método até que uma tarefa seja concluída.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/5bdb6693.aspx">byte</a>
      </td>
      <td valign="top">
Tipo inteiro de 8 bits sem sinal.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/900fyy8e.aspx">delegate</a>
      </td>
      <td valign="top">
Usado para encapsular um método ou um método anônimo.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/sbbt4032.aspx">enum</a>
      </td>
      <td valign="top">
Declara uma enumeração, um conjunto de constantes nomeadas.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/8627sbea.aspx">event</a>
      </td>
      <td valign="top">
Declara um evento em uma classe do publicador.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/f58wzh21.aspx">fixed</a>
      </td>
      <td valign="top">
Impede que uma variável seja realocada.
      </td>
    </tr>
    <tr>
      <td valign="top">
get </td>
      <td valign="top">
Define um método acessor que recupera o valor de uma propriedade.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/dd469484.aspx">in</a>
      </td>
      <td valign="top">
Permite que um parâmetro aceite um tipo menos derivado em uma interface genérica.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/9kkx3h3c.aspx">object</a>
      </td>
      <td valign="top">
Um alias para o tipo <code>Object</code> no .NET Framework.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/t3c3bfhx.aspx">out</a>
      </td>
      <td valign="top">
Modificador de parâmetro ou declaração de parâmetro de tipo genérico.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/ebca9ah3.aspx">override</a>
      </td>
      <td valign="top">
Estende ou modifica a implementação de um membro herdado.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/6b0scde8.aspx">partial</a>
      </td>
      <td valign="top">
Declara uma definição a ser dividida em vários arquivos ou separa uma definição de método de sua implementação.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/acdd6hb7.aspx">readonly</a>
      </td>
      <td valign="top">
Declara que um membro de classe pode ser atribuído somente no momento da declaração ou pelo construtor de classe.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/14akc2c7.aspx">ref</a>
      </td>
      <td valign="top">
Faz com que um argumento seja passado por referência, em vez de por valor.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/ms228368.aspx">set</a>
      </td>
      <td valign="top">
Define um método acessor que define o valor de uma propriedade.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/362314fe.aspx">string</a>
      </td>
      <td valign="top">
Alias para o tipo <code>String</code> no .NET Framework.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/ah19swz4.aspx">struct</a>
      </td>
      <td valign="top">
Um tipo de valor que encapsula um grupo de variáveis relacionadas.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/58918ffs.aspx">typeof</a>
      </td>
      <td valign="top">
Obtém o tipo de um objeto.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/bb383973.aspx">var</a>
      </td>
      <td valign="top">
Declara uma variável local de tipo implícito.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/a1khb4f8.aspx">value</a>
      </td>
      <td valign="top">
Referencia o valor que o código do cliente deseja atribuir a uma propriedade.
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="https://msdn.microsoft.com/en-us/library/9fkccyh4.aspx">virtual</a>
      </td>
      <td valign="top">
Permite que um método seja substituído em uma classe derivada.
      </td>
    </tr>
  </tbody>
</table>


<a name="interop" />

## <a name="interoperating-with-existing-java-code"></a>Interoperação com código Java existente

Se você tem a funcionalidade existente do Java que não deseja converter em C#, você pode reutilizar as bibliotecas Java existentes em aplicativos Xamarin.Android por meio de duas técnicas:

-  **Criar uma biblioteca de associações de Java** &ndash; usando essa abordagem, você usa ferramentas Xamarin para gerar wrappers C# em torno de tipos Java. Esses wrappers são chamados *associações*. Como resultado, seu aplicativo xamarin pode usar o arquivo *.jar* chamando esses wrappers.

-  **Interface nativa Java** &ndash; a JNI (*Interface nativa Java*) é uma estrutura que permite que aplicativos C# chamem ou sejam chamados por código Java.

Para obter mais informações sobre essas técnicas, consulte [Visão geral da integração Java](~/android/platform/java-integration/index.md).



## <a name="for-further-reading"></a>Para leitura adicional

O [Guia de programação em C#](https://msdn.microsoft.com/en-us/library/67ef8sbd.aspx) do MSDN é uma ótima maneira de começar a aprender a linguagem de programação C#, e você pode usar a [Referência de C#](https://msdn.microsoft.com/en-us/library/618ayhy6.aspx) para pesquisar recursos específicos da linguagem C#.

Da mesma forma que o conhecimento de Java trata-se tanto de familiaridade com as bibliotecas de classes Java quanto de conhecer a linguagem Java propriamente dita, o conhecimento prático do C# requer alguma familiaridade com o .NET Framework. O pacote de aprendizado [Migrar para C# e o .NET Framework, para desenvolvedores de Java](https://www.microsoft.com/en-us/download/details.aspx?id=6073) da Microsoft é uma boa maneira de saber mais sobre o .NET Framework de uma perspectiva de Java (proporcionando também uma compreensão mais profunda do C#).

Quando você estiver pronto para lidar com o seu primeiro projeto do Xamarin.Android em C#, nossa série [Hello, Android](~/android/get-started/hello-android/index.md) pode lhe ajudar a criar seu primeiro aplicativo Xamarin.Android e avançar ainda mais no seu entendimento dos fundamentos de desenvolvimento de aplicativos Android com o Xamarin.



## <a name="summary"></a>Resumo

Este artigo fornece uma introdução para o ambiente de programação C# do Xamarin.Android ponto de vista de um desenvolvedor Java. Ele indica as semelhanças entre C# e Java, explicando suas diferenças práticas. Ele apresenta assemblies e namespaces, explica como importar tipos externos e fornece uma visão geral das diferenças em modificadores de acesso, genéricos, derivação de classe, chamada de métodos de classe base, substituição de métodos e manipulação de eventos. Ele apresenta recursos C# que não estão disponíveis em Java, tais como propriedades, programação assíncrona de `async`/`await`, lambdas, delegados C# e o sistema de manipulação de eventos do C#. Ele inclui tabelas de palavras-chave importantes do C#, explica como interoperar com bibliotecas Java existentes e fornece links para documentação relacionada para estudo posterior.


## <a name="related-links"></a>Links relacionados

- [Visão geral da integração Java](~/android/platform/java-integration/index.md)
- [Guia de Programação em C#](https://msdn.microsoft.com/en-us/library/67ef8sbd.aspx)
- [Referência de C#](https://msdn.microsoft.com/en-us/library/618ayhy6.aspx)
- [Migrar para C# e o .NET Framework para desenvolvedores Java](https://www.microsoft.com/en-us/download/details.aspx?id=6073)