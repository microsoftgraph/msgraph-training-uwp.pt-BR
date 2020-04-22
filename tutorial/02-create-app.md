<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você criará um novo aplicativo UWP.

1. Abra o Visual Studio e selecione **criar um novo projeto**. Escolha a opção **aplicativo em branco (universal do Windows)** que usa C# e, em seguida, selecione **Avançar**.

    ![Visual Studio 2019 criar nova caixa de diálogo de projeto](./images/vs-create-new-project.png)

1. Na caixa de diálogo **configurar seu novo projeto** , `GraphTutorial` Insira o campo **nome do projeto** e selecione **criar**.

    ![Visual Studio 2019 configurar a caixa de diálogo novo projeto](./images/vs-configure-new-project.png)

    > [!IMPORTANT]
    > Certifique-se de inserir exatamente o mesmo nome para o projeto do Visual Studio especificado nas instruções do laboratório. O nome do projeto do Visual Studio se torna parte do namespace no código. O código dentro dessas instruções depende do namespace correspondente ao nome do projeto do Visual Studio especificado nessas instruções. Se você usar um nome de projeto diferente, o código não será compilado, a menos que você ajuste todos os namespaces para corresponder ao nome do projeto do Visual Studio que você digitou ao criar o projeto.

1. Clique em **OK**. Na caixa de diálogo **novo projeto da plataforma universal do Windows** , verifique se a **versão mínima** está definida como `Windows 10, Version 1809 (10.0; Build 17763)` ou posterior e selecione **OK**.

## <a name="install-nuget-packages"></a>Instalar pacotes NuGet

Antes de prosseguir, instale alguns pacotes NuGet adicionais que serão usados posteriormente.

- [Microsoft. Toolkit. UWP. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls/) para adicionar alguns controles da interface do usuário para notificações no aplicativo e indicadores de carregamento.
- [Microsoft. Toolkit. UWP. UI. Controls. DataGrid](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid/) para exibir as informações retornadas pelo Microsoft Graph.
- [Microsoft. Toolkit. Graph. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Graph.Controls) para lidar com a recuperação de tokens de acesso e logon.

1. Selecione **ferramentas > Gerenciador de pacotes do NuGet > console do Gerenciador de pacotes**. No console do Gerenciador de pacotes, insira os seguintes comandos.

    ```powershell
    Install-Package Microsoft.Toolkit.Uwp.Ui.Controls -Version 6.0.0
    Install-Package Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid -Version 6.0.0
    Install-Package Microsoft.Toolkit.Graph.Controls -IncludePrerelease
    ```

## <a name="design-the-app"></a>Projetar o aplicativo

Nesta seção, você criará a interface do usuário para o aplicativo.

1. Comece adicionando uma variável de nível de aplicativo para rastrear o estado de autenticação. No Gerenciador de soluções, expanda **app. XAML** e abra **app.XAML.cs**. Adicione a propriedade a seguir à `App` classe.

    ```csharp
    public bool IsAuthenticated { get; set; }
    ```

1. Definir o layout da página principal. Abra `MainPage.xaml` e substitua todo o conteúdo pelo seguinte.

    :::code language="xaml" source="../demo/GraphTutorial/MainPage.xaml" id="MainPageXamlSnippet":::

    Isso define um [NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) básico com links de navegação de **calendário** e **Home** para atuar como o modo de exibição principal do aplicativo. Ele também adiciona um controle [LoginButton](https://github.com/windows-toolkit/Graph-Controls) no cabeçalho do modo de exibição. Esse controle permitirá que o usuário entre e saia. O controle ainda não está totalmente habilitado, você irá configurá-lo em um exercício posterior.

1. Clique com o botão direito do mouse no projeto do **tutorial de gráfico** no Gerenciador de soluções e selecione **Adicionar > novo item...**. Escolha **página em branco**, `HomePage.xaml` digite no campo **nome** e selecione **Adicionar**. Substitua o elemento `<Grid>` existente no arquivo com o seguinte.

    :::code language="xaml" source="../demo/GraphTutorial/HomePage.xaml" id="HomePageGridSnippet" highlight="2-5":::

1. Expanda **MainPage. XAML** no Gerenciador de soluções `MainPage.xaml.cs`e abra. Adicione a função a seguir à `MainPage` classe para gerenciar o estado de autenticação.

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="SetAuthStateSnippet":::

1. Adicione o seguinte código ao `MainPage()` Construtor **após** a `this.InitializeComponent();` linha.

    ```csharp
    // Initialize auth state to false
    SetAuthState(false);

    // Configure MSAL provider
    // TEMPORARY
    MsalProvider.ClientId = "11111111-1111-1111-1111-111111111111";

    // Navigate to HomePage.xaml
    RootFrame.Navigate(typeof(HomePage));
    ```

    Quando o aplicativo é iniciado pela primeira vez, ele inicializa o estado `false` de autenticação e navega até a Home Page.

1. Adicione o manipulador de eventos a seguir para carregar a página solicitada quando o usuário seleciona um item do modo de exibição de navegação.

    ```csharp
    private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
    {
        var invokedItem = args.InvokedItem as string;

        switch (invokedItem.ToLower())
        {
            case "calendar":
                throw new NotImplementedException();
                break;
            case "home":
            default:
                RootFrame.Navigate(typeof(HomePage));
                break;
        }
    }
    ```

1. Salve todas as suas alterações e, em seguida, pressione **F5** ou selecione **debug > iniciar a depuração** no Visual Studio.

    > [!NOTE]
    > Certifique-se de selecionar a configuração apropriada para seu computador (ARM, x64, x86).

    ![Captura de tela da home page](./images/create-app-01.png)
