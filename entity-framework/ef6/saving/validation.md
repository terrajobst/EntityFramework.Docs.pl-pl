---
title: Walidacja — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 457af0c32f0fe4804dbfe6e348664efb1af517c9
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565378"
---
# <a name="data-validation"></a>Sprawdzanie poprawności danych
> [!NOTE]
> **Dr 4.1 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 4,1. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie mają zastosowania

Zawartość na tej stronie jest dostosowana z artykułu utworzonego pierwotnie przez Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).

Entity Framework zapewnia szeroką gamę funkcji walidacji, które mogą przechodzić do interfejsu użytkownika w celu weryfikacji po stronie klienta lub służy do sprawdzania poprawności po stronie serwera. Używając najpierw kodu, można określić walidacji przy użyciu adnotacji lub konfiguracji interfejsu API Fluent. Dodatkowe walidacje i bardziej skomplikowane, można określić w kodzie i będzie działać niezależnie od tego, czy model przyniesie najpierw od kodu, najpierw modelu czy bazy danych.

## <a name="the-model"></a>Model

Zaprezentowanie walidacji przy użyciu prostej pary klas: Blog i wpis.

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

## <a name="data-annotations"></a>Adnotacje danych

Code First używa adnotacji z `System.ComponentModel.DataAnnotations` zestawu jako jednego środka konfigurowania pierwszej klasy kodowej. Wśród tych adnotacji są te `Required`, które dostarczają reguły, takie jak, `MaxLength` i `MinLength`. Wiele aplikacji klienckich platformy .NET rozpoznaje również te adnotacje, na przykład ASP.NET MVC. Po stronie klienta i po stronie serwera można wykonać te adnotacje. Na przykład można wymusić, aby Właściwość tytuł bloga była właściwością wymaganą.

``` csharp
[Required]
public string Title { get; set; }
```

W przypadku braku dodatkowych zmian kodu lub znaczników w aplikacji istniejąca aplikacja MVC przeprowadzi weryfikację po stronie klienta, nawet dynamicznie kompilując komunikat przy użyciu nazw właściwości i adnotacji.

![Rysunek 1](~/ef6/media/figure01.png)

W metodzie post tego widoku Utwórz Entity Framework służy do zapisywania nowego bloga w bazie danych, ale Walidacja po stronie klienta MVC jest wyzwalana, zanim aplikacja osiągnie ten kod.

Sprawdzanie poprawności po stronie klienta nie jest jednak niezgodne z punktorem. Użytkownicy mogą mieć wpływ na funkcje przeglądarki lub jeszcze gorszy, haker może użyć pewnych lew, aby uniknąć walidacji interfejsu użytkownika. Ale Entity Framework również rozpozna `Required` adnotację i zweryfikuje ją.

Prostym sposobem na przetestowanie jest wyłączenie funkcji walidacji po stronie klienta. Można to zrobić w pliku Web. config aplikacji MVC. Sekcja appSettings ma klucz dla ClientValidationEnabled. Ustawienie tego klucza na wartość false uniemożliwi interfejsowi użytkownika wykonywanie walidacji.

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

Nawet po wyłączeniu weryfikacji po stronie klienta otrzymasz taką samą odpowiedź w aplikacji. Komunikat o błędzie "pole Tytuł jest wymagane" będzie wyświetlany jako poprzednio. Z tego powodu będzie można sprawdzić poprawność po stronie serwera. Entity Framework przeprowadzi weryfikację w `Required` adnotacji (zanim będzie mogła nawet jednocześnie `INSERT` utworzyć polecenie do wysłania do bazy danych) i zwrócić błąd do MVC, który wyświetli komunikat.

## <a name="fluent-api"></a>Interfejs API Fluent

Możesz użyć interfejsu API Fluent pierwszego kodu zamiast adnotacji, aby uzyskać tę samą stronę klienta & weryfikacji po stronie serwera. Zamiast używać `Required`, pokażę to przy użyciu walidacji MaxLength.

Konfiguracje interfejsu API Fluent są stosowane, ponieważ kod najpierw kompiluje model z klas. Konfiguracje można wstrzyknąć przez zastąpienie metody OnModelCreating DbContext klasy. Poniżej znajduje się Konfiguracja określająca, że właściwość Bloggername nie może być dłuższa niż 10 znaków.

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

Błędy sprawdzania poprawności zgłoszone na podstawie konfiguracji interfejsu API Fluent nie będą automatycznie docierać do interfejsu użytkownika, ale można je przechwycić w kodzie, a następnie odpowiedzieć odpowiednio.

Poniżej przedstawiono kod błędu obsługi wyjątków w klasie BlogController aplikacji, który przechwytuje ten błąd sprawdzania poprawności, gdy Entity Framework próbuje zapisać blog o wartości większej niż 10 znaków.

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

Sprawdzanie poprawności nie jest automatycznie przenoszone do widoku, który jest używany przez inny kod, który `ModelState.AddModelError` używa. Gwarantuje to, że szczegóły błędu przyczynią go do widoku, który następnie będzie `ValidationMessageFor` używać Htmlhelper do wyświetlania błędu.

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

`IValidatableObject`jest interfejsem, który znajduje `System.ComponentModel.DataAnnotations`się w. Chociaż nie jest on częścią interfejsu API Entity Framework, można nadal korzystać z niego do walidacji po stronie serwera w klasach Entity Framework. `IValidatableObject``Validate` zapewnia metodę, która Entity Framework będzie wywoływana podczas metody savechangesu lub można wywołać siebie w dowolnym momencie, aby sprawdzić poprawność klas.

Konfiguracje takie jak `Required` i `MaxLength` wykonywanie walidacji dla jednego pola. `Validate` W metodzie można mieć jeszcze bardziej złożoną logikę, na przykład porównując dwa pola.

W poniższym przykładzie `Blog` Klasa została rozszerzona w celu zaimplementowania `IValidatableObject` , a następnie `Title` Podaj regułę, która `BloggerName` nie może być zgodna.

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

`ValidationResult` Konstruktor przyjmuje`string`komunikat o błędzie i tablicę, która reprezentuje nazwy elementów członkowskich, które są skojarzone z walidacją. `string` Ponieważ ta Walidacja sprawdza zarówno `Title` `BloggerName`, jak i obie nazwy właściwości są zwracane.

W przeciwieństwie do walidacji dostarczonej przez interfejs API Fluent, ten wynik sprawdzania poprawności zostanie rozpoznany przez widok i program obsługi wyjątków, który został wcześniej użyty do dodania `ModelState` błędu do nie jest zbędny. Ponieważ ustawiam obie nazwy właściwości w `ValidationResult`, MVC HtmlHelpers wyświetla komunikat o błędzie dla obu tych właściwości.

![Rysunek 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext. ValidateEntity

`DbContext`ma metodę `ValidateEntity`z możliwością zastąpienia. Po wywołaniu `SaveChanges`, Entity Framework wywoła tę metodę dla każdej jednostki w pamięci podręcznej, której stan nie `Unchanged`jest. Logikę walidacji można umieścić bezpośrednio w tym miejscu, a nawet użyć tej metody do wywołania, na `Blog.Validate` przykład, metody dodanej w poprzedniej sekcji.

Oto przykład `ValidateEntity` przesłonięcia, który sprawdza poprawność `Post`nowych s, aby upewnić się, że tytuł wpisu nie został już użyty. Najpierw sprawdza, czy jednostka jest wpisem i czy jego stan jest dodawany. W takim przypadku szuka w bazie danych, czy istnieje już wpis o takim samym tytule. Jeśli istnieje już istniejący wpis, zostanie utworzony nowy `DbEntityValidationResult` .

`DbEntityValidationResult``DbEntityEntry` domy aia`ICollection<DbValidationErrors>` dla jednej jednostki. Na początku tej metody wystąpienie `DbEntityValidationResult` jest tworzone, a następnie wszystkie wykryte błędy są dodawane `ValidationErrors` do kolekcji.

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

## <a name="explicitly-triggering-validation"></a>Jawne wyzwalanie walidacji

Wywołanie `SaveChanges` wyzwalające wszystkie walidacje omówione w tym artykule. Nie trzeba jednak polegać na `SaveChanges`. Możesz chcieć sprawdzić poprawność w innym miejscu aplikacji.

`DbContext.GetValidationErrors`spowoduje wyzwolenie wszystkich walidacji, które są zdefiniowane przez adnotacje lub interfejs API Fluent, sprawdzanie poprawności `IValidatableObject` utworzone w (na `Blog.Validate`przykład) i walidacje wykonane w `DbContext.ValidateEntity` metodzie.

Poniższy kod wywoła wywołanie `GetValidationErrors` w bieżącym wystąpieniu obiektu. `DbContext` `ValidationErrors`są pogrupowane według typu jednostki `DbEntityValidationResult`w. Kod wykonuje iterację najpierw za pomocą `DbEntityValidationResult`s zwracanego przez metodę, a następnie za `DbValidationError` pomocą każdego z nich.

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

## <a name="other-considerations-when-using-validation"></a>Inne zagadnienia dotyczące korzystania z walidacji

Oto kilka innych punktów, które należy wziąć pod uwagę podczas korzystania z Entity Framework weryfikacji:

- Ładowanie z opóźnieniem jest wyłączone podczas walidacji
- EF sprawdzi poprawność adnotacji danych dla niemapowanych właściwości (właściwości, które nie są zamapowane do kolumny w bazie danych).
- Walidacja jest wykonywana po wykryciu zmian `SaveChanges`w trakcie. Jeśli wprowadzisz zmiany podczas weryfikacji, odpowiadasz za powiadomienie śledzenia zmian
- `DbUnexpectedValidationException`jest zgłaszany w przypadku wystąpienia błędów podczas walidacji
- Aspekty Entity Framework uwzględnione w modelu (maksymalna długość, wymagana itp.) spowodują walidację, nawet jeśli nie ma adnotacji danych w klasach i/lub użyto projektanta EF do utworzenia modelu
- Reguły pierwszeństwa:
  - Wywołania interfejsu API Fluent przesłaniają odpowiednie adnotacje danych
- Kolejność wykonywania:
  - Walidacja właściwości występuje przed walidacją typu
  - Walidacja typu występuje tylko wtedy, gdy Walidacja właściwości zostanie zakończona pomyślnie
- Jeśli właściwość jest złożona, jego Walidacja również będzie obejmować:
  - Walidacja na poziomie właściwości we właściwościach typu złożonego
  - Walidacja poziomu typu dla typu złożonego, `IValidatableObject` w tym walidacji typu złożonego

## <a name="summary"></a>Podsumowanie

Interfejs API sprawdzania poprawności w Entity Framework odgrywa dobrze z walidacją po stronie klienta w MVC, ale nie musisz polegać na weryfikacji po stronie klienta. Entity Framework będzie obsłużyć walidację po stronie serwera dla adnotacji lub konfiguracji zastosowanej przy użyciu kodu interfejsu API pierwszego Fluent.

Przedstawiono także szereg punktów rozszerzalności służących do dostosowywania zachowania, niezależnie od tego, `IValidatableObject` czy jest używany interfejs, `DbContext.ValidateEntity` czy też do metody. Te ostatnie dwa sposoby weryfikacji są dostępne za pośrednictwem programu `DbContext`, niezależnie od tego, czy do opisania modelu koncepcyjnego jest używany przepływ pracy Code First, model First czy Database First.
