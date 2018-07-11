---
title: Wprowadzenie do platformy ASP.NET Core — istniejącej bazy danych — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: e28149346ccd7531449ea696505588317471e6dd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949156"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Wprowadzenie do programu EF Core programu ASP.NET Core z istniejącej bazy danych

W tym instruktażu utworzysz aplikację ASP.NET Core MVC, który wykonuje podstawowe dane access używający narzędzia Entity Framework. Odtwarzanie użyje do tworzenia modelu Entity Framework, w oparciu o istniejącą bazę danych.

> [!TIP]  
> Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) w witrynie GitHub.

## <a name="prerequisites"></a>Wymagania wstępne

Do przeprowadzenia tego instruktażu potrzebne są następujące wymagania wstępne:

* [Program Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) za pomocą tych obciążeniach:
  * **ASP.NET i tworzenie aplikacji internetowych** (w obszarze **sieć Web i chmura**)
  * **Programowanie dla wielu platform .NET core** (w obszarze **inne zestawy narzędzi**)
* [.NET core 2.0 SDK](https://www.microsoft.com/net/download/core).
* [Do obsługi blogów bazy danych](#blogging-database)

### <a name="blogging-database"></a>Do obsługi blogów bazy danych

W tym samouczku **do obsługi blogów** bazy danych w ramach wystąpienia LocalDb jako istniejącej bazy danych.

> [!TIP]  
> Jeśli masz już utworzoną **do obsługi blogów** bazy danych jako część innym samouczku, można pominąć te kroki.

* Otwórz program Visual Studio
* **Narzędzia -> nawiązać połączenie z bazą danych...**
* Wybierz **programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**
* Wprowadź **(localdb) \mssqllocaldb** jako **nazwy serwera**
* Wprowadź **wzorca** jako **Nazwa bazy danych** i kliknij przycisk **OK**
* Baza danych master jest teraz wyświetlany w obszarze **połączeń danych** w **Eksploratora serwera**
* Kliknij prawym przyciskiem myszy w bazie danych w **Eksploratora serwera** i wybierz **nowe zapytanie**
* Skopiuj skrypt wymienione poniżej pod warunkiem do edytora zapytań
* Kliknij prawym przyciskiem myszy w edytorze zapytań, a następnie wybierz pozycję **wykonania**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

* Otwórz program Visual Studio 2017
* **Plik -> Nowy -> Projekt...**
* Z menu po lewej stronie wybierz **zainstalowane -> Szablony -> Visual C# -> Sieć Web**
* Wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)** szablonu projektu
* Wprowadź **EFGetStarted.AspNetCore.ExistingDb** jako nazwę i kliknij przycisk **OK**
* Poczekaj, aż **Nowa aplikacja internetowa ASP.NET Core** wyświetlać okno dialogowe
* W obszarze **platformy ASP.NET Core szablonów w wersji 2.0** wybierz **aplikacji sieci Web (Model-View-Controller)**
* Upewnij się, że **uwierzytelniania** ustawiono **bez uwierzytelniania**
* Kliknij przycisk **OK**

## <a name="install-entity-framework"></a>Instalowanie programu Entity Framework

Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem. W tym przewodniku używa programu SQL Server. Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).

* **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Firma Microsoft będzie używać pewnych narzędzi Entity Framework Tools do utworzenia modelu z bazy danych. Dlatego firma Microsoft zainstaluje również za pomocą pakietu narzędzi:

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`

Firma Microsoft będzie używać niektórych narzędzi funkcja tworzenia szkieletu ASP.NET Core do tworzenia widoków i kontrolerów później. Dlatego firma Microsoft zainstaluje tego pakietu projektu:

* Uruchom `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

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

 Klasy jednostki są proste obiekty języka C#, które reprezentują będziesz zapytań i zapisywanie danych.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 Kontekst reprezentuje sesję z bazą danych i umożliwia zapytania i Zapisz wystąpień klas jednostek.

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
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

### <a name="remove-inline-context-configuration"></a>Usuń konfigurację kontekstu wbudowane

W programie ASP.NET Core konfiguracji jest zazwyczaj wykonywane w **Startup.cs**. Z tego wzorca, firma Microsoft przeniesie konfiguracji dostawcy bazy danych do **Startup.cs**.

* Otwórz `Models\BloggingContext.cs`
* Usuń `OnConfiguring(...)` — metoda

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* Dodaj następujący Konstruktor, co umożliwi użytkownikom konfiguracji, które zostaną przekazane do kontekstu przez wstrzykiwanie zależności

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a>Rejestrowanie i konfigurowanie kontekstu w pliku Startup.cs

Aby nasze kontrolerów MVC użytkowania `BloggingContext` pobierzemy zarejestrował ją jako usługę.

* Otwórz **Startup.cs**
* Dodaj następujący kod `using` instrukcji na początku pliku

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Teraz możemy użyć `AddDbContext(...)` metodę, aby zarejestrować go jako usługę.
* Znajdź `ConfigureServices(...)` — metoda
* Dodaj następujący kod, aby zarejestrować kontekst jako usługa

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> W rzeczywistej aplikacji zwykle przełączyć parametry połączenia w pliku konfiguracji. Dla uproszczenia firma Microsoft jest definiowanie w kodzie. Aby uzyskać więcej informacji, zobacz [parametry połączenia](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller"></a>Tworzenie kontrolera

Firma Microsoft będzie następnie Włącz tworzenie szkieletów w projekcie.

* Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj -> kontrolera...**
* Wybierz **pełną zależności** i kliknij przycisk **Dodaj**
* Można zignorować zgodnie z instrukcjami w `ScaffoldingReadMe.txt` pliku, który zostanie otwarty

Teraz, gdy jest włączona funkcja szkieletów, firma Microsoft tworzenia szkieletu kontrolera dla `Blog` jednostki.

* Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj -> kontrolera...**
* Wybierz **kontroler MVC z widokami używający narzędzia Entity Framework** i kliknij przycisk **Ok**
* Ustaw **klasa modelu** do **Blog** i **klasa kontekstu danych** do **BloggingContext**
* Kliknij przycisk **Dodaj**

## <a name="run-the-application"></a>Uruchamianie aplikacji

Teraz można uruchomić aplikacji, aby zobaczyć go w działaniu.

* **Debuguj -> Start bez debugowania**
* Utworzysz aplikację i Otwórz w przeglądarce sieci web
* Przejdź do `/Blogs`
* Kliknij przycisk **Utwórz nową**
* Wprowadź **adresu Url** nowego bloga, a następnie kliknij przycisk **Create**

![obraz](_static/create.png)

![obraz](_static/index-existing-db.png)
