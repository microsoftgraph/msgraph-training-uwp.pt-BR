<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="de173-101">Nesta seção, você adicionará a capacidade de criar eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="de173-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="de173-102">Adicione uma nova página para o novo modo de exibição de eventos.</span><span class="sxs-lookup"><span data-stu-id="de173-102">Add a new page for the new event view.</span></span> <span data-ttu-id="de173-103">Clique com o botão direito do mouse no projeto **GraphTutorial** no Gerenciador de soluções e selecione **Adicionar > novo item...**. Escolha **página em branco**, digite `NewEventPage.xaml` no campo **nome** e selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="de173-103">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `NewEventPage.xaml` in the **Name** field, and select **Add**.</span></span>

1. <span data-ttu-id="de173-104">Abra **NewEventPage. XAML** e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="de173-104">Open **NewEventPage.xaml** and replace its contents with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/NewEventPage.xaml" id="NewEventPageXamlSnippet":::

1. <span data-ttu-id="de173-105">Abra o **NewEventPage.XAML.cs** e adicione as seguintes `using` instruções à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="de173-105">Open **NewEventPage.xaml.cs** and add the following `using` statements to the top of the file.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="UsingStatementsSnippet":::

1. <span data-ttu-id="de173-106">Adicione a interface **INotifyPropertyChange** à classe **NewEventPage** .</span><span class="sxs-lookup"><span data-stu-id="de173-106">Add the **INotifyPropertyChange** interface to the **NewEventPage** class.</span></span> <span data-ttu-id="de173-107">Substitua a declaração de classe existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="de173-107">Replace the existing class declaration with the following.</span></span>

    ```csharp
    public sealed partial class NewEventPage : Page, INotifyPropertyChanged
    {
        public NewEventPage()
        {
            this.InitializeComponent();
            DataContext = this;
        }
    }
    ```

1. <span data-ttu-id="de173-108">Adicione as propriedades a seguir à classe **NewEventPage** .</span><span class="sxs-lookup"><span data-stu-id="de173-108">Add the following properties to the **NewEventPage** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="PropertiesSnippet":::

1. <span data-ttu-id="de173-109">Adicione o código a seguir para obter o fuso horário do usuário do Microsoft Graph quando a página for carregada.</span><span class="sxs-lookup"><span data-stu-id="de173-109">Add the following code to get the user's time zone from Microsoft Graph when the page loads.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="LoadTimeZoneSnippet":::

1. <span data-ttu-id="de173-110">Adicione o código a seguir para criar o evento.</span><span class="sxs-lookup"><span data-stu-id="de173-110">Add the following code to create the event.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="CreateEventSnippet":::

1. <span data-ttu-id="de173-111">Modifique o `NavView_ItemInvoked` método no arquivo **MainPage.XAML.cs** para substituir a instrução existente `switch` pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="de173-111">Modify the `NavView_ItemInvoked` method in the **MainPage.xaml.cs** file to replace the existing `switch` statement with the following.</span></span>

    ```csharp
    switch (invokedItem.ToLower())
    {
        case "new event":
            RootFrame.Navigate(typeof(NewEventPage));
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

1. <span data-ttu-id="de173-112">Salve suas alterações e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="de173-112">Save your changes and run the app.</span></span> <span data-ttu-id="de173-113">Entre, selecione o novo item de menu de **eventos** , preencha o formulário e selecione **criar** para adicionar um evento ao calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="de173-113">Sign in, select the **New event** menu item, fill in the form, and select **Create** to add an event to the user's calendar.</span></span>

    ![Uma captura de tela da página novo evento](images/create-event-01.png)
