<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Nesta etapa, você integrará o controle [AadLogin](https://docs.microsoft.com/dotnet/api/microsoft.toolkit.uwp.ui.controls.graph.aadlogin?view=win-comm-toolkit-dotnet-stable) do [Kit de ferramentas da Comunidade do Windows](https://github.com/Microsoft/WindowsCommunityToolkit) no aplicativo.

Clique com o botão direito do mouse no projeto do **tutorial de gráfico** no Gerenciador de soluções e selecione **Adicionar > novo item...**. Escolha **arquivo de recursos (. resw)**, nomeie o `OAuth.resw` arquivo e selecione **Adicionar**. Quando o novo arquivo é aberto no Visual Studio, crie dois recursos da seguinte maneira.

- **Nome:** `AppId`, **Valor:** a ID do aplicativo que você gerou no portal de registro do aplicativo
- **Nome:** `Scopes`, **Valor:**`User.Read Calendars.Read`

![Uma captura de tela do arquivo OAuth. resw no editor do Visual Studio](./images/edit-resources-01.png)

> [!IMPORTANT]
> Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir `OAuth.resw` o arquivo do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.

## <a name="configure-the-aadlogin-control"></a>Configurar o controle AadLogin

Comece adicionando código para ler os valores do arquivo de recursos. Abra `MainPage.xaml.cs` e adicione a seguinte `using` instrução à parte superior do arquivo.

```cs
using Microsoft.Toolkit.Services.MicrosoftGraph;
```

Substitua a linha `RootFrame.Navigate(typeof(HomePage));` pelo código a seguir.

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

Este código carrega as configurações de `OAuth.resw` e inicializa a instância global do `MicrosoftGraphService` com esses valores.

Agora, adicione um manipulador de eventos `SignInCompleted` para o evento `AadLogin` no controle. Abra o `MainPage.xaml` arquivo e substitua o elemento `<graphControls:AadLogin>` existente pelo seguinte.

```xml
<graphControls:AadLogin x:Name="Login"
    HorizontalAlignment="Left"
    View="SmallProfilePhotoLeft"
    AllowSignInAsDifferentUser="False"
    SignInCompleted="Login_SignInCompleted"
    SignOutCompleted="Login_SignOutCompleted"
    />
```

Em seguida, adicione as seguintes funções `MainPage` à classe `MainPage.xaml.cs`no.

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

Por fim, no Gerenciador de soluções, expanda **homepage. XAML** e abrir `HomePage.xaml.cs`. Adicione o seguinte código após a `this.InitializeComponent();` linha.

```cs
if ((App.Current as App).IsAuthenticated)
{
    HomePageMessage.Text = "Welcome! Please use the menu to the left to select a view.";
}
```

Reinicie o aplicativo e clique no controle de **entrada** na parte superior do aplicativo. Depois que você entrar, a interface do usuário deverá mudar para indicar que você entrou com êxito.

![Uma captura de tela do aplicativo após entrar](./images/add-aad-auth-01.png)

> [!NOTE]
> O `AadLogin` controle implementa a lógica de armazenamento e atualização do token de acesso para você. Os tokens são armazenados no armazenamento seguro e atualizados conforme necessário.
