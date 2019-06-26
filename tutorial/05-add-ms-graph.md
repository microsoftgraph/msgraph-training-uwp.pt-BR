<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph no aplicativo. Para este aplicativo, você usará a [biblioteca de cliente do Microsoft Graph para .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para fazer chamadas para o Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obter eventos de calendário do Outlook

Comece adicionando uma nova página para o modo de exibição de calendário. Clique com o botão direito do mouse no projeto do **tutorial de gráfico** no Gerenciador de soluções e selecione **Adicionar > novo item...**. Escolha **página em branco**, `CalendarPage.xaml` digite no campo **nome** e selecione **Adicionar**.

Abra `CalendarPage.xaml` e adicione a seguinte linha dentro do elemento `<Grid>` existente.

```xml
<TextBlock x:Name="Events" TextWrapping="Wrap"/>
```

Abra `CalendarPage.xaml.cs` e adicione as seguintes `using` instruções na parte superior do arquivo.

```cs
using Microsoft.Toolkit.Services.MicrosoftGraph;
using Microsoft.Toolkit.Uwp.UI.Controls;
using Newtonsoft.Json;
```

Em seguida, adicione as seguintes funções `CalendarPage` à classe.

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

Considere o que o código `OnNavigatedTo` está fazendo.

- A URL que será chamada é `/v1.0/me/events`.
- A `Select` função limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.
- A `OrderBy` função classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.

Antes de executar o aplicativo, para poder navegar até esta página de calendário, modifique o `NavView_ItemInvoked` método no `MainPage.xaml.cs` arquivo para substituir a `throw new NotImplementedException();` linha pelo da seguinte maneira.

```cs
case "calendar":
    RootFrame.Navigate(typeof(CalendarPage));
    break;
```

Agora, você pode executar o aplicativo, entrar e clicar no item de navegação **calendário** no menu à esquerda. Você deve ver um despejo JSON dos eventos no calendário do usuário.

## <a name="display-the-results"></a>Exibir os resultados

Agora você pode substituir o despejo JSON por algo para exibir os resultados de forma amigável. Substitua todo o conteúdo de `CalendarPage.xaml` com o seguinte.

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

Isso substitui o `TextBlock` por a `DataGrid`. Agora, `CalendarPage.xaml.cs` abra e substitua `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` a linha pelo seguinte.

```cs
EventList.ItemsSource = events.CurrentPage.ToList();
```

Se você executar o aplicativo agora e selecionar o calendário, você deve obter uma lista de eventos em uma grade de dados. No entanto, os valores **inicial** e **final** são exibidos de forma não amigável. Você pode controlar como esses valores são exibidos usando um [conversor de valor](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).

Clique com o botão direito do mouse no projeto do **tutorial de gráfico** no Gerenciador de soluções e selecione **Adicionar > classe...**. Nomeie a classe `GraphDateTimeTimeZoneConverter.cs` e selecione **Adicionar**. Substitua todo o conteúdo do arquivo pelo seguinte.

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

Este código usa a estrutura [dateTimeTimeZone](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/datetimetimezone) retornada pelo Microsoft Graph e o analisa em um `DateTimeOffset` objeto. Em seguida, converte o valor no fuso horário do usuário e retorna o valor formatado.

Abra `CalendarPage.xaml` e adicione o seguinte **antes** do `<Grid>` elemento.

```xml
<Page.Resources>
    <local:GraphDateTimeTimeZoneConverter x:Key="DateTimeTimeZoneValueConverter" />
</Page.Resources>
```

Em seguida, substitua `Binding="{Binding Start.DateTime}"` a linha pelo seguinte.

```xml
Binding="{Binding Start, Converter={StaticResource DateTimeTimeZoneValueConverter}}"
```

Substitua a `Binding="{Binding End.DateTime}"` linha pelo seguinte.

```xml
Binding="{Binding End, Converter={StaticResource DateTimeTimeZoneValueConverter}}"
```

Execute o aplicativo, entre e clique no item de navegação **calendário** . Você deve ver a lista de eventos com os valores **inicial** e **final** formatados.

![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
