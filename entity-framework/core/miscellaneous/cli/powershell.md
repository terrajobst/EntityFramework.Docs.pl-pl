---
title: EF Core informacje dotyczące narzędzi (Konsola Menedżera pakietów) — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 468698d1bbd17d4ad10b1b1601bfbc315a01c1ff
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688709"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="4a8fc-102">Entity Framework Core odnoszą się narzędzia — Konsola Menedżera pakietów w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a8fc-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="4a8fc-103">Narzędzia konsoli Menedżera pakietów (PMC) dla platformy Entity Framework Core wykonywać zadania podczas projektowania.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="4a8fc-104">Na przykład można utworzyć [migracje](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), zastosuj migracje i wygenerować kod dla modelu, w oparciu o istniejącą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="4a8fc-105">Polecenia uruchamiane wewnątrz programu Visual Studio przy użyciu [Konsola Menedżera pakietów](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="4a8fc-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="4a8fc-106">Te narzędzia działają w projektach .NET Core i .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="4a8fc-107">Jeśli nie używasz programu Visual Studio, firma Microsoft zaleca [narzędzi wiersza polecenia programu EF Core](dotnet.md) zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="4a8fc-108">Narzędzia interfejsu wiersza polecenia są wieloplatformowe i wykonywania w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="4a8fc-109">Instalowanie narzędzi</span><span class="sxs-lookup"><span data-stu-id="4a8fc-109">Installing the tools</span></span>

<span data-ttu-id="4a8fc-110">Procedury instalowania i aktualizowania narzędzia różnią się między platformą ASP.NET Core 2.1 + i wcześniejszych wersji lub innych typów projektów.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="4a8fc-111">Platforma ASP.NET Core w wersji 2.1 i nowsze</span><span class="sxs-lookup"><span data-stu-id="4a8fc-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="4a8fc-112">Narzędzia są automatycznie uwzględniane w projektach programu ASP.NET Core 2.1 +, ponieważ `Microsoft.EntityFrameworkCore.Tools` pakietu znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4a8fc-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4a8fc-113">W związku z tym nie trzeba nic robić, aby zainstalować narzędzia, ale trzeba:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>
* <span data-ttu-id="4a8fc-114">Przywróć pakiety przed użyciem narzędzia w nowym projekcie.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="4a8fc-115">Zainstaluj pakiet, aby zaktualizować narzędzia do nowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="4a8fc-116">Aby upewnić się, że otrzymujesz najnowszej wersji narzędzia, firma Microsoft zaleca również wykonać poniższe czynności:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="4a8fc-117">Edytuj swoje *.csproj* pliku i Dodaj wiersz, określając najnowszą wersję [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="4a8fc-118">Na przykład *.csproj* plik może zawierać `ItemGroup` , wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="4a8fc-119">Aktualizacja narzędzi, gdy zostanie wyświetlony komunikat jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="4a8fc-120">Wersja narzędzi programu EF Core "2.1.1-rtm-30846" jest starsza niż w przypadku środowiska uruchomieniowego "2.1.3-rtm-32065".</span><span class="sxs-lookup"><span data-stu-id="4a8fc-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="4a8fc-121">Aktualizacja narzędzi do najnowszych funkcji i poprawek błędów.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="4a8fc-122">Aby zaktualizować narzędzia:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-122">To update the tools:</span></span>
* <span data-ttu-id="4a8fc-123">Zainstaluj najnowszy zestaw .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="4a8fc-124">Aktualizacja programu Visual Studio do najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="4a8fc-125">Edytuj *.csproj* plik zawiera odwołania do pakietu do najnowszego pakietu narzędzi, jak pokazano wcześniej.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="4a8fc-126">Inne wersje i typy projektów</span><span class="sxs-lookup"><span data-stu-id="4a8fc-126">Other versions and project types</span></span>

<span data-ttu-id="4a8fc-127">Instalowanie narzędzi Konsola Menedżera pakietów, uruchamiając następujące polecenie w **Konsola Menedżera pakietów**:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="4a8fc-128">Aktualizacja narzędzi, uruchamiając następujące polecenie w **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="4a8fc-129">Weryfikowanie instalacji</span><span class="sxs-lookup"><span data-stu-id="4a8fc-129">Verify the installation</span></span>

<span data-ttu-id="4a8fc-130">Sprawdź, czy narzędzia są zainstalowane, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="4a8fc-131">Dane wyjściowe wyglądają następująco (program nie nakazuje możesz wersję narzędzia używasz):</span><span class="sxs-lookup"><span data-stu-id="4a8fc-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a><span data-ttu-id="4a8fc-132">Przy użyciu narzędzi</span><span class="sxs-lookup"><span data-stu-id="4a8fc-132">Using the tools</span></span>

<span data-ttu-id="4a8fc-133">Przed rozpoczęciem korzystania z narzędzi:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-133">Before using the tools:</span></span>
* <span data-ttu-id="4a8fc-134">Należy zrozumieć różnicę między docelowej i uruchamianie projektu.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="4a8fc-135">Dowiedz się, jak używać narzędzi z biblioteki klas .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="4a8fc-136">Dla projektów ASP.NET Core należy ustawić środowisko.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="4a8fc-137">Element docelowy i uruchamianie projektu</span><span class="sxs-lookup"><span data-stu-id="4a8fc-137">Target and startup project</span></span>

<span data-ttu-id="4a8fc-138">Polecenia można znaleźć *projektu* i *projekt startowy*.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="4a8fc-139">*Projektu* jest także znana jako *projekt docelowy* ponieważ jest za której polecenia Dodawanie lub usuwanie plików.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="4a8fc-140">Domyślnie **projekt domyślny** wybranego w **Konsola Menedżera pakietów** jest projekt docelowy.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="4a8fc-141">Należy określić inny projekt jako projekt docelowy za pomocą <nobr> `--project` </nobr> opcji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="4a8fc-142">*Projekt startowy* jest tą, która narzędzia skompilować i uruchomić.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="4a8fc-143">Narzędzia konieczne wykonanie kodu aplikacji w czasie projektowania, aby uzyskać informacje na temat projektu, takie jak parametry połączenia bazy danych i Konfiguracja modelu.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="4a8fc-144">Domyślnie **projekt startowy** w **Eksploratora rozwiązań** jest projektem startowym.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="4a8fc-145">Należy określić inny projekt jako projekt startowy, za pomocą <nobr> `--startup-project` </nobr> opcji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="4a8fc-146">Projekt startowy i projekt docelowy często są tym samym projekcie.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="4a8fc-147">Jest to typowy scenariusz, w którym są one oddzielnych projektów, gdy:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="4a8fc-148">EF Core klasy kontekstu i jednostki są w bibliotece klas .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="4a8fc-149">Aplikacją sieci web lub aplikacji konsolowej .NET Core odwołuje się do biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="4a8fc-150">Istnieje również możliwość [umieść kod migracji w bibliotece klas, niezależnie od kontekstu programu EF Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="4a8fc-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="4a8fc-151">Innych platform docelowych</span><span class="sxs-lookup"><span data-stu-id="4a8fc-151">Other target frameworks</span></span>

<span data-ttu-id="4a8fc-152">Narzędzia Konsola Menedżera pakietów pracować z projektami .NET Core lub .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="4a8fc-153">Aplikacje, które mają modelu platformy EF Core w bibliotece klas programu .NET Standard może nie mieć platformy .NET Core lub .NET Framework projektu.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="4a8fc-154">Na przykład dotyczy to aplikacji platformy Xamarin i platformy uniwersalnej Windows.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="4a8fc-155">W takich przypadkach można utworzyć platformy .NET Core lub .NET Framework projekt aplikacji konsoli którego jedynym celem jest do działania jako projekt startowy dla narzędzi.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="4a8fc-156">Projekt może być fikcyjnego projektu bez rzeczywistego kodu &mdash; jest wymagana tylko do zapewnienia celu narzędzi.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="4a8fc-157">Dlaczego jest projektem fikcyjnego wymagane?</span><span class="sxs-lookup"><span data-stu-id="4a8fc-157">Why is a dummy project required?</span></span> <span data-ttu-id="4a8fc-158">Jak wspomniano wcześniej, narzędzia konieczne wykonanie kodu aplikacji w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="4a8fc-159">Aby to zrobić, muszą one korzystania ze środowiska uruchomieniowego .NET Core lub .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="4a8fc-160">Jeśli model programu EF Core jest w projekcie, który jest przeznaczony dla platformy .NET Core lub .NET Framework, narzędzia programu EF Core "pożyczać" środowisko uruchomieniowe z projektu.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="4a8fc-161">Nie można wykonać, jeśli model programu EF Core jest w bibliotece klas programu .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="4a8fc-162">.NET Standard nie jest rzeczywistą implementację .NET; jest określenie zestawu interfejsów API, który musi obsługiwać implementacje platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="4a8fc-163">W związku z tym .NET Standard nie jest wystarczająca dla narzędzi programu EF Core do wykonywania kodu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="4a8fc-164">Projekt zastępczy, utworzone jako projekt startowy zawiera platformy konkretnego celu, w którym narzędzia można załadować biblioteki klas .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="4a8fc-165">Środowiska ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a8fc-165">ASP.NET Core environment</span></span>

<span data-ttu-id="4a8fc-166">Aby określić środowisko dla projektów ASP.NET Core, ustaw **env:ASPNETCORE_ENVIRONMENT** przed uruchomieniem polecenia.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="4a8fc-167">Wspólne parametry</span><span class="sxs-lookup"><span data-stu-id="4a8fc-167">Common parameters</span></span>

<span data-ttu-id="4a8fc-168">W poniższej tabeli przedstawiono parametry, które są wspólne dla wszystkich poleceń programu EF Core:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="4a8fc-169">Parametr</span><span class="sxs-lookup"><span data-stu-id="4a8fc-169">Parameter</span></span>                 | <span data-ttu-id="4a8fc-170">Opis</span><span class="sxs-lookup"><span data-stu-id="4a8fc-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4a8fc-171">-Kontekstu \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="4a8fc-171">-Context \<String></span></span>        | <span data-ttu-id="4a8fc-172">`DbContext` Klasy.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-172">The `DbContext` class to use.</span></span> <span data-ttu-id="4a8fc-173">Nazwa klasy tylko lub w pełni kwalifikowana za pomocą przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="4a8fc-174">Jeśli ten parametr zostanie pominięty, programem EF Core umożliwia znalezienie klasy kontekstu.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="4a8fc-175">W przypadku wielu klas kontekstu, ten parametr jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="4a8fc-176">-Projekt \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="4a8fc-176">-Project \<String></span></span>        | <span data-ttu-id="4a8fc-177">Projekt docelowy.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-177">The target project.</span></span> <span data-ttu-id="4a8fc-178">Jeśli ten parametr zostanie pominięty, **projekt domyślny** dla **Konsola Menedżera pakietów** służy jako projekt docelowy.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="4a8fc-179">Projekt startowy - \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="4a8fc-179">-StartupProject \<String></span></span> | <span data-ttu-id="4a8fc-180">Projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-180">The startup project.</span></span> <span data-ttu-id="4a8fc-181">Jeśli ten parametr zostanie pominięty, **projekt startowy** w **właściwości rozwiązania** służy jako projekt docelowy.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="4a8fc-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="4a8fc-182">-Verbose</span></span>                  | <span data-ttu-id="4a8fc-183">Wyświetlić pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="4a8fc-184">Aby wyświetlić Pomoc dotyczącą polecenia, użyj programu PowerShell `Get-Help` polecenia.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="4a8fc-185">Parametry kontekstu, projekt i projekt startowy obsługują rozszerzenia karty.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="4a8fc-186">Dodaj migracji</span><span class="sxs-lookup"><span data-stu-id="4a8fc-186">Add-Migration</span></span>

<span data-ttu-id="4a8fc-187">Dodaje nową migrację.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-187">Adds a new migration.</span></span>

<span data-ttu-id="4a8fc-188">Parametry:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-188">Parameters:</span></span>

| <span data-ttu-id="4a8fc-189">Parametr</span><span class="sxs-lookup"><span data-stu-id="4a8fc-189">Parameter</span></span>                         | <span data-ttu-id="4a8fc-190">Opis</span><span class="sxs-lookup"><span data-stu-id="4a8fc-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4a8fc-191"><nobr>— Nazwa \<ciąg ><nobr></span><span class="sxs-lookup"><span data-stu-id="4a8fc-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="4a8fc-192">Nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-192">The name of the migration.</span></span> <span data-ttu-id="4a8fc-193">To jest parametr pozycyjne i jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="4a8fc-194"><nobr>-OutputDir \<ciąg ></nobr></span><span class="sxs-lookup"><span data-stu-id="4a8fc-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="4a8fc-195">Katalog (i podrzędnej przestrzeni nazw) do użycia.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="4a8fc-196">Ścieżki są względne wobec katalogu docelowego projektu.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="4a8fc-197">Wartość domyślna to "Migracja".</span><span class="sxs-lookup"><span data-stu-id="4a8fc-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="4a8fc-198">Baza danych listy</span><span class="sxs-lookup"><span data-stu-id="4a8fc-198">Drop-Database</span></span>

<span data-ttu-id="4a8fc-199">Porzuca bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-199">Drops the database.</span></span>

<span data-ttu-id="4a8fc-200">Parametry:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-200">Parameters:</span></span>

| <span data-ttu-id="4a8fc-201">Parametr</span><span class="sxs-lookup"><span data-stu-id="4a8fc-201">Parameter</span></span> | <span data-ttu-id="4a8fc-202">Opis</span><span class="sxs-lookup"><span data-stu-id="4a8fc-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="4a8fc-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="4a8fc-203">-WhatIf</span></span>   | <span data-ttu-id="4a8fc-204">Pokaż bazę danych, która będzie można usunąć, ale nie należy usuwać jej.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="4a8fc-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="4a8fc-205">Get-DbContext</span></span>

<span data-ttu-id="4a8fc-206">Wyświetla dostępne `DbContext` typów.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-206">Lists available `DbContext` types.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="4a8fc-207">Usuń migrację</span><span class="sxs-lookup"><span data-stu-id="4a8fc-207">Remove-Migration</span></span>

<span data-ttu-id="4a8fc-208">Usuwa ostatniej migracji (wycofanie zmian w kodzie, które zostały przygotowane do migracji).</span><span class="sxs-lookup"><span data-stu-id="4a8fc-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="4a8fc-209">Parametry:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-209">Parameters:</span></span>

| <span data-ttu-id="4a8fc-210">Parametr</span><span class="sxs-lookup"><span data-stu-id="4a8fc-210">Parameter</span></span> | <span data-ttu-id="4a8fc-211">Opis</span><span class="sxs-lookup"><span data-stu-id="4a8fc-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="4a8fc-212">-Force</span><span class="sxs-lookup"><span data-stu-id="4a8fc-212">-Force</span></span>    | <span data-ttu-id="4a8fc-213">Przywróć migracji (wycofać zmiany, które zostały zastosowane do bazy danych).</span><span class="sxs-lookup"><span data-stu-id="4a8fc-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="4a8fc-214">Tworzenie szkieletu DbContext</span><span class="sxs-lookup"><span data-stu-id="4a8fc-214">Scaffold-DbContext</span></span>

<span data-ttu-id="4a8fc-215">Generuje kod dla `DbContext` i typy jednostek bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="4a8fc-216">Aby `Scaffold-DbContext` można wygenerować typu jednostki, w tabeli bazy danych musi mieć klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="4a8fc-217">Parametry:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-217">Parameters:</span></span>

| <span data-ttu-id="4a8fc-218">Parametr</span><span class="sxs-lookup"><span data-stu-id="4a8fc-218">Parameter</span></span>                          | <span data-ttu-id="4a8fc-219">Opis</span><span class="sxs-lookup"><span data-stu-id="4a8fc-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4a8fc-220"><nobr>-Connection \<ciąg ></nobr></span><span class="sxs-lookup"><span data-stu-id="4a8fc-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="4a8fc-221">Parametry połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-221">The connection string to the database.</span></span> <span data-ttu-id="4a8fc-222">W przypadku projektów ASP.NET Core 2.x wartość może być *nazwa =\<nazwa parametrów połączenia >*.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="4a8fc-223">W takim przypadku nazwa pochodzi od źródła konfiguracji, które są skonfigurowane dla projektu.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="4a8fc-224">To jest parametr pozycyjne i jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="4a8fc-225"><nobr>-Provider \<ciąg ></nobr></span><span class="sxs-lookup"><span data-stu-id="4a8fc-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="4a8fc-226">Dostawca do użycia.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-226">The provider to use.</span></span> <span data-ttu-id="4a8fc-227">Zazwyczaj jest to nazwa pakietu NuGet, na przykład: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="4a8fc-228">To jest parametr pozycyjne i jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="4a8fc-229">-OutputDir \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="4a8fc-229">-OutputDir \<String></span></span>               | <span data-ttu-id="4a8fc-230">Katalog, który można umieścić pliki w.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-230">The directory to put files in.</span></span> <span data-ttu-id="4a8fc-231">Ścieżki są względne wobec katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="4a8fc-232">-ContextDir \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="4a8fc-232">-ContextDir \<String></span></span>              | <span data-ttu-id="4a8fc-233">Katalog, aby umieścić `DbContext` w pliku.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="4a8fc-234">Ścieżki są względne wobec katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="4a8fc-235">-Kontekstu \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="4a8fc-235">-Context \<String></span></span>                 | <span data-ttu-id="4a8fc-236">Nazwa `DbContext` klasy do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="4a8fc-237">-Schematów \<String [] ></span><span class="sxs-lookup"><span data-stu-id="4a8fc-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="4a8fc-238">Schematów tabel w celu wygenerowania typów jednostek dla.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="4a8fc-239">Jeśli ten parametr zostanie pominięty, uwzględniono wszystkich schematów.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="4a8fc-240">-Tabele \<String [] ></span><span class="sxs-lookup"><span data-stu-id="4a8fc-240">-Tables \<String[]></span></span>                | <span data-ttu-id="4a8fc-241">Tabele, aby wygenerować typy jednostek dla.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-241">The tables to generate entity types for.</span></span> <span data-ttu-id="4a8fc-242">Jeżeli ten parametr zostanie pominięty, uwzględniane są wszystkie tabele.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="4a8fc-243">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="4a8fc-243">-DataAnnotations</span></span>                   | <span data-ttu-id="4a8fc-244">Aby skonfigurować model (tam, gdzie jest to możliwe), należy użyć atrybutów.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="4a8fc-245">Jeżeli pominięto ten parametr jest używany tylko interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="4a8fc-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="4a8fc-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="4a8fc-247">Użyj nazwy tabel i kolumn dokładnie tak, jak pojawiają się w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="4a8fc-248">Jeśli ten parametr zostanie pominięty, nazwy baz danych zostały zmienione, aby ściślej odpowiadają Konwencji stylistycznych nazwy języka C#.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="4a8fc-249">-Force</span><span class="sxs-lookup"><span data-stu-id="4a8fc-249">-Force</span></span>                             | <span data-ttu-id="4a8fc-250">Nadpisz istniejące pliki.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="4a8fc-251">Przykład:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="4a8fc-252">Przykład szkielety mechanizmów tylko wybrane tabele i tworzy kontekst w oddzielnym folderze z określoną nazwą:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="4a8fc-253">Skrypt migracji</span><span class="sxs-lookup"><span data-stu-id="4a8fc-253">Script-Migration</span></span>

<span data-ttu-id="4a8fc-254">Generuje skrypt SQL, który dotyczy wszystkich zmian z jedną migrację wybranych innej wybranej migracji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="4a8fc-255">Parametry:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-255">Parameters:</span></span>

| <span data-ttu-id="4a8fc-256">Parametr</span><span class="sxs-lookup"><span data-stu-id="4a8fc-256">Parameter</span></span>                | <span data-ttu-id="4a8fc-257">Opis</span><span class="sxs-lookup"><span data-stu-id="4a8fc-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4a8fc-258">*-From* \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="4a8fc-258">*-From* \<String></span></span>        | <span data-ttu-id="4a8fc-259">Począwszy od migracji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-259">The starting migration.</span></span> <span data-ttu-id="4a8fc-260">Migracje mogą być określane według nazwy lub identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="4a8fc-261">Numer 0 jest przypadkiem szczególnym, oznacza to *przed migracją pierwszy*.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="4a8fc-262">Wartość domyślna to 0.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="4a8fc-263">*— Do* \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="4a8fc-263">*-To* \<String></span></span>          | <span data-ttu-id="4a8fc-264">Końcowy migracji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-264">The ending migration.</span></span> <span data-ttu-id="4a8fc-265">Domyślnie do ostatniego migracji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="4a8fc-266"><nobr>-Idempotentne</nobr></span><span class="sxs-lookup"><span data-stu-id="4a8fc-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="4a8fc-267">Generowanie skryptu, który może służyć w bazie danych w każdej migracji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="4a8fc-268">-Dane wyjściowe \<ciąg ></span><span class="sxs-lookup"><span data-stu-id="4a8fc-268">-Output \<String></span></span>        | <span data-ttu-id="4a8fc-269">Plik można zapisać wynik.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-269">The file to write the result to.</span></span> <span data-ttu-id="4a8fc-270">Jeśli ten parametr zostanie pominięty, plik jest tworzony z użyciem nazwy wygenerowanej w tym samym folderze, ponieważ pliki środowiska uruchomieniowego aplikacji są tworzone, na przykład: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="4a8fc-271">To, z danych wyjściowych z obsługą i parametrów rozszerzenia karty.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="4a8fc-272">Poniższy przykład tworzy skrypt w przypadku migracji InitialCreate przy użyciu nazwy migracji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="4a8fc-273">Poniższy przykład tworzy skrypt wszystkich migracji po migracji InitialCreate przy użyciu identyfikatora migracji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="4a8fc-274">Aktualizuj bazy danych</span><span class="sxs-lookup"><span data-stu-id="4a8fc-274">Update-Database</span></span>

<span data-ttu-id="4a8fc-275">Aktualizuje bazę danych do ostatniej migracji lub określony migracji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="4a8fc-276">Parametr</span><span class="sxs-lookup"><span data-stu-id="4a8fc-276">Parameter</span></span>                           | <span data-ttu-id="4a8fc-277">Opis</span><span class="sxs-lookup"><span data-stu-id="4a8fc-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="4a8fc-278"><nobr>*— Migracja* \<ciąg ></nobr></span><span class="sxs-lookup"><span data-stu-id="4a8fc-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="4a8fc-279">Migracja docelowego.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-279">The target migration.</span></span> <span data-ttu-id="4a8fc-280">Migracje mogą być określane według nazwy lub identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="4a8fc-281">Numer 0 jest przypadkiem szczególnym, oznacza to *przed migracją pierwszy* i powoduje, że wszystkich migracji, które można przywrócić.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="4a8fc-282">Jeśli migracja nie zostanie określony, polecenie domyślne ostatnio migracji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="4a8fc-283">Parametr migracji obsługuje rozszerzenia karty.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="4a8fc-284">Poniższy przykład przywraca wszystkich migracji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="4a8fc-285">Poniższe przykłady aktualizują bazę danych do określonego migracji.</span><span class="sxs-lookup"><span data-stu-id="4a8fc-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="4a8fc-286">Pierwszy używa nazwy migracji, a drugi używa Identyfikatora migracji:</span><span class="sxs-lookup"><span data-stu-id="4a8fc-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="4a8fc-287">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4a8fc-287">Additional resources</span></span>

* [<span data-ttu-id="4a8fc-288">Migracje</span><span class="sxs-lookup"><span data-stu-id="4a8fc-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="4a8fc-289">Odtwarzanie</span><span class="sxs-lookup"><span data-stu-id="4a8fc-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
