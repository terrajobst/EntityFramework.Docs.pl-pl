---
title: Przenoszenie z programu EF6 do programu EF Core — przenoszenie modelu opartego na EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997414"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="4b436-102">Przenoszenie modelu opartego na EDMX EF6 do programu EF Core</span><span class="sxs-lookup"><span data-stu-id="4b436-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="4b436-103">EF Core nie obsługuje formatu pliku EDMX dla modeli.</span><span class="sxs-lookup"><span data-stu-id="4b436-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="4b436-104">Najlepszym rozwiązaniem do portu tych modeli jest do generowania nowego modelu opartego na kodzie z bazy danych dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4b436-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="4b436-105">Instalowanie pakietów programu EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="4b436-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="4b436-106">Zainstaluj `Microsoft.EntityFrameworkCore.Tools` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="4b436-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="4b436-107">Ponowne generowanie modelu</span><span class="sxs-lookup"><span data-stu-id="4b436-107">Regenerate the model</span></span>

<span data-ttu-id="4b436-108">Funkcje odtwarzania można teraz używać do tworzenia modeli, w oparciu o istniejącą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="4b436-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="4b436-109">Uruchom następujące polecenie w konsoli Menedżera pakietów (Narzędzia -> Menedżer pakietów NuGet -> Konsola Menedżera pakietów).</span><span class="sxs-lookup"><span data-stu-id="4b436-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="4b436-110">Zobacz [Konsola Menedżera pakietów (Visual Studio)](../../core/miscellaneous/cli/powershell.md) opcji polecenia do tworzenia szkieletu podzbioru tabel itp.</span><span class="sxs-lookup"><span data-stu-id="4b436-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="4b436-111">Na przykład poniżej przedstawiono polecenia do tworzenia szkieletu modelu z bazy danych do obsługi blogów w ramach wystąpienia programu SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="4b436-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="4b436-112">Usuń EF6 model</span><span class="sxs-lookup"><span data-stu-id="4b436-112">Remove EF6 model</span></span>

<span data-ttu-id="4b436-113">EF6 model należy teraz usunąć z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4b436-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="4b436-114">Jest dobrym rozwiązaniem pozostawić pakiet NuGet platformy EF6 (EntityFramework), zainstalowane, zgodnie z programem EF Core i EF6 mogą być używane side-by-side w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4b436-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="4b436-115">Jednak w przypadku korzystania z platformy EF6 w żadnych obszarów aplikacji nie są pomyślny przebieg operacji, następnie odinstalowaniu pakietu pomoże spowodować błędy kompilacji na fragmenty kodu, które wymagają uwagi.</span><span class="sxs-lookup"><span data-stu-id="4b436-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="4b436-116">Aktualizowanie kodu</span><span class="sxs-lookup"><span data-stu-id="4b436-116">Update your code</span></span>

<span data-ttu-id="4b436-117">W tym momencie jest kwestią adresowania błędy kompilacji i recenzowania kod, aby sprawdzić, czy zmiany zachowania między EF6 i EF Core będzie miało wpływ na możesz.</span><span class="sxs-lookup"><span data-stu-id="4b436-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="4b436-118">Testowanie portu</span><span class="sxs-lookup"><span data-stu-id="4b436-118">Test the port</span></span>

<span data-ttu-id="4b436-119">Po prostu, ponieważ kompiluje aplikację, nie znaczy, że pomyślnie są przenoszone do programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="4b436-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="4b436-120">Należy przetestować wszystkie obszary w aplikacji, aby upewnić się, że żadne zmiany zachowania niekorzystnie wpłynąć na nie wpływ aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4b436-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="4b436-121">Zobacz [rozpoczęcie pracy z programem EF Core programu ASP.NET Core z istniejącej bazy danych](xref:core/get-started/aspnetcore/existing-db) Aby uzyskać dodatkowe informacje na temat sposobu pracy z istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="4b436-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
