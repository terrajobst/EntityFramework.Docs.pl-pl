---
title: Obsługiwane implementacje .NET — EF Core
author: bricelam
ms.date: 03/03/2020
uid: core/platforms/index
ms.openlocfilehash: 693d4cae85eddf86d01e17084415147c52a008c7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417323"
---
# <a name="net-implementations-supported-by-ef-core"></a><span data-ttu-id="1c015-102">Implementacje platformy .NET obsługiwane przez EF Core</span><span class="sxs-lookup"><span data-stu-id="1c015-102">.NET implementations supported by EF Core</span></span>

<span data-ttu-id="1c015-103">Chcemy, aby EF Core były dostępne dla deweloperów we wszystkich nowoczesnych implementacjach platformy .NET i nadal pracujemy nad tym celem.</span><span class="sxs-lookup"><span data-stu-id="1c015-103">We want EF Core to be available to developers on all modern .NET implementations, and we're still working towards that goal.</span></span> <span data-ttu-id="1c015-104">Mimo że obsługa EF Core na platformie .NET Core jest objęta testowaniem automatycznym i wiele aplikacji, o których wiadomo, że ich używanie powiodło się, rozwiązanie mono, Xamarin i platformy UWP zawierają pewne problemy.</span><span class="sxs-lookup"><span data-stu-id="1c015-104">While EF Core's support on .NET Core is covered by automated testing and many applications known to be using it successfully, Mono, Xamarin and UWP have some issues.</span></span>

## <a name="overview"></a><span data-ttu-id="1c015-105">Omówienie</span><span class="sxs-lookup"><span data-stu-id="1c015-105">Overview</span></span>

<span data-ttu-id="1c015-106">W poniższej tabeli przedstawiono wskazówki dotyczące każdej implementacji platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="1c015-106">The following table provides guidance for each .NET implementation:</span></span>

| <span data-ttu-id="1c015-107">EF Core</span><span class="sxs-lookup"><span data-stu-id="1c015-107">EF Core</span></span>                       | <span data-ttu-id="1c015-108">2,1 i 3,1</span><span class="sxs-lookup"><span data-stu-id="1c015-108">2.1 and 3.1</span></span> |
|:------------------------------|:------------|
| <span data-ttu-id="1c015-109">.NET Standard</span><span class="sxs-lookup"><span data-stu-id="1c015-109">.NET Standard</span></span>                 | <span data-ttu-id="1c015-110">2.0</span><span class="sxs-lookup"><span data-stu-id="1c015-110">2.0</span></span>         |
| <span data-ttu-id="1c015-111">.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c015-111">.NET Core</span></span>                     | <span data-ttu-id="1c015-112">2.0</span><span class="sxs-lookup"><span data-stu-id="1c015-112">2.0</span></span>         |
| <span data-ttu-id="1c015-113">.NET Framework<sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="1c015-113">.NET Framework<sup>(1)</sup></span></span>  | <span data-ttu-id="1c015-114">4.7.2</span><span class="sxs-lookup"><span data-stu-id="1c015-114">4.7.2</span></span>       |
| <span data-ttu-id="1c015-115">Mono</span><span class="sxs-lookup"><span data-stu-id="1c015-115">Mono</span></span>                          | <span data-ttu-id="1c015-116">5.4</span><span class="sxs-lookup"><span data-stu-id="1c015-116">5.4</span></span>         |
| <span data-ttu-id="1c015-117">Xamarin. iOS<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="1c015-117">Xamarin.iOS<sup>(2)</sup></span></span>     | <span data-ttu-id="1c015-118">10.14</span><span class="sxs-lookup"><span data-stu-id="1c015-118">10.14</span></span>       |
| <span data-ttu-id="1c015-119">Xamarin. Android<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="1c015-119">Xamarin.Android<sup>(2)</sup></span></span> | <span data-ttu-id="1c015-120">8.0</span><span class="sxs-lookup"><span data-stu-id="1c015-120">8.0</span></span>         |
| <span data-ttu-id="1c015-121">PLATFORMY UWP<sup>(3)</sup></span><span class="sxs-lookup"><span data-stu-id="1c015-121">UWP<sup>(3)</sup></span></span>             | <span data-ttu-id="1c015-122">10.0.16299</span><span class="sxs-lookup"><span data-stu-id="1c015-122">10.0.16299</span></span>  |
| <span data-ttu-id="1c015-123">Unity<sup>(4)</sup></span><span class="sxs-lookup"><span data-stu-id="1c015-123">Unity<sup>(4)</sup></span></span>           | <span data-ttu-id="1c015-124">2018,1</span><span class="sxs-lookup"><span data-stu-id="1c015-124">2018.1</span></span>      |

<span data-ttu-id="1c015-125"><sup>(1)</sup> zapoznaj się z sekcją [.NET Framework](#net-framework) poniżej.</span><span class="sxs-lookup"><span data-stu-id="1c015-125"><sup>(1)</sup> See the [.NET Framework](#net-framework) section below.</span></span>

<span data-ttu-id="1c015-126"><sup>(2)</sup> istnieją problemy i znane ograniczenia dotyczące platformy Xamarin, które mogą uniemożliwić poprawne działanie niektórych aplikacji korzystających z programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="1c015-126"><sup>(2)</sup> There are issues and known limitations with Xamarin which may prevent some applications developed using EF Core from working correctly.</span></span> <span data-ttu-id="1c015-127">Zapoznaj się z listą [aktywnych problemów](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) dotyczących obejść.</span><span class="sxs-lookup"><span data-stu-id="1c015-127">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) for workarounds.</span></span>

<span data-ttu-id="1c015-128"><sup>(3)</sup> EF Core 2.0.1 i nowsze zalecane.</span><span class="sxs-lookup"><span data-stu-id="1c015-128"><sup>(3)</sup> EF Core 2.0.1 and newer recommended.</span></span> <span data-ttu-id="1c015-129">Zainstaluj [pakiet .NET Core platformy UWP 6. x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span><span class="sxs-lookup"><span data-stu-id="1c015-129">Install the [.NET Core UWP 6.x package](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span></span> <span data-ttu-id="1c015-130">Zapoznaj się z sekcją [platforma uniwersalna systemu Windows](#universal-windows-platform) tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="1c015-130">See the [Universal Windows Platform](#universal-windows-platform) section of this article.</span></span>

<span data-ttu-id="1c015-131"><sup>(4)</sup> występują problemy i znane ograniczenia dotyczące aparatu Unity.</span><span class="sxs-lookup"><span data-stu-id="1c015-131"><sup>(4)</sup> There are issues and known limitations with Unity.</span></span> <span data-ttu-id="1c015-132">Sprawdź listę [aktywnych problemów](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span><span class="sxs-lookup"><span data-stu-id="1c015-132">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span></span>

## <a name="net-framework"></a><span data-ttu-id="1c015-133">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="1c015-133">.NET Framework</span></span>

<span data-ttu-id="1c015-134">Aplikacje, które są przeznaczone dla .NET Framework mogą potrzebować zmian do pracy z bibliotekami .NET Standard:</span><span class="sxs-lookup"><span data-stu-id="1c015-134">Applications that target .NET Framework may need changes to work with .NET Standard libraries:</span></span>

<span data-ttu-id="1c015-135">Edytuj plik projektu i upewnij się, że w grupie Właściwości początkowej pojawia się następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="1c015-135">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

<span data-ttu-id="1c015-136">W przypadku projektów testowych upewnij się również, że jest obecny następujący wpis:</span><span class="sxs-lookup"><span data-stu-id="1c015-136">For test projects, also make sure the following entry is present:</span></span>

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

<span data-ttu-id="1c015-137">Jeśli chcesz użyć starszej wersji programu Visual Studio, upewnij się, że [uaktualniasz klienta NuGet do wersji 3.6.0](https://www.nuget.org/downloads) , aby współpracował z bibliotekami .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="1c015-137">If you want to use an older version of Visual Studio, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

<span data-ttu-id="1c015-138">Zalecamy również Migrowanie z pliku Packages. config programu NuGet do PackageReference, jeśli jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="1c015-138">We also recommend migrating from NuGet packages.config to PackageReference if possible.</span></span> <span data-ttu-id="1c015-139">Dodaj następującą właściwość do pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="1c015-139">Add the following property to your project file:</span></span>

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a><span data-ttu-id="1c015-140">Platforma uniwersalna systemu Windows</span><span class="sxs-lookup"><span data-stu-id="1c015-140">Universal Windows Platform</span></span>

<span data-ttu-id="1c015-141">We wcześniejszych wersjach EF Core i .NET platformy UWP istniały liczne problemy ze zgodnością, szczególnie w przypadku aplikacji skompilowanych za pomocą .NET Native łańcucha narzędzi.</span><span class="sxs-lookup"><span data-stu-id="1c015-141">Earlier versions of EF Core and .NET UWP had numerous compatibility issues, especially with applications compiled with the .NET Native toolchain.</span></span> <span data-ttu-id="1c015-142">Nowa wersja programu .NET platformy UWP dodaje obsługę .NET Standard 2,0 i zawiera .NET Native 2,0, które naprawiają większość raportowanych wcześniej problemów ze zgodnością.</span><span class="sxs-lookup"><span data-stu-id="1c015-142">The new .NET UWP version adds support for .NET Standard 2.0 and contains .NET Native 2.0, which fixes most of the compatibility issues previously reported.</span></span> <span data-ttu-id="1c015-143">EF Core 2.0.1 został dokładnie przetestowany przy użyciu platformy UWP, ale testowanie nie jest zautomatyzowane.</span><span class="sxs-lookup"><span data-stu-id="1c015-143">EF Core 2.0.1 has been tested more thoroughly with UWP but testing is not automated.</span></span>

<span data-ttu-id="1c015-144">W przypadku korzystania z EF Core w platformy UWP:</span><span class="sxs-lookup"><span data-stu-id="1c015-144">When using EF Core on UWP:</span></span>

* <span data-ttu-id="1c015-145">Aby zoptymalizować wydajność zapytań, unikaj typów anonimowych w zapytaniach LINQ.</span><span class="sxs-lookup"><span data-stu-id="1c015-145">To optimize query performance, avoid anonymous types in LINQ queries.</span></span> <span data-ttu-id="1c015-146">Wdrożenie aplikacji platformy UWP w sklepie App Store wymaga, aby aplikacja była skompilowana przy użyciu .NET Native.</span><span class="sxs-lookup"><span data-stu-id="1c015-146">Deploying a UWP application to the app store requires an application to be compiled with .NET Native.</span></span> <span data-ttu-id="1c015-147">Zapytania z typami anonimowymi mają gorszą wydajność na .NET Native.</span><span class="sxs-lookup"><span data-stu-id="1c015-147">Queries with anonymous types have worse performance on .NET Native.</span></span>

* <span data-ttu-id="1c015-148">Aby zoptymalizować wydajność `SaveChanges()`, użyj [ChangeTrackingStrategy. ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) i Implementuj [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx)i [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) w typach jednostek.</span><span class="sxs-lookup"><span data-stu-id="1c015-148">To optimize `SaveChanges()` performance, use [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) and implement [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx), and [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types.</span></span>

## <a name="report-issues"></a><span data-ttu-id="1c015-149">Zgłoś problemy</span><span class="sxs-lookup"><span data-stu-id="1c015-149">Report issues</span></span>

<span data-ttu-id="1c015-150">W przypadku dowolnej kombinacji, która nie działa zgodnie z oczekiwaniami, zachęcamy do tworzenia nowych problemów w ramach [EF Core śledzenia problemów](https://github.com/aspnet/entityframeworkcore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="1c015-150">For any combination that doesn�t work as expected, we encourage creating new issues on the [EF Core issue tracker](https://github.com/aspnet/entityframeworkcore/issues/new).</span></span> <span data-ttu-id="1c015-151">W przypadku problemów specyficznych dla platformy Xamarin Użyj narzędzia do śledzenia problemów dla platformy [Xamarin. Android](https://github.com/xamarin/xamarin-android/issues/new) lub [Xamarin. iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span><span class="sxs-lookup"><span data-stu-id="1c015-151">For Xamarin-specific issues use the issue tracker for [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) or [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span></span>
