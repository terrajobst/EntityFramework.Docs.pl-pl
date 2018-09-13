---
title: Metoda Load - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: 3a0d11552b6bfd8b83f15c58c6cb9f945d9d4536
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490899"
---
# <a name="the-load-method"></a>Metoda Load
Istnieje kilka scenariuszy, w których możesz chcieć ładowanie jednostek z bazy danych do kontekstu, bez żadnego z tych jednostek natychmiast działania. Dobrym przykładem Trwa ładowanie jednostek dla wiązania danych, zgodnie z opisem w [dane lokalne](~/ef6/querying/local-data.md). Typowym sposobem to jest Napisz zapytanie LINQ, a następnie wywołać tolist — na nim tylko po to, aby od razu odrzucić utworzona lista. Metoda rozszerzenia obciążenia działa podobnie jak tolist — z wyjątkiem tego, aby całkowicie uniknąć tworzenia listy.  

Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.  

Poniżej przedstawiono dwa przykłady użycia obciążenia. Pierwszy pochodzi z aplikacji powiązanie danych formularzy Windows, których obciążenia jest używana do wykonywania zapytań dla jednostek przed powiązania do kolekcji lokalnej, zgodnie z opisem w [dane lokalne](~/ef6/querying/local-data.md):  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

W drugim przykładzie pokazano przy użyciu ładowania do ładowania filtrowanym kolekcji powiązanych jednostek, zgodnie z opisem w [ładowanie powiązanych jednostek](~/ef6/querying/related-data.md):  

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
