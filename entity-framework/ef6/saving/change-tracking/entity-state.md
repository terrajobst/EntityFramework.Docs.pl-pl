---
title: Praca z stany jednostki - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: ef0e8d5a5a9d66adab7046088c49d8cd472edc8a
ms.sourcegitcommit: e5f9ca4aa41e64141fa63a1e5fcf4d4775d67d24
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899655"
---
# <a name="working-with-entity-states"></a>Praca z stany jednostki
W tym temacie opisano sposób dodawania i dołączyć jednostek do kontekstu i jak przetwarza je podczas SaveChanges Entity Framework.
Śledzenie stanu jednostek, gdy są one połączone z kontekstu, ale w scenariuszach odłączone lub N-warstwowa można pozwolić, aby wiedzieć, jakie stanu jednostek EF powinien być w zajmuje się platformy Entity Framework.
Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.  

## <a name="entity-states-and-savechanges"></a>Stany jednostki i SaveChanges

Jednostki może być jeden z pięciu stanów zdefiniowanych przez wyliczenie EntityState. Te stany są następujące:  

- Dodano: jednostka jest śledzony przez kontekst, ale jeszcze nie istnieje w bazie danych  
- Niezmieniona: jednostka jest śledzony przez kontekst i istnieje w bazie danych i jego wartości właściwości nie zmieniły się od wartości w bazie danych  
- Zmodyfikowane: jednostka jest śledzony przez kontekst i istnieje w bazie danych, a niektóre lub wszystkie wartości właściwości zostały zmodyfikowane  
- Usunięto: jednostka jest śledzony przez kontekst i istnieje w bazie danych, ale została oznaczona do usunięcia z bazy danych następnym razem, gdy zostanie wywołana SaveChanges  
- Odłączony: jednostka nie jest śledzony przez kontekst  

SaveChanges wykonuje różne znaczenie w przypadku jednostek w różnych stanach:  

- Niezmienione jednostki są nie korzystały SaveChanges. Aktualizacje nie są wysyłane do bazy danych dla obiektów w stanie niezmieniony.  
- Dodano jednostki są wstawiane do bazy danych, a następnie stają się zmian podczas SaveChanges zwraca.  
- Jednostki zmodyfikowane zostaną zaktualizowane w bazie danych, a następnie stają się zmian podczas SaveChanges zwraca.  
- Usuniętych jednostek są usuwane z bazy danych, a następnie zostanie odłączony od kontekstu.  

W poniższych przykładach pokazano sposób, w którym można zmienić stanu jednostki lub grafu jednostki.  

## <a name="adding-a-new-entity-to-the-context"></a>Dodawanie nowej jednostki do kontekstu  

Nowa jednostka można dodać do kontekstu poprzez wywołanie metody Add w DbSet.
To powoduje umieszczenie jednostki w stanie Added, co oznacza, że zostanie on wstawiony do bazy danych przy następnym wywołaniu funkcji SaveChanges.
Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

Innym sposobem dodania nowego obiektu dla kontekstu jest aby zmienić jego stan dodany. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

Na koniec możesz dodać nową jednostkę do kontekstu przez podłączenie jej do innej jednostki, która jest już śledzony.
Może to być, dodając nową jednostkę do właściwości nawigacji kolekcji innej jednostki lub przez ustawienie właściwości nawigacji odwołanie do innej jednostki, aby wskazywał nową jednostkę. Na przykład:  

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

Należy zauważyć, że dla wszystkich tych przykładach, jeśli jednostka dodawany ma odwołania do innych jednostek, które nie są jeszcze śledzone następnie te nowe jednostki zostanie także dodana do kontekstu i zostaną wstawione do bazy danych przy następnym wywołaniu funkcji SaveChanges.  

## <a name="attaching-an-existing-entity-to-the-context"></a>Dołączanie istniejącej jednostki do kontekstu  

Jeśli masz jednostki, który znasz już istnieje w bazie danych, ale który nie jest obecnie śledzony przez kontekst, a następnie będzie widnieć napis kontekstu do śledzenia jednostek przy użyciu metody Dołącz na DbSet. Jednostki będą w stanie niezmieniony w kontekście. Na przykład:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

Należy pamiętać, że nie zostaną wprowadzone zmiany do bazy danych jeśli SaveChanges nazywa się bez wykonania tej czynności manipulowania dołączonych jednostki. Jest tak, ponieważ jednostka jest w stanie niezmieniony.  

Innym sposobem na dołączanie istniejącej jednostki do kontekstu jest aby zmienić jego stan Unchanged. Na przykład:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Należy pamiętać, że dla obu tych przykładach Jeśli jednostki dołączany ma odwołania do innych jednostek, które nie są jeszcze śledzone następnie te nowe jednostki będą również dołączone do kontekstu w stanie niezmieniony.  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a>Dołączanie do istniejącego, ale zmodyfikowano jednostkę do kontekstu  

Jeśli masz jednostki, który znasz już istnieje w bazie danych, ale które mogą wprowadzono zmiany, a następnie będzie widnieć napis kontekstu, aby dołączyć jednostki i ustaw jego stan zmodyfikowane.
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

Po zmianie stanu na zmodyfikowane właściwości jednostki zostanie oznaczona jako zmodyfikowana i wartości właściwości wysyłanych do bazy danych po wywołaniu funkcji SaveChanges.  

Należy pamiętać, że jeśli jednostka dołączany zawiera odwołania do innych jednostek, które nie są jeszcze śledzone, następnie te nowe jednostki zostanie dołączony do kontekstu w stanie niezmieniony — ich nie zostaną automatycznie wprowadzone zmodyfikowane.
Jeśli masz wiele jednostek, które muszą być oznaczone zmodyfikowane należy ustawić stan dla każdego z tych jednostek indywidualnie.  

## <a name="changing-the-state-of-a-tracked-entity"></a>Zmiany stanu jednostki śledzonych  

Można zmienić stanu jednostki, która jest już śledzony przez ustawienie właściwości stanu na jej wejścia. Na przykład:  

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

Należy pamiętać, że wywołanie apletu Dodaj lub Dołącz do jednostki, która jest już śledzony umożliwia także zmiany stanu jednostki. Na przykład wywołanie dołączenia dla jednostki, która jest obecnie w stanie dodany zmieni jego stan na Unchanged.  

## <a name="insert-or-update-pattern"></a>Wstaw lub zaktualizuj wzorzec  

Typowym wzorcem dla niektórych aplikacji jest dodawanie jednostki jako nowy (co powoduje wstawienie bazy danych) lub dołączyć jednostkę jako istniejące i oznacz go jako zmodyfikowana (co powoduje aktualizację bazy danych) w zależności od wartości klucza podstawowego.
Na przykład podczas korzystania z kluczy podstawowych całkowitą bazy danych, generowany jest wspólne dla obiektu z kluczem zero jako nowe i jednostki z kluczem różna od zera, jak istniejące.
Ten wzorzec można osiągnąć przez ustawienie stanu jednostki, w oparciu o sprawdzenie wartości klucza podstawowego. Na przykład:  

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

Należy pamiętać, że po zmianie stanu na zmodyfikowane właściwości jednostki zostanie oznaczona jako zmodyfikowana, i wartości właściwości wysyłanych do bazy danych po wywołaniu funkcji SaveChanges.  
