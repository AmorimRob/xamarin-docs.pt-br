---
title: Usando TestFlight
description: "O TestFlight agora é de propriedade da Apple e é a principal maneira de fazer o teste beta de seus aplicativos Xamarin.iOS. Este artigo o orientará por todas as etapas do Processo de TestFlight – do upload do aplicativo até o trabalho com o iTunes Connect."
ms.topic: article
ms.prod: xamarin
ms.assetid: BA880768-2BC8-41E4-B57E-A56F8EED4690
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 8de7b91e5854e5c660788cdca055860b2ba0139e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="using-testflight"></a>Usando TestFlight

_O TestFlight agora é propriedade da Apple, e é a principal maneira de fazer o teste beta de seus aplicativos Xamarin.iOS. Este artigo o orientará por todas as etapas do Processo de TestFlight – do upload do aplicativo até o trabalho com o iTunes Connect._

Teste beta é parte integrante do ciclo de desenvolvimento de software e há muitos aplicativos de plataforma cruzada oferecendo-se para simplificar esse processo, como [HockeyApp](http://hockeyapp.net/features/), [Applause](http://www.applause.com/mobile-app-testing) e, é claro, o Teste Beta de Aplicativo Nativo do Google Play para aplicativos Android. Este documento concentra-se no TestFlight da Apple.

O TestFlight é o serviço de teste beta da Apple para aplicativos iOS e é acessível apenas por meio do [iTunes Connect](https://itunesconnect.apple.com/). Está disponível para aplicativos iOS 8.0 e posteriores. TestFlight permite testes beta com usuários internos e externos e, devido a uma análise de aplicativo Beta para estes últimos, garante um processo muito mais fácil na análise final ao publicar para a App Store.


Anteriormente, o binário era gerado no Visual Studio para Mac e carregado para o site TestFlightApp para distribuição aos testadores. Com o novo processo, há vários aprimoramentos que permitem que você tenha aplicativos de alta qualidade e bem testados aplicativos na App Store. Por exemplo:


- A análise de Aplicativo Beta necessária para testes externos garante uma chance maior de sucesso para a Análise da App Store final, uma vez que ambos exigem aderência às diretrizes da Apple.
- Antes do upload, o aplicativo precisa ser registrado com o iTunes Connect. Isso garante que não haja incompatibilidade entre perfis, nomes e certificados de provisionamento.
- O aplicativo TestFlight agora é um aplicativo iOS real, assim, opera com mais rapidez.
- Depois da conclusão do teste beta, o processo de mover o aplicativo para análise é rápido e eficiente; basta um clique de botão.

## <a name="requirements"></a>Requisitos

Somente aplicativos iOS 8.0 ou superiores pode ser testado por meio de TestFlight.

Todos os testadores devem testar o aplicativo em pelo menos um dispositivo iOS 8. No entanto, a melhor prática recomenda testar seu aplicativo em todas as versões do iOS

## <a name="provisioning"></a>Provisionamento

Para testar builds com o TestFlight, é necessário criar um *perfil de distribuição na App Store* com o novo direito beta. Esse direito permite que teste beta por meio do TestFlight e quaisquer **novos** perfis de distribuição da App Store automaticamente contêm esse direito. Você pode seguir as instruções passo a passo no guia [Creating a Distribution Profile](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) (Como criar um perfil de distribuição) para gerar um novo perfil.

Você pode confirmar que seu perfil de distribuição contém o direito beta ao [validar seu build no Xcode](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md), conforme ilustrado abaixo:

[ ![](testflight-images/validate-build.png "Enviando o aplicativo para a Apple")](testflight-images/validate-build.png)


## <a name="testflight-workflow"></a>Fluxo de trabalho TestFlight

O fluxo de trabalho a seguir descreve as etapas necessárias para começar a usar TestFlight para testes Beta do seu aplicativo:

1. Para novos aplicativos, criar um [registro do iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md).
2. [Arquive e Publique](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) seu aplicativo no iTunes Connect.
3. Gerencie Testes Beta:
    - Adicione Metadados.
    - Adicione Usuários Internos:
        - Máximo de 25 Usuários.
    - Adicione Usuários Externos:
        - Máximo de 1.000 Usuários.
        - Requer uma análise de teste beta, que exige a conformidade com as diretrizes da Apple.
4. Receba Comentários de usuários, aja com base neles e retorne para a etapa 2.

## <a name="create-an-itunes-connect-record"></a>Criar um registro do iTunes Connect

1.  Faça logon no [Portal do iTunes Connect](https://itunesconnect.apple.com/) usando suas credenciais de desenvolvedor da Apple.
2.  Selecione **Meus Aplicativos**:

    [ ![](testflight-images/my-apps.png "Selecione Meus aplicativos")](testflight-images/my-apps.png)


3.  Na tela **Meus aplicativos**, clique no botão **+** no canto superior esquerdo da tela para adicionar um novo aplicativo. Se você tiver contas de desenvolvedor Mac e iOS, escolha o tipo do novo aplicativo aqui.

Você verá a janela de envio **Novo aplicativo do iOS**, que deverá conter exatamente as mesmas informações que Info.plist do seu aplicativo

Para obter mais informações sobre como criar um novo registro do iTunes Connect, consulte o guia [Creating an iTunes Connect Record](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) (Como criar um registro do iTunes Connect).



###  <a name="completing-the-new-ios-app-submission-form"></a>Preenchendo o novo formulário de envio de aplicativo iOS

O formulário deve refletir exatamente as informações no arquivo Info.plist do seu aplicativo, conforme ilustrado abaixo:

[ ![](testflight-images/infoplist.png "O Info.plist do aplicativo")](testflight-images/infoplist.png)
[ ![](testflight-images/newiosapp.png "O formulário no iTunes Connect")](testflight-images/newiosapp.png)

-  **Nome** – o nome descritivo usado ao configurar o Pacote de Aplicativo. Deve ser uma correspondência exata da entrada de **Nome do aplicativo** em seu `Info.plist`.
-  **Idioma primário** – o idioma de base usado no aplicativo. Geralmente é qualquer idioma que você fale.
-  **ID do pacote** – um menu suspenso listando todas as IDs do Aplicativo criadas em sua conta de desenvolvedor.
    *   **Sufixo da ID do Pacote** – se você tiver uma ID do Pacote curinga (ou seja, terminando com um *, como no exemplo acima), uma caixa adicional será exibida solicitando o sufixo da ID do Pacote. No exemplo, a **ID do Pacote** é `mobi.chkn.*`, o sufixo é **PageView**. Juntos, eles formam o **Identificador do Pacote** em nosso `Info.plist`.
-  **Versão** – número de versão do aplicativo que está sendo carregado. Ele é escolhido pelo desenvolvedor.
-  **SKU** – SKU é uma ID exclusiva para seu aplicativo, que não será vista pelos usuários. Pode ser considerada de maneira semelhante a uma ID do produto. No exemplo acima, escolhi a data junto com um número de versão para essa data.


## <a name="upload-your-app"></a>Carregar seu aplicativo

Quando o registro do iTunes Connect tiver sido criado, você poderá carregar novos builds. Lembre-se de que builds devem ter o novo direito beta.

Primeiro, compile o [distribuível final](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) no IDE e, em seguida, [envie seu aplicativo para a Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) por meio do Carregador de Aplicativos ou a função arquivo morto no Xcode.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio para Mac](#tab/vsmac)

###  <a name="create-an-archive"></a>Criar um arquivo morto

 Para compilar um binário no Visual Studio para Mac, você precisará usar a função _Arquivo Morto_. Clique com o botão direito dou mouse no projeto e selecione **Arquivo Morto para Publicação**, conforme ilustrado abaixo:

 [ ![](testflight-images/new-archive.png "Selecione Arquivo Morto para Publicação")](testflight-images/new-archive.png)


 Consulte o guia [Criando o distribuível](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) para obter mais informações.

###  <a name="sign-and-distribute-your-app"></a>Assinar e distribuir seu aplicativo

 Criar um arquivo morto abrirá automaticamente a **Exibição de Arquivos Mortos**, exibindo todos os projetos em arquivo morto, agrupados por solução. Para assinar seu aplicativo e prepará-lo para distribuição, selecione **Assinar e Distribuir…**, conforme mostrado abaixo:

[ ![](testflight-images/archive-view.png "Criar um arquivo morto abrirá automaticamente a Exibição de Arquivos Mortos")](testflight-images/archive-view.png)

 Isso abrirá o assistente de publicação. Selecione o canal de distribuição da **App Store** para criar um pacote e abra o Application Loader. Na tela de Perfil de Provisionamento, selecione sua identidade de assinatura e o perfil de provisionamento ou assine novamente com outra identidade. Verifique os detalhes do seu pacote e, em seguida, clique em **Publicar** para salvar seu `.ipa`

[ ![](testflight-images/group.png "Selecione sua identidade de assinatura e o perfil de provisionamento ou assine novamente com outra identidade")](testflight-images/group.png)

 Consulte a seção [Enviando seu aplicativo para a Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) para saber mais sobre essas etapas.

###  <a name="submitting-your-build"></a>Enviando seu Build
 O assistente de publicação abrirá o programa Carregador de Aplicativos para você carregar seu build para o iTunes Connect. Selecione a opção **Entregar seu Aplicativo** e, em seguida, carregue o arquivo `.ipa` criado acima. O Carregador de Aplicativos validará e carregará seu build para o iTunes Connect.

 Consulte a seção [Enviando seu aplicativo para a Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) para saber mais sobre essas etapas.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

###  <a name="building-your-final-distributable"></a>Compilando seu distribuível final
 Uma vez que o plug-in Xamarin para Visual Studio não dá suporte a arquivamento de aplicativos Xamarin.iOS para publicação na App Store, há duas opções para publicar um aplicativo iOS do Visual Studio. Elas são:

1. Carregar um IPA criado por meio do comando de Compilar IPA Adhoc.
1. Carregar um pacote `.app` compactado.

 O guia [Criando o distribuível](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) contém instruções para essas duas opções.

###  <a name="submitting-your-build"></a>Enviando seu Build
 Para enviar seu aplicativo para a Apple, você precisará ir para seu Host do Build e usar o programa Carregador de Aplicativos, que é instalado como parte do Xcode. Para obter mais informações sobre como acessar o Carregador de Aplicativos, confira o guia [Access Application Loader](http://help.apple.com/itc/apploader/#/apdATD1E927-D1E1A1303-D1E927A1126) (Acessar o Carregador de Aplicativos) da Apple.

Após abri-lo, selecione a opção **Deliver Your App**  (Entregar seu Aplicativo) e carregue o zip ou arquivo `.ipa` criado acima. O Carregador de Aplicativos validará e carregará seu build para o iTunes Connect.

 Consulte a seção [Enviando seu aplicativo para a Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) para saber mais sobre essas etapas.

-----


O guia [Publishing to the App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) (Como publicar na App Store) descreve todas as etapas acima em mais detalhes; consulte-o para uma análise mais detalhada do processo de envio à App Store.

Ao retornar para a seção **Meus Aplicativos** do iTunes Connect, você deverá ver que seu aplicativo foi carregado com êxito. Neste ponto, você agora está pronto para fazer alguns Testes Beta!

## <a name="manage-beta-testing"></a>Gerenciar Testes Beta

### <a name="add-metadata"></a>Adicionar Metadados

Para começar a usar o TestFlight, navegue até a guia **Pré-lançamento** de seu aplicativo. Você verá três guias mostrando uma lista de Builds, Testadores Internos e Testadores Externos, conforme ilustrado abaixo:

[ ![](testflight-images/app-uploaded.png "Guias Builds, Testadores Internos e Testadores Externos")](testflight-images/app-uploaded.png)

Para adicionar metadados a seu aplicativo, clique no número de build e, em seguida, em TestFlight:

[ ![](testflight-images/metadata.png "Adicione Metadados")](testflight-images/metadata.png)

Em **Informações de Teste**, você pode fornecer aos testadores informações significativas sobre seu aplicativo, por exemplo:

- O que testar
- Descrição do seu aplicativo.
- URL de marketing — dará informações sobre o aplicativo que você está adicionando.
- URL da Política de Privacidade — URL que apresenta informações sobre a política de privacidade de sua empresa.
- Email de Comentários.

Observe que esses metadados **não são** necessários para testadores internos, mas **são** necessários para testadores externos.

<a name="beta-testing" />

### <a name="enable-beta-testing"></a>Habilitar Testes Beta

Quando estiver pronto para começar a testar seu aplicativo, ative a opção **Teste Beta do TestFlight** para sua versão:

[ ![](testflight-images/turn-on-testing.png "Ative a opção Teste beta do TestFlight")](testflight-images/turn-on-testing.png)

Cada build está ativo por **60 dias** a partir da data em que você ativou a Opção Beta do TestFlight. Você pode ver quantos dias restam para cada build na página **Informações de Teste**:

[ ![](testflight-images/daysleft.png "A página Informações de Teste")](testflight-images/daysleft.png)

O teste pode ser desativado a qualquer momento.

### <a name="internal-testers"></a>Testadores Internos

Testadores Internos são membros de sua equipe de desenvolvimento atribuídos a uma das funções a seguir no iTunes Connect:

-  **Administrador** – um administrador é responsável por adicionar e gerenciar novos usuários no iTunes Connect.
-  **Legal** – o Agente de Equipe é o único usuário administrativo que terá a função Legal atribuída. Isso permite que ele assine contratos legais.
-  **Técnico** – um usuário técnico pode alterar a maioria das propriedades em relação a um aplicativo. Por exemplo, editar informações do aplicativo, carregar um binário e enviar um aplicativo para análise.

Cada build pode ser compartilhado com um máximo de 25 membros.

Para adicionar testadores, navegue até **Usuários e Funções** na tela principal do iTunes Connect:

[ ![](testflight-images/users-and-roles.png "Usuários e Funções na tela principal do iTunes Connect")](testflight-images/users-and-roles.png)

Usuários do iTunes Connect existentes aparecerão na lista. Para selecioná-los, clique no nome, ative a opção **Testador Interno** e clique em **Salvar**:

[ ![](testflight-images/internal-tester.png "Ative a opção Testador Interno")](testflight-images/internal-tester.png)

Para adicionar um usuário que não esteja na lista, selecione o botão **+** ao lado de *Usuários* e forneça um Nome, Sobrenome e endereço de email para criar uma conta. O usuário precisará confirmar seu email para ativar a conta:

[ ![](testflight-images/add-new-user.png "Adicionando um usuário")](testflight-images/add-new-user.png)

Se você voltar a **Meus Aplicativos > Pré-Lançamento > Testadores Internos**, verá os usuários adicionados aos testes beta Internos do TestFlight:

[ ![](testflight-images/select-users.png "Uma lista de usuários que foram adicionados para teste beta interno do TestFlight")](testflight-images/select-users.png)

Você pode convidar esses testadores selecionando o nome deles e clicando no botão **Convidar**. Eles receberão um email com um convite para testar seu aplicativo.

Você pode ver o status do convite na coluna status da página Testadores Internos:

[ ![](testflight-images/status-added.png "O status do convite")](testflight-images/status-added.png)


### <a name="external-testers"></a>Testadores Externos

Antes de convidar Testadores Externos para fazer testes beta do seu aplicativo, o aplicativo precisa passar por uma Análise de Aplicativo Beta e, portanto, estar em conformidade com as [Diretrizes de análise da App Store](https://developer.apple.com/app-store/review/guidelines/).

Para enviar seu aplicativo para análise, clique no texto **Enviar para Análise de Aplicativo Beta** ao lado de seu build, conforme mostra a imagem abaixo:

[ ![](testflight-images/beta-app-review.png "Envie para Análise de Aplicativo Beta")](testflight-images/beta-app-review.png)

Para seu aplicativo ser aprovado na análise, você deve inserir todos os metadados necessários na página Informações do Beta do TestFlight.

Agora você pode começar a preparar convites e adicionar até 2.000 testadores externos por meio da guia Testadores Externos, inserindo seu email, Nome e Sobrenome, conforme ilustra a captura de tela abaixo. O email que você inserir não precisa ser sua ID da Apple; ele é apenas o email que receberá o convite.

[ ![](testflight-images/add-external.png "Convide testadores")](testflight-images/add-external.png)

Se você tiver um grande número de testadores externos, poderá usar o link **Importar Arquivo** link para importar um arquivo `CSV` com o seguinte formato por linha:

``` 
first name, last name, email address
```

Você também pode adicionar testadores externos a grupos diferentes para ajudar a manter seus testadores organizados.

Depois de inserir os detalhes dos testadores externos, clique em **Adicionar** e confirme que os usuários deem consentimento para convidá-los:

[ ![](testflight-images/confirm-consent.png "Confirme que você tem o consentimento dos usuários para convidá-los")](testflight-images/confirm-consent.png)

Somente após uma análise de Aplicativo Beta bem-sucedida você poderá enviar convites para testadores externos. Neste ponto, o texto em **Externo** na página do build mudará para **Enviar Convites**. Clique nessa opção para enviar convites para todos os testadores já adicionados.

Se seu aplicativo tiver sido rejeitado, você precisará corrigir os problemas mostrados na **Central de Resolução** e reenviar o todo binário atualizado para análise.

## <a name="as-a-beta-tester"></a>Como um Testador Beta

Depois que você convidar o testador, ele receberá um email semelhante ao da captura de tela abaixo:

[ ![](testflight-images/tester-email.png "Um exemplo de e-mail de convite")](testflight-images/tester-email.png)

Depois de clicarem no botão **Abrir no TestFlight**, seu aplicativo será aberto no aplicativo TestFlight ou, se ele não tiver sido baixado, será direcionado para a App Store e permitirá que seja baixado.

Quando seu aplicativo for aberto no TestFlight, ele mostrará os detalhes do que testar e solicitará que o testador instale o aplicativo no dispositivo iOS 8.0 (ou superior):

[ ![](testflight-images/install-app.png "O TestFlight mostrará os detalhes do que será testado")](testflight-images/install-app.png)

Os builds de teste serão indicados na tela inicial do dispositivo por um ponto laranja antes do nome do aplicativo.

Os testadores podem fazer comentários por meio do aplicativo TestFlight e você receberá essas informações no endereço de email informado nos metadados.

### <a name="beta-testing-complete"></a>Teste beta concluído

Depois que o teste beta tiver sido concluído, você poderá enviar seu aplicativo para análise da App Store pela Apple. Esse processo é feito de modo muito direto no iTunes Connect clicando no botão **Enviar para Análise**, conforme ilustrado abaixo:

[ ![](testflight-images/submit-for-review.png "Clique no botão Enviar para Análise")](testflight-images/submit-for-review.png)

## <a name="summary"></a>Resumo

Este artigo analisou como usar Testes Beta do TestFlight da Apple por meio do iTunes Connect. Abordou como carregar um novo build para o iTunes Connect e como convidar testadores Beta internos e externos a usar nosso aplicativo.

## <a name="related-links"></a>Links relacionados

- [Criando um registro do iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Publicando na App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Provisionando um aplicativo para distribuição na App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#provisioning)
- [Usando o Apple TestFlight Beta](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/BetaTestingTheApp.html#//apple_ref/doc/uid/TP40011225-CH35-SW2)