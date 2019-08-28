# <a name="aspnet-core-url-rewriting-sample"></a><span data-ttu-id="d6cb5-101">Beispiel für die URL-Umschreibung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6cb5-101">ASP.NET Core URL Rewriting Sample</span></span>

<span data-ttu-id="d6cb5-102">Dieses Beispiel veranschaulicht die Verwendung der URL umschreibenden Middleware in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d6cb5-102">This sample illustrates usage of ASP.NET Core URL Rewriting Middleware.</span></span> <span data-ttu-id="d6cb5-103">In der App werden die Optionen zum Umleiten und Neuschreiben von URLs demonstriert.</span><span class="sxs-lookup"><span data-stu-id="d6cb5-103">The app demonstrates URL redirect and URL rewriting options.</span></span>

<span data-ttu-id="d6cb5-104">Beim Ausführen des Beispiels wird die neu geschriebene oder umgeleitete URL von Antworten ohne Datei zurückgegeben, wenn eine der Regeln auf eine Anforderungs-URL angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="d6cb5-104">When running the sample, non-file responses return the rewritten or redirected URL when one of the rules is applied to a request URL.</span></span> <span data-ttu-id="d6cb5-105">In den Beispielen für XML- und Textdateien, wird die Datei von der Middleware für statische Dateien verarbeitet, nachdem die Anforderungs-URL von der Middleware umgeschrieben wurde.</span><span class="sxs-lookup"><span data-stu-id="d6cb5-105">For the XML and text file examples, Static File Middleware serves the file after the request URL is rewritten by the middleware.</span></span>

## <a name="examples-in-this-sample"></a><span data-ttu-id="d6cb5-106">Beispiele</span><span class="sxs-lookup"><span data-stu-id="d6cb5-106">Examples in this sample</span></span>

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - <span data-ttu-id="d6cb5-107">Statuscode für Erfolg: 302 Found (302 Gefunden)</span><span class="sxs-lookup"><span data-stu-id="d6cb5-107">Success status code: 302 (Found)</span></span>
  - <span data-ttu-id="d6cb5-108">Beispiel (Umleitung): **/redirect-rule/{capture_group}** zu **/redirected/{capture_group}**</span><span class="sxs-lookup"><span data-stu-id="d6cb5-108">Example (redirect): **/redirect-rule/{capture_group}** to **/redirected/{capture_group}**</span></span>
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - <span data-ttu-id="d6cb5-109">Statuscode für Erfolg: 200 (OK)</span><span class="sxs-lookup"><span data-stu-id="d6cb5-109">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="d6cb5-110">Beispiel (Umschreibung): **/rewrite-rule/{capture_group_1}/{capture_group_2}** in **/rewritten?var1={capture_group_1}&var2={capture_group_2}**</span><span class="sxs-lookup"><span data-stu-id="d6cb5-110">Example (rewrite): **/rewrite-rule/{capture_group_1}/{capture_group_2}** to **/rewritten?var1={capture_group_1}&var2={capture_group_2}**</span></span>
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - <span data-ttu-id="d6cb5-111">Statuscode für Erfolg: 302 Found (302 Gefunden)</span><span class="sxs-lookup"><span data-stu-id="d6cb5-111">Success status code: 302 (Found)</span></span>
  - <span data-ttu-id="d6cb5-112">Beispiel (Umleitung): **/apache-mod-rules-redirect/{capture_group}** zu **/redirected?id={capture_group}**</span><span class="sxs-lookup"><span data-stu-id="d6cb5-112">Example (redirect): **/apache-mod-rules-redirect/{capture_group}** to **/redirected?id={capture_group}**</span></span>
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - <span data-ttu-id="d6cb5-113">Statuscode für Erfolg: 200 (OK)</span><span class="sxs-lookup"><span data-stu-id="d6cb5-113">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="d6cb5-114">Beispiel (Umschreibung): **/iis-rules-rewrite/{capture_group}** in **/rewritten?id={capture_group}**</span><span class="sxs-lookup"><span data-stu-id="d6cb5-114">Example (rewrite): **/iis-rules-rewrite/{capture_group}** to **/rewritten?id={capture_group}**</span></span>
* `Add(RedirectXmlFileRequests)`
  - <span data-ttu-id="d6cb5-115">Statuscode für Erfolg: 301 Moved permanently (301 Permanent verschoben)</span><span class="sxs-lookup"><span data-stu-id="d6cb5-115">Success status code: 301 (Moved Permanently)</span></span>
  - <span data-ttu-id="d6cb5-116">Beispiel (Umleitung): **/file.xml** zu **/xmlfiles/file.xml**</span><span class="sxs-lookup"><span data-stu-id="d6cb5-116">Example (redirect): **/file.xml** to **/xmlfiles/file.xml**</span></span>
* `Add(RewriteTextFileRequests)`
  - <span data-ttu-id="d6cb5-117">Statuscode für Erfolg: 200 (OK)</span><span class="sxs-lookup"><span data-stu-id="d6cb5-117">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="d6cb5-118">Beispiel (Neuschreibung): **/some_file.txt** zu **/file.txt**</span><span class="sxs-lookup"><span data-stu-id="d6cb5-118">Example (rewrite): **/some_file.txt** to **/file.txt**</span></span>
* `Add(new RedirectImageRequests(".png", "/png-images")))`<br>`Add(new RedirectImageRequests(".jpg", "/jpg-images")))`
  - <span data-ttu-id="d6cb5-119">Statuscode für Erfolg: 301 Moved permanently (301 Permanent verschoben)</span><span class="sxs-lookup"><span data-stu-id="d6cb5-119">Success status code: 301 (Moved Permanently)</span></span>
  - <span data-ttu-id="d6cb5-120">Beispiel (Umleitung): **/image.png** zu **/png-images/image.png**</span><span class="sxs-lookup"><span data-stu-id="d6cb5-120">Example (redirect): **/image.png** to **/png-images/image.png**</span></span>
  - <span data-ttu-id="d6cb5-121">Beispiel (Umleitung): **/image.jpg** zu **/jpg-images/image.jpg**</span><span class="sxs-lookup"><span data-stu-id="d6cb5-121">Example (redirect): **/image.jpg** to **/jpg-images/image.jpg**</span></span>

## <a name="use-a-physicalfileprovider"></a><span data-ttu-id="d6cb5-122">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="d6cb5-122">Use a PhysicalFileProvider</span></span>

<span data-ttu-id="d6cb5-123">Sie können auch ein `IFileProvider`-Objekt abrufen, indem Sie ein `PhysicalFileProvider`-Objekt erstellen, das an die Methoden `AddApacheModRewrite()` und `AddIISUrlRewrite()` übergeben wird:</span><span class="sxs-lookup"><span data-stu-id="d6cb5-123">You can also obtain an `IFileProvider` by creating a `PhysicalFileProvider` to pass into the `AddApacheModRewrite()` and `AddIISUrlRewrite()` methods:</span></span>

```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```

## <a name="secure-redirection-extensions"></a><span data-ttu-id="d6cb5-124">Sichere Umleitungserweiterungen</span><span class="sxs-lookup"><span data-stu-id="d6cb5-124">Secure redirection extensions</span></span>

<span data-ttu-id="d6cb5-125">Dieses Beispiel enthält eine `WebHostBuilder`-Konfiguration für die App zur Verwendung von URLs (`https://localhost:5001`, `https://localhost`) und ein Testzertifikat (*testCert.pfx*) als Unterstützung beim Erkunden der sicheren Umleitungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="d6cb5-125">This sample includes `WebHostBuilder` configuration for the app to use URLs (`https://localhost:5001`, `https://localhost`) and a test certificate (*testCert.pfx*) to assist in exploring the secure redirect methods.</span></span> <span data-ttu-id="d6cb5-126">Wenn dem Server Port 443 bereits zugewiesen wurde oder dieser verwendet wird, funktioniert das `https://localhost`-Beispiel nicht. &mdash; Entfernen Sie `ListenOptions` für Port 443 in der `CreateWebHostBuilder`-Methode der *Program.cs*-Datei oder lösen Sie Port 443 aus dem Server heraus, sodass der Port von Kestrel verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="d6cb5-126">If the server already has port 443 assigned or in use, the `https://localhost` example doesn't work&mdash;remove the `ListenOptions` for port 443 in the `CreateWebHostBuilder` method of the *Program.cs* file or unbind port 443 on the server so that Kestrel can use the port.</span></span>

| <span data-ttu-id="d6cb5-127">Methode</span><span class="sxs-lookup"><span data-stu-id="d6cb5-127">Method</span></span>                           | <span data-ttu-id="d6cb5-128">Statuscode</span><span class="sxs-lookup"><span data-stu-id="d6cb5-128">Status Code</span></span> |    <span data-ttu-id="d6cb5-129">Port</span><span class="sxs-lookup"><span data-stu-id="d6cb5-129">Port</span></span>    |
| -------------------------------- | :---------: | :--------: |
| `.AddRedirectToHttpsPermanent()` |     <span data-ttu-id="d6cb5-130">301</span><span class="sxs-lookup"><span data-stu-id="d6cb5-130">301</span></span>     | <span data-ttu-id="d6cb5-131">NULL (465)</span><span class="sxs-lookup"><span data-stu-id="d6cb5-131">null (465)</span></span> |
| `.AddRedirectToHttps()`          |     <span data-ttu-id="d6cb5-132">302</span><span class="sxs-lookup"><span data-stu-id="d6cb5-132">302</span></span>     | <span data-ttu-id="d6cb5-133">NULL (465)</span><span class="sxs-lookup"><span data-stu-id="d6cb5-133">null (465)</span></span> |
| `.AddRedirectToHttps(301)`       |     <span data-ttu-id="d6cb5-134">301</span><span class="sxs-lookup"><span data-stu-id="d6cb5-134">301</span></span>     | <span data-ttu-id="d6cb5-135">NULL (465)</span><span class="sxs-lookup"><span data-stu-id="d6cb5-135">null (465)</span></span> |
| `.AddRedirectToHttps(301, 5001)` |     <span data-ttu-id="d6cb5-136">301</span><span class="sxs-lookup"><span data-stu-id="d6cb5-136">301</span></span>     |    <span data-ttu-id="d6cb5-137">5001</span><span class="sxs-lookup"><span data-stu-id="d6cb5-137">5001</span></span>    |