---
title: Tools für ASP.NET-Core Blazor
author: guardrex
description: Informieren Sie sich über die Tools, die zum Erstellen von Blazor-Apps verfügbar sind.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/07/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/tooling
zone_pivot_groups: operating-systems-set-one
ms.openlocfilehash: 30a76eda0e94ee7bb2b2d3db918bc029865bdf1a
ms.sourcegitcommit: f7873c02c1505c99106cbc708f37e18fc0a496d1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86147645"
---
# <a name="tooling-for-aspnet-core-blazor"></a><span data-ttu-id="ba70e-103">Tools für ASP.NET-Core Blazor</span><span class="sxs-lookup"><span data-stu-id="ba70e-103">Tooling for ASP.NET Core Blazor</span></span>

<span data-ttu-id="ba70e-104">Von [Daniel Roth](https://github.com/danroth27) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ba70e-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

::: zone pivot="os-windows"

1. <span data-ttu-id="ba70e-105">Installieren Sie die neueste Version von [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**.</span><span class="sxs-lookup"><span data-stu-id="ba70e-105">Install the latest version of [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

1. <span data-ttu-id="ba70e-106">Erstellen Sie ein neues Projekt.</span><span class="sxs-lookup"><span data-stu-id="ba70e-106">Create a new project.</span></span>

1. <span data-ttu-id="ba70e-107">Klicken Sie auf **Blazor-App**.</span><span class="sxs-lookup"><span data-stu-id="ba70e-107">Select **Blazor App**.</span></span> <span data-ttu-id="ba70e-108">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="ba70e-108">Select **Next**.</span></span>

1. <span data-ttu-id="ba70e-109">Geben Sie im Feld **Projektname** einen Projektnamen ein, oder übernehmen Sie den Standardnamen.</span><span class="sxs-lookup"><span data-stu-id="ba70e-109">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="ba70e-110">Vergewissern Sie sich, dass der Eintrag für den **Speicherort** korrekt ist, oder geben Sie einen Speicherort für das Projekt an.</span><span class="sxs-lookup"><span data-stu-id="ba70e-110">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="ba70e-111">Wählen Sie **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="ba70e-111">Select **Create**.</span></span>

1. <span data-ttu-id="ba70e-112">Wählen Sie für eine Blazor WebAssembly-Benutzeroberfläche die **Blazor WebAssembly-App-** Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="ba70e-112">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="ba70e-113">Wählen Sie für eine Blazor Server-Benutzeroberfläche die **Blazor Server-App-** Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="ba70e-113">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="ba70e-114">Wählen Sie **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="ba70e-114">Select **Create**.</span></span>

   <span data-ttu-id="ba70e-115">Informationen zu den zwei Blazor-Hostingmodellen ( *Blazor WebAssembly* und *Blazor Server* ) finden Sie unter <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="ba70e-115">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="ba70e-116">Drücken Sie <kbd>STRG</kbd>+<kbd>F5</kbd>, um die App auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ba70e-116">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

<span data-ttu-id="ba70e-117">Weitere Informationen zum Festlegen des ASP.NET Core-HTTPS-Entwicklungszertifikats als vertrauenswürdig finden Sie hier: <xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos>.</span><span class="sxs-lookup"><span data-stu-id="ba70e-117">For more information on trusting the ASP.NET Core HTTPS development certificate, see <xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos>.</span></span>

::: zone-end

::: zone pivot="os-linux"

1. <span data-ttu-id="ba70e-118">Installieren Sie die neueste Version des [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="ba70e-118">Install the latest version of the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span> <span data-ttu-id="ba70e-119">Wenn Sie das SDK zuvor installiert haben, können Sie die installierte Version ermitteln, indem Sie den folgenden Befehl in einer Befehlsshell ausführen:</span><span class="sxs-lookup"><span data-stu-id="ba70e-119">If you previously installed the SDK, you can determine your installed version by executing the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet --version
   ```

1. <span data-ttu-id="ba70e-120">Installieren Sie die neueste Version von [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="ba70e-120">Install the latest version of [Visual Studio Code](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="ba70e-121">Installieren Sie die aktuellste [C# für Visual Studio Code-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span><span class="sxs-lookup"><span data-stu-id="ba70e-121">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span></span>

1. <span data-ttu-id="ba70e-122">Für eine Blazor WebAssembly-Benutzeroberfläche führen Sie den folgenden Befehl in einer Befehlsshell aus:</span><span class="sxs-lookup"><span data-stu-id="ba70e-122">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   <span data-ttu-id="ba70e-123">Für eine Blazor Server-Benutzeroberfläche führen Sie den folgenden Befehl in einer Befehlsshell aus:</span><span class="sxs-lookup"><span data-stu-id="ba70e-123">For a Blazor Server experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   <span data-ttu-id="ba70e-124">Informationen zu den zwei Blazor-Hostingmodellen ( *Blazor WebAssembly* und *Blazor Server* ) finden Sie unter <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="ba70e-124">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="ba70e-125">Öffnen Sie in Visual Studio Code den Ordner `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="ba70e-125">Open the `WebApplication1` folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="ba70e-126">Die IDE fordert an, dass Sie Ressourcen zum Erstellen und Debuggen des Projekts hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ba70e-126">The IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="ba70e-127">Wählen Sie **Ja**.</span><span class="sxs-lookup"><span data-stu-id="ba70e-127">Select **Yes**.</span></span>

1. <span data-ttu-id="ba70e-128">Drücken Sie <kbd>STRG</kbd>+<kbd>F5</kbd>, um die App auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ba70e-128">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

## <a name="trust-a-development-certificate"></a><span data-ttu-id="ba70e-129">Festlegen eines Entwicklungszertifikats als vertrauenswürdig</span><span class="sxs-lookup"><span data-stu-id="ba70e-129">Trust a development certificate</span></span>

<span data-ttu-id="ba70e-130">Es gibt keine zentrale Möglichkeit, ein Zertifikat in Linux als vertrauenswürdig festzulegen.</span><span class="sxs-lookup"><span data-stu-id="ba70e-130">There's no centralized way to trust a certificate on Linux.</span></span> <span data-ttu-id="ba70e-131">In der Regel wird einer der folgenden Ansätze verwendet:</span><span class="sxs-lookup"><span data-stu-id="ba70e-131">Typically, one of the following approaches is adopted:</span></span>

* <span data-ttu-id="ba70e-132">Ausschließen der App-URL über die Ausschlussliste des Browsers.</span><span class="sxs-lookup"><span data-stu-id="ba70e-132">Exclude the app's URL in browser's exclude list.</span></span>
* <span data-ttu-id="ba70e-133">Festlegen aller selbstsignierten Zertifikate für `localhost` als vertrauenswürdig.</span><span class="sxs-lookup"><span data-stu-id="ba70e-133">Trust all self-signed certificates for `localhost`.</span></span>
* <span data-ttu-id="ba70e-134">Hinzufügen des Zertifikats zur Liste der vertrauenswürdigen Zertifikate im Browser.</span><span class="sxs-lookup"><span data-stu-id="ba70e-134">Add the certificate to the list of trusted certificates in the browser.</span></span>

<span data-ttu-id="ba70e-135">Weitere Informationen finden Sie in der Dokumentation zu Ihrem Browser und Ihrer Linux-Distribution.</span><span class="sxs-lookup"><span data-stu-id="ba70e-135">For more information, see the guidance provided by your browser and Linux distro.</span></span>

::: zone-end

::: zone pivot="os-macos"

1. <span data-ttu-id="ba70e-136">Installieren Sie [Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="ba70e-136">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

1. <span data-ttu-id="ba70e-137">Wählen Sie **Datei** > **Neue Projektmappe** aus, oder erstellen Sie im **Startfenster** ein **Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="ba70e-137">Select **File** > **New Solution** or create a **New** project from the **Start Window**.</span></span>

1. <span data-ttu-id="ba70e-138">Klicken Sie auf der Randleiste auf **Web and Console** > **App** (Web und Konsole).</span><span class="sxs-lookup"><span data-stu-id="ba70e-138">In the sidebar, select **Web and Console** > **App**.</span></span>

   <span data-ttu-id="ba70e-139">Wählen Sie für eine Blazor WebAssembly-Benutzeroberfläche die **Blazor WebAssembly-App-** Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="ba70e-139">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="ba70e-140">Wählen Sie für eine Blazor Server-Benutzeroberfläche die **Blazor Server-App-** Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="ba70e-140">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="ba70e-141">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="ba70e-141">Select **Next**.</span></span>

   <span data-ttu-id="ba70e-142">Informationen zu den zwei Blazor-Hostingmodellen ( *Blazor WebAssembly* und *Blazor Server* ) finden Sie unter <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="ba70e-142">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="ba70e-143">Vergewissern Sie sich, dass **Authentifizierung** auf **Keine Authentifizierung** festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="ba70e-143">Confirm that **Authentication** is set to **No Authentication**.</span></span> <span data-ttu-id="ba70e-144">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="ba70e-144">Select **Next**.</span></span>

1. <span data-ttu-id="ba70e-145">Benennen Sie im Feld **Projektname** die App `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="ba70e-145">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="ba70e-146">Wählen Sie **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="ba70e-146">Select **Create**.</span></span>

1. <span data-ttu-id="ba70e-147">Wählen Sie **Ausführen** > **Ohne Debuggen starten** aus, um die App *ohne Debugger* auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ba70e-147">Select **Run** > **Start Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="ba70e-148">Führen Sie die App entweder über **Ausführen** > **Debuggen starten** oder über die Schaltfläche „Ausführen“ (&#9654;) *mit dem Debugger aus*.</span><span class="sxs-lookup"><span data-stu-id="ba70e-148">Run the app with **Run** > **Start Debugging** or the Run (&#9654;) button to run the app *with the debugger*.</span></span>

<span data-ttu-id="ba70e-149">Wenn eine Eingabeaufforderung dem Entwicklungszertifikat zu vertrauen scheint, vertrauen Sie dem Zertifikat und fahren Sie fort.</span><span class="sxs-lookup"><span data-stu-id="ba70e-149">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span> <span data-ttu-id="ba70e-150">Das Benutzer- und Keychainkennwort sind erforderlich, um dem Zertifikat zu vertrauen.</span><span class="sxs-lookup"><span data-stu-id="ba70e-150">The user and keychain passwords are required to trust the certificate.</span></span> <span data-ttu-id="ba70e-151">Weitere Informationen zum Festlegen des ASP.NET Core-HTTPS-Entwicklungszertifikats als vertrauenswürdig finden Sie hier: <xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos>.</span><span class="sxs-lookup"><span data-stu-id="ba70e-151">For more information on trusting the ASP.NET Core HTTPS development certificate, see <xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos>.</span></span>

::: zone-end