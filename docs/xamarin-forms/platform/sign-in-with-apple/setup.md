---
title: Etapas de instalação-entrar com a Apple para Xamarin. Forms
description: Entrar com a configuração da Apple difere dependendo das diferentes plataformas que seu aplicativo móvel tem como destino.
ms.prod: xamarin
ms.assetid: 8F712802-395B-469B-B5BE-C927AD1A8391
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 09/10/2019
ms.openlocfilehash: c46da6b4ed877cc85f98e6ef0ab2b9a28d811723
ms.sourcegitcommit: cf56d2bae34dc0f8e94c2d3d28d5f460d59807bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70986005"
---
# <a name="setup-sign-in-with-apple-for-xamarinforms"></a>Entrar com a Apple para Xamarin. Forms

![Esta API está atualmente em visualização](~/media/shared/preview.png)

Este guia aborda a série de etapas necessárias para configurar seus aplicativos de plataforma cruzada para que sejam avançados de entrar com a Apple. Embora a configuração da Apple seja direta no portal do desenvolvedor da Apple, etapas adicionais são necessárias para criar uma relação segura entre o Android e a Apple. 

## <a name="apple-developer-setup"></a>Instalação do desenvolvedor da Apple

Antes de usar a entrada com a Apple em seus aplicativos, você precisará abordar algumas etapas de instalação na seção [certificados, identificadores & perfis](https://developer.apple.com/account/resources/) do portal do desenvolvedor da Apple.

### <a name="apple-sign-in-domain"></a>Domínio de entrada da Apple

Registre seu nome de domínio e verifique-o com a Apple na seção [mais](https://developer.apple.com/account/resources/services/list) da seção *certificados, identificadores & perfis* .

![Mais seções](sign-in-images/readme-signin-domain-configure.png)

Adicione seu domínio e clique em **registrar**.

![formulário de registro de domínio](sign-in-images/readme-signin-domain-more.png)

> [!NOTE]
> Se você vir um erro sobre o seu domínio não estar em conformidade com o SPF, você precisará adicionar um registro TXT do DNS do SPF ao seu domínio e esperar que ele seja propagado antes de continuar: O SPF TXT pode ser semelhante a este:`v=spf1 a a:myapp.com -all`

Em seguida, você precisará verificar a propriedade do domínio clicando em **baixar** para recuperar `apple-developer-domain-association.txt` o arquivo e `.well-known` carregá-lo na pasta do site do seu domínio.

Depois que `.well-known/apple-developer-domain-association.txt` o arquivo for carregado e acessível, você poderá clicar em **verificar** para que a Apple Verifique sua propriedade de domínio.

> [!NOTE]
> A Apple verificará a `https://`Propriedade com. Verifique se você tem a instalação do SSL e se o arquivo está acessível por meio de uma URL segura.

Este processo foi concluído com êxito antes de continuar.

## <a name="setup-your-app-id"></a>Configurar sua ID do aplicativo

Na seção [identificadores](https://developer.apple.com/account/resources/identifiers/list) , crie um novo identificador e escolha IDs de **aplicativo**. Se você já tiver uma ID do aplicativo, escolha editá-la em vez disso.

![Criar uma nova ID do aplicativo](sign-in-images/readme-appid-create.png)

Habilite **a entrada com a Apple**. Provavelmente, você desejará usar a opção **habilitar como ID do aplicativo primário** .

![Habilitar a entrada com a Apple](sign-in-images/readme-appid-signin.png)

Salve as alterações de ID do aplicativo.

## <a name="create-a-service-id"></a>Criar uma ID de serviço

Na seção [identificadores](https://developer.apple.com/account/resources/identifiers/list/serviceId) , crie um novo identificador e escolha IDs de **serviço**.

![Criar uma nova ID de serviço](sign-in-images/readme-serviceid-create.png)

Dê à sua ID de serviços uma descrição e um identificador.  Esse identificador será seu `ServerId`.  Certifique-se de habilitar a **entrada com a Apple**.

Antes de continuar, clique em **Configurar** ao lado da opção _entrar com a Apple_ que você habilitou.

No painel de configuração, verifique se a **ID correta do aplicativo primário** está selecionada.

Em seguida, escolha o **domínio da Web** que você configurou anteriormente.

Por fim, adicione uma ou mais **URLs de retorno**.  Qualquer `redirect_uri` um que você use posteriormente deve ser registrado aqui exatamente como você o usa.  Certifique-se de incluir `http://` o `https://` ou na URL ao inseri-lo.

> [!NOTE]
> Para fins de teste, não é `127.0.0.1` possível `localhost`usar o ou o `local.test`, mas você pode usar outros domínios, como.  Se você optar por fazer isso, poderá editar o arquivo do `hosts` computador para resolver esse domínio fictício para seu endereço IP local.

![Configurar sua entrada na Apple](sign-in-images/readme-serviceid-configure.png)

Salve suas alterações quando terminar.

## <a name="create-a-key-for-your-services-id"></a>Criar uma chave para sua ID de serviços

Na seção [chaves](https://developer.apple.com/account/resources/authkeys/list) , crie uma nova **chave**.

Dê um nome à sua chave e habilite a **entrada com a Apple**.

![Criar uma nova chave](sign-in-images/readme-key-create.png)

Clique em **Configurar** ao lado de _entrar com a Apple_.

Verifique se a **ID do aplicativo primário** correta está selecionada e clique em **salvar**.

Clique em **continuar** e **Registre-se** para criar a nova chave.

Em seguida, você terá apenas uma oportunidade de baixar a chave que acabou de gerar.  Clique em **Baixar**.

![Chave de download](sign-in-images/readme-key-download.png)

Além disso, anote sua **ID de chave** nesta etapa. Isso será usado para o `KeyId` mais tarde.

Você terá baixado um arquivo `.p8` de chave.  Você pode abrir esse arquivo no bloco de notas ou VSCode para ver o conteúdo do texto.  Eles devem ter uma aparência semelhante a:

```
-----BEGIN PRIVATE KEY-----
MIGTAgEAMBMGBasGSM49AgGFCCqGSM49AwEHBHkwdwIBAQQg3MX8n6VnQ2WzgEy0
Skoz9uOvatLMKTUIPyPCAejzzUCgCgYIKoZIzj0DAQehRANCAARZ0DoM6QPqpJxP
JKSlWz0AohFhYre10EXPkjrih4jTm+b0AeG2BGuoIWd18i8FimGDgK6IzHHPsEqj
DHF5Svq0
-----END PRIVATE KEY-----
```

Nomeie essa chave `P8FileContents` e mantenha-a em um local seguro. Você o usará ao integrar esse serviço em seu aplicativo móvel.

## <a name="summary"></a>Resumo

Este artigo descreveu as etapas necessárias para configurar a entrada com a Apple para uso em seus aplicativos Xamarin. Forms.

## <a name="related-links"></a>Links relacionados

- [Entre com as diretrizes da Apple](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)
  