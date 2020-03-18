---
title: Erweiterte ASP.NET Core-Blazor-Szenarios
author: guardrex
description: Informieren Sie sich über erweiterte Szenarios in Blazor einschließlich der Integration manueller RenderTreeBuilder-Logik in eine App.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/18/2020
no-loc:
- Blazor
- SignalR
uid: blazor/advanced-scenarios
ms.openlocfilehash: 5edbbe36e8389bac0335594b1e4331aee1c02867
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78647413"
---
# <a name="aspnet-core-blazor-advanced-scenarios"></a><span data-ttu-id="08b24-103">Erweiterte ASP.NET Core Blazor-Szenarios</span><span class="sxs-lookup"><span data-stu-id="08b24-103">ASP.NET Core Blazor advanced scenarios</span></span>

<span data-ttu-id="08b24-104">Von [Luke Latham](https://github.com/guardrex) und [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="08b24-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="blazor-server-circuit-handler"></a><span data-ttu-id="08b24-105">Blazor Server-Verbindungshandler</span><span class="sxs-lookup"><span data-stu-id="08b24-105">Blazor Server circuit handler</span></span>

<span data-ttu-id="08b24-106">Blazor Server ermöglicht Code das Definieren eines *Verbindungshandlers*, mit dem Code für Änderungen am Zustand einer Benutzerverbindung ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="08b24-106">Blazor Server allows code to define a *circuit handler*, which allows running code on changes to the state of a user's circuit.</span></span> <span data-ttu-id="08b24-107">Ein Verbindungshandler wird durch das Ableiten von `CircuitHandler` und Registrieren der Klasse im Dienstcontainer der App implementiert.</span><span class="sxs-lookup"><span data-stu-id="08b24-107">A circuit handler is implemented by deriving from `CircuitHandler` and registering the class in the app's service container.</span></span> <span data-ttu-id="08b24-108">Im folgenden Beispiel eines Verbindungshandlers werden geöffnete SignalR-Verbindungen nachverfolgt:</span><span class="sxs-lookup"><span data-stu-id="08b24-108">The following example of a circuit handler tracks open SignalR connections:</span></span>

```csharp
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Server.Circuits;

public class TrackingCircuitHandler : CircuitHandler
{
    private HashSet<Circuit> _circuits = new HashSet<Circuit>();

    public override Task OnConnectionUpAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Add(circuit);

        return Task.CompletedTask;
    }

    public override Task OnConnectionDownAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Remove(circuit);

        return Task.CompletedTask;
    }

    public int ConnectedCircuits => _circuits.Count;
}
```

<span data-ttu-id="08b24-109">Verbindungshandler werden mithilfe von DI registriert.</span><span class="sxs-lookup"><span data-stu-id="08b24-109">Circuit handlers are registered using DI.</span></span> <span data-ttu-id="08b24-110">Bereichsbezogene Instanzen werden pro Verbindungsinstanz erstellt.</span><span class="sxs-lookup"><span data-stu-id="08b24-110">Scoped instances are created per instance of a circuit.</span></span> <span data-ttu-id="08b24-111">Mithilfe von `TrackingCircuitHandler` aus dem vorherigen Beispiel wird ein Singletondienst erstellt, da der Zustand aller Verbindungen nachverfolgt werden muss:</span><span class="sxs-lookup"><span data-stu-id="08b24-111">Using the `TrackingCircuitHandler` in the preceding example, a singleton service is created because the state of all circuits must be tracked:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...
    services.AddSingleton<CircuitHandler, TrackingCircuitHandler>();
}
```

<span data-ttu-id="08b24-112">Wenn die Methoden eines benutzerdefinierten Verbindungshandlers einen Ausnahmefehler auslöst, ist dieser Ausnahmefehler für die Blazor Server-Verbindung schwerwiegend.</span><span class="sxs-lookup"><span data-stu-id="08b24-112">If a custom circuit handler's methods throw an unhandled exception, the exception is fatal to the Blazor Server circuit.</span></span> <span data-ttu-id="08b24-113">Umschließen Sie den Code in einer oder mehreren [try-catch](/dotnet/csharp/language-reference/keywords/try-catch)-Anweisungen mit Fehlerbehandlung und Protokollierung, um Ausnahmen im Code oder in aufgerufenen Methoden eines Handlers zu tolerieren.</span><span class="sxs-lookup"><span data-stu-id="08b24-113">To tolerate exceptions in a handler's code or called methods, wrap the code in one or more [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span>

<span data-ttu-id="08b24-114">Wenn eine Verbindung beendet wird, weil ein Benutzer die Verbindung getrennt hat und das Framework den Verbindungsstatus bereinigt, gibt das Framework den DI-Bereich der Verbindung frei.</span><span class="sxs-lookup"><span data-stu-id="08b24-114">When a circuit ends because a user has disconnected and the framework is cleaning up the circuit state, the framework disposes of the circuit's DI scope.</span></span> <span data-ttu-id="08b24-115">Wenn der Bereich freigegeben wird, werden alle Dienste im Verbindungsbereich verworfen, die <xref:System.IDisposable?displayProperty=fullName> implementieren.</span><span class="sxs-lookup"><span data-stu-id="08b24-115">Disposing the scope disposes any circuit-scoped DI services that implement <xref:System.IDisposable?displayProperty=fullName>.</span></span> <span data-ttu-id="08b24-116">Wenn ein DI-Dienst während der Freigabe einen Ausnahmefehler auslöst, protokolliert das Framework die Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="08b24-116">If any DI service throws an unhandled exception during disposal, the framework logs the exception.</span></span>

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="08b24-117">Manuelle RenderTreeBuilder-Logik</span><span class="sxs-lookup"><span data-stu-id="08b24-117">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="08b24-118">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` stellt Methoden zum Bearbeiten von Komponenten und Elementen bereit. Dazu gehört auch das manuelle Erstellen von Komponenten in C#-Code.</span><span class="sxs-lookup"><span data-stu-id="08b24-118">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="08b24-119">Die Verwendung von `RenderTreeBuilder` zum Erstellen von Komponenten ist ein erweitertes Szenario.</span><span class="sxs-lookup"><span data-stu-id="08b24-119">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="08b24-120">Eine falsch formatierte Komponente (z. B. ein nicht geschlossenes Markuptag) kann zu undefiniertem Verhalten führen.</span><span class="sxs-lookup"><span data-stu-id="08b24-120">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="08b24-121">Beachten Sie die folgende `PetDetails`-Komponente, die manuell in eine andere Komponente integriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="08b24-121">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="08b24-122">Im folgenden Beispiel generiert die Schleife in der `CreateComponent`-Methode drei `PetDetails`-Komponenten.</span><span class="sxs-lookup"><span data-stu-id="08b24-122">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="08b24-123">Wenn Sie `RenderTreeBuilder`-Methoden aufrufen, um die Komponenten (`OpenComponent` und `AddAttribute`) zu erstellen, sind die Sequenznummern Zeilennummern des Quellcodes.</span><span class="sxs-lookup"><span data-stu-id="08b24-123">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="08b24-124">Der diff-Algorithmus von Blazor basiert auf den Sequenznummern, die unterschiedlichen Codezeilen und nicht unterschiedlichen Aufrufen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="08b24-124">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="08b24-125">Hartcodieren Sie die Argumente für Sequenznummern, wenn Sie eine Komponente mit `RenderTreeBuilder`-Methoden erstellen.</span><span class="sxs-lookup"><span data-stu-id="08b24-125">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="08b24-126">**Die Verwendung einer Berechnung oder eines Leistungszählers zum Generieren der Sequenznummer kann zu schlechter Leistung führen.**</span><span class="sxs-lookup"><span data-stu-id="08b24-126">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="08b24-127">Weitere Informationen finden Sie im Abschnitt [Auf Codezeilennummern und nicht auf die Ausführungsreihenfolge bezogene Sequenznummern](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order).</span><span class="sxs-lookup"><span data-stu-id="08b24-127">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="08b24-128">`BuiltContent`-Komponente:</span><span class="sxs-lookup"><span data-stu-id="08b24-128">`BuiltContent` component:</span></span>

```razor
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> [!WARNING]
> <span data-ttu-id="08b24-129">Die Typen in `Microsoft.AspNetCore.Components.RenderTree` ermöglichen die Verarbeitung der *Ergebnisse* von Renderingvorgängen.</span><span class="sxs-lookup"><span data-stu-id="08b24-129">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="08b24-130">Hierbei handelt es sich um interne Details der Blazor-Frameworkimplementierung.</span><span class="sxs-lookup"><span data-stu-id="08b24-130">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="08b24-131">Diese Typen sollten als *instabil* betrachtet werden und in zukünftigen Versionen geändert werden.</span><span class="sxs-lookup"><span data-stu-id="08b24-131">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="08b24-132">Auf Codezeilennummern und nicht auf die Ausführungsreihenfolge bezogene Sequenznummern</span><span class="sxs-lookup"><span data-stu-id="08b24-132">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="08b24-133">Razor-Komponentendateien ( *.razor*) werden immer kompiliert.</span><span class="sxs-lookup"><span data-stu-id="08b24-133">Razor component files (*.razor*) are always compiled.</span></span> <span data-ttu-id="08b24-134">Die Kompilierung stellt gegenüber der Interpretation von Code einen Vorteil dar, da der Kompilierungsschritt zum Einfügen von Informationen verwendet werden kann, die die App-Leistung zur Laufzeit erhöhen können.</span><span class="sxs-lookup"><span data-stu-id="08b24-134">Compilation is a potential advantage over interpreting code because the compile step can be used to inject information that improves app performance at runtime.</span></span>

<span data-ttu-id="08b24-135">Ein wichtiges Beispiel für diese Verbesserungen sind *Sequenznummern*.</span><span class="sxs-lookup"><span data-stu-id="08b24-135">A key example of these improvements involves *sequence numbers*.</span></span> <span data-ttu-id="08b24-136">Sequenznummern geben der Laufzeit an, welche Ausgaben aus den unterschiedlichen und geordneten Codezeilen stammen.</span><span class="sxs-lookup"><span data-stu-id="08b24-136">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="08b24-137">Diese Informationen werden von der Laufzeit verwendet, um effiziente und zeitlich lineare Strukturunterschiede zu generieren. Diese Methode ist viel schneller als es für einen allgemeinen diff-Strukturalgorithmus normalerweise möglich ist.</span><span class="sxs-lookup"><span data-stu-id="08b24-137">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="08b24-138">Sehen Sie sich die folgende Razor-Komponentendatei ( *.razor*) an:</span><span class="sxs-lookup"><span data-stu-id="08b24-138">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="08b24-139">Der vorangehende Code wird in etwa wie folgt kompiliert:</span><span class="sxs-lookup"><span data-stu-id="08b24-139">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="08b24-140">Wenn der Code zum ersten Mal ausgeführt wird, erhält der Generator Folgendes, wenn `someFlag` `true` ist:</span><span class="sxs-lookup"><span data-stu-id="08b24-140">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="08b24-141">Sequenz</span><span class="sxs-lookup"><span data-stu-id="08b24-141">Sequence</span></span> | <span data-ttu-id="08b24-142">Typ</span><span class="sxs-lookup"><span data-stu-id="08b24-142">Type</span></span>      | <span data-ttu-id="08b24-143">Daten</span><span class="sxs-lookup"><span data-stu-id="08b24-143">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="08b24-144">0</span><span class="sxs-lookup"><span data-stu-id="08b24-144">0</span></span>        | <span data-ttu-id="08b24-145">Textknoten</span><span class="sxs-lookup"><span data-stu-id="08b24-145">Text node</span></span> | <span data-ttu-id="08b24-146">First</span><span class="sxs-lookup"><span data-stu-id="08b24-146">First</span></span>  |
| <span data-ttu-id="08b24-147">1</span><span class="sxs-lookup"><span data-stu-id="08b24-147">1</span></span>        | <span data-ttu-id="08b24-148">Textknoten</span><span class="sxs-lookup"><span data-stu-id="08b24-148">Text node</span></span> | <span data-ttu-id="08b24-149">Second</span><span class="sxs-lookup"><span data-stu-id="08b24-149">Second</span></span> |

<span data-ttu-id="08b24-150">Stellen Sie sich vor, dass `someFlag` `false` wird und das Markup wieder gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="08b24-150">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="08b24-151">Dieses Mal empfängt der Generator Folgendes:</span><span class="sxs-lookup"><span data-stu-id="08b24-151">This time, the builder receives:</span></span>

| <span data-ttu-id="08b24-152">Sequenz</span><span class="sxs-lookup"><span data-stu-id="08b24-152">Sequence</span></span> | <span data-ttu-id="08b24-153">Typ</span><span class="sxs-lookup"><span data-stu-id="08b24-153">Type</span></span>       | <span data-ttu-id="08b24-154">Daten</span><span class="sxs-lookup"><span data-stu-id="08b24-154">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="08b24-155">1</span><span class="sxs-lookup"><span data-stu-id="08b24-155">1</span></span>        | <span data-ttu-id="08b24-156">Textknoten</span><span class="sxs-lookup"><span data-stu-id="08b24-156">Text node</span></span>  | <span data-ttu-id="08b24-157">Second</span><span class="sxs-lookup"><span data-stu-id="08b24-157">Second</span></span> |

<span data-ttu-id="08b24-158">Wenn die Laufzeit einen diff-Algorithmus ausführt, wird angezeigt, dass das Element bei Sequenz `0` entfernt wurde. Daraufhin wird das folgende triviale *Bearbeitungsskript* generiert:</span><span class="sxs-lookup"><span data-stu-id="08b24-158">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="08b24-159">Entfernen Sie den ersten Textknoten.</span><span class="sxs-lookup"><span data-stu-id="08b24-159">Remove the first text node.</span></span>

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a><span data-ttu-id="08b24-160">Problem beim programmgesteuerten Generieren von Sequenznummern</span><span class="sxs-lookup"><span data-stu-id="08b24-160">The problem with generating sequence numbers programmatically</span></span>

<span data-ttu-id="08b24-161">Angenommen, Sie haben stattdessen die folgende Buildlogik für die Renderstruktur geschrieben:</span><span class="sxs-lookup"><span data-stu-id="08b24-161">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="08b24-162">Die erste Ausgabe lautet nun wie folgt:</span><span class="sxs-lookup"><span data-stu-id="08b24-162">Now, the first output is:</span></span>

| <span data-ttu-id="08b24-163">Sequenz</span><span class="sxs-lookup"><span data-stu-id="08b24-163">Sequence</span></span> | <span data-ttu-id="08b24-164">Typ</span><span class="sxs-lookup"><span data-stu-id="08b24-164">Type</span></span>      | <span data-ttu-id="08b24-165">Daten</span><span class="sxs-lookup"><span data-stu-id="08b24-165">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="08b24-166">0</span><span class="sxs-lookup"><span data-stu-id="08b24-166">0</span></span>        | <span data-ttu-id="08b24-167">Textknoten</span><span class="sxs-lookup"><span data-stu-id="08b24-167">Text node</span></span> | <span data-ttu-id="08b24-168">First</span><span class="sxs-lookup"><span data-stu-id="08b24-168">First</span></span>  |
| <span data-ttu-id="08b24-169">1</span><span class="sxs-lookup"><span data-stu-id="08b24-169">1</span></span>        | <span data-ttu-id="08b24-170">Textknoten</span><span class="sxs-lookup"><span data-stu-id="08b24-170">Text node</span></span> | <span data-ttu-id="08b24-171">Second</span><span class="sxs-lookup"><span data-stu-id="08b24-171">Second</span></span> |

<span data-ttu-id="08b24-172">Dieses Ergebnis ist mit dem des vorherigen Falls identisch, sodass keine negativen Probleme aufgetreten sind.</span><span class="sxs-lookup"><span data-stu-id="08b24-172">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="08b24-173">`someFlag` ist beim zweiten Rendering `false`, und die Ausgabe lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="08b24-173">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="08b24-174">Sequenz</span><span class="sxs-lookup"><span data-stu-id="08b24-174">Sequence</span></span> | <span data-ttu-id="08b24-175">Typ</span><span class="sxs-lookup"><span data-stu-id="08b24-175">Type</span></span>      | <span data-ttu-id="08b24-176">Daten</span><span class="sxs-lookup"><span data-stu-id="08b24-176">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="08b24-177">0</span><span class="sxs-lookup"><span data-stu-id="08b24-177">0</span></span>        | <span data-ttu-id="08b24-178">Textknoten</span><span class="sxs-lookup"><span data-stu-id="08b24-178">Text node</span></span> | <span data-ttu-id="08b24-179">Second</span><span class="sxs-lookup"><span data-stu-id="08b24-179">Second</span></span> |

<span data-ttu-id="08b24-180">Dieses Mal zeigt der diff-Algorithmus an, dass *zwei* Änderungen aufgetreten sind, und der Algorithmus generiert das folgende Bearbeitungsskript:</span><span class="sxs-lookup"><span data-stu-id="08b24-180">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="08b24-181">Ändern Sie den Wert des ersten Textknotens in `Second`.</span><span class="sxs-lookup"><span data-stu-id="08b24-181">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="08b24-182">Entfernen Sie den zweiten Textknoten.</span><span class="sxs-lookup"><span data-stu-id="08b24-182">Remove the second text node.</span></span>

<span data-ttu-id="08b24-183">Durch das Erzeugen der Sequenznummern sind alle nützlichen Informationen dazu verloren gegangen, wo die `if/else`-Branches und -Schleifen im ursprünglichen Code vorhanden waren.</span><span class="sxs-lookup"><span data-stu-id="08b24-183">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="08b24-184">Dies führt zu einem **doppelt so langen diff** wie zuvor.</span><span class="sxs-lookup"><span data-stu-id="08b24-184">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="08b24-185">Dies ist ein triviales Beispiel.</span><span class="sxs-lookup"><span data-stu-id="08b24-185">This is a trivial example.</span></span> <span data-ttu-id="08b24-186">In realistischeren Fällen mit komplexen und tief geschachtelten Strukturen und vor allem mit Schleifen sind die Leistungskosten in der Regel höher.</span><span class="sxs-lookup"><span data-stu-id="08b24-186">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is usually higher.</span></span> <span data-ttu-id="08b24-187">Anstatt sofort festzustellen, welche Schleifenblöcke oder Branches eingefügt oder entfernt wurden, muss der diff-Algorithmus eine tiefe Rekursion der Renderstrukturen durchführen.</span><span class="sxs-lookup"><span data-stu-id="08b24-187">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees.</span></span> <span data-ttu-id="08b24-188">Dies führt in der Regel dazu, dass längere Bearbeitungsskripts erstellt werden müssen, da der diff-Algorithmus falsch informiert ist, wie sich die alten und neuen Strukturen aufeinander beziehen.</span><span class="sxs-lookup"><span data-stu-id="08b24-188">This usually results in having to build longer edit scripts because the diff algorithm is misinformed about how the old and new structures relate to each other.</span></span>

### <a name="guidance-and-conclusions"></a><span data-ttu-id="08b24-189">Anleitungen und Schlussfolgerungen</span><span class="sxs-lookup"><span data-stu-id="08b24-189">Guidance and conclusions</span></span>

* <span data-ttu-id="08b24-190">Die App-Leistung leidet, wenn Sequenznummern dynamisch generiert werden.</span><span class="sxs-lookup"><span data-stu-id="08b24-190">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="08b24-191">Das Framework kann zur Laufzeit automatisch keine eigenen Sequenznummern erstellen, da die erforderlichen Informationen nicht vorhanden sind, es sei denn, sie werden zur Kompilierzeit aufgezeichnet.</span><span class="sxs-lookup"><span data-stu-id="08b24-191">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="08b24-192">Schreiben Sie keine langen Blöcke manuell implementierter `RenderTreeBuilder`-Logik.</span><span class="sxs-lookup"><span data-stu-id="08b24-192">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="08b24-193">Bevorzugen Sie *.razor*-Dateien, und ermöglichen Sie es dem Compiler, die Sequenznummern zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="08b24-193">Prefer *.razor* files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="08b24-194">Wenn Sie manuelle `RenderTreeBuilder`-Logik nicht vermeiden können, teilen Sie lange Codeblöcke in kleinere Teile auf, die in `OpenRegion`/`CloseRegion`-Aufrufe eingebunden werden.</span><span class="sxs-lookup"><span data-stu-id="08b24-194">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="08b24-195">Jede Region verfügt über einen eigenen separaten Bereich von Sequenznummern, sodass Sie in jeder Region von 0 (Null; oder von jeder anderen beliebigen Zahl) aus starten können.</span><span class="sxs-lookup"><span data-stu-id="08b24-195">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="08b24-196">Wenn Sequenznummern hartcodiert sind, erfordert der diff-Algorithmus nur, dass die Sequenznummern den Wert erhöhen.</span><span class="sxs-lookup"><span data-stu-id="08b24-196">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="08b24-197">Die Anfangswerte und Lücken sind irrelevant.</span><span class="sxs-lookup"><span data-stu-id="08b24-197">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="08b24-198">Eine legitime Option besteht darin, die Codezeilennummer als Sequenznummer zu verwenden oder von 0 (Null) aus zu starten und Werte in Einer- oder Hunderterschritten (oder einem beliebigen bevorzugten Intervall) zu erhöhen.</span><span class="sxs-lookup"><span data-stu-id="08b24-198">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="08b24-199"> verwendet Sequenznummern, während andere Benutzeroberflächenframeworks diese nicht für Verzeichnisvergleiche verwenden.</span><span class="sxs-lookup"><span data-stu-id="08b24-199"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="08b24-200">Das Vergleichen ist weitaus schneller, wenn Sequenznummern verwendet werden. Außerdem hat Blazor den Vorteil eines Kompilierungsschritts, der Sequenznummern automatisch für Entwickler verarbeitet, die *.razor*-Dateien erstellen.</span><span class="sxs-lookup"><span data-stu-id="08b24-200">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a><span data-ttu-id="08b24-201">Ausführen umfangreicher Datenübertragungen in Blazor Server-Apps</span><span class="sxs-lookup"><span data-stu-id="08b24-201">Perform large data transfers in Blazor Server apps</span></span>

<span data-ttu-id="08b24-202">In einigen Szenarios müssen große Datenmengen zwischen JavaScript und Blazorübertragen werden.</span><span class="sxs-lookup"><span data-stu-id="08b24-202">In some scenarios, large amounts of data must be transferred between JavaScript and Blazor.</span></span> <span data-ttu-id="08b24-203">In der Regel erfolgen große Datenübertragungen in den folgenden Fällen:</span><span class="sxs-lookup"><span data-stu-id="08b24-203">Typically, large data transfers occur when:</span></span>

* <span data-ttu-id="08b24-204">Dateisystem-APIs des Browsers werden für das Hochladen oder Herunterladen einer Datei verwendet.</span><span class="sxs-lookup"><span data-stu-id="08b24-204">Browser file system APIs are used to upload or download a file.</span></span>
* <span data-ttu-id="08b24-205">Es ist eine Interaktion mit einer Drittanbieterbibliothek erforderlich.</span><span class="sxs-lookup"><span data-stu-id="08b24-205">Interop with a third party library is required.</span></span>

<span data-ttu-id="08b24-206">In Blazor Server gibt es eine Einschränkung, um zu verhindern, dass einzelne große Nachrichten übergeben werden, die zu Leistungsproblemen führen können.</span><span class="sxs-lookup"><span data-stu-id="08b24-206">In Blazor Server, a limitation is in place to prevent passing single large messages that may result in performance issues.</span></span>

<span data-ttu-id="08b24-207">Beachten Sie die folgenden Anleitungen, wenn Sie Code zum Übertragen von Daten zwischen JavaScript und Blazor entwickeln:</span><span class="sxs-lookup"><span data-stu-id="08b24-207">Consider the following guidance when developing code that transfers data between JavaScript and Blazor:</span></span>

* <span data-ttu-id="08b24-208">Segmentieren Sie die Daten in kleinere Teile, und senden Sie die Datensegmente sequenziell, bis alle Daten vom Server empfangen wurden.</span><span class="sxs-lookup"><span data-stu-id="08b24-208">Slice the data into smaller pieces, and send the data segments sequentially until all of the data is received by the server.</span></span>
* <span data-ttu-id="08b24-209">Ordnen Sie in JavaScript- und C#-Code keine großen Objekte zu.</span><span class="sxs-lookup"><span data-stu-id="08b24-209">Don't allocate large objects in JavaScript and C# code.</span></span>
* <span data-ttu-id="08b24-210">Blockieren Sie den hauptsächlichen Benutzeroberflächenthread nicht für lange Zeiträume, wenn Sie Daten senden oder empfangen.</span><span class="sxs-lookup"><span data-stu-id="08b24-210">Don't block the main UI thread for long periods when sending or receiving data.</span></span>
* <span data-ttu-id="08b24-211">Geben Sie belegten Arbeitsspeicher frei, wenn der Prozess abgeschlossen oder abgebrochen wird.</span><span class="sxs-lookup"><span data-stu-id="08b24-211">Free any memory consumed when the process is completed or cancelled.</span></span>
* <span data-ttu-id="08b24-212">Erzwingen Sie die folgenden zusätzlichen Anforderungen aus Sicherheitsgründen:</span><span class="sxs-lookup"><span data-stu-id="08b24-212">Enforce the following additional requirements for security purposes:</span></span>
  * <span data-ttu-id="08b24-213">Deklarieren Sie die maximale Datei- oder Datengröße, die übermittelt werden kann.</span><span class="sxs-lookup"><span data-stu-id="08b24-213">Declare the maximum file or data size that can be passed.</span></span>
  * <span data-ttu-id="08b24-214">Deklarieren Sie die minimale Uploadrate vom Client an den Server.</span><span class="sxs-lookup"><span data-stu-id="08b24-214">Declare the minimum upload rate from the client to the server.</span></span>
* <span data-ttu-id="08b24-215">Nachdem die Daten vom Server empfangen wurden, ist mit den Daten Folgendes möglich:</span><span class="sxs-lookup"><span data-stu-id="08b24-215">After the data is received by the server, the data can be:</span></span>
  * <span data-ttu-id="08b24-216">Sie können temporär in einem Speicherpuffer gespeichert werden, bis alle Segmente gesammelt wurden.</span><span class="sxs-lookup"><span data-stu-id="08b24-216">Temporarily stored in a memory buffer until all of the segments are collected.</span></span>
  * <span data-ttu-id="08b24-217">Sie können sofort verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="08b24-217">Consumed immediately.</span></span> <span data-ttu-id="08b24-218">Beispielsweise können die Daten sofort in einer Datenbank gespeichert oder auf den Datenträger geschrieben werden, wenn die einzelnen Segmente empfangen werden.</span><span class="sxs-lookup"><span data-stu-id="08b24-218">For example, the data can be stored immediately in a database or written to disk as each segment is received.</span></span>

<span data-ttu-id="08b24-219">Die folgende Dateiuploaderklasse verarbeitet JS Interop mit dem Client.</span><span class="sxs-lookup"><span data-stu-id="08b24-219">The following file uploader class handles JS interop with the client.</span></span> <span data-ttu-id="08b24-220">Die Uploaderklasse verwendet JS Interop für Folgendes:</span><span class="sxs-lookup"><span data-stu-id="08b24-220">The uploader class uses JS interop to:</span></span>

* <span data-ttu-id="08b24-221">Abrufen des Clients, um ein Datensegment zu senden</span><span class="sxs-lookup"><span data-stu-id="08b24-221">Poll the client to send a data segment.</span></span>
* <span data-ttu-id="08b24-222">Abbrechen der Transaktion, wenn ein Timeout für den Abruf auftritt</span><span class="sxs-lookup"><span data-stu-id="08b24-222">Abort the transaction if polling times out.</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.JSInterop;

public class FileUploader : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private readonly int _segmentSize = 6144;
    private readonly int _maxBase64SegmentSize = 8192;
    private readonly DotNetObjectReference<FileUploader> _thisReference;
    private List<IMemoryOwner<byte>> _uploadedSegments = 
        new List<IMemoryOwner<byte>>();

    public FileUploader(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public async Task<Stream> ReceiveFile(string selector, int maxSize)
    {
        var fileSize = 
            await _jsRuntime.InvokeAsync<int>("getFileSize", selector);

        if (fileSize > maxSize)
        {
            return null;
        }

        var numberOfSegments = Math.Floor(fileSize / (double)_segmentSize) + 1;
        var lastSegmentBytes = 0;
        string base64EncodedSegment;

        for (var i = 0; i < numberOfSegments; i++)
        {
            try
            {
                base64EncodedSegment = 
                    await _jsRuntime.InvokeAsync<string>(
                        "receiveSegment", i, selector);

                if (base64EncodedSegment.Length < _maxBase64SegmentSize && 
                    i < numberOfSegments - 1)
                {
                    return null;
                }
            }
            catch
            {
                return null;
            }

          var current = MemoryPool<byte>.Shared.Rent(_segmentSize);

          if (!Convert.TryFromBase64String(base64EncodedSegment, 
              current.Memory.Slice(0, _segmentSize).Span, out lastSegmentBytes))
          {
              return null;
          }

          _uploadedSegments.Add(current);
        }

        var segments = _uploadedSegments;
        _uploadedSegments = null;

        return new SegmentedStream(segments, _segmentSize, lastSegmentBytes);
    }

    public void Dispose()
    {
        if (_uploadedSegments != null)
        {
            foreach (var segment in _uploadedSegments)
            {
                segment.Dispose();
            }
        }
    }
}
```

<span data-ttu-id="08b24-223">Im vorherigen Beispiel:</span><span class="sxs-lookup"><span data-stu-id="08b24-223">In the preceding example:</span></span>

* <span data-ttu-id="08b24-224">`_maxBase64SegmentSize` wird auf `8192` festgelegt. Dies wird folgendermaßen berechnet: `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span><span class="sxs-lookup"><span data-stu-id="08b24-224">The `_maxBase64SegmentSize` is set to `8192`, which is calculated from `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span></span>
* <span data-ttu-id="08b24-225">Die .NET Core-APIs für die Speicherverwaltung auf niedriger Ebene werden zum Speichern der Speichersegmente auf dem Server in `_uploadedSegments` verwendet.</span><span class="sxs-lookup"><span data-stu-id="08b24-225">Low-level .NET Core memory management APIs are used to store the memory segments on the server in `_uploadedSegments`.</span></span>
* <span data-ttu-id="08b24-226">Eine `ReceiveFile`-Methode wird verwendet, um den Upload über JS Interop zu verarbeiten:</span><span class="sxs-lookup"><span data-stu-id="08b24-226">A `ReceiveFile` method is used to handle the upload through JS interop:</span></span>
  * <span data-ttu-id="08b24-227">Die Dateigröße wird in Bytes über JS Interop mit `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)` angegeben.</span><span class="sxs-lookup"><span data-stu-id="08b24-227">The file size is determined in bytes through JS interop with `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span></span>
  * <span data-ttu-id="08b24-228">Die Anzahl der zu empfangenden Segmente wird berechnet und in `numberOfSegments`gespeichert.</span><span class="sxs-lookup"><span data-stu-id="08b24-228">The number of segments to receive are calculated and stored in `numberOfSegments`.</span></span>
  * <span data-ttu-id="08b24-229">Die Segmente werden in einer `for`-Schleife durch JS Interop mit `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)` angefordert.</span><span class="sxs-lookup"><span data-stu-id="08b24-229">The segments are requested in a `for` loop through JS interop with `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span></span> <span data-ttu-id="08b24-230">Alle Segmente bis auf das letzte müssen vor dem Decodieren eine Größe von 8.192 Bytes haben.</span><span class="sxs-lookup"><span data-stu-id="08b24-230">All segments but the last must be 8,192 bytes before decoding.</span></span> <span data-ttu-id="08b24-231">Der Client wird gezwungen, die Daten auf effiziente Weise zu senden.</span><span class="sxs-lookup"><span data-stu-id="08b24-231">The client is forced to send the data in an efficient manner.</span></span>
  * <span data-ttu-id="08b24-232">Für jedes empfangene Segment werden vor der Decodierung mit <xref:System.Convert.TryFromBase64String*> Überprüfungen ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="08b24-232">For each segment received, checks are performed before decoding with <xref:System.Convert.TryFromBase64String*>.</span></span>
  * <span data-ttu-id="08b24-233">Ein Datenstrom mit den Daten wird nach Abschluss des Uploads als neuer <xref:System.IO.Stream> (`SegmentedStream`) zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="08b24-233">A stream with the data is returned as a new <xref:System.IO.Stream> (`SegmentedStream`) after the upload is complete.</span></span>

<span data-ttu-id="08b24-234">Die segmentierte Streamklasse macht die Liste der Segmente als schreibgeschützten, nicht suchbaren <xref:System.IO.Stream> verfügbar:</span><span class="sxs-lookup"><span data-stu-id="08b24-234">The segmented stream class exposes the list of segments as a readonly non-seekable <xref:System.IO.Stream>:</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;

public class SegmentedStream : Stream
{
    private readonly ReadOnlySequence<byte> _sequence;
    private long _currentPosition = 0;

    public SegmentedStream(IList<IMemoryOwner<byte>> segments, int segmentSize, 
        int lastSegmentSize)
    {
        if (segments.Count == 1)
        {
            _sequence = new ReadOnlySequence<byte>(
                segments[0].Memory.Slice(0, lastSegmentSize));
            return;
        }

        var sequenceSegment = new BufferSegment<byte>(
            segments[0].Memory.Slice(0, segmentSize));
        var lastSegment = sequenceSegment;

        for (int i = 1; i < segments.Count; i++)
        {
            var isLastSegment = i + 1 == segments.Count;
            lastSegment = lastSegment.Append(segments[i].Memory.Slice(
                0, isLastSegment ? lastSegmentSize : segmentSize));
        }

        _sequence = new ReadOnlySequence<byte>(
            sequenceSegment, 0, lastSegment, lastSegmentSize);
    }

    public override long Position
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public override int Read(byte[] buffer, int offset, int count)
    {
        var bytesToWrite = (int)(_currentPosition + count < _sequence.Length ? 
            count : _sequence.Length - _currentPosition);
        var data = _sequence.Slice(_currentPosition, bytesToWrite);
        data.CopyTo(buffer.AsSpan(offset, bytesToWrite));
        _currentPosition += bytesToWrite;

        return bytesToWrite;
    }

    private class BufferSegment<T> : ReadOnlySequenceSegment<T>
    {
        public BufferSegment(ReadOnlyMemory<T> memory)
        {
            Memory = memory;
        }

        public BufferSegment<T> Append(ReadOnlyMemory<T> memory)
        {
            var segment = new BufferSegment<T>(memory)
            {
                RunningIndex = RunningIndex + Memory.Length
            };

            Next = segment;

            return segment;
        }
    }

    public override bool CanRead => true;

    public override bool CanSeek => false;

    public override bool CanWrite => false;

    public override long Length => throw new NotImplementedException();

    public override void Flush() => throw new NotImplementedException();

    public override long Seek(long offset, SeekOrigin origin) => 
        throw new NotImplementedException();

    public override void SetLength(long value) => 
        throw new NotImplementedException();

    public override void Write(byte[] buffer, int offset, int count) => 
        throw new NotImplementedException();
}
```

<span data-ttu-id="08b24-235">Der folgende Code implementiert JavaScript-Funktionen, um die Daten zu empfangen:</span><span class="sxs-lookup"><span data-stu-id="08b24-235">The following code implements JavaScript functions to receive the data:</span></span>

```javascript
function getFileSize(selector) {
  const file = getFile(selector);
  return file.size;
}

async function receiveSegment(segmentNumber, selector) {
  const file = getFile(selector);
  var segments = getFileSegments(file);
  var index = segmentNumber * 6144;
  return await getNextChunk(file, index);
}

function getFile(selector) {
  const element = document.querySelector(selector);
  if (!element) {
    throw new Error('Invalid selector');
  }
  const files = element.files;
  if (!files || files.length === 0) {
    throw new Error(`Element ${elementId} doesn't contain any files.`);
  }
  const file = files[0];
  return file;
}

function getFileSegments(file) {
  const segments = Math.floor(size % 6144 === 0 ? size / 6144 : 1 + size / 6144);
  return segments;
}

async function getNextChunk(file, index) {
  const length = file.size - index <= 6144 ? file.size - index : 6144;
  const chunk = file.slice(index, index + length);
  index += length;
  const base64Chunk = await this.base64EncodeAsync(chunk);
  return { base64Chunk, index };
}

async function base64EncodeAsync(chunk) {
  const reader = new FileReader();
  const result = new Promise((resolve, reject) => {
    reader.addEventListener('load',
      () => {
        const base64Chunk = reader.result;
        const cleanChunk = 
          base64Chunk.replace('data:application/octet-stream;base64,', '');
        resolve(cleanChunk);
      },
      false);
    reader.addEventListener('error', reject);
  });
  reader.readAsDataURL(chunk);
  return result;
}
```