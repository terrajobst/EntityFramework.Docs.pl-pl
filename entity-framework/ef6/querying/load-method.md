---
title: Metoda Load-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: bcea8ab2477f44281cd5de824457a72a84ccc766
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417124"
---
# <a name="the-load-method"></a><span data-ttu-id="24983-102">Metoda Load</span><span class="sxs-lookup"><span data-stu-id="24983-102">The Load Method</span></span>
<span data-ttu-id="24983-103">Istnieje kilka scenariuszy, w których może zaistnieć potrzeba załadowania jednostek z bazy danych do kontekstu bez natychmiastowego wykonywania jakichkolwiek czynności z tymi jednostkami.</span><span class="sxs-lookup"><span data-stu-id="24983-103">There are several scenarios where you may want to load entities from the database into the context without immediately doing anything with those entities.</span></span> <span data-ttu-id="24983-104">Dobrym przykładem jest ładowanie jednostek dla powiązania danych zgodnie z opisem w [danych lokalnych](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="24983-104">A good example of this is loading entities for data binding as described in [Local Data](~/ef6/querying/local-data.md).</span></span> <span data-ttu-id="24983-105">Typowym sposobem wykonania tej czynności jest zapisanie zapytania LINQ, a następnie Wywołaj ToList — na nim, aby natychmiast odrzucić utworzoną listę.</span><span class="sxs-lookup"><span data-stu-id="24983-105">One common way to do this is to write a LINQ query and then call ToList on it, only to immediately discard the created list.</span></span> <span data-ttu-id="24983-106">Metoda ładowania rozszerzenia działa podobnie jak ToList —, z tą różnicą, że nie pozwala ona na całkowite utworzenie listy.</span><span class="sxs-lookup"><span data-stu-id="24983-106">The Load extension method works just like ToList except that it avoids the creation of the list altogether.</span></span>  

<span data-ttu-id="24983-107">Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="24983-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="24983-108">Poniżej przedstawiono dwa przykłady użycia ładowania.</span><span class="sxs-lookup"><span data-stu-id="24983-108">Here are two examples of using Load.</span></span> <span data-ttu-id="24983-109">Pierwszy jest pobierany z Windows Forms aplikacji do powiązania danych, w której obciążenie jest używane do wykonywania zapytań dotyczących jednostek przed powiązaniem z kolekcją lokalną, zgodnie z opisem w temacie [dane lokalne](~/ef6/querying/local-data.md):</span><span class="sxs-lookup"><span data-stu-id="24983-109">The first is taken from a Windows Forms data binding application where Load is used to query for entities before binding to the local collection, as described in [Local Data](~/ef6/querying/local-data.md):</span></span>  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

<span data-ttu-id="24983-110">Drugi przykład pokazuje użycie obciążenia do załadowania filtrowanej kolekcji pokrewnych jednostek, zgodnie z opisem w temacie [ładowanie powiązanych jednostek](~/ef6/querying/related-data.md):</span><span class="sxs-lookup"><span data-stu-id="24983-110">The second example shows using Load to load a filtered collection of related entities, as described in [Loading Related Entities](~/ef6/querying/related-data.md):</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework"))
        .Load();
}
```  
