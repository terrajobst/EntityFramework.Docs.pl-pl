---
title: Przegląd Entity Framework 6 — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 28a13879416a52cbe8035c23013f16390c75c4c9
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416333"
---
# <a name="entity-framework-6"></a><span data-ttu-id="ce3cc-102">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="ce3cc-102">Entity Framework 6</span></span>
<span data-ttu-id="ce3cc-103">Entity Framework 6 (EF6) to próba i przetestowana funkcja mapowania obiektów relacyjnych (O/RM) dla platformy .NET z wieloma latami tworzenia i stabilizacji funkcji.</span><span class="sxs-lookup"><span data-stu-id="ce3cc-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="ce3cc-104">Jako O/RM EF6 zmniejszają niezgodność między działami relacyjnymi i zorientowanymi na obiekty, dzięki czemu deweloperzy mogą pisać aplikacje, które współdziałają z danymi przechowywanymi w relacyjnych bazach danych przy użyciu obiektów .NET o jednoznacznie określonym typie, które reprezentują domenę aplikacji, i eliminując konieczność korzystania z dużej części kodu "wodociąging" z dostępem do danych, które zwykle wymagają zapisu.</span><span class="sxs-lookup"><span data-stu-id="ce3cc-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="ce3cc-105">EF6 implementuje wiele popularnych funkcji O/RM:</span><span class="sxs-lookup"><span data-stu-id="ce3cc-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="ce3cc-106">Mapowanie klas jednostek [poco](xref:ef6/resources/glossary#poco) , które nie są zależne od żadnych typów EF</span><span class="sxs-lookup"><span data-stu-id="ce3cc-106">Mapping of [POCO](xref:ef6/resources/glossary#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="ce3cc-107">Automatyczne śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="ce3cc-107">Automatic change tracking</span></span>
- <span data-ttu-id="ce3cc-108">Rozpoznawanie tożsamości i jednostka pracy</span><span class="sxs-lookup"><span data-stu-id="ce3cc-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="ce3cc-109">Eager, opóźnienie i jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="ce3cc-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="ce3cc-110">Tłumaczenie zapytań o jednoznacznie określonym typie przy użyciu [LINQ (zapytanie w języku INtegrated Language)](https://aka.ms/AA6hsvu)</span><span class="sxs-lookup"><span data-stu-id="ce3cc-110">Translation of strongly-typed queries using [LINQ (Language INtegrated Query)](https://aka.ms/AA6hsvu)</span></span>
- <span data-ttu-id="ce3cc-111">Bogate możliwości mapowania, w tym obsługa:</span><span class="sxs-lookup"><span data-stu-id="ce3cc-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="ce3cc-112">Relacje jeden-do-jednego, jeden-do-wielu i wiele-do-wielu</span><span class="sxs-lookup"><span data-stu-id="ce3cc-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="ce3cc-113">Dziedziczenie (tabela na hierarchię, tabela na typ i tabela dla konkretnej klasy)</span><span class="sxs-lookup"><span data-stu-id="ce3cc-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="ce3cc-114">Typy złożone</span><span class="sxs-lookup"><span data-stu-id="ce3cc-114">Complex types</span></span>
  - <span data-ttu-id="ce3cc-115">Procedury składowane</span><span class="sxs-lookup"><span data-stu-id="ce3cc-115">Stored procedures</span></span>
- <span data-ttu-id="ce3cc-116">Projektant wizualny służący do tworzenia modeli jednostek.</span><span class="sxs-lookup"><span data-stu-id="ce3cc-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="ce3cc-117">Środowisko "Code First" do tworzenia modeli jednostek przez napisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="ce3cc-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="ce3cc-118">Modele można generować z istniejących baz danych, a następnie edytować je ręcznie lub tworzyć od podstaw, a następnie używać do generowania nowych baz danych.</span><span class="sxs-lookup"><span data-stu-id="ce3cc-118">Models can either be generated from existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="ce3cc-119">Integracja z .NET Framework modelami aplikacji, w tym ASP.NET, i za pomocą powiązań danych z WPF i WinForms.</span><span class="sxs-lookup"><span data-stu-id="ce3cc-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="ce3cc-120">Łączność z bazą danych oparta na ADO.NET i wielu [dostawcach](xref:ef6/fundamentals/providers/index) dostępnych do łączenia się z SQL Server, Oracle, MySQL, SQLite, POSTGRESQL, DB2 itp.</span><span class="sxs-lookup"><span data-stu-id="ce3cc-120">Database connectivity based on ADO.NET and numerous [providers](xref:ef6/fundamentals/providers/index) available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="ce3cc-121">Czy należy używać EF6 czy EF Core?</span><span class="sxs-lookup"><span data-stu-id="ce3cc-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="ce3cc-122">EF Core to bardziej nowoczesny, lekki i rozszerzalny wersja Entity Framework, która ma bardzo podobne możliwości i korzyści EF6.</span><span class="sxs-lookup"><span data-stu-id="ce3cc-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="ce3cc-123">EF Core to pełny ponowny zapis i zawiera wiele nowych funkcji, które nie są dostępne w EF6, chociaż nadal nie ma niektórych najbardziej zaawansowanych funkcji mapowania EF6.</span><span class="sxs-lookup"><span data-stu-id="ce3cc-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="ce3cc-124">Jeśli zestaw funkcji spełnia Twoje wymagania, należy rozważyć użycie EF Core w nowych aplikacjach.</span><span class="sxs-lookup"><span data-stu-id="ce3cc-124">Consider using EF Core in new applications if the feature set matches your requirements.</span></span>
<span data-ttu-id="ce3cc-125">[Porównaj EF Core &AMP; Ef6](xref:efcore-and-ef6/index) sprawdza ten wybór bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="ce3cc-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-started"></a>[<span data-ttu-id="ce3cc-126">Rozpoczęcie pracy</span><span class="sxs-lookup"><span data-stu-id="ce3cc-126">Get Started</span></span>](xref:ef6/get-started)

<span data-ttu-id="ce3cc-127">Dodaj pakiet NuGet EntityFramework do projektu lub zainstaluj [Entity Framework Tools dla programu Visual Studio](https://aka.ms/AA6i8c5).</span><span class="sxs-lookup"><span data-stu-id="ce3cc-127">Add the EntityFramework NuGet package to your project or install the [Entity Framework Tools for Visual Studio](https://aka.ms/AA6i8c5).</span></span> <span data-ttu-id="ce3cc-128">Następnie Obejrzyj filmy wideo, samouczki odczytywania oraz zaawansowaną dokumentację, aby ułatwić EF6.</span><span class="sxs-lookup"><span data-stu-id="ce3cc-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="ce3cc-129">Wcześniejsze wersje Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ce3cc-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="ce3cc-130">Jest to dokumentacja dotycząca najnowszej wersji programu Entity Framework 6, chociaż większość z nich ma zastosowanie również do poprzednich wersji.</span><span class="sxs-lookup"><span data-stu-id="ce3cc-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="ce3cc-131">Zapoznaj się z [nowościami](xref:ef6/what-is-new/index) i [poprzednimi wersjami](xref:ef6/what-is-new/past-releases) , aby zapoznać się z pełną listą wydań EF i wprowadzonych przez nie funkcji.</span><span class="sxs-lookup"><span data-stu-id="ce3cc-131">Check out [What's New](xref:ef6/what-is-new/index) and [Past Releases](xref:ef6/what-is-new/past-releases) for a complete list of EF releases and the features they introduced.</span></span>
