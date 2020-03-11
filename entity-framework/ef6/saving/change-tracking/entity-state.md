---
title: Praca z Stanami jednostek — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: ef0e8d5a5a9d66adab7046088c49d8cd472edc8a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419700"
---
# <a name="working-with-entity-states"></a>Praca z Stanami jednostek
W tym temacie opisano sposób dodawania i dołączania jednostek do kontekstu oraz sposobu, w jaki Entity Framework przetwarzają je podczas metody SaveChanges.
Entity Framework ma obsłużyć śledzenie stanu jednostek, gdy są one połączone z kontekstem, ale w przypadku scenariuszy rozłączonych lub N-warstwowych można pozwolić, aby EF wiedział, jaki jest stan jednostek.
Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.  

## <a name="entity-states-and-savechanges"></a>Stany jednostek i metody SaveChanges

Jednostka może znajdować się w jednym z pięciu Stanów, zgodnie z definicją EntityState. Te Stany to:  

- Dodano: jednostka jest śledzona przez kontekst, ale jeszcze nie istnieje w bazie danych  
- Niezmieniony: jednostka jest śledzona przez kontekst i istnieje w bazie danych, a jej wartości właściwości nie uległy zmianie z wartości w bazie danych.  
- Zmodyfikowano: jednostka jest śledzona przez kontekst i istnieje w bazie danych, a niektóre lub wszystkie wartości właściwości zostały zmodyfikowane.  
- Usunięto: jednostka jest śledzona przez kontekst i istnieje w bazie danych, ale została oznaczona do usunięcia z bazy danych przy następnym wywołaniu metody SaveChanges  
- Odłączono: jednostka nie jest śledzona przez kontekst  

Metody SaveChanges wykonuje różne rzeczy dla jednostek w różnych stanach:  

- Niezmienione jednostki nie są rozróżniane przez metody SaveChanges. Aktualizacje nie są wysyłane do bazy danych dla jednostek w stanie niezmienionym.  
- Dodane jednostki są wstawiane do bazy danych, a następnie stają się niezmienione, gdy metody SaveChanges zwraca.  
- Zmodyfikowane jednostki są aktualizowane w bazie danych, a następnie stają się niezmienione, gdy metody SaveChanges zwraca.  
- Usunięte jednostki są usuwane z bazy danych, a następnie odłączane od kontekstu.  

W poniższych przykładach pokazano, jak można zmienić stan jednostki lub grafu jednostki.  

## <a name="adding-a-new-entity-to-the-context"></a>Dodawanie nowej jednostki do kontekstu  

Nową jednostkę można dodać do kontekstu, wywołując metodę Add w Nieogólnymi.
Powoduje to umieszczenie jednostki w stanie dodany, co oznacza, że zostanie ona wstawiona do bazy danych przy następnym wywołaniu metody SaveChanges.
Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

Innym sposobem dodania nowej jednostki do kontekstu jest zmiana jej stanu na dodany. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

Na koniec można dodać nową jednostkę do kontekstu, łącząc ją z inną jednostką, która jest już śledzona.
Może to być spowodowane dodaniem nowej jednostki do właściwości nawigacji kolekcji innej jednostki lub przez ustawienie właściwości nawigacji referencyjnej innej jednostki, aby wskazywała nową jednostkę. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

Należy zauważyć, że dla wszystkich tych przykładów, jeśli dodawana jednostka zawiera odwołania do innych jednostek, które nie są jeszcze śledzone, te nowe jednostki zostaną również dodane do kontekstu i zostaną wstawione do bazy danych przy następnym wywołaniu metody SaveChanges.  

## <a name="attaching-an-existing-entity-to-the-context"></a>Dołączanie istniejącej jednostki do kontekstu  

Jeśli masz już istniejącą jednostkę w bazie danych, która nie jest obecnie śledzona przez kontekst, możesz powiedzieć kontekstowi, aby śledził jednostkę przy użyciu metody Attach w Nieogólnymi. Jednostka będzie w stanie niezmienionym w kontekście. Na przykład:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

Należy zauważyć, że w bazie danych nie zostaną wprowadzone żadne zmiany, jeśli metody SaveChanges jest wywoływana bez wykonywania jakichkolwiek innych operacji manipulowania załączoną jednostką. Wynika to z faktu, że jednostka jest w stanie niezmienionym.  

Innym sposobem dołączenia istniejącej jednostki do kontekstu jest zmiana stanu na niezmieniony. Na przykład:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Należy zauważyć, że w obu tych przykładach, jeśli dołączona jednostka ma odwołania do innych jednostek, które nie są jeszcze śledzone, te nowe jednostki zostaną również dołączone do kontekstu w stanie niezmienionym.  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a>Dołączanie istniejącej, ale zmodyfikowanej jednostki do kontekstu  

Jeśli masz już istniejącą jednostkę w bazie danych, ale w której można było wprowadzić zmiany, możesz poinstruować kontekst, aby dołączyć jednostkę i ustawić jej stan na zmodyfikowano.
Na przykład:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

Zmiana stanu na zmodyfikowano wszystkie właściwości jednostki zostanie oznaczona jako zmodyfikowana, a wszystkie wartości właściwości będą wysyłane do bazy danych po wywołaniu metody SaveChanges.  

Należy pamiętać, że jeśli dołączona jednostka ma odwołania do innych jednostek, które nie są jeszcze śledzone, wówczas te nowe jednostki zostaną dołączone do kontekstu w stanie niezmienionym — nie zostaną automatycznie zmodyfikowane.
Jeśli masz wiele jednostek, które muszą zostać oznaczone jako zmodyfikowane, należy ustawić dla każdej z nich stan osobno.  

## <a name="changing-the-state-of-a-tracked-entity"></a>Zmiana stanu śledzonej jednostki  

Stan jednostki, która jest już śledzona, można zmienić, ustawiając właściwość State na jej wpis. Na przykład:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Należy zauważyć, że wywołanie metody Add lub Attach dla jednostki, która jest już śledzona, może być również używane do zmiany stanu jednostki. Na przykład wywołanie metody Attach dla jednostki, która jest obecnie w stanie dodany, zmieni swój stan na niezmieniony.  

## <a name="insert-or-update-pattern"></a>Wstaw lub Aktualizuj wzorzec  

Typowym wzorcem dla niektórych aplikacji jest dodanie jednostki jako nowej (w wyniku wstawienia bazy danych) lub dołączenie jednostki jako istniejącej i oznaczenie jej jako zmodyfikowanej (w wyniku aktualizacji bazy danych) w zależności od wartości klucza podstawowego.
Na przykład podczas korzystania z wygenerowanej przez bazę danych liczb całkowitych klucze podstawowe są wspólne do traktowania jednostki z kluczem zerowym jako nowy i jednostki z niezerowym kluczem jako istniejący.
Ten wzorzec można osiągnąć przez ustawienie stanu jednostki na podstawie sprawdzenia wartości klucza podstawowego. Na przykład:  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

Należy zauważyć, że po zmianie stanu na zmodyfikowane wszystkie właściwości jednostki zostaną oznaczone jako zmodyfikowane, a wszystkie wartości właściwości będą wysyłane do bazy danych po wywołaniu metody SaveChanges.  
