---
title: Alternar
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 0cf1c4d749eb85a7e0f4c035e10e2e7a40e0c711
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/09/2018
---
# <a name="switch"></a>Alternar

O `Switch` widget (mostrada abaixo) permite que um usuário alternar entre os dois estados, tais como ativo ou desativado. O `Switch` valor padrão é OFF. Em seus estados ON e OFF, o widget é mostrado abaixo:

[![Capturas de tela de um widget de comutador e Estados](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>Criar um comutador

Para criar um comutador, simplesmente declare um `Switch` elemento no XML da seguinte maneira:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

Isso cria um comutador básico, conforme mostrado abaixo:

[![Captura de tela do aplicativo de demonstração exibindo um comutador no estado OFF](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>Alterando os valores padrão

O texto que o controle exibe para o ON e OFF estados e o valor padrão são configuráveis. Por exemplo, para que o comutador padrão como ON e ler Sim/Não, em vez de ligar/desligar, podemos definir o `checked`, `textOn`, e `textOff` atributos no XML a seguir.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```



## <a name="providing-a-title"></a>Fornecendo um título

O `Switch` widget também com suporte, incluindo um rótulo de texto, definindo o `text` atributo da seguinte maneira:

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Essa marcação produz a seguinte captura de tela em tempo de execução:

[![Captura de tela do aplicativo de demonstração com texto horizontal antecede o widget de comutador](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

Quando um `Switch`de alterações de valor, ele gera um `CheckedChange` eventos.
Por exemplo, no código a seguir, podemos capturar esse evento e apresentar um `Toast` widget com uma mensagem com base no `isChecked` valor `Switch`, que é passado para o manipulador de eventos como parte do `CompoundButton.CheckedChangeEventArg` argumento.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>Links relacionados

- [SwitchDemo (exemplo)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/SwitchDemo/)
- [Guia Layout Tutorial](~/android/user-interface/layouts/tab-layout/index.md)
- [Introdução ao sabor de sorvete Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Plataforma 4.0 Android](http://developer.android.com/sdk/android-4.0.html)