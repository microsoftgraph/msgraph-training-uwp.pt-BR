<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4d06c-101">Nesta seção, você criará um novo aplicativo UWP.</span><span class="sxs-lookup"><span data-stu-id="4d06c-101">In this section you'll create a new UWP app.</span></span>

1. <span data-ttu-id="4d06c-102">Abra o Visual Studio e selecione **criar um novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="4d06c-102">Open Visual Studio, and select **Create a new project**.</span></span> <span data-ttu-id="4d06c-103">Escolha a opção **aplicativo em branco (universal do Windows)** que usa C# e, em seguida, selecione **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="4d06c-103">Choose the **Blank App (Universal Windows)** option that uses C#, then select **Next**.</span></span>

    ![Visual Studio 2019 criar nova caixa de diálogo de projeto](./images/vs-create-new-project.png)

1. <span data-ttu-id="4d06c-105">Na caixa de diálogo **configurar seu novo projeto** , `GraphTutorial` Insira o campo **nome do projeto** e selecione **criar**.</span><span class="sxs-lookup"><span data-stu-id="4d06c-105">In the **Configure your new project** dialog, enter `GraphTutorial` in the **Project name** field and select **Create**.</span></span>

    ![Visual Studio 2019 configurar a caixa de diálogo novo projeto](./images/vs-configure-new-project.png)

    > [!IMPORTANT]
    > <span data-ttu-id="4d06c-107">Certifique-se de inserir exatamente o mesmo nome para o projeto do Visual Studio especificado nas instruções do laboratório.</span><span class="sxs-lookup"><span data-stu-id="4d06c-107">Ensure that you enter the exact same name for the Visual Studio Project that is specified in these lab instructions.</span></span> <span data-ttu-id="4d06c-108">O nome do projeto do Visual Studio se torna parte do namespace no código.</span><span class="sxs-lookup"><span data-stu-id="4d06c-108">The Visual Studio Project name becomes part of the namespace in the code.</span></span> <span data-ttu-id="4d06c-109">O código dentro dessas instruções depende do namespace correspondente ao nome do projeto do Visual Studio especificado nessas instruções.</span><span class="sxs-lookup"><span data-stu-id="4d06c-109">The code inside these instructions depends on the namespace matching the Visual Studio Project name specified in these instructions.</span></span> <span data-ttu-id="4d06c-110">Se você usar um nome de projeto diferente, o código não será compilado, a menos que você ajuste todos os namespaces para corresponder ao nome do projeto do Visual Studio que você digitou ao criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="4d06c-110">If you use a different project name the code will not compile unless you adjust all the namespaces to match the Visual Studio Project name you enter when you create the project.</span></span>

1. <span data-ttu-id="4d06c-111">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="4d06c-111">Select **OK**.</span></span> <span data-ttu-id="4d06c-112">Na caixa de diálogo **novo projeto da plataforma universal do Windows** , verifique se a **versão mínima** está definida como `Windows 10, Version 1809 (10.0; Build 17763)` ou posterior e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="4d06c-112">In the **New Universal Windows Platform Project** dialog, ensure that the **Minimum version** is set to `Windows 10, Version 1809 (10.0; Build 17763)` or later and select **OK**.</span></span>

## <a name="install-nuget-packages"></a><span data-ttu-id="4d06c-113">Instalar pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="4d06c-113">Install NuGet packages</span></span>

<span data-ttu-id="4d06c-114">Antes de prosseguir, instale alguns pacotes NuGet adicionais que serão usados posteriormente.</span><span class="sxs-lookup"><span data-stu-id="4d06c-114">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="4d06c-115">[Microsoft. Toolkit. UWP. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls/) para adicionar alguns controles da interface do usuário para notificações no aplicativo e indicadores de carregamento.</span><span class="sxs-lookup"><span data-stu-id="4d06c-115">[Microsoft.Toolkit.Uwp.Ui.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls/) to add some UI controls for in-app notifications and loading indicators.</span></span>
- <span data-ttu-id="4d06c-116">[Microsoft. Toolkit. UWP. UI. Controls. DataGrid](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid/) para exibir as informações retornadas pelo Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="4d06c-116">[Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid/) to display the information returned by Microsoft Graph.</span></span>
- <span data-ttu-id="4d06c-117">[Microsoft. Toolkit. Graph. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Graph.Controls) para lidar com a recuperação de tokens de acesso e logon.</span><span class="sxs-lookup"><span data-stu-id="4d06c-117">[Microsoft.Toolkit.Graph.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Graph.Controls) to handle login and access token retrieval.</span></span>

1. <span data-ttu-id="4d06c-118">Selecione **ferramentas > Gerenciador de pacotes do NuGet > console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="4d06c-118">Select **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="4d06c-119">No console do Gerenciador de pacotes, insira os seguintes comandos.</span><span class="sxs-lookup"><span data-stu-id="4d06c-119">In the Package Manager Console, enter the following commands.</span></span>

    ```powershell
    Install-Package Microsoft.Toolkit.Uwp.Ui.Controls -Version 6.0.0
    Install-Package Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid -Version 6.0.0
    Install-Package Microsoft.Toolkit.Graph.Controls -IncludePrerelease
    ```

## <a name="design-the-app"></a><span data-ttu-id="4d06c-120">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="4d06c-120">Design the app</span></span>

<span data-ttu-id="4d06c-121">Nesta seção, você criará a interface do usuário para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4d06c-121">In this section you'll create the UI for the app.</span></span>

1. <span data-ttu-id="4d06c-122">Comece adicionando uma variável de nível de aplicativo para rastrear o estado de autenticação.</span><span class="sxs-lookup"><span data-stu-id="4d06c-122">Start by adding an application-level variable to track authentication state.</span></span> <span data-ttu-id="4d06c-123">No Gerenciador de soluções, expanda **app. XAML** e abra **app.XAML.cs**.</span><span class="sxs-lookup"><span data-stu-id="4d06c-123">In Solution Explorer, expand **App.xaml** and open **App.xaml.cs**.</span></span> <span data-ttu-id="4d06c-124">Adicione a propriedade a seguir à `App` classe.</span><span class="sxs-lookup"><span data-stu-id="4d06c-124">Add the following property to the `App` class.</span></span>

    ```csharp
    public bool IsAuthenticated { get; set; }
    ```

1. <span data-ttu-id="4d06c-125">Definir o layout da página principal.</span><span class="sxs-lookup"><span data-stu-id="4d06c-125">Define the layout for the main page.</span></span> <span data-ttu-id="4d06c-126">Abra `MainPage.xaml` e substitua todo o conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="4d06c-126">Open `MainPage.xaml` and replace its entire contents with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/MainPage.xaml" id="MainPageXamlSnippet":::

    <span data-ttu-id="4d06c-127">Isso define um [NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) básico com links de navegação de **calendário** e **Home** para atuar como o modo de exibição principal do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4d06c-127">This defines a basic [NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) with **Home** and **Calendar** navigation links to act as the main view of the app.</span></span> <span data-ttu-id="4d06c-128">Ele também adiciona um controle [LoginButton](https://github.com/windows-toolkit/Graph-Controls) no cabeçalho do modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="4d06c-128">It also adds a [LoginButton](https://github.com/windows-toolkit/Graph-Controls) control in the header of the view.</span></span> <span data-ttu-id="4d06c-129">Esse controle permitirá que o usuário entre e saia. O controle ainda não está totalmente habilitado, você irá configurá-lo em um exercício posterior.</span><span class="sxs-lookup"><span data-stu-id="4d06c-129">That control will allow the user to sign in and out. The control isn't fully enabled yet, you will configure it in a later exercise.</span></span>

1. <span data-ttu-id="4d06c-130">Clique com o botão direito do mouse no projeto do **tutorial de gráfico** no Gerenciador de soluções e selecione **Adicionar > novo item...**. Escolha **página em branco**, `HomePage.xaml` digite no campo **nome** e selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="4d06c-130">Right-click the **graph-tutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `HomePage.xaml` in the **Name** field, and select **Add**.</span></span> <span data-ttu-id="4d06c-131">Substitua o elemento `<Grid>` existente no arquivo com o seguinte.</span><span class="sxs-lookup"><span data-stu-id="4d06c-131">Replace the existing `<Grid>` element in the file with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/HomePage.xaml" id="HomePageGridSnippet" highlight="2-5":::

1. <span data-ttu-id="4d06c-132">Expanda **MainPage. XAML** no Gerenciador de soluções `MainPage.xaml.cs`e abra.</span><span class="sxs-lookup"><span data-stu-id="4d06c-132">Expand **MainPage.xaml** in Solution Explorer and open `MainPage.xaml.cs`.</span></span> <span data-ttu-id="4d06c-133">Adicione a função a seguir à `MainPage` classe para gerenciar o estado de autenticação.</span><span class="sxs-lookup"><span data-stu-id="4d06c-133">Add the following function to the `MainPage` class to manage authentication state.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="SetAuthStateSnippet":::

1. <span data-ttu-id="4d06c-134">Adicione o seguinte código ao `MainPage()` Construtor **após** a `this.InitializeComponent();` linha.</span><span class="sxs-lookup"><span data-stu-id="4d06c-134">Add the following code to the `MainPage()` constructor **after** the `this.InitializeComponent();` line.</span></span>

    ```csharp
    // Initialize auth state to false
    SetAuthState(false);

    // Configure MSAL provider
    // TEMPORARY
    MsalProvider.ClientId = "11111111-1111-1111-1111-111111111111";

    // Navigate to HomePage.xaml
    RootFrame.Navigate(typeof(HomePage));
    ```

    <span data-ttu-id="4d06c-135">Quando o aplicativo é iniciado pela primeira vez, ele inicializa o estado `false` de autenticação e navega até a Home Page.</span><span class="sxs-lookup"><span data-stu-id="4d06c-135">When the app first starts, it will initialize the authentication state to `false` and navigate to the home page.</span></span>

1. <span data-ttu-id="4d06c-136">Adicione o manipulador de eventos a seguir para carregar a página solicitada quando o usuário seleciona um item do modo de exibição de navegação.</span><span class="sxs-lookup"><span data-stu-id="4d06c-136">Add the following event handler to load the requested page when the user selects an item from the navigation view.</span></span>

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

1. <span data-ttu-id="4d06c-137">Salve todas as suas alterações e, em seguida, pressione **F5** ou selecione **debug > iniciar a depuração** no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4d06c-137">Save all of your changes, then press **F5** or select **Debug > Start Debugging** in Visual Studio.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4d06c-138">Certifique-se de selecionar a configuração apropriada para seu computador (ARM, x64, x86).</span><span class="sxs-lookup"><span data-stu-id="4d06c-138">Make sure you select the appropriate configuration for your machine (ARM, x64, x86).</span></span>

    ![Captura de tela da home page](./images/create-app-01.png)
