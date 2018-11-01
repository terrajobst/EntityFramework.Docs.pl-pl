---
title: Rozpoczynanie pracy — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: c8d53b47d215c0db673c9058e9d78a7e2e7b895f
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250741"
---
# <a name="getting-started-with-entity-framework-core"></a><span data-ttu-id="f5bdd-102">Wprowadzenie do platformy Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f5bdd-102">Getting Started with Entity Framework Core</span></span>

## <a name="installing-ef-coreinstallindexmd"></a>[<span data-ttu-id="f5bdd-103">Instalowanie programu EF Core</span><span class="sxs-lookup"><span data-stu-id="f5bdd-103">Installing EF Core</span></span>](install/index.md)

<span data-ttu-id="f5bdd-104">Podsumowanie kroków niezbędnych do dodawania programu EF Core do aplikacji w różnych platformach oraz popularnych środowisk IDE.</span><span class="sxs-lookup"><span data-stu-id="f5bdd-104">A summary of the steps necessary to add EF Core to your application in different platforms and popular IDEs.</span></span>

## <a name="step-by-step-tutorials"></a><span data-ttu-id="f5bdd-105">Samouczki krok po kroku</span><span class="sxs-lookup"><span data-stu-id="f5bdd-105">Step-by-step Tutorials</span></span>

<span data-ttu-id="f5bdd-106">Przedstawione tu samouczki nie wymagają wcześniejszej wiedzy dotyczącej korzystania z programów Entity Framework Core ani konkretnego środowiska IDE.</span><span class="sxs-lookup"><span data-stu-id="f5bdd-106">These introductory tutorials require no previous knowledge of Entity Framework Core or a particular IDE.</span></span> <span data-ttu-id="f5bdd-107">Pozwolą na przejście krok po kroku przez proces tworzenia prostej aplikacji, która wykonuje kwerendę i zapisuje dane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f5bdd-107">They will take you step-by-step through creating a simple application that queries and saves data from a database.</span></span> <span data-ttu-id="f5bdd-108">Udostępniliśmy te samouczki, aby ułatwić rozpoczęcie pracy w różnych systemach operacyjnych i typach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f5bdd-108">We have provided tutorials to get you started on various operating systems and application types.</span></span>

<span data-ttu-id="f5bdd-109">Entity Framework Core może utworzyć model w oparciu o istniejącą bazę danych lub utworzyć bazę danych w oparciu o istniejący model.</span><span class="sxs-lookup"><span data-stu-id="f5bdd-109">Entity Framework Core can create a model based on an existing database, or create a database for you based on your model.</span></span> <span data-ttu-id="f5bdd-110">Dostępne są samouczki obejmujące oba te zastosowania.</span><span class="sxs-lookup"><span data-stu-id="f5bdd-110">There are tutorials that demonstrate both of these approaches.</span></span>

* <span data-ttu-id="f5bdd-111">.NET framework (Konsola aplikacji formularzy WinForms, WPF)</span><span class="sxs-lookup"><span data-stu-id="f5bdd-111">.NET Framework (Console apps, WinForms, WPF)</span></span>
  * [<span data-ttu-id="f5bdd-112">Nowa baza danych</span><span class="sxs-lookup"><span data-stu-id="f5bdd-112">New Database</span></span>](full-dotnet/new-db.md)
  * [<span data-ttu-id="f5bdd-113">Istniejąca baza danych</span><span class="sxs-lookup"><span data-stu-id="f5bdd-113">Existing Database</span></span>](full-dotnet/existing-db.md)
* <span data-ttu-id="f5bdd-114">.NET core (Windows, macOS i Linux)</span><span class="sxs-lookup"><span data-stu-id="f5bdd-114">.NET Core (Windows, macOS, Linux)</span></span>
  * [<span data-ttu-id="f5bdd-115">Nowa baza danych</span><span class="sxs-lookup"><span data-stu-id="f5bdd-115">New Database</span></span>](netcore/new-db-sqlite.md)
* <span data-ttu-id="f5bdd-116">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f5bdd-116">ASP.NET Core</span></span>
  * [<span data-ttu-id="f5bdd-117">Nowa baza danych</span><span class="sxs-lookup"><span data-stu-id="f5bdd-117">New Database</span></span>](aspnetcore/new-db.md)
  * [<span data-ttu-id="f5bdd-118">Istniejąca baza danych</span><span class="sxs-lookup"><span data-stu-id="f5bdd-118">Existing Database</span></span>](aspnetcore/existing-db.md)
  * [<span data-ttu-id="f5bdd-119">Produkty EF Core i Razor Pages</span><span class="sxs-lookup"><span data-stu-id="f5bdd-119">EF Core and Razor Pages</span></span>](/aspnet/core/data/ef-rp/intro)
* <span data-ttu-id="f5bdd-120">Platforma uniwersalna systemu Windows (UWP)</span><span class="sxs-lookup"><span data-stu-id="f5bdd-120">Universal Windows Platform (UWP)</span></span>
  * [<span data-ttu-id="f5bdd-121">Nowa baza danych</span><span class="sxs-lookup"><span data-stu-id="f5bdd-121">New Database</span></span>](uwp/getting-started.md)

> [!NOTE]  
> <span data-ttu-id="f5bdd-122">Samouczki i towarzyszące im przykłady zostały zaktualizowane pod kątem programu EF Core w wersji 2.1 .</span><span class="sxs-lookup"><span data-stu-id="f5bdd-122">These tutorials and the accompanying samples have been updated to use EF Core 2.1.</span></span> <span data-ttu-id="f5bdd-123">Jednak w większości przypadków, przy wprowadzeniu zaledwie minimalnych zmian w instrukcjach, powinno być możliwe utworzenie aplikacji zgodnych z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="f5bdd-123">However, in the majority of cases it should be possible to create applications that use previous releases, with minimal modification to the instructions.</span></span> 
