---
title: Najpierw kod istniejącej bazy danych — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
caps.latest.revision: 3
ms.openlocfilehash: 47581a7ae9ff534d26ce82365bcbe2b254247495
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912725"
---
# <a name="code-first-to-an-existing-database"></a>Najpierw kod istniejącej bazy danych
W tym przewodniku krok po kroku i wideo zawierają wprowadzenie do rozwiązania deweloperskiego Code First przeznaczonych dla istniejącej bazy danych. Kod umożliwia najpierw zdefiniować model za pomocą języka C\# lub klasy VB.Net. Opcjonalnie dodatkowej konfiguracji można wykonać przy użyciu atrybutów klas i właściwości lub za pomocą interfejsu API fluent.

## <a name="watch-the-video"></a>Obejrzyj wideo
To wideo jest [teraz dostępna w witrynie Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).

## <a name="pre-requisites"></a>Wymagania wstępne

Musisz mieć **programu Visual Studio 2012** lub **programu Visual Studio 2013** zainstalowany w celu przeprowadzenia tego instruktażu.

Należy również wersja **6.1** (lub nowszym) z **Entity Framework Tools for Visual Studio** zainstalowane. Zobacz [uzyskać Entity Framework](~/ef6/fundamentals/install.md) informacji na temat instalowania najnowszej wersji narzędzi Entity Framework Tools.

## <a name="1-create-an-existing-database"></a>1. Utwórz istniejącej bazy danych

Zazwyczaj przeznaczonych do istniejącej bazy danych, zostanie już utworzony, ale w tym przewodniku należy utworzyć bazy danych w celu uzyskania dostępu.

Rozpocznijmy i wygenerować bazę danych.

-   Otwórz program Visual Studio
-   **Widok —&gt; Eksploratora serwera**
-   Kliknij prawym przyciskiem myszy **połączeń danych -&gt; Dodaj połączenie...**
-   Jeśli nie jest połączona z bazą danych z **Eksploratora serwera** zanim będzie konieczne wybranie **programu Microsoft SQL Server** jako źródło danych

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   Połącz się z wystąpieniem usługi LocalDB, a następnie wprowadź **do obsługi blogów** jako nazwa bazy danych

    ![LocalDBConnection](~/ef6/media/localdbconnection.png)

-   Wybierz **OK** i uzyskasz, jeśli chcesz utworzyć nową bazę danych, wybierz opcję **tak**

    ![CreateDatabaseDialog](~/ef6/media/createdatabasedialog.png)

-   Nowe bazy danych będą teraz wyświetlane w Eksploratorze serwera, kliknij prawym przyciskiem myszy na nim i wybierz **nowe zapytanie**
-   Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonania**

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a>2. Tworzenie aplikacji

Aby zachować ich prostotę zamierzamy utworzyć aplikację podstawowa konsola, która używa Code First do dostępu do danych:

-   Otwórz program Visual Studio
-   **Plik —&gt; New -&gt; projektu...**
-   Wybierz **Windows** menu po lewej stronie i **aplikacji konsoli**
-   Wprowadź **CodeFirstExistingDatabaseSample** jako nazwę
-   Wybierz **OK**

 

## <a name="3-reverse-engineer-model"></a>3. Model odtwarzania

Będziemy korzystać z narzędzi Entity Framework Tools for Visual Studio ułatwia nam dotarcie kodu początkowego do mapowania na bazie danych. Te narzędzia po prostu generują kod, który można także wpisać ręcznie, jeśli użytkownik sobie tego życzy.

-   **Projekt —&gt; Dodaj nowy element...**
-   Wybierz **danych** menu po lewej stronie i następnie **ADO.NET Entity Data Model**
-   Wprowadź **BloggingContext** jako nazwę i kliknij przycisk **OK**
-   Spowoduje to uruchomienie **Kreator modelu Entity Data Model**
-   Wybierz **Code First z bazy danych** i kliknij przycisk **dalej**

    ![WizardOneCFE](~/ef6/media/wizardonecfe.png)

-   Wybierz połączenie do bazy danych utworzonej w pierwszej sekcji, a następnie kliknij przycisk **dalej**

    ![WizardTwoCFE](~/ef6/media/wizardtwocfe.png)

-   Kliknij pole wyboru obok pozycji **tabel** importowania wszystkich tabel, a następnie kliknij przycisk **Zakończ**

    ![WizardThreeCFE](~/ef6/media/wizardthreecfe.png)

Po procesu odtwarzania wykonaniu określonej liczby elementów będzie zostały dodane do projektu, możemy przyjrzeć się elementy dodane.

### <a name="configuration-file"></a>Plik konfiguracji

Plik App.config został dodany do projektu, ten plik zawiera parametry połączenia z istniejącą bazą danych.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*Można zauważyć za niektóre inne ustawienia w pliku konfiguracji, są ustawienia domyślne programu EF, które informują Code First, gdzie można utworzyć bazy danych. Ponieważ firma Microsoft jest mapowany do istniejącej bazy danych, te ustawienia zostaną zignorowane w naszej aplikacji.*

### <a name="derived-context"></a>Kontekst pochodne

A **BloggingContext** klasy został dodany do projektu. Kontekst reprezentuje sesję z bazą danych, dzięki czemu nam zapytania i Zapisz dane.
Udostępnia kontekst **DbSet&lt;TEntity&gt;**  dla każdego typu w naszym modelu. Można także zauważyć, że domyślny konstruktor wywołuje konstruktora podstawowego przy użyciu **nazwa =** składni. Oznacza to Code First, że parametry połączenia do użycia dla tego kontekstu powinny być załadowane z pliku konfiguracji.

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

*Należy zawsze używać **nazwa =** składni, w przypadku korzystania z parametrów połączenia w pliku konfiguracji. Daje to gwarancję, że jeśli nie ma parametrów połączenia następnie Entity Framework zgłosi zamiast tworzenia nowej bazy danych przez Konwencję.*

### <a name="model-classes"></a>Klasy modelu

Na koniec **Blog** i **wpis** klasy również zostały dodane do projektu. Są to klasy domeny, które tworzą nasz model. Zobaczysz adnotacje danych stosowany do klas do określenia konfiguracji, której konwencje Code First nie będzie wyrównane z struktury istniejącej bazy danych. Na przykład, zobaczysz **StringLength** adnotacja **Blog.Name** i **Blog.Url** ponieważ mają maksymalną długość **200** w bazy danych (Code First domyślna ma używać długość maksymalna obsługiwana przez dostawcę bazy danych — **nvarchar(max)** w programie SQL Server).

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a>4. Odczytywanie i zapisywanie danych

Teraz, gdy model, nadszedł czas na potrzeby dostępu do niektórych danych. Implementowanie **Main** method in Class metoda **Program.cs** jak pokazano poniżej. Ten kod tworzy nowe wystąpienie klasy nasz kontekst, a następnie używa go, aby wstawić nowy **blogu**. Następnie wykorzystuje zapytania LINQ można pobrać wszystkich **blogi** z bazy danych uporządkowana w kolejności alfabetycznej według **tytuł**.

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

Możesz teraz uruchomić aplikację i ją przetestować.

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>Co zrobić, jeśli Moja baza danych zmiany?

Code First Kreatora bazy danych jest przeznaczona do generowania początkowego zestaw punktu klas, które można dostosować i modyfikować. Zmiany schematu bazy danych można ręcznie edytować klasy lub wykonania innego odtwarzania, aby zastąpić klasy.

## <a name="using-code-first-migrations-to-an-existing-database"></a>Za pomocą migracje Code First istniejącą bazę danych

Jeśli chcesz użyć migracje Code First z istniejącej bazy danych, zobacz [migracje Code First istniejącą bazę danych](~/ef6/modeling/code-first/migrations/existing-database.md).

## <a name="summary"></a>Podsumowanie

W tym przewodniku przyjrzeliśmy się rozwiązania deweloperskiego Code First przy użyciu istniejącej bazy danych. Użyliśmy narzędzi Entity Framework Tools dla programu Visual Studio do odtworzenia zestaw klas, które są mapowane do bazy danych i może służyć do przechowywania i pobierania danych.
