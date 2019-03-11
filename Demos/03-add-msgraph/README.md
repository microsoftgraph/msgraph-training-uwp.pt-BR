# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="694b8-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="694b8-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="694b8-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="694b8-102">Prerequisites</span></span>

<span data-ttu-id="694b8-103">Para executar o projeto concluído nessa pasta, você precisará do seguinte:</span><span class="sxs-lookup"><span data-stu-id="694b8-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="694b8-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="694b8-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) installed on your development machine.</span></span> <span data-ttu-id="694b8-105">Se você não tiver o Visual Studio, visite o link anterior para opções de download.</span><span class="sxs-lookup"><span data-stu-id="694b8-105">If you do not have Visual Studio, visit the previous link for download options.</span></span> <span data-ttu-id="694b8-106">(**Observação:** este tutorial foi escrito com o Visual Studio 2017 versão 15,81.</span><span class="sxs-lookup"><span data-stu-id="694b8-106">(**Note:** This tutorial was written with Visual Studio 2017 version 15.81.</span></span> <span data-ttu-id="694b8-107">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="694b8-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="694b8-108">Uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="694b8-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="694b8-109">Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="694b8-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="694b8-110">Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="694b8-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="694b8-111">Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="694b8-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-native-application-with-the-application-registration-portal"></a><span data-ttu-id="694b8-112">Registrar um aplicativo nativo com o portal de registro do aplicativo</span><span class="sxs-lookup"><span data-stu-id="694b8-112">Register a native application with the Application Registration Portal</span></span>

1. <span data-ttu-id="694b8-113">Abra um navegador e navegue até o [portal de registro do aplicativo](https://apps.dev.microsoft.com) e faça logon usando uma **conta pessoal** (aka: conta da Microsoft) ou **conta corporativa ou de estudante**.</span><span class="sxs-lookup"><span data-stu-id="694b8-113">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="694b8-114">Selecione **Adicionar um aplicativo** na parte superior da página.</span><span class="sxs-lookup"><span data-stu-id="694b8-114">Select **Add an app** at the top of the page.</span></span>

    > <span data-ttu-id="694b8-115">**Observação:** Se você vir mais de um botão **Adicionar um aplicativo** na página, selecione aquele que corresponde à lista **aplicativos convergidos** .</span><span class="sxs-lookup"><span data-stu-id="694b8-115">**Note:** If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="694b8-116">Na página **registrar seu aplicativo** , configure o **nome do aplicativo** para o tutorial de **gráfico do UWP** e selecione **criar**.</span><span class="sxs-lookup"><span data-stu-id="694b8-116">On the **Register your application** page, set the **Application Name** to **UWP Graph Tutorial** and select **Create**.</span></span>

    ![Captura de tela da criação de um novo aplicativo no site do portal de registro de aplicativo](../../../Images/arp-create-app-01.png)

1. <span data-ttu-id="694b8-118">Na página **registro do tutorial do gráfico UWP** , na seção **Propriedades** , copie a **ID do aplicativo** , pois você precisará dela mais tarde.</span><span class="sxs-lookup"><span data-stu-id="694b8-118">On the **UWP Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Captura de tela da ID do aplicativo recém-criado](../../../Images/arp-create-app-02.png)

1. <span data-ttu-id="694b8-120">Role para baixo até a seção **plataformas** .</span><span class="sxs-lookup"><span data-stu-id="694b8-120">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="694b8-121">Selecione **Adicionar plataforma**.</span><span class="sxs-lookup"><span data-stu-id="694b8-121">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="694b8-122">Na caixa de diálogo **Adicionar plataforma** , selecione **aplicativo nativo**.</span><span class="sxs-lookup"><span data-stu-id="694b8-122">In the **Add Platform** dialog, select **Native Application**.</span></span>

        ![Captura de tela criando uma plataforma para o aplicativo](../../../Images/arp-create-app-03.png)

1. <span data-ttu-id="694b8-124">Role até a parte inferior da página e selecione **salvar**.</span><span class="sxs-lookup"><span data-stu-id="694b8-124">Scroll to the bottom of the page and select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="694b8-125">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="694b8-125">Configure the sample</span></span>

1. <span data-ttu-id="694b8-126">ReNomear `OAuth.resw.example` o `OAuth.resw`arquivo para.</span><span class="sxs-lookup"><span data-stu-id="694b8-126">Rename the `OAuth.resw.example` file to `OAuth.resw`.</span></span>
1. <span data-ttu-id="694b8-127">Abra `graph-tutorial.sln` no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="694b8-127">Open `graph-tutorial.sln` in Visual Studio.</span></span>
1. <span data-ttu-id="694b8-128">Edite `OAuth.resw` o arquivo no Visual Studio. Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="694b8-128">Edit the `OAuth.resw` file in visual studio.Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="694b8-129">No Gerenciador de soluções, clique com o botão direito do mouse na solução do **tutorial do gráfico** e escolha **restaurar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="694b8-129">In Solution Explorer, right-click the **graph-tutorial** solution and choose **Restore NuGet Packages**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="694b8-130">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="694b8-130">Run the sample</span></span>

<span data-ttu-id="694b8-131">No Visual Studio, pressione **F5** ou escolha **depurar > iniciar depuração**.</span><span class="sxs-lookup"><span data-stu-id="694b8-131">In Visual Studio, press **F5** or choose **Debug > Start Debugging**.</span></span>