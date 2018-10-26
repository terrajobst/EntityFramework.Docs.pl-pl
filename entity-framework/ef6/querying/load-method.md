---
title: Metoda Load - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: bcea8ab2477f44281cd5de824457a72a84ccc766
ms.sourcegitcommit: 4a795285004612ac03ab26532ac09ca333cb4c8f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123820"
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
        .Where(p => p.Tags.Contains("entity-framework"))
        .Load();
}
```  
