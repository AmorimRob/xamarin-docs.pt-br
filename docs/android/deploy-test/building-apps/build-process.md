---
title: Processo de build
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 68ddb9baa008ec8222b4399a5ab25330fda2afd1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="build-process"></a>Processo de build

<a name="Overview" />

## <a name="overview"></a>Visão geral

O processo de build do Xamarin.Android é responsável por juntar tudo isto: [gerar `Resource.designer.cs`](~/android/internals/api-design.md), dar suporte a `AndroidAsset`, `AndroidResource` e outras [ações de build](#Build_Actions), gerar [wrappers que podem ser chamados pelo Android](~/android/platform/java-integration/android-callable-wrappers.md) e gerar um `.apk` para execução em dispositivos Android.

<a name="App_Packaging" />
<a name="Application_Packages" />

## <a name="application-packages"></a>Pacotes de aplicativos

Em termos gerais, há dois tipos de pacotes de aplicativos do Android (arquivos `.apk`) que o sistema de build do Xamarin.Android pode gerar:

-   Builds de **versão**, que são totalmente independentes e não requerem pacotes adicionais para executar. Esses são os pacotes que seriam fornecidos para uma loja de aplicativos.

-   Builds de **depuração**, que não o são.

Não coincidentemente, eles correspondem ao `Configuration` do MSBuild que produz o pacote.

<a name="Shared_Runtime" />

### <a name="shared-runtime"></a>Tempo de execução compartilhado

O *tempo de execução compartilhado* é um par de pacotes Android adicionais que fornecem a biblioteca de classes base (`mscorlib.dll`, etc.) e a biblioteca de associações do Android (`Mono.Android.dll`, etc.). Builds de depuração contam com o tempo de execução compartilhado em vez de incluir os assemblies da biblioteca de classes base e da biblioteca de associações dentro do pacote do aplicativo Android, permitindo que o pacote de depuração seja menor.

O tempo de execução compartilhado pode ser desabilitado em builds de depuração, definindo a propriedade `$(AndroidUseSharedRuntime)` para `False`.

<a name="Fast_Deployment" />

### <a name="fast-deployment"></a>Fast Deployment

O *Fast Deployment* trabalha em conjunto com o tempo de execução compartilhado para reduzir ainda mais o tamanho do pacote do aplicativo Android. Isso é feito não agrupando assemblies do aplicativo dentro do pacote. Em vez disso, eles são copiados para o destino por meio de `adb push`. Esse processo agiliza o ciclo de build/implantação/depuração porque se *somente* assemblies são alterados, o pacote não é reinstalado. Em vez disso, apenas os assemblies atualizados são sincronizados novamente ao dispositivo de destino.

O Fast Deployment reconhecidamente falha em dispositivos que bloqueiam a sincronização de `adb` ao diretório `/data/data/@PACKAGE_NAME@/files/.__override__`.

O Fast Deployment é habilitado por padrão e pode ser desabilitado em builds de depuração, definindo a propriedade `$(EmbedAssembliesIntoApk)` para `True`.


<a name="MSBuild_Projects" />

## <a name="msbuild-projects"></a>Projetos do MSBuild

O processo de build do Xamarin.Android baseia-se no MSBuild, que também é o formato de arquivo de projeto usado pelo Visual Studio para Mac e pelo Visual Studio.
Em geral, os usuários não precisarão editar os arquivos do MSBuild manualmente &ndash; o IDE cria projetos totalmente funcionais e os atualiza com eventuais alterações feitas, além de invocar destinos de build automaticamente conforme necessário.

Usuários avançados talvez queiram fazer coisas não compatíveis com a interface gráfica do IDE, portanto, o processo de build pode ser personalizado editando-se o arquivo de projeto diretamente.
Esta página documenta somente os recursos e personalizações específicos do Xamarin.Android &ndash; muitas outras coisas são possíveis com os itens, propriedades e destinos normais do MSBuild.

<a name="Build_Targets" />

## <a name="build-targets"></a>Destinos de build

Os destinos de build a seguir são definidos para projetos de Xamarin.Android:

-   **Build** &ndash; compila o pacote.

-   **Clean** &ndash; remove todos os arquivos gerados pelo processo de build.

-   **Install** &ndash; instala o pacote para o dispositivo padrão ou o dispositivo virtual.

-   **Uninstall** &ndash; desinstala o pacote do dispositivo padrão ou do dispositivo virtual.

-   **SignAndroidPackage** &ndash; cria e assina o pacote (`.apk`). Use com `/p:Configuration=Release` para gerar pacotes de "Versão" independentes.

-   **UpdateAndroidResources** &ndash; atualiza o arquivo `Resource.designer.cs`. Esse destino geralmente é chamado pelo IDE quando novos recursos são adicionados ao projeto.

<a name="Build_Properties" />

## <a name="build-properties"></a>Propriedades de build

Propriedades do MSBuild controlam o comportamento dos destinos. Elas são especificadas no arquivo de projeto, por exemplo, **MyApp.csproj**, dentro de um [elemento PropertyGroup do MSBuild](http://msdn.microsoft.com/en-us/library/t4w159bs.aspx).

-   **Configuration** &ndash; especifica a configuração de build a ser usada, como "Debug" ou "Release". A propriedade Configuration é usada para determinar os valores padrão de outras propriedades que determinam o comportamento de destino. Configurações adicionais podem ser criadas dentro de seu IDE.

    *Por padrão*, a configuração `Debug` fará com que os destinos `Install` e `SignAndroidPackage` criem um pacote do Android menor, que exigirá a presença de outros arquivos e pacotes para funcionar.

    A configuração padrão `Release` fará com que os destinos `Install` e `SignAndroidPackage` criem um pacote Android *autônomo*, que pode ser usado sem a instalação de outros pacotes ou arquivos.

-   **DebugSymbols** &ndash; um valor booliano que determina se o pacote Android é *depurável*, em combinação com a propriedade `$(DebugType)`. Um pacote depurável contém símbolos de depuração, define o atributo `//application/@android:debuggable` para `true` e adiciona automaticamente a permissão `INTERNET` para um depurador possa ser anexado ao processo. Um aplicativo é depurável se `DebugSymbols` é `True` *e* `DebugType` é a cadeia de caracteres vazia ou `Full`.

-   **DebugType** &ndash; especifica o [tipo de símbolos de depuração](http://msdn.microsoft.com/en-us/library/s5c8athz.aspx) para gerar como parte do build, o que também afeta se o aplicativo é ou não depurável. Os possíveis valores incluem:

    - **Full**: todos os símbolos são gerados. Se a propriedade do MSBuild `DebugSymbols` também é `True`, o pacote do aplicativo é depurável.

    - **PdbOnly**: os símbolos "PDB" são gerados. O pacote do aplicativo *não* será depurável.

    Se `DebugType` não está definida ou é uma cadeia de caracteres vazia, a propriedade `DebugSymbols` controla se o aplicativo é ou não depurável.


### <a name="install-properties"></a>Propriedades de instalação

Propriedades de instalação controlam o comportamento dos destinos `Install` e `Uninstall`.

-   **AdbTarget** &ndash; especifica o dispositivo de destino do Android no qual o pacote Android pode ser instalado ou do qual ele pode ser removido. O valor dessa propriedade o mesmo da [opção de dispositivo de destino `adb`](http://developer.android.com/tools/help/adb.html#issuingcommands):

    ```bash
    # Install package onto emulator via -e
    # Use `/Library/Frameworks/Mono.framework/Commands/xbuild` on OS X
    MSBuild /t:Install ProjectName.csproj /p:AdbTarget=-e
    ```

<a name="App_Packaging" />

### <a name="packaging-properties"></a>Propriedades de empacotamento

Propriedades de empacotamento controlam a criação do pacote Android e são usadas pelos destinos `Install` e `SignAndroidPackage`.
As [propriedades de assinatura](#Signing_Properties) também são relevantes ao empacotar aplicativos de versão.


-   **AndroidApplication** &ndash; um valor booliano que indica se o projeto é para um aplicativo Android (`True`) ou para um projeto de biblioteca Android (`False` ou ausente).

    Apenas um projeto com `<AndroidApplication>True</AndroidApplication>` pode estar presente em um pacote Android. (Infelizmente isso é ainda não verificado, que pode resultar em erros sutis e bizarros relacionados aos recursos do Android.)

-   **AndroidBuildApplicationPackage** &ndash; um valor booliano que indica se o pacote (.apk) deve ou não ser criado e assinado. Definir esse valor como `True` é equivalente a usar o destino de build [SignAndroidPackage](#Build_Targets).

    O suporte para essa propriedade foi adicionado após o Xamarin.Android 7.1.

    Essa propriedade é `False` por padrão.

-   **AndroidEnableMultiDex** &ndash; uma propriedade booliana que determina se o suporte a Multi-Dex será usado no `.apk` final.

    O suporte para essa propriedade foi adicionado no Xamarin.Android 5.1.

    Essa propriedade é `False` por padrão.

-   **AndroidEnableSGenConcurrent** &ndash; uma propriedade booliana que determina se o [coletor de GC simultâneo](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#concurrent-sgen) do Mono será ou não usado.

    O suporte para essa propriedade foi adicionado no Xamarin.Android 7.2.

    Essa propriedade é `False` por padrão.

-   **AndroidFastDeploymentType** &ndash; uma lista de valores separados por `:` (vírgula) para controlar quais tipos podem ser implantados no [diretório do Fast Deployment](#Fast_Deployment) no dispositivo de destino quando a propriedade [`$(EmbedAssembliesIntoApk)`](#EmbedAssembliesIntoApk) do MSBuild é `False`. Se um recurso é implantado por Fast Deployment, ele *não* é inserido no `.apk` gerado, o que pode acelerar os tempos de implantação. (Quanto mais rápido ele é implantado, menor a frequência com que o `.apk` precisa ser recriado, acelerando assim o processo de instalação.) Os valores válidos incluem:

    - `Assemblies`: implantar os assemblies do aplicativo.

    - `Dexes`: implantar arquivos `.dex`, recursos do Android e ativos do Android. **Esse valor pode ser usado *somente* em dispositivos que executam o Android 4.4 ou posterior (API-19).**

    O valor padrão é `Assemblies`.

    **Experimental**. Adicionado no Xamarin.Android 6.1.

-   **AndroidApplicationJavaClass** &ndash; o nome de classe Java completo a ser usado no lugar de `android.app.Application` quando uma classe herda de [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/).

    Geralmente, essa propriedade é definida por *outras* propriedades, tais como a propriedade [`$(AndroidEnableMultiDex)`](#AndroidEnableMultiDex) do MSBuild.

    Adicionado no Xamarin.Android 6.1.

-   **AndroidHttpClientHandlerType** &ndash; permite definir o valor da [variável de ambiente `XA_HTTP_CLIENT_HANDLER_TYPE`](~/android/deploy-test/environment.md).
    Esse valor não substituirá um valor `XA_HTTP_CLIENT_HANDLER_TYPE` especificado explicitamente. Um valor de variável de ambiente `XA_HTTP_CLIENT_HANDLER_TYPE` especificado em um arquivo [`@(AndroidEnvironment)`](#AndroidEnvironment) terá precedência.

    Adicionado no Xamarin.Android 6.1.

-   **AndroidTlsProvider** &ndash; um valor de cadeia de caracteres que especifica qual provedor TLS deve ser usado em um aplicativo. Os valores possíveis são: Os valores válidos incluem:

    - `btls`: usar [Boring SSL](https://boringssl.googlesource.com/boringssl) para a comunicação por TLS com [HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.aspx).
      Isso permite o uso da TLS 1.2.

    - `legacy`: usar a implementação de SSL gerenciada histórica para interação de rede. Isso *não* é compatível com a TLS 1.2.

    - `default`, ou cadeia de caracteres não definida/vazia: no Xamarin.Android 7.1, isso é equivalente a `legacy`.

    O valor padrão é a cadeia de caracteres vazia.

    **Experimental**. Adicionado no Xamarin.Android 7.1.

-   **AndroidLinkMode** &ndash; especifica qual tipo de [vinculação](~/android/deploy-test/linker.md) deve ser realizada em assemblies contidos no pacote Android. Usado somente em projetos de aplicativo Android. O valor padrão é *SdkOnly*. Os valores válidos são:

    - **None**: nenhuma tentativa de vinculação ocorrerá.

    - **SdkOnly**: a vinculação ocorrerá apenas nas bibliotecas de classe base, não nos assemblies do usuário.

    - **Full**: a vinculação ocorrerá nas bibliotecas de classe base e nos assemblies do usuário. **Observação:** o uso de um valor `AndroidLinkMode` definido como *Full* normalmente resulta em aplicativos com falha, especialmente quando a reflexão é usada. Evite usar isso, a menos que você *realmente* saiba o que está fazendo.

    ```xml
    <AndroidLinkMode>SdkOnly</AndroidLinkMode>
    ```

-   **AndroidLinkSkip** &ndash; especifica uma lista delimitada por ponto e vírgula (`;`) de nomes de assembly, sem extensões de arquivo, de assemblies que não devem ser vinculados. Usado somente em projetos de aplicativo Android.

    ```xml
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
    ```

-   **AndroidManagedSymbols** &ndash; uma propriedade booliana que controla se pontos de sequência são gerados para que o nome do arquivo e as informações de número de linha possam ser extraídos dos rastreamentos de pilha `Release`.

    Adicionado no Xamarin.Android 6.1.

-   **AndroidManifest** &ndash; especifica um nome de arquivo a ser usado como modelo para o [`AndroidManifest.xml`](~/android/platform/android-manifest.md) do aplicativo.
    Durante o build, quaisquer outros valores necessários serão mesclados para produzir o `AndroidManifest.xml` propriamente dito.
    O `$(AndroidManifest)` deve conter o nome do pacote no atributo `/manifest/@package`.

-   **AndroidSdkBuildToolsVersion** &ndash; o pacote de ferramentas de build do Android SDK fornece as ferramentas **aapt** e **zipalign**, entre outras. Várias versões diferentes do pacote de ferramentas de build podem ser instaladas simultaneamente. O pacote de ferramentas de build escolhido para empacotamento é criado procurando e usando uma versão de ferramentas de build "preferencial", se uma está presente; se a versão "preferencial" *não* está presente, o pacote de ferramentas de build com a versão mais alta entre os instalados é usado.

    A propriedade de MSBuild `$(AndroidSdkBuildToolsVersion)` contém a versão das ferramentas de build preferencial. O sistema de build do Xamarin.Android fornece um valor padrão em `Xamarin.Android.Common.targets`, o qual pode ser substituído no arquivo de projeto para escolher uma versão de ferramentas de build alternativa se, por exemplo, o aapt mais recente está falhando e sabe-se que uma versão anterior do aapt funciona.

-   **AndroidSupportedAbis** &ndash; uma propriedade de cadeia de caracteres que contém uma lista delimitada por ponto e vírgula (`;`) de ABIs que devem ser incluídos no `.apk`.

    Os valores compatíveis incluem:

    -   `armeabi`
    -   `armeabi-v7a`
    -   `x86`
    -   `arm64-v8a`: requer o Xamarin.Android 5.1 e posterior.
    -   `x86_64`: requer o Xamarin.Android 5.1 e posterior.

-   **AndroidUseSharedRuntime** &ndash; uma propriedade booliana que determina se os *pacotes de tempo de execução compartilhado* são necessários para executar o aplicativo no dispositivo de destino. Depender dos pacotes de tempo de execução compartilhado permite que o pacote do aplicativo seja menor, acelerando o processo de criação e implantação de pacote e resultando em um ciclo de build/implantação/depuração mais rápido.

    Essa propriedade deve ser `True` para builds de depuração e `False` para projetos de versão.

-   **AotAssemblies** &ndash; uma propriedade booliana que determina se os assemblies serão ou não compilados Ahead of Time em código nativo e incluídos no `.apk`.

    O suporte para essa propriedade foi adicionado no Xamarin.Android 5.1.

    Essa propriedade é `False` por padrão.

-   **EmbedAssembliesIntoApk** &ndash; uma propriedade booliana que determina se os assemblies do aplicativo devem ser inseridos no pacote do aplicativo.

    Essa propriedade deve ser `True` para builds de versão e `False` para builds de depuração. Ele *pode* precisar ser `True` em builds de depuração se o Fast Deployment não é compatível com o dispositivo de destino.

    Quando essa propriedade é `False`, a propriedade `$(AndroidFastDeploymentType)` do MSBuild também controla o que é inserido no `.apk`, o que pode afetar os tempos de implantação e de rebuild.

-   **EnableLLVM** &ndash; uma propriedade booliana que determina se o LLVM será ou não usado ao compilar assemblies Ahead of Time em código nativo.

    O suporte para essa propriedade foi adicionado no Xamarin.Android 5.1.

    Essa propriedade é `False` por padrão.

    Essa propriedade é ignorada, a menos que a propriedade `$(AotAssemblies)` do MSBuild seja `True`.

-   **EnableProguard** &ndash; uma propriedade booliana que determina se [proguard](http://developer.android.com/tools/help/proguard.html) é ou não executado como parte do processo de empacotamento para vincular o código Java.

    O suporte para essa propriedade foi adicionado no Xamarin.Android 5.1.

    Essa propriedade é `False` por padrão.

    Quando `True`, os arquivos de [ProguardConfiguration](#ProguardConfiguration) serão usados para controlar a execução de `proguard`.

-   **JavaMaximumHeapSize** &ndash; especifica o valor do parâmetro **java**
    `-Xmx` a ser usado ao criar o arquivo `.dex` como parte do processo de empacotamento. Se não for especificado, a opção `-Xmx` não é fornecida para **java**.

    Especificar essa propriedade é necessária se o [destino `_CompileDex` gera um `java.lang.OutOfMemoryError`](https://bugzilla.xamarin.com/show_bug.cgi?id=18327).

    ```xml
    <JavaMaximumHeapSize>1G</JavaMaximumHeapSize>
    ```

-   **JavaOptions** &ndash; especifica as opções de linha de comando adicionais a serem passadas para **java** ao compilar o arquivo `.dex`.

-   **MandroidI18n** &ndash; especifica o suporte à internacionalização incluído com o aplicativo, tal como tabelas de agrupamento e de classificação. O valor é uma lista separada por vírgula ou ponto e vírgula de um ou mais dos seguintes valores que não diferenciam maiúsculas e minúsculas:

    -   **None**: não incluir nenhuma codificação adicional.

    -   **All**: incluir todas as codificações disponíveis.

    -   **CJK**: incluir codificações de Chinês, Japonês e Coreano, tais como *Japonês (EUC)* \[enc-jp, CP51932\], *Japonês (Shift-JIS)* \[iso-2022-jp, shift\_jis, CP932\], *Japonês (JIS)* \[CP50220\], *Chinês Simplificado (GB2312)* \[gb2312, CP936\], *Coreano (UHC)* \[ks\_c\_5601-1987, CP949\], *Coreano (EUC)* \[euc-kr, CP51949\], *Chinês Tradicional (Big5)* \[big5, CP950\] e *Chinês Simplificado (GB18030)* \[GB18030, CP54936\].

    -   **Oriente Médio**: incluir codificações do oriente médio, tais como *Turco (Windows)* \[iso-8859-9, CP1254\], *Hebraico (Windows)* \[windows-1255, CP1255\], *Árabe (Windows)* \[windows-1256, CP1256\], *Árabe (ISO)* \[iso-8859-6, CP28596\], *Hebraico (ISO)* \[iso-8859-8, CP28598\], *Latim 5 (ISO)* \[iso-8859-9, CP28599\] e *Hebraico (alternativa ao ISO)* \[iso-8859-8, CP38598\].

    -   **Outros**: incluir outras codificações como *Cirílico (Windows)* \[CP1251\], *Báltico (Windows)* \[iso-8859-4, CP1257\], *Vietnamita (Windows)* \[CP1258\], *Cirílico (KOI8-R)* \[koi8-r, CP1251\], *Ucraniano (KOI8-U)* \[koi8-u, CP1251\], *Báltico (ISO)* \[iso-8859-4, CP1257\], *Cirílico (ISO)* \[iso-8859-5, CP1251\], *ISCII Davenagari* \[x-iscii-de, CP57002\], *ISCII Bengalês* \[x-iscii-be, CP57003\], *ISCII Tâmil* \[x-iscii-ta, CP57004\], *ISCII Télugo* \[x-iscii-te, CP57005\], *ISCII Assamês* \[x-iscii-as, CP57006\], *ISCII Odia* \[x-iscii-or, CP57007\], *ISCII Canarim* \[x-iscii-ka, CP57008\], *ISCII Malaiala* \[x-iscii-ma, CP57009\], *ISCII Guzerate* \[x-iscii-gu, CP57010\], *ISCII Panjabi* \[x-iscii-pa, CP57011\] e *Tailandês (Windows)* \[CP874\].

    -   **Raro**: incluir codificações raras, tais como *IBM EBCDIC (Turco)* \[CP1026\], *IBM EBCDIC (Open Systems Latim 1)* \[CP1047\], *IBM EBCDIC (EUA-Canadá com Euro)* \[CP1140\], *IBM EBCDIC (Alemanha com Euro)* \[CP1141\], *IBM EBCDIC (Dinamarca/Noruega com Euro)* \[CP1142\], *IBM EBCDIC (Finlândia/Suécia com Euro)* \[CP1143\], *IBM EBCDIC (Itália com Euro)* \[CP1144\], *IBM EBCDIC (América Latina/Espanha com Euro)* \[CP1145\], *IBM EBCDIC (Reino Unido com Euro)* \[CP1146\], *IBM EBCDIC (França com Euro)* \[CP1147\], *IBM EBCDIC (Internacional com Euro)* \[CP1148\], *IBM EBCDIC (Islandês com Euro)* \[CP1149\], *IBM EBCDIC (Alemanha)* \[CP20273\], *IBM EBCDIC (Dinamarca/Noruega)* \[CP20277\], *IBM EBCDIC (Finlândia/Suécia)* \[CP20278\], *IBM EBCDIC (Itália)* \[CP20280\], *IBM EBCDIC (América Latina/Espanha)* \[CP20284\], *IBM EBCDIC (Reino Unido)* \[CP20285\], *IBM EBCDIC (Katakana Japonês Estendido)* \[CP20290\], *IBM EBCDIC (França)* \[CP20297\], *IBM EBCDIC (Árabe)* \[CP20420\], *IBM EBCDIC (Hebraico)* \[CP20424\], *IBM EBCDIC (Islandês)* \[CP20871\], *IBM EBCDIC (Cirílico – Servo, Búlgaro)* \[CP21025\], *IBM EBCDIC (EUA-Canadá)* \[CP37\], *IBM EBCDIC (Internacional)* \[CP500\], *Árabe (ASMO 708)* \[CP708\], *Centro-europeu (DOS)* \[CP852\]*, Cirílico (DOS)* \[CP855\], *Turco (DOS)* \[CP857\], *Europeu Ocidental (DOS com Euro)* \[CP858\], *Hebraico (DOS)* \[CP862\], *Árabe (DOS)* \[CP864\], *Russo (DOS)* \[CP866\], *Grego (DOS)* \[CP869\], *IBM EBCDIC (Latim 2)* \[CP870\] e *IBM EBCDIC (Grego)* \[CP875\].

    -   **Oeste**: incluir codificações ocidentais, tais como *Europeu Ocidental (Mac)* \[macintosh, CP10000\], *Islandês (Mac)* \[x-mac-islandês, CP10079\], *Centro-europeu (Windows)* \[iso 8859-2, CP1250\], *Europeu Ocidental (Windows)* \[iso 8859-1, CP1252\], *Grego (Windows)* \[iso-8859-7, CP1253\], *Centro-europeu (ISO)* \[iso 8859-2, CP28592\], *Latim 3 (ISO)* \[iso 8859-3, CP28593\], *Grego (ISO)* \[iso-8859-7, CP28597\], *Latim 9 (ISO)*  \[iso 8859-15, CP28605\], *OEM Estados Unidos* \[CP437\], *Europeu Ocidental (DOS)* \[CP850\], *Português (DOS)* \[CP860\], *Islandês (DOS)* \[CP861\], *Francês Canadense (DOS)* \[CP863\], e *Nórdico (DOS)* \[CP865\].

    ```xml
    <MandroidI18n>West</MandroidI18n>
    ```

-   **MonoSymbolArchive** &ndash; Uma propriedade booliana que controla se artefatos `.mSYM` são criados para uso posterior com `mono-symbolicate`, para extrair informações &ldquo;reais&rdquo; de nome do arquivo e de número de linha dos rastreamentos de pilha de versão.

    Isso é True por padrão para aplicativos de &ldquo;versão&rdquo; que têm os símbolos de depuração habilitados: [`$(EmbedAssembliesIntoApk)`](#EmbedAssembliesIntoApk), `$(DebugSymbols)` e `$(Optimize)` são True.

    Adicionado no Xamarin.Android 7.1.

-   **AndroidVersionCodePattern** &ndash; uma propriedade de cadeia de caracteres que permite ao desenvolvedor personalizar o `versionCode` no manifesto.
    Confira [Criar um código de versão para o APK](~/android/deploy-test/building-apps/abi-specific-apks.md) para obter informações sobre como decidir um `versionCode`.

    Alguns exemplos: se `abi` for `armeabi` e `versionCode` no manifesto for `123`, `{abi}{versionCode}` produzirá um versionCode de `1123` quando `$(AndroidCreatePackagePerAbi)` for True; caso contrário, gerará um valor de 123.
    Se `abi` é `x86_64` e `versionCode` no manifesto é `44`. Isso produzirá `544` quando `$(AndroidCreatePackagePerAbi)` for True; caso contrário, gerará um valor de `44`.

    Se nós incluíssemos uma cadeia de caracteres de formato de preenchimento esquerdo `{abi}{versionCode:0000}`, isso produziria `50044` porque estamos aplicando preenchimento esquerdo ao `versionCode` com `0`. Alternativamente, você pode usar o preenchimento decimal como `{abi}{versionCode:D4}`, que tem o mesmo efeito do exemplo anterior.

    Somente os formatos de cadeias de caracteres de preenchimento '0' e 'Dx' são compatíveis, já que o valor PRECISA ser um inteiro.

    Itens de chave pré-definidos

    -   **abi** &ndash; insere a abi direcionada para o aplicativo
        -   1 &ndash; `armeabi`
        -   2 &ndash; `armeabi-v7a`
        -   3 &ndash; `x86`
        -   4 &ndash; `arm64-v8a`
        -   5 &ndash; `x86_64`

    -   **minSDK** &ndash; insere o valor mínimo compatível do SDK encontrado no `AndroidManifest.xml`, ou então `11` se não há nenhum definido.

    -   **versionCode** &ndash; usa o código da versão diretamente do `Properties\AndroidManifest.xml`.

    Você pode definir os itens personalizados usando a propriedade [AndroidVersionCodeProperties](#AndroidVersionCodeProperties).

    Adicionado no Xamarin.Android 7.2.

-   **AndroidVersionCodeProperties** &ndash; uma propriedade de cadeia de caracteres que permite ao desenvolvedor definir itens personalizados para usar com o [AndroidVersionCodePattern](#AndroidVersionCodePattern).
    Eles estão na forma de um par `key=value`. Todos os itens no `value` devem ser valores inteiros. Por exemplo: `screen=23;target=$(_SupportedApiLevel)`.
    Como você pode ver, você pode fazer uso de propriedades do MSBuild existentes ou personalizadas na cadeia de caracteres.

    Adicionado no Xamarin.Android 7.2.


### <a name="binding-project-build-properties"></a>Propriedades de build do projeto de associação

As seguintes propriedades de MSBuild são usadas com [projetos de associação](~/android/platform/binding-java-library/index.md):

-   **AndroidClassParser** &ndash; uma propriedade de cadeia de caracteres que controla como os arquivos `.jar` são analisados. Os possíveis valores incluem:

    - **class-parse**: usa `class-parse.exe` para analisar o código de bytes Java diretamente, sem a assistência de uma JVM. Esse valor é experimental.


    - **jar2xml**: usar `jar2xml.jar` para usar a reflexão do Java para extrair tipos e membros de um arquivo `.jar`.

    As vantagens de `class-parse` sobre `jar2xml` são:

    - `class-parse` é capaz de extrair nomes de parâmetro do código de bytes Java que contém símbolos de *depuração* (por exemplo, código de bytes compilado com `javac -g`).

    - `class-parse` não "ignora" classes que herdam, ou contêm, membros de tipos não resolvidos.

    **Experimental**. Adicionado no Xamarin.Android 6.0.

    O valor padrão é `jar2xml`.

    O valor padrão será alterado em uma versão futura.

-   **AndroidCodegenTarget** &ndash; uma propriedade de cadeia de caracteres que controla a ABI de destino de geração de código. Os possíveis valores incluem:

    - **XamarinAndroid**: usa a API de associação de JNI presente desde o Mono para Android 1.0. Assemblies de associação criados com Xamarin.Android 5.0 ou posterior podem ser executados apenas no Xamarin.Android 5.0 ou posterior (adições de API/ABI), mas o *código-fonte* é compatível com o das versões anteriores do produto.

    - **XAJavaInterop1**: usar Java.Interop para invocações de JNI. Assemblies de associação usando `XAJavaInterop1` só podem compilar e executar com o Xamarin.Android 6.1 ou posterior. O Xamarin.Android 6.1 e os posteriores associam `Mono.Android.dll` com esse valor.

      Os benefícios de `XAJavaInterop1` incluem:

      - Assemblies menores.

      - Cache em `jmethodID` para invocações de método `base`, desde que todos os outros tipos de associação na hierarquia de herança sejam compilados com o `XAJavaInterop1` ou posterior.

      - Cache em `jmethodID` de construtores do Java Callable Wrapper para subclasses gerenciadas.

    O valor padrão é `XamarinAndroid`.

    O valor padrão será alterado em uma versão futura.


<a name="Resgen" />
<a name="Resource_Properties" />

### <a name="resource-properties"></a>Propriedades de recurso

Propriedades do recurso controlam a geração do arquivo `Resource.designer.cs`, que fornece acesso a recursos do Android.

-   **AndroidResgenExtraArgs** &ndash; especifica as opções de linha de comando adicionais para passar para o comando **aapt** ao processar ativos e recursos do Android.

-   **AndroidResgenFile** &ndash; especifica o nome do arquivo de recurso para gerar. O modelo padrão define isso como `Resource.designer.cs`.

-   **MonoAndroidResourcePrefix** &ndash; especifica um *prefixo de caminho* que é removido do início dos nomes de arquivo com uma ação de build de `AndroidResource`. Isso é para permitir a alteração do local em que os recursos estão localizados.

    O valor padrão é `Resources`. Altere isso para `res` para a estrutura do projeto Java.

-   **AndroidExplicitCrunch** &ndash; se você estiver criando um aplicativo com um número muito grande de desenháveis locais, um build (ou rebuild) inicial pode levar minutos para ser concluído. Para acelerar o processo de build, tente incluir essa propriedade e configurá-la como `True`. Quando essa propriedade é definida, o processo de build pré-compacta os arquivos PNG.

    **Experimental**. Adicionado no Xamarin.Android 7.0.


<a name="Signing" />
<a name="Signing_Properties" />

### <a name="signing-properties"></a>Propriedades de assinatura

As propriedades de assinatura controlam como o pacote do aplicativo é assinado, para que ele possa ser instalado em um dispositivo Android. Para permitir uma iteração de build mais rápida, as tarefas do Xamarin.Android não assinam pacotes durante o processo de build, porque assinar é algo muito lento. Em vez disso, eles são assinados (se necessário) antes da instalação ou durante a exportação, pelo IDE ou pelo destino de build *Install*. Invocar o destino *SignAndroidPackage* produzirá um pacote com o sufixo `-Signed.apk` no diretório de saída.

Por padrão, o destino da assinatura gera uma nova chave de assinatura de depuração, se necessário. Se você quiser usar uma chave específica, por exemplo, em um servidor de build, as seguintes propriedades de MSBuild podem ser usadas:

-   **AndroidKeyStore** &ndash; um valor booliano que indica se as informações de autenticação personalizadas devem ser usadas. O valor padrão é `False`, que significa que a chave de assinatura de depuração padrão será usada para assinar pacotes.

-   **AndroidSigningKeyAlias** &ndash; especifica o alias para a chave no repositório de chaves. Este é o valor de **keytool-alias** usado ao criar o repositório de chaves.

-   **AndroidSigningKeyPass** &ndash; especifica a senha da chave de dentro do arquivo do repositório de chaves. Esse é o valor digitado quando `keytool` solicita **Insira a senha da chave para $(AndroidSigningKeyAlias)**.

-   **AndroidSigningKeyStore** &ndash; especifica o nome do arquivo do repositório de chaves criado por `keytool`. Isso corresponde ao valor fornecido à opção **keytool -keystore**.

-   **AndroidSigningStorePass** &ndash; especifica a senha para `$(AndroidSigningKeyStore)`. Esse é o valor fornecido a `keytool` ao criar o arquivo de repositório de chaves e ao solicitar **Insira a senha do repositório de chaves:**.

Por exemplo, considere a seguinte invocação de `keytool`:

```shell
$ keytool -genkey -v -keystore filename.keystore -alias keystore.alias -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password: keystore.filename password
Re-enter new password: keystore.filename password
...
Is CN=... correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: ...
Enter key password for keystore.alias
        (RETURN if same as keystore password): keystore.alias password
[Storing filename.keystore]
```

Para usar o armazenamento de chaves gerado acima, use o grupo de propriedades:

```xml
<PropertyGroup>
    <AndroidKeyStore>True</AndroidKeyStore>
    <AndroidSigningKeyStore>filename.keystore</AndroidSigningKeyStore>
    <AndroidSigningStorePass>keystore.filename password</AndroidSigningStorePass>
    <AndroidSigningKeyAlias>keystore.alias</AndroidSigningKeyAlias>
    <AndroidSigningKeyPass>keystore.alias password</AndroidSigningKeyPass>
</PropertyGroup>
```

<a name="Build_Actions" />

## <a name="build-actions"></a>Ações de Build

*Ações de build* são [aplicadas a arquivos](http://msdn.microsoft.com/en-us/library/bb629388.aspx) dentro do projeto e controlam como o arquivo é processado.

<a name="AndroidEnvironment" />

### <a name="androidenvironment"></a>AndroidEnvironment

Arquivos com uma ação de build `AndroidEnvironment` são usados para [inicializar variáveis de ambiente e propriedades do sistema durante a inicialização do processo](~/android/deploy-test/environment.md).
A ação de build `AndroidEnvironment` pode ser aplicada a vários arquivos, e eles serão avaliados sem nenhuma ordem específica (então não especifique a mesma propriedade de sistema ou variável de ambiente em vários arquivos).

<a name="Java_Interop_Support" />
<a name="AndroidJavaSource" />

### <a name="androidjavasource"></a>AndroidJavaSource

Arquivos com uma ação de build `AndroidJavaSource` são código-fonte do Java que será incluído no pacote do Android final.

<a name="AndroidJavaLibrary" />

### <a name="androidjavalibrary"></a>AndroidJavaLibrary

Arquivos com uma ação de build `AndroidJavaLibrary` são arquivos Java (arquivos `.jar`) que serão incluídos no pacote do Android final.

<a name="Resources" />
<a name="AndroidResource" />

### <a name="androidresource"></a>AndroidResource

Todos os arquivos com uma ação de build *AndroidResource* são compilados em recursos do Android durante o processo de build e tornados acessíveis por meio de `$(AndroidResgenFile)`.

```xml
<ItemGroup>
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
```

Usuários mais avançados talvez queiram usar diferentes recursos em configurações diferentes, mas com o mesmo caminho efetivo. Isso pode ser alcançado tendo vários diretórios do recurso e tendo arquivos com os mesmos caminhos relativos dentro desses diferentes diretórios, além de usar condições do MSBuild para incluir condicionalmente arquivos diferentes em configurações diferentes. Por exemplo:

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
<ItemGroup  Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug\values\strings.xml"/>
</ItemGroup>
<PropertyGroup>
  <MonoAndroidResourcePrefix>Resources;Resources-Debug<MonoAndroidResourcePrefix>
</PropertyGroup>
```

**LogicalName** &ndash; especifica o caminho do recurso explicitamente. Permite o &ldquo;uso de nomes alternativos&rdquo; para arquivos para que eles estejam disponíveis como vários nomes de recurso distintos.

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources/values/strings.xml"/>
</ItemGroup>
<ItemGroup Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug/values/strings.xml">
    <LogicalName>values/strings.xml</LogicalName>
  </AndroidResource>
</ItemGroup>
```

<a name="Native_Library_Support" />
<a name="AndroidNativeLibrary" />

### <a name="androidnativelibrary"></a>AndroidNativeLibrary

[Bibliotecas nativas](~/android/platform/native-libraries.md) são adicionadas ao build definindo-se as ações de build delas para `AndroidNativeLibrary`.

Observe que, como o Android é compatível com várias ABIs (interfaces binárias de aplicativo), o sistema de build deve saber para qual ABI a biblioteca nativa é compilada. Há duas maneiras de fazer isso:

1.  "Detecção" de caminho.
2.  Usando o atributo do item `Abi`.

Com a detecção de caminho, o nome do diretório pai da biblioteca nativa é usado para especificar a ABI usada como destino pela biblioteca. Portanto, se você adicionar `lib/armeabi/libfoo.so` ao build, em a ABI será "detectada" como `armeabi`.


### <a name="item-attribute-name"></a>Nome de atributo de item

**Abi** &ndash; especifica a ABI da biblioteca nativa.

```xml
<ItemGroup>
  <AndroidNativeLibrary Include="path/to/libfoo.so">
    <Abi>armeabi</Abi>
  </AndroidNativeLibrary>
</ItemGroup>
```

### <a name="content"></a>Conteúdo

A ação de build `Content` normal não é compatível (pois ainda não descobrimos como dar suporte a ela sem uma etapa de primeira execução de custo possivelmente alto).

Começando pelo Xamarin.Android 5.1, uma eventual tentativa de usar a ação de build `@(Content)` resultará em um aviso `XA0101`.

### <a name="linkdescription"></a>LinkDescription

Arquivos com uma ação de build *LinkDescription* são usados para [controlar o comportamento do vinculador](~/cross-platform/deploy-test/linker.md).


<a name="ProguardConfiguration" />

### <a name="proguardconfiguration"></a>ProguardConfiguration

Arquivos com uma ação de build *ProguardConfiguration* contêm opções que são usadas para controlar o comportamento de `proguard`. Para obter mais informações sobre essa ação de build, consulte [ProGuard](~/android/deploy-test/release-prep/proguard.md).

Esses arquivos são ignorados, a menos que a propriedade `$(EnableProguard)` do MSBuild seja `True`.


<a name="Target_Definitions" />

## <a name="target-definitions"></a>Definições de destino

As partes do processo de build específicas do Xamarin.Android são definidas em `$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets`, mas destinos normais específicos a uma linguagem, tais como *Microsoft.CSharp.targets*, também são necessários para compilar o assembly.

As seguintes propriedades de build devem ser definidas antes de importar quaisquer destinos de linguagem de programação:

```xml
<PropertyGroup>
  <TargetFrameworkIdentifier>MonoDroid</TargetFrameworkIdentifier>
  <MonoDroidVersion>v1.0</MonoDroidVersion>
  <TargetFrameworkVersion>v2.2</TargetFrameworkVersion>
</PropertyGroup>
```

Todos esses destinos e propriedades podem ser incluídos para C# importando *Xamarin.Android.CSharp.targets*:

```xml
<Import Project="$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets" />
```

Esse arquivo pode ser facilmente adaptado para outras linguagens de programação.