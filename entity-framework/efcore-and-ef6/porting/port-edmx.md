---
title: Przenoszenie z EF6 do EF Core — przenoszenie modelu opartego na EDMX - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416924"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="5ef4a-102">Przenoszenie modelu opartego na EF6 EDMX do ef core</span><span class="sxs-lookup"><span data-stu-id="5ef4a-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="5ef4a-103">EF Core nie obsługuje formatu pliku EDMX dla modeli.</span><span class="sxs-lookup"><span data-stu-id="5ef4a-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="5ef4a-104">Najlepszym rozwiązaniem do portu tych modeli, jest wygenerowanie nowego modelu opartego na kodzie z bazy danych dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5ef4a-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="5ef4a-105">Instalowanie pakietów EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="5ef4a-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="5ef4a-106">Zainstaluj `Microsoft.EntityFrameworkCore.Tools` pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="5ef4a-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="5ef4a-107">Regeneracja modelu</span><span class="sxs-lookup"><span data-stu-id="5ef4a-107">Regenerate the model</span></span>

<span data-ttu-id="5ef4a-108">Teraz można użyć funkcji inżynierii odwrotnej do utworzenia modelu na podstawie istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5ef4a-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="5ef4a-109">Uruchom następujące polecenie w konsoli Menedżera pakietów (Narzędzia – > Menedżer pakietów NuGet – > konsola Menedżera pakietów).</span><span class="sxs-lookup"><span data-stu-id="5ef4a-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="5ef4a-110">Zobacz [Konsoli Menedżera pakietów (Visual Studio)](../../core/miscellaneous/cli/powershell.md) dla opcji polecenia do szkieletu podzbiór tabel itp.</span><span class="sxs-lookup"><span data-stu-id="5ef4a-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="5ef4a-111">Na przykład w tym miejscu znajduje się polecenie szkieletu modelu z bazy danych blogów w wystąpieniu usługi SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="5ef4a-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="5ef4a-112">Usuń model EF6</span><span class="sxs-lookup"><span data-stu-id="5ef4a-112">Remove EF6 model</span></span>

<span data-ttu-id="5ef4a-113">Można teraz usunąć model EF6 z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5ef4a-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="5ef4a-114">Jest w porządku, aby pozostawić ef6 NuGet pakiet (EntityFramework) zainstalowany, jak EF Core i EF6 mogą być używane obok siebie w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5ef4a-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="5ef4a-115">Jednak jeśli nie zamierzasz używać EF6 w żadnych obszarach aplikacji, a następnie odinstalowanie pakietu pomoże dać błędy kompilacji na kawałki kodu, które wymagają uwagi.</span><span class="sxs-lookup"><span data-stu-id="5ef4a-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="5ef4a-116">Aktualizowanie kodu</span><span class="sxs-lookup"><span data-stu-id="5ef4a-116">Update your code</span></span>

<span data-ttu-id="5ef4a-117">W tym momencie jest to kwestia adresowania błędów kompilacji i przeglądania kodu, aby sprawdzić, czy zmiany zachowania między EF6 i EF Core wpłynie na Ciebie.</span><span class="sxs-lookup"><span data-stu-id="5ef4a-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="5ef4a-118">Przetestuj port</span><span class="sxs-lookup"><span data-stu-id="5ef4a-118">Test the port</span></span>

<span data-ttu-id="5ef4a-119">Tylko dlatego, że aplikacja kompiluje, nie oznacza, że jest pomyślnie przeniesiony do EF Core.</span><span class="sxs-lookup"><span data-stu-id="5ef4a-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="5ef4a-120">Należy przetestować wszystkie obszary aplikacji, aby upewnić się, że żadna ze zmian zachowania nie wpłynęła negatywnie na aplikację.</span><span class="sxs-lookup"><span data-stu-id="5ef4a-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
