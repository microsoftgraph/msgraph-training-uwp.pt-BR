<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7a26c-101">Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7a26c-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="7a26c-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="7a26c-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="7a26c-103">Nesta etapa, você integrará o controle [AadLogin](https://docs.microsoft.com/dotnet/api/microsoft.toolkit.uwp.ui.controls.graph.aadlogin?view=win-comm-toolkit-dotnet-stable) do [Kit de ferramentas da Comunidade do Windows](https://github.com/Microsoft/WindowsCommunityToolkit) no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7a26c-103">In this step you will integrate the [AadLogin](https://docs.microsoft.com/dotnet/api/microsoft.toolkit.uwp.ui.controls.graph.aadlogin?view=win-comm-toolkit-dotnet-stable) control from the [Windows Community Toolkit](https://github.com/Microsoft/WindowsCommunityToolkit) into the application.</span></span>

<span data-ttu-id="7a26c-104">Clique com o botão direito do mouse no projeto do **tutorial de gráfico** no Gerenciador de soluções e escolha **Adicionar > novo item...**. Escolha **arquivo de recursos (. resw)**, nomeie o `OAuth.resw` arquivo e escolha **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="7a26c-104">Right-click the **graph-tutorial** project in Solution Explorer and choose **Add > New Item...**. Choose **Resources File (.resw)**, name the file `OAuth.resw` and choose **Add**.</span></span> <span data-ttu-id="7a26c-105">Quando o novo arquivo é aberto no Visual Studio, crie dois recursos da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="7a26c-105">When the new file opens in Visual Studio, create two resources as follows.</span></span>

- <span data-ttu-id="7a26c-106">**Nome:** `AppId`, **Valor:** a ID do aplicativo que você gerou no portal de registro do aplicativo</span><span class="sxs-lookup"><span data-stu-id="7a26c-106">**Name:** `AppId`, **Value:** the app ID you generated in Application Registration Portal</span></span>
- <span data-ttu-id="7a26c-107">**Nome:** `Scopes`, **Valor:**`User.Read Calendars.Read`</span><span class="sxs-lookup"><span data-stu-id="7a26c-107">**Name:** `Scopes`, **Value:** `User.Read Calendars.Read`</span></span>

![Uma captura de tela do arquivo OAuth. resw no editor do Visual Studio](./images/edit-resources-01.png)

> [!IMPORTANT]
> <span data-ttu-id="7a26c-109">Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir `OAuth.resw` o arquivo do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7a26c-109">If you're using source control such as git, now would be a good time to exclude the `OAuth.resw` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="configure-the-aadlogin-control"></a><span data-ttu-id="7a26c-110">Configurar o controle AadLogin</span><span class="sxs-lookup"><span data-stu-id="7a26c-110">Configure the AadLogin control</span></span>

<span data-ttu-id="7a26c-111">Comece adicionando código para ler os valores do arquivo de recursos.</span><span class="sxs-lookup"><span data-stu-id="7a26c-111">Start by adding code to read the values out of the resources file.</span></span> <span data-ttu-id="7a26c-112">Abra `MainPage.xaml.cs` e adicione a seguinte `using` instrução à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="7a26c-112">Open `MainPage.xaml.cs` and add the following `using` statement to the top of the file.</span></span>

```cs
using Microsoft.Toolkit.Services.MicrosoftGraph;
```

<span data-ttu-id="7a26c-113">Substitua a linha `RootFrame.Navigate(typeof(HomePage));` pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="7a26c-113">Replace the `RootFrame.Navigate(typeof(HomePage));` line with the following code.</span></span>

```cs
// Load OAuth settings
var oauthSettings = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("OAuth");
var appId = oauthSettings.GetString("AppId");
var scopes = oauthSettings.GetString("Scopes");

if (string.IsNullOrEmpty(appId) || string.IsNullOrEmpty(scopes))
{
    Notification.Show("Could not load OAuth Settings from resource file.");
}
else
{
    // Initialize Graph
    MicrosoftGraphService.Instance.AuthenticationModel = MicrosoftGraphEnums.AuthenticationModel.V2;
    MicrosoftGraphService.Instance.Initialize(appId,
        MicrosoftGraphEnums.ServicesToInitialize.UserProfile,
        scopes.Split(' '));

    // Navigate to HomePage.xaml
    RootFrame.Navigate(typeof(HomePage));
}
```

<span data-ttu-id="7a26c-114">Este código carrega as configurações de `OAuth.resw` e inicializa a instância global do `MicrosoftGraphService` com esses valores.</span><span class="sxs-lookup"><span data-stu-id="7a26c-114">This code loads the settings from `OAuth.resw` and initializes the global instance of the `MicrosoftGraphService` with those values.</span></span>

<span data-ttu-id="7a26c-115">Agora, adicione um manipulador de eventos `SignInCompleted` para o evento `AadLogin` no controle.</span><span class="sxs-lookup"><span data-stu-id="7a26c-115">Now add an event handler for the `SignInCompleted` event on the `AadLogin` control.</span></span> <span data-ttu-id="7a26c-116">Abra o `MainPage.xaml` arquivo e substitua o elemento `<graphControls:AadLogin>` existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="7a26c-116">Open the `MainPage.xaml` file and replace the existing `<graphControls:AadLogin>` element with the following.</span></span>

```xml
<graphControls:AadLogin x:Name="Login"
    HorizontalAlignment="Left"
    View="SmallProfilePhotoLeft"
    AllowSignInAsDifferentUser="False"
    SignInCompleted="Login_SignInCompleted"
    SignOutCompleted="Login_SignOutCompleted"
    />
```

<span data-ttu-id="7a26c-117">Em seguida, adicione as seguintes funções `MainPage` à classe `MainPage.xaml.cs`no.</span><span class="sxs-lookup"><span data-stu-id="7a26c-117">Then add the following functions to the `MainPage` class in `MainPage.xaml.cs`.</span></span>

```cs
private void Login_SignInCompleted(object sender, Microsoft.Toolkit.Uwp.UI.Controls.Graph.SignInEventArgs e)
{
    // Set the auth state
    SetAuthState(true);
    // Reload the home page
    RootFrame.Navigate(typeof(HomePage));
}

private void Login_SignOutCompleted(object sender, EventArgs e)
{
    // Set the auth state
    SetAuthState(false);
    // Reload the home page
    RootFrame.Navigate(typeof(HomePage));
}
```

<span data-ttu-id="7a26c-118">Por fim, no Gerenciador de soluções, expanda **homepage. XAML** e abrir `HomePage.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="7a26c-118">Finally, in Solution Explorer, expand **HomePage.xaml** and open `HomePage.xaml.cs`.</span></span> <span data-ttu-id="7a26c-119">Adicione o seguinte código após a `this.InitializeComponent();` linha.</span><span class="sxs-lookup"><span data-stu-id="7a26c-119">Add the following code after the `this.InitializeComponent();` line.</span></span>

```cs
if ((App.Current as App).IsAuthenticated)
{
    HomePageMessage.Text = "Welcome! Please use the menu to the left to select a view.";
}
```

<span data-ttu-id="7a26c-120">ReInicie o aplicativo e clique no controle de **entrada** na parte superior do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7a26c-120">Restart the app and click the **Sign In** control at the top of the app.</span></span> <span data-ttu-id="7a26c-121">Depois que você entrar, a interface do usuário deverá mudar para indicar que você entrou com êxito.</span><span class="sxs-lookup"><span data-stu-id="7a26c-121">Once you've signed in, the UI should change to indicate that you've successfully signed-in.</span></span>

![Uma captura de tela do aplicativo após entrar](./images/add-aad-auth-01.png)

> [!NOTE]
> <span data-ttu-id="7a26c-123">O `AadLogin` controle implementa a lógica de armazenamento e atualização do token de acesso para você.</span><span class="sxs-lookup"><span data-stu-id="7a26c-123">The `AadLogin` control implements the logic of storing and refreshing the access token for you.</span></span> <span data-ttu-id="7a26c-124">Os tokens são armazenados no armazenamento seguro e atualizados conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="7a26c-124">The tokens are stored in secure storage and refreshed as needed.</span></span>