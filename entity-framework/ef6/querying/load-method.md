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
# <a name="the-load-method"></a>Metoda Load
Istnieje kilka scenariuszy, w których może zaistnieć potrzeba załadowania jednostek z bazy danych do kontekstu bez natychmiastowego wykonywania jakichkolwiek czynności z tymi jednostkami. Dobrym przykładem jest ładowanie jednostek dla powiązania danych zgodnie z opisem w [danych lokalnych](~/ef6/querying/local-data.md). Typowym sposobem wykonania tej czynności jest zapisanie zapytania LINQ, a następnie Wywołaj ToList — na nim, aby natychmiast odrzucić utworzoną listę. Metoda ładowania rozszerzenia działa podobnie jak ToList —, z tą różnicą, że nie pozwala ona na całkowite utworzenie listy.  

Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.  

Poniżej przedstawiono dwa przykłady użycia ładowania. Pierwszy jest pobierany z Windows Forms aplikacji do powiązania danych, w której obciążenie jest używane do wykonywania zapytań dotyczących jednostek przed powiązaniem z kolekcją lokalną, zgodnie z opisem w temacie [dane lokalne](~/ef6/querying/local-data.md):  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

Drugi przykład pokazuje użycie obciążenia do załadowania filtrowanej kolekcji pokrewnych jednostek, zgodnie z opisem w temacie [ładowanie powiązanych jednostek](~/ef6/querying/related-data.md):  

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
