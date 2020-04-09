---
title: Omówienie struktury jednostek 6 — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 28a13879416a52cbe8035c23013f16390c75c4c9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416333"
---
# <a name="entity-framework-6"></a><span data-ttu-id="47748-102">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="47748-102">Entity Framework 6</span></span>
<span data-ttu-id="47748-103">Entity Framework 6 (EF6) jest wypróbowanym i przetestowanym maperem obiekto-relacyjne (O/RM) dla platformy .NET z wieloletnim rozwojem i stabilizacją funkcji.</span><span class="sxs-lookup"><span data-stu-id="47748-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="47748-104">Jako O/RM, EF6 zmniejsza niezgodność impedancji między światami relacyjnych i obiektowych, umożliwiając deweloperom pisanie aplikacji, które współdziałają z danymi przechowywanymi w relacyjnych bazach danych przy użyciu silnie typizowanych obiektów .NET, które reprezentują domenę aplikacji, i eliminując potrzebę dużej części kodu dostępu do danych "instalacyjnego", który zwykle muszą zapisywać.</span><span class="sxs-lookup"><span data-stu-id="47748-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="47748-105">EF6 implementuje wiele popularnych funkcji O/RM:</span><span class="sxs-lookup"><span data-stu-id="47748-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="47748-106">Mapowanie klas jednostek [POCO,](xref:ef6/resources/glossary#poco) które nie zależą od żadnych typów EF</span><span class="sxs-lookup"><span data-stu-id="47748-106">Mapping of [POCO](xref:ef6/resources/glossary#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="47748-107">Automatyczne śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="47748-107">Automatic change tracking</span></span>
- <span data-ttu-id="47748-108">Rozpoznawanie tożsamości i jednostka pracy</span><span class="sxs-lookup"><span data-stu-id="47748-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="47748-109">Chętne, leniwe i wyraźne ładowanie</span><span class="sxs-lookup"><span data-stu-id="47748-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="47748-110">Tłumaczenie silnie typizowanych zapytań przy użyciu [LINQ (Language INtegrated Query)](https://aka.ms/AA6hsvu)</span><span class="sxs-lookup"><span data-stu-id="47748-110">Translation of strongly-typed queries using [LINQ (Language INtegrated Query)](https://aka.ms/AA6hsvu)</span></span>
- <span data-ttu-id="47748-111">Rozbudowa funkcji mapowania, w tym obsługa:</span><span class="sxs-lookup"><span data-stu-id="47748-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="47748-112">Relacje jeden do jednego, jeden do wielu i wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="47748-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="47748-113">Dziedziczenie (tabela według hierarchii, tabela według typu i tabela na klasę betonową)</span><span class="sxs-lookup"><span data-stu-id="47748-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="47748-114">Typy złożone</span><span class="sxs-lookup"><span data-stu-id="47748-114">Complex types</span></span>
  - <span data-ttu-id="47748-115">Procedury składowane</span><span class="sxs-lookup"><span data-stu-id="47748-115">Stored procedures</span></span>
- <span data-ttu-id="47748-116">Projektant wizualizacji do tworzenia modeli jednostek.</span><span class="sxs-lookup"><span data-stu-id="47748-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="47748-117">Środowisko "Code First" do tworzenia modeli jednostek przez pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="47748-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="47748-118">Modele mogą być generowane z istniejących baz danych, a następnie ręcznie edytowane, lub mogą być tworzone od podstaw, a następnie używane do generowania nowych baz danych.</span><span class="sxs-lookup"><span data-stu-id="47748-118">Models can either be generated from existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="47748-119">Integracja z modelami aplikacji .NET Framework, w tym ASP.NET i za pośrednictwem powiązania danych, z WPF I WinForms.</span><span class="sxs-lookup"><span data-stu-id="47748-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="47748-120">Łączność bazy danych w oparciu o ADO.NET i wielu [dostawców](xref:ef6/fundamentals/providers/index) dostępnych do łączenia się z SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, itp.</span><span class="sxs-lookup"><span data-stu-id="47748-120">Database connectivity based on ADO.NET and numerous [providers](xref:ef6/fundamentals/providers/index) available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="47748-121">Czy należy używać EF6 lub EF Core?</span><span class="sxs-lookup"><span data-stu-id="47748-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="47748-122">EF Core to bardziej nowoczesna, lekka i rozszerzalna wersja entity framework, która ma bardzo podobne możliwości i korzyści do EF6.</span><span class="sxs-lookup"><span data-stu-id="47748-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="47748-123">EF Core jest kompletny przepisać i zawiera wiele nowych funkcji nie są dostępne w EF6, chociaż nadal brakuje niektórych z najbardziej zaawansowanych możliwości mapowania EF6.</span><span class="sxs-lookup"><span data-stu-id="47748-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="47748-124">Należy rozważyć użycie EF Core w nowych aplikacjach, jeśli zestaw funkcji spełnia twoje wymagania.</span><span class="sxs-lookup"><span data-stu-id="47748-124">Consider using EF Core in new applications if the feature set matches your requirements.</span></span>
<span data-ttu-id="47748-125">[Porównaj EF Core & EF6](xref:efcore-and-ef6/index) analizuje ten wybór bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="47748-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-started"></a>[<span data-ttu-id="47748-126">Rozpocząć</span><span class="sxs-lookup"><span data-stu-id="47748-126">Get Started</span></span>](xref:ef6/get-started)

<span data-ttu-id="47748-127">Dodaj pakiet EnFramework NuGet do projektu lub zainstaluj [narzędzia struktury encji dla programu Visual Studio](https://aka.ms/AA6i8c5).</span><span class="sxs-lookup"><span data-stu-id="47748-127">Add the EntityFramework NuGet package to your project or install the [Entity Framework Tools for Visual Studio](https://aka.ms/AA6i8c5).</span></span> <span data-ttu-id="47748-128">Następnie obejrzyj filmy, przeczytaj samouczki i zaawansowaną dokumentację, aby w pełni wykorzystać ef6.</span><span class="sxs-lookup"><span data-stu-id="47748-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="47748-129">Poprzednie wersje struktury encji</span><span class="sxs-lookup"><span data-stu-id="47748-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="47748-130">Jest to dokumentacja dla najnowszej wersji programu Entity Framework 6, chociaż wiele z nich dotyczy również poprzednich wersji.</span><span class="sxs-lookup"><span data-stu-id="47748-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="47748-131">Zapoznaj się [z informacjami o wersjach What's New](xref:ef6/what-is-new/index) i [Past, aby](xref:ef6/what-is-new/past-releases) uzyskać pełną listę wydań EF i wprowadzonych przez nich funkcji.</span><span class="sxs-lookup"><span data-stu-id="47748-131">Check out [What's New](xref:ef6/what-is-new/index) and [Past Releases](xref:ef6/what-is-new/past-releases) for a complete list of EF releases and the features they introduced.</span></span>
