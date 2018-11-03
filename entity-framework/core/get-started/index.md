---
title: Rozpoczynanie pracy — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: 744ea587207775f3a5b9f7b14ba5959c55539c13
ms.sourcegitcommit: 39080d38e1adea90db741257e60dc0e7ed08aa82
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980018"
---
# <a name="getting-started-with-entity-framework-core"></a><span data-ttu-id="a8f6d-102">Wprowadzenie do platformy Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="a8f6d-102">Getting Started with Entity Framework Core</span></span>

## <a name="installing-ef-coreinstallindexmd"></a>[<span data-ttu-id="a8f6d-103">Instalowanie programu EF Core</span><span class="sxs-lookup"><span data-stu-id="a8f6d-103">Installing EF Core</span></span>](install/index.md)

<span data-ttu-id="a8f6d-104">Podsumowanie kroków niezbędnych do dodawania programu EF Core do aplikacji w różnych platformach oraz popularnych środowisk IDE.</span><span class="sxs-lookup"><span data-stu-id="a8f6d-104">A summary of the steps necessary to add EF Core to your application in different platforms and popular IDEs.</span></span>

## <a name="step-by-step-tutorials"></a><span data-ttu-id="a8f6d-105">Samouczki krok po kroku</span><span class="sxs-lookup"><span data-stu-id="a8f6d-105">Step-by-step Tutorials</span></span>

<span data-ttu-id="a8f6d-106">Te wprowadzających samouczków wymagają nie wcześniejsza wiedza Entity Framework Core lub dowolnym określonym środowisku IDE.</span><span class="sxs-lookup"><span data-stu-id="a8f6d-106">These introductory tutorials require no previous knowledge of Entity Framework Core or any particular IDE.</span></span> <span data-ttu-id="a8f6d-107">One spowoduje przejście krok po kroku proces tworzenia prostej aplikacji, który wykonuje kwerendę i zapisuje dane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a8f6d-107">They will take you step-by-step through the creation of a simple application that queries and saves data from a database.</span></span> <span data-ttu-id="a8f6d-108">Udostępniliśmy te samouczki, aby ułatwić rozpoczęcie pracy w różnych systemach operacyjnych i typach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a8f6d-108">We have provided tutorials to get you started on various operating systems and application types.</span></span>

<span data-ttu-id="a8f6d-109">Entity Framework Core może utworzyć model w oparciu o istniejącą bazę danych lub utworzyć bazę danych w oparciu o istniejący model.</span><span class="sxs-lookup"><span data-stu-id="a8f6d-109">Entity Framework Core can create a model based on an existing database, or create a database for you based on your model.</span></span> <span data-ttu-id="a8f6d-110">Dostępne są samouczki obejmujące oba te zastosowania.</span><span class="sxs-lookup"><span data-stu-id="a8f6d-110">There are tutorials that demonstrate both of these approaches.</span></span>

* <span data-ttu-id="a8f6d-111">.NET framework (Konsola aplikacji formularzy WinForms, WPF)</span><span class="sxs-lookup"><span data-stu-id="a8f6d-111">.NET Framework (Console apps, WinForms, WPF)</span></span>
  * [<span data-ttu-id="a8f6d-112">Nowa baza danych</span><span class="sxs-lookup"><span data-stu-id="a8f6d-112">New Database</span></span>](full-dotnet/new-db.md)
  * [<span data-ttu-id="a8f6d-113">Istniejąca baza danych</span><span class="sxs-lookup"><span data-stu-id="a8f6d-113">Existing Database</span></span>](full-dotnet/existing-db.md)
* <span data-ttu-id="a8f6d-114">.NET core (Windows, macOS i Linux)</span><span class="sxs-lookup"><span data-stu-id="a8f6d-114">.NET Core (Windows, macOS, Linux)</span></span>
  * [<span data-ttu-id="a8f6d-115">Nowa baza danych</span><span class="sxs-lookup"><span data-stu-id="a8f6d-115">New Database</span></span>](netcore/new-db-sqlite.md)
* <span data-ttu-id="a8f6d-116">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8f6d-116">ASP.NET Core</span></span>
  * [<span data-ttu-id="a8f6d-117">Nowa baza danych</span><span class="sxs-lookup"><span data-stu-id="a8f6d-117">New Database</span></span>](aspnetcore/new-db.md)
  * [<span data-ttu-id="a8f6d-118">Istniejąca baza danych</span><span class="sxs-lookup"><span data-stu-id="a8f6d-118">Existing Database</span></span>](aspnetcore/existing-db.md)
  * [<span data-ttu-id="a8f6d-119">Produkty EF Core i Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a8f6d-119">EF Core and Razor Pages</span></span>](/aspnet/core/data/ef-rp/intro)
* <span data-ttu-id="a8f6d-120">Platforma uniwersalna systemu Windows (UWP)</span><span class="sxs-lookup"><span data-stu-id="a8f6d-120">Universal Windows Platform (UWP)</span></span>
  * [<span data-ttu-id="a8f6d-121">Nowa baza danych</span><span class="sxs-lookup"><span data-stu-id="a8f6d-121">New Database</span></span>](uwp/getting-started.md)

> [!NOTE]  
> <span data-ttu-id="a8f6d-122">Samouczki i towarzyszące im przykłady zostały zaktualizowane pod kątem programu EF Core w wersji 2.1 .</span><span class="sxs-lookup"><span data-stu-id="a8f6d-122">These tutorials and the accompanying samples have been updated to use EF Core 2.1.</span></span> <span data-ttu-id="a8f6d-123">Jednak w większości przypadków, przy wprowadzeniu zaledwie minimalnych zmian w instrukcjach, powinno być możliwe utworzenie aplikacji zgodnych z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="a8f6d-123">However, in the majority of cases it should be possible to create applications that use previous releases, with minimal modification to the instructions.</span></span> 
