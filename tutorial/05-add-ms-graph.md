<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7a019-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7a019-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="7a019-102">Para este aplicativo, você usará a [biblioteca de cliente do Microsoft Graph para .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="7a019-102">For this application, you will use the [Microsoft Graph Client Library for .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="7a019-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="7a019-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="7a019-104">Adicione uma nova página para o modo de exibição de calendário.</span><span class="sxs-lookup"><span data-stu-id="7a019-104">Add a new page for the calendar view.</span></span> <span data-ttu-id="7a019-105">Clique com o botão direito do mouse no projeto **GraphTutorial** no Gerenciador de soluções e selecione **Adicionar > novo item...**. Escolha **página em branco**, digite `CalendarPage.xaml` no campo **nome** e selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="7a019-105">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `CalendarPage.xaml` in the **Name** field, and select **Add**.</span></span>

1. <span data-ttu-id="7a019-106">Abra `CalendarPage.xaml` e adicione a seguinte linha dentro do `<Grid>` elemento existente.</span><span class="sxs-lookup"><span data-stu-id="7a019-106">Open `CalendarPage.xaml` and add the following line inside the existing `<Grid>` element.</span></span>

    ```xaml
    <TextBlock x:Name="Events" TextWrapping="Wrap"/>
    ```

1. <span data-ttu-id="7a019-107">Abra `CalendarPage.xaml.cs` e adicione as seguintes `using` instruções na parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="7a019-107">Open `CalendarPage.xaml.cs` and add the following `using` statements at the top of the file.</span></span>

    ```csharp
    using Microsoft.Graph;
    using Microsoft.Toolkit.Graph.Providers;
    using Microsoft.Toolkit.Uwp.UI.Controls;
    using Newtonsoft.Json;
    ```

1. <span data-ttu-id="7a019-108">Adicione as seguintes funções à `CalendarPage` classe.</span><span class="sxs-lookup"><span data-stu-id="7a019-108">Add the following functions to the `CalendarPage` class.</span></span>

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
            // Get the user's mailbox settings to determine
            // their time zone
            var user = await graphClient.Me.Request()
                .Select(u => new { u.MailboxSettings })
                .GetAsync();

            var startOfWeek = GetUtcStartOfWeekInTimeZone(DateTime.Today, user.MailboxSettings.TimeZone);
            var endOfWeek = startOfWeek.AddDays(7);

            var queryOptions = new List<QueryOption>
            {
                new QueryOption("startDateTime", startOfWeek.ToString("o")),
                new QueryOption("endDateTime", endOfWeek.ToString("o"))
            };

            // Get the events
            var events = await graphClient.Me.CalendarView.Request(queryOptions)
                .Header("Prefer", $"outlook.timezone=\"{user.MailboxSettings.TimeZone}\"")
                .Select(ev => new
                {
                    ev.Subject,
                    ev.Organizer,
                    ev.Start,
                    ev.End
                })
                .OrderBy("start/dateTime")
                .Top(50)
                .GetAsync();

            // TEMPORARY: Show the results as JSON
            Events.Text = JsonConvert.SerializeObject(events.CurrentPage);
        }
        catch (ServiceException ex)
        {
            ShowNotification($"Exception getting events: {ex.Message}");
        }

        base.OnNavigatedTo(e);
    }

    private static DateTime GetUtcStartOfWeekInTimeZone(DateTime today, string timeZoneId)
    {
        TimeZoneInfo userTimeZone = TimeZoneInfo.FindSystemTimeZoneById(timeZoneId);

        // Assumes Sunday as first day of week
        int diff = System.DayOfWeek.Sunday - today.DayOfWeek;

        // create date as unspecified kind
        var unspecifiedStart = DateTime.SpecifyKind(today.AddDays(diff), DateTimeKind.Unspecified);

        // convert to UTC
        return TimeZoneInfo.ConvertTimeToUtc(unspecifiedStart, userTimeZone);
    }
    ```

    <span data-ttu-id="7a019-109">Considere o que o código `OnNavigatedTo` está fazendo.</span><span class="sxs-lookup"><span data-stu-id="7a019-109">Consider with the code in `OnNavigatedTo` is doing.</span></span>

    - <span data-ttu-id="7a019-110">A URL que será chamada é `/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="7a019-110">The URL that will be called is `/me/calendarview`.</span></span>
        - <span data-ttu-id="7a019-111">Os `startDateTime` `endDateTime` parâmetros e definem o início e o fim do modo de exibição de calendário.</span><span class="sxs-lookup"><span data-stu-id="7a019-111">The `startDateTime` and `endDateTime` parameters define the start and end of the calendar view.</span></span>
        - <span data-ttu-id="7a019-112">O `Prefer: outlook.timezone` cabeçalho faz com que os `start` `end` eventos e sejam retornados no fuso horário do usuário.</span><span class="sxs-lookup"><span data-stu-id="7a019-112">The `Prefer: outlook.timezone` header causes the `start` and `end` of the events to be returned in the user's time zone.</span></span>
        - <span data-ttu-id="7a019-113">A `Select` função limita os campos retornados para cada evento para apenas aqueles que o aplicativo realmente usará.</span><span class="sxs-lookup"><span data-stu-id="7a019-113">The `Select` function limits the fields returned for each event to just those the app will actually use.</span></span>
        - <span data-ttu-id="7a019-114">A `OrderBy` função classifica os resultados pela data e hora de início.</span><span class="sxs-lookup"><span data-stu-id="7a019-114">The `OrderBy` function sorts the results by the start date and time.</span></span>
        - <span data-ttu-id="7a019-115">A `Top` função solicita no máximo 50 eventos.</span><span class="sxs-lookup"><span data-stu-id="7a019-115">The `Top` function requests at most 50 events.</span></span>

1. <span data-ttu-id="7a019-116">Modifique o `NavView_ItemInvoked` método no `MainPage.xaml.cs` arquivo para substituir a instrução existente `switch` pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="7a019-116">Modify the `NavView_ItemInvoked` method in the `MainPage.xaml.cs` file to replace the existing `switch` statement with the following.</span></span>

    ```csharp
    switch (invokedItem.ToLower())
    {
        case "new event":
            throw new NotImplementedException();
            break;
        case "calendar":
            RootFrame.Navigate(typeof(CalendarPage));
            break;
        case "home":
        default:
            RootFrame.Navigate(typeof(HomePage));
            break;
    }
    ```

<span data-ttu-id="7a019-117">Agora, você pode executar o aplicativo, entrar e clicar no item de navegação **calendário** no menu à esquerda.</span><span class="sxs-lookup"><span data-stu-id="7a019-117">You can now run the app, sign in, and click the **Calendar** navigation item in the left-hand menu.</span></span> <span data-ttu-id="7a019-118">Você deve ver um despejo JSON dos eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="7a019-118">You should see a JSON dump of the events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="7a019-119">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="7a019-119">Display the results</span></span>

1. <span data-ttu-id="7a019-120">Substitua todo o conteúdo de `CalendarPage.xaml` com o seguinte.</span><span class="sxs-lookup"><span data-stu-id="7a019-120">Replace the entire contents of `CalendarPage.xaml` with the following.</span></span>

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

1. <span data-ttu-id="7a019-121">Abra `CalendarPage.xaml.cs` e substitua a `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` linha pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="7a019-121">Open `CalendarPage.xaml.cs` and replace the `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` line with the following.</span></span>

    ```csharp
    EventList.ItemsSource = events.CurrentPage.ToList();
    ```

    <span data-ttu-id="7a019-122">Se você executar o aplicativo agora e selecionar o calendário, você deve obter uma lista de eventos em uma grade de dados.</span><span class="sxs-lookup"><span data-stu-id="7a019-122">If you run the app now and select the calendar, you should get a list of events in a data grid.</span></span> <span data-ttu-id="7a019-123">No entanto, os valores **inicial** e **final** são exibidos de forma não amigável.</span><span class="sxs-lookup"><span data-stu-id="7a019-123">However, the **Start** and **End** values are displayed in a non-user-friendly manner.</span></span> <span data-ttu-id="7a019-124">Você pode controlar como esses valores são exibidos usando um [conversor de valor](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).</span><span class="sxs-lookup"><span data-stu-id="7a019-124">You can control how those values are displayed by using a [value converter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).</span></span>

1. <span data-ttu-id="7a019-125">Clique com o botão direito do mouse no projeto **GraphTutorial** no Gerenciador de soluções e selecione **Adicionar > classe...**. Nomeie a classe `GraphDateTimeTimeZoneConverter.cs` e selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="7a019-125">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > Class...**. Name the class `GraphDateTimeTimeZoneConverter.cs` and select **Add**.</span></span> <span data-ttu-id="7a019-126">Substitua todo o conteúdo do arquivo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="7a019-126">Replace the entire contents of the file with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/GraphDateTimeTimeZoneConverter.cs" id="ConverterSnippet":::

    <span data-ttu-id="7a019-127">Este código usa a estrutura [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) retornada pelo Microsoft Graph e o analisa em um `DateTimeOffset` objeto.</span><span class="sxs-lookup"><span data-stu-id="7a019-127">This code takes the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) structure returned by Microsoft Graph and parses it into a `DateTimeOffset` object.</span></span> <span data-ttu-id="7a019-128">Em seguida, converte o valor no fuso horário do usuário e retorna o valor formatado.</span><span class="sxs-lookup"><span data-stu-id="7a019-128">It then converts the value into the user's time zone and returns the formatted value.</span></span>

1. <span data-ttu-id="7a019-129">Abra `CalendarPage.xaml` e adicione o seguinte **antes** do `<Grid>` elemento.</span><span class="sxs-lookup"><span data-stu-id="7a019-129">Open `CalendarPage.xaml` and add the following **before** the `<Grid>` element.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="ResourcesSnippet":::

1. <span data-ttu-id="7a019-130">Substitua os dois últimos `DataGridTextColumn` elementos pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="7a019-130">Replace the last two `DataGridTextColumn` elements with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="BindingSnippet" highlight="4,9":::

1. <span data-ttu-id="7a019-131">Execute o aplicativo, entre e clique no item de navegação **calendário** .</span><span class="sxs-lookup"><span data-stu-id="7a019-131">Run the app, sign in, and click the **Calendar** navigation item.</span></span> <span data-ttu-id="7a019-132">Você deve ver a lista de eventos com os valores **inicial** e **final** formatados.</span><span class="sxs-lookup"><span data-stu-id="7a019-132">You should see the list of events with the **Start** and **End** values formatted.</span></span>

    ![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
