---
title: Wprowadzenie do platformy ASP.NET Core — istniejącej bazy danych — EF Core
author: rowanmiller
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 84e2e4bc1bdc774fa059fa893e0f8ac128931feb
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447186"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Wprowadzenie do programu EF Core programu ASP.NET Core z istniejącej bazy danych

W tym samouczku utworzysz aplikację ASP.NET Core MVC, który wykonuje podstawowe dane dostępu przy użyciu platformy Entity Framework Core. Możesz odtwarzać istniejącą bazę danych do tworzenia modelu Entity Framework.

[Wyświetlanie przykładowych w tym artykule w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).

## <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj następujące oprogramowanie:

* [Visual Studio 2017 w wersji 15.7](https://www.visualstudio.com/downloads/) za pomocą tych obciążeniach:
  * **ASP.NET i tworzenie aplikacji internetowych** (w obszarze **sieć Web i chmura**)
  * **Programowanie dla wielu platform .NET core** (w obszarze **inne zestawy narzędzi**)
* [.NET core 2.1 SDK](https://www.microsoft.com/net/download/core).

## <a name="create-blogging-database"></a>Utwórz bazę danych do obsługi blogów

W tym samouczku **do obsługi blogów** bazy danych w ramach wystąpienia LocalDb jako istniejącej bazy danych. Jeśli masz już utworzoną **do obsługi blogów** bazy danych jako część innego samouczek, należy pominąć tę procedurę.

* Otwórz program Visual Studio
* **Narzędzia -> nawiązać połączenie z bazą danych...**
* Wybierz **programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**
* Wprowadź **(localdb) \mssqllocaldb** jako **nazwy serwera**
* Wprowadź **wzorca** jako **Nazwa bazy danych** i kliknij przycisk **OK**
* Baza danych master jest teraz wyświetlany w obszarze **połączeń danych** w **Eksploratora serwera**
* Kliknij prawym przyciskiem myszy w bazie danych w **Eksploratora serwera** i wybierz **nowe zapytanie**
* Skopiuj skrypt w edytorze zapytań
* Kliknij prawym przyciskiem myszy w edytorze zapytań, a następnie wybierz pozycję **wykonania**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

* Otwórz program Visual Studio 2017
* **Plik > Nowy > Projekt...**
* Z menu po lewej stronie wybierz **zainstalowane > Visual C# > sieci Web**
* Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu projektu
* Wprowadź **EFGetStarted.AspNetCore.ExistingDb** jako nazwę i kliknij przycisk **OK**
* Poczekaj, aż **Nowa aplikacja internetowa ASP.NET Core** wyświetlać okno dialogowe
* Upewnij się, że menu rozwijanego platform docelowych jest ustawiona na **platformy .NET Core**, a lista rozwijana wersji jest ustawiona na **platformy ASP.NET Core 2.1**
* Wybierz **aplikacji sieci Web (Model-View-Controller)** szablonu
* Upewnij się, że **uwierzytelniania** ustawiono **bez uwierzytelniania**
* Kliknij przycisk **OK**

## <a name="install-entity-framework-core"></a>Instalowanie platformy Entity Framework Core

Aby zainstalować programu EF Core, należy zainstalować pakiet dla dostawców bazy danych programu EF Core, który ma pod kątem. Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md). 

Na potrzeby tego samouczka nie trzeba zainstalować pakiet dostawcy, ponieważ w tym samouczku użyto programu SQL Server. Pakiet dostawcy programu SQL Server znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

## <a name="reverse-engineer-your-model"></a>Model odtworzyć.

Teraz nadszedł czas na tworzenie modelu platformy EF, w oparciu o istniejącą bazę danych.

* **Narzędzia -> pakietu NuGet Manager -> Konsola Menedżera pakietów**
* Uruchom następujące polecenie, aby utworzyć model z istniejącej bazy danych:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Jeśli zostanie wyświetlony błąd wskazujący `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, a następnie zamknij i otwórz ponownie program Visual Studio.

> [!TIP]  
> Można określić tabel, które mają zostać wygenerowane jednostki dla, dodając `-Tables` argument polecenia powyżej. Na przykład `-Tables Blog,Post`.

Proces odtwarzania utworzony klas jednostek (`Blog.cs` & `Post.cs`) i pochodnej kontekstu (`BloggingContext.cs`) na podstawie schematu istniejącej bazy danych.

 Klasy jednostki są proste obiekty języka C#, które reprezentują będziesz zapytań i zapisywanie danych. Poniżej przedstawiono `Blog` i `Post` klas jednostek:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> Aby włączyć ładowanie z opóźnieniem, należy wybrać właściwości nawigacji `virtual` (Blog.Post i Post.Blog).

 Kontekst reprezentuje sesję z bazą danych i umożliwia zapytania i Zapisz wystąpień klas jednostek.

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a>Zarejestrowanie kontekst wstrzykiwanie zależności

Pojęcie wstrzykiwanie zależności stanowi podstawę do platformy ASP.NET Core. Usługi (takie jak `BloggingContext`) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług, (na przykład kontrolerach MVC) znajdują się następnie tych usług za pomocą właściwości lub parametry konstruktora. Aby uzyskać więcej informacji na temat wstrzykiwanie zależności zobacz [wstrzykiwanie zależności](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) artykuł w witrynie programu ASP.NET.

### <a name="register-and-configure-your-context-in-startupcs"></a>Rejestrowanie i konfigurowanie kontekstu w pliku Startup.cs

Zapewnienie `BloggingContext` dostępne dla kontrolerów MVC, zarejestruj go jako usługę.

* Otwórz **Startup.cs**
* Dodaj następujący kod `using` instrukcji na początku pliku

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Teraz możesz używać `AddDbContext(...)` metodę, aby zarejestrować go jako usługę.
* Znajdź `ConfigureServices(...)` — metoda
* Dodaj następujący wyróżniony kod, aby zarejestrować kontekst jako usługa

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> W rzeczywistej aplikacji zwykle przełączyć parametry połączenia w zmiennej środowisku lub pliku konfiguracji. W tym samouczku dla uproszczenia, ma możesz zdefiniować go w kodzie. Aby uzyskać więcej informacji, zobacz [parametry połączenia](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller-and-views"></a>Tworzenie kontrolera i widoki

* Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj -> kontrolera...**
* Wybierz **kontroler MVC z widokami używający narzędzia Entity Framework** i kliknij przycisk **Ok**
* Ustaw **klasa modelu** do **Blog** i **klasa kontekstu danych** do **BloggingContext**
* Kliknij przycisk **Dodaj**

## <a name="run-the-application"></a>Uruchamianie aplikacji

Teraz można uruchomić aplikacji, aby zobaczyć go w działaniu.

* **Debuguj -> Start bez debugowania**
* Aplikacja zostanie skompilowana i zostanie otwarta w przeglądarce sieci web
* Przejdź do `/Blogs`
* Kliknij przycisk **Utwórz nową**
* Wprowadź **adresu Url** nowego bloga, a następnie kliknij przycisk **Create**

  ![Tworzenie strony](_static/create.png)

  ![Strona indeksu](_static/index-existing-db.png)

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat sposobu tworzenia szkieletu klasy kontekstu i jednostek, zobacz następujące artykuły:
* [Entity Framework Core odnoszą się narzędzia — interfejs wiersza polecenia platformy .NET](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [Entity Framework Core odnoszą się narzędzia — Konsola Menedżera pakietów](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)