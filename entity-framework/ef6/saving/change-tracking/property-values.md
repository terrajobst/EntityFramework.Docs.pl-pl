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
# <a name="working-with-property-values"></a><span data-ttu-id="00c5a-102">Praca z wartościami właściwości</span><span class="sxs-lookup"><span data-stu-id="00c5a-102">Working with property values</span></span>
<span data-ttu-id="00c5a-103">W przypadku większości Entity Framework należy zachować ostrożność śledzenia stanu, oryginalnych wartości i bieżących wartości właściwości wystąpień jednostek.</span><span class="sxs-lookup"><span data-stu-id="00c5a-103">For the most part Entity Framework will take care of tracking the state, original values, and current values of the properties of your entity instances.</span></span> <span data-ttu-id="00c5a-104">Mogą jednak wystąpić sytuacje — takie jak scenariusze rozłączenia — w przypadku, gdy chcesz wyświetlić informacje o właściwościach i manipulować nimi.</span><span class="sxs-lookup"><span data-stu-id="00c5a-104">However, there may be some cases - such as disconnected scenarios - where you want to view or manipulate the information EF has about the properties.</span></span> <span data-ttu-id="00c5a-105">Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="00c5a-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="00c5a-106">Entity Framework śledzi dwie wartości dla każdej właściwości śledzonej jednostki.</span><span class="sxs-lookup"><span data-stu-id="00c5a-106">Entity Framework keeps track of two values for each property of a tracked entity.</span></span> <span data-ttu-id="00c5a-107">Bieżąca wartość to, jak nazwa wskazuje bieżącą wartość właściwości w jednostce.</span><span class="sxs-lookup"><span data-stu-id="00c5a-107">The current value is, as the name indicates, the current value of the property in the entity.</span></span> <span data-ttu-id="00c5a-108">Oryginalna wartość jest wartością, którą Właściwość miała w czasie, gdy zapytanie o jednostkę z bazy danych lub dołączono do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="00c5a-108">The original value is the value that the property had when the entity was queried from the database or attached to the context.</span></span>  

<span data-ttu-id="00c5a-109">Istnieją dwa ogólne mechanizmy pracy z wartościami właściwości:</span><span class="sxs-lookup"><span data-stu-id="00c5a-109">There are two general mechanisms for working with property values:</span></span>  

- <span data-ttu-id="00c5a-110">Wartość pojedynczej właściwości można uzyskać w jednoznacznie określonym sposobie przy użyciu metody właściwości.</span><span class="sxs-lookup"><span data-stu-id="00c5a-110">The value of a single property can be obtained in a strongly typed way using the Property method.</span></span>  
- <span data-ttu-id="00c5a-111">Wartości wszystkich właściwości jednostki można odczytać w obiekcie dbpropertyvalues.</span><span class="sxs-lookup"><span data-stu-id="00c5a-111">Values for all properties of an entity can be read into a DbPropertyValues object.</span></span> <span data-ttu-id="00c5a-112">Dbpropertyvalues następnie działa jako obiekt podobny do słownika, aby umożliwić odczytywanie i ustawianie wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="00c5a-112">DbPropertyValues then acts as a dictionary-like object to allow property values to be read and set.</span></span> <span data-ttu-id="00c5a-113">Wartości w obiekcie dbpropertyvalue można ustawić na podstawie wartości w innych obiektach dbpropertyvalue lub z wartości w innym obiekcie, takich jak inna kopia jednostki lub obiekt prostego transferu danych (DTO).</span><span class="sxs-lookup"><span data-stu-id="00c5a-113">The values in a DbPropertyValues object can be set from values in another DbPropertyValues object or from values in some other object, such as another copy of the entity or a simple data transfer object (DTO).</span></span>  

<span data-ttu-id="00c5a-114">W poniższych sekcjach przedstawiono przykłady użycia powyższych mechanizmów.</span><span class="sxs-lookup"><span data-stu-id="00c5a-114">The sections below show examples of using both of the above mechanisms.</span></span>  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a><span data-ttu-id="00c5a-115">Pobieranie i Ustawianie bieżącej lub oryginalnej wartości pojedynczej właściwości</span><span class="sxs-lookup"><span data-stu-id="00c5a-115">Getting and setting the current or original value of an individual property</span></span>  

<span data-ttu-id="00c5a-116">W poniższym przykładzie pokazano, jak można odczytać bieżącą wartość właściwości, a następnie ustawić nową wartość:</span><span class="sxs-lookup"><span data-stu-id="00c5a-116">The example below shows how the current value of a property can be read and then set to a new value:</span></span>  

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

<span data-ttu-id="00c5a-117">Użyj właściwości OriginalValue zamiast właściwości CurrentValue, aby odczytać lub ustawić oryginalną wartość.</span><span class="sxs-lookup"><span data-stu-id="00c5a-117">Use the OriginalValue property instead of the CurrentValue property to read or set the original value.</span></span>  

<span data-ttu-id="00c5a-118">Zwróć uwagę, że zwracana wartość jest wpisana jako "Object", gdy do określenia nazwy właściwości zostanie użyty ciąg.</span><span class="sxs-lookup"><span data-stu-id="00c5a-118">Note that the returned value is typed as “object” when a string is used to specify the property name.</span></span> <span data-ttu-id="00c5a-119">Z drugiej strony zwracana wartość jest silnie wpisana, jeśli jest używane wyrażenie lambda.</span><span class="sxs-lookup"><span data-stu-id="00c5a-119">On the other hand, the returned value is strongly typed if a lambda expression is used.</span></span>  

<span data-ttu-id="00c5a-120">Ustawienie wartości właściwości w taki sposób spowoduje oznaczenie właściwości jako modyfikowanej, jeśli nowa wartość różni się od starej wartości.</span><span class="sxs-lookup"><span data-stu-id="00c5a-120">Setting the property value like this will only mark the property as modified if the new value is different from the old value.</span></span>  

<span data-ttu-id="00c5a-121">Gdy wartość właściwości jest ustawiona w ten sposób, zmiana jest wykrywana automatycznie, nawet jeśli AutoDetectChanges jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="00c5a-121">When a property value is set in this way the change is automatically detected even if AutoDetectChanges is turned off.</span></span>  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a><span data-ttu-id="00c5a-122">Pobieranie i Ustawianie bieżącej wartości niezamapowanej właściwości</span><span class="sxs-lookup"><span data-stu-id="00c5a-122">Getting and setting the current value of an unmapped property</span></span>  

<span data-ttu-id="00c5a-123">Nie można również odczytać bieżącej wartości właściwości, która nie jest zamapowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="00c5a-123">The current value of a property that is not mapped to the database can also be read.</span></span> <span data-ttu-id="00c5a-124">Przykładem niezamapowanej właściwości może być Właściwość RssLink w blogu.</span><span class="sxs-lookup"><span data-stu-id="00c5a-124">An example of an unmapped property could be an RssLink property on Blog.</span></span> <span data-ttu-id="00c5a-125">Ta wartość może być obliczana w oparciu o BlogId i dlatego nie musi być przechowywana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="00c5a-125">This value may be calculated based on the BlogId, and therefore doesn't need to be stored in the database.</span></span> <span data-ttu-id="00c5a-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="00c5a-126">For example:</span></span>  

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

<span data-ttu-id="00c5a-127">Bieżącą wartość można również ustawić, jeśli właściwość uwidacznia metodę ustawiającą.</span><span class="sxs-lookup"><span data-stu-id="00c5a-127">The current value can also be set if the property exposes a setter.</span></span>  

<span data-ttu-id="00c5a-128">Odczytywanie wartości niezamapowanych właściwości jest przydatne podczas przeprowadzania Entity Framework weryfikacji niezamapowanych właściwości.</span><span class="sxs-lookup"><span data-stu-id="00c5a-128">Reading the values of unmapped properties is useful when performing Entity Framework validation of unmapped properties.</span></span> <span data-ttu-id="00c5a-129">Z tego samego powodu bieżące wartości można odczytać i ustawić dla właściwości jednostek, które nie są aktualnie śledzone przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="00c5a-129">For the same reason current values can be read and set for properties of entities that are not currently being tracked by the context.</span></span> <span data-ttu-id="00c5a-130">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="00c5a-130">For example:</span></span>  

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

<span data-ttu-id="00c5a-131">Należy zauważyć, że oryginalne wartości nie są dostępne dla niezamapowanych właściwości lub dla właściwości jednostek, które nie są śledzone przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="00c5a-131">Note that original values are not available for unmapped properties or for properties of entities that are not being tracked by the context.</span></span>  

## <a name="checking-whether-a-property-is-marked-as-modified"></a><span data-ttu-id="00c5a-132">Sprawdzanie, czy właściwość jest oznaczona jako zmodyfikowana</span><span class="sxs-lookup"><span data-stu-id="00c5a-132">Checking whether a property is marked as modified</span></span>  

<span data-ttu-id="00c5a-133">W poniższym przykładzie pokazano, jak sprawdzić, czy dana właściwość jest oznaczona jako zmodyfikowano:</span><span class="sxs-lookup"><span data-stu-id="00c5a-133">The example below shows how to check whether or not an individual property is marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

<span data-ttu-id="00c5a-134">Wartości zmodyfikowanych właściwości są wysyłane jako aktualizacje bazy danych po wywołaniu metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="00c5a-134">The values of modified properties are sent as updates to the database when SaveChanges is called.</span></span>  

##  <a name="marking-a-property-as-modified"></a><span data-ttu-id="00c5a-135">Oznaczanie właściwości jako zmodyfikowane</span><span class="sxs-lookup"><span data-stu-id="00c5a-135">Marking a property as modified</span></span>  

<span data-ttu-id="00c5a-136">W poniższym przykładzie pokazano, jak wymusić oznaczenie pojedynczej właściwości jako zmodyfikowanej:</span><span class="sxs-lookup"><span data-stu-id="00c5a-136">The example below shows how to force an individual property to be marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

<span data-ttu-id="00c5a-137">Oznaczenie właściwości jako zmodyfikowanej powoduje, że aktualizacja ma być wysyłana do bazy danych dla właściwości, gdy metody SaveChanges jest wywoływana, nawet jeśli bieżąca wartość właściwości jest taka sama jak oryginalna wartość.</span><span class="sxs-lookup"><span data-stu-id="00c5a-137">Marking a property as modified forces an update to be send to the database for the property when SaveChanges is called even if the current value of the property is the same as its original value.</span></span>  

<span data-ttu-id="00c5a-138">Obecnie nie można zresetować pojedynczej właściwości, aby nie była modyfikowana po oznaczeniu jej jako zmodyfikowana.</span><span class="sxs-lookup"><span data-stu-id="00c5a-138">It is not currently possible to reset an individual property to be not modified after it has been marked as modified.</span></span> <span data-ttu-id="00c5a-139">Jest to coś, co planujemy obsłużyć w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="00c5a-139">This is something we plan to support in a future release.</span></span>  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a><span data-ttu-id="00c5a-140">Odczytywanie bieżących, oryginalnych i wartości bazy danych dla wszystkich właściwości jednostki</span><span class="sxs-lookup"><span data-stu-id="00c5a-140">Reading current, original, and database values for all properties of an entity</span></span>  

<span data-ttu-id="00c5a-141">W poniższym przykładzie pokazano, jak odczytać bieżące wartości, oryginalne wartości i wartości w rzeczywistości w bazie danych dla wszystkich zamapowanych właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="00c5a-141">The example below shows how to read the current values, the original values, and the values actually in the database for all mapped properties of an entity.</span></span>  

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

<span data-ttu-id="00c5a-142">Bieżące wartości to wartości, które obecnie zawierają właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="00c5a-142">The current values are the values that the properties of the entity currently contain.</span></span> <span data-ttu-id="00c5a-143">Pierwotne wartości to wartości, które zostały odczytane z bazy danych podczas wykonywania zapytania dotyczącego jednostki.</span><span class="sxs-lookup"><span data-stu-id="00c5a-143">The original values are the values that were read from the database when the entity was queried.</span></span> <span data-ttu-id="00c5a-144">Wartości bazy danych są wartościami, które są obecnie przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="00c5a-144">The database values are the values as they are currently stored in the database.</span></span> <span data-ttu-id="00c5a-145">Pobieranie wartości bazy danych jest przydatne, gdy wartości w bazie danych mogły ulec zmianie od momentu, w którym dana kwerenda została utworzona przez innego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="00c5a-145">Getting the database values is useful when the values in the database may have changed since the entity was queried such as when a concurrent edit to the database has been made by another user.</span></span>  

## <a name="setting-current-or-original-values-from-another-object"></a><span data-ttu-id="00c5a-146">Ustawianie bieżących lub oryginalnych wartości z innego obiektu</span><span class="sxs-lookup"><span data-stu-id="00c5a-146">Setting current or original values from another object</span></span>  

<span data-ttu-id="00c5a-147">Bieżące lub oryginalne wartości śledzonej jednostki można aktualizować przez kopiowanie wartości z innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="00c5a-147">The current or original values of a tracked entity can be updated by copying values from another object.</span></span> <span data-ttu-id="00c5a-148">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="00c5a-148">For example:</span></span>  

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

<span data-ttu-id="00c5a-149">Uruchomienie kodu powyżej spowoduje wydrukowanie:</span><span class="sxs-lookup"><span data-stu-id="00c5a-149">Running the code above will print out:</span></span>  

```console
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

<span data-ttu-id="00c5a-150">Ta technika jest czasami stosowana podczas aktualizowania jednostki przy użyciu wartości uzyskanych z wywołania usługi lub klienta w aplikacji n-warstwowej.</span><span class="sxs-lookup"><span data-stu-id="00c5a-150">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="00c5a-151">Należy zauważyć, że użyty obiekt nie musi być tego samego typu co jednostka, tak długo, jak ma właściwości, których nazwy są zgodne z tymi, które są takie same.</span><span class="sxs-lookup"><span data-stu-id="00c5a-151">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="00c5a-152">W powyższym przykładzie wystąpienie elementu BlogDTO jest używane do aktualizowania oryginalnych wartości.</span><span class="sxs-lookup"><span data-stu-id="00c5a-152">In the example above, an instance of BlogDTO is used to update the original values.</span></span>  

<span data-ttu-id="00c5a-153">Należy pamiętać, że tylko właściwości, które są ustawione na różne wartości, gdy skopiowane z innego obiektu zostaną oznaczone jako zmodyfikowane.</span><span class="sxs-lookup"><span data-stu-id="00c5a-153">Note that only properties that are set to different values when copied from the other object will be marked as modified.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="00c5a-154">Ustawianie bieżących lub oryginalnych wartości ze słownika</span><span class="sxs-lookup"><span data-stu-id="00c5a-154">Setting current or original values from a dictionary</span></span>  

<span data-ttu-id="00c5a-155">Bieżące lub oryginalne wartości śledzonej jednostki można aktualizować przez kopiowanie wartości ze słownika lub innej struktury danych.</span><span class="sxs-lookup"><span data-stu-id="00c5a-155">The current or original values of a tracked entity can be updated by copying values from a dictionary or some other data structure.</span></span> <span data-ttu-id="00c5a-156">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="00c5a-156">For example:</span></span>  

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

<span data-ttu-id="00c5a-157">Użyj właściwości OriginalValues zamiast właściwości CurrentValues, aby ustawić oryginalne wartości.</span><span class="sxs-lookup"><span data-stu-id="00c5a-157">Use the OriginalValues property instead of the CurrentValues property to set original values.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a><span data-ttu-id="00c5a-158">Ustawianie bieżących lub oryginalnych wartości ze słownika przy użyciu właściwości</span><span class="sxs-lookup"><span data-stu-id="00c5a-158">Setting current or original values from a dictionary using Property</span></span>  

<span data-ttu-id="00c5a-159">Alternatywą dla korzystania z CurrentValues lub OriginalValues, jak pokazano powyżej, jest użycie metody właściwości w celu ustawienia wartości każdej właściwości.</span><span class="sxs-lookup"><span data-stu-id="00c5a-159">An alternative to using CurrentValues or OriginalValues as shown above is to use the Property method to set the value of each property.</span></span> <span data-ttu-id="00c5a-160">Może to być preferowany, gdy trzeba ustawić wartości właściwości złożonych.</span><span class="sxs-lookup"><span data-stu-id="00c5a-160">This can be preferable when you need to set the values of complex properties.</span></span> <span data-ttu-id="00c5a-161">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="00c5a-161">For example:</span></span>  

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

<span data-ttu-id="00c5a-162">W powyższym przykładzie można uzyskać dostęp do właściwości złożonych przy użyciu kropkowanych nazw.</span><span class="sxs-lookup"><span data-stu-id="00c5a-162">In the example above complex properties are accessed using dotted names.</span></span> <span data-ttu-id="00c5a-163">Aby uzyskać informacje o innych sposobach uzyskiwania dostępu do właściwości złożonych, zobacz dwie sekcje w dalszej części tego tematu w odniesieniu do właściwości złożonych.</span><span class="sxs-lookup"><span data-stu-id="00c5a-163">For other ways to access complex properties see the two sections later in this topic specifically about complex properties.</span></span>  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="00c5a-164">Tworzenie sklonowanego obiektu zawierającego bieżące, oryginalne lub wartości bazy danych</span><span class="sxs-lookup"><span data-stu-id="00c5a-164">Creating a cloned object containing current, original, or database values</span></span>  

<span data-ttu-id="00c5a-165">Obiekt dbpropertyvalue zwrócony z CurrentValues, OriginalValues lub GetDatabaseValues może służyć do tworzenia klonu jednostki.</span><span class="sxs-lookup"><span data-stu-id="00c5a-165">The DbPropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity.</span></span> <span data-ttu-id="00c5a-166">Ten klon będzie zawierać wartości właściwości z obiektu dbpropertyvalue użytego do jego utworzenia.</span><span class="sxs-lookup"><span data-stu-id="00c5a-166">This clone will contain the property values from the DbPropertyValues object used to create it.</span></span> <span data-ttu-id="00c5a-167">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="00c5a-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

<span data-ttu-id="00c5a-168">Zwróć uwagę, że zwrócony obiekt nie jest jednostką i nie jest śledzony przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="00c5a-168">Note that the object returned is not the entity and is not being tracked by the context.</span></span> <span data-ttu-id="00c5a-169">Zwrócony obiekt nie ma również żadnych relacji ustawionych na inne obiekty.</span><span class="sxs-lookup"><span data-stu-id="00c5a-169">The returned object also does not have any relationships set to other objects.</span></span>  

<span data-ttu-id="00c5a-170">Sklonowany obiekt może być przydatny do rozwiązywania problemów dotyczących współbieżnych aktualizacji bazy danych, szczególnie w przypadku używania interfejsu użytkownika, który obejmuje powiązanie danych z obiektami określonego typu.</span><span class="sxs-lookup"><span data-stu-id="00c5a-170">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially where a UI that involves data binding to objects of a certain type is being used.</span></span>  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a><span data-ttu-id="00c5a-171">Pobieranie i Ustawianie bieżących lub oryginalnych wartości właściwości złożonych</span><span class="sxs-lookup"><span data-stu-id="00c5a-171">Getting and setting the current or original values of complex properties</span></span>  

<span data-ttu-id="00c5a-172">Wartość całego obiektu złożonego można odczytać i ustawić przy użyciu metody właściwości tak samo, jak może być właściwością pierwotną.</span><span class="sxs-lookup"><span data-stu-id="00c5a-172">The value of an entire complex object can be read and set using the Property method just as it can be for a primitive property.</span></span> <span data-ttu-id="00c5a-173">Ponadto możesz przejść do szczegółów obiektu złożonego i odczytać lub ustawić właściwości tego obiektu, a nawet zagnieżdżony obiekt.</span><span class="sxs-lookup"><span data-stu-id="00c5a-173">In addition you can drill down into the complex object and read or set properties of that object, or even a nested object.</span></span> <span data-ttu-id="00c5a-174">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="00c5a-174">Here are some examples:</span></span>  

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

<span data-ttu-id="00c5a-175">Użyj właściwości OriginalValue zamiast właściwości CurrentValue, aby pobrać lub ustawić oryginalną wartość.</span><span class="sxs-lookup"><span data-stu-id="00c5a-175">Use the OriginalValue property instead of the CurrentValue property to get or set an original value.</span></span>  

<span data-ttu-id="00c5a-176">Należy zauważyć, że właściwość lub metoda ComplexProperty może być używana w celu uzyskania dostępu do właściwości złożonej.</span><span class="sxs-lookup"><span data-stu-id="00c5a-176">Note that either the Property or the ComplexProperty method can be used to access a complex property.</span></span> <span data-ttu-id="00c5a-177">Jednak Metoda ComplexProperty musi zostać użyta, jeśli chcesz przejść do obiektu złożonego z dodatkowymi wywołaniami właściwości lub ComplexProperty.</span><span class="sxs-lookup"><span data-stu-id="00c5a-177">However, the ComplexProperty method must be used if you wish to drill down into the complex object with additional Property or ComplexProperty calls.</span></span>  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a><span data-ttu-id="00c5a-178">Korzystanie z funkcji dbpropertyvalues w celu uzyskania dostępu do właściwości złożonych</span><span class="sxs-lookup"><span data-stu-id="00c5a-178">Using DbPropertyValues to access complex properties</span></span>  

<span data-ttu-id="00c5a-179">W przypadku korzystania z CurrentValues, OriginalValues lub GetDatabaseValues w celu uzyskania wszystkich bieżących, oryginalnych lub wartości bazy danych dla jednostki, wartości wszelkich właściwości złożonych są zwracane jako zagnieżdżone obiekty dbpropertyvalues.</span><span class="sxs-lookup"><span data-stu-id="00c5a-179">When you use CurrentValues, OriginalValues, or GetDatabaseValues to get all the current, original, or database values for an entity, the values of any complex properties are returned as nested DbPropertyValues objects.</span></span> <span data-ttu-id="00c5a-180">Te obiekty zagnieżdżone można następnie użyć do uzyskania wartości obiektu złożonego.</span><span class="sxs-lookup"><span data-stu-id="00c5a-180">These nested objects can then be used to get values of the complex object.</span></span> <span data-ttu-id="00c5a-181">Na przykład następująca metoda spowoduje wydrukowanie wartości wszystkich właściwości, w tym wartości wszelkich złożonych właściwości i zagnieżdżonych właściwości złożonych.</span><span class="sxs-lookup"><span data-stu-id="00c5a-181">For example, the following method will print out the values of all properties, including values of any complex properties and nested complex properties.</span></span>  

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

<span data-ttu-id="00c5a-182">Aby wydrukować wszystkie bieżące wartości właściwości, Metoda zostałaby wywołana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="00c5a-182">To print out all current property values the method would be called like this:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
