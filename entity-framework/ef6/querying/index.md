---
title: Wykonywanie zapytań i znajdowanie jednostek — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417169"
---
# <a name="querying-and-finding-entities"></a>Wykonywanie zapytań i znajdowanie jednostek
W tym temacie omówiono różne sposoby wykonywania zapytań dotyczących danych przy użyciu Entity Framework, w tym LINQ i metody Find. Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.  

## <a name="finding-entities-using-a-query"></a>Znajdowanie jednostek przy użyciu zapytania  

Nieogólnymi i IDbSet implementują interfejs IQueryable i dlatego mogą być używane jako punkt wyjścia do pisania zapytania LINQ względem bazy danych. To nie jest odpowiednie miejsce do szczegółowego omówienia LINQ, ale poniżej przedstawiono kilka prostych przykładów:  

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

Należy pamiętać, że Nieogólnymi i IDbSet zawsze tworzą zapytania względem bazy danych i zawsze będą obejmowały rundę do bazy danych, nawet jeśli zwrócona jednostka już istnieje w kontekście. Zapytanie jest wykonywane w odniesieniu do bazy danych, gdy:  

- Jest on wyliczany przez instrukcję **foreach** (C#) lub **dla każdej** instrukcji (Visual Basic).  
- Jest on wyliczany przez operację kolekcji, taką jak [ToArray —](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary)lub [ToList —](https://msdn.microsoft.com/library/bb342261).  
- Operatory LINQ, takie jak [First](https://msdn.microsoft.com/library/bb291976) lub [any](https://msdn.microsoft.com/library/bb337697) , są określone w najbardziej zewnętrznej części zapytania.  
- Następujące metody są wywoływane: Metoda [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) Extension w Nieogólnymi, [DbEntityEntry. reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)i Database. ExecuteSqlCommand.  

Gdy wyniki są zwracane z bazy danych, obiekty, które nie istnieją w kontekście, są dołączane do kontekstu. Jeśli obiekt znajduje się już w kontekście, zwracany jest istniejący obiekt (bieżące i oryginalne wartości właściwości obiektu w tym wpisie **nie** są zastępowane wartościami bazy danych).  

Podczas wykonywania zapytania jednostki, które zostały dodane do kontekstu, ale nie zostały jeszcze zapisane w bazie danych, nie są zwracane jako część zestawu wyników. Aby uzyskać dane, które znajdują się w kontekście, zobacz [dane lokalne](~/ef6/querying/local-data.md).  

Jeśli zapytanie nie zwraca żadnych wierszy z bazy danych, wynik będzie pustą kolekcją, a nie **wartością null**.  

## <a name="finding-entities-using-primary-keys"></a>Znajdowanie jednostek przy użyciu kluczy podstawowych  

Metoda Find w Nieogólnymi używa wartości klucza podstawowego, aby próbować znaleźć jednostkę śledzoną przez kontekst. Jeśli jednostka nie zostanie znaleziona w kontekście, zapytanie zostanie wysłane do bazy danych, aby znaleźć w niej jednostkę. Zwracana jest wartość null, jeśli jednostka nie została znaleziona w kontekście lub w bazie danych.  

Wyszukiwanie różni się od użycia zapytania na dwa znaczące sposoby:  

- Przepodróżowanie do bazy danych zostanie wykonane tylko wtedy, gdy jednostka z danym kluczem nie zostanie znaleziona w kontekście.  
- Znajdź zwróci jednostki, które znajdują się w stanie dodany. Oznacza to, że Znajdź zwróci jednostki, które zostały dodane do kontekstu, ale nie zostały jeszcze zapisane w bazie danych.  
### <a name="finding-an-entity-by-primary-key"></a>Znajdowanie jednostki według klucza podstawowego  

Poniższy kod przedstawia niektóre zastosowania wyszukiwania:  

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

Entity Framework umożliwia jednostkom posiadanie kluczy złożonych — to jest klucz składający się z więcej niż jednej właściwości. Można na przykład mieć jednostkę BlogSettings, która reprezentuje ustawienia użytkowników dla określonego bloga. Ze względu na to, że użytkownik będzie miał tylko jeden BlogSettings dla każdego blogu, który można wybrać, aby klucz podstawowy BlogSettings kombinację BlogId i nazwę użytkownika. Poniższy kod próbuje znaleźć BlogSettings z BlogId = 3 i username = "johndoe1987":  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

Należy pamiętać, że jeśli masz klucze złożone, musisz użyć elementu ColumnAttribute lub interfejsu API Fluent, aby określić kolejność właściwości klucza złożonego. Wywołanie metody Find musi używać tej kolejności podczas określania wartości, które tworzą klucz.  
