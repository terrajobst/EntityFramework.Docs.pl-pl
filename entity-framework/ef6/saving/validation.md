---
title: Walidacja — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 2c5e6f1b3f60862124bafcac42e8859a7591f8e6
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812153"
---
# <a name="data-validation"></a><span data-ttu-id="112ec-102">Walidacja danych</span><span class="sxs-lookup"><span data-stu-id="112ec-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="112ec-103">**Dr 4.1 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 4,1.</span><span class="sxs-lookup"><span data-stu-id="112ec-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="112ec-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie mają zastosowania</span><span class="sxs-lookup"><span data-stu-id="112ec-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="112ec-105">Zawartość na tej stronie jest dostosowana z artykułu utworzonego pierwotnie przez Julie Lerman ([https://thedatafarm.com](https://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="112ec-105">The content on this page is adapted from an article originally written by Julie Lerman ([https://thedatafarm.com](https://thedatafarm.com)).</span></span>

<span data-ttu-id="112ec-106">Entity Framework zapewnia szeroką gamę funkcji walidacji, które mogą przechodzić do interfejsu użytkownika w celu weryfikacji po stronie klienta lub służy do sprawdzania poprawności po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="112ec-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="112ec-107">Używając najpierw kodu, można określić walidacji przy użyciu adnotacji lub konfiguracji interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="112ec-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="112ec-108">Dodatkowe walidacje i bardziej skomplikowane, można określić w kodzie i będzie działać niezależnie od tego, czy model przyniesie najpierw od kodu, najpierw modelu czy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="112ec-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="112ec-109">Model</span><span class="sxs-lookup"><span data-stu-id="112ec-109">The model</span></span>

<span data-ttu-id="112ec-110">Zaprezentowanie walidacji przy użyciu prostej pary klas: blog i post.</span><span class="sxs-lookup"><span data-stu-id="112ec-110">I'll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
public class Blog
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string BloggerName { get; set; }
    public DateTime DateCreated { get; set; }
    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public DateTime DateCreated { get; set; }
    public string Content { get; set; }
    public int BlogId { get; set; }
    public ICollection<Comment> Comments { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="112ec-111">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="112ec-111">Data Annotations</span></span>

<span data-ttu-id="112ec-112">Code First używa adnotacji z zestawu `System.ComponentModel.DataAnnotations` jako jednego środka konfigurowania pierwszej klasy kodowej.</span><span class="sxs-lookup"><span data-stu-id="112ec-112">Code First uses annotations from the `System.ComponentModel.DataAnnotations` assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="112ec-113">Wśród tych adnotacji są te, które zawierają reguły, takie jak `Required`, `MaxLength` i `MinLength`.</span><span class="sxs-lookup"><span data-stu-id="112ec-113">Among these annotations are those which provide rules such as the `Required`, `MaxLength` and `MinLength`.</span></span> <span data-ttu-id="112ec-114">Wiele aplikacji klienckich platformy .NET rozpoznaje również te adnotacje, na przykład ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="112ec-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="112ec-115">Po stronie klienta i po stronie serwera można wykonać te adnotacje.</span><span class="sxs-lookup"><span data-stu-id="112ec-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="112ec-116">Na przykład można wymusić, aby Właściwość tytuł bloga była właściwością wymaganą.</span><span class="sxs-lookup"><span data-stu-id="112ec-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
[Required]
public string Title { get; set; }
```

<span data-ttu-id="112ec-117">W przypadku braku dodatkowych zmian kodu lub znaczników w aplikacji istniejąca aplikacja MVC przeprowadzi weryfikację po stronie klienta, nawet dynamicznie kompilując komunikat przy użyciu nazw właściwości i adnotacji.</span><span class="sxs-lookup"><span data-stu-id="112ec-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Rysunek 1](~/ef6/media/figure01.png)

<span data-ttu-id="112ec-119">W metodzie post tego widoku Utwórz Entity Framework służy do zapisywania nowego bloga w bazie danych, ale Walidacja po stronie klienta MVC jest wyzwalana, zanim aplikacja osiągnie ten kod.</span><span class="sxs-lookup"><span data-stu-id="112ec-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC's client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="112ec-120">Sprawdzanie poprawności po stronie klienta nie jest jednak niezgodne z punktorem.</span><span class="sxs-lookup"><span data-stu-id="112ec-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="112ec-121">Użytkownicy mogą mieć wpływ na funkcje przeglądarki lub jeszcze gorszy, haker może użyć pewnych lew, aby uniknąć walidacji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="112ec-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="112ec-122">Ale Entity Framework również rozpoznaje `Required` adnotację i sprawdza poprawność.</span><span class="sxs-lookup"><span data-stu-id="112ec-122">But Entity Framework will also recognize the `Required` annotation and validate it.</span></span>

<span data-ttu-id="112ec-123">Prostym sposobem na przetestowanie jest wyłączenie funkcji walidacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="112ec-123">A simple way to test this is to disable MVC's client-side validation feature.</span></span> <span data-ttu-id="112ec-124">Można to zrobić w pliku Web. config aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="112ec-124">You can do this in the MVC application's web.config file.</span></span> <span data-ttu-id="112ec-125">Sekcja appSettings ma klucz dla ClientValidationEnabled.</span><span class="sxs-lookup"><span data-stu-id="112ec-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="112ec-126">Ustawienie tego klucza na wartość false uniemożliwi interfejsowi użytkownika wykonywanie walidacji.</span><span class="sxs-lookup"><span data-stu-id="112ec-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

<span data-ttu-id="112ec-127">Nawet po wyłączeniu weryfikacji po stronie klienta otrzymasz taką samą odpowiedź w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="112ec-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="112ec-128">Komunikat o błędzie "pole Tytuł jest wymagane" będzie wyświetlany jako poprzednio.</span><span class="sxs-lookup"><span data-stu-id="112ec-128">The error message "The Title field is required" will be displayed as before.</span></span> <span data-ttu-id="112ec-129">Z tego powodu będzie można sprawdzić poprawność po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="112ec-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="112ec-130">Entity Framework przeprowadzi weryfikację w adnotacji `Required` (przed tym, że będzie ona mogła utworzyć polecenie `INSERT` do wysłania do bazy danych) i zwrócić błąd do MVC, który wyświetli komunikat.</span><span class="sxs-lookup"><span data-stu-id="112ec-130">Entity Framework will perform the validation on the `Required` annotation (before it even bothers to build an `INSERT` command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="112ec-131">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="112ec-131">Fluent API</span></span>

<span data-ttu-id="112ec-132">Możesz użyć interfejsu API Fluent pierwszego kodu zamiast adnotacji, aby uzyskać tę samą stronę klienta & weryfikacji po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="112ec-132">You can use code first's fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="112ec-133">Zamiast używać `Required`, pokażę to przy użyciu walidacji MaxLength.</span><span class="sxs-lookup"><span data-stu-id="112ec-133">Rather than use `Required`, I'll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="112ec-134">Konfiguracje interfejsu API Fluent są stosowane, ponieważ kod najpierw kompiluje model z klas.</span><span class="sxs-lookup"><span data-stu-id="112ec-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="112ec-135">Konfiguracje można wstrzyknąć przez zastąpienie metody OnModelCreating DbContext klasy.</span><span class="sxs-lookup"><span data-stu-id="112ec-135">You can inject the configurations by overriding the DbContext class' OnModelCreating  method.</span></span> <span data-ttu-id="112ec-136">Poniżej znajduje się Konfiguracja określająca, że właściwość Bloggername nie może być dłuższa niż 10 znaków.</span><span class="sxs-lookup"><span data-stu-id="112ec-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>().Property(p => p.BloggerName).HasMaxLength(10);
    }
}
```

<span data-ttu-id="112ec-137">Błędy sprawdzania poprawności zgłoszone na podstawie konfiguracji interfejsu API Fluent nie będą automatycznie docierać do interfejsu użytkownika, ale można je przechwycić w kodzie, a następnie odpowiedzieć odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="112ec-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="112ec-138">Poniżej przedstawiono kod błędu obsługi wyjątków w klasie BlogController aplikacji, który przechwytuje ten błąd sprawdzania poprawności, gdy Entity Framework próbuje zapisać blog o wartości większej niż 10 znaków.</span><span class="sxs-lookup"><span data-stu-id="112ec-138">Here's some exception handling error code in the application's BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

``` csharp
[HttpPost]
public ActionResult Edit(int id, Blog blog)
{
    try
    {
        db.Entry(blog).State = EntityState.Modified;
        db.SaveChanges();
        return RedirectToAction("Index");
    }
    catch (DbEntityValidationException ex)
    {
        var error = ex.EntityValidationErrors.First().ValidationErrors.First();
        this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
        return View();
    }
}
```

<span data-ttu-id="112ec-139">Sprawdzanie poprawności nie jest automatycznie przenoszone do widoku, dlatego jest używany dodatkowy kod, który używa `ModelState.AddModelError`.</span><span class="sxs-lookup"><span data-stu-id="112ec-139">The validation doesn't automatically get passed back into the view which is why the additional code that uses `ModelState.AddModelError` is being used.</span></span> <span data-ttu-id="112ec-140">Dzięki temu szczegóły błędu zostaną wprowadzone do widoku, który następnie użyje `ValidationMessageFor` Htmlhelper do wyświetlenia błędu.</span><span class="sxs-lookup"><span data-stu-id="112ec-140">This ensures that the error details make it to the view which will then use the `ValidationMessageFor` Htmlhelper to display the error.</span></span>

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="112ec-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="112ec-141">IValidatableObject</span></span>

<span data-ttu-id="112ec-142">`IValidatableObject` to interfejs, który znajduje się w `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="112ec-142">`IValidatableObject` is an interface that lives in `System.ComponentModel.DataAnnotations`.</span></span> <span data-ttu-id="112ec-143">Chociaż nie jest on częścią interfejsu API Entity Framework, można nadal korzystać z niego do walidacji po stronie serwera w klasach Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="112ec-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="112ec-144">`IValidatableObject` udostępnia metodę `Validate`, która Entity Framework będzie wywoływana podczas metody savechangesu lub można wywołać siebie w dowolnym momencie, aby sprawdzić poprawność klas.</span><span class="sxs-lookup"><span data-stu-id="112ec-144">`IValidatableObject` provides a `Validate` method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="112ec-145">Konfiguracje, takie jak `Required` i `MaxLength`, sprawdzają poprawność jednego pola.</span><span class="sxs-lookup"><span data-stu-id="112ec-145">Configurations such as `Required` and `MaxLength` perform validation on a single field.</span></span> <span data-ttu-id="112ec-146">W metodzie `Validate` można mieć jeszcze bardziej złożoną logikę, na przykład porównując dwa pola.</span><span class="sxs-lookup"><span data-stu-id="112ec-146">In the `Validate` method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="112ec-147">W poniższym przykładzie Klasa `Blog` została rozszerzona w celu zaimplementowania `IValidatableObject`, a następnie podaj regułę, której `Title` i `BloggerName` nie będą zgodne.</span><span class="sxs-lookup"><span data-stu-id="112ec-147">In the following example, the `Blog` class has been extended to implement `IValidatableObject` and then provide a rule that the `Title` and `BloggerName` cannot match.</span></span>

``` csharp
public class Blog : IValidatableObject
{
    public int Id { get; set; }

    [Required]
    public string Title { get; set; }

    public string BloggerName { get; set; }
    public DateTime DateCreated { get; set; }
    public virtual ICollection<Post> Posts { get; set; }

    public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
    {
        if (Title == BloggerName)
        {
            yield return new ValidationResult(
                "Blog Title cannot match Blogger Name",
                new[] { nameof(Title), nameof(BloggerName) });
        }
    }
}
```

<span data-ttu-id="112ec-148">Konstruktor `ValidationResult` przyjmuje `string`, który reprezentuje komunikat o błędzie i tablicę `string`s reprezentującą nazwy elementów członkowskich, które są skojarzone z walidacją.</span><span class="sxs-lookup"><span data-stu-id="112ec-148">The `ValidationResult` constructor takes a `string` that represents the error message and an array of `string`s that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="112ec-149">Ponieważ ta Walidacja sprawdza zarówno `Title`, jak i `BloggerName`, zwracane są obie nazwy właściwości.</span><span class="sxs-lookup"><span data-stu-id="112ec-149">Since this validation checks both the `Title` and the `BloggerName`, both property names are returned.</span></span>

<span data-ttu-id="112ec-150">W przeciwieństwie do walidacji dostarczonej przez interfejs API Fluent, ten wynik sprawdzania poprawności zostanie rozpoznany przez widok i program obsługi wyjątków, który został wcześniej użyty do dodania błędu do `ModelState` jest zbędny.</span><span class="sxs-lookup"><span data-stu-id="112ec-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into `ModelState` is unnecessary.</span></span> <span data-ttu-id="112ec-151">Ponieważ ustawiam obie nazwy właściwości w `ValidationResult`, MVC HtmlHelpers wyświetla komunikat o błędzie dla obu tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="112ec-151">Because I set both property names in the `ValidationResult`, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![Rysunek 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="112ec-153">DbContext. ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="112ec-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="112ec-154">`DbContext` ma metodę z możliwością zastąpienia o nazwie `ValidateEntity`.</span><span class="sxs-lookup"><span data-stu-id="112ec-154">`DbContext` has an overridable method called `ValidateEntity`.</span></span> <span data-ttu-id="112ec-155">Gdy wywołasz `SaveChanges`, Entity Framework wywoła tę metodę dla każdej jednostki w pamięci podręcznej, której stan nie jest `Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="112ec-155">When you call `SaveChanges`, Entity Framework will call this method for each entity in its cache whose state is not `Unchanged`.</span></span> <span data-ttu-id="112ec-156">Logikę walidacji można umieścić bezpośrednio w tym miejscu, a nawet użyć tej metody do wywołania, na przykład, metody `Blog.Validate` dodanej w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="112ec-156">You can put validation logic directly in here or even use this method to call, for example, the `Blog.Validate` method added in the previous section.</span></span>

<span data-ttu-id="112ec-157">Oto przykład przesłonięcia `ValidateEntity`, który sprawdza poprawność nowych `Post`s, aby upewnić się, że tytuł wpisu nie został już użyty.</span><span class="sxs-lookup"><span data-stu-id="112ec-157">Here's an example of a `ValidateEntity` override that validates new `Post`s to ensure that the post title hasn't been used already.</span></span> <span data-ttu-id="112ec-158">Najpierw sprawdza, czy jednostka jest wpisem i czy jego stan jest dodawany.</span><span class="sxs-lookup"><span data-stu-id="112ec-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="112ec-159">W takim przypadku szuka w bazie danych, czy istnieje już wpis o takim samym tytule.</span><span class="sxs-lookup"><span data-stu-id="112ec-159">If that's the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="112ec-160">Jeśli istnieje już istniejący wpis, zostanie utworzony nowy `DbEntityValidationResult`.</span><span class="sxs-lookup"><span data-stu-id="112ec-160">If there is an existing post already, then a new `DbEntityValidationResult` is created.</span></span>

<span data-ttu-id="112ec-161">`DbEntityValidationResult` `DbEntityEntry` i `ICollection<DbValidationErrors>` dla jednej jednostki.</span><span class="sxs-lookup"><span data-stu-id="112ec-161">`DbEntityValidationResult` houses a `DbEntityEntry` and an `ICollection<DbValidationErrors>` for a single entity.</span></span> <span data-ttu-id="112ec-162">Na początku tej metody zostanie utworzone wystąpienie `DbEntityValidationResult`, a następnie wszystkie wykryte błędy zostaną dodane do `ValidationErrors` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="112ec-162">At the start of this method, a `DbEntityValidationResult` is instantiated and then any errors that are discovered are added into its `ValidationErrors` collection.</span></span>

``` csharp
protected override DbEntityValidationResult ValidateEntity (
    System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
    IDictionary<object, object> items)
{
    var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());

    if (entityEntry.Entity is Post post && entityEntry.State == EntityState.Added)
    {
        // Check for uniqueness of post title
        if (Posts.Where(p => p.Title == post.Title).Any())
        {
            result.ValidationErrors.Add(
                    new System.Data.Entity.Validation.DbValidationError(
                        nameof(Title),
                        "Post title must be unique."));
        }
    }

    if (result.ValidationErrors.Count > 0)
    {
        return result;
    }
    else
    {
        return base.ValidateEntity(entityEntry, items);
    }
}
```

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="112ec-163">Jawne wyzwalanie walidacji</span><span class="sxs-lookup"><span data-stu-id="112ec-163">Explicitly triggering validation</span></span>

<span data-ttu-id="112ec-164">Wywołanie `SaveChanges` wyzwala wszystkie walidacje omówione w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="112ec-164">A call to `SaveChanges` triggers all of the validations covered in this article.</span></span> <span data-ttu-id="112ec-165">Nie trzeba jednak polegać na `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="112ec-165">But you don't need to rely on `SaveChanges`.</span></span> <span data-ttu-id="112ec-166">Możesz chcieć sprawdzić poprawność w innym miejscu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="112ec-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="112ec-167">`DbContext.GetValidationErrors` wyzwoli wszystkie walidacje, te zdefiniowane przez adnotacje lub interfejs API Fluent, weryfikację utworzoną w `IValidatableObject` (na przykład `Blog.Validate`) i walidacje wykonywane w metodzie `DbContext.ValidateEntity`.</span><span class="sxs-lookup"><span data-stu-id="112ec-167">`DbContext.GetValidationErrors` will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in `IValidatableObject` (for example, `Blog.Validate`), and the validations performed in the `DbContext.ValidateEntity` method.</span></span>

<span data-ttu-id="112ec-168">Poniższy kod wywoła `GetValidationErrors` na bieżącym wystąpieniu `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="112ec-168">The following code will call `GetValidationErrors` on the current instance of a `DbContext`.</span></span> <span data-ttu-id="112ec-169">`ValidationErrors` są pogrupowane według typu jednostki w `DbEntityValidationResult`.</span><span class="sxs-lookup"><span data-stu-id="112ec-169">`ValidationErrors` are grouped by entity type into `DbEntityValidationResult`.</span></span> <span data-ttu-id="112ec-170">Kod iteruje najpierw za pomocą `DbEntityValidationResult`s zwróconym przez metodę, a następnie za pomocą poszczególnych `DbValidationError` wewnątrz.</span><span class="sxs-lookup"><span data-stu-id="112ec-170">The code iterates first through the `DbEntityValidationResult`s returned by the method and then through each `DbValidationError` inside.</span></span>

``` csharp
foreach (var validationResult in db.GetValidationErrors())
{
    foreach (var error in validationResult.ValidationErrors)
    {
        Debug.WriteLine(
            "Entity Property: {0}, Error {1}",
            error.PropertyName,
            error.ErrorMessage);
    }
}
```

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="112ec-171">Inne zagadnienia dotyczące korzystania z walidacji</span><span class="sxs-lookup"><span data-stu-id="112ec-171">Other considerations when using validation</span></span>

<span data-ttu-id="112ec-172">Oto kilka innych punktów, które należy wziąć pod uwagę podczas korzystania z Entity Framework weryfikacji:</span><span class="sxs-lookup"><span data-stu-id="112ec-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

- <span data-ttu-id="112ec-173">Ładowanie z opóźnieniem jest wyłączone podczas walidacji</span><span class="sxs-lookup"><span data-stu-id="112ec-173">Lazy loading is disabled during validation</span></span>
- <span data-ttu-id="112ec-174">EF sprawdzi poprawność adnotacji danych dla niemapowanych właściwości (właściwości, które nie są zamapowane do kolumny w bazie danych).</span><span class="sxs-lookup"><span data-stu-id="112ec-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database)</span></span>
- <span data-ttu-id="112ec-175">Walidacja jest wykonywana po wykryciu zmian podczas `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="112ec-175">Validation is performed after changes are detected during `SaveChanges`.</span></span> <span data-ttu-id="112ec-176">Jeśli wprowadzisz zmiany podczas weryfikacji, odpowiadasz za powiadomienie śledzenia zmian</span><span class="sxs-lookup"><span data-stu-id="112ec-176">If you make changes during validation it is your responsibility to notify the change tracker</span></span>
- <span data-ttu-id="112ec-177">`DbUnexpectedValidationException` jest generowany w przypadku wystąpienia błędów podczas walidacji</span><span class="sxs-lookup"><span data-stu-id="112ec-177">`DbUnexpectedValidationException` is thrown if errors occur during validation</span></span>
- <span data-ttu-id="112ec-178">Aspekty Entity Framework uwzględnione w modelu (maksymalna długość, wymagana itp.) spowodują walidację, nawet jeśli nie ma adnotacji danych w klasach i/lub użyto projektanta EF do utworzenia modelu</span><span class="sxs-lookup"><span data-stu-id="112ec-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are no data annotations in your classes and/or you used the EF Designer to create your model</span></span>
- <span data-ttu-id="112ec-179">Reguły pierwszeństwa:</span><span class="sxs-lookup"><span data-stu-id="112ec-179">Precedence rules:</span></span>
  - <span data-ttu-id="112ec-180">Wywołania interfejsu API Fluent przesłaniają odpowiednie adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="112ec-180">Fluent API calls override the corresponding data annotations</span></span>
- <span data-ttu-id="112ec-181">Kolejność wykonywania:</span><span class="sxs-lookup"><span data-stu-id="112ec-181">Execution order:</span></span>
  - <span data-ttu-id="112ec-182">Walidacja właściwości występuje przed walidacją typu</span><span class="sxs-lookup"><span data-stu-id="112ec-182">Property validation occurs before type validation</span></span>
  - <span data-ttu-id="112ec-183">Walidacja typu występuje tylko wtedy, gdy Walidacja właściwości zostanie zakończona pomyślnie</span><span class="sxs-lookup"><span data-stu-id="112ec-183">Type validation only occurs if property validation succeeds</span></span>
- <span data-ttu-id="112ec-184">Jeśli właściwość jest złożona, jego Walidacja również będzie obejmować:</span><span class="sxs-lookup"><span data-stu-id="112ec-184">If a property is complex, its validation will also include:</span></span>
  - <span data-ttu-id="112ec-185">Walidacja na poziomie właściwości we właściwościach typu złożonego</span><span class="sxs-lookup"><span data-stu-id="112ec-185">Property-level validation on the complex type properties</span></span>
  - <span data-ttu-id="112ec-186">Walidacja poziomu typu dla typu złożonego, w tym `IValidatableObject` walidacji typu złożonego</span><span class="sxs-lookup"><span data-stu-id="112ec-186">Type level validation on the complex type, including `IValidatableObject` validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="112ec-187">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="112ec-187">Summary</span></span>

<span data-ttu-id="112ec-188">Interfejs API sprawdzania poprawności w Entity Framework odgrywa dobrze z walidacją po stronie klienta w MVC, ale nie musisz polegać na weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="112ec-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="112ec-189">Entity Framework będzie obsłużyć walidację po stronie serwera dla adnotacji lub konfiguracji zastosowanej przy użyciu kodu interfejsu API pierwszego Fluent.</span><span class="sxs-lookup"><span data-stu-id="112ec-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="112ec-190">Przedstawiono również szereg punktów rozszerzalności służących do dostosowywania zachowania, niezależnie od tego, czy używasz interfejsu `IValidatableObject`, czy też naciśniesz do metody `DbContext.ValidateEntity`.</span><span class="sxs-lookup"><span data-stu-id="112ec-190">You also saw a number of extensibility points for customizing the behavior whether you use the `IValidatableObject` interface or tap into the `DbContext.ValidateEntity` method.</span></span> <span data-ttu-id="112ec-191">Te ostatnie dwa sposoby weryfikacji są dostępne za pośrednictwem `DbContext`, niezależnie od tego, czy do opisania modelu koncepcyjnego jest używany przepływ pracy Code First, Model First czy Database First.</span><span class="sxs-lookup"><span data-stu-id="112ec-191">And these last two means of validation are available through the `DbContext`, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
