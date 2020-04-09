---
title: Wykonywanie zapytań i znajdowanie jednostek — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417169"
---
# <a name="querying-and-finding-entities"></a>Wykonywanie zapytań i znajdowanie encji
W tym temacie opisano różne sposoby wykonywania zapytań o dane przy użyciu entity framework, w tym LINQ i Find metody. Techniki pokazane w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i EF Designer.  

## <a name="finding-entities-using-a-query"></a>Znajdowanie encji przy użyciu kwerendy  

DbSet i IDbSet implementują IQueryable i dlatego mogą być używane jako punkt wyjścia do pisania zapytania LINQ w bazie danych. Nie jest to odpowiednie miejsce do dogłębnej dyskusji linq, ale oto kilka prostych przykładów:  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs with names starting with B
    var blogs = from b in context.Blogs
                   where b.Name.StartsWith("B")
                   select b;

    // Query for the Blog named ADO.NET Blog
    var blog = context.Blogs
                    .Where(b => b.Name == "ADO.NET Blog")
                    .FirstOrDefault();
}
```  

Należy zauważyć, że DbSet i IDbSet zawsze tworzyć zapytania względem bazy danych i zawsze będzie obejmować podróż w obie strony do bazy danych, nawet jeśli zwrócone jednostki już istnieją w kontekście. Kwerenda jest wykonywana względem bazy danych, gdy:  

- Jest wyliczona przez **foreach** (C#) lub **for Each** (Visual Basic) instrukcji.  
- Jest wyliczona przez operację zbierania, taką jak [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary)lub [ToList](https://msdn.microsoft.com/library/bb342261).  
- Linq operatorów, takich jak [First](https://msdn.microsoft.com/library/bb291976) lub [Any](https://msdn.microsoft.com/library/bb337697) są określone w najbardziej zewnętrznej części kwerendy.  
- Następujące metody są wywoływane: [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) extension method on a DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)i Database.ExecuteSqlCommand.  

Gdy wyniki są zwracane z bazy danych, obiekty, które nie istnieją w kontekście są dołączone do kontekstu. Jeśli obiekt jest już w kontekście, zwracany jest istniejący obiekt (bieżące i oryginalne wartości właściwości obiektu we wpisie **nie** są zastępowane wartościami bazy danych).  

Podczas wykonywania kwerendy jednostki, które zostały dodane do kontekstu, ale nie zostały jeszcze zapisane w bazie danych, nie są zwracane jako część zestawu wyników. Aby uzyskać dane, które są w kontekście, zobacz [Dane lokalne](~/ef6/querying/local-data.md).  

Jeśli kwerenda zwraca żadnych wierszy z bazy danych, wynik będzie pustą kolekcją, a nie **null**.  

## <a name="finding-entities-using-primary-keys"></a>Znajdowanie encji przy użyciu kluczy podstawowych  

Find Metoda na DbSet używa wartości klucza podstawowego, aby spróbować znaleźć jednostkę śledzone przez kontekst. Jeśli jednostka nie zostanie znaleziona w kontekście, kwerenda zostanie wysłana do bazy danych w celu znalezienia tej jednostki. Null jest zwracany, jeśli jednostka nie zostanie znaleziona w kontekście lub w bazie danych.  

Znajdź różni się od używania kwerendy na dwa istotne sposoby:  

- Podróż w obie strony do bazy danych zostanie dokonana tylko wtedy, gdy jednostka z danym kluczem nie zostanie znaleziona w kontekście.  
- Znajdź zwróci jednostki, które są w stanie Dodane. Oznacza to, że Find zwróci jednostki, które zostały dodane do kontekstu, ale nie zostały jeszcze zapisane w bazie danych.  
### <a name="finding-an-entity-by-primary-key"></a>Znajdowanie jednostki według klucza podstawowego  

Poniższy kod pokazuje niektóre zastosowania funkcji Znajdź:  

``` csharp
using (var context = new BloggingContext())
{
    // Will hit the database
    var blog = context.Blogs.Find(3);

    // Will return the same instance without hitting the database
    var blogAgain = context.Blogs.Find(3);

    context.Blogs.Add(new Blog { Id = -1 });

    // Will find the new blog even though it does not exist in the database
    var newBlog = context.Blogs.Find(-1);

    // Will find a User which has a string primary key
    var user = context.Users.Find("johndoe1987");
}
```  

### <a name="finding-an-entity-by-composite-primary-key"></a>Znajdowanie jednostki według złożonego klucza podstawowego  

Entity Framework umożliwia jednostek mają klucze złożone — to klucz, który składa się z więcej niż jednej właściwości. Na przykład można mieć BlogSettings jednostki, która reprezentuje ustawienia użytkowników dla określonego bloga. Ponieważ użytkownik będzie tylko kiedykolwiek jeden BlogSettings dla każdego bloga można wybrać, aby podstawowy klucz BlogSettings połączenie BlogId i nazwa użytkownika. Następujący kod próbuje znaleźć BlogSettings z BlogId = 3 i Nazwa użytkownika = "johndoe1987":  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

Należy zauważyć, że gdy masz klucze złożone, należy użyć ColumnAttribute lub płynnego interfejsu API, aby określić kolejność właściwości klucza złożonego. Wywołanie Find musi używać tej kolejności podczas określania wartości, które tworzą klucz.  
