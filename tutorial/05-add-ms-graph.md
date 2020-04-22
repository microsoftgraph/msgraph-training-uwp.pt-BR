<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="328ef-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="328ef-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="328ef-102">Para este aplicativo, você usará a [biblioteca de cliente do Microsoft Graph para .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="328ef-102">For this application, you will use the [Microsoft Graph Client Library for .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="328ef-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="328ef-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="328ef-104">Adicione uma nova página para o modo de exibição de calendário.</span><span class="sxs-lookup"><span data-stu-id="328ef-104">Add a new page for the calendar view.</span></span> <span data-ttu-id="328ef-105">Clique com o botão direito do mouse no projeto **GraphTutorial** no Gerenciador de soluções e selecione **Adicionar > novo item...**. Escolha **página em branco**, `CalendarPage.xaml` digite no campo **nome** e selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="328ef-105">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `CalendarPage.xaml` in the **Name** field, and select **Add**.</span></span>

1. <span data-ttu-id="328ef-106">Abra `CalendarPage.xaml` e adicione a seguinte linha dentro do elemento `<Grid>` existente.</span><span class="sxs-lookup"><span data-stu-id="328ef-106">Open `CalendarPage.xaml` and add the following line inside the existing `<Grid>` element.</span></span>

    ```xaml
    <TextBlock x:Name="Events" TextWrapping="Wrap"/>
    ```

1. <span data-ttu-id="328ef-107">Abra `CalendarPage.xaml.cs` e adicione as seguintes `using` instruções na parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="328ef-107">Open `CalendarPage.xaml.cs` and add the following `using` statements at the top of the file.</span></span>

    ```csharp
    using Microsoft.Toolkit.Graph.Providers;
    using Microsoft.Toolkit.Uwp.UI.Controls;
    using Newtonsoft.Json;
    ```

1. <span data-ttu-id="328ef-108">Adicione as seguintes funções à `CalendarPage` classe.</span><span class="sxs-lookup"><span data-stu-id="328ef-108">Add the following functions to the `CalendarPage` class.</span></span>

    ```csharp
    private void ShowNotification(string message)
    {
        // Get the main page that contains the InAppNotification
        var mainPage = (Window.Current.Content as Frame).Content as MainPage;

        // Get the notification control
        var notification = mainPage.FindName("Notification") as InAppNotification;

        notification.Show(message);
    }

    protected override async void OnNavigatedTo(NavigationEventArgs e)
    {
        // Get the Graph client from the provider
        var graphClient = ProviderManager.Instance.GlobalProvider.Graph;

        try
        {
            // Get the events
            var events = await graphClient.Me.Events.Request()
                .Select("subject,organizer,start,end")
                .OrderBy("createdDateTime DESC")
                .GetAsync();

            // TEMPORARY: Show the results as JSON
            Events.Text = JsonConvert.SerializeObject(events.CurrentPage);
        }
        catch(Microsoft.Graph.ServiceException ex)
        {
            ShowNotification($"Exception getting events: {ex.Message}");
        }

        base.OnNavigatedTo(e);
    }
    ```

    <span data-ttu-id="328ef-109">Considere o que o código `OnNavigatedTo` está fazendo.</span><span class="sxs-lookup"><span data-stu-id="328ef-109">Consider with the code in `OnNavigatedTo` is doing.</span></span>

    - <span data-ttu-id="328ef-110">A URL que será chamada é `/v1.0/me/events`.</span><span class="sxs-lookup"><span data-stu-id="328ef-110">The URL that will be called is `/v1.0/me/events`.</span></span>
    - <span data-ttu-id="328ef-111">A `Select` função limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.</span><span class="sxs-lookup"><span data-stu-id="328ef-111">The `Select` function limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="328ef-112">A `OrderBy` função classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="328ef-112">The `OrderBy` function sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="328ef-113">Modifique o `NavView_ItemInvoked` método no `MainPage.xaml.cs` arquivo para substituir a instrução existente `switch` pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="328ef-113">Modify the `NavView_ItemInvoked` method in the `MainPage.xaml.cs` file to replace the existing `switch` statement with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="SwitchStatementSnippet" highlight="4":::

<span data-ttu-id="328ef-114">Agora, você pode executar o aplicativo, entrar e clicar no item de navegação **calendário** no menu à esquerda.</span><span class="sxs-lookup"><span data-stu-id="328ef-114">You can now run the app, sign in, and click the **Calendar** navigation item in the left-hand menu.</span></span> <span data-ttu-id="328ef-115">Você deve ver um despejo JSON dos eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="328ef-115">You should see a JSON dump of the events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="328ef-116">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="328ef-116">Display the results</span></span>

1. <span data-ttu-id="328ef-117">Substitua todo o conteúdo de `CalendarPage.xaml` com o seguinte.</span><span class="sxs-lookup"><span data-stu-id="328ef-117">Replace the entire contents of `CalendarPage.xaml` with the following.</span></span>

    ```xaml
    <Page
        x:Class="GraphTutorial.CalendarPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:GraphTutorial"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:controls="using:Microsoft.Toolkit.Uwp.UI.Controls"
        mc:Ignorable="d"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <Grid>
            <controls:DataGrid x:Name="EventList" Grid.Row="1"
                    AutoGenerateColumns="False">
                <controls:DataGrid.Columns>
                    <controls:DataGridTextColumn
                            Header="Organizer"
                            Width="SizeToCells"
                            Binding="{Binding Organizer.EmailAddress.Name}"
                            FontSize="20" />
                    <controls:DataGridTextColumn
                            Header="Subject"
                            Width="SizeToCells"
                            Binding="{Binding Subject}"
                            FontSize="20" />
                    <controls:DataGridTextColumn
                            Header="Start"
                            Width="SizeToCells"
                            Binding="{Binding Start.DateTime}"
                            FontSize="20" />
                    <controls:DataGridTextColumn
                            Header="End"
                            Width="SizeToCells"
                            Binding="{Binding End.DateTime}"
                            FontSize="20" />
                </controls:DataGrid.Columns>
            </controls:DataGrid>
        </Grid>
    </Page>
    ```

1. <span data-ttu-id="328ef-118">Abra `CalendarPage.xaml.cs` e substitua a `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` linha pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="328ef-118">Open `CalendarPage.xaml.cs` and replace the `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` line with the following.</span></span>

    ```csharp
    EventList.ItemsSource = events.CurrentPage.ToList();
    ```

    <span data-ttu-id="328ef-119">Se você executar o aplicativo agora e selecionar o calendário, você deve obter uma lista de eventos em uma grade de dados.</span><span class="sxs-lookup"><span data-stu-id="328ef-119">If you run the app now and select the calendar, you should get a list of events in a data grid.</span></span> <span data-ttu-id="328ef-120">No entanto, os valores **inicial** e **final** são exibidos de forma não amigável.</span><span class="sxs-lookup"><span data-stu-id="328ef-120">However, the **Start** and **End** values are displayed in a non-user-friendly manner.</span></span> <span data-ttu-id="328ef-121">Você pode controlar como esses valores são exibidos usando um [conversor de valor](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).</span><span class="sxs-lookup"><span data-stu-id="328ef-121">You can control how those values are displayed by using a [value converter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).</span></span>

1. <span data-ttu-id="328ef-122">Clique com o botão direito do mouse no projeto **GraphTutorial** no Gerenciador de soluções e selecione **Adicionar > classe...**. Nomeie a classe `GraphDateTimeTimeZoneConverter.cs` e selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="328ef-122">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > Class...**. Name the class `GraphDateTimeTimeZoneConverter.cs` and select **Add**.</span></span> <span data-ttu-id="328ef-123">Substitua todo o conteúdo do arquivo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="328ef-123">Replace the entire contents of the file with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/GraphDateTimeTimeZoneConverter.cs" id="ConverterSnippet":::

    <span data-ttu-id="328ef-124">Este código usa a estrutura [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) retornada pelo Microsoft Graph e o analisa em um `DateTimeOffset` objeto.</span><span class="sxs-lookup"><span data-stu-id="328ef-124">This code takes the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) structure returned by Microsoft Graph and parses it into a `DateTimeOffset` object.</span></span> <span data-ttu-id="328ef-125">Em seguida, converte o valor no fuso horário do usuário e retorna o valor formatado.</span><span class="sxs-lookup"><span data-stu-id="328ef-125">It then converts the value into the user's time zone and returns the formatted value.</span></span>

1. <span data-ttu-id="328ef-126">Abra `CalendarPage.xaml` e adicione o seguinte **antes** do `<Grid>` elemento.</span><span class="sxs-lookup"><span data-stu-id="328ef-126">Open `CalendarPage.xaml` and add the following **before** the `<Grid>` element.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="ResourcesSnippet":::

1. <span data-ttu-id="328ef-127">Substitua os dois `DataGridTextColumn` últimos elementos pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="328ef-127">Replace the last two `DataGridTextColumn` elements with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="BindingSnippet" highlight="4,9":::

1. <span data-ttu-id="328ef-128">Execute o aplicativo, entre e clique no item de navegação **calendário** .</span><span class="sxs-lookup"><span data-stu-id="328ef-128">Run the app, sign in, and click the **Calendar** navigation item.</span></span> <span data-ttu-id="328ef-129">Você deve ver a lista de eventos com os valores **inicial** e **final** formatados.</span><span class="sxs-lookup"><span data-stu-id="328ef-129">You should see the list of events with the **Start** and **End** values formatted.</span></span>

    ![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
