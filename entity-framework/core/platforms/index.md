---
title: Obsługiwane implementacje platformy .NET — EF Core
author: bricelam
ms.date: 03/03/2020
uid: core/platforms/index
ms.openlocfilehash: 693d4cae85eddf86d01e17084415147c52a008c7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417323"
---
# <a name="net-implementations-supported-by-ef-core"></a><span data-ttu-id="0b877-102">Implementacje .NET obsługiwane przez EF Core</span><span class="sxs-lookup"><span data-stu-id="0b877-102">.NET implementations supported by EF Core</span></span>

<span data-ttu-id="0b877-103">Chcemy, aby EF Core był dostępny dla deweloperów we wszystkich nowoczesnych implementacjach platformy .NET i nadal pracujemy nad tym celem.</span><span class="sxs-lookup"><span data-stu-id="0b877-103">We want EF Core to be available to developers on all modern .NET implementations, and we're still working towards that goal.</span></span> <span data-ttu-id="0b877-104">Podczas gdy wsparcie EF Core w programie .NET Core jest objęte automatycznymi testami i wiele aplikacji, o których wiadomo, że używa go pomyślnie, Mono, Xamarin i platformy uniwersalnej systemu zbytu mają pewne problemy.</span><span class="sxs-lookup"><span data-stu-id="0b877-104">While EF Core's support on .NET Core is covered by automated testing and many applications known to be using it successfully, Mono, Xamarin and UWP have some issues.</span></span>

## <a name="overview"></a><span data-ttu-id="0b877-105">Omówienie</span><span class="sxs-lookup"><span data-stu-id="0b877-105">Overview</span></span>

<span data-ttu-id="0b877-106">Poniższa tabela zawiera wskazówki dla każdej implementacji platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="0b877-106">The following table provides guidance for each .NET implementation:</span></span>

| <span data-ttu-id="0b877-107">EF Core</span><span class="sxs-lookup"><span data-stu-id="0b877-107">EF Core</span></span>                       | <span data-ttu-id="0b877-108">2.1 i 3.1</span><span class="sxs-lookup"><span data-stu-id="0b877-108">2.1 and 3.1</span></span> |
|:------------------------------|:------------|
| <span data-ttu-id="0b877-109">.NET Standard</span><span class="sxs-lookup"><span data-stu-id="0b877-109">.NET Standard</span></span>                 | <span data-ttu-id="0b877-110">2.0</span><span class="sxs-lookup"><span data-stu-id="0b877-110">2.0</span></span>         |
| <span data-ttu-id="0b877-111">.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b877-111">.NET Core</span></span>                     | <span data-ttu-id="0b877-112">2.0</span><span class="sxs-lookup"><span data-stu-id="0b877-112">2.0</span></span>         |
| <span data-ttu-id="0b877-113">.NET Framework<sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="0b877-113">.NET Framework<sup>(1)</sup></span></span>  | <span data-ttu-id="0b877-114">4.7.2</span><span class="sxs-lookup"><span data-stu-id="0b877-114">4.7.2</span></span>       |
| <span data-ttu-id="0b877-115">Mono</span><span class="sxs-lookup"><span data-stu-id="0b877-115">Mono</span></span>                          | <span data-ttu-id="0b877-116">5.4</span><span class="sxs-lookup"><span data-stu-id="0b877-116">5.4</span></span>         |
| <span data-ttu-id="0b877-117">Xamarin.iOS<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="0b877-117">Xamarin.iOS<sup>(2)</sup></span></span>     | <span data-ttu-id="0b877-118">10.14</span><span class="sxs-lookup"><span data-stu-id="0b877-118">10.14</span></span>       |
| <span data-ttu-id="0b877-119">Xamarin.Android<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="0b877-119">Xamarin.Android<sup>(2)</sup></span></span> | <span data-ttu-id="0b877-120">8.0</span><span class="sxs-lookup"><span data-stu-id="0b877-120">8.0</span></span>         |
| <span data-ttu-id="0b877-121">UwP<sup>(3)</sup></span><span class="sxs-lookup"><span data-stu-id="0b877-121">UWP<sup>(3)</sup></span></span>             | <span data-ttu-id="0b877-122">10.0.16299</span><span class="sxs-lookup"><span data-stu-id="0b877-122">10.0.16299</span></span>  |
| <span data-ttu-id="0b877-123">Jedność<sup>(4)</sup></span><span class="sxs-lookup"><span data-stu-id="0b877-123">Unity<sup>(4)</sup></span></span>           | <span data-ttu-id="0b877-124">2018.1</span><span class="sxs-lookup"><span data-stu-id="0b877-124">2018.1</span></span>      |

<span data-ttu-id="0b877-125"><sup>(1)</sup> Zobacz sekcję [.NET Framework](#net-framework) poniżej.</span><span class="sxs-lookup"><span data-stu-id="0b877-125"><sup>(1)</sup> See the [.NET Framework](#net-framework) section below.</span></span>

<span data-ttu-id="0b877-126"><sup>(2)</sup> Istnieją problemy i znane ograniczenia z xamarin, które mogą uniemożliwić niektóre aplikacje opracowane przy użyciu EF Core działa poprawnie.</span><span class="sxs-lookup"><span data-stu-id="0b877-126"><sup>(2)</sup> There are issues and known limitations with Xamarin which may prevent some applications developed using EF Core from working correctly.</span></span> <span data-ttu-id="0b877-127">Sprawdź listę [aktywnych problemów](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) pod kątem obejść.</span><span class="sxs-lookup"><span data-stu-id="0b877-127">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) for workarounds.</span></span>

<span data-ttu-id="0b877-128"><sup>(3)</sup> EF Core 2.0.1 i nowsze zalecane.</span><span class="sxs-lookup"><span data-stu-id="0b877-128"><sup>(3)</sup> EF Core 2.0.1 and newer recommended.</span></span> <span data-ttu-id="0b877-129">Zainstaluj [pakiet platformy .NET Core UWP 6.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span><span class="sxs-lookup"><span data-stu-id="0b877-129">Install the [.NET Core UWP 6.x package](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span></span> <span data-ttu-id="0b877-130">Zobacz sekcję [Uniwersalna platforma systemu Windows](#universal-windows-platform) w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="0b877-130">See the [Universal Windows Platform](#universal-windows-platform) section of this article.</span></span>

<span data-ttu-id="0b877-131"><sup>(4)</sup> Istnieją problemy i znane ograniczenia jedności.</span><span class="sxs-lookup"><span data-stu-id="0b877-131"><sup>(4)</sup> There are issues and known limitations with Unity.</span></span> <span data-ttu-id="0b877-132">Sprawdź listę [aktywnych problemów](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span><span class="sxs-lookup"><span data-stu-id="0b877-132">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span></span>

## <a name="net-framework"></a><span data-ttu-id="0b877-133">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="0b877-133">.NET Framework</span></span>

<span data-ttu-id="0b877-134">Aplikacje przeznaczone dla programu .NET Framework mogą wymagać zmian w celu pracy z bibliotekami .NET Standard:</span><span class="sxs-lookup"><span data-stu-id="0b877-134">Applications that target .NET Framework may need changes to work with .NET Standard libraries:</span></span>

<span data-ttu-id="0b877-135">Edytuj plik projektu i upewnij się, że w początkowej grupie właściwości pojawi się następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="0b877-135">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

<span data-ttu-id="0b877-136">W przypadku projektów testowych upewnij się również, że dostępny jest następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="0b877-136">For test projects, also make sure the following entry is present:</span></span>

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

<span data-ttu-id="0b877-137">Jeśli chcesz użyć starszej wersji programu Visual Studio, upewnij się, że [uaktualnisz klienta NuGet do wersji 3.6.0,](https://www.nuget.org/downloads) aby pracować z bibliotekami .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="0b877-137">If you want to use an older version of Visual Studio, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

<span data-ttu-id="0b877-138">Zalecamy również migrację z pakietu NuGet.config do PackageReference, jeśli to możliwe.</span><span class="sxs-lookup"><span data-stu-id="0b877-138">We also recommend migrating from NuGet packages.config to PackageReference if possible.</span></span> <span data-ttu-id="0b877-139">Dodaj następującą właściwość do pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="0b877-139">Add the following property to your project file:</span></span>

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a><span data-ttu-id="0b877-140">Platforma uniwersalna systemu Windows</span><span class="sxs-lookup"><span data-stu-id="0b877-140">Universal Windows Platform</span></span>

<span data-ttu-id="0b877-141">Starsze wersje platformy uniwersalnej systemu Windows i platformy uniwersalnej systemu Windows firmy .NET miały wiele problemów ze zgodnością, szczególnie w przypadku aplikacji skompilowanych z natywnym pasmem narzędzi .NET.</span><span class="sxs-lookup"><span data-stu-id="0b877-141">Earlier versions of EF Core and .NET UWP had numerous compatibility issues, especially with applications compiled with the .NET Native toolchain.</span></span> <span data-ttu-id="0b877-142">Nowa wersja platformy uniwersalnej systemu Windows .NET dodaje obsługę platformy .NET Standard 2.0 i zawiera program .NET Native 2.0, który rozwiązuje większość wcześniej zgłoszonych problemów ze zgodnością.</span><span class="sxs-lookup"><span data-stu-id="0b877-142">The new .NET UWP version adds support for .NET Standard 2.0 and contains .NET Native 2.0, which fixes most of the compatibility issues previously reported.</span></span> <span data-ttu-id="0b877-143">EF Core 2.0.1 został przetestowany dokładniej za pomocą platformy uniwersalnej systemu uniwersalnego systemu, ale testowanie nie jest zautomatyzowane.</span><span class="sxs-lookup"><span data-stu-id="0b877-143">EF Core 2.0.1 has been tested more thoroughly with UWP but testing is not automated.</span></span>

<span data-ttu-id="0b877-144">W przypadku korzystania z EF Core na platformie uniwersalnej systemu wyurzcie na platformy uniwersalnej systemu:</span><span class="sxs-lookup"><span data-stu-id="0b877-144">When using EF Core on UWP:</span></span>

* <span data-ttu-id="0b877-145">Aby zoptymalizować wydajność kwerendy, należy unikać typów anonimowych w zapytaniach LINQ.</span><span class="sxs-lookup"><span data-stu-id="0b877-145">To optimize query performance, avoid anonymous types in LINQ queries.</span></span> <span data-ttu-id="0b877-146">Wdrażanie aplikacji platformy uniwersalnej systemu i platformy uniwersalnej w sklepie z aplikacjami wymaga skompilowania aplikacji z programem .NET Native.</span><span class="sxs-lookup"><span data-stu-id="0b877-146">Deploying a UWP application to the app store requires an application to be compiled with .NET Native.</span></span> <span data-ttu-id="0b877-147">Zapytania z typami anonimowymi mają gorszą wydajność na natywnej .NET.</span><span class="sxs-lookup"><span data-stu-id="0b877-147">Queries with anonymous types have worse performance on .NET Native.</span></span>

* <span data-ttu-id="0b877-148">Aby `SaveChanges()` zoptymalizować wydajność, należy użyć [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) i zaimplementować [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx)i [INotifyCollectionZmieniane](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) w typach jednostek.</span><span class="sxs-lookup"><span data-stu-id="0b877-148">To optimize `SaveChanges()` performance, use [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) and implement [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx), and [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types.</span></span>

## <a name="report-issues"></a><span data-ttu-id="0b877-149">Zgłaszanie problemów</span><span class="sxs-lookup"><span data-stu-id="0b877-149">Report issues</span></span>

<span data-ttu-id="0b877-150">Dla każdej kombinacji, która nie działa zgodnie z oczekiwaniami, zachęcamy do tworzenia nowych problemów na [tracker problemu EF Core](https://github.com/aspnet/entityframeworkcore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="0b877-150">For any combination that doesn�t work as expected, we encourage creating new issues on the [EF Core issue tracker](https://github.com/aspnet/entityframeworkcore/issues/new).</span></span> <span data-ttu-id="0b877-151">W przypadku problemów specyficznych dla platformy Xamarin użyj modułu śledzącego problemy dla [platformy Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) lub [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span><span class="sxs-lookup"><span data-stu-id="0b877-151">For Xamarin-specific issues use the issue tracker for [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) or [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span></span>
