<span data-ttu-id="7907b-101">Führen Sie die folgenden .NET Core-CLI-Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="7907b-101">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="7907b-102">Die oben aufgeführten Befehle fügen Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="7907b-102">The preceding commands add:</span></span>

* <span data-ttu-id="7907b-103">Das [Gerüstbautool „aspnet-codegenerator“](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span><span class="sxs-lookup"><span data-stu-id="7907b-103">The [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>
* <span data-ttu-id="7907b-104">Die Entity Framework Core-Tools für die .NET Core-CLI.</span><span class="sxs-lookup"><span data-stu-id="7907b-104">The Entity Framework Core Tools for the .NET Core CLI.</span></span>
* <span data-ttu-id="7907b-105">Den EF Core SQLite-Anbieter, der das EF Core-Paket als Abhängigkeit installiert.</span><span class="sxs-lookup"><span data-stu-id="7907b-105">The EF Core SQLite provider, which installs the EF Core package as a dependency.</span></span>
* <span data-ttu-id="7907b-106">Für Gerüstbau erforderliche Pakete: `Microsoft.VisualStudio.Web.CodeGeneration.Design` und `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="7907b-106">Packages needed for scaffolding: `Microsoft.VisualStudio.Web.CodeGeneration.Design` and `Microsoft.EntityFrameworkCore.SqlServer`.</span></span>