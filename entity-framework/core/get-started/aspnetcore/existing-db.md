---
title: Wprowadzenie do ASP.NET Core — istniejąca baza danych — EF Core
author: rowanmiller
description: Wprowadzenie do EF Core na ASP.NET Core z istniejącą bazą danych
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 6b0ed0a9222644bee31d23234aa27b2084137f4a
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497511"
---
# <a name="get-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Wprowadzenie do EF Core na ASP.NET Core z istniejącą bazą danych

W tym samouczku utworzysz aplikację ASP.NET Core MVC, która wykonuje podstawowy dostęp do danych przy użyciu Entity Framework Core. Odtwarzanie istniejącej bazy danych w celu utworzenia modelu Entity Framework.

[Zapoznaj się z przykładem tego artykułu w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).

## <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj następujące oprogramowanie:

* [Program Visual Studio 2017 15,7](https://www.visualstudio.com/downloads/) z następującymi obciążeniami:
  * **ASP.NET i programowanie dla sieci Web** (w obszarze **chmura & sieci Web**)
  * **Programowanie dla wielu platform w środowisku .NET Core** (w obszarze **inne zestawy narzędzi**)
* [Zestaw SDK platformy .NET Core 2,1](https://www.microsoft.com/net/download/core).

## <a name="create-blogging-database"></a>Tworzenie bazy danych blogów

W tym samouczku jest używany baza danych **blogów** w wystąpieniu LocalDB jako istniejąca baza danych. Jeśli baza danych **blogów** została już utworzona w ramach innego samouczka, Pomiń te kroki.

* Otwórz program Visual Studio
* **Narzędzia — > połączyć się z bazą danych...**
* Wybierz **Microsoft SQL Server** i kliknij przycisk **Kontynuuj** .
* Wprowadź **(LocalDB) \mssqllocaldb** jako **nazwę serwera**
* Wprowadź **wzorzec** jako **nazwę bazy danych** , a następnie kliknij przycisk **OK** .
* Baza danych Master jest teraz wyświetlana w obszarze **połączenia danych** w **Eksplorator serwera**
* Kliknij prawym przyciskiem myszy bazę danych w **Eksplorator serwera** a następnie wybierz pozycję **nowe zapytanie** .
* Skopiuj skrypt wymieniony poniżej do edytora zapytań
* Kliknij prawym przyciskiem myszy Edytor zapytań i wybierz polecenie **Execute (wykonaj** ).

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

* Open Visual Studio 2017
* **Plik > Nowy > projektu...**
* Z menu po lewej stronie wybierz pozycję **zainstalowane C# > Visual > Web**
* Wybierz szablon projektu **aplikacji sieci Web ASP.NET Core**
* Wprowadź **EFGetStarted. AspNetCore. ExistingDb** jako nazwę (musi dokładnie pasować do przestrzeni nazw, która została później użyta w kodzie), i kliknij przycisk **OK** . 
* Zaczekaj na wyświetlenie okna dialogowego **Nowa aplikacja sieci Web ASP.NET Core**
* Upewnij się, że na liście rozwijanej platformy docelowej jest ustawiona wartość **.NET Core**, a na liście rozwijanej wersji jest ustawiona wartość **ASP.NET Core 2,1**
* Wybierz szablon **aplikacja sieci Web (Model-View-Controller)**
* Upewnij się, że **uwierzytelnianie** jest ustawione na wartość **bez uwierzytelniania**
* Kliknij przycisk **OK**

## <a name="install-entity-framework-core"></a>Zainstaluj Entity Framework Core

Aby zainstalować EF Core, należy zainstalować pakiet dla dostawców usługi EF Core Database, które mają być przeznaczone do celów. Aby uzyskać listę dostępnych dostawców, zobacz [dostawcy bazy danych](../../providers/index.md). 

W tym samouczku nie trzeba instalować pakietu dostawcy, ponieważ samouczek używa SQL Server. Pakiet dostawcy SQL Server jest zawarty w [pakiecie Microsoft. AspnetCore. app](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

## <a name="reverse-engineer-your-model"></a>Odtwarzanie modelu

Teraz można utworzyć model EF na podstawie istniejącej bazy danych.

* **Narzędzia — > Menedżer pakietów NuGet — > konsoli Menedżera pakietów**
* Uruchom następujące polecenie, aby utworzyć model z istniejącej bazy danych:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Jeśli zostanie wyświetlony komunikat o błędzie z `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`informacją, Zamknij i ponownie otwórz program Visual Studio.

> [!TIP]  
> Możesz określić, które tabele mają być generowane przez dodanie `-Tables` argumentu do powyższego polecenia. Na przykład `-Tables Blog,Post`.

Proces odtwarzania`Blog.cs`utworzył klasy jednostek ( & `Post.cs`) i kontekst pochodny (`BloggingContext.cs`) na podstawie schematu istniejącej bazy danych.

 Klasy jednostek są prostymi C# obiektami, które reprezentują dane, które będą używane do wykonywania zapytań i zapisywania. Oto klasy jednostek `Post`i: `Blog`

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> Aby włączyć ładowanie z opóźnieniem, możesz tworzyć właściwości `virtual` nawigacji (blog.post i post).

 Kontekst reprezentuje sesję z bazą danych i umożliwia wykonywanie zapytań i zapisywanie wystąpień klas jednostek.

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

## <a name="register-your-context-with-dependency-injection"></a>Rejestrowanie kontekstu z iniekcją zależności

Pojęcie iniekcji zależności jest środkowe do ASP.NET Core. Usługi (takie jak `BloggingContext`) są rejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (takich jak kontrolery MVC), są następnie udostępniane przez parametry lub właściwości konstruktora. Aby uzyskać więcej informacji na temat iniekcji zależności, zobacz artykuł [iniekcja zależności](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) w witrynie ASP.NET.

### <a name="register-and-configure-your-context-in-startupcs"></a>Rejestrowanie i Konfigurowanie kontekstu w programie Startup.cs

Aby udostępnić `BloggingContext` kontrolery MVC, zarejestruj je jako usługę.

* Otwórz **Startup.cs**
* Dodaj następujące `using` instrukcje na początku pliku

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Teraz możesz użyć `AddDbContext(...)` metody, aby zarejestrować ją jako usługę.
* `ConfigureServices(...)` Znajdowanie metody
* Dodaj następujący wyróżniony kod, aby zarejestrować kontekst jako usługę

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> W prawdziwej aplikacji zwykle umieszcza się parametry połączenia w pliku konfiguracyjnym lub zmiennej środowiskowej. Dla uproszczenia ten samouczek został zdefiniowany w kodzie. Aby uzyskać więcej informacji, zobacz [Parametry połączenia](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller-and-views"></a>Tworzenie kontrolera i widoków

* Kliknij prawym przyciskiem myszy  folder controllers w **Eksplorator rozwiązań** a następnie wybierz pozycję **Dodaj > kontroler...**
* Wybierz **kontroler MVC z widokami przy użyciu Entity Framework** i kliknij przycisk **OK** .
* Ustaw **klasę modelu** na **blog** i **klasę kontekstu danych** na **BloggingContext**
* Kliknij przycisk **Dodaj**

## <a name="run-the-application"></a>Uruchamianie aplikacji

Teraz możesz uruchomić aplikację, aby zobaczyć ją w działaniu.

* **Debugowanie — > Uruchom bez debugowania**
* Aplikacja zostanie skompilowana i otwarta w przeglądarce internetowej
* Przejdź do `/Blogs`
* Kliknij przycisk **Utwórz nowy**
* Wprowadź **adres URL** nowego bloga i kliknij pozycję **Utwórz** .

  ![Tworzenie strony](_static/create.png)

  ![Strona indeksu](_static/index-existing-db.png)

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat tworzenia szkieletu klas kontekstu i jednostki, zobacz następujące artykuły:
* [Odwrócenie inżynierii](xref:core/managing-schemas/scaffolding)
* [Dokumentacja narzędzi Entity Framework Core Tools — interfejs wiersza polecenia platformy .NET](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [Dokumentacja narzędzi Entity Framework Core Tools — konsola Menedżera pakietów](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)
