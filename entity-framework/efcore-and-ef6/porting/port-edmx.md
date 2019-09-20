---
title: Przenoszenie z EF6 do EF Core-Porting model oparty na EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: a1c978e141f47a39fff59eb5fc417a63afd37b04
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71148996"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="e472c-102">Przenoszenie modelu opartego na EF6 EDMX do EF Core</span><span class="sxs-lookup"><span data-stu-id="e472c-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="e472c-103">EF Core nie obsługuje formatu pliku EDMX dla modeli.</span><span class="sxs-lookup"><span data-stu-id="e472c-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="e472c-104">Najlepszą opcją przenoszenia tych modeli jest generowanie nowego modelu opartego na kodzie z bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e472c-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="e472c-105">Zainstaluj EF Core pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="e472c-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="e472c-106">Zainstaluj pakiet `Microsoft.EntityFrameworkCore.Tools` NuGet.</span><span class="sxs-lookup"><span data-stu-id="e472c-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="e472c-107">Ponowne generowanie modelu</span><span class="sxs-lookup"><span data-stu-id="e472c-107">Regenerate the model</span></span>

<span data-ttu-id="e472c-108">Teraz można użyć funkcji odtwarzania, aby utworzyć model oparty na istniejącej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e472c-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="e472c-109">Uruchom następujące polecenie w konsoli Menedżera pakietów (Narzędzia — > Menedżer pakietów NuGet — > konsoli Menedżera pakietów).</span><span class="sxs-lookup"><span data-stu-id="e472c-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="e472c-110">Zobacz [konsola Menedżera pakietów (Visual Studio)](../../core/miscellaneous/cli/powershell.md) , aby wyświetlić opcje poleceń umożliwiające tworzenie szkieletu podzestawu tabel itp.</span><span class="sxs-lookup"><span data-stu-id="e472c-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="e472c-111">Na przykład poniżej przedstawiono polecenie do tworzenia szkieletu modelu z bazy danych blogów w wystąpieniu SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="e472c-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="e472c-112">Usuń model EF6</span><span class="sxs-lookup"><span data-stu-id="e472c-112">Remove EF6 model</span></span>

<span data-ttu-id="e472c-113">Teraz można usunąć model EF6 z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e472c-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="e472c-114">Warto pozostawić zainstalowany pakiet NuGet EF6 (EntityFramework), ponieważ EF Core i EF6 mogą być używane obok siebie w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e472c-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="e472c-115">Jeśli jednak nie zamierzasz korzystać z EF6 w żadnych obszarach aplikacji, dezinstalacja pakietu pomoże Ci w wydaniu błędów kompilacji dla fragmentów kodu, które wymagają uwagi.</span><span class="sxs-lookup"><span data-stu-id="e472c-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="e472c-116">Aktualizowanie kodu</span><span class="sxs-lookup"><span data-stu-id="e472c-116">Update your code</span></span>

<span data-ttu-id="e472c-117">W tym momencie należy rozmieścić błędy kompilacji i przeglądać kod, aby sprawdzić, czy zachowanie zmienia się między EF6 a EF Core będzie miało wpływ na Ciebie.</span><span class="sxs-lookup"><span data-stu-id="e472c-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="e472c-118">Testowanie portu</span><span class="sxs-lookup"><span data-stu-id="e472c-118">Test the port</span></span>

<span data-ttu-id="e472c-119">Tak samo, jak Twoja aplikacja kompiluje, nie oznacza, że została pomyślnie przeniesiono do EF Core.</span><span class="sxs-lookup"><span data-stu-id="e472c-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="e472c-120">Należy przetestować wszystkie obszary aplikacji, aby upewnić się, że żadna zmiana zachowania nie miała negatywnego wpływu na aplikację.</span><span class="sxs-lookup"><span data-stu-id="e472c-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
