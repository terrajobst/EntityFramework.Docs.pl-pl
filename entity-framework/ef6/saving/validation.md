---
title: Sprawdzanie poprawności — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 65639b0f91f54ee2cd1336f6b6cd4caf45ede680
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251027"
---
# <a name="data-validation"></a>Sprawdzanie poprawności danych
> [!NOTE]
> **EF4.1 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 4.1. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania

Zawartość na tej stronie są zaczerpnięte z, a artykuł pierwotnie napisane przez Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).

Entity Framework udostępnia wiele różnych funkcji sprawdzania poprawności, które może przekazywać za pośrednictwem interfejsu użytkownika w celu weryfikacji po stronie klienta lub można użyć do weryfikacji po stronie serwera. Po raz pierwszy przy użyciu kodu, można określić operacji sprawdzania poprawności przy użyciu adnotacji lub fluent konfiguracji interfejsu API. Dodatkowe sprawdzanie poprawności i bardziej złożone, można określić w kodzie i będzie działać, czy model ma za sobą kodu po pierwsze, najpierw modelu lub najpierw bazy danych.

## <a name="the-model"></a>Model

Zademonstruję walidacji przy użyciu prostego pary klas: Blog i Post.

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
## <a name="data-annotations"></a>Adnotacje danych

Kod używa pierwsze adnotacje z zestawu System.ComponentModel.DataAnnotations jako jednym ze sposobów konfigurowania kodu pierwszej klasy. Wśród tych adnotacji są tymi, które zapewniają reguły, takie jak wymaganych, MaxLength, MinLength. Liczba aplikacji klienta .NET rozpoznaje również tych adnotacji, na przykład platformy ASP.NET MVC. Obie strony serwera i weryfikacji po stronie klienta przy użyciu tych adnotacji można osiągnąć. Na przykład możesz wymusić właściwości Tytuł blogu wymaganej właściwości.

``` csharp
    [Required]
    public string Title { get; set; }
```

Bez dodatkowego kodu i znaczników zmiany w aplikacji istniejącej aplikacji MVC przeprowadzi weryfikację po stronie klienta, nawet dynamiczne tworzenie komunikat przy użyciu nazwy właściwości i adnotacji.

![Rysunek 1.](~/ef6/media/figure01.png)

We wpisie kopii metody tego widoku Create, platformy Entity Framework jest używany do zapisywania nowego bloga w bazie danych, ale weryfikacji po stronie klienta dla platformy MVC jest wyzwalany, zanim aplikacja osiągnie ten kod.

Weryfikacji po stronie klienta nie jest jednak punktor dowodu. Użytkownicy mogą mieć wpływ na funkcje przeglądarki lub co gorsza, haker może używać niektórych trickery w celu uniknięcia walidacji interfejsu użytkownika. Ale Entity Framework rozpoznaje adnotację wymagane również i zweryfikuje go.

Jest to prosty sposób testować tę aplikację można wyłączyć funkcję weryfikacji po stronie klienta w MVC. Można to zrobić w pliku web.config aplikacji MVC. W sekcji appSettings ma klucz dla ClientValidationEnabled. Ustawienie wartości false dla tego klucza uniemożliwi interfejs użytkownika wykonywania operacji sprawdzania poprawności.

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

Nawet w przypadku weryfikacji po stronie klienta, wyłączona uzyskasz tę samą odpowiedź do aplikacji. Komunikat o błędzie "Tytuł pole jest wymagane" będą wyświetlane jako. Z wyjątkiem teraz będą wynik weryfikacji po stronie serwera. Entity Framework będzie wykonywać sprawdzanie poprawności adnotację, wymagane (przed nawet bothers kompilacji i Wstaw polecenie, aby wysłać do bazy danych) i zwrócenie błędu do MVC, co spowoduje wyświetlenie komunikatu.

## <a name="fluent-api"></a>Interfejs Fluent API

Możesz użyć kodu przez pierwszy interfejs fluent API zamiast adnotacje do Uzyskaj tego samego klienta po stronie serwera po stronie sprawdzania poprawności. Zamiast użycia wymagane, czy pokazano tej weryfikacji MaxLength przy użyciu.

Fluent API konfiguracje są stosowane w kodzie najpierw jest Budowanie modelu z klas. Konfiguracje można wstrzyknąć poprzez zastąpienie metody OnModelCreating klasy DbContext. Poniżej przedstawiono konfigurację, określając, że właściwość BloggerName nie może być dłuższa niż 10 znaków.

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

Błędy sprawdzania poprawności generowany na podstawie konfiguracji interfejsu API Fluent nie będzie automatycznie zasięg interfejsu użytkownika, ale można przechwytywać go w kodzie i następnie odpowiedź do niego odpowiednio.

Oto niektóre błąd kodu w klasie BlogController aplikacji, która przechwytuje ten błąd sprawdzania poprawności, gdy Entity Framework próbuje zapisać blog BloggerName, która przekracza maksymalną liczbę znaków 10 obsługi wyjątków.

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

Sprawdzanie poprawności nie Pobierz automatycznie przekazywane do widoku, dlatego dodatkowy kod, który używa ModelState.AddModelError jest używany. Daje to gwarancję, że szczegóły błędu przenoszone do widoku, który zostanie użyty ValidationMessageFor Htmlhelper do wyświetlenia błędu.

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

IValidatableObject jest interfejsem, który znajduje się w System.ComponentModel.DataAnnotations. Mimo że nie jest częścią interfejsu API programu Entity Framework, można nadal korzystać z go do weryfikacji po stronie serwera w Twoich zajęciach Entity Framework. IValidatableObject udostępnia metodę sprawdzania poprawności, która wywoła Entity Framework podczas SaveChanges lub możesz wywołać samodzielnie ilekroć, którą chcesz zweryfikować klasy.

Konfiguracje, takie jak wymagane i MaxLength wykonać validaton według jednego pola. W metodzie sprawdzania poprawności może być jeszcze bardziej złożonej logiki, na przykład porównanie dwóch pól.

W poniższym przykładzie klasa blogu został rozszerzony do zaimplementowania IValidatableObject, a następnie podaj regułę, która tytuł i BloggerName nie może być zgodna.

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

Konstruktor ValidationResult przyjmuje ciąg reprezentujący komunikat o błędzie i Tablica ciągów, które reprezentują nazwy elementów członkowskich, które są skojarzone z weryfikacji. Ponieważ ta weryfikacja, tytuł i BloggerName, zwracane są obie nazwy właściwości.

W przeciwieństwie do sprawdzania poprawności, udostępniony przez interfejs Fluent API ten wynik sprawdzania poprawności zostanie rozpoznane przez widok, a program obsługi wyjątku, który wcześniej używane do dodawania błąd do ModelState nie jest konieczne. Ponieważ obie nazwy właściwości ustawione w ValidationResult, MVC HtmlHelpers wyświetlić komunikat o błędzie dla obu tych właściwości.

![Rysunek 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext.ValidateEntity

Kontekst DbContext ma metodę z możliwością o nazwie ValidateEntity. Po wywołaniu funkcji SaveChanges Entity Framework będzie wywoływać tej metody dla każdej jednostki w pamięci podręcznej, którego stan nie jest bez zmian. Logikę weryfikacji można umieścić bezpośrednio w tutaj lub nawet użycie tej metody do wywołania, na przykład metoda Blog.Validate dodany w poprzedniej sekcji.

Oto przykład zastąpienia ValidateEntity, która weryfikuje nowych wpisów, aby upewnić się, że tytuł wpisu nie został już użyty. Go najpierw sprawdza, czy jednostka jest wpis i że jego stan zostanie dodany. Jeśli tak jest rzeczywiście, następnie szuka w bazie danych, aby sprawdzić, czy jest już wpis o tej samej nazwie. Jeśli istniejący wpis już istnieje, jest tworzony nowy DbEntityValidationResult.

DbEntityValidationResult przechowuje DbEntityEntry i ICollection DbValidationErrors dla pojedynczej jednostki. Na początku tej metody DbEntityValidationResult zostanie uruchomiony, a następnie wszelkie błędy, które zostały wykryte zostaną dodane do swojej kolekcji ValidationErrors.

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

## <a name="explicitly-triggering-validation"></a>Jawnie wyzwalania sprawdzania poprawności

Po wywołaniu funkcji SaveChanges wyzwala wszystkie operacje sprawdzania poprawności omówione w tym artykule. Ale nie trzeba polegać na SaveChanges. Możesz sprawdzić w innym miejscu w aplikacji.

DbContext.GetValidationErrors wyzwoli wszystkie operacje sprawdzania poprawności, te określone przez adnotacje lub interfejsu API Fluent, sprawdzanie poprawności utworzonych w IValidatableObject (na przykład Blog.Validate) i sprawdzeń wykonywane w DbContext.ValidateEntity Metoda.

Poniższy kod wywoła GetValidationErrors na bieżącym wystąpieniu typu DbContext. ValidationErrors są pogrupowane według typu jednostki do DbValidationRestuls. Kod wykonuje iterację najpierw za pomocą DbValidationResults zwracany przez metodę, a następnie za pomocą każdego ValidationError wewnątrz.

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

## <a name="other-considerations-when-using-validation"></a>Inne uwagi dotyczące korzystania z weryfikacji

Poniżej przedstawiono kilka kwestii do rozważenia podczas korzystania z programu Entity Framework sprawdzania poprawności:

-   Powolne ładowanie jest wyłączone podczas sprawdzania poprawności.
-   EF zostanie przeprowadzona Weryfikacja adnotacje danych na temat właściwości — zamapowany (właściwości, które nie są zamapowane na kolumnę w bazie danych).
-   Sprawdzanie poprawności jest wykonywane po zmiany są wykrywane podczas SaveChanges. W przypadku wprowadzenia zmian podczas sprawdzania poprawności jest odpowiedzialny za powiadomienie śledzenie zmian.
-   DbUnexpectedValidationException jest generowany, jeśli występują błędy podczas sprawdzania poprawności.
-   Aspektami, które platformy Entity Framework zawiera w modelu (maksymalną długość wymaganych itp.) spowoduje, że sprawdzanie poprawności, nawet jeśli nie ma adnotacji danych w Twoich zajęciach i/lub projektancie platformy EF został użyty do utworzenia modelu.
-   Reguły pierwszeństwa:
    -   Wywołania interfejsu API Fluent zastąpienia odpowiedniego adnotacji danych
-   Kolejność wykonywania:
    -   Właściwości sprawdzania poprawności jest wcześniejsza niż typ sprawdzania poprawności
    -   Sprawdzanie poprawności typu tylko wtedy, gdy właściwość weryfikacja zakończy się powodzeniem
-   Jeśli właściwość jest złożony jego weryfikacji będzie również udostępniać program:
    -   Właściwość poziom weryfikacji we właściwościach typu złożonego
    -   Sprawdzanie poziomu typu na typ złożony, w tym weryfikacji IValidatableObject na typ złożony

## <a name="summary"></a>Podsumowanie

Sprawdzanie poprawności interfejsu API platformy Entity Framework odgrywa bardzo dobrze za pomocą weryfikacji po stronie klienta w MVC, ale nie trzeba polegać na weryfikacji po stronie klienta. Zajmie się weryfikacji po stronie serwera dla DataAnnotations lub konfiguracje zostały zastosowane przy użyciu kodu pierwszy Fluent interfejsu API platformy Entity Framework.

Przedstawiono także szereg punkty rozszerzeń dostosowywania zachowania przy użyciu interfejsu IValidatableObject lub naciśnij tę wartość do metody DbContet.ValidateEntity. I te ostatnie dwa sposób sprawdzania poprawności są dostępne za pośrednictwem typu DbContext, czy użyć Code First, pierwszego modelu lub bazy danych pierwszego przepływu pracy do opisania model koncepcyjny.
