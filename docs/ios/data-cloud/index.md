---
title: "Dados e serviços de nuvem"
description: "Estabilização e guias de implantação"
ms.topic: article
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: b5e250c7c505958c8293970321b6173e6086e7b1
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/12/2018
---
# <a name="data-and-cloud-services"></a>Dados e serviços de nuvem


##  <a name="data-accessiosdata-clouddataindexmd"></a>[Acesso a dados](~/ios/data-cloud/data/index.md)

A maioria dos aplicativos têm alguns requisitos para salvar dados no dispositivo local. A menos que a quantidade de dados é muito pequena, isso geralmente exige um banco de dados e uma camada de dados do aplicativo para gerenciar o acesso ao banco de dados. iOS tem o mecanismo de banco de dados Sqlite "interno" e o acesso aos dados é simplificado pela plataforma do Xamarin que vem com o provedor de dados SQLite.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

A API de armazenamento no iCloud no iOS 5 permite que aplicativos salvar os documentos do usuário e dados específicos do aplicativo para um local central e acessar esses itens em todos os dispositivos do usuário.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

A estrutura CloudKit simplifica o desenvolvimento de aplicativos que iCloud de acesso. Isso inclui a recuperação de dados de aplicativo e direitos de ativo e ser capaz de armazenar com segurança informações do aplicativo. Esse kit oferece aos usuários uma camada de anonimato permitindo o acesso a aplicativos com sua IDs de iCloud sem compartilhar informações pessoais.