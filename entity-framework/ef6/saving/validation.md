---
title: Sprawdzanie poprawności — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
caps.latest.revision: 3
ms.openlocfilehash: 758865255d7868337dc1d7801bd9ff77f0bb57a9
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949264"
---
# <a name="data-validation"></a><span data-ttu-id="e8512-102">Sprawdzanie poprawności danych</span><span class="sxs-lookup"><span data-stu-id="e8512-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="e8512-103">**EF4.1 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="e8512-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="e8512-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania</span><span class="sxs-lookup"><span data-stu-id="e8512-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="e8512-105">Zawartość na tej stronie są zaczerpnięte z, a artykuł pierwotnie napisane przez Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="e8512-105">The content on this page is adapted from and article originally written by Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span></span>

<span data-ttu-id="e8512-106">Entity Framework udostępnia wiele różnych funkcji sprawdzania poprawności, które może przekazywać za pośrednictwem interfejsu użytkownika w celu weryfikacji po stronie klienta lub można użyć do weryfikacji po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="e8512-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="e8512-107">Po raz pierwszy przy użyciu kodu, można określić operacji sprawdzania poprawności przy użyciu adnotacji lub fluent konfiguracji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="e8512-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="e8512-108">Dodatkowe sprawdzanie poprawności i bardziej złożone, można określić w kodzie i będzie działać, czy model ma za sobą kodu po pierwsze, najpierw modelu lub najpierw bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e8512-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="e8512-109">Model</span><span class="sxs-lookup"><span data-stu-id="e8512-109">The model</span></span>

<span data-ttu-id="e8512-110">Zademonstruję walidacji przy użyciu prostego pary klas: Blog i Post.</span><span class="sxs-lookup"><span data-stu-id="e8512-110">I’ll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
    public class Blog
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public string BloggerName { get; set; }
          public DateTime DateCreated { get; set; }
          public virtual ICollection<Post> Posts { get; set; }
          }
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
## <a name="data-annotations"></a><span data-ttu-id="e8512-111">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="e8512-111">Data Annotations</span></span>

<span data-ttu-id="e8512-112">Kod używa pierwsze adnotacje z zestawu System.ComponentModel.DataAnnotations jako jednym ze sposobów konfigurowania kodu pierwszej klasy.</span><span class="sxs-lookup"><span data-stu-id="e8512-112">Code First uses annotations from the System.ComponentModel.DataAnnotations assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="e8512-113">Wśród tych adnotacji są tymi, które zapewniają reguły, takie jak wymaganych, MaxLength, MinLength.</span><span class="sxs-lookup"><span data-stu-id="e8512-113">Among these annotations are those which provide rules such as the Required, MaxLength and MinLength.</span></span> <span data-ttu-id="e8512-114">Liczba aplikacji klienta .NET rozpoznaje również tych adnotacji, na przykład platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e8512-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="e8512-115">Obie strony serwera i weryfikacji po stronie klienta przy użyciu tych adnotacji można osiągnąć.</span><span class="sxs-lookup"><span data-stu-id="e8512-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="e8512-116">Na przykład możesz wymusić właściwości Tytuł blogu wymaganej właściwości.</span><span class="sxs-lookup"><span data-stu-id="e8512-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="e8512-117">Bez dodatkowego kodu i znaczników zmiany w aplikacji istniejącej aplikacji MVC przeprowadzi weryfikację po stronie klienta, nawet dynamiczne tworzenie komunikat przy użyciu nazwy właściwości i adnotacji.</span><span class="sxs-lookup"><span data-stu-id="e8512-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![figure01](~/ef6/media/figure01.png)

<span data-ttu-id="e8512-119">We wpisie kopii metody tego widoku Create, platformy Entity Framework jest używany do zapisywania nowego bloga w bazie danych, ale weryfikacji po stronie klienta dla platformy MVC jest wyzwalany, zanim aplikacja osiągnie ten kod.</span><span class="sxs-lookup"><span data-stu-id="e8512-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC’s client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="e8512-120">Weryfikacji po stronie klienta nie jest jednak punktor dowodu.</span><span class="sxs-lookup"><span data-stu-id="e8512-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="e8512-121">Użytkownicy mogą mieć wpływ na funkcje przeglądarki lub co gorsza, haker może używać niektórych trickery w celu uniknięcia walidacji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e8512-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="e8512-122">Ale Entity Framework rozpoznaje adnotację wymagane również i zweryfikuje go.</span><span class="sxs-lookup"><span data-stu-id="e8512-122">But Entity Framework will also recognize the Required annotation and validate it.</span></span>

<span data-ttu-id="e8512-123">Jest to prosty sposób testować tę aplikację można wyłączyć funkcję weryfikacji po stronie klienta w MVC.</span><span class="sxs-lookup"><span data-stu-id="e8512-123">A simple way to test this is to disable MVC’s client-side validation feature.</span></span> <span data-ttu-id="e8512-124">Można to zrobić w pliku web.config aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="e8512-124">You can do this in the MVC application’s web.config file.</span></span> <span data-ttu-id="e8512-125">W sekcji appSettings ma klucz dla ClientValidationEnabled.</span><span class="sxs-lookup"><span data-stu-id="e8512-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="e8512-126">Ustawienie wartości false dla tego klucza uniemożliwi interfejs użytkownika wykonywania operacji sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="e8512-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

<span data-ttu-id="e8512-127">Nawet w przypadku weryfikacji po stronie klienta, wyłączona uzyskasz tę samą odpowiedź do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e8512-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="e8512-128">Komunikat o błędzie "Tytuł pole jest wymagane" będą wyświetlane jako.</span><span class="sxs-lookup"><span data-stu-id="e8512-128">The error message “The Title field is required” will be displayed as.</span></span> <span data-ttu-id="e8512-129">Z wyjątkiem teraz będą wynik weryfikacji po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="e8512-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="e8512-130">Entity Framework będzie wykonywać sprawdzanie poprawności adnotację, wymagane (przed nawet bothers kompilacji i Wstaw polecenie, aby wysłać do bazy danych) i zwrócenie błędu do MVC, co spowoduje wyświetlenie komunikatu.</span><span class="sxs-lookup"><span data-stu-id="e8512-130">Entity Framework will perform the validation on the Required annotation (before it even bothers to build and INSERT command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e8512-131">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="e8512-131">Fluent API</span></span>

<span data-ttu-id="e8512-132">Możesz użyć kodu przez pierwszy interfejs fluent API zamiast adnotacje do Uzyskaj tego samego klienta po stronie serwera po stronie sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="e8512-132">You can use code first’s fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="e8512-133">Zamiast użycia wymagane, czy pokazano tej weryfikacji MaxLength przy użyciu.</span><span class="sxs-lookup"><span data-stu-id="e8512-133">Rather than use Required, I’ll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="e8512-134">Fluent API konfiguracje są stosowane w kodzie najpierw jest Budowanie modelu z klas.</span><span class="sxs-lookup"><span data-stu-id="e8512-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="e8512-135">Konfiguracje można wstrzyknąć poprzez zastąpienie metody OnModelCreating klasy DbContext.</span><span class="sxs-lookup"><span data-stu-id="e8512-135">You can inject the configurations by overriding the DbContext class’ OnModelCreating  method.</span></span> <span data-ttu-id="e8512-136">Poniżej przedstawiono konfigurację, określając, że właściwość BloggerName nie może być dłuższa niż 10 znaków.</span><span class="sxs-lookup"><span data-stu-id="e8512-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="e8512-137">Błędy sprawdzania poprawności generowany na podstawie konfiguracji interfejsu API Fluent nie będzie automatycznie zasięg interfejsu użytkownika, ale można przechwytywać go w kodzie i następnie odpowiedź do niego odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="e8512-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="e8512-138">Oto niektóre błąd kodu w klasie BlogController aplikacji, która przechwytuje ten błąd sprawdzania poprawności, gdy Entity Framework próbuje zapisać blog BloggerName, która przekracza maksymalną liczbę znaków 10 obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="e8512-138">Here’s some exception handling error code in the application’s BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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
        catch(DbEntityValidationException ex)
        {
            var error = ex.EntityValidationErrors.First().ValidationErrors.First();
            this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
            return View();
        }
    }
```

<span data-ttu-id="e8512-139">Sprawdzanie poprawności nie Pobierz automatycznie przekazywane do widoku, dlatego dodatkowy kod, który używa ModelState.AddModelError jest używany.</span><span class="sxs-lookup"><span data-stu-id="e8512-139">The validation doesn’t automatically get passed back into the view which is why the additional code that uses ModelState.AddModelError is being used.</span></span> <span data-ttu-id="e8512-140">Daje to gwarancję, że szczegóły błędu przenoszone do widoku, który zostanie użyty ValidationMessageFor Htmlhelper do wyświetlenia błędu.</span><span class="sxs-lookup"><span data-stu-id="e8512-140">This ensures that the error details make it to the view which will then use the ValidationMessageFor Htmlhelper to display the error.</span></span>

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="e8512-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="e8512-141">IValidatableObject</span></span>

<span data-ttu-id="e8512-142">IValidatableObject jest interfejsem, który znajduje się w System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="e8512-142">IValidatableObject is an interface that lives in System.ComponentModel.DataAnnotations.</span></span> <span data-ttu-id="e8512-143">Mimo że nie jest częścią interfejsu API programu Entity Framework, można nadal korzystać z go do weryfikacji po stronie serwera w Twoich zajęciach Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8512-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="e8512-144">IValidatableObject udostępnia metodę sprawdzania poprawności, która wywoła Entity Framework podczas SaveChanges lub możesz wywołać samodzielnie ilekroć, którą chcesz zweryfikować klasy.</span><span class="sxs-lookup"><span data-stu-id="e8512-144">IValidatableObject provides a Validate method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="e8512-145">Konfiguracje, takie jak wymagane i MaxLength wykonać validaton według jednego pola.</span><span class="sxs-lookup"><span data-stu-id="e8512-145">Configurations such as Required and MaxLength perform validaton on a single field.</span></span> <span data-ttu-id="e8512-146">W metodzie sprawdzania poprawności może być jeszcze bardziej złożonej logiki, na przykład porównanie dwóch pól.</span><span class="sxs-lookup"><span data-stu-id="e8512-146">In the Validate method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="e8512-147">W poniższym przykładzie klasa blogu został rozszerzony do zaimplementowania IValidatableObject, a następnie podaj regułę, która tytuł i BloggerName nie może być zgodna.</span><span class="sxs-lookup"><span data-stu-id="e8512-147">In the following example, the Blog class has been extended to implement IValidatableObject and then provide a rule that the Title and BloggerName cannot match.</span></span>

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
                 yield return new ValidationResult
                  ("Blog Title cannot match Blogger Name", new[] { "Title", “BloggerName” });
             }
         }
     }
```

<span data-ttu-id="e8512-148">Konstruktor ValidationResult przyjmuje ciąg reprezentujący komunikat o błędzie i Tablica ciągów, które reprezentują nazwy elementów członkowskich, które są skojarzone z weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="e8512-148">The ValidationResult constructor takes a string that represents the error message and an array of strings that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="e8512-149">Ponieważ ta weryfikacja, tytuł i BloggerName, zwracane są obie nazwy właściwości.</span><span class="sxs-lookup"><span data-stu-id="e8512-149">Since this validation checks both the Title and the BloggerName, both property names are returned.</span></span>

<span data-ttu-id="e8512-150">W przeciwieństwie do sprawdzania poprawności, udostępniony przez interfejs Fluent API ten wynik sprawdzania poprawności zostanie rozpoznane przez widok, a program obsługi wyjątku, który wcześniej używane do dodawania błąd do ModelState nie jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="e8512-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into ModelState is unnecessary.</span></span> <span data-ttu-id="e8512-151">Ponieważ obie nazwy właściwości ustawione w ValidationResult, MVC HtmlHelpers wyświetlić komunikat o błędzie dla obu tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="e8512-151">Because I set both property names in the ValidationResult, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![figure02](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="e8512-153">DbContext.ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="e8512-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="e8512-154">Kontekst DbContext ma metodę z możliwością o nazwie ValidateEntity.</span><span class="sxs-lookup"><span data-stu-id="e8512-154">DbContext has an Overridable method called ValidateEntity.</span></span> <span data-ttu-id="e8512-155">Po wywołaniu funkcji SaveChanges Entity Framework będzie wywoływać tej metody dla każdej jednostki w pamięci podręcznej, którego stan nie jest bez zmian.</span><span class="sxs-lookup"><span data-stu-id="e8512-155">When you call SaveChanges, Entity Framework will call this method for each entity in its cache whose state is not Unchanged.</span></span> <span data-ttu-id="e8512-156">Logikę weryfikacji można umieścić bezpośrednio w tutaj lub nawet użycie tej metody do wywołania, na przykład metoda Blog.Validate dodany w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="e8512-156">You can put validation logic directly in here or even use this method to call, for example, the Blog.Validate method added in the previous section.</span></span>

<span data-ttu-id="e8512-157">Oto przykład zastąpienia ValidateEntity, która weryfikuje nowych wpisów, aby upewnić się, że tytuł wpisu nie został już użyty.</span><span class="sxs-lookup"><span data-stu-id="e8512-157">Here’s an example of a ValidateEntity override that validates new Posts to ensure that the post title hasn’t been used already.</span></span> <span data-ttu-id="e8512-158">Go najpierw sprawdza, czy jednostka jest wpis i że jego stan zostanie dodany.</span><span class="sxs-lookup"><span data-stu-id="e8512-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="e8512-159">Jeśli tak jest rzeczywiście, następnie szuka w bazie danych, aby sprawdzić, czy jest już wpis o tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="e8512-159">If that’s the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="e8512-160">Jeśli istniejący wpis już istnieje, jest tworzony nowy DbEntityValidationResult.</span><span class="sxs-lookup"><span data-stu-id="e8512-160">If there is an existing post already, then a new DbEntityValidationResult is created.</span></span>

<span data-ttu-id="e8512-161">DbEntityValidationResult przechowuje DbEntityEntry i ICollection DbValidationErrors dla pojedynczej jednostki.</span><span class="sxs-lookup"><span data-stu-id="e8512-161">DbEntityValidationResult houses a DbEntityEntry and an ICollection of DbValidationErrors for a single entity.</span></span> <span data-ttu-id="e8512-162">Na początku tej metody DbEntityValidationResult zostanie uruchomiony, a następnie wszelkie błędy, które zostały wykryte zostaną dodane do swojej kolekcji ValidationErrors.</span><span class="sxs-lookup"><span data-stu-id="e8512-162">At the start of this method, a  DbEntityValidationResult is instantiated and then any errors that are discovered are added into its ValidationErrors collection.</span></span>

``` csharp
    protected override DbEntityValidationResult ValidateEntity (
        System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
        IDictionary\<object, object> items)
    {
        var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());
        if (entityEntry.Entity is Post && entityEntry.State == EntityState.Added)
        {
            Post post = entityEntry.Entity as Post;
            //check for uniqueness of post title
            if (Posts.Where(p => p.Title == post.Title).Count() > 0)
            {
                result.ValidationErrors.Add(
                        new System.Data.Entity.Validation.DbValidationError("Title",
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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="e8512-163">Jawnie wyzwalania sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="e8512-163">Explicitly triggering validation</span></span>

<span data-ttu-id="e8512-164">Po wywołaniu funkcji SaveChanges wyzwala wszystkie operacje sprawdzania poprawności omówione w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="e8512-164">A call to SaveChanges triggers all of the validations covered in this article.</span></span> <span data-ttu-id="e8512-165">Ale nie trzeba polegać na SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="e8512-165">But you don’t need to rely on SaveChanges.</span></span> <span data-ttu-id="e8512-166">Możesz sprawdzić w innym miejscu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e8512-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="e8512-167">DbContext.GetValidationErrors wyzwoli wszystkie operacje sprawdzania poprawności, te określone przez adnotacje lub interfejsu API Fluent, sprawdzanie poprawności utworzonych w IValidatableObject (na przykład Blog.Validate) i sprawdzeń wykonywane w DbContext.ValidateEntity Metoda.</span><span class="sxs-lookup"><span data-stu-id="e8512-167">DbContext.GetValidationErrors will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in IValidatableObject (for example, Blog.Validate), and the validations performed in the DbContext.ValidateEntity method.</span></span>

<span data-ttu-id="e8512-168">Poniższy kod wywoła GetValidationErrors na bieżącym wystąpieniu typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="e8512-168">The following code will call GetValidationErrors on the current instance of a DbContext.</span></span> <span data-ttu-id="e8512-169">ValidationErrors są pogrupowane według typu jednostki do DbValidationRestuls.</span><span class="sxs-lookup"><span data-stu-id="e8512-169">ValidationErrors are grouped by entity type into DbValidationRestuls.</span></span> <span data-ttu-id="e8512-170">Kod wykonuje iterację najpierw za pomocą DbValidationResults zwracany przez metodę, a następnie za pomocą każdego ValidationError wewnątrz.</span><span class="sxs-lookup"><span data-stu-id="e8512-170">The code iterates first through the DbValidationResults returned by the method and then through each ValidationError inside.</span></span>

``` csharp
    foreach (var validationResults in db.GetValidationErrors())
        {
            foreach (var error in validationResults.ValidationErrors)
            {
                Debug.WriteLine(
                                  "Entity Property: {0}, Error {1}",
                                  error.PropertyName,
                                  error.ErrorMessage);
            }
        }
```

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="e8512-171">Inne uwagi dotyczące korzystania z weryfikacji</span><span class="sxs-lookup"><span data-stu-id="e8512-171">Other considerations when using validation</span></span>

<span data-ttu-id="e8512-172">Poniżej przedstawiono kilka kwestii do rozważenia podczas korzystania z programu Entity Framework sprawdzania poprawności:</span><span class="sxs-lookup"><span data-stu-id="e8512-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

-   <span data-ttu-id="e8512-173">Powolne ładowanie jest wyłączone podczas sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="e8512-173">Lazy loading is disabled during validation.</span></span>
-   <span data-ttu-id="e8512-174">EF zostanie przeprowadzona Weryfikacja adnotacje danych na temat właściwości — zamapowany (właściwości, które nie są zamapowane na kolumnę w bazie danych).</span><span class="sxs-lookup"><span data-stu-id="e8512-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database).</span></span>
-   <span data-ttu-id="e8512-175">Sprawdzanie poprawności jest wykonywane po zmiany są wykrywane podczas SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="e8512-175">Validation is performed after changes are detected during SaveChanges.</span></span> <span data-ttu-id="e8512-176">W przypadku wprowadzenia zmian podczas sprawdzania poprawności jest odpowiedzialny za powiadomienie śledzenie zmian.</span><span class="sxs-lookup"><span data-stu-id="e8512-176">If you make changes during validation it is your responsibility to notify the change tracker.</span></span>
-   <span data-ttu-id="e8512-177">DbUnexpectedValidationException jest generowany, jeśli występują błędy podczas sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="e8512-177">DbUnexpectedValidationException is thrown if errors occur during validation.</span></span>
-   <span data-ttu-id="e8512-178">Aspektami, które platformy Entity Framework zawiera w modelu (maksymalną długość wymaganych itp.) spowoduje, że sprawdzanie poprawności, nawet jeśli nie ma adnotacji danych w Twoich zajęciach i/lub projektancie platformy EF został użyty do utworzenia modelu.</span><span class="sxs-lookup"><span data-stu-id="e8512-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are not data annotations on your classes and/or you used the EF Designer to create your model.</span></span>
-   <span data-ttu-id="e8512-179">Reguły pierwszeństwa:</span><span class="sxs-lookup"><span data-stu-id="e8512-179">Precedence rules:</span></span>
    -   <span data-ttu-id="e8512-180">Wywołania interfejsu API Fluent zastąpienia odpowiedniego adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="e8512-180">Fluent API calls override the corresponding data annotations</span></span>
-   <span data-ttu-id="e8512-181">Kolejność wykonywania:</span><span class="sxs-lookup"><span data-stu-id="e8512-181">Execution order:</span></span>
    -   <span data-ttu-id="e8512-182">Właściwości sprawdzania poprawności jest wcześniejsza niż typ sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="e8512-182">Property validation occurs before type validation</span></span>
    -   <span data-ttu-id="e8512-183">Sprawdzanie poprawności typu tylko wtedy, gdy właściwość weryfikacja zakończy się powodzeniem</span><span class="sxs-lookup"><span data-stu-id="e8512-183">Type validation only occurs if property validation succeeds</span></span>
-   <span data-ttu-id="e8512-184">Jeśli właściwość jest złożony jego weryfikacji będzie również udostępniać program:</span><span class="sxs-lookup"><span data-stu-id="e8512-184">If a property is complex its validation will also include:</span></span>
    -   <span data-ttu-id="e8512-185">Właściwość poziom weryfikacji we właściwościach typu złożonego</span><span class="sxs-lookup"><span data-stu-id="e8512-185">Property-level validation on the complex type properties</span></span>
    -   <span data-ttu-id="e8512-186">Sprawdzanie poziomu typu na typ złożony, w tym weryfikacji IValidatableObject na typ złożony</span><span class="sxs-lookup"><span data-stu-id="e8512-186">Type level validation on the complex type, including IValidatableObject validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="e8512-187">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="e8512-187">Summary</span></span>

<span data-ttu-id="e8512-188">Sprawdzanie poprawności interfejsu API platformy Entity Framework odgrywa bardzo dobrze za pomocą weryfikacji po stronie klienta w MVC, ale nie trzeba polegać na weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="e8512-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="e8512-189">Zajmie się weryfikacji po stronie serwera dla DataAnnotations lub konfiguracje zostały zastosowane przy użyciu kodu pierwszy Fluent interfejsu API platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8512-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="e8512-190">Przedstawiono także szereg punkty rozszerzeń dostosowywania zachowania przy użyciu interfejsu IValidatableObject lub naciśnij tę wartość do metody DbContet.ValidateEntity.</span><span class="sxs-lookup"><span data-stu-id="e8512-190">You also saw a number of extensibility points for customizing the behavior whether you use the IValidatableObject interface or tap into the DbContet.ValidateEntity method.</span></span> <span data-ttu-id="e8512-191">I te ostatnie dwa sposób sprawdzania poprawności są dostępne za pośrednictwem typu DbContext, czy użyć Code First, pierwszego modelu lub bazy danych pierwszego przepływu pracy do opisania model koncepcyjny.</span><span class="sxs-lookup"><span data-stu-id="e8512-191">And these last two means of validation are available through the DbContext, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
