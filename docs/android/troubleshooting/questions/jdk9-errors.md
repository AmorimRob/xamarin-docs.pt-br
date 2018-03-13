---
title: Xamarin e JDK 9
description: Este artigo explica como resolver erros de Java Development Kit (JDK) 9 em xamarin.
ms.topic: article
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 9e1dd2fac015783e06bad6656f09a687f3abba2c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinandroid-and-jdk-9"></a>Xamarin e JDK 9

_Este artigo explica como resolver erros de Java Development Kit (JDK) 9 em xamarin._


## <a name="overview"></a>Visão geral

Xamarin usa Java Development Kit (JDK) para integrar o SDK do Android para a criação de aplicativos do Android e executando o Android designer. As versões mais recentes do SDK do Android (API 24 e superior) requerem o JDK 8 (1.8). **Como as ferramentas do SDK do Android disponíveis no Google ainda não são compatíveis com o JDK 9, xamarin não funciona com o JDK 9.**

## <a name="jdk-9-errors"></a>JDK 9 erros

Se você tentar compilar um projeto xamarin com 9 JDK instalado, você obterá um erro explícito, indicando que não há suporte para JDK 9.

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors  
```

Para resolver esses erros, você deve instalar o JDK 8 (1.8) conforme explicado em [como atualizar a versão do Java Development Kit (JDK)?](~/android/troubleshooting/questions/update-jdk.md).


## <a name="checking-the-jdk-version"></a>Verificar a versão do JDK

Você pode verificar qual versão do Java que você instalou, digitando o seguinte comando (o JDK `bin` diretório deve estar no seu `PATH`):

```shell
java -version
```

Se o JDK 9 instalado, você verá uma mensagem semelhante ao seguinte:

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

Se o JDK 9 estiver instalado, você deve instalar o Java JDK 8 (1.8). Para obter informações sobre como instalar o JDK 8, consulte [como atualizar a versão do Java Development Kit (JDK)?](~/android/troubleshooting/questions/update-jdk.md)

## <a name="known-issues-with-jdk-9"></a>Problemas conhecidos com o JDK 9

### <a name="apksigner"></a>apksigner

Há um problema conhecido com apksigner e JDK 9 no qual o `apksigner.bat` arquivo invoca o `apksigner.jar` com `-Djava.ext.dirs` em vez de `-classpath` que espera JDK 9. É recomendável usar o JDK 8 (1.8). Para obter informações sobre como instalar o JDK 8, consulte [como atualizar a versão do Java Development Kit (JDK)?](~/android/troubleshooting/questions/update-jdk.md)

Após a desinstalação do JDK 9, certifique-se de que o caminho a seguir não está definido no seu `PATH` variável de ambiente que apontará para o JDK 9: `C:\ProgramData\Oracle\Java\javapath`. Após a remoção, `java -version` em uma linha de comando deve mostrar o JDK 8.