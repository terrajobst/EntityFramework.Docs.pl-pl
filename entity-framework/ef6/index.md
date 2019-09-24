---
title: Przegląd Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: fcd514eebbf09e50403b95c88db04c33520011e9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198059"
---
# <a name="entity-framework-6"></a><span data-ttu-id="abdfc-102">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="abdfc-102">Entity Framework 6</span></span>
<span data-ttu-id="abdfc-103">Entity Framework 6 (EF6) to próba i przetestowana funkcja mapowania obiektów relacyjnych (O/RM) dla platformy .NET z wieloma latami tworzenia i stabilizacji funkcji.</span><span class="sxs-lookup"><span data-stu-id="abdfc-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="abdfc-104">Jako O/RM EF6 zmniejszają niezgodność zależności między światowymi i zorientowanymi na obiektach, dzięki czemu deweloperzy mogą pisać aplikacje, które współdziałają z danymi przechowywanymi w relacyjnych bazach danych przy użyciu obiektów .NET o jednoznacznie określonym typie, które reprezentują domena aplikacji i eliminuje konieczność użycia dużej części kodu "wodociąging" do uzyskiwania dostępu do danych, które zwykle wymagają zapisu.</span><span class="sxs-lookup"><span data-stu-id="abdfc-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="abdfc-105">EF6 implementuje wiele popularnych funkcji O/RM:</span><span class="sxs-lookup"><span data-stu-id="abdfc-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="abdfc-106">Mapowanie klas jednostek [poco](~/ef6/resources/glossary.md#poco) , które nie są zależne od żadnych typów EF</span><span class="sxs-lookup"><span data-stu-id="abdfc-106">Mapping of [POCO](~/ef6/resources/glossary.md#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="abdfc-107">Automatyczne śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="abdfc-107">Automatic change tracking</span></span>
- <span data-ttu-id="abdfc-108">Rozpoznawanie tożsamości i jednostka pracy</span><span class="sxs-lookup"><span data-stu-id="abdfc-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="abdfc-109">Eager, opóźnienie i jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="abdfc-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="abdfc-110">Tłumaczenie zapytań o jednoznacznie określonym typie przy użyciu LINQ (zapytanie w języku INtegrated Language)</span><span class="sxs-lookup"><span data-stu-id="abdfc-110">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span>
- <span data-ttu-id="abdfc-111">Bogate możliwości mapowania, w tym obsługa:</span><span class="sxs-lookup"><span data-stu-id="abdfc-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="abdfc-112">Relacje jeden-do-jednego, jeden-do-wielu i wiele-do-wielu</span><span class="sxs-lookup"><span data-stu-id="abdfc-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="abdfc-113">Dziedziczenie (tabela na hierarchię, tabela na typ i tabela dla konkretnej klasy)</span><span class="sxs-lookup"><span data-stu-id="abdfc-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="abdfc-114">Typy złożone</span><span class="sxs-lookup"><span data-stu-id="abdfc-114">Complex types</span></span>
  - <span data-ttu-id="abdfc-115">Procedury składowane</span><span class="sxs-lookup"><span data-stu-id="abdfc-115">Stored procedures</span></span>
- <span data-ttu-id="abdfc-116">Projektant wizualny służący do tworzenia modeli jednostek.</span><span class="sxs-lookup"><span data-stu-id="abdfc-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="abdfc-117">Środowisko "Code First" do tworzenia modeli jednostek przez napisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="abdfc-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="abdfc-118">Modele można generować z istniejących baz danych, a następnie edytować je ręcznie lub tworzyć od podstaw, a następnie używać do generowania nowych baz danych.</span><span class="sxs-lookup"><span data-stu-id="abdfc-118">Models can either be generated from existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="abdfc-119">Integracja z .NET Framework modelami aplikacji, w tym ASP.NET, i za pomocą powiązań danych z WPF i WinForms.</span><span class="sxs-lookup"><span data-stu-id="abdfc-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="abdfc-120">Łączność z bazą danych oparta na ADO.NET i wielu dostawcach dostępnych do łączenia się z SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 itp.</span><span class="sxs-lookup"><span data-stu-id="abdfc-120">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="abdfc-121">Czy należy używać EF6 czy EF Core?</span><span class="sxs-lookup"><span data-stu-id="abdfc-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="abdfc-122">EF Core to bardziej nowoczesny, lekki i rozszerzalny wersja Entity Framework, która ma bardzo podobne możliwości i korzyści EF6.</span><span class="sxs-lookup"><span data-stu-id="abdfc-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="abdfc-123">EF Core to pełny ponowny zapis i zawiera wiele nowych funkcji, które nie są dostępne w EF6, chociaż nadal nie ma niektórych najbardziej zaawansowanych funkcji mapowania EF6.</span><span class="sxs-lookup"><span data-stu-id="abdfc-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="abdfc-124">Jeśli zestaw funkcji spełnia Twoje wymagania, należy rozważyć użycie EF Core w nowych aplikacjach.</span><span class="sxs-lookup"><span data-stu-id="abdfc-124">Consider using EF Core in new applications if the feature set matches your requirements.</span></span>
<span data-ttu-id="abdfc-125">[Porównaj EF Core &AMP; Ef6](xref:efcore-and-ef6/index) sprawdza ten wybór bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="abdfc-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="abdfc-126">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="abdfc-126">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="abdfc-127">Dodaj pakiet NuGet EntityFramework do projektu lub zainstaluj Entity Framework Tools dla programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="abdfc-127">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="abdfc-128">Następnie Obejrzyj filmy wideo, samouczki odczytywania oraz zaawansowaną dokumentację, aby ułatwić EF6.</span><span class="sxs-lookup"><span data-stu-id="abdfc-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="abdfc-129">Wcześniejsze wersje Entity Framework</span><span class="sxs-lookup"><span data-stu-id="abdfc-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="abdfc-130">Jest to dokumentacja dotycząca najnowszej wersji programu Entity Framework 6, chociaż większość z nich ma zastosowanie również do poprzednich wersji.</span><span class="sxs-lookup"><span data-stu-id="abdfc-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="abdfc-131">Zapoznaj się z [nowościami](~/ef6/what-is-new/index.md) i [poprzednimi wersjami](~/ef6/what-is-new/past-releases.md) , aby zapoznać się z pełną listą wydań EF i wprowadzonych przez nie funkcji.</span><span class="sxs-lookup"><span data-stu-id="abdfc-131">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
