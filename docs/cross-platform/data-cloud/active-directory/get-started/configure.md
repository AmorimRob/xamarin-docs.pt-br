---
title: "Etapa 2. Configurar o acesso de serviço para aplicativos móveis"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: a679d404eace121b38463e35350b8b85ec08b1e0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>Etapa 2. Configurar o acesso de serviço para aplicativos móveis

Sempre que qualquer recurso, por exemplo, o aplicativo web, o serviço web, etc. precisa ser protegido pelo Active Directory do Azure, ele precisa ser registrado. Todos os serviços ou aplicativos seguros que podem ser vistos em **aplicativos** guia. Aqui você pode selecionar o aplicativo que precisa ser acessado a partir de aplicativos móveis e dar acesso a ele.

1. Sobre o **configurar** guia, localize **permissões para outros aplicativos** seção:

  ![](configure-images/2.1-configure.png "Na guia Configurar, localize a seção permissões a outros aplicativos")

2.  Clique em **Adicionar aplicativo** botão. Na próxima tela pop-up, você verá a lista de todos os aplicativos que são protegidos pelo Active Directory do Azure. Selecione os aplicativos que precisam ser acessados pelo aplicativo móvel.

  ![](configure-images/2.2-add-application.png "Selecione os aplicativos que precisam ser acessados pelo aplicativo móvel")

3. Depois de selecionar o aplicativo, selecione novamente o aplicativo adicionado recentemente no **permissões para outros aplicativos** seção e conceder os direitos apropriados.

  ![](configure-images/2.3-permissions.png "Depois de selecionar o aplicativo, uma vez selecione o aplicativo adicionado recentemente na seção permissões a outros aplicativos e conceder direitos apropriados")

4. Por fim, **salvar** a configuração. Esses serviços agora devem estar disponíveis em aplicativos móveis!



## <a name="related-links"></a>Links relacionados

- [Exemplo de NativeClient Microsoft](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)