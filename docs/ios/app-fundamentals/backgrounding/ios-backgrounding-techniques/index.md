---
title: "iOS Backgrounding técnicas"
ms.topic: article
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b623e9a13c7b4fd3854ee6a144d6401720d9ba8c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="ios-backgrounding-techniques"></a>iOS Backgrounding técnicas

As seções a seguir, exploraremos os seguintes recursos de iOS junto com as opções de backgrounding existentes:

-  [Tarefas do plano de fundo oportunista](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -preservar a vida útil da bateria, executando tarefas em segundo plano em partes oportunistas quando o dispositivo está ativo para outros tipos de processamento.
-  [O serviço de transferência do plano de fundo](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) - confiável carregar e baixar arquivos, independentemente do tamanho de arquivo ou o status de rede.
-  [Busca de plano de fundo](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) -atualizar um aplicativo do plano de fundo em intervalos determinados pelo sistema.
-  [Notificações remotas](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) -notificações por push de uso para disparar atualizações de conteúdo em segundo plano antes do usuário abre o aplicativo, com uma opção para notificar o usuário ou atualizar silenciosamente.
-  **Atualizações de interface do usuário do plano de fundo** - preparar o aplicativo da interface do usuário para o usuário e atualizar o instantâneo do aplicativo, todos os do plano de fundo.