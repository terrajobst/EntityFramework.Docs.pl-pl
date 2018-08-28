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
# <a name="handling-concurrency-conflicts"></a>Obsługa konfliktów współbieżności
Podjęto próbę zapisać jednostkę w bazie danych w niej danych nie zmienił się od jednostki mamy nadzieję, że został załadowany oznacza, że optymistycznie polega na współbieżność. Jeśli okazuje się, że dane zostały zmienione, a następnie zostanie zgłoszony wyjątek i należy rozwiązać konflikt, zanim spróbujesz ponownie zapisać. W tym temacie omówiono sposób obsługi wyjątków w programie Entity Framework. Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.  

Ten wpis nie jest właściwym miejscem dla pełne omówienie optymistycznej współbieżności. Poniższe sekcje zakładają pewną wiedzę na temat rozpoznawania współbieżności i wyświetlanie wzorców do wykonywania typowych zadań.  

Wiele z tych wzorców wprowadzić korzystanie z tematów omówionych w [Praca z wartościami właściwości](~/ef6/saving/change-tracking/property-values.md).  

Rozwiązywanie problemów współbieżności, gdy używana jest niezależne skojarzenia (gdzie klucza obcego nie została zamapowana na właściwość w jednostce) jest znacznie trudniejsze niż w przypadku używania powiązań kluczy obcych. W związku z tym jeśli czy rozdzielczość współbieżności w aplikacji zalecane jest zawsze mapy kluczy obcych do jednostki. Poniższe przykłady założono, że używasz skojarzenia klucza obcego.  

DbUpdateConcurrencyException jest generowany przez SaveChanges po wykryciu wyjątek optymistycznej współbieżności podczas próby zapisania jednostki, która używa skojarzenia klucza obcego.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>Rozwiązywanie wyjątków optymistycznej współbieżności za pomocą Reload (bazy danych wins)  

Metoda ponowne załadowanie może służyć do zastąpienie bieżących wartości jednostki przy użyciu wartości teraz w bazie danych. Jednostka następnie zwykle znajduje się wstecz do użytkownika w pewnej postaci i użytkownik próbuje ponownie wprowadzić swoje zmiany i Zapisz ponownie. Na przykład:  

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

Dobrym sposobem, aby zasymulować wyjątku współbieżności jest Ustaw punkt przerwania w wywołaniu funkcji SaveChanges, a następnie zmodyfikuj jednostki, do której jest zapisywany w bazie danych przy użyciu innego narzędzia, takie jak program SQL Management Studio. Można także wstawić wiersza przed SaveChanges można zaktualizować bazy danych bezpośrednio przy użyciu klasy SqlCommand. Na przykład:  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

Metoda wpisy na DbUpdateConcurrencyException zwraca DbEntityEntry wystąpień jednostki, których nie można zaktualizować. (Ta właściwość obecnie zawsze zwraca pojedynczej wartości dla współbieżności problemy. Jego może zwracać wiele wartości dla Wyjątki ogólne aktualizacji.) Zamiast w niektórych sytuacjach może być można pobrać Wpisy dla wszystkich obiektów, które mogą być wymagane ponowne załadowanie z bazy danych, i wywołanie Załaduj ponownie dla każdego z nich.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>Rozwiązywania wyjątków optymistycznej współbieżności jako klienta usługi wins  

W powyższym przykładzie używającej ponowne ładowanie jest czasami nazywane bazy danych wins lub magazynu usługi wins, ponieważ wartości w jednostce są zastępowane przez wartości z bazy danych. Czasami możesz nie przeciwieństwo i Zastąp wartości w bazie danych z wartościami, które aktualnie w jednostce. To jest czasami nazywane wins klienta i może odbywać się przez uzyskanie wartości bieżącej bazy danych i ustawisz je jako oryginalnych wartości jednostki. (Zobacz [Praca z wartościami właściwości](~/ef6/saving/change-tracking/property-values.md) uzyskać informacji na temat bieżących i oryginalnych wartości.) Na przykład:  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a>Niestandardowe rozwiązanie wyjątki optymistycznej współbieżności  

Czasami można połączyć ze sobą wartości obecnie w bazie danych z wartościami, które aktualnie w jednostce. Wymaga to zazwyczaj kilka niestandardowych interakcji logiki lub użytkownika. Na przykład mogą stanowić formularz do użytkownika, zawierającą bieżące wartości, wartości w bazie danych, a domyślny zestaw rozwiązane wartości. Użytkownik może następnie edytować rozwiązane wartości zgodnie z potrzebami i byłoby te rozwiązania wartości, które są zapisywane w bazie danych. Można to zrobić przy użyciu obiektów DbPropertyValues zwrócony z CurrentValues i GetDatabaseValues przy uruchamianiu jednostki. Na przykład:  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a>Niestandardowe rozwiązanie wyjątki optymistycznej współbieżności przy użyciu obiektów  

Powyższy kod używa wystąpienia DbPropertyValues przekazywanie wokół bieżącego, bazy danych i rozwiązane wartości. Czasami może być łatwiejsze do użycia wystąpienia tego typu jednostki dla tego. Można to zrobić przy użyciu metod ToObject i SetValues DbPropertyValues. Na przykład:  

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
