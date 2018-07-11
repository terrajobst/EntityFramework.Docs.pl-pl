---
title: Uaktualnianie z programu EF Core 1.0 RC2 do RTM — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 9561eac253517188251fece9a03f434482246051
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949060"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="ea298-102">Uaktualnianie z programu EF Core 1.0 RC2 do RTM</span><span class="sxs-lookup"><span data-stu-id="ea298-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="ea298-103">Ten artykuł zawiera wskazówki dotyczące przenoszenia aplikacji skompilowanej za pomocą pakietów 1.0.0 RC2 RTM.</span><span class="sxs-lookup"><span data-stu-id="ea298-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="ea298-104">Wersje pakietów</span><span class="sxs-lookup"><span data-stu-id="ea298-104">Package Versions</span></span>

<span data-ttu-id="ea298-105">Nazwy pakietów najwyższego poziomu, które zazwyczaj można zainstalować na aplikację nie zmienił się od wersji RC2 i RTM.</span><span class="sxs-lookup"><span data-stu-id="ea298-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="ea298-106">**Należy uaktualnić zainstalowane pakiety do wersji RTM:**</span><span class="sxs-lookup"><span data-stu-id="ea298-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="ea298-107">Pakiety środowiska uruchomieniowego (na przykład `Microsoft.EntityFrameworkCore.SqlServer`) zmieniła się z `1.0.0-rc2-final` do `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="ea298-107">Runtime packages (for example, `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="ea298-108">`Microsoft.EntityFrameworkCore.Tools` Pakietu zmieniła się z `1.0.0-preview1-final` do `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="ea298-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="ea298-109">Należy pamiętać, że narzędzia jest nadal w wersji wstępnej.</span><span class="sxs-lookup"><span data-stu-id="ea298-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="ea298-110">Migracja istniejących może być konieczne maxLength dodane</span><span class="sxs-lookup"><span data-stu-id="ea298-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="ea298-111">W wersji RC2, definicji kolumny, w przypadku migracji wyglądał jak `table.Column<string>(nullable: true)` i długość kolumny został wyszukiwane w niektóre metadane są przechowywane w kodzie migracji.</span><span class="sxs-lookup"><span data-stu-id="ea298-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="ea298-112">W wersji RTM, długość jest teraz zawarta w utworzony szkielet kodu `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="ea298-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="ea298-113">Istniejące migracji, które zostały szkieletu przed użyciem RTM nie będzie miał `maxLength` określony argument.</span><span class="sxs-lookup"><span data-stu-id="ea298-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="ea298-114">Oznacza to, maksymalna długość obsługiwane przez bazę danych, który będzie używany (`nvarchar(max)` w programie SQL Server).</span><span class="sxs-lookup"><span data-stu-id="ea298-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="ea298-115">Może to być dobrym rozwiązaniem dla niektórych kolumn, ale także kolumny, które są częścią klucza, klucz obcy lub indeksu, należy zaktualizować maksymalną długość.</span><span class="sxs-lookup"><span data-stu-id="ea298-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="ea298-116">Zgodnie z Konwencją 450 jest maksymalna długość używane do kluczy, kluczy obcych i indeksowanych kolumn.</span><span class="sxs-lookup"><span data-stu-id="ea298-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="ea298-117">Jeśli długość skonfigurowano jawnie w modelu, następnie należy użyć tej długości zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="ea298-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="ea298-118">**ASP.NET Identity**</span><span class="sxs-lookup"><span data-stu-id="ea298-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="ea298-119">Ta zmiana ma wpływ na projekty użycia produktu ASP.NET Identity, które zostały utworzone na podstawie pre-RTM szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="ea298-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="ea298-120">Szablon projektu obejmuje migrację użyty do utworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ea298-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="ea298-121">Ta migracja musi być edytowany, aby określić maksymalną długość `256` dla następujących kolumn.</span><span class="sxs-lookup"><span data-stu-id="ea298-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="ea298-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="ea298-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="ea298-123">Nazwa</span><span class="sxs-lookup"><span data-stu-id="ea298-123">Name</span></span>

    * <span data-ttu-id="ea298-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="ea298-124">NormalizedName</span></span>

*  <span data-ttu-id="ea298-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="ea298-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="ea298-126">Adres e-mail</span><span class="sxs-lookup"><span data-stu-id="ea298-126">Email</span></span>

   * <span data-ttu-id="ea298-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="ea298-127">NormalizedEmail</span></span>

   * <span data-ttu-id="ea298-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="ea298-128">NormalizedUserName</span></span>

   * <span data-ttu-id="ea298-129">UserName</span><span class="sxs-lookup"><span data-stu-id="ea298-129">UserName</span></span>

<span data-ttu-id="ea298-130">Nie można dokonać tej zmiany spowoduje następujący wyjątek podczas początkowej migracji jest stosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ea298-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="ea298-131">.NET core: Usuń "import" w pliku project.json</span><span class="sxs-lookup"><span data-stu-id="ea298-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="ea298-132">Jeśli zostały przeznaczone dla platformy .NET Core za pomocą RC2, trzeba było dodać `imports` do pliku project.json jako rozwiązanie tymczasowe dla niektórych zależności programu EF Core nie obsługuje .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="ea298-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="ea298-133">Te można teraz usunąć.</span><span class="sxs-lookup"><span data-stu-id="ea298-133">These can now be removed.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> <span data-ttu-id="ea298-134">Począwszy od wersji 1.0 RTM, [zestawu .NET Core SDK](https://www.microsoft.com/net/download/core) nie obsługuje już `project.json` lub tworzenia aplikacji platformy .NET Core przy użyciu programu Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="ea298-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="ea298-135">Firma Microsoft zaleca [migracji z plików project.json do csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="ea298-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="ea298-136">Jeśli używasz programu Visual Studio, zaleca się uaktualnienie do [programu Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="ea298-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="ea298-137">Platformy uniwersalnej systemu Windows: Dodać przekierowania powiązań</span><span class="sxs-lookup"><span data-stu-id="ea298-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="ea298-138">Przy próbie uruchomienia programu EF poleceń na wynikach projektów platformy uniwersalnej Windows (UWP) w następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="ea298-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="ea298-139">Należy ręcznie dodać przekierowania powiązań do projektu platformy uniwersalnej systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="ea298-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="ea298-140">Utwórz plik o nazwie `App.config` w projekcie folder główny i dodać przekierowania do wersji poprawny zestaw.</span><span class="sxs-lookup"><span data-stu-id="ea298-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
