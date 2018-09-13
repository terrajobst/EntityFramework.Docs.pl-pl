---
title: Przegląd — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 35292d2a724d02ecdc96b53550858eb0b6dd79d1
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489235"
---
# <a name="entity-framework-6"></a><span data-ttu-id="7acea-102">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="7acea-102">Entity Framework 6</span></span>
<span data-ttu-id="7acea-103">Entity Framework 6 (EF6) jest sprawdzonych obiektowo relacyjny mapowania (O/RM) dla platformy .NET przy użyciu wielu lat pracy nad rozwój funkcji i stabilizacją.</span><span class="sxs-lookup"><span data-stu-id="7acea-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="7acea-104">Jako Obiektowo, EF6 zmniejsza niezgodności impedancji między rozwiązań relacyjnych i zorientowane obiektowo, dzięki czemu deweloperzy mogą pisać aplikacje, które współdziałają z danymi przechowywanymi w relacyjnej bazy danych za pomocą silnie typizowanych obiektów platformy .NET, które reprezentują Aplikacja firmy domeny i eliminując potrzebę dużą część zazwyczaj wymaga, aby napisać kod "instalację" dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="7acea-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="7acea-105">EF6 implementuje wiele popularnych funkcji Obiektowo:</span><span class="sxs-lookup"><span data-stu-id="7acea-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="7acea-106">Mapowanie [POCO](~/ef6/resources/glossary.md#poco) klas jednostek, które nie są zależne od żadnych typów EF</span><span class="sxs-lookup"><span data-stu-id="7acea-106">Mapping of [POCO](~/ef6/resources/glossary.md#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="7acea-107">Automatyczne śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="7acea-107">Automatic change tracking</span></span>
- <span data-ttu-id="7acea-108">Rozwiązanie tożsamości i jednostki pracy</span><span class="sxs-lookup"><span data-stu-id="7acea-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="7acea-109">Eager, opóźnieniem i jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="7acea-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="7acea-110">Tłumaczenie silnie typizowane zapytań za pomocą LINQ (Language INtegrated Query)</span><span class="sxs-lookup"><span data-stu-id="7acea-110">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span>
- <span data-ttu-id="7acea-111">Obsługa zaawansowanych możliwości mapowania, w tym:</span><span class="sxs-lookup"><span data-stu-id="7acea-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="7acea-112">Relacje jeden do jednego, jeden do wielu i wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="7acea-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="7acea-113">Dziedziczenie (Tabela w danej hierarchii, tabelę według typu, a tabela na konkretnej klasy)</span><span class="sxs-lookup"><span data-stu-id="7acea-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="7acea-114">Typy złożone</span><span class="sxs-lookup"><span data-stu-id="7acea-114">Complex types</span></span>
  - <span data-ttu-id="7acea-115">Procedury składowane</span><span class="sxs-lookup"><span data-stu-id="7acea-115">Stored procedures</span></span>
- <span data-ttu-id="7acea-116">Projektant wizualny pozwala tworzyć modele jednostki.</span><span class="sxs-lookup"><span data-stu-id="7acea-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="7acea-117">"Code First" środowisko do tworzenia modeli entity przez napisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="7acea-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="7acea-118">Modele może być wygenerowany z istniejących baz danych, a następnie ręcznie modyfikować lub można je tworzyć od podstaw i następnie używany do generowania nowych baz danych.</span><span class="sxs-lookup"><span data-stu-id="7acea-118">Models can either be generated from existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="7acea-119">Integracja z modeli aplikacji .NET Framework, w tym ASP.NET i za pomocą wiązania danych oraz platforma WPF i WinForms.</span><span class="sxs-lookup"><span data-stu-id="7acea-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="7acea-120">Łączność z bazą danych na podstawie ADO.NET i wielu dostawców można podłączyć do programu SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 itd.</span><span class="sxs-lookup"><span data-stu-id="7acea-120">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="7acea-121">Należy użyć EF6 i EF Core?</span><span class="sxs-lookup"><span data-stu-id="7acea-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="7acea-122">EF Core jest bardziej nowoczesny, uproszczone i rozszerzalny wersję platformy Entity Framework jest bardzo podobne funkcje i korzyści z platformy EF6.</span><span class="sxs-lookup"><span data-stu-id="7acea-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="7acea-123">EF Core jest pełne ponowne zapisywanie adresów i zawiera wiele nowych funkcji nie jest dostępna w EF6, mimo że nadal brakuje niektórych najbardziej zaawansowanych możliwości mapowania EF6.</span><span class="sxs-lookup"><span data-stu-id="7acea-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="7acea-124">Firma Microsoft zaleca korzystanie z programu EF Core w nowej aplikacji, tak długo, jak zestaw funkcji pasuje do wymagań.</span><span class="sxs-lookup"><span data-stu-id="7acea-124">We recommend using EF Core in new applications as long as the feature set matches your requirements.</span></span>
<span data-ttu-id="7acea-125">[Porównanie programów EF Core i EF6](xref:efcore-and-ef6/index) sprawdza, czy ten wybór bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="7acea-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="7acea-126">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="7acea-126">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="7acea-127">Dodaj pakiet NuGet platformy EntityFramework do projektu lub instalowanie narzędzi Entity Framework Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7acea-127">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="7acea-128">Następnie Obejrzyj wideo, odczytu, samouczki i zaawansowane dokumentację, aby pomóc Państwu jak najlepiej wykorzystać możliwości platformy EF6.</span><span class="sxs-lookup"><span data-stu-id="7acea-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="7acea-129">Wcześniejsze wersje programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="7acea-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="7acea-130">Jest to dokumentację dla najnowszej wersji programu Entity Framework 6, ale część informacji ma zastosowanie również do poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="7acea-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="7acea-131">Zapoznaj się z [What's New](~/ef6/what-is-new/index.md) i [wydania w ciągu ostatnich](~/ef6/what-is-new/past-releases.md) pełną listę wydań EF i funkcje one wprowadzone.</span><span class="sxs-lookup"><span data-stu-id="7acea-131">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
