---
title: Praca z wartościami właściwości — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: d8a18182754980d79b71df3f227b30c4ce40366f
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182140"
---
# <a name="working-with-property-values"></a>Praca z wartościami właściwości
W przypadku większości Entity Framework należy zachować ostrożność śledzenia stanu, oryginalnych wartości i bieżących wartości właściwości wystąpień jednostek. Mogą jednak wystąpić sytuacje — takie jak scenariusze rozłączenia — w przypadku, gdy chcesz wyświetlić informacje o właściwościach i manipulować nimi. Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.  

Entity Framework śledzi dwie wartości dla każdej właściwości śledzonej jednostki. Bieżąca wartość to, jak nazwa wskazuje bieżącą wartość właściwości w jednostce. Oryginalna wartość jest wartością, którą Właściwość miała w czasie, gdy zapytanie o jednostkę z bazy danych lub dołączono do kontekstu.  

Istnieją dwa ogólne mechanizmy pracy z wartościami właściwości:  

- Wartość pojedynczej właściwości można uzyskać w jednoznacznie określonym sposobie przy użyciu metody właściwości.  
- Wartości wszystkich właściwości jednostki można odczytać w obiekcie dbpropertyvalues. Dbpropertyvalues następnie działa jako obiekt podobny do słownika, aby umożliwić odczytywanie i ustawianie wartości właściwości. Wartości w obiekcie dbpropertyvalue można ustawić na podstawie wartości w innych obiektach dbpropertyvalue lub z wartości w innym obiekcie, takich jak inna kopia jednostki lub obiekt prostego transferu danych (DTO).  

W poniższych sekcjach przedstawiono przykłady użycia powyższych mechanizmów.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>Pobieranie i Ustawianie bieżącej lub oryginalnej wartości pojedynczej właściwości  

W poniższym przykładzie pokazano, jak można odczytać bieżącą wartość właściwości, a następnie ustawić nową wartość:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

Użyj właściwości OriginalValue zamiast właściwości CurrentValue, aby odczytać lub ustawić oryginalną wartość.  

Zwróć uwagę, że zwracana wartość jest wpisana jako "Object", gdy do określenia nazwy właściwości zostanie użyty ciąg. Z drugiej strony zwracana wartość jest silnie wpisana, jeśli jest używane wyrażenie lambda.  

Ustawienie wartości właściwości w taki sposób spowoduje oznaczenie właściwości jako modyfikowanej, jeśli nowa wartość różni się od starej wartości.  

Gdy wartość właściwości jest ustawiona w ten sposób, zmiana jest wykrywana automatycznie, nawet jeśli AutoDetectChanges jest wyłączona.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>Pobieranie i Ustawianie bieżącej wartości niezamapowanej właściwości  

Nie można również odczytać bieżącej wartości właściwości, która nie jest zamapowana do bazy danych. Przykładem niezamapowanej właściwości może być Właściwość RssLink w blogu. Ta wartość może być obliczana w oparciu o BlogId i dlatego nie musi być przechowywana w bazie danych. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    // Read the current value of an unmapped property
    var rssLink = context.Entry(blog).Property(p => p.RssLink).CurrentValue;

    // Use a string to specify the property name
    var rssLinkAgain = context.Entry(blog).Property("RssLink").CurrentValue;
}
```  

Bieżącą wartość można również ustawić, jeśli właściwość uwidacznia metodę ustawiającą.  

Odczytywanie wartości niezamapowanych właściwości jest przydatne podczas przeprowadzania Entity Framework weryfikacji niezamapowanych właściwości. Z tego samego powodu bieżące wartości można odczytać i ustawić dla właściwości jednostek, które nie są aktualnie śledzone przez kontekst. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    // Create an entity that is not being tracked
    var blog = new Blog { Name = "ADO.NET Blog" };

    // Read and set the current value of Name as before
    var currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";
    var currentName2 = context.Entry(blog).Property("Name").CurrentValue;
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

Należy zauważyć, że oryginalne wartości nie są dostępne dla niezamapowanych właściwości lub dla właściwości jednostek, które nie są śledzone przez kontekst.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>Sprawdzanie, czy właściwość jest oznaczona jako zmodyfikowana  

W poniższym przykładzie pokazano, jak sprawdzić, czy dana właściwość jest oznaczona jako zmodyfikowano:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

Wartości zmodyfikowanych właściwości są wysyłane jako aktualizacje bazy danych po wywołaniu metody SaveChanges.  

##  <a name="marking-a-property-as-modified"></a>Oznaczanie właściwości jako zmodyfikowane  

W poniższym przykładzie pokazano, jak wymusić oznaczenie pojedynczej właściwości jako zmodyfikowanej:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

Oznaczenie właściwości jako zmodyfikowanej powoduje, że aktualizacja ma być wysyłana do bazy danych dla właściwości, gdy metody SaveChanges jest wywoływana, nawet jeśli bieżąca wartość właściwości jest taka sama jak oryginalna wartość.  

Obecnie nie można zresetować pojedynczej właściwości, aby nie była modyfikowana po oznaczeniu jej jako zmodyfikowana. Jest to coś, co planujemy obsłużyć w przyszłej wersji.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>Odczytywanie bieżących, oryginalnych i wartości bazy danych dla wszystkich właściwości jednostki  

W poniższym przykładzie pokazano, jak odczytać bieżące wartości, oryginalne wartości i wartości w rzeczywistości w bazie danych dla wszystkich zamapowanych właściwości jednostki.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Make a modification to Name in the tracked entity
    blog.Name = "My Cool Blog";

    // Make a modification to Name in the database
    context.Database.SqlCommand("update dbo.Blogs set Name = 'My Boring Blog' where Id = 1");

    // Print out current, original, and database values
    Console.WriteLine("Current values:");
    PrintValues(context.Entry(blog).CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(context.Entry(blog).OriginalValues);

    Console.WriteLine("\nDatabase values:");
    PrintValues(context.Entry(blog).GetDatabaseValues());
}

public static void PrintValues(DbPropertyValues values)
{
    foreach (var propertyName in values.PropertyNames)
    {
        Console.WriteLine("Property {0} has value {1}",
                          propertyName, values[propertyName]);
    }
}
```  

Bieżące wartości to wartości, które obecnie zawierają właściwości jednostki. Pierwotne wartości to wartości, które zostały odczytane z bazy danych podczas wykonywania zapytania dotyczącego jednostki. Wartości bazy danych są wartościami, które są obecnie przechowywane w bazie danych. Pobieranie wartości bazy danych jest przydatne, gdy wartości w bazie danych mogły ulec zmianie od momentu, w którym dana kwerenda została utworzona przez innego użytkownika.  

## <a name="setting-current-or-original-values-from-another-object"></a>Ustawianie bieżących lub oryginalnych wartości z innego obiektu  

Bieżące lub oryginalne wartości śledzonej jednostki można aktualizować przez kopiowanie wartości z innego obiektu. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var coolBlog = new Blog { Id = 1, Name = "My Cool Blog" };
    var boringBlog = new BlogDto { Id = 1, Name = "My Boring Blog" };

    // Change the current and original values by copying the values from other objects
    var entry = context.Entry(blog);
    entry.CurrentValues.SetValues(coolBlog);
    entry.OriginalValues.SetValues(boringBlog);

    // Print out current and original values
    Console.WriteLine("Current values:");
    PrintValues(entry.CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(entry.OriginalValues);
}

public class BlogDto
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```  

Uruchomienie kodu powyżej spowoduje wydrukowanie:  

```console
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

Ta technika jest czasami stosowana podczas aktualizowania jednostki przy użyciu wartości uzyskanych z wywołania usługi lub klienta w aplikacji n-warstwowej. Należy zauważyć, że użyty obiekt nie musi być tego samego typu co jednostka, tak długo, jak ma właściwości, których nazwy są zgodne z tymi, które są takie same. W powyższym przykładzie wystąpienie elementu BlogDTO jest używane do aktualizowania oryginalnych wartości.  

Należy pamiętać, że tylko właściwości, które są ustawione na różne wartości, gdy skopiowane z innego obiektu zostaną oznaczone jako zmodyfikowane.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>Ustawianie bieżących lub oryginalnych wartości ze słownika  

Bieżące lub oryginalne wartości śledzonej jednostki można aktualizować przez kopiowanie wartości ze słownika lub innej struktury danych. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "The New ADO.NET Blog" },
        { "Url", "blogs.msdn.com/adonet" },
    };

    var currentValues = context.Entry(blog).CurrentValues;

    foreach (var propertyName in newValues.Keys)
    {
        currentValues[propertyName] = newValues[propertyName];
    }

    PrintValues(currentValues);
}
```  

Użyj właściwości OriginalValues zamiast właściwości CurrentValues, aby ustawić oryginalne wartości.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>Ustawianie bieżących lub oryginalnych wartości ze słownika przy użyciu właściwości  

Alternatywą dla korzystania z CurrentValues lub OriginalValues, jak pokazano powyżej, jest użycie metody właściwości w celu ustawienia wartości każdej właściwości. Może to być preferowany, gdy trzeba ustawić wartości właściwości złożonych. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "John Doe" },
        { "Location.City", "Redmond" },
        { "Location.State.Name", "Washington" },
        { "Location.State.Code", "WA" },
    };

    var entry = context.Entry(user);

    foreach (var propertyName in newValues.Keys)
    {
        entry.Property(propertyName).CurrentValue = newValues[propertyName];
    }
}
```  

W powyższym przykładzie można uzyskać dostęp do właściwości złożonych przy użyciu kropkowanych nazw. Aby uzyskać informacje o innych sposobach uzyskiwania dostępu do właściwości złożonych, zobacz dwie sekcje w dalszej części tego tematu w odniesieniu do właściwości złożonych.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>Tworzenie sklonowanego obiektu zawierającego bieżące, oryginalne lub wartości bazy danych  

Obiekt dbpropertyvalue zwrócony z CurrentValues, OriginalValues lub GetDatabaseValues może służyć do tworzenia klonu jednostki. Ten klon będzie zawierać wartości właściwości z obiektu dbpropertyvalue użytego do jego utworzenia. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

Zwróć uwagę, że zwrócony obiekt nie jest jednostką i nie jest śledzony przez kontekst. Zwrócony obiekt nie ma również żadnych relacji ustawionych na inne obiekty.  

Sklonowany obiekt może być przydatny do rozwiązywania problemów dotyczących współbieżnych aktualizacji bazy danych, szczególnie w przypadku używania interfejsu użytkownika, który obejmuje powiązanie danych z obiektami określonego typu.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>Pobieranie i Ustawianie bieżących lub oryginalnych wartości właściwości złożonych  

Wartość całego obiektu złożonego można odczytać i ustawić przy użyciu metody właściwości tak samo, jak może być właściwością pierwotną. Ponadto możesz przejść do szczegółów obiektu złożonego i odczytać lub ustawić właściwości tego obiektu, a nawet zagnieżdżony obiekt. Oto kilka przykładów:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    // Get the Location complex object
    var location = context.Entry(user)
                       .Property(u => u.Location)
                       .CurrentValue;

    // Get the nested State complex object using chained calls
    var state1 = context.Entry(user)
                     .ComplexProperty(u => u.Location)
                     .Property(l => l.State)
                     .CurrentValue;

    // Get the nested State complex object using a single lambda expression
    var state2 = context.Entry(user)
                     .Property(u => u.Location.State)
                     .CurrentValue;

    // Get the nested State complex object using a dotted string
    var state3 = context.Entry(user)
                     .Property("Location.State")
                     .CurrentValue;

    // Get the value of the Name property on the nested State complex object using chained calls
    var name1 = context.Entry(user)
                       .ComplexProperty(u => u.Location)
                       .ComplexProperty(l => l.State)
                       .Property(s => s.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a single lambda expression
    var name2 = context.Entry(user)
                       .Property(u => u.Location.State.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a dotted string
    var name3 = context.Entry(user)
                       .Property("Location.State.Name")
                       .CurrentValue;
}
```  

Użyj właściwości OriginalValue zamiast właściwości CurrentValue, aby pobrać lub ustawić oryginalną wartość.  

Należy zauważyć, że właściwość lub metoda ComplexProperty może być używana w celu uzyskania dostępu do właściwości złożonej. Jednak Metoda ComplexProperty musi zostać użyta, jeśli chcesz przejść do obiektu złożonego z dodatkowymi wywołaniami właściwości lub ComplexProperty.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>Korzystanie z funkcji dbpropertyvalues w celu uzyskania dostępu do właściwości złożonych  

W przypadku korzystania z CurrentValues, OriginalValues lub GetDatabaseValues w celu uzyskania wszystkich bieżących, oryginalnych lub wartości bazy danych dla jednostki, wartości wszelkich właściwości złożonych są zwracane jako zagnieżdżone obiekty dbpropertyvalues. Te obiekty zagnieżdżone można następnie użyć do uzyskania wartości obiektu złożonego. Na przykład następująca metoda spowoduje wydrukowanie wartości wszystkich właściwości, w tym wartości wszelkich złożonych właściwości i zagnieżdżonych właściwości złożonych.  

``` csharp
public static void WritePropertyValues(string parentPropertyName, DbPropertyValues propertyValues)
{
    foreach (var propertyName in propertyValues.PropertyNames)
    {
        var nestedValues = propertyValues[propertyName] as DbPropertyValues;
        if (nestedValues != null)
        {
            WritePropertyValues(parentPropertyName + propertyName + ".", nestedValues);
        }
        else
        {
            Console.WriteLine("Property {0}{1} has value {2}",
                              parentPropertyName, propertyName,
                              propertyValues[propertyName]);
        }
    }
}
```  

Aby wydrukować wszystkie bieżące wartości właściwości, Metoda zostałaby wywołana w następujący sposób:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
