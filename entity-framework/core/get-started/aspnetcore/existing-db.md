---
title: Wprowadzenie do platformy ASP.NET Core - istniejącej bazy danych — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: db2469d0badd428734425c1f568667f00bef2f4f
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
ms.locfileid: "30151017"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Wprowadzenie do podstawowych EF na platformy ASP.NET Core z istniejącej bazy danych

W tym przykładzie utworzysz aplikacji platformy ASP.NET Core MVC, który wykonuje dostęp do podstawowych danych przy użyciu programu Entity Framework. Odtwarzanie użyje do utworzenia modelu programu Entity Framework, na podstawie istniejącej bazy danych.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) w witrynie GitHub.

## <a name="prerequisites"></a>Wymagania wstępne

Do przeprowadzenia tego instruktażu potrzebne są następujące wymagania wstępne:

* [15 ustęp 3 programu Visual Studio 2017](https://www.visualstudio.com/downloads/) z te obciążenia:
  * **ASP.NET i sieć web development** (w obszarze **sieci Web & chmurze**)
  * **Programowanie wieloplatformowych .NET core** (w obszarze **inne procesami**)
* [.NET core 2.0 SDK](https://www.microsoft.com/net/download/core).
* [Blog bazy danych](#blogging-database)

### <a name="blogging-database"></a>Blog bazy danych

W tym samouczku używana **obsługi blogów** bazy danych w wystąpieniu bazy danych LocalDb jako istniejącej bazy danych.

> [!TIP]  
> Jeśli utworzono już **obsługi blogów** bazy danych jako część innego samouczek, można pominąć te kroki.

* Otwórz program Visual Studio
* **Narzędzia -> łączenia z bazą danych...**
* Wybierz **programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**
* Wprowadź **(localdb) \mssqllocaldb** jako **nazwa serwera**
* Wprowadź **wzorca** jako **Nazwa bazy danych** i kliknij przycisk **OK**
* Baza danych master jest teraz wyświetlany w obszarze **połączenia danych** w **Eksploratora serwera**
* Kliknij prawym przyciskiem myszy w bazie danych w **Eksploratora serwera** i wybierz **nowej kwerendy**
* Skopiuj skrypt wymienione poniżej do edytora zapytań
* Kliknij prawym przyciskiem myszy w edytorze zapytań i wybierz **Execute**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

* Otwórz 2017 Visual Studio
* **Plik -> Nowy -> Projekt...**
* Wybierz z menu po lewej stronie **zainstalowana -> Szablony -> Visual C# -> Sieć Web**
* Wybierz **aplikacji sieci Web platformy ASP.NET Core (.NET Core)** szablonu projektu
* Wprowadź **EFGetStarted.AspNetCore.ExistingDb** jako nazwy i kliknij przycisk **OK**
* Poczekaj, aż **nową aplikację sieci Web Core ASP.NET** wyświetlać okno dialogowe
* W obszarze **ASP.NET Core szablony 2.0** wybierz **aplikacji sieci Web (Model-View-Controller)**
* Upewnij się, że **uwierzytelniania** ustawiono **bez uwierzytelniania**
* Kliknij przycisk **OK**

## <a name="install-entity-framework"></a>Instalowanie programu Entity Framework

Aby użyć EF podstawowe, należy zainstalować pakiet dla powszechne bazy danych, który ma być docelowa. W tym przewodniku zastosowano programu SQL Server. Lista dostępnych dostawców [dostawcy bazy danych](../../providers/index.md).

* **Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów**

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Użyjemy niektóre Entity Framework narzędzia do tworzenia modeli z bazy danych. Dlatego zostanie zainstalowany pakiet narzędzi również:

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`

Użyjemy niektóre platformy ASP.NET Core szkieletów narzędzia do tworzenia widoków i kontrolerów później. Dlatego zostanie zainstalowany ten pakiet projektu:

* Uruchom `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

## <a name="reverse-engineer-your-model"></a>Odtworzyć modelu

Teraz nadszedł czas, aby utworzyć model EF na podstawie istniejącej bazy danych.

* **Pakiet NuGet –> Narzędzia Menedżera –> Konsola Menedżera pakietów**
* Uruchom następujące polecenie, aby utworzyć model z istniejącej bazy danych:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Jeśli zostanie wyświetlony błąd informujący o `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, a następnie zamknij i otwórz ponownie program Visual Studio.

> [!TIP]  
> Można określić tabel, które mają zostać wygenerowane przez dodanie jednostek dla `-Tables` argument polecenia powyżej. Np. `-Tables Blog,Post`.

Proces odtwarzania tworzony klas jednostek (`Blog.cs` & `Post.cs`) i kontekst pochodnych (`BloggingContext.cs`) na podstawie schematu istniejącej bazy danych.

 Klas jednostek to proste C# obiekty reprezentujące dane można będzie kwerend i zapisywanie.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 Kontekst reprezentuje sesji z bazą danych i służy do wykonywania zapytań i zapisać wystąpień klas jednostek.

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

## <a name="register-your-context-with-dependency-injection"></a>Zarejestruj kontekst iniekcji zależności

Pojęcie iniekcji zależności jest podstawą do platformy ASP.NET Core. Usługi (takie jak `BloggingContext`) są zarejestrowane w usłudze iniekcji zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (takich jak kontrolerów MVC) są następnie udostępniane tych usług za pomocą konstruktora parametry lub właściwości. Aby uzyskać więcej informacji na temat iniekcji zależności zobacz [iniekcji zależności](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) artykuł w witrynie platformy ASP.NET.

### <a name="remove-inline-context-configuration"></a>Usuń konfigurację kontekstu wbudowany

W ASP.NET Core konfiguracji jest zazwyczaj wykonywane w **Startup.cs**. Z tego wzorca, możemy przenieść konfiguracji dostawcy bazy danych do **Startup.cs**.

* Otwórz `Models\BloggingContext.cs`
* Usuń `OnConfiguring(...)` — metoda

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* Dodaj następujący Konstruktor, co pozwoli konfigurację do przekazania do kontekstu przez iniekcji zależności

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a>Zarejestruj i skonfiguruj kontekst w pliku Startup.cs

Aby naszych kontrolerów MVC korzystać z `BloggingContext` zamierzamy, aby zarejestrować go jako usługa.

* Otwórz **Startup.cs**
* Dodaj następujące `using` instrukcje na początku pliku

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Teraz możemy użyć `AddDbContext(...)` metody, aby zarejestrować go jako usługa.
* Zlokalizuj `ConfigureServices(...)` — metoda
* Dodaj następujący kod, aby zarejestrować kontekstu w trybie usługi

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> W rzeczywistej aplikacji zwykle przełączyć parametry połączenia w pliku konfiguracji. Dla uproszczenia możemy są definiowane w kodzie. Aby uzyskać więcej informacji, zobacz [parametry połączenia](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller"></a>Tworzenie kontrolera

Następnie firma Microsoft będzie aktywują szkielet w naszym projektu.

* Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **kontrolera -> Dodaj...**
* Wybierz **pełne zależności** i kliknij przycisk **Dodaj**
* Możesz zignorować instrukcje `ScaffoldingReadMe.txt` pliku, który zostanie otwarty

Teraz, gdy jest włączona funkcja szkieletów, możemy utworzyć szkielet kontrolera dla `Blog` jednostki.

* Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **kontrolera -> Dodaj...**
* Wybierz **kontroler MVC z widokami używający narzędzia Entity Framework** i kliknij przycisk **Ok**
* Ustaw **klasa modelu** do **Blog** i **klasa kontekstu danych** do **BloggingContext**
* Kliknij przycisk **Dodaj**

## <a name="run-the-application"></a>Uruchamianie aplikacji

Można teraz uruchomić aplikację, aby zobaczyć ją w akcji.

* **Debugowanie -> Start bez debugowania**
* Aplikacja kompilacji i Otwórz w przeglądarce sieci web
* Przejdź do `/Blogs`
* Kliknij przycisk **Utwórz nową**
* Wprowadź **adres Url** nowy blog i kliknij **Utwórz**

![obraz](_static/create.png)

![obraz](_static/index-existing-db.png)
