---
title: Wykonywanie zapytań i wyszukiwanie jednostek - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 19e70bc5bcfdd0c81186c6139661395ebb1ee61f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993284"
---
# <a name="querying-and-finding-entities"></a>Wykonywanie zapytań i wyszukiwanie jednostek
W tym temacie omówiono różne sposoby, które można wyszukiwać dane przy użyciu platformy Entity Framework, w tym LINQ i metody Find. Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.  

## <a name="finding-entities-using-a-query"></a>Wyszukiwanie jednostek przy użyciu zapytania  

DbSet IDbSet implementować interfejs IQueryable i dlatego może służyć jako punktu wyjścia do pisania zapytania LINQ w odniesieniu do bazy danych. To nie jest właściwym miejscem dla szczegółowe omówienie LINQ, ale poniżej przedstawiono dwa proste przykłady:  

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

Należy pamiętać, że DbSet IDbSet zawsze tworzenie zapytań względem bazy danych i zawsze będzie obejmować komunikacji dwustronnej w bazie danych, nawet jeśli zwróconych już istnieje w kontekście. Zapytanie jest wykonywane względem bazy danych po:  

- Są wyliczane przez **foreach** (C#) lub **dla każdego** — instrukcja (Visual Basic).  
- Jego są wyliczane przez operację kolekcji takich jak [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), lub [tolist —](https://msdn.microsoft.com/library/bb342261).  
- Operatory LINQ, takich jak [pierwszy](https://msdn.microsoft.com/library/bb291976) lub [wszelkie](https://msdn.microsoft.com/library/bb337697) są określone w najbardziej zewnętrznej część zapytania.  
- Następujące metody są wywoływane: [obciążenia](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) metody rozszerzenia na DbSet [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)i Database.ExecuteSqlCommand.  

Gdy wyniki są zwracane z bazy danych, obiekty, które nie istnieją w kontekście są dołączone do kontekstu. Jeśli obiekt jest już w kontekście, istniejący obiekt jest zwracany (są bieżące i oryginalne wartości właściwości obiektu we wpisie **nie** zastąpione wartościami bazy danych).  

Podczas wykonywania zapytania jednostek, które zostały dodane do kontekstu, ale nie zostały zapisane w bazie danych nie są zwracane jako część zestawu wyników. Aby uzyskać dane, które znajduje się w kontekście, zobacz [dane lokalne](~/ef6/querying/local-data.md).  

Jeśli zapytanie zwraca żadnych wierszy z bazy danych, wynik będzie pustą kolekcję, zamiast **null**.  

## <a name="finding-entities-using-primary-keys"></a>Wyszukiwanie jednostek przy użyciu kluczy podstawowych  

Metody Find na DbSet używa wartość klucza podstawowego do podejmą próbę odnalezienia śledzone przez kontekst jednostki. Jeśli jednostka nie zostanie znaleziony w kontekście następnie kwerendę otrzymasz do bazy danych można znaleźć jednostki istnieje. Jeśli jednostka nie zostanie odnaleziona w kontekście lub w bazie danych, zwracana jest wartość null.  

Znajdź różni się od przy użyciu zapytania na dwa istotne sposoby:  

- Przesłania danych do bazy danych będą dostępne tylko w przypadku, jeśli jednostki z danym kluczem nie zostanie znaleziony w kontekście.  
- Znajdź zwróci jednostek, które są w stanie dodany. Znajdź zwróci jednostek, które zostały dodane do kontekstu, ale nie zostały zapisane w bazie danych.  
### <a name="finding-an-entity-by-primary-key"></a>Wyszukiwanie jednostek według klucza podstawowego  

Poniższy kod pokazuje do niektórych zastosowań wyszukiwania:  

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

### <a name="finding-an-entity-by-composite-primary-key"></a>Wyszukiwanie jednostki według złożony klucz podstawowy  

Entity Framework umożliwia jednostki mają klucze złożone — czyli klucz, który składa się z więcej niż jednej właściwości. Na przykład można mieć jednostki BlogSettings, która reprezentuje ustawienia użytkowników, dla konkretnego blogu. Ponieważ użytkownik będzie istniało tylko jeden BlogSettings dla każdego bloga, które można wykonać następujące akcje wybrała się klucz podstawowy BlogSettings kombinacją BlogId i nazwy użytkownika. Poniższy kod próbuje znaleźć BlogSettings z BlogId = 3, a nazwa użytkownika = "johndoe1987":  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

Należy pamiętać, że w przypadku kluczy złożonych należy użyć ColumnAttribute lub interfejsu API fluent Aby określić kolejność właściwości klucza złożonego. Podczas określania wartości, które tworzą klucz, wywołania do znalezienia należy użyć tej kolejności.  
