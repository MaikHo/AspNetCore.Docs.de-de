---
title: Hosten und Bereitstellen von clientseitigem Blazor
author: guardrex
description: Erfahren Sie, wie Sie eine Blazor-App mithilfe von ASP.NET Core, Content Delivery Network (CDN), Dateiservern und GitHub-Seiten hosten und bereitstellen.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/client-side
ms.openlocfilehash: 01a612029f415f583908c3bf2adc2e6d35167acb
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614717"
---
# <a name="host-and-deploy-blazor-client-side"></a><span data-ttu-id="4fe1a-103">Hosten und Bereitstellen von clientseitigem Blazor</span><span class="sxs-lookup"><span data-stu-id="4fe1a-103">Host and deploy Blazor client-side</span></span>

<span data-ttu-id="4fe1a-104">Von [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) und [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="4fe1a-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="host-configuration-values"></a><span data-ttu-id="4fe1a-105">Hostkonfigurationswerte</span><span class="sxs-lookup"><span data-stu-id="4fe1a-105">Host configuration values</span></span>

<span data-ttu-id="4fe1a-106">Bei Blazor-Apps, für die das [clientseitige Hostingmodell](xref:blazor/hosting-models#client-side-hosting-model) verwendet wird, können die folgenden Hostkonfigurationswerte als Befehlszeilenargumente zur Laufzeit in der Entwicklungsumgebung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-106">Blazor apps that use the [client-side hosting model](xref:blazor/hosting-models#client-side-hosting-model) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="4fe1a-107">Inhaltsstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="4fe1a-107">Content Root</span></span>

<span data-ttu-id="4fe1a-108">Mit dem Argument `--contentroot` wird der absolute Pfad zu dem Verzeichnis festgelegt, das die Inhaltsdateien der App enthält.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-108">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span> <span data-ttu-id="4fe1a-109">In den folgenden Beispielen ist `/content-root-path` der Stammpfad für Inhalte der App.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-109">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="4fe1a-110">Das Argument wird beim lokalen Ausführen der App bei einer Eingabeaufforderung übergeben.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-110">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="4fe1a-111">Führen Sie den folgenden Befehl über das Verzeichnis der App aus:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-111">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="4fe1a-112">Fügen Sie der Datei *launchSettings.json* der App im **IIS Express**-Profil einen Eintrag hinzu.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-112">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="4fe1a-113">Diese Einstellung wird verwendet, wenn die Anwendung mit dem Visual Studio-Debugger und an einer Eingabeaufforderung mit `dotnet run` ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-113">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="4fe1a-114">Geben Sie in Visual Studio das Argument in **Eigenschaften** > **Debuggen** > **Anwendungsargumente** ein.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-114">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="4fe1a-115">Wenn Sie das Argument auf der Visual Studio-Eigenschaftenseite festlegen, wird das Argument der Datei *launchSettings.json* hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-115">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="4fe1a-116">Basispfad</span><span class="sxs-lookup"><span data-stu-id="4fe1a-116">Path base</span></span>

<span data-ttu-id="4fe1a-117">Mit dem Argument `--pathbase` wird der Basispfad für eine App festgelegt, die lokal mit einem virtuellen Pfad ausgeführt wird, der sich nicht im Stammverzeichnis befindet (d. h. das `<base>`-Tag `href` wird für das Staging und die Produktion auf einen anderen Pfad als `/` festgelegt).</span><span class="sxs-lookup"><span data-stu-id="4fe1a-117">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="4fe1a-118">In den folgenden Beispielen ist `/virtual-path` die Pfadbasis der App.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-118">In the following examples, `/virtual-path` is the app's path base.</span></span> <span data-ttu-id="4fe1a-119">Weitere Informationen finden Sie im Abschnitt [Basispfad einer App](#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="4fe1a-119">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4fe1a-120">Im Gegensatz zu dem Pfad, der für `href` des `<base>`-Tags bereitgestellt wird, wird beim Übergeben des `--pathbase`-Argumentwerts kein Schrägstrich (`/`) nachgestellt.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-120">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="4fe1a-121">Wenn der Basispfad einer App im `<base>`-Tag als `<base href="/CoolApp/">` (mit nachgestelltem Schrägstrich) bereitgestellt wird, wird der Argumentwert in der Befehlszeile als `--pathbase=/CoolApp` (ohne nachgestellten Schrägstrich) übergeben.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-121">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="4fe1a-122">Das Argument wird beim lokalen Ausführen der App bei einer Eingabeaufforderung übergeben.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-122">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="4fe1a-123">Führen Sie den folgenden Befehl über das Verzeichnis der App aus:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-123">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/virtual-path
  ```

* <span data-ttu-id="4fe1a-124">Fügen Sie der Datei *launchSettings.json* der App im **IIS Express**-Profil einen Eintrag hinzu.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-124">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="4fe1a-125">Diese Einstellung wird verwendet, wenn die Anwendung mit dem Visual Studio-Debugger und an einer Eingabeaufforderung mit `dotnet run` ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-125">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/virtual-path"
  ```

* <span data-ttu-id="4fe1a-126">Geben Sie in Visual Studio das Argument in **Eigenschaften** > **Debuggen** > **Anwendungsargumente** ein.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-126">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="4fe1a-127">Wenn Sie das Argument auf der Visual Studio-Eigenschaftenseite festlegen, wird das Argument der Datei *launchSettings.json* hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-127">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/virtual-path
  ```

### <a name="urls"></a><span data-ttu-id="4fe1a-128">URLs</span><span class="sxs-lookup"><span data-stu-id="4fe1a-128">URLs</span></span>

<span data-ttu-id="4fe1a-129">Mit dem Argument `--urls` werden die IP-oder Hostadressen mit Ports und Protokollen festgelegt, die auf Anforderungen lauschen sollen.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-129">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="4fe1a-130">Das Argument wird beim lokalen Ausführen der App bei einer Eingabeaufforderung übergeben.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-130">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="4fe1a-131">Führen Sie den folgenden Befehl über das Verzeichnis der App aus:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-131">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="4fe1a-132">Fügen Sie der Datei *launchSettings.json* der App im **IIS Express**-Profil einen Eintrag hinzu.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-132">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="4fe1a-133">Diese Einstellung wird verwendet, wenn die Anwendung mit dem Visual Studio-Debugger und an einer Eingabeaufforderung mit `dotnet run` ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-133">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="4fe1a-134">Geben Sie in Visual Studio das Argument in **Eigenschaften** > **Debuggen** > **Anwendungsargumente** ein.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-134">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="4fe1a-135">Wenn Sie das Argument auf der Visual Studio-Eigenschaftenseite festlegen, wird das Argument der Datei *launchSettings.json* hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-135">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deployment"></a><span data-ttu-id="4fe1a-136">Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="4fe1a-136">Deployment</span></span>

<span data-ttu-id="4fe1a-137">Mit dem [clientseitigen Hostingmodell](xref:blazor/hosting-models#client-side-hosting-model) ist Folgendes möglich:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-137">With the [client-side hosting model](xref:blazor/hosting-models#client-side-hosting-model):</span></span>

* <span data-ttu-id="4fe1a-138">Die Blazor-App, die jeweiligen Abhängigkeiten und die .NET-Runtime werden im Browser herunterladen.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-138">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="4fe1a-139">Die App wird direkt im UI-Thread des Browsers ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-139">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="4fe1a-140">Dabei wird eine der folgenden beiden Strategien unterstützt:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-140">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="4fe1a-141">Die Blazor-App wird von einer ASP.NET Core-App unterstützt.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-141">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="4fe1a-142">Diese Strategie wird im Abschnitt [Gehostete Bereitstellung mit ASP.NET Core](#hosted-deployment-with-aspnet-core) behandelt.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-142">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="4fe1a-143">Die Blazor-App wird auf einem statischen Hosting-Webserver oder -Webservice abgelegt, auf dem .NET nicht zur Unterstützung der Blazor-App verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-143">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="4fe1a-144">Diese Strategie wird im Abschnitt [Eigenständige Bereitstellung](#standalone-deployment) behandelt.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-144">This strategy is covered in the [Standalone deployment](#standalone-deployment) section.</span></span>

## <a name="configure-the-linker"></a><span data-ttu-id="4fe1a-145">Konfigurieren des Linkers</span><span class="sxs-lookup"><span data-stu-id="4fe1a-145">Configure the Linker</span></span>

<span data-ttu-id="4fe1a-146">Blazor führt auf jedem Build eine IL-Verknüpfung (Intermediate Language, Zwischensprache) durch, um nicht benötigte IL aus den Ausgabeassemblys zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-146">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="4fe1a-147">Verknüpfungen mit Assemblys können beim Erstellen des Builds gesteuert werden.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-147">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="4fe1a-148">Weitere Informationen finden Sie unter <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-148">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="4fe1a-149">Erneutes Generieren von URLs für ein ordnungsgemäßes Routing</span><span class="sxs-lookup"><span data-stu-id="4fe1a-149">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="4fe1a-150">Routinganforderungen für Seitenkomponenten in einer clientseitigen App sind etwas komplexer als Routinganforderungen an eine serverseitige, gehostete App.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-150">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="4fe1a-151">Betrachten wir eine clientseitige App mit zwei Seiten:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-151">Consider a client-side app with two pages:</span></span>

* <span data-ttu-id="4fe1a-152">**_Main.cshtml:_** Lädt den Anwendungsstamm und enthält einen Link zur Infoseite (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="4fe1a-152">**_Main.cshtml_** &ndash; Loads at the root of the app and contains a link to the About page (`href="About"`).</span></span>
* <span data-ttu-id="4fe1a-153">**_About.cshtml:_** Infoseite.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-153">**_About.cshtml_** &ndash; About page.</span></span>

<span data-ttu-id="4fe1a-154">Wenn das Standarddokument der App über die Adressleiste des Browsers (z. B. `https://www.contoso.com/`) angefordert wird, geschieht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-154">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="4fe1a-155">Der Browser sendet eine Anforderung.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-155">The browser makes a request.</span></span>
1. <span data-ttu-id="4fe1a-156">Die Standardseite wird zurückgegeben, in der Regel *index.html*.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-156">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="4fe1a-157">*index.html* startet die App.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-157">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="4fe1a-158">Der Router von Blazor wird geladen, und die Hauptseite von Razor (*Main.cshtml*) wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-158">Blazor's router loads, and the Razor Main page (*Main.cshtml*) is displayed.</span></span>

<span data-ttu-id="4fe1a-159">Wenn auf der Hauptseite der Link zur Infoseite ausgewählt wird, wird die Infoseite geladen.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-159">On the Main page, selecting the link to the About page loads the About page.</span></span> <span data-ttu-id="4fe1a-160">Der Link zur Infoseite kann auf dem Client ausgewählt werden, da der Blazor-Router dafür sorgt, dass der Browser im Internet keine Anforderung für `About` an `www.contoso.com` sendet, und stattdessen die Infoseite selbst bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-160">Selecting the link to the About page works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the About page itself.</span></span> <span data-ttu-id="4fe1a-161">Alle Anforderungen von internen Seiten *innerhalb der clientseitigen App* funktionieren auf dieselbe Weise: Durch Anforderungen werden keine browserbasierten Anforderungen an serverseitig gehostete Ressourcen im Internet ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-161">All of the requests for internal pages *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="4fe1a-162">Der Router verarbeitet Anforderungen intern.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-162">The router handles the requests internally.</span></span>

<span data-ttu-id="4fe1a-163">Wenn eine Anforderung für `www.contoso.com/About` über die Adressleiste des Browsers gesendet wird, tritt bei der Anforderung ein Fehler auf.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-163">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="4fe1a-164">Diese Ressource ist im Internethost der App nicht vorhanden. Daher wird die Antwort *404 Nicht gefunden* zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-164">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="4fe1a-165">Da Browser Anforderungen für clientseitige Seiten an internetbasierte Hosts senden, müssen Webserver und Hostingdienste alle Anforderungen für Ressourcen, die sich nicht physisch auf dem Server befinden, auf der Seite *index.html* erneut generieren.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-165">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="4fe1a-166">Wenn *index.html* zurückgegeben wird, übernimmt der clientseitige Router der App und antwortet mit der richtigen Ressource.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-166">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="4fe1a-167">Basispfad einer App</span><span class="sxs-lookup"><span data-stu-id="4fe1a-167">App base path</span></span>

<span data-ttu-id="4fe1a-168">Beim *Basispfad einer App* handelt es sich um den virtuellen Stammpfad der App auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-168">The *app base path* is the virtual app root path on the server.</span></span> <span data-ttu-id="4fe1a-169">Eine App, die sich beispielsweise auf dem Contoso-Server in einem virtuellen Ordner unter `/CoolApp/` befindet, wird unter `https://www.contoso.com/CoolApp` erreicht, wobei der virtuelle Basispfad `/CoolApp/` lautet.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-169">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="4fe1a-170">Indem der Basispfad der App auf `CoolApp/` festgelegt wird, erkennt die App, wo sie sich virtuell auf dem Server befindet.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-170">By setting the app base path to `CoolApp/`, the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="4fe1a-171">Die App kann den Basispfad der App verwenden, um URLs relativ zum Stammverzeichnis der App über eine Komponente zu erstellen, die sich nicht im Stammverzeichnis befindet.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-171">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="4fe1a-172">Dadurch ist es möglich, dass Komponenten, die sich in unterschiedlichen Ebenen der Verzeichnisstruktur befinden, Links zu anderen Ressourcen an Speicherorten in der gesamten App erstellen können.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-172">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="4fe1a-173">Ferner wird der Basispfad der App verwendet, um Klicks auf Links abzufangen, bei denen sich das `href`-Ziel des Links innerhalb des URI-Raums für den Basispfad der App befindet – um die interne Navigation kümmert sich der Blazor-Router.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-173">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="4fe1a-174">Bei vielen Hostingszenarios ist der virtuelle Pfad des Servers zur App das Stammverzeichnis der App.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-174">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="4fe1a-175">In diesen Fällen ist der Basispfad der App ein Schrägstrich (`<base href="/" />`). Hierbei handelt es sich um die Standardkonfiguration für eine App.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-175">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="4fe1a-176">Bei anderen Hostingszenarios wie etwa bei GitHub-Seiten und virtuellen IIS-Verzeichnissen oder untergeordneten Anwendungen muss der Basispfad der App auf den virtuellen Pfad des Servers zur App festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-176">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="4fe1a-177">Zum Festlegen des Basispfads der App fügen Sie das `<base>`-Tag in *index.html* innerhalb der `<head>`-Tagelemente hinzu bzw. aktualisieren es.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-177">To set the app's base path, add or update the `<base>` tag in *index.html* found within the `<head>` tag elements.</span></span> <span data-ttu-id="4fe1a-178">Legen Sie den `href`-Attributwert auf `virtual-path/` (der nachgestellte Schrägstrich ist erforderlich) fest, wobei `virtual-path/` der vollständige virtuelle Stammpfad der App auf dem Server ist.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-178">Set the `href` attribute value to `virtual-path/` (the trailing slash is required), where `virtual-path/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="4fe1a-179">Im vorherigen Beispiel wurde der virtuelle Pfad auf `CoolApp/` festgelegt: `<base href="CoolApp/">`.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-179">In the preceding example, the virtual path is set to `CoolApp/`: `<base href="CoolApp/">`.</span></span>

<span data-ttu-id="4fe1a-180">Bei einer App, für die ein virtueller Pfad konfiguriert wurde, der sich nicht im Stammverzeichnis befindet (z. B. `<base href="CoolApp/">`), werden die Ressourcen der App nicht gefunden, *wenn die App lokal ausgeführt wird*.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-180">For an app with a non-root virtual path configured (for example, `<base href="CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="4fe1a-181">Um dieses Problem bei der lokalen Entwicklung und bei Tests zu beheben, können Sie ein *Pfadbasis*-Argument bereitstellen, das dem `href`-Wert des `<base>`-Tags zur Laufzeit entspricht.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-181">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="4fe1a-182">Damit das Pfadbasis-Argument bei der lokalen Ausführung der App mit dem Stammpfad (`/`) übergeben wird, führen Sie den folgenden Befehl über das Verzeichnis der App aus:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-182">To pass the path base argument with the root path (`/`) when running the app locally, execute the following command from the app's directory:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="4fe1a-183">Die App antwortet lokal unter `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-183">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="4fe1a-184">Weitere Informationen finden Sie im Abschnitt zum [Hostkonfigurationswert für die Pfadbasis](#path-base).</span><span class="sxs-lookup"><span data-stu-id="4fe1a-184">For more information, see the section on the [path base host configuration value](#path-base).</span></span>

<span data-ttu-id="4fe1a-185">Wenn für eine App das [clientseitige Hostingmodell](xref:blazor/hosting-models#client-side-hosting-model) (basierend auf der **Blazor**-Projektvorlage) verwendet wird und die App als untergeordnete IIS-Anwendung in einer ASP.NET Core-App gehostet wird, muss der geerbte ASP.NET Core Module-Handler deaktiviert werden, oder es muss sichergestellt werden, dass der `<handlers>`-Abschnitt der Stammanwendung (also der übergeordneten Anwendung) in der Datei *web.config* von der untergeordneten Anwendung nicht geerbt wird.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-185">If an app uses the [client-side hosting model](xref:blazor/hosting-models#client-side-hosting-model) (based on the **Blazor** project template) and is hosted as an IIS sub-application in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>

<span data-ttu-id="4fe1a-186">Entfernen Sie den Handler in der veröffentlichen *web.config*-Datei der App, indem Sie der Datei einen `<handlers>`-Abschnitt hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-186">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

```xml
<handlers>
  <remove name="aspNetCore" />
</handlers>
```

<span data-ttu-id="4fe1a-187">Alternativ können Sie auch die Vererbung des `<system.webServer>`-Abschnitts der Stammanwendung (d. h. der übergeordneten Anwendung) deaktivieren, indem Sie ein `<location>`-Element verwenden, bei dem `inheritInChildApplications` auf `false` festgelegt ist:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-187">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" ... />
      </handlers>
      <aspNetCore ... />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="4fe1a-188">Das Entfernen des Handlers bzw. das Deaktivieren der Vererbung wird zusätzlich zu der in diesem Abschnitt beschriebenen Konfiguration des Basispfads der App durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-188">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="4fe1a-189">Legen Sie den Basispfad der App in der *index.html*-Datei der App auf den IIS-Alias fest, der beim Konfigurieren der untergeordneten App in IIS verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-189">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="4fe1a-190">Gehostete Bereitstellung mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4fe1a-190">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="4fe1a-191">Mit einer *gehosteten Bereitstellung* wird die clientseitige Blazor-App über eine auf einem Server ausgeführte [ASP.NET Core-App](xref:index) für Browser bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-191">A *hosted deployment* serves the client-side Blazor app to browsers from an [ASP.NET Core app](xref:index) that runs on a server.</span></span>

<span data-ttu-id="4fe1a-192">Die Blazor-App ist mit der ASP.NET Core-App in der veröffentlichen Ausgabe enthalten, sodass die beiden Apps zusammen bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-192">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="4fe1a-193">Hierfür wird ein Webserver benötigt, auf dem eine ASP.NET Core-App gehostet werden kann.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-193">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="4fe1a-194">Bei einer gehosteten Bereitstellung enthält Visual Studio die **Blazor-Projektvorlage (in ASP.NET Core gehostet)** (`blazorhosted`-Vorlage bei Verwendung des Befehls [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="4fe1a-194">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="4fe1a-195">Weitere Informationen zum Hosten und Bereitstellen von ASP.NET Core-Apps finden Sie unter <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-195">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="4fe1a-196">Informationen zum Bereitstellen für Azure App Service finden Sie unter <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-196">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="4fe1a-197">Eigenständige Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="4fe1a-197">Standalone deployment</span></span>

<span data-ttu-id="4fe1a-198">Mit einer *eigenständigen Bereitstellung* wird die clientseitige Blazor-App in Form von statischen Dateien bereitgestellt, die von Clients direkt angefordert werden.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-198">A *standalone deployment* serves the client-side Blazor app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="4fe1a-199">Für die Bereitstellung der Blazor-App wird kein Webserver verwendet.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-199">A web server isn't used to serve the Blazor app.</span></span>

### <a name="iis"></a><span data-ttu-id="4fe1a-200">IIS</span><span class="sxs-lookup"><span data-stu-id="4fe1a-200">IIS</span></span>

<span data-ttu-id="4fe1a-201">IIS ist ein leistungsfähiger Server für statische Dateien für Blazor-Apps.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-201">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="4fe1a-202">Informationen zum Konfigurieren von IIS zum Hosten von Blazor finden Sie unter [Build a Static Website on IIS (Erstellen einer statischen Website unter IIS)](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="4fe1a-202">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="4fe1a-203">Veröffentlichte Objekte werden im Ordner */bin/Release/{Zielframework}/publish* erstellt.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-203">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="4fe1a-204">Die Inhalte des Ordners *publish* werden auf dem Webserver oder über den Hostingdienst gehostet.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-204">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="4fe1a-205">web.config</span><span class="sxs-lookup"><span data-stu-id="4fe1a-205">web.config</span></span>

<span data-ttu-id="4fe1a-206">Beim Veröffentlichen eines Blazor-Projekts wird eine *web.config*-Datei mit der folgenden IIS-Konfiguration erstellt:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-206">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="4fe1a-207">Für die folgenden Dateierweiterungen werden MIME-Typen festgelegt:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-207">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="4fe1a-208">*.dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="4fe1a-208">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="4fe1a-209">*.json* &ndash; `application/json`</span><span class="sxs-lookup"><span data-stu-id="4fe1a-209">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="4fe1a-210">*.wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="4fe1a-210">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="4fe1a-211">*.woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="4fe1a-211">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="4fe1a-212">*.woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="4fe1a-212">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="4fe1a-213">Für die folgenden MIME-Typen wird die HTTP-Komprimierung aktiviert:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-213">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="4fe1a-214">URL-Rewrite-Modul-Regeln werden eingerichtet:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-214">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="4fe1a-215">Stellen Sie das Unterverzeichnis bereit, in dem sich die statischen Objekte der App befinden (*{ASSEMBLYNAME}/dist/{ANGEFORDERTER PFAD}*).</span><span class="sxs-lookup"><span data-stu-id="4fe1a-215">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="4fe1a-216">Richten Sie ein SPA-Fallbackrouting ein, sodass Anforderungen für nicht dateibasierte Objekte an das Standarddokument der App im entsprechenden Ordner für statische Objekte (*{ASSEMBLYNAME}/dist/index.html*) umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-216">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="4fe1a-217">Installieren des URL-Rewrite-Moduls</span><span class="sxs-lookup"><span data-stu-id="4fe1a-217">Install the URL Rewrite Module</span></span>

<span data-ttu-id="4fe1a-218">Das [URL-Rewrite-Modul](https://www.iis.net/downloads/microsoft/url-rewrite) wird zum Umschreiben von URLs benötigt.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-218">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="4fe1a-219">Das Modul wird nicht standardmäßig installiert und ist für die Installation als Webserver (IIS)-Rollendienstfunktion nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-219">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="4fe1a-220">Das Modul muss von der IIS-Website heruntergeladen werden.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-220">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="4fe1a-221">Verwenden Sie den Webplattform-Installer zur Installation des Moduls:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-221">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="4fe1a-222">Navigieren Sie lokal zur [URL-Rewrite-Module-Downloadseite](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="4fe1a-222">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="4fe1a-223">Wählen Sie zum Herunterladen der englischen Version des WebPI-Installers **WebPI** aus.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-223">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="4fe1a-224">Wählen Sie zum Herunterladen des Installers in einer anderen Sprache die entsprechende Architektur für den Server (x86/x64) aus.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-224">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="4fe1a-225">Kopieren Sie den Installer auf den Server.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-225">Copy the installer to the server.</span></span> <span data-ttu-id="4fe1a-226">Führen Sie den Installer aus.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-226">Run the installer.</span></span> <span data-ttu-id="4fe1a-227">Klicken Sie auf die Schaltfläche **Installieren**, und stimmen Sie den Lizenzbedingungen zu.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-227">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="4fe1a-228">Der Server muss nach Abschluss der Installation nicht neu gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-228">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="4fe1a-229">Konfigurieren der Website</span><span class="sxs-lookup"><span data-stu-id="4fe1a-229">Configure the website</span></span>

<span data-ttu-id="4fe1a-230">Legen Sie für den **physischen Pfad** der Website den Ordner der App fest.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-230">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="4fe1a-231">Der Ordner enthält Folgendes:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-231">The folder contains:</span></span>

* <span data-ttu-id="4fe1a-232">Die Datei *web.config*, die von IIS zum Konfigurieren der Website verwendet wird, mit den erforderlichen Umleitungsregeln und Dateiinhaltstypen</span><span class="sxs-lookup"><span data-stu-id="4fe1a-232">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="4fe1a-233">Den Ordner der App für statische Objekte</span><span class="sxs-lookup"><span data-stu-id="4fe1a-233">The app's static asset folder.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="4fe1a-234">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="4fe1a-234">Troubleshooting</span></span>

<span data-ttu-id="4fe1a-235">Wenn die Fehlermeldung *500: Interner Serverfehler* angezeigt wird und IIS-Manager beim Zugriff auf die Konfiguration der Website eine Fehlermeldung anzeigt, überprüfen Sie, ob das URL-Rewrite-Modul installiert ist.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-235">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="4fe1a-236">Wenn das Modul nicht installiert ist, kann die Datei *web.config* von IIS nicht analysiert werden.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-236">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="4fe1a-237">Dadurch wird verhindert, dass die Konfiguration der Website von IIS-Manager geladen wird, und dass die statischen Blazor-Dateien auf der Website bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-237">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="4fe1a-238">Weitere Informationen zur Problembehandlung von Bereitstellungen für IIS finden Sie unter <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-238">For more information on troubleshooting deployments to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>

### <a name="nginx"></a><span data-ttu-id="4fe1a-239">Nginx</span><span class="sxs-lookup"><span data-stu-id="4fe1a-239">Nginx</span></span>

<span data-ttu-id="4fe1a-240">Die folgende *nginx.conf*-Datei wurde vereinfacht, um zu zeigen, wie Nginx so konfiguriert wird, dass die Datei *Index.html* gesendet wird, wenn auf der Festplatte keine entsprechende Datei gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-240">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

<span data-ttu-id="4fe1a-241">Weitere Informationen zur Nginx-Webserverkonfiguration für die Produktion finden Sie unter [Creating NGINX Plus and NGINX Configuration Files (Erstellen von Konfigurationsdateien für NGINX Plus und NGINX)](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span><span class="sxs-lookup"><span data-stu-id="4fe1a-241">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="4fe1a-242">Nginx in Docker</span><span class="sxs-lookup"><span data-stu-id="4fe1a-242">Nginx in Docker</span></span>

<span data-ttu-id="4fe1a-243">Richten Sie zum Hosten von Blazor in Docker mithilfe von Nginx die Dockerfile für die Verwendung des auf Alpine basierenden Nginx-Images ein.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-243">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="4fe1a-244">Aktualisieren Sie die Dockerfile zum Kopieren der Datei *nginx.config* in den Container.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-244">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="4fe1a-245">Fügen Sie der Dockerfile eine Zeile hinzu, wie im folgenden Beispiel dargestellt:</span><span class="sxs-lookup"><span data-stu-id="4fe1a-245">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a><span data-ttu-id="4fe1a-246">GitHub-Seiten</span><span class="sxs-lookup"><span data-stu-id="4fe1a-246">GitHub Pages</span></span>

<span data-ttu-id="4fe1a-247">Fügen Sie zum Verarbeiten von URL-Umschreibungen eine *404.html*-Datei mit einem Skript hinzu, mit dem die Umleitung der Anforderung an die Seite *index.html* verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-247">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="4fe1a-248">Ein Beispiel für eine Implementierung, das von der Community bereitgestellt wurde, finden Sie unter [Single Page Apps for GitHub Pages (Single-Page-Apps für GitHub Pages)](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages auf GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="4fe1a-248">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="4fe1a-249">Ein Beispiel für die Verwendung des Community-Ansatzes finden Sie unter [blazor-demo/blazor-demo.github.io auf GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([Livewebsite](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="4fe1a-249">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="4fe1a-250">Wenn Sie anstelle einer Organisationswebsite eine Projektwebsite verwenden, fügen Sie der Datei *index.html* das `<base>`-Tag hinzu, oder aktualisieren Sie das Tag in der Datei.</span><span class="sxs-lookup"><span data-stu-id="4fe1a-250">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="4fe1a-251">Legen Sie den Wert des `href`-Attributs auf den Namen des GitHub-Repositorys mit einem nachfolgenden Slash fest (z.B. `my-repository/`).</span><span class="sxs-lookup"><span data-stu-id="4fe1a-251">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>