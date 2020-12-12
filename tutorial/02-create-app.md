<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4c194-101">Nesta seção, você criará um novo aplicativo UWP.</span><span class="sxs-lookup"><span data-stu-id="4c194-101">In this section you'll create a new UWP app.</span></span>

1. <span data-ttu-id="4c194-102">Abra o Visual Studio e selecione **criar um novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="4c194-102">Open Visual Studio, and select **Create a new project**.</span></span> <span data-ttu-id="4c194-103">Escolha a opção **aplicativo em branco (universal do Windows)** que usa C# e, em seguida, selecione **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="4c194-103">Choose the **Blank App (Universal Windows)** option that uses C#, then select **Next**.</span></span>

    ![Visual Studio 2019 criar nova caixa de diálogo de projeto](./images/vs-create-new-project.png)

1. <span data-ttu-id="4c194-105">Na caixa de diálogo **configurar seu novo projeto** , insira `GraphTutorial` o campo **nome do projeto** e selecione **criar**.</span><span class="sxs-lookup"><span data-stu-id="4c194-105">In the **Configure your new project** dialog, enter `GraphTutorial` in the **Project name** field and select **Create**.</span></span>

    ![Visual Studio 2019 configurar a caixa de diálogo novo projeto](./images/vs-configure-new-project.png)

    > [!IMPORTANT]
    > <span data-ttu-id="4c194-107">Certifique-se de inserir exatamente o mesmo nome para o projeto do Visual Studio especificado nas instruções do laboratório.</span><span class="sxs-lookup"><span data-stu-id="4c194-107">Ensure that you enter the exact same name for the Visual Studio Project that is specified in these lab instructions.</span></span> <span data-ttu-id="4c194-108">O nome do projeto do Visual Studio se torna parte do namespace no código.</span><span class="sxs-lookup"><span data-stu-id="4c194-108">The Visual Studio Project name becomes part of the namespace in the code.</span></span> <span data-ttu-id="4c194-109">O código dentro dessas instruções depende do namespace correspondente ao nome do projeto do Visual Studio especificado nessas instruções.</span><span class="sxs-lookup"><span data-stu-id="4c194-109">The code inside these instructions depends on the namespace matching the Visual Studio Project name specified in these instructions.</span></span> <span data-ttu-id="4c194-110">Se você usar um nome de projeto diferente, o código não será compilado, a menos que você ajuste todos os namespaces para corresponder ao nome do projeto do Visual Studio que você digitou ao criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="4c194-110">If you use a different project name the code will not compile unless you adjust all the namespaces to match the Visual Studio Project name you enter when you create the project.</span></span>

1. <span data-ttu-id="4c194-111">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="4c194-111">Select **OK**.</span></span> <span data-ttu-id="4c194-112">Na caixa de diálogo **novo projeto da plataforma universal do Windows** , verifique se a **versão mínima** está definida como `Windows 10, Version 1809 (10.0; Build 17763)` ou posterior e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="4c194-112">In the **New Universal Windows Platform Project** dialog, ensure that the **Minimum version** is set to `Windows 10, Version 1809 (10.0; Build 17763)` or later and select **OK**.</span></span>

## <a name="install-nuget-packages"></a><span data-ttu-id="4c194-113">Instalar pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="4c194-113">Install NuGet packages</span></span>

<span data-ttu-id="4c194-114">Antes de prosseguir, instale alguns pacotes NuGet adicionais que serão usados posteriormente.</span><span class="sxs-lookup"><span data-stu-id="4c194-114">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="4c194-115">[Microsoft. Toolkit. UWP. UI. Controls. DataGrid](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid/) para exibir as informações retornadas pelo Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="4c194-115">[Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid/) to display the information returned by Microsoft Graph.</span></span>
- <span data-ttu-id="4c194-116">[Microsoft. Toolkit. Graph. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Graph.Controls) para lidar com a recuperação de tokens de acesso e logon.</span><span class="sxs-lookup"><span data-stu-id="4c194-116">[Microsoft.Toolkit.Graph.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Graph.Controls) to handle login and access token retrieval.</span></span>

1. <span data-ttu-id="4c194-117">Selecione **ferramentas > Gerenciador de pacotes do NuGet > console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="4c194-117">Select **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="4c194-118">No console do Gerenciador de pacotes, insira os seguintes comandos.</span><span class="sxs-lookup"><span data-stu-id="4c194-118">In the Package Manager Console, enter the following commands.</span></span>

    ```powershell
    Install-Package Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid -IncludePrerelease
    Install-Package Microsoft.Toolkit.Graph.Controls -IncludePrerelease
    ```

## <a name="design-the-app"></a><span data-ttu-id="4c194-119">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="4c194-119">Design the app</span></span>

<span data-ttu-id="4c194-120">Nesta seção, você criará a interface do usuário para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4c194-120">In this section you'll create the UI for the app.</span></span>

1. <span data-ttu-id="4c194-121">Comece adicionando uma variável de nível de aplicativo para rastrear o estado de autenticação.</span><span class="sxs-lookup"><span data-stu-id="4c194-121">Start by adding an application-level variable to track authentication state.</span></span> <span data-ttu-id="4c194-122">No Gerenciador de soluções, expanda **app. XAML** e abra **app.XAML.cs**.</span><span class="sxs-lookup"><span data-stu-id="4c194-122">In Solution Explorer, expand **App.xaml** and open **App.xaml.cs**.</span></span> <span data-ttu-id="4c194-123">Adicione a propriedade a seguir à `App` classe.</span><span class="sxs-lookup"><span data-stu-id="4c194-123">Add the following property to the `App` class.</span></span>

    ```csharp
    public bool IsAuthenticated { get; set; }
    ```

1. <span data-ttu-id="4c194-124">Definir o layout da página principal.</span><span class="sxs-lookup"><span data-stu-id="4c194-124">Define the layout for the main page.</span></span> <span data-ttu-id="4c194-125">Abra `MainPage.xaml` e substitua todo o conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="4c194-125">Open `MainPage.xaml` and replace its entire contents with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/MainPage.xaml" id="MainPageXamlSnippet":::

    <span data-ttu-id="4c194-126">Isso define um [NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) básico com links de navegação **Home**, **Calendar** e **New Event** para atuar como o modo de exibição principal do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4c194-126">This defines a basic [NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) with **Home**, **Calendar**, and **New event** navigation links to act as the main view of the app.</span></span> <span data-ttu-id="4c194-127">Ele também adiciona um controle [LoginButton](https://github.com/windows-toolkit/Graph-Controls) no cabeçalho do modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="4c194-127">It also adds a [LoginButton](https://github.com/windows-toolkit/Graph-Controls) control in the header of the view.</span></span> <span data-ttu-id="4c194-128">Esse controle permitirá que o usuário entre e saia. O controle ainda não está totalmente habilitado, você irá configurá-lo em um exercício posterior.</span><span class="sxs-lookup"><span data-stu-id="4c194-128">That control will allow the user to sign in and out. The control isn't fully enabled yet, you will configure it in a later exercise.</span></span>

1. <span data-ttu-id="4c194-129">Clique com o botão direito do mouse no projeto do **tutorial de gráfico** no Gerenciador de soluções e selecione **Adicionar > novo item...**. Escolha **página em branco**, digite `HomePage.xaml` no campo **nome** e selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="4c194-129">Right-click the **graph-tutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `HomePage.xaml` in the **Name** field, and select **Add**.</span></span> <span data-ttu-id="4c194-130">Substitua o `<Grid>` elemento existente no arquivo com o seguinte.</span><span class="sxs-lookup"><span data-stu-id="4c194-130">Replace the existing `<Grid>` element in the file with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/HomePage.xaml" id="HomePageGridSnippet" highlight="2-5":::

1. <span data-ttu-id="4c194-131">Expanda **MainPage. XAML** no Gerenciador de soluções e abra `MainPage.xaml.cs` .</span><span class="sxs-lookup"><span data-stu-id="4c194-131">Expand **MainPage.xaml** in Solution Explorer and open `MainPage.xaml.cs`.</span></span> <span data-ttu-id="4c194-132">Adicione a função a seguir à `MainPage` classe para gerenciar o estado de autenticação.</span><span class="sxs-lookup"><span data-stu-id="4c194-132">Add the following function to the `MainPage` class to manage authentication state.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="SetAuthStateSnippet":::

1. <span data-ttu-id="4c194-133">Adicione o seguinte código ao `MainPage()` Construtor **após** a `this.InitializeComponent();` linha.</span><span class="sxs-lookup"><span data-stu-id="4c194-133">Add the following code to the `MainPage()` constructor **after** the `this.InitializeComponent();` line.</span></span>

    ```csharp
    // Initialize auth state to false
    SetAuthState(false);

    // Configure MSAL provider
    // TEMPORARY
    MsalProvider.ClientId = "11111111-1111-1111-1111-111111111111";

    // Navigate to HomePage.xaml
    RootFrame.Navigate(typeof(HomePage));
    ```

    <span data-ttu-id="4c194-134">Quando o aplicativo é iniciado pela primeira vez, ele inicializa o estado de autenticação `false` e navega até a Home Page.</span><span class="sxs-lookup"><span data-stu-id="4c194-134">When the app first starts, it will initialize the authentication state to `false` and navigate to the home page.</span></span>

1. <span data-ttu-id="4c194-135">Adicione o manipulador de eventos a seguir para carregar a página solicitada quando o usuário seleciona um item do modo de exibição de navegação.</span><span class="sxs-lookup"><span data-stu-id="4c194-135">Add the following event handler to load the requested page when the user selects an item from the navigation view.</span></span>

    ```csharp
    private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
    {
        var invokedItem = args.InvokedItem as string;

        switch (invokedItem.ToLower())
        {
            case "new event":
                throw new NotImplementedException();
                break;
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

1. <span data-ttu-id="4c194-136">Salve todas as suas alterações e, em seguida, pressione **F5** ou selecione **debug > iniciar a depuração** no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4c194-136">Save all of your changes, then press **F5** or select **Debug > Start Debugging** in Visual Studio.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4c194-137">Certifique-se de selecionar a configuração apropriada para seu computador (ARM, x64, x86).</span><span class="sxs-lookup"><span data-stu-id="4c194-137">Make sure you select the appropriate configuration for your machine (ARM, x64, x86).</span></span>

    ![Captura de tela da home page](./images/create-app-01.png)
