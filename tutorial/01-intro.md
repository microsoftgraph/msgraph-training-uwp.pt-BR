<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="bc89e-101">Este tutorial ensina como criar um aplicativo da plataforma universal do Windows (UWP) que usa a API do Microsoft Graph para recuperar informações de calendário de um usuário.</span><span class="sxs-lookup"><span data-stu-id="bc89e-101">This tutorial teaches you how to build a Universal Windows Platform (UWP) app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="bc89e-102">Se preferir baixar o tutorial concluído, você poderá baixar ou clonar o repositório do [GitHub](https://github.com/microsoftgraph/msgraph-training-uwp).</span><span class="sxs-lookup"><span data-stu-id="bc89e-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-uwp).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc89e-103">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="bc89e-103">Prerequisites</span></span>

<span data-ttu-id="bc89e-104">Antes de iniciar este tutorial, você deve ter o [Visual Studio](https://visualstudio.microsoft.com/vs/) instalado em um computador executando o Windows 10 com o [modo de desenvolvedor ativado](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).</span><span class="sxs-lookup"><span data-stu-id="bc89e-104">Before you start this tutorial, you should have [Visual Studio](https://visualstudio.microsoft.com/vs/) installed on a computer running Windows 10 with [Developer mode turned on](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).</span></span> <span data-ttu-id="bc89e-105">Se você não tiver o Visual Studio, visite o link anterior para opções de download.</span><span class="sxs-lookup"><span data-stu-id="bc89e-105">If you do not have Visual Studio, visit the previous link for download options.</span></span>

<span data-ttu-id="bc89e-106">Você também deve ter uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bc89e-106">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="bc89e-107">Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="bc89e-107">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="bc89e-108">Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="bc89e-108">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="bc89e-109">Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="bc89e-109">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="bc89e-110">Este tutorial foi escrito com o Visual Studio 2019 versão 16.5.0.</span><span class="sxs-lookup"><span data-stu-id="bc89e-110">This tutorial was written with Visual Studio 2019 version 16.5.0.</span></span> <span data-ttu-id="bc89e-111">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="bc89e-111">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="watch-the-tutorial"></a><span data-ttu-id="bc89e-112">Assista ao tutorial</span><span class="sxs-lookup"><span data-stu-id="bc89e-112">Watch the tutorial</span></span>

<span data-ttu-id="bc89e-113">Este módulo foi gravado e está disponível no canal do Office Development YouTube.</span><span class="sxs-lookup"><span data-stu-id="bc89e-113">This module has been recorded and is available in the Office Development YouTube channel.</span></span>

<!-- markdownlint-disable MD033 MD034 -->
<br/>

> [!VIDEO https://www.youtube-nocookie.com/embed/oBYCBxkWMRA]
<!-- markdownlint-enable MD033 MD034 -->

## <a name="feedback"></a><span data-ttu-id="bc89e-114">Comentários</span><span class="sxs-lookup"><span data-stu-id="bc89e-114">Feedback</span></span>

<span data-ttu-id="bc89e-115">Forneça comentários sobre este tutorial no [repositório do GitHub](https://github.com/microsoftgraph/msgraph-training-uwp).</span><span class="sxs-lookup"><span data-stu-id="bc89e-115">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-uwp).</span></span>
