# <a name="how-to-run-the-completed-project"></a>Como executar o projeto concluído

## <a name="prerequisites"></a>Pré-requisitos

Para executar o projeto concluído nessa pasta, você precisará do seguinte:

- [Visual Studio](https://visualstudio.microsoft.com/vs/) instalado em sua máquina de desenvolvimento. Se você não tiver o Visual Studio, visite o link anterior para opções de download. (**Observação:** este tutorial foi escrito com o Visual Studio 2019 versão 16.5.0. As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.
- Uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft.

Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:

- Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.

## <a name="register-a-native-application-with-the-azure-active-directory-admin-center"></a>Registrar um aplicativo nativo com o centro de administração do Azure Active Directory

1. Abra um navegador, navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com) e faça logon usando uma **conta pessoal** (também conhecida como conta da Microsoft) ou **Conta Corporativa ou de Estudante**.

1. Selecione **Azure Active Directory** na navegação esquerda e selecione **Registros de aplicativos** em **Gerenciar**.

    ![Uma captura de tela dos registros de aplicativo ](/tutorial/images/aad-portal-app-registrations.png)

1. Selecione **Novo registro**. Na página **Registrar um aplicativo**, defina os valores da seguinte forma.

    - Defina **Nome** para `UWP Graph Tutorial`.
    - Defina **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**.
    - Em **URI de redirecionamento**, altere o menu suspenso para **cliente público (Desktop & móvel)** e defina `https://login.microsoftonline.com/common/oauth2/nativeclient`o valor como.

    ![Uma captura de tela da página registrar um aplicativo](/tutorial/images/aad-register-app.png)

1. Escolha **Registrar**. Na página do **tutorial do gráfico UWP** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](/tutorial/images/aad-application-id.png)

## <a name="configure-the-sample"></a>Configurar o exemplo

1. Renomear `OAuth.resw.example` o `OAuth.resw`arquivo para.
1. Abra `graph-tutorial.sln` no Visual Studio.
1. Edite `OAuth.resw` o arquivo no Visual Studio. Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.
1. No Gerenciador de soluções, clique com o botão direito do mouse na solução do **tutorial do gráfico** e escolha **restaurar pacotes NuGet**.

## <a name="run-the-sample"></a>Executar o exemplo

No Visual Studio, pressione **F5** ou escolha **depurar > iniciar a depuração**.
