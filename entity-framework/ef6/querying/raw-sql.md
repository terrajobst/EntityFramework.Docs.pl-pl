---
title: Pierwotne zapytania SQL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 6b00648939ccedffeed09b4e1d6e8d70fa262a36
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490587"
---
# <a name="raw-sql-queries"></a>Pierwotne zapytania SQL
Platformy Entity Framework umożliwia tworzenie zapytań za pomocą LINQ z Twoich zajęciach jednostki. Jednak może być razy, które chcesz uruchamiać zapytania przy użyciu surowego SQL bezpośrednio w bazie danych. W tym wywołaniem procedury składowane, które mogą być przydatne w przypadku Code First modeli, które aktualnie nie obsługują mapowanie procedur składowanych. Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.  

## <a name="writing-sql-queries-for-entities"></a>Pisanie zapytań SQL dla jednostek  

Metoda SqlQuery na DbSet umożliwia pierwotne zapytania SQL, który ma zostać zapisany, który zwróci wystąpień jednostek. Zwracanych obiektów będą śledzone przez kontekst, tak jak powinny, jeśli zostały zwrócone przez zapytanie LINQ. Na przykład:  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

Należy pamiętać, że podobnie jak w przypadku zapytań LINQ, zapytanie nie jest wykonywany do momentu wyliczane są wyniki — w powyższym przykładzie jest to zrobić za pomocą wywołania tolist —.  

Należy zachować ostrożność przy każdym pierwotne zapytania SQL są zapisywane w dwóch powodów. Po pierwsze zapytanie mają być zapisywane upewnij się, że tylko zwraca jednostek, które są naprawdę żądanego typu. Na przykład podczas korzystania z funkcji, takich jak dziedziczenie jest łatwo można napisać zapytanie, które spowoduje utworzenie jednostek, które są nieprawidłowego typu CLR.  

Po drugie niektóre typy pierwotne zapytania SQL ujawnia potencjalne zagrożenia bezpieczeństwa, zwłaszcza w części dotyczącej ataki przez wstrzyknięcie kodu SQL. Upewnij się, że użyto parametrów w zapytaniu w prawidłowy sposób, aby zabezpieczyć się przed takimi atakami.  

### <a name="loading-entities-from-stored-procedures"></a>Trwa ładowanie jednostek z procedur składowanych  

DbSet.SqlQuery służy do ładowania jednostki w wynikach procedury składowanej. Na przykład poniższy kod wywołuje dbo. Procedura GetBlogs w bazie danych:  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

Parametry można też przekazać do procedury składowanej przy użyciu następującej składni:  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a>Pisanie zapytań SQL dla typów innych niż jednostek  

Zapytanie SQL zwraca wystąpień dowolnego typu, w tym typów pierwotnych, mogą być tworzone przy użyciu metody SqlQuery klasy bazy danych. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

Kontekst nigdy nie będą śledzone wyniki zwrócone z SqlQuery w bazie danych, nawet jeśli obiekty są wystąpieniami typu jednostki.  

## <a name="sending-raw-commands-to-the-database"></a>Wysyłania poleceń pierwotnych do bazy danych  

W poleceniach zapytań nie mogą być wysyłane do bazy danych przy użyciu metody ExecuteSqlCommand w bazie danych. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

Należy zauważyć, że wszelkie zmiany wprowadzone do danych w bazie danych przy użyciu ExecuteSqlCommand nieprzezroczysta dla kontekstu aż do jednostek są załadowane lub ponownie załadować z bazy danych.  

### <a name="output-parameters"></a>Parametry wyjściowe  

Jeśli używane są parametry wyjściowe, ich wartości nie będzie dostępne, dopóki wyniki zostały całkowicie odczytane. Jest to spowodowane bazowego zachowanie obiekt DbDataReader, zobacz [podczas pobierania danych przy użyciu elementu DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) Aby uzyskać więcej informacji.  
