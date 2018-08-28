---
title: Obsługa konfliktów współbieżności — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: f233af217287dd6bf35e5b7fea8e44974168b312
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997813"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="b175d-102">Obsługa konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="b175d-102">Handling Concurrency Conflicts</span></span>
<span data-ttu-id="b175d-103">Podjęto próbę zapisać jednostkę w bazie danych w niej danych nie zmienił się od jednostki mamy nadzieję, że został załadowany oznacza, że optymistycznie polega na współbieżność.</span><span class="sxs-lookup"><span data-stu-id="b175d-103">Optimistic concurrency involves optimistically attempting to save your entity to the database in the hope that the data there has not changed since the entity was loaded.</span></span> <span data-ttu-id="b175d-104">Jeśli okazuje się, że dane zostały zmienione, a następnie zostanie zgłoszony wyjątek i należy rozwiązać konflikt, zanim spróbujesz ponownie zapisać.</span><span class="sxs-lookup"><span data-stu-id="b175d-104">If it turns out that the data has changed then an exception is thrown and you must resolve the conflict before attempting to save again.</span></span> <span data-ttu-id="b175d-105">W tym temacie omówiono sposób obsługi wyjątków w programie Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b175d-105">This topic covers how to handle such exceptions in Entity Framework.</span></span> <span data-ttu-id="b175d-106">Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="b175d-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="b175d-107">Ten wpis nie jest właściwym miejscem dla pełne omówienie optymistycznej współbieżności.</span><span class="sxs-lookup"><span data-stu-id="b175d-107">This post is not the appropriate place for a full discussion of optimistic concurrency.</span></span> <span data-ttu-id="b175d-108">Poniższe sekcje zakładają pewną wiedzę na temat rozpoznawania współbieżności i wyświetlanie wzorców do wykonywania typowych zadań.</span><span class="sxs-lookup"><span data-stu-id="b175d-108">The sections below assume some knowledge of concurrency resolution and show patterns for common tasks.</span></span>  

<span data-ttu-id="b175d-109">Wiele z tych wzorców wprowadzić korzystanie z tematów omówionych w [Praca z wartościami właściwości](~/ef6/saving/change-tracking/property-values.md).</span><span class="sxs-lookup"><span data-stu-id="b175d-109">Many of these patterns make use of the topics discussed in [Working with Property Values](~/ef6/saving/change-tracking/property-values.md).</span></span>  

<span data-ttu-id="b175d-110">Rozwiązywanie problemów współbieżności, gdy używana jest niezależne skojarzenia (gdzie klucza obcego nie została zamapowana na właściwość w jednostce) jest znacznie trudniejsze niż w przypadku używania powiązań kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="b175d-110">Resolving concurrency issues when you are using independent associations (where the foreign key is not mapped to a property in your entity) is much more difficult than when you are using foreign key associations.</span></span> <span data-ttu-id="b175d-111">W związku z tym jeśli czy rozdzielczość współbieżności w aplikacji zalecane jest zawsze mapy kluczy obcych do jednostki.</span><span class="sxs-lookup"><span data-stu-id="b175d-111">Therefore if you are going to do concurrency resolution in your application it is advised that you always map foreign keys into your entities.</span></span> <span data-ttu-id="b175d-112">Poniższe przykłady założono, że używasz skojarzenia klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="b175d-112">All the examples below assume that you are using foreign key associations.</span></span>  

<span data-ttu-id="b175d-113">DbUpdateConcurrencyException jest generowany przez SaveChanges po wykryciu wyjątek optymistycznej współbieżności podczas próby zapisania jednostki, która używa skojarzenia klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="b175d-113">A DbUpdateConcurrencyException is thrown by SaveChanges when an optimistic concurrency exception is detected while attempting to save an entity that uses foreign key associations.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a><span data-ttu-id="b175d-114">Rozwiązywanie wyjątków optymistycznej współbieżności za pomocą Reload (bazy danych wins)</span><span class="sxs-lookup"><span data-stu-id="b175d-114">Resolving optimistic concurrency exceptions with Reload (database wins)</span></span>  

<span data-ttu-id="b175d-115">Metoda ponowne załadowanie może służyć do zastąpienie bieżących wartości jednostki przy użyciu wartości teraz w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b175d-115">The Reload method can be used to overwrite the current values of the entity with the values now in the database.</span></span> <span data-ttu-id="b175d-116">Jednostka następnie zwykle znajduje się wstecz do użytkownika w pewnej postaci i użytkownik próbuje ponownie wprowadzić swoje zmiany i Zapisz ponownie.</span><span class="sxs-lookup"><span data-stu-id="b175d-116">The entity is then typically given back to the user in some form and they must try to make their changes again and re-save.</span></span> <span data-ttu-id="b175d-117">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b175d-117">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;

        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update the values of the entity that failed to save from the store
            ex.Entries.Single().Reload();
        }

    } while (saveFailed);
}
```  

<span data-ttu-id="b175d-118">Dobrym sposobem, aby zasymulować wyjątku współbieżności jest Ustaw punkt przerwania w wywołaniu funkcji SaveChanges, a następnie zmodyfikuj jednostki, do której jest zapisywany w bazie danych przy użyciu innego narzędzia, takie jak program SQL Management Studio.</span><span class="sxs-lookup"><span data-stu-id="b175d-118">A good way to simulate a concurrency exception is to set a breakpoint on the SaveChanges call and then modify an entity that is being saved in the database using another tool such as SQL Management Studio.</span></span> <span data-ttu-id="b175d-119">Można także wstawić wiersza przed SaveChanges można zaktualizować bazy danych bezpośrednio przy użyciu klasy SqlCommand.</span><span class="sxs-lookup"><span data-stu-id="b175d-119">You could also insert a line before SaveChanges to update the database directly using SqlCommand.</span></span> <span data-ttu-id="b175d-120">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b175d-120">For example:</span></span>  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

<span data-ttu-id="b175d-121">Metoda wpisy na DbUpdateConcurrencyException zwraca DbEntityEntry wystąpień jednostki, których nie można zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="b175d-121">The Entries method on DbUpdateConcurrencyException returns the DbEntityEntry instances for the entities that failed to update.</span></span> <span data-ttu-id="b175d-122">(Ta właściwość obecnie zawsze zwraca pojedynczej wartości dla współbieżności problemy.</span><span class="sxs-lookup"><span data-stu-id="b175d-122">(This property currently always returns a single value for concurrency issues.</span></span> <span data-ttu-id="b175d-123">Jego może zwracać wiele wartości dla Wyjątki ogólne aktualizacji.) Zamiast w niektórych sytuacjach może być można pobrać Wpisy dla wszystkich obiektów, które mogą być wymagane ponowne załadowanie z bazy danych, i wywołanie Załaduj ponownie dla każdego z nich.</span><span class="sxs-lookup"><span data-stu-id="b175d-123">It may return multiple values for general update exceptions.) An alternative for some situations might be to get entries for all entities that may need to be reloaded from the database and call reload for each of these.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a><span data-ttu-id="b175d-124">Rozwiązywania wyjątków optymistycznej współbieżności jako klienta usługi wins</span><span class="sxs-lookup"><span data-stu-id="b175d-124">Resolving optimistic concurrency exceptions as client wins</span></span>  

<span data-ttu-id="b175d-125">W powyższym przykładzie używającej ponowne ładowanie jest czasami nazywane bazy danych wins lub magazynu usługi wins, ponieważ wartości w jednostce są zastępowane przez wartości z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b175d-125">The example above that uses Reload is sometimes called database wins or store wins because the values in the entity are overwritten by values from the database.</span></span> <span data-ttu-id="b175d-126">Czasami możesz nie przeciwieństwo i Zastąp wartości w bazie danych z wartościami, które aktualnie w jednostce.</span><span class="sxs-lookup"><span data-stu-id="b175d-126">Sometimes you may wish to do the opposite and overwrite the values in the database with the values currently in the entity.</span></span> <span data-ttu-id="b175d-127">To jest czasami nazywane wins klienta i może odbywać się przez uzyskanie wartości bieżącej bazy danych i ustawisz je jako oryginalnych wartości jednostki.</span><span class="sxs-lookup"><span data-stu-id="b175d-127">This is sometimes called client wins and can be done by getting the current database values and setting them as the original values for the entity.</span></span> <span data-ttu-id="b175d-128">(Zobacz [Praca z wartościami właściwości](~/ef6/saving/change-tracking/property-values.md) uzyskać informacji na temat bieżących i oryginalnych wartości.) Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b175d-128">(See [Working with Property Values](~/ef6/saving/change-tracking/property-values.md) for information on current and original values.) For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update original values from the database
            var entry = ex.Entries.Single();
            entry.OriginalValues.SetValues(entry.GetDatabaseValues());
        }

    } while (saveFailed);
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a><span data-ttu-id="b175d-129">Niestandardowe rozwiązanie wyjątki optymistycznej współbieżności</span><span class="sxs-lookup"><span data-stu-id="b175d-129">Custom resolution of optimistic concurrency exceptions</span></span>  

<span data-ttu-id="b175d-130">Czasami można połączyć ze sobą wartości obecnie w bazie danych z wartościami, które aktualnie w jednostce.</span><span class="sxs-lookup"><span data-stu-id="b175d-130">Sometimes you may want to combine the values currently in the database with the values currently in the entity.</span></span> <span data-ttu-id="b175d-131">Wymaga to zazwyczaj kilka niestandardowych interakcji logiki lub użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b175d-131">This usually requires some custom logic or user interaction.</span></span> <span data-ttu-id="b175d-132">Na przykład mogą stanowić formularz do użytkownika, zawierającą bieżące wartości, wartości w bazie danych, a domyślny zestaw rozwiązane wartości.</span><span class="sxs-lookup"><span data-stu-id="b175d-132">For example, you might present a form to the user containing the current values, the values in the database, and a default set of resolved values.</span></span> <span data-ttu-id="b175d-133">Użytkownik może następnie edytować rozwiązane wartości zgodnie z potrzebami i byłoby te rozwiązania wartości, które są zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b175d-133">The user would then edit the resolved values as necessary and it would be these resolved values that get saved to the database.</span></span> <span data-ttu-id="b175d-134">Można to zrobić przy użyciu obiektów DbPropertyValues zwrócony z CurrentValues i GetDatabaseValues przy uruchamianiu jednostki.</span><span class="sxs-lookup"><span data-stu-id="b175d-134">This can be done using the DbPropertyValues objects returned from CurrentValues and GetDatabaseValues on the entity’s entry.</span></span> <span data-ttu-id="b175d-135">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b175d-135">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            var entry = ex.Entries.Single();
            var currentValues = entry.CurrentValues;
            var databaseValues = entry.GetDatabaseValues();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValues = databaseValues.Clone();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency(currentValues, databaseValues, resolvedValues);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValues);
        }
    } while (saveFailed);
}

public void HaveUserResolveConcurrency(DbPropertyValues currentValues,
                                       DbPropertyValues databaseValues,
                                       DbPropertyValues resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them edit the resolved values to get the correct resolution.
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a><span data-ttu-id="b175d-136">Niestandardowe rozwiązanie wyjątki optymistycznej współbieżności przy użyciu obiektów</span><span class="sxs-lookup"><span data-stu-id="b175d-136">Custom resolution of optimistic concurrency exceptions using objects</span></span>  

<span data-ttu-id="b175d-137">Powyższy kod używa wystąpienia DbPropertyValues przekazywanie wokół bieżącego, bazy danych i rozwiązane wartości.</span><span class="sxs-lookup"><span data-stu-id="b175d-137">The code above uses DbPropertyValues instances for passing around current, database, and resolved values.</span></span> <span data-ttu-id="b175d-138">Czasami może być łatwiejsze do użycia wystąpienia tego typu jednostki dla tego.</span><span class="sxs-lookup"><span data-stu-id="b175d-138">Sometimes it may be easier to use instances of your entity type for this.</span></span> <span data-ttu-id="b175d-139">Można to zrobić przy użyciu metod ToObject i SetValues DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="b175d-139">This can be done using the ToObject and SetValues methods of DbPropertyValues.</span></span> <span data-ttu-id="b175d-140">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b175d-140">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            // as instances of the entity type
            var entry = ex.Entries.Single();
            var databaseValues = entry.GetDatabaseValues();
            var databaseValuesAsBlog = (Blog)databaseValues.ToObject();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValuesAsBlog = (Blog)databaseValues.ToObject();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency((Blog)entry.Entity,
                                       databaseValuesAsBlog,
                                       resolvedValuesAsBlog);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValuesAsBlog);
        }

    } while (saveFailed);
}

public void HaveUserResolveConcurrency(Blog entity,
                                       Blog databaseValues,
                                       Blog resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them update the resolved values to get the correct resolution.
}
```  
