---
title: Eksportowanie z EF6 do Core EF — przenoszenie pliku EDMX, na podstawie modelu
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="07f21-102">Eksportowanie EF6 EDMX na podstawie modelu EF Core</span><span class="sxs-lookup"><span data-stu-id="07f21-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="07f21-103">Podstawowe EF nie obsługuje formatu pliku EDMX dla modeli.</span><span class="sxs-lookup"><span data-stu-id="07f21-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="07f21-104">Najlepszym rozwiązaniem do portu te modele jest aby wygenerować nowy model opartej na kodzie z bazy danych dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="07f21-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="07f21-105">Instalowanie pakietów EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="07f21-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="07f21-106">Zainstaluj `Microsoft.EntityFrameworkCore.Tools` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="07f21-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="07f21-107">Ponowne generowanie modelu</span><span class="sxs-lookup"><span data-stu-id="07f21-107">Regenerate the model</span></span>

<span data-ttu-id="07f21-108">Funkcje odtwarzania można teraz używać do utworzenia modelu na podstawie istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="07f21-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="07f21-109">Uruchom następujące polecenie w konsoli Menedżera pakietów (Narzędzia –> Menedżera pakietów NuGet –> Konsola Menedżera pakietów).</span><span class="sxs-lookup"><span data-stu-id="07f21-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="07f21-110">Zobacz [Konsola Menedżera pakietów (Visual Studio)](../../core/miscellaneous/cli/powershell.md) dla opcji polecenia utworzyć szkielet podzbiór tabelach itp.</span><span class="sxs-lookup"><span data-stu-id="07f21-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="07f21-111">Na przykład w tym miejscu jest polecenie, aby utworzyć szkielet modelu z bazy danych w wystąpieniu programu SQL Server LocalDB obsługi blogów.</span><span class="sxs-lookup"><span data-stu-id="07f21-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="07f21-112">Usuń EF6 model</span><span class="sxs-lookup"><span data-stu-id="07f21-112">Remove EF6 model</span></span>

<span data-ttu-id="07f21-113">Teraz należy usunąć modelu EF6 z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="07f21-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="07f21-114">Bardzo można pozostawić pakiet EF6 NuGet (EntityFramework), zainstalowane, EF Core i EF6 mogą być używane obok siebie w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="07f21-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="07f21-115">Jednak jeśli nie są zamierza użyć EF6 w obszary aplikacji, następnie odinstalowaniu pakietu pomoże spowodować błędy kompilacji na fragmenty kodu, które wymagają uwagi.</span><span class="sxs-lookup"><span data-stu-id="07f21-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="07f21-116">Zaktualizuj kod</span><span class="sxs-lookup"><span data-stu-id="07f21-116">Update your code</span></span>

<span data-ttu-id="07f21-117">W tym momencie jest kwestią adresowania błędy kompilacji i recenzowania kod, aby zobaczyć, czy zmiany zachowania między EF6 i podstawowe EF będzie miało wpływ na możesz.</span><span class="sxs-lookup"><span data-stu-id="07f21-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="07f21-118">Testowanie portu</span><span class="sxs-lookup"><span data-stu-id="07f21-118">Test the port</span></span>

<span data-ttu-id="07f21-119">Tak, ponieważ aplikacja kompiluje, oznacza to, że pomyślnie jest przenoszone na EF rdzeń.</span><span class="sxs-lookup"><span data-stu-id="07f21-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="07f21-120">Należy przetestować wszystkie obszary w aplikacji w celu zapewnienia, że zmiany zachowania ma negatywny wpływ na aplikację.</span><span class="sxs-lookup"><span data-stu-id="07f21-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="07f21-121">Zobacz [wprowadzenie EF rdzeni na platformy ASP.NET Core z istniejącej bazy danych](xref:core/get-started/aspnetcore/existing-db) uzyskać dodatkowe informacje na temat pracy z istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="07f21-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
