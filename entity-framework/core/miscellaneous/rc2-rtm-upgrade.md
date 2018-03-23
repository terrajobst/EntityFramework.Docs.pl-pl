---
title: Uaktualnianie z EF Core RC2 1.0 w wersji RTM - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 4bb4c5736708413f6581cad250b089b7bc22a559
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="14aca-102">Uaktualnianie z EF Core 1.0 RC2 do wersji RTM</span><span class="sxs-lookup"><span data-stu-id="14aca-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="14aca-103">Ten artykuł zawiera wskazówki dotyczące przenoszenia aplikacji skompilowanej za pomocą pakietów RC2 do 1.0.0 RTM.</span><span class="sxs-lookup"><span data-stu-id="14aca-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="14aca-104">Wersje pakietów</span><span class="sxs-lookup"><span data-stu-id="14aca-104">Package Versions</span></span>

<span data-ttu-id="14aca-105">Nazwy pakietów najwyższego poziomu, które zazwyczaj można zainstalować na aplikację jednakowy RC2 i RTM.</span><span class="sxs-lookup"><span data-stu-id="14aca-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="14aca-106">**Należy uaktualnić do wersji RTM zainstalowanych pakietów:**</span><span class="sxs-lookup"><span data-stu-id="14aca-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="14aca-107">Środowisko uruchomieniowe pakietów (np. `Microsoft.EntityFrameworkCore.SqlServer`) zmieniła się z `1.0.0-rc2-final` do `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="14aca-107">Runtime packages (e.g. `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="14aca-108">`Microsoft.EntityFrameworkCore.Tools` Pakietu zmieniła się z `1.0.0-preview1-final` do `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="14aca-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="14aca-109">Należy pamiętać, że narzędzia nadal wersji wstępnej.</span><span class="sxs-lookup"><span data-stu-id="14aca-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="14aca-110">Migracja istniejących może być konieczne maxLength dodane</span><span class="sxs-lookup"><span data-stu-id="14aca-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="14aca-111">RC2, definicji kolumny w przypadku migracji tablicą jak `table.Column<string>(nullable: true)` i długość kolumny, która została wyszukiwane w niektóre metadane są przechowywane w kodzie migracji.</span><span class="sxs-lookup"><span data-stu-id="14aca-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="14aca-112">W wersji RTM, długość jest teraz zawarta w kodzie szkieletu `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="14aca-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="14aca-113">Wszystkie istniejące migracji, które zostały szkieletu przed użyciem RTM nie będą miały `maxLength` : określono nieprawidłowy argument.</span><span class="sxs-lookup"><span data-stu-id="14aca-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="14aca-114">Oznacza to, maksymalna długość obsługiwana przez bazę danych, który będzie używany (`nvarchar(max)` w programie SQL Server).</span><span class="sxs-lookup"><span data-stu-id="14aca-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="14aca-115">Może to być poprawnie dla niektórych kolumn, ale kolumny będące częścią klucza, klucz obcy lub indeks muszą zostać zaktualizowane maksymalną długość.</span><span class="sxs-lookup"><span data-stu-id="14aca-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="14aca-116">Według Konwencji 450 jest maksymalną długość używane dla kluczy, kluczy obcych i indeksowanych kolumn.</span><span class="sxs-lookup"><span data-stu-id="14aca-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="14aca-117">Jeśli długość skonfigurowano jawnie w modelu, następnie należy użyć tej długości zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="14aca-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="14aca-118">**ASP.NET Identity**</span><span class="sxs-lookup"><span data-stu-id="14aca-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="14aca-119">Ta zmiana wpływa na projektów, użyj tożsamości platformy ASP.NET, które zostały utworzone z wersji pre-RTM szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="14aca-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="14aca-120">Szablon projektu zawiera migracji używany do tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="14aca-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="14aca-121">Tej migracji należy edytować, aby określić maksymalną długość `256` dla następujących kolumn.</span><span class="sxs-lookup"><span data-stu-id="14aca-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="14aca-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="14aca-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="14aca-123">Nazwa</span><span class="sxs-lookup"><span data-stu-id="14aca-123">Name</span></span>

    * <span data-ttu-id="14aca-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="14aca-124">NormalizedName</span></span>

*  <span data-ttu-id="14aca-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="14aca-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="14aca-126">Adres e-mail</span><span class="sxs-lookup"><span data-stu-id="14aca-126">Email</span></span>

   * <span data-ttu-id="14aca-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="14aca-127">NormalizedEmail</span></span>

   * <span data-ttu-id="14aca-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="14aca-128">NormalizedUserName</span></span>

   * <span data-ttu-id="14aca-129">UserName</span><span class="sxs-lookup"><span data-stu-id="14aca-129">UserName</span></span>

<span data-ttu-id="14aca-130">Nie można wprowadzić tej zmiany spowoduje następujący wyjątek podczas początkowej migracji jest stosowany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="14aca-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="14aca-131">Oprogramowanie .NET core: Usuń "imports" w pliku project.json</span><span class="sxs-lookup"><span data-stu-id="14aca-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="14aca-132">Jeśli zostały przeznaczonych dla platformy .NET Core z RC2, trzeba było dodać `imports` do pliku project.json tymczasowo dla niektórych podstawowych EF zależności nie obsługuje .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="14aca-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="14aca-133">Te można teraz usunąć.</span><span class="sxs-lookup"><span data-stu-id="14aca-133">These can now be removed.</span></span>

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
> <span data-ttu-id="14aca-134">Począwszy od wersji 1.0 RTM, [.NET Core SDK](https://www.microsoft.com/net/download/core) nie obsługuje już `project.json` lub tworzenie aplikacji platformy .NET Core za pomocą programu Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="14aca-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="14aca-135">Firma Microsoft zaleca [migracji z project.json do csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="14aca-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="14aca-136">Jeśli używasz programu Visual Studio, zaleca się uaktualniania do [programu Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="14aca-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="14aca-137">Platformy uniwersalnej systemu Windows: Dodaj przekierowania powiązania</span><span class="sxs-lookup"><span data-stu-id="14aca-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="14aca-138">Próba uruchomienia polecenia EF na projekty systemu Windows platformy Uniwersalnej powoduje następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="14aca-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="14aca-139">Należy ręcznie Dodaj przekierowania powiązania do projektu platformy uniwersalnej systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="14aca-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="14aca-140">Utwórz plik o nazwie `App.config` w projekcie folder główny i Dodaj przekierowania do wersji zestawu poprawne.</span><span class="sxs-lookup"><span data-stu-id="14aca-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

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
