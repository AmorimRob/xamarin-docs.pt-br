---
title: "Verificação de impressões digitais"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/23/2016
ms.openlocfilehash: 678ceaf122550c6561541533405fe3500d192110
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="scanning-for-fingerprints"></a>Verificação de impressões digitais

Agora que já vimos como preparar um aplicativo xamarin para usar a autenticação de impressão digital, vamos retornar para o `FingerprintManager.Authenticate` método e discutir seu lugar na autenticação de impressão digital Android 6.0. Uma visão geral do fluxo de trabalho para a autenticação de impressão digital é descrita nesta lista:

1. Invocar `FingerprintManager.Authenticate`, passando um `CryptoObject` e um `FingerprintManager.AuthenticationCallback` instância. O `CryptoObject` é usado para garantir que o resultado da autenticação de impressão digital não foi violado. 
2. Subclasse o [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) classe. Uma instância dessa classe será fornecida para `FingerprintManager` quando impressão digital inicia a autenticação. Quando terminar o scanner de impressão digital, ele invocará um dos métodos de retorno de chamada nessa classe.
3. Escreva código para atualizar a interface do usuário para permitir que o usuário sabe que o dispositivo foi iniciado o scanner de impressão digital e está aguardando a interação do usuário. 
4. Quando o scanner de impressão digital é feito, Android retorna resultados para o aplicativo invocando um método no `FingerprintManager.AuthenticationCallback` instância que foi fornecida na etapa anterior.
5. O aplicativo informar ao usuário sobre os resultados de autenticação de impressão digital e reagir aos resultados conforme apropriado. 

O trecho de código a seguir é um exemplo de um método em uma atividade que inicia a verificação de impressões digitais:

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Vamos examinar cada um desses parâmetros no `Authenticate` método um pouco mais detalhadamente:

* O primeiro parâmetro é um _criptografia_ que usem o scanner de impressão digital para ajudar a autenticar os resultados de uma verificação de impressão digital do objeto. Esse objeto pode ser `null`, caso em que o aplicativo precisa confiar cegamente que nada foi violado os resultados de impressão digital. É recomendável que um `CryptoObject` ser instanciados e fornecidas para o `FingerprintManager` em vez de null. [Criando um CryptObject](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) explica detalhadamente como instanciar um `CryptoObject` com base em um `Cipher`.
* O segundo parâmetro é sempre zero. A documentação do Android identifica como o conjunto de sinalizadores e é mais provável é reservada para uso futuro. 
* O terceiro parâmetro, `cancellationSignal` é um objeto usado para desativar o scanner de impressão digital e cancelar a solicitação atual. Este é um [CancellationSignal Android](http://developer.android.com/reference/android/os/CancellationSignal.html)e não é um tipo do .NET framework.
* O quarto parâmetro é obrigatório e é uma classe que as subclasses de `AuthenticationCallback` classe abstrata. Métodos nessa classe serão invocados para sinalizar para clientes quando o `FingerprintManager` foi concluído e quais são os resultados. Como há muito para entender sobre como implementar o `AuthenticationCallback`, será abordada em [é própria seção](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md).
* O quinto parâmetro seja opcional `Handler` instância. Se um `Handler` objeto for fornecido, o `FingerprintManager` usará o `Looper` daquele objeto ao processar as mensagens do hardware de impressão digital. Normalmente, um não precisa fornecer um `Handler`, o FingerprintManager usará o `Looper` do aplicativo.

## <a name="cancelling-a-fingerprint-scan"></a>Cancelar uma verificação de impressão digital

Talvez seja necessário para o usuário (ou o aplicativo) cancelar a verificação de impressão digital depois que ele foi iniciado. Nessa situação, invocar o [ `IsCancelled` ](http://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) método o [ `CancellationSignal` ](http://developer.android.com/reference/android/os/CancellationSignal.html) que foi fornecido para `FingerprintManager.Authenticate` quando ele foi chamado para iniciar a varredura de impressão digital.

Agora que já vimos o `Authenticate` método, vamos examinar alguns dos parâmetros de mais importante em mais detalhes. Primeiro, vamos examinar [responder a chamadas de autenticação](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md), que discutiremos como subclasse o [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html), permitindo que um aplicativo do Android reagir à resultados fornecidos pelo scanner de impressão digital.




## <a name="related-links"></a>Links relacionados

- [CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)