---
title: "Pode atualizar o modelo de padrão de xamarin. Forms a um pacote NuGet mais recente?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: c626e8b4a01a55fac5d2c07f0c511241056c2774
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Pode atualizar o modelo de padrão de xamarin. Forms a um pacote NuGet mais recente?

Este guia usa o modelo xamarin. Forms PCL como um exemplo, mas o mesmo método geral também funcionará para o modelo de projeto compartilhado do xamarin. Forms. Este guia foi escrito com o exemplo de atualização do xamarin. Forms 1.5.1.6471 para 2.1.0.6529, mas as mesmas etapas são possíveis definir outras versões como padrão em vez disso.

1.  Copie o modelo original `.zip` de:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2.  Descompacte o `.zip` para um local temporário.

3.  Altere todas as ocorrências da antiga versão do pacote de formulários para a nova versão que você deseja usar.
    *   `FormsTemplate\FormsTemplate.vstemplate`
    *   `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    *   `FormsTemplate.WinPhone\FormsTemplate.WinPhone.vstemplate`
    *   `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    Exemplo: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4.  Altere o elemento "nome" da janela principal [arquivo de modelo de multiprojeto](http://msdn.microsoft.com/library/ms185308.aspx) (`Xamarin.Forms.PCL.vstemplate`) para torná-lo exclusivo. Por exemplo:
    > <Name>Aplicativo em branco (xamarin. Forms portátil) - 2.1.0.6529</Name>

5.  Novamente, compacte a pasta de modelo inteiro. Certifique-se de corresponder à estrutura do arquivo original do `.zip` arquivo. O `Xamarin.Forms.PCL.vstemplate` arquivo deve estar no topo do `.zip` arquivo, não em todas as pastas.

6.  Crie um subdiretório de "Aplicativos móveis" na sua pasta de modelos do Visual Studio por usuário:
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7.  Copie a pasta do novo modelo de backup compactado para a nova pasta de "Aplicativos móveis".

8.  Baixe o pacote do NuGet que corresponde à versão da etapa 3. Por exemplo, [http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529](http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (Consulte também [http://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file](http://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file)) e copie-o para na subpasta apropriada da pasta de extensões Xamarin Visual Studio:
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`