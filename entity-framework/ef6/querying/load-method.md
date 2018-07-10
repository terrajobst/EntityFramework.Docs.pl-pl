---
title: Metoda Load - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
caps.latest.revision: 3
ms.openlocfilehash: 83af79220b52de6e3063868fd9bdac56867d49cb
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912755"
---
# <a name="the-load-method"></a><span data-ttu-id="04bb0-102">Metoda Load</span><span class="sxs-lookup"><span data-stu-id="04bb0-102">The Load Method</span></span>
<span data-ttu-id="04bb0-103">Istnieje kilka scenariuszy, w których możesz chcieć ładowanie jednostek z bazy danych do kontekstu, bez żadnego z tych jednostek natychmiast działania.</span><span class="sxs-lookup"><span data-stu-id="04bb0-103">There are several scenarios where you may want to load entities from the database into the context without immediately doing anything with those entities.</span></span> <span data-ttu-id="04bb0-104">Dobrym przykładem Trwa ładowanie jednostek dla wiązania danych, zgodnie z opisem w [dane lokalne](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="04bb0-104">A good example of this is loading entities for data binding as described in [Local Data](~/ef6/querying/local-data.md).</span></span> <span data-ttu-id="04bb0-105">Typowym sposobem to jest Napisz zapytanie LINQ, a następnie wywołać tolist — na nim tylko po to, aby od razu odrzucić utworzona lista.</span><span class="sxs-lookup"><span data-stu-id="04bb0-105">One common way to do this is to write a LINQ query and then call ToList on it, only to immediately discard the created list.</span></span> <span data-ttu-id="04bb0-106">Metoda rozszerzenia obciążenia działa podobnie jak tolist — z wyjątkiem tego, aby całkowicie uniknąć tworzenia listy.</span><span class="sxs-lookup"><span data-stu-id="04bb0-106">The Load extension method works just like ToList except that it avoids the creation of the list altogether.</span></span>  

<span data-ttu-id="04bb0-107">Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="04bb0-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="04bb0-108">Poniżej przedstawiono dwa przykłady użycia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="04bb0-108">Here are two examples of using Load.</span></span> <span data-ttu-id="04bb0-109">Pierwszy pochodzi z aplikacji powiązanie danych formularzy Windows, których obciążenia jest używana do wykonywania zapytań dla jednostek przed powiązania do kolekcji lokalnej, zgodnie z opisem w [dane lokalne](~/ef6/querying/local-data.md):</span><span class="sxs-lookup"><span data-stu-id="04bb0-109">The first is taken from a Windows Forms data binding application where Load is used to query for entities before binding to the local collection, as described in [Local Data](~/ef6/querying/local-data.md):</span></span>  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

<span data-ttu-id="04bb0-110">W drugim przykładzie pokazano przy użyciu ładowania do ładowania filtrowanym kolekcji powiązanych jednostek, zgodnie z opisem w [ładowanie powiązanych jednostek](~/ef6/querying/related-data.md):</span><span class="sxs-lookup"><span data-stu-id="04bb0-110">The second example shows using Load to load a filtered collection of related entities, as described in [Loading Related Entities](~/ef6/querying/related-data.md):</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  
