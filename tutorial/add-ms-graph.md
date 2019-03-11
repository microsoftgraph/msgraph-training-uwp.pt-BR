<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ddc64-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ddc64-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="ddc64-102">Para este aplicativo, você usará a [biblioteca de cliente do Microsoft Graph para .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="ddc64-102">For this application, you will use the [Microsoft Graph Client Library for .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="ddc64-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="ddc64-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="ddc64-104">Comece adicionando uma nova página para o modo de exibição de calendário.</span><span class="sxs-lookup"><span data-stu-id="ddc64-104">Start by adding a new page for the calendar view.</span></span> <span data-ttu-id="ddc64-105">Clique com o botão direito do mouse no projeto do **tutorial de gráfico** no Gerenciador de soluções e escolha **Adicionar > novo item...**. Escolha **página em branco**, `CalendarPage.xaml` digite no campo **nome** e escolha **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="ddc64-105">Right-click the **graph-tutorial** project in Solution Explorer and choose **Add > New Item...**. Choose **Blank Page**, enter `CalendarPage.xaml` in the **Name** field, and choose **Add**.</span></span>

<span data-ttu-id="ddc64-106">Abra `CalendarPage.xaml` e adicione a seguinte linha dentro do elemento `<Grid>` existente.</span><span class="sxs-lookup"><span data-stu-id="ddc64-106">Open `CalendarPage.xaml` and add the following line inside the existing `<Grid>` element.</span></span>

```xml
<TextBlock x:Name="Events" TextWrapping="Wrap"/>
```

<span data-ttu-id="ddc64-107">Abra `CalendarPage.xaml.cs` e adicione as seguintes `using` instruções na parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="ddc64-107">Open `CalendarPage.xaml.cs` and add the following `using` statements at the top of the file.</span></span>

```cs
using Microsoft.Toolkit.Services.MicrosoftGraph;
using Microsoft.Toolkit.Uwp.UI.Controls;
using Newtonsoft.Json;
```

<span data-ttu-id="ddc64-108">Em seguida, adicione as seguintes funções `CalendarPage` à classe.</span><span class="sxs-lookup"><span data-stu-id="ddc64-108">Then add the following functions to the `CalendarPage` class.</span></span>

```cs
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
    // Get the Graph client from the service
    var graphClient = MicrosoftGraphService.Instance.GraphProvider;

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

<span data-ttu-id="ddc64-109">Considere o que o código `OnNavigatedTo` está fazendo.</span><span class="sxs-lookup"><span data-stu-id="ddc64-109">Consider with the code in `OnNavigatedTo` is doing.</span></span>

- <span data-ttu-id="ddc64-110">A URL que será chamada é `/v1.0/me/events`.</span><span class="sxs-lookup"><span data-stu-id="ddc64-110">The URL that will be called is `/v1.0/me/events`.</span></span>
- <span data-ttu-id="ddc64-111">A `Select` função limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.</span><span class="sxs-lookup"><span data-stu-id="ddc64-111">The `Select` function limits the fields returned for each events to just those the view will actually use.</span></span>
- <span data-ttu-id="ddc64-112">A `OrderBy` função classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="ddc64-112">The `OrderBy` function sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="ddc64-113">Antes de executar o aplicativo, para poder navegar até esta página de calendário, modifique o `NavView_ItemInvoked` método no `MainPage.xaml.cs` arquivo para substituir a `throw new NotImplementedException();` linha pelo da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="ddc64-113">Just before running the app, in order to be able to navigate to this calendar page, modify the `NavView_ItemInvoked` method in the `MainPage.xaml.cs` file to replace the `throw new NotImplementedException();` line with as follows.</span></span>

```cs
case "calendar":
    RootFrame.Navigate(typeof(CalendarPage));
    break;
```

<span data-ttu-id="ddc64-114">Agora, você pode executar o aplicativo, entrar e clicar no item de navegação **calendário** no menu à esquerda.</span><span class="sxs-lookup"><span data-stu-id="ddc64-114">You can now run the app, sign in, and click the **Calendar** navigation item in the left-hand menu.</span></span> <span data-ttu-id="ddc64-115">Você deve ver um despejo JSON dos eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="ddc64-115">You should see a JSON dump of the events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="ddc64-116">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="ddc64-116">Display the results</span></span>

<span data-ttu-id="ddc64-117">Agora você pode substituir o despejo JSON por algo para exibir os resultados de forma amigável.</span><span class="sxs-lookup"><span data-stu-id="ddc64-117">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="ddc64-118">Substitua todo o conteúdo de `CalendarPage.xaml` com o seguinte.</span><span class="sxs-lookup"><span data-stu-id="ddc64-118">Replace the entire contents of `CalendarPage.xaml` with the following.</span></span>

```xml
<Page
    x:Class="graph_tutorial.CalendarPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:graph_tutorial"
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

<span data-ttu-id="ddc64-119">Isso substitui o `TextBlock` por a `DataGrid`.</span><span class="sxs-lookup"><span data-stu-id="ddc64-119">This replaces the `TextBlock` with a `DataGrid`.</span></span> <span data-ttu-id="ddc64-120">Agora, `CalendarPage.xaml.cs` abra e substitua `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` a linha pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="ddc64-120">Now open `CalendarPage.xaml.cs` and replace the `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` line with the following.</span></span>

```cs
EventList.ItemsSource = events.CurrentPage.ToList();
```

<span data-ttu-id="ddc64-121">Se você executar o aplicativo agora e selecionar o calendário, você deve obter uma lista de eventos em uma grade de dados.</span><span class="sxs-lookup"><span data-stu-id="ddc64-121">If you run the app now and select the calendar, you should get a list of events in a data grid.</span></span> <span data-ttu-id="ddc64-122">No enTanto, os valores **inicial** e **final** são exibidos de forma não amigável.</span><span class="sxs-lookup"><span data-stu-id="ddc64-122">However, the **Start** and **End** values are displayed in a non-user-friendly manner.</span></span> <span data-ttu-id="ddc64-123">Você pode controlar como esses valores são exibidos usando um [conversor de valor](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).</span><span class="sxs-lookup"><span data-stu-id="ddc64-123">You can control how those values are displayed by using a [value converter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).</span></span>

<span data-ttu-id="ddc64-124">Clique com o botão direito do mouse no projeto do **tutorial de gráfico** no Gerenciador de soluções e escolha **Adicionar classe >...**. Nomeie a classe `GraphDateTimeTimeZoneConverter.cs` e escolha **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="ddc64-124">Right-click the **graph-tutorial** project in Solution Explorer and choose **Add > Class...**. Name the class `GraphDateTimeTimeZoneConverter.cs` and choose **Add**.</span></span> <span data-ttu-id="ddc64-125">Substitua todo o conteúdo do arquivo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="ddc64-125">Replace the entire contents of the file with the following.</span></span>

```cs
using Microsoft.Graph;
using System;

namespace graph_tutorial
{
    class GraphDateTimeTimeZoneConverter : Windows.UI.Xaml.Data.IValueConverter
    {
        public object Convert(object value, Type targetType, object parameter, string language)
        {
            DateTimeTimeZone date = value as DateTimeTimeZone;

            if (date != null)
            {
                // Resolve the time zone
                var timezone = TimeZoneInfo.FindSystemTimeZoneById(date.TimeZone);
                // Parse method assumes local time, which may not be the case
                var parsedDateAsLocal = DateTimeOffset.Parse(date.DateTime);
                // Determine the offset from UTC time for the specific date
                // Making this call adjusts for DST as appropriate
                var tzOffset = timezone.GetUtcOffset(parsedDateAsLocal.DateTime);
                // Create a new DateTimeOffset with the specific offset from UTC
                var correctedDate = new DateTimeOffset(parsedDateAsLocal.DateTime, tzOffset);
                // Return the local date time string
                return correctedDate.LocalDateTime.ToString();
            }

            return string.Empty;
        }

        public object ConvertBack(object value, Type targetType, object parameter, string language)
        {
            throw new NotImplementedException();
        }
    }
}
```

<span data-ttu-id="ddc64-126">Este código usa a estrutura [dateTimeTimeZone](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/datetimetimezone) retornada pelo Microsoft Graph e o analisa em um `DateTimeOffset` objeto.</span><span class="sxs-lookup"><span data-stu-id="ddc64-126">This code takes the [dateTimeTimeZone](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/datetimetimezone) structure returned by Microsoft Graph and parses it into a `DateTimeOffset` object.</span></span> <span data-ttu-id="ddc64-127">Em seguida, converte o valor no fuso horário do usuário e retorna o valor formatado.</span><span class="sxs-lookup"><span data-stu-id="ddc64-127">It then converts the value into the user's time zone and returns the formatted value.</span></span>

<span data-ttu-id="ddc64-128">Abra `CalendarPage.xaml` e adicione o seguinte **antes** do `<Grid>` elemento.</span><span class="sxs-lookup"><span data-stu-id="ddc64-128">Open `CalendarPage.xaml` and add the following **before** the `<Grid>` element.</span></span>

```xml
<Page.Resources>
    <local:GraphDateTimeTimeZoneConverter x:Key="DateTimeTimeZoneValueConverter" />
</Page.Resources>
```

<span data-ttu-id="ddc64-129">Em seguida, substitua `Binding="{Binding Start.DateTime}"` a linha pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="ddc64-129">Then, replace the `Binding="{Binding Start.DateTime}"` line with the following.</span></span>

```xml
Binding="{Binding Start, Converter={StaticResource DateTimeTimeZoneValueConverter}}"
```

<span data-ttu-id="ddc64-130">Substitua a `Binding="{Binding End.DateTime}"` linha pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="ddc64-130">Replace the `Binding="{Binding End.DateTime}"` line with the following.</span></span>

```xml
Binding="{Binding End, Converter={StaticResource DateTimeTimeZoneValueConverter}}"
```

<span data-ttu-id="ddc64-131">Execute o aplicativo, entre e clique no item de navegação **calendário** .</span><span class="sxs-lookup"><span data-stu-id="ddc64-131">Run the app, sign in, and click the **Calendar** navigation item.</span></span> <span data-ttu-id="ddc64-132">Você deve ver a lista de eventos com os valores **inicial** e **final** formatados.</span><span class="sxs-lookup"><span data-stu-id="ddc64-132">You should see the list of events with the **Start** and **End** values formatted.</span></span>

![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
