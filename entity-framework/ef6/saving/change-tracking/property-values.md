---
title: Praca z wartości właściwości - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: afde503bb4ed15fcf83a57053541cd5da8c89835
ms.sourcegitcommit: 50521b4a2f71139e6a7210a69ac73da582ef46cf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/27/2019
ms.locfileid: "67416674"
---
# <a name="working-with-property-values"></a>Praca z wartościami właściwości
W większości przypadków zajmie się Entity Framework śledzenia stanu, oryginalne wartości oraz bieżące wartości właściwości wystąpienia jednostki. Jednak może być pewnych przypadkach — takich jak rozłączonych scenariuszy —, w której chcesz wyświetlić i modyfikować EF ma informacje o właściwościach. Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.  

Entity Framework śledzi informacje o dwóch wartości dla każdej właściwości śledzone jednostki. Bieżąca wartość to, jak wskazuje nazwa, bieżąca wartość właściwości w obiekcie. Oryginalną wartość jest wartością, której właściwość jednostki pobieranych z bazy danych lub został dołączony do kontekstu.  

Istnieją dwa mechanizmy ogólne dotyczące pracy z wartości właściwości:  

- Wartość jednej właściwości mogą być uzyskane w silnie typizowany sposób przy użyciu metody właściwości.  
- Do obiektu DbPropertyValues można odczytać wartości dla wszystkich właściwości jednostki. DbPropertyValues następnie działa jako obiekt słownika przypominającej umożliwiające odczyt i ustawianie wartości właściwości. Można ustawić wartości w obiekcie DbPropertyValues z wartości w innym obiekcie DbPropertyValues lub wartości w innym obiekcie, takich jak kopiowanie innej jednostki lub obiektu transferu prostych danych (DTO).  

Poniższe sekcje pokazują przykłady przy użyciu zarówno mechanizmów powyżej.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>Pobieranie i ustawianie bieżącej lub oryginalna wartość wybranej właściwości  

W poniższym przykładzie pokazano, jak można odczytywać i następnie ustawić nową wartość bieżącą wartość właściwości:  

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

Użyj właściwości OriginalValue zamiast właściwości CurrentValue odczytać lub ustawić oryginalnej wartości.  

Należy pamiętać, że zwracana wartość jest wpisana jako "object", gdy ciąg jest używany do określenia nazwy właściwości. Z drugiej strony zwrócona wartość zdecydowanie jest wpisane, jeśli wyrażenie lambda jest używany.  

Ustawienie wartości właściwości, takie jak to tylko spowoduje oznaczenie właściwości, jak modyfikacji, jeśli nowa wartość jest inna niż poprzednia wartość.  

Gdy wartość właściwości jest ustawiana w ten sposób zmiany jest wykrywane automatycznie, nawet wtedy, gdy AutoDetectChanges jest wyłączona.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>Pobieranie i ustawianie bieżącej wartości właściwości niezamapowane  

Bieżąca wartość właściwości, która nie została zamapowana do bazy danych, również mogą być odczytywane. Przykład niezamapowane właściwości może być właściwością RssLink na blogu. Ta wartość może być obliczona w oparciu o BlogId i w związku z tym nie mają być przechowywane w bazie danych. Na przykład:  

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

Bieżąca wartość można również ustawić, jeśli właściwość udostępnia metody ustawiającej.  

Odczytywanie wartości właściwości niezamapowane jest przydatne w przypadku, gdy wykonywanie weryfikacji platformy Entity Framework niezamapowane właściwości. Z tego samego powodu bieżących wartości może odczytywać i ustawić dla właściwości jednostki, które nie są obecnie są śledzone przez kontekst. Na przykład:  

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

Pamiętaj, że oryginalne wartości nie są dostępne dla właściwości niezamapowane właściwości jednostki, które nie są śledzone przez kontekst.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>Sprawdzanie, czy właściwość jest oznaczona jako zmodyfikowana  

W poniższym przykładzie pokazano, jak sprawdzić, czy poszczególne właściwość jest oznaczona jako zmodyfikowana:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

Wartości zmodyfikowane właściwości są wysyłane jako aktualizacje w bazie danych po wywołaniu funkcji SaveChanges.  

##  <a name="marking-a-property-as-modified"></a>Oznaczanie jako zmodyfikowana właściwość  

W poniższym przykładzie pokazano, jak wymusić pojedynczej właściwości, aby być oznaczona jako zmodyfikowana:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

Oznaczanie właściwość jako zmodyfikowane wymusza aktualizację można wysyłać do bazy danych dla właściwości po wywołaniu funkcji SaveChanges nawet, jeśli bieżąca wartość właściwości jest taka sama jak oryginalną wartość.  

Nie jest obecnie możliwe zresetować pojedynczej właściwości nie można zmodyfikować po została oznaczona jako zmodyfikowana. Jest to coś, co firma Microsoft planuje się obsługiwać w przyszłej wersji.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>Odczytywanie bieżącego, oryginalne i wartości z bazy danych dla wszystkich właściwości jednostki  

W poniższym przykładzie pokazano, jak odczytać bieżące wartości, oryginalne wartości i wartości, które faktycznie w bazie danych dla wszystkich zamapowanych właściwości jednostki.  

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

Bieżące wartości są wartościami, które obecnie zawiera właściwości jednostki. Oryginalne wartości są wartościami, które zostały odczytane z bazy danych, gdy badano jednostki. Wartości bazy danych są wartości, ponieważ są one przechowywane w bazie danych. Pobieranie wartości z bazy danych jest przydatne w przypadku, gdy mógł ulec zmianie wartości w bazie danych, ponieważ badano jednostki, takie jak podczas edytowania równoczesne bazy danych zostały dokonane przez innych użytkowników.  

## <a name="setting-current-or-original-values-from-another-object"></a>Ustawianie bieżącej lub oryginalnej wartości z innego obiektu  

Bieżące i oryginalne wartości śledzonych jednostki mogą być aktualizowane przez skopiowanie wartości z innego obiektu. Na przykład:  

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

Uruchamianie kodu powyżej zostanie wydrukowana:  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

Ta technika jest czasami używana podczas aktualizowania jednostki przy użyciu wartości z wywołania usługi lub klienta w aplikacji n-warstwowej. Pamiętaj, że obiekt używany nie musi być tego samego typu co podmiot, tak długo, jak długo ma właściwości, których nazwy odpowiadają jednostki. W powyższym przykładzie wystąpienie BlogDTO służy do aktualizacji oryginalnych wartości.  

Należy zauważyć, że tylko te właściwości, które są ustawione na różnych wartości podczas kopiowania z innego obiektu zostanie oznaczona jako zmodyfikowana.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>Ustawianie bieżącej lub oryginalnej wartości ze słownika  

Bieżące i oryginalne wartości śledzonych jednostki mogą być aktualizowane przez skopiowanie wartości ze słownika lub inne struktury danych. Na przykład:  

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

Użyj właściwości OriginalValues zamiast właściwości CurrentValues można ustawić oryginalnej wartości.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>Ustawianie bieżącej lub oryginalnej wartości ze słownika przy użyciu właściwości  

Alternatywa dla użycia CurrentValues lub OriginalValues, jak pokazano powyżej jest użycie metody właściwości można ustawić wartość każdej właściwości. Może to być pożądane, gdy należy ustawić wartości właściwości złożonych. Na przykład:  

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

W przykładzie powyżej złożonych właściwości są dostępne przy użyciu notacji nazw. Inne sposoby dostępu do właściwości złożonych zobacz dwie sekcje w dalszej części tego tematu sobie konkretnie o złożonych właściwości.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>Tworzenie Sklonowany obiekt zawierający bieżące, oryginalny lub wartościami bazy danych  

Zwróciła obiekt DbPropertyValues CurrentValues, OriginalValues, lub GetDatabaseValues może służyć do tworzenia klonu jednostki. Klonuj ten będzie zawierać wartości właściwości z obiektu DbPropertyValues użyty do jego utworzenia. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

Należy pamiętać, że zwrócony obiekt nie jest obiektem i nie jest śledzony przez kontekst. Zwrócony obiekt nie ma również wszystkie relacje z zestawu do innych obiektów.  

Sklonowany obiekt może być przydatna podczas rozwiązywania problemów związanych z równoczesnych aktualizacji w bazie danych, szczególnie gdy interfejs użytkownika, który obejmuje powiązywanie danych do obiektów określonego typu jest używany.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>Pobieranie i ustawianie bieżącej lub oryginalnej wartości właściwości złożonych  

Wartość cały obiekt złożony można odczytu i zestawu przy użyciu metody właściwości tak, jak można uzyskać właściwość typu pierwotnego. Ponadto możesz przejść do szczegółów obiektu złożonego i odczytu lub ustaw właściwości tego obiektu lub zagnieżdżony obiekt. Oto kilka przykładów:  

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

Użyj właściwości OriginalValue zamiast właściwości CurrentValue, aby pobrać lub ustawić oryginalnej wartości.  

Należy pamiętać, że właściwość lub metoda ComplexProperty może służyć do dostępu do właściwości złożonej. Jednak metoda ComplexProperty należy użyć, jeśli chcesz przejść do obiektu złożonego za pomocą dodatkowych właściwości lub ComplexProperty wywołuje.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>Za pomocą DbPropertyValues uzyskiwania dostępu do właściwości złożonych  

Jeśli używasz CurrentValues, OriginalValues lub GetDatabaseValues można pobrać wszystkie bieżące, oryginalnym lub wartości bazy danych dla jednostki, wartości wszystkich właściwości złożone są zwracane jako obiekty zagnieżdżone DbPropertyValues. Te zagnieżdżone obiekty mogą następnie służyć do uzyskiwania wartości obiektu złożonego. Na przykład poniższa metoda zostanie wydrukowana wartości wszystkich właściwości, w tym wartości wszelkich złożone i zagnieżdżonych właściwości złożonych.  

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

Aby wydrukować wszystkie bieżące wartości właściwości metoda będzie wywołana następująco:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
