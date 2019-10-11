---
title: Dokumentacja narzędzi EF Core Tools (konsola Menedżera pakietów) — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 45370a82131da9db8b724fe395d41b1e3641fcf8
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181333"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="1fd26-102">Dokumentacja narzędzi Entity Framework Core Tools — konsola Menedżera pakietów w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1fd26-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="1fd26-103">Narzędzia konsoli Menedżera pakietów (PMC) dla Entity Framework Core wykonują zadania deweloperskie czasu projektowania.</span><span class="sxs-lookup"><span data-stu-id="1fd26-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="1fd26-104">Na przykład tworzą [migracje](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), stosują migracje i generują kod dla modelu na podstawie istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1fd26-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="1fd26-105">Polecenia są uruchamiane w programie Visual Studio przy użyciu [konsoli Menedżera pakietów](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="1fd26-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="1fd26-106">Te narzędzia współpracują z projektami .NET Framework i .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1fd26-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="1fd26-107">Jeśli nie korzystasz z programu Visual Studio, zamiast tego zalecamy używanie [EF Core narzędzi wiersza polecenia](dotnet.md) .</span><span class="sxs-lookup"><span data-stu-id="1fd26-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="1fd26-108">Narzędzia interfejsu wiersza polecenia są na wielu platformach i uruchamiane w wierszu poleceń.</span><span class="sxs-lookup"><span data-stu-id="1fd26-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="1fd26-109">Instalowanie narzędzi</span><span class="sxs-lookup"><span data-stu-id="1fd26-109">Installing the tools</span></span>

<span data-ttu-id="1fd26-110">Procedury instalowania i aktualizowania narzędzi różnią się między wersjami ASP.NET Core 2.1 + i starszymi lub innymi typami projektów.</span><span class="sxs-lookup"><span data-stu-id="1fd26-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="1fd26-111">ASP.NET Core wersja 2,1 lub nowsza</span><span class="sxs-lookup"><span data-stu-id="1fd26-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="1fd26-112">Narzędzia są automatycznie dołączane do ASP.NET Core 2.1 i projektu, ponieważ pakiet `Microsoft.EntityFrameworkCore.Tools` jest zawarty w [pakiecie Microsoft. AspNetCore. app](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1fd26-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="1fd26-113">W związku z tym nie trzeba wykonywać żadnych czynności w celu zainstalowania narzędzi, ale trzeba:</span><span class="sxs-lookup"><span data-stu-id="1fd26-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>
* <span data-ttu-id="1fd26-114">Przywróć pakiety przed użyciem narzędzi w nowym projekcie.</span><span class="sxs-lookup"><span data-stu-id="1fd26-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="1fd26-115">Zainstaluj pakiet, aby zaktualizować narzędzia do nowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="1fd26-116">Aby upewnić się, że korzystasz z najnowszej wersji narzędzi, zalecamy również wykonanie następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1fd26-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="1fd26-117">Edytuj plik *. csproj* i Dodaj wiersz określający najnowszą wersję pakietu [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) .</span><span class="sxs-lookup"><span data-stu-id="1fd26-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="1fd26-118">Na przykład plik *. csproj* może zawierać `ItemGroup`, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="1fd26-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="1fd26-119">Zaktualizuj narzędzia, gdy zostanie wyświetlony komunikat podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="1fd26-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="1fd26-120">Narzędzia EF Core w wersji "2.1.1-RTM-30846" są starsze niż środowisko uruchomieniowe "2.1.3-RTM-32065".</span><span class="sxs-lookup"><span data-stu-id="1fd26-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="1fd26-121">Zaktualizuj narzędzia dla najnowszych funkcji i poprawek błędów.</span><span class="sxs-lookup"><span data-stu-id="1fd26-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="1fd26-122">Aby zaktualizować narzędzia:</span><span class="sxs-lookup"><span data-stu-id="1fd26-122">To update the tools:</span></span>
* <span data-ttu-id="1fd26-123">Zainstaluj najnowszą zestaw .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="1fd26-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="1fd26-124">Zaktualizuj program Visual Studio do najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="1fd26-125">Edytuj plik *. csproj* , tak aby zawierał odwołanie do pakietu dla najnowszych narzędzi, jak pokazano wcześniej.</span><span class="sxs-lookup"><span data-stu-id="1fd26-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="1fd26-126">Inne wersje i typy projektów</span><span class="sxs-lookup"><span data-stu-id="1fd26-126">Other versions and project types</span></span>

<span data-ttu-id="1fd26-127">Zainstaluj narzędzia konsoli Menedżera pakietów, uruchamiając następujące polecenie w **konsoli Menedżera pakietów**:</span><span class="sxs-lookup"><span data-stu-id="1fd26-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="1fd26-128">Zaktualizuj narzędzia, uruchamiając następujące polecenie w **konsoli Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="1fd26-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="1fd26-129">Weryfikowanie instalacji</span><span class="sxs-lookup"><span data-stu-id="1fd26-129">Verify the installation</span></span>

<span data-ttu-id="1fd26-130">Sprawdź, czy narzędzia są zainstalowane, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="1fd26-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="1fd26-131">Dane wyjściowe wyglądają następująco (nie informuje o jakiej wersji narzędzi, z których korzystasz):</span><span class="sxs-lookup"><span data-stu-id="1fd26-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

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

## <a name="using-the-tools"></a><span data-ttu-id="1fd26-132">Korzystanie z narzędzi</span><span class="sxs-lookup"><span data-stu-id="1fd26-132">Using the tools</span></span>

<span data-ttu-id="1fd26-133">Przed rozpoczęciem korzystania z narzędzi:</span><span class="sxs-lookup"><span data-stu-id="1fd26-133">Before using the tools:</span></span>
* <span data-ttu-id="1fd26-134">Zapoznaj się z różnicą między projektem docelowym i początkowym.</span><span class="sxs-lookup"><span data-stu-id="1fd26-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="1fd26-135">Dowiedz się, jak używać narzędzi z bibliotekami klas .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="1fd26-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="1fd26-136">W przypadku projektów ASP.NET Core Ustaw środowisko.</span><span class="sxs-lookup"><span data-stu-id="1fd26-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="1fd26-137">Projekt docelowy i startowy</span><span class="sxs-lookup"><span data-stu-id="1fd26-137">Target and startup project</span></span>

<span data-ttu-id="1fd26-138">Polecenia odnoszą się do *projektu* i *projektu startowego*.</span><span class="sxs-lookup"><span data-stu-id="1fd26-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="1fd26-139">*Projekt* jest również znany jako *projekt docelowy* , ponieważ jest to miejsce, w którym polecenia dodają lub usuwają pliki.</span><span class="sxs-lookup"><span data-stu-id="1fd26-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="1fd26-140">Domyślnie **domyślnym projektem** wybranym w **konsoli Menedżera pakietów** jest projekt docelowy.</span><span class="sxs-lookup"><span data-stu-id="1fd26-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="1fd26-141">Możesz określić inny projekt jako projekt docelowy przy użyciu opcji <nobr>`--project`</nobr> .</span><span class="sxs-lookup"><span data-stu-id="1fd26-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="1fd26-142">*Projekt startowy* jest tym, że narzędzia kompilują i uruchamiają.</span><span class="sxs-lookup"><span data-stu-id="1fd26-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="1fd26-143">Narzędzia muszą wykonywać kod aplikacji w czasie projektowania, aby uzyskać informacje o projekcie, takie jak parametry połączenia bazy danych i Konfiguracja modelu.</span><span class="sxs-lookup"><span data-stu-id="1fd26-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="1fd26-144">Domyślnie **projekt startowy** w **Eksplorator rozwiązań** jest projektem startowym.</span><span class="sxs-lookup"><span data-stu-id="1fd26-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="1fd26-145">Możesz określić inny projekt jako projekt startowy przy użyciu opcji <nobr>`--startup-project`</nobr> .</span><span class="sxs-lookup"><span data-stu-id="1fd26-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="1fd26-146">Projekt startowy i projekt docelowy są często tymi samymi projektami.</span><span class="sxs-lookup"><span data-stu-id="1fd26-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="1fd26-147">Typowy scenariusz, w którym są oddzielnymi projektami, to:</span><span class="sxs-lookup"><span data-stu-id="1fd26-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="1fd26-148">EF Core kontekstu i klasy jednostek znajdują się w bibliotece klas .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1fd26-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="1fd26-149">Aplikacja konsolowa platformy .NET Core lub aplikacja internetowa odwołuje się do biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="1fd26-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="1fd26-150">Możliwe jest również [umieszczenie kodu migracji w bibliotece klas odrębnie od kontekstu EF Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="1fd26-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="1fd26-151">Inne platformy docelowe</span><span class="sxs-lookup"><span data-stu-id="1fd26-151">Other target frameworks</span></span>

<span data-ttu-id="1fd26-152">Narzędzia konsoli Menedżera pakietów współpracują z projektami .NET Core lub .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1fd26-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="1fd26-153">Aplikacje, które mają model EF Core w bibliotece klas .NET Standard mogą nie mieć projektu .NET Core lub .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1fd26-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="1fd26-154">Na przykład jest to prawdziwe w aplikacjach Xamarin i platforma uniwersalna systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="1fd26-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="1fd26-155">W takich przypadkach można utworzyć projekt aplikacji konsolowej .NET Core lub .NET Framework, którego jedynym celem jest działanie jako projekt startowy dla narzędzi.</span><span class="sxs-lookup"><span data-stu-id="1fd26-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="1fd26-156">Projekt może być fikcyjnym projektem bez kodu rzeczywistego &mdash; jest to konieczne tylko w celu udostępnienia obiektu docelowego dla narzędzi.</span><span class="sxs-lookup"><span data-stu-id="1fd26-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="1fd26-157">Dlaczego jest wymagany projekt fikcyjny?</span><span class="sxs-lookup"><span data-stu-id="1fd26-157">Why is a dummy project required?</span></span> <span data-ttu-id="1fd26-158">Jak wspomniano wcześniej, narzędzia muszą wykonać kod aplikacji w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="1fd26-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="1fd26-159">W tym celu należy użyć środowiska uruchomieniowego .NET Core lub .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1fd26-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="1fd26-160">Gdy model EF Core znajduje się w projekcie, który jest przeznaczony dla programu .NET Core lub .NET Framework, narzędzia EF Core zażyczą sobie środowisko uruchomieniowe z projektu.</span><span class="sxs-lookup"><span data-stu-id="1fd26-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="1fd26-161">Nie można tego zrobić, jeśli model EF Core znajduje się w .NET Standardej bibliotece klas.</span><span class="sxs-lookup"><span data-stu-id="1fd26-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="1fd26-162">.NET Standard nie jest rzeczywistą implementacją platformy .NET; jest to specyfikacja zestawu interfejsów API, które muszą być obsługiwane przez implementacje platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="1fd26-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="1fd26-163">W związku z tym .NET Standard nie są wystarczające dla narzędzi EF Core do wykonywania kodu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="1fd26-164">Projekt fikcyjny tworzony do użycia jako projekt startowy zapewnia konkretną platformę docelową, do której narzędzia mogą ładować .NET Standard biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="1fd26-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="1fd26-165">Środowisko ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1fd26-165">ASP.NET Core environment</span></span>

<span data-ttu-id="1fd26-166">Aby określić środowisko dla projektów ASP.NET Core, ustaw **env: ASPNETCORE_ENVIRONMENT** przed uruchomionymi poleceniami.</span><span class="sxs-lookup"><span data-stu-id="1fd26-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="1fd26-167">Parametry wspólne</span><span class="sxs-lookup"><span data-stu-id="1fd26-167">Common parameters</span></span>

<span data-ttu-id="1fd26-168">W poniższej tabeli przedstawiono parametry wspólne dla wszystkich poleceń EF Core:</span><span class="sxs-lookup"><span data-stu-id="1fd26-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="1fd26-169">Parametr</span><span class="sxs-lookup"><span data-stu-id="1fd26-169">Parameter</span></span>                 | <span data-ttu-id="1fd26-170">Opis</span><span class="sxs-lookup"><span data-stu-id="1fd26-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fd26-171">-Context \<String ></span><span class="sxs-lookup"><span data-stu-id="1fd26-171">-Context \<String></span></span>        | <span data-ttu-id="1fd26-172">Klasa `DbContext` do użycia.</span><span class="sxs-lookup"><span data-stu-id="1fd26-172">The `DbContext` class to use.</span></span> <span data-ttu-id="1fd26-173">Nazwa klasy lub w pełni kwalifikowana z przestrzeniami nazw.</span><span class="sxs-lookup"><span data-stu-id="1fd26-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="1fd26-174">Jeśli ten parametr zostanie pominięty, EF Core odnajdzie klasę kontekstową.</span><span class="sxs-lookup"><span data-stu-id="1fd26-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="1fd26-175">Jeśli istnieje wiele klas kontekstu, ten parametr jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="1fd26-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="1fd26-176">-Project @no__t — 0String ></span><span class="sxs-lookup"><span data-stu-id="1fd26-176">-Project \<String></span></span>        | <span data-ttu-id="1fd26-177">Projekt docelowy.</span><span class="sxs-lookup"><span data-stu-id="1fd26-177">The target project.</span></span> <span data-ttu-id="1fd26-178">Jeśli ten parametr zostanie pominięty, **domyślny projekt** **konsoli Menedżera pakietów** jest używany jako projekt docelowy.</span><span class="sxs-lookup"><span data-stu-id="1fd26-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="1fd26-179">-StartupProject \<String ></span><span class="sxs-lookup"><span data-stu-id="1fd26-179">-StartupProject \<String></span></span> | <span data-ttu-id="1fd26-180">Projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="1fd26-180">The startup project.</span></span> <span data-ttu-id="1fd26-181">Jeśli ten parametr zostanie pominięty, **projekt startowy** we **właściwościach rozwiązania** jest używany jako projekt docelowy.</span><span class="sxs-lookup"><span data-stu-id="1fd26-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="1fd26-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="1fd26-182">-Verbose</span></span>                  | <span data-ttu-id="1fd26-183">Pokaż pełne dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="1fd26-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="1fd26-184">Aby wyświetlić informacje pomocy dotyczące polecenia, należy użyć polecenia `Get-Help` programu PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1fd26-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="1fd26-185">Parametry Context, Project i StartupProject obsługują rozszerzanie kart.</span><span class="sxs-lookup"><span data-stu-id="1fd26-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="1fd26-186">Dodawanie-migracja</span><span class="sxs-lookup"><span data-stu-id="1fd26-186">Add-Migration</span></span>

<span data-ttu-id="1fd26-187">Dodaje nową migrację.</span><span class="sxs-lookup"><span data-stu-id="1fd26-187">Adds a new migration.</span></span>

<span data-ttu-id="1fd26-188">Parametry:</span><span class="sxs-lookup"><span data-stu-id="1fd26-188">Parameters:</span></span>

| <span data-ttu-id="1fd26-189">Parametr</span><span class="sxs-lookup"><span data-stu-id="1fd26-189">Parameter</span></span>                         | <span data-ttu-id="1fd26-190">Opis</span><span class="sxs-lookup"><span data-stu-id="1fd26-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fd26-191">@no__t -0-Name \<String > <nobr></span><span class="sxs-lookup"><span data-stu-id="1fd26-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="1fd26-192">Nazwa migracji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-192">The name of the migration.</span></span> <span data-ttu-id="1fd26-193">Jest to parametr pozycyjny i jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="1fd26-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="1fd26-194"><nobr>-OutputDir \<String ></nobr></span><span class="sxs-lookup"><span data-stu-id="1fd26-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="1fd26-195">Katalog (i podrzędna przestrzeń nazw) do użycia.</span><span class="sxs-lookup"><span data-stu-id="1fd26-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="1fd26-196">Ścieżki są względne dla docelowego katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="1fd26-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="1fd26-197">Wartość domyślna to "migracje".</span><span class="sxs-lookup"><span data-stu-id="1fd26-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="1fd26-198">Usuń bazę danych</span><span class="sxs-lookup"><span data-stu-id="1fd26-198">Drop-Database</span></span>

<span data-ttu-id="1fd26-199">Odrzuca bazę danych.</span><span class="sxs-lookup"><span data-stu-id="1fd26-199">Drops the database.</span></span>

<span data-ttu-id="1fd26-200">Parametry:</span><span class="sxs-lookup"><span data-stu-id="1fd26-200">Parameters:</span></span>

| <span data-ttu-id="1fd26-201">Parametr</span><span class="sxs-lookup"><span data-stu-id="1fd26-201">Parameter</span></span> | <span data-ttu-id="1fd26-202">Opis</span><span class="sxs-lookup"><span data-stu-id="1fd26-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="1fd26-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="1fd26-203">-WhatIf</span></span>   | <span data-ttu-id="1fd26-204">Pokazuje, która baza danych zostanie porzucona, ale nie Porzuć jej.</span><span class="sxs-lookup"><span data-stu-id="1fd26-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="1fd26-205">Kontekst Get-</span><span class="sxs-lookup"><span data-stu-id="1fd26-205">Get-DbContext</span></span>

<span data-ttu-id="1fd26-206">Pobiera informacje o typie `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1fd26-206">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="1fd26-207">Usuń migrację</span><span class="sxs-lookup"><span data-stu-id="1fd26-207">Remove-Migration</span></span>

<span data-ttu-id="1fd26-208">Usuwa ostatnią migrację (przywraca zmiany kodu, które zostały wykonane podczas migracji).</span><span class="sxs-lookup"><span data-stu-id="1fd26-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="1fd26-209">Parametry:</span><span class="sxs-lookup"><span data-stu-id="1fd26-209">Parameters:</span></span>

| <span data-ttu-id="1fd26-210">Parametr</span><span class="sxs-lookup"><span data-stu-id="1fd26-210">Parameter</span></span> | <span data-ttu-id="1fd26-211">Opis</span><span class="sxs-lookup"><span data-stu-id="1fd26-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="1fd26-212">-Wymuś</span><span class="sxs-lookup"><span data-stu-id="1fd26-212">-Force</span></span>    | <span data-ttu-id="1fd26-213">Przywróć migrację (Wycofaj zmiany, które zostały zastosowane do bazy danych).</span><span class="sxs-lookup"><span data-stu-id="1fd26-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="1fd26-214">Szkielet — DbContext</span><span class="sxs-lookup"><span data-stu-id="1fd26-214">Scaffold-DbContext</span></span>

<span data-ttu-id="1fd26-215">Generuje kod dla `DbContext` i typów jednostek dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1fd26-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="1fd26-216">Aby `Scaffold-DbContext` w celu wygenerowania typu jednostki, tabela bazy danych musi mieć klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="1fd26-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="1fd26-217">Parametry:</span><span class="sxs-lookup"><span data-stu-id="1fd26-217">Parameters:</span></span>

| <span data-ttu-id="1fd26-218">Parametr</span><span class="sxs-lookup"><span data-stu-id="1fd26-218">Parameter</span></span>                          | <span data-ttu-id="1fd26-219">Opis</span><span class="sxs-lookup"><span data-stu-id="1fd26-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fd26-220"><nobr>-Connection \<String ></nobr></span><span class="sxs-lookup"><span data-stu-id="1fd26-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="1fd26-221">Parametry połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="1fd26-221">The connection string to the database.</span></span> <span data-ttu-id="1fd26-222">W przypadku projektów ASP.NET Core 2. x wartością może być *Nazwa = \<name parametrów połączenia >* .</span><span class="sxs-lookup"><span data-stu-id="1fd26-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="1fd26-223">W takim przypadku nazwa pochodzi ze źródeł konfiguracji skonfigurowanych dla projektu.</span><span class="sxs-lookup"><span data-stu-id="1fd26-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="1fd26-224">Jest to parametr pozycyjny i jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="1fd26-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="1fd26-225"><nobr>-Dostawca \<String ></nobr></span><span class="sxs-lookup"><span data-stu-id="1fd26-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="1fd26-226">Dostawca do użycia.</span><span class="sxs-lookup"><span data-stu-id="1fd26-226">The provider to use.</span></span> <span data-ttu-id="1fd26-227">Zazwyczaj jest to nazwa pakietu NuGet, na przykład: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="1fd26-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="1fd26-228">Jest to parametr pozycyjny i jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="1fd26-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="1fd26-229">-OutputDir \<String ></span><span class="sxs-lookup"><span data-stu-id="1fd26-229">-OutputDir \<String></span></span>               | <span data-ttu-id="1fd26-230">Katalog, w którym mają zostać umieszczone pliki.</span><span class="sxs-lookup"><span data-stu-id="1fd26-230">The directory to put files in.</span></span> <span data-ttu-id="1fd26-231">Ścieżki są względne dla katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="1fd26-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="1fd26-232">-ContextDir \<String ></span><span class="sxs-lookup"><span data-stu-id="1fd26-232">-ContextDir \<String></span></span>              | <span data-ttu-id="1fd26-233">Katalog, w którym ma zostać umieszczony plik `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1fd26-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="1fd26-234">Ścieżki są względne dla katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="1fd26-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="1fd26-235">-Context \<String ></span><span class="sxs-lookup"><span data-stu-id="1fd26-235">-Context \<String></span></span>                 | <span data-ttu-id="1fd26-236">Nazwa klasy `DbContext` do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="1fd26-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="1fd26-237">-Schematy \<String [] ></span><span class="sxs-lookup"><span data-stu-id="1fd26-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="1fd26-238">Schematy tabel, dla których mają zostać wygenerowane typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="1fd26-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="1fd26-239">Jeśli ten parametr zostanie pominięty, zostaną uwzględnione wszystkie schematy.</span><span class="sxs-lookup"><span data-stu-id="1fd26-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="1fd26-240">-Tabele \<String [] ></span><span class="sxs-lookup"><span data-stu-id="1fd26-240">-Tables \<String[]></span></span>                | <span data-ttu-id="1fd26-241">Tabele, dla których mają zostać wygenerowane typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="1fd26-241">The tables to generate entity types for.</span></span> <span data-ttu-id="1fd26-242">Jeśli ten parametr zostanie pominięty, zostaną uwzględnione wszystkie tabele.</span><span class="sxs-lookup"><span data-stu-id="1fd26-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="1fd26-243">— Adnotacje</span><span class="sxs-lookup"><span data-stu-id="1fd26-243">-DataAnnotations</span></span>                   | <span data-ttu-id="1fd26-244">Użyj atrybutów, aby skonfigurować model (tam, gdzie to możliwe).</span><span class="sxs-lookup"><span data-stu-id="1fd26-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="1fd26-245">Jeśli ten parametr zostanie pominięty, zostanie użyty tylko interfejs API Fluent.</span><span class="sxs-lookup"><span data-stu-id="1fd26-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="1fd26-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="1fd26-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="1fd26-247">Nazwy tabel i kolumn należy używać dokładnie tak, jak pojawiają się one w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1fd26-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="1fd26-248">Jeśli ten parametr zostanie pominięty, nazwy baz danych są zmieniane na bardziej ściśle C# zgodne z konwencjami stylu nazwy.</span><span class="sxs-lookup"><span data-stu-id="1fd26-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="1fd26-249">-Wymuś</span><span class="sxs-lookup"><span data-stu-id="1fd26-249">-Force</span></span>                             | <span data-ttu-id="1fd26-250">Zastąp istniejące pliki.</span><span class="sxs-lookup"><span data-stu-id="1fd26-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="1fd26-251">Przykład:</span><span class="sxs-lookup"><span data-stu-id="1fd26-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="1fd26-252">Przykład, który szkieletuje tylko wybrane tabele i tworzy kontekst w osobnym folderze o określonej nazwie:</span><span class="sxs-lookup"><span data-stu-id="1fd26-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="1fd26-253">Skrypt — migracja</span><span class="sxs-lookup"><span data-stu-id="1fd26-253">Script-Migration</span></span>

<span data-ttu-id="1fd26-254">Generuje skrypt SQL, który stosuje wszystkie zmiany z jednej wybranej migracji do innej wybranej migracji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="1fd26-255">Parametry:</span><span class="sxs-lookup"><span data-stu-id="1fd26-255">Parameters:</span></span>

| <span data-ttu-id="1fd26-256">Parametr</span><span class="sxs-lookup"><span data-stu-id="1fd26-256">Parameter</span></span>                | <span data-ttu-id="1fd26-257">Opis</span><span class="sxs-lookup"><span data-stu-id="1fd26-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fd26-258">*-From* \<String ></span><span class="sxs-lookup"><span data-stu-id="1fd26-258">*-From* \<String></span></span>        | <span data-ttu-id="1fd26-259">Rozpoczynanie migracji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-259">The starting migration.</span></span> <span data-ttu-id="1fd26-260">Migracje mogą być identyfikowane według nazwy lub identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="1fd26-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="1fd26-261">Liczba 0 to specjalny przypadek, który oznacza *przed pierwszą migracją*.</span><span class="sxs-lookup"><span data-stu-id="1fd26-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="1fd26-262">Wartość domyślna to 0.</span><span class="sxs-lookup"><span data-stu-id="1fd26-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="1fd26-263">*-Do* \<String ></span><span class="sxs-lookup"><span data-stu-id="1fd26-263">*-To* \<String></span></span>          | <span data-ttu-id="1fd26-264">Kończenie migracji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-264">The ending migration.</span></span> <span data-ttu-id="1fd26-265">Wartość domyślna to Ostatnia migracja.</span><span class="sxs-lookup"><span data-stu-id="1fd26-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="1fd26-266"><nobr>-Idempotentne</nobr></span><span class="sxs-lookup"><span data-stu-id="1fd26-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="1fd26-267">Generuj skrypt, którego można użyć w bazie danych w dowolnej migracji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="1fd26-268">-Output \<String ></span><span class="sxs-lookup"><span data-stu-id="1fd26-268">-Output \<String></span></span>        | <span data-ttu-id="1fd26-269">Plik, w którym ma zostać zapisany wynik.</span><span class="sxs-lookup"><span data-stu-id="1fd26-269">The file to write the result to.</span></span> <span data-ttu-id="1fd26-270">Jeśli ten parametr zostanie pominięty, plik zostanie utworzony przy użyciu wygenerowanej nazwy w tym samym folderze, w którym są tworzone pliki środowiska uruchomieniowego aplikacji, na przykład: */obj/Debug/netcoreapp2.1/ghbkztfz.SQL/* .</span><span class="sxs-lookup"><span data-stu-id="1fd26-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="1fd26-271">Parametry do, od i Output obsługują rozszerzanie tabulacji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="1fd26-272">Poniższy przykład tworzy skrypt migracji InitialCreate przy użyciu nazwy migracji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="1fd26-273">Poniższy przykład tworzy skrypt dla wszystkich migracji po migracji InitialCreate przy użyciu identyfikatora migracji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="1fd26-274">Update-Database</span><span class="sxs-lookup"><span data-stu-id="1fd26-274">Update-Database</span></span>

<span data-ttu-id="1fd26-275">Aktualizuje bazę danych do ostatniej migracji lub do określonej migracji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="1fd26-276">Parametr</span><span class="sxs-lookup"><span data-stu-id="1fd26-276">Parameter</span></span>                           | <span data-ttu-id="1fd26-277">Opis</span><span class="sxs-lookup"><span data-stu-id="1fd26-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1fd26-278"><nobr> *-@No__t migracji* — 2String ></nobr></span><span class="sxs-lookup"><span data-stu-id="1fd26-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="1fd26-279">Migracja docelowa.</span><span class="sxs-lookup"><span data-stu-id="1fd26-279">The target migration.</span></span> <span data-ttu-id="1fd26-280">Migracje mogą być identyfikowane według nazwy lub identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="1fd26-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="1fd26-281">Liczba 0 jest szczególnym przypadkiem *przed pierwszą migracją* i powoduje przywrócenie wszystkich migracji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="1fd26-282">Jeśli migracja nie zostanie określona, polecenie domyślnie przestanie być ostatnią migracją.</span><span class="sxs-lookup"><span data-stu-id="1fd26-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="1fd26-283">Parametr migracji obsługuje rozszerzanie tabulacji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="1fd26-284">Poniższy przykład przywraca wszystkie migracje.</span><span class="sxs-lookup"><span data-stu-id="1fd26-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="1fd26-285">Poniższe przykłady umożliwiają zaktualizowanie bazy danych do określonej migracji.</span><span class="sxs-lookup"><span data-stu-id="1fd26-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="1fd26-286">Pierwsza z nich używa nazwy migracji, a druga używa identyfikatora migracji:</span><span class="sxs-lookup"><span data-stu-id="1fd26-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="1fd26-287">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1fd26-287">Additional resources</span></span>

* [<span data-ttu-id="1fd26-288">Migracje</span><span class="sxs-lookup"><span data-stu-id="1fd26-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="1fd26-289">Odwrócenie inżynierii</span><span class="sxs-lookup"><span data-stu-id="1fd26-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
