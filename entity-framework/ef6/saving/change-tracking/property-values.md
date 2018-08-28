---
title: Praca z wartości właściwości - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: a9b969950ec7dcfb86a2abc9c8bd6cc24899948c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998306"
---
# <a name="working-with-property-values"></a><span data-ttu-id="26070-102">Praca z wartościami właściwości</span><span class="sxs-lookup"><span data-stu-id="26070-102">Working with property values</span></span>
<span data-ttu-id="26070-103">W większości przypadków zajmie się Entity Framework śledzenia stanu, oryginalne wartości oraz bieżące wartości właściwości wystąpienia jednostki.</span><span class="sxs-lookup"><span data-stu-id="26070-103">For the most part Entity Framework will take care of tracking the state, original values, and current values of the properties of your entity instances.</span></span> <span data-ttu-id="26070-104">Jednak może być pewnych przypadkach — takich jak rozłączonych scenariuszy —, w której chcesz wyświetlić i modyfikować EF ma informacje o właściwościach.</span><span class="sxs-lookup"><span data-stu-id="26070-104">However, there may be some cases - such as disconnected scenarios - where you want to view or manipulate the information EF has about the properties.</span></span> <span data-ttu-id="26070-105">Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="26070-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="26070-106">Entity Framework śledzi informacje o dwóch wartości dla każdej właściwości śledzone jednostki.</span><span class="sxs-lookup"><span data-stu-id="26070-106">Entity Framework keeps track of two values for each property of a tracked entity.</span></span> <span data-ttu-id="26070-107">Bieżąca wartość to, jak wskazuje nazwa, bieżąca wartość właściwości w obiekcie.</span><span class="sxs-lookup"><span data-stu-id="26070-107">The current value is, as the name indicates, the current value of the property in the entity.</span></span> <span data-ttu-id="26070-108">Oryginalną wartość jest wartością, której właściwość jednostki pobieranych z bazy danych lub został dołączony do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="26070-108">The original value is the value that the property had when the entity was queried from the database or attached to the context.</span></span>  

<span data-ttu-id="26070-109">Istnieją dwa mechanizmy ogólne dotyczące pracy z wartości właściwości:</span><span class="sxs-lookup"><span data-stu-id="26070-109">There are two general mechanisms for working with property values:</span></span>  

- <span data-ttu-id="26070-110">Wartość jednej właściwości mogą być uzyskane w silnie typizowany sposób przy użyciu metody właściwości.</span><span class="sxs-lookup"><span data-stu-id="26070-110">The value of a single property can be obtained in a strongly typed way using the Property method.</span></span>  
- <span data-ttu-id="26070-111">Do obiektu DbPropertyValues można odczytać wartości dla wszystkich właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="26070-111">Values for all properties of an entity can be read into a DbPropertyValues object.</span></span> <span data-ttu-id="26070-112">DbPropertyValues następnie działa jako obiekt słownika przypominającej umożliwiające odczyt i ustawianie wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="26070-112">DbPropertyValues then acts as a dictionary-like object to allow property values to be read and set.</span></span> <span data-ttu-id="26070-113">Można ustawić wartości w obiekcie DbPropertyValues z wartości w innym obiekcie DbPropertyValues lub wartości w innym obiekcie, takich jak kopiowanie innej jednostki lub obiektu transferu prostych danych (DTO).</span><span class="sxs-lookup"><span data-stu-id="26070-113">The values in a DbPropertyValues object can be set from values in another DbPropertyValues object or from values in some other object, such as another copy of the entity or a simple data transfer object (DTO).</span></span>  

<span data-ttu-id="26070-114">Poniższe sekcje pokazują przykłady przy użyciu zarówno mechanizmów powyżej.</span><span class="sxs-lookup"><span data-stu-id="26070-114">The sections below show examples of using both of the above mechanisms.</span></span>  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a><span data-ttu-id="26070-115">Pobieranie i ustawianie bieżącej lub oryginalna wartość wybranej właściwości</span><span class="sxs-lookup"><span data-stu-id="26070-115">Getting and setting the current or original value of an individual property</span></span>  

<span data-ttu-id="26070-116">W poniższym przykładzie pokazano, jak można odczytywać i następnie ustawić nową wartość bieżącą wartość właściwości:</span><span class="sxs-lookup"><span data-stu-id="26070-116">The example below shows how the current value of a property can be read and then set to a new value:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(name).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="26070-117">Użyj właściwości OriginalValue zamiast właściwości CurrentValue odczytać lub ustawić oryginalnej wartości.</span><span class="sxs-lookup"><span data-stu-id="26070-117">Use the OriginalValue property instead of the CurrentValue property to read or set the original value.</span></span>  

<span data-ttu-id="26070-118">Należy pamiętać, że zwracana wartość jest wpisana jako "object", gdy ciąg jest używany do określenia nazwy właściwości.</span><span class="sxs-lookup"><span data-stu-id="26070-118">Note that the returned value is typed as “object” when a string is used to specify the property name.</span></span> <span data-ttu-id="26070-119">Z drugiej strony zwrócona wartość zdecydowanie jest wpisane, jeśli wyrażenie lambda jest używany.</span><span class="sxs-lookup"><span data-stu-id="26070-119">On the other hand, the returned value is strongly typed if a lambda expression is used.</span></span>  

<span data-ttu-id="26070-120">Ustawienie wartości właściwości, takie jak to tylko spowoduje oznaczenie właściwości, jak modyfikacji, jeśli nowa wartość jest inna niż poprzednia wartość.</span><span class="sxs-lookup"><span data-stu-id="26070-120">Setting the property value like this will only mark the property as modified if the new value is different from the old value.</span></span>  

<span data-ttu-id="26070-121">Gdy wartość właściwości jest ustawiana w ten sposób zmiany jest wykrywane automatycznie, nawet wtedy, gdy AutoDetectChanges jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="26070-121">When a property value is set in this way the change is automatically detected even if AutoDetectChanges is turned off.</span></span>  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a><span data-ttu-id="26070-122">Pobieranie i ustawianie bieżącej wartości właściwości niezamapowane</span><span class="sxs-lookup"><span data-stu-id="26070-122">Getting and setting the current value of an unmapped property</span></span>  

<span data-ttu-id="26070-123">Bieżąca wartość właściwości, która nie została zamapowana do bazy danych, również mogą być odczytywane.</span><span class="sxs-lookup"><span data-stu-id="26070-123">The current value of a property that is not mapped to the database can also be read.</span></span> <span data-ttu-id="26070-124">Przykład niezamapowane właściwości może być właściwością RssLink na blogu.</span><span class="sxs-lookup"><span data-stu-id="26070-124">An example of an unmapped property could be an RssLink property on Blog.</span></span> <span data-ttu-id="26070-125">Ta wartość może być obliczona w oparciu o BlogId i w związku z tym nie mają być przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="26070-125">This value may be calculated based on the BlogId, and therefore doesn't need to be stored in the database.</span></span> <span data-ttu-id="26070-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="26070-126">For example:</span></span>  

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

<span data-ttu-id="26070-127">Bieżąca wartość można również ustawić, jeśli właściwość udostępnia metody ustawiającej.</span><span class="sxs-lookup"><span data-stu-id="26070-127">The current value can also be set if the property exposes a setter.</span></span>  

<span data-ttu-id="26070-128">Odczytywanie wartości właściwości niezamapowane jest przydatne w przypadku, gdy wykonywanie weryfikacji platformy Entity Framework niezamapowane właściwości.</span><span class="sxs-lookup"><span data-stu-id="26070-128">Reading the values of unmapped properties is useful when performing Entity Framework validation of unmapped properties.</span></span> <span data-ttu-id="26070-129">Z tego samego powodu bieżących wartości może odczytywać i ustawić dla właściwości jednostki, które nie są obecnie są śledzone przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="26070-129">For the same reason current values can be read and set for properties of entities that are not currently being tracked by the context.</span></span> <span data-ttu-id="26070-130">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="26070-130">For example:</span></span>  

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

<span data-ttu-id="26070-131">Pamiętaj, że oryginalne wartości nie są dostępne dla właściwości niezamapowane właściwości jednostki, które nie są śledzone przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="26070-131">Note that original values are not available for unmapped properties or for properties of entities that are not being tracked by the context.</span></span>  

## <a name="checking-whether-a-property-is-marked-as-modified"></a><span data-ttu-id="26070-132">Sprawdzanie, czy właściwość jest oznaczona jako zmodyfikowana</span><span class="sxs-lookup"><span data-stu-id="26070-132">Checking whether a property is marked as modified</span></span>  

<span data-ttu-id="26070-133">W poniższym przykładzie pokazano, jak sprawdzić, czy poszczególne właściwość jest oznaczona jako zmodyfikowana:</span><span class="sxs-lookup"><span data-stu-id="26070-133">The example below shows how to check whether or not an individual property is marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

<span data-ttu-id="26070-134">Wartości zmodyfikowane właściwości są wysyłane jako aktualizacje w bazie danych po wywołaniu funkcji SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="26070-134">The values of modified properties are sent as updates to the database when SaveChanges is called.</span></span>  

##  <a name="marking-a-property-as-modified"></a><span data-ttu-id="26070-135">Oznaczanie jako zmodyfikowana właściwość</span><span class="sxs-lookup"><span data-stu-id="26070-135">Marking a property as modified</span></span>  

<span data-ttu-id="26070-136">W poniższym przykładzie pokazano, jak wymusić pojedynczej właściwości, aby być oznaczona jako zmodyfikowana:</span><span class="sxs-lookup"><span data-stu-id="26070-136">The example below shows how to force an individual property to be marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

<span data-ttu-id="26070-137">Oznaczanie właściwość jako zmodyfikowane wymusza aktualizację można wysyłać do bazy danych dla właściwości po wywołaniu funkcji SaveChanges nawet, jeśli bieżąca wartość właściwości jest taka sama jak oryginalną wartość.</span><span class="sxs-lookup"><span data-stu-id="26070-137">Marking a property as modified forces an update to be send to the database for the property when SaveChanges is called even if the current value of the property is the same as its original value.</span></span>  

<span data-ttu-id="26070-138">Nie jest obecnie możliwe zresetować pojedynczej właściwości nie można zmodyfikować po została oznaczona jako zmodyfikowana.</span><span class="sxs-lookup"><span data-stu-id="26070-138">It is not currently possible to reset an individual property to be not modified after it has been marked as modified.</span></span> <span data-ttu-id="26070-139">Jest to coś, co firma Microsoft planuje się obsługiwać w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="26070-139">This is something we plan to support in a future release.</span></span>  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a><span data-ttu-id="26070-140">Odczytywanie bieżącego, oryginalne i wartości z bazy danych dla wszystkich właściwości jednostki</span><span class="sxs-lookup"><span data-stu-id="26070-140">Reading current, original, and database values for all properties of an entity</span></span>  

<span data-ttu-id="26070-141">W poniższym przykładzie pokazano, jak odczytać bieżące wartości, oryginalne wartości i wartości, które faktycznie w bazie danych dla wszystkich zamapowanych właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="26070-141">The example below shows how to read the current values, the original values, and the values actually in the database for all mapped properties of an entity.</span></span>  

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

<span data-ttu-id="26070-142">Bieżące wartości są wartościami, które obecnie zawiera właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="26070-142">The current values are the values that the properties of the entity currently contain.</span></span> <span data-ttu-id="26070-143">Oryginalne wartości są wartościami, które zostały odczytane z bazy danych, gdy badano jednostki.</span><span class="sxs-lookup"><span data-stu-id="26070-143">The original values are the values that were read from the database when the entity was queried.</span></span> <span data-ttu-id="26070-144">Wartości bazy danych są wartości, ponieważ są one przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="26070-144">The database values are the values as they are currently stored in the database.</span></span> <span data-ttu-id="26070-145">Pobieranie wartości z bazy danych jest przydatne w przypadku, gdy mógł ulec zmianie wartości w bazie danych, ponieważ badano jednostki, takie jak podczas edytowania równoczesne bazy danych zostały dokonane przez innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="26070-145">Getting the database values is useful when the values in the database may have changed since the entity was queried such as when a concurrent edit to the database has been made by another user.</span></span>  

## <a name="setting-current-or-original-values-from-another-object"></a><span data-ttu-id="26070-146">Ustawianie bieżącej lub oryginalnej wartości z innego obiektu</span><span class="sxs-lookup"><span data-stu-id="26070-146">Setting current or original values from another object</span></span>  

<span data-ttu-id="26070-147">Bieżące i oryginalne wartości śledzonych jednostki mogą być aktualizowane przez skopiowanie wartości z innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="26070-147">The current or original values of a tracked entity can be updated by copying values from another object.</span></span> <span data-ttu-id="26070-148">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="26070-148">For example:</span></span>  

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

<span data-ttu-id="26070-149">Uruchamianie kodu powyżej zostanie wydrukowana:</span><span class="sxs-lookup"><span data-stu-id="26070-149">Running the code above will print out:</span></span>  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

<span data-ttu-id="26070-150">Ta technika jest czasami używana podczas aktualizowania jednostki przy użyciu wartości z wywołania usługi lub klienta w aplikacji n-warstwowej.</span><span class="sxs-lookup"><span data-stu-id="26070-150">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="26070-151">Pamiętaj, że obiekt używany nie musi być tego samego typu co podmiot, tak długo, jak długo ma właściwości, których nazwy odpowiadają jednostki.</span><span class="sxs-lookup"><span data-stu-id="26070-151">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="26070-152">W powyższym przykładzie wystąpienie BlogDTO służy do aktualizacji oryginalnych wartości.</span><span class="sxs-lookup"><span data-stu-id="26070-152">In the example above, an instance of BlogDTO is used to update the original values.</span></span>  

<span data-ttu-id="26070-153">Należy zauważyć, że tylko te właściwości, które są ustawione na różnych wartości podczas kopiowania z innego obiektu zostanie oznaczona jako zmodyfikowana.</span><span class="sxs-lookup"><span data-stu-id="26070-153">Note that only properties that are set to different values when copied from the other object will be marked as modified.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="26070-154">Ustawianie bieżącej lub oryginalnej wartości ze słownika</span><span class="sxs-lookup"><span data-stu-id="26070-154">Setting current or original values from a dictionary</span></span>  

<span data-ttu-id="26070-155">Bieżące i oryginalne wartości śledzonych jednostki mogą być aktualizowane przez skopiowanie wartości ze słownika lub inne struktury danych.</span><span class="sxs-lookup"><span data-stu-id="26070-155">The current or original values of a tracked entity can be updated by copying values from a dictionary or some other data structure.</span></span> <span data-ttu-id="26070-156">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="26070-156">For example:</span></span>  

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

<span data-ttu-id="26070-157">Użyj właściwości OriginalValues zamiast właściwości CurrentValues można ustawić oryginalnej wartości.</span><span class="sxs-lookup"><span data-stu-id="26070-157">Use the OriginalValues property instead of the CurrentValues property to set original values.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a><span data-ttu-id="26070-158">Ustawianie bieżącej lub oryginalnej wartości ze słownika przy użyciu właściwości</span><span class="sxs-lookup"><span data-stu-id="26070-158">Setting current or original values from a dictionary using Property</span></span>  

<span data-ttu-id="26070-159">Alternatywa dla użycia CurrentValues lub OriginalValues, jak pokazano powyżej jest użycie metody właściwości można ustawić wartość każdej właściwości.</span><span class="sxs-lookup"><span data-stu-id="26070-159">An alternative to using CurrentValues or OriginalValues as shown above is to use the Property method to set the value of each property.</span></span> <span data-ttu-id="26070-160">Może to być pożądane, gdy należy ustawić wartości właściwości złożonych.</span><span class="sxs-lookup"><span data-stu-id="26070-160">This can be preferable when you need to set the values of complex properties.</span></span> <span data-ttu-id="26070-161">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="26070-161">For example:</span></span>  

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

<span data-ttu-id="26070-162">W przykładzie powyżej złożonych właściwości są dostępne przy użyciu notacji nazw.</span><span class="sxs-lookup"><span data-stu-id="26070-162">In the example above complex properties are accessed using dotted names.</span></span> <span data-ttu-id="26070-163">Inne sposoby dostępu do właściwości złożonych zobacz dwie sekcje w dalszej części tego tematu sobie konkretnie o złożonych właściwości.</span><span class="sxs-lookup"><span data-stu-id="26070-163">For other ways to access complex properties see the two sections later in this topic specifically about complex properties.</span></span>  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="26070-164">Tworzenie Sklonowany obiekt zawierający bieżące, oryginalny lub wartościami bazy danych</span><span class="sxs-lookup"><span data-stu-id="26070-164">Creating a cloned object containing current, original, or database values</span></span>  

<span data-ttu-id="26070-165">Zwróciła obiekt DbPropertyValues CurrentValues, OriginalValues, lub GetDatabaseValues może służyć do tworzenia klonu jednostki.</span><span class="sxs-lookup"><span data-stu-id="26070-165">The DbPropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity.</span></span> <span data-ttu-id="26070-166">Klonuj ten będzie zawierać wartości właściwości z obiektu DbPropertyValues użyty do jego utworzenia.</span><span class="sxs-lookup"><span data-stu-id="26070-166">This clone will contain the property values from the DbPropertyValues object used to create it.</span></span> <span data-ttu-id="26070-167">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="26070-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

<span data-ttu-id="26070-168">Należy pamiętać, że zwrócony obiekt nie jest obiektem i nie jest śledzony przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="26070-168">Note that the object returned is not the entity and is not being tracked by the context.</span></span> <span data-ttu-id="26070-169">Zwrócony obiekt nie ma również wszystkie relacje z zestawu do innych obiektów.</span><span class="sxs-lookup"><span data-stu-id="26070-169">The returned object also does not have any relationships set to other objects.</span></span>  

<span data-ttu-id="26070-170">Sklonowany obiekt może być przydatna podczas rozwiązywania problemów związanych z równoczesnych aktualizacji w bazie danych, szczególnie gdy interfejs użytkownika, który obejmuje powiązywanie danych do obiektów określonego typu jest używany.</span><span class="sxs-lookup"><span data-stu-id="26070-170">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially where a UI that involves data binding to objects of a certain type is being used.</span></span>  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a><span data-ttu-id="26070-171">Pobieranie i ustawianie bieżącej lub oryginalnej wartości właściwości złożonych</span><span class="sxs-lookup"><span data-stu-id="26070-171">Getting and setting the current or original values of complex properties</span></span>  

<span data-ttu-id="26070-172">Wartość cały obiekt złożony można odczytu i zestawu przy użyciu metody właściwości tak, jak można uzyskać właściwość typu pierwotnego.</span><span class="sxs-lookup"><span data-stu-id="26070-172">The value of an entire complex object can be read and set using the Property method just as it can be for a primitive property.</span></span> <span data-ttu-id="26070-173">Ponadto możesz przejść do szczegółów obiektu złożonego i odczytu lub ustaw właściwości tego obiektu lub zagnieżdżony obiekt.</span><span class="sxs-lookup"><span data-stu-id="26070-173">In addition you can drill down into the complex object and read or set properties of that object, or even a nested object.</span></span> <span data-ttu-id="26070-174">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="26070-174">Here are some examples:</span></span>  

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

<span data-ttu-id="26070-175">Użyj właściwości OriginalValue zamiast właściwości CurrentValue, aby pobrać lub ustawić oryginalnej wartości.</span><span class="sxs-lookup"><span data-stu-id="26070-175">Use the OriginalValue property instead of the CurrentValue property to get or set an original value.</span></span>  

<span data-ttu-id="26070-176">Należy pamiętać, że właściwość lub metoda ComplexProperty może służyć do dostępu do właściwości złożonej.</span><span class="sxs-lookup"><span data-stu-id="26070-176">Note that either the Property or the ComplexProperty method can be used to access a complex property.</span></span> <span data-ttu-id="26070-177">Jednak metoda ComplexProperty należy użyć, jeśli chcesz przejść do obiektu złożonego za pomocą dodatkowych właściwości lub ComplexProperty wywołuje.</span><span class="sxs-lookup"><span data-stu-id="26070-177">However, the ComplexProperty method must be used if you wish to drill down into the complex object with additional Property or ComplexProperty calls.</span></span>  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a><span data-ttu-id="26070-178">Za pomocą DbPropertyValues uzyskiwania dostępu do właściwości złożonych</span><span class="sxs-lookup"><span data-stu-id="26070-178">Using DbPropertyValues to access complex properties</span></span>  

<span data-ttu-id="26070-179">Jeśli używasz CurrentValues, OriginalValues lub GetDatabaseValues można pobrać wszystkie bieżące, oryginalnym lub wartości bazy danych dla jednostki, wartości wszystkich właściwości złożone są zwracane jako obiekty zagnieżdżone DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="26070-179">When you use CurrentValues, OriginalValues, or GetDatabaseValues to get all the current, original, or database values for an entity, the values of any complex properties are returned as nested DbPropertyValues objects.</span></span> <span data-ttu-id="26070-180">Te zagnieżdżone obiekty mogą następnie służyć do uzyskiwania wartości obiektu złożonego.</span><span class="sxs-lookup"><span data-stu-id="26070-180">These nested objects can then be used to get values of the complex object.</span></span> <span data-ttu-id="26070-181">Na przykład poniższa metoda zostanie wydrukowana wartości wszystkich właściwości, w tym wartości wszelkich złożone i zagnieżdżonych właściwości złożonych.</span><span class="sxs-lookup"><span data-stu-id="26070-181">For example, the following method will print out the values of all properties, including values of any complex properties and nested complex properties.</span></span>  

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

<span data-ttu-id="26070-182">Aby wydrukować wszystkie bieżące wartości właściwości metoda będzie wywołana następująco:</span><span class="sxs-lookup"><span data-stu-id="26070-182">To print out all current property values the method would be called like this:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
