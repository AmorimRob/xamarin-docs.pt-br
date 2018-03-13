---
title: "Arquivo de IPA é 0 bytes"
ms.topic: article
ms.prod: xamarin
ms.assetid: 376BBA27-8694-4E63-9976-BF60349D42D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: e5a1225663020b08bd2d0aa5ae1291e2c044a0ce
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="ipa-file-is-0-bytes"></a>Arquivo de IPA é 0 bytes

> [!IMPORTANT]
> Esse problema foi resolvido em versões recentes do Xamarin. No entanto, se o problema ocorre na versão mais recente do software, envie um [novo bug](~/cross-platform/troubleshooting/questions/howto-file-bug.md) com o controle de versão completo completo e informações de log de saída de compilação.



Ocorreram alguns problemas conhecidos em versões anteriores do Xamarin que pode fazer com que o arquivo de IPA no Windows para ter 0 bytes. 

### <a name="fixed-in-xamarin-for-visual-studio-311584"></a>Corrigido no Xamarin para Visual Studio 3.11.584 
- [Bug 24416 - configuração de compilação "Ad Hoc" na linha de comando faz não copiar IPA arquivo para Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=24416)
- [Bug 24417 - alteração "propriedades do projeto de IPA do iOS -> Opções -> nome do pacote" impede que IPA sendo copiado de volta para o Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=24417)
- [Bug 29822 - definir [XVS.iOS 3.11] "Compilação" número diferente de "Versão" número causas IPA não deve ser copiado para o Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=29822)

### <a name="fixed-in-xamarin-for-visual-studio-410496"></a>Corrigido no Xamarin para Visual Studio 4.1.0.496
- [Bugs 27989 - mostra o arquivo de ipa no servidor de compilação falhar se o nome do Assembly não coincide com o nome do projeto](https://bugzilla.xamarin.com/show_bug.cgi?id=27989)