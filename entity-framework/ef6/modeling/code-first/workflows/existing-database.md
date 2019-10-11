---
title: Code First do istniejącej bazy danych — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 61980bbd1f236f496a9d4fd92aa52264f1454615
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182621"
---
# <a name="code-first-to-an-existing-database"></a>Code First do istniejącej bazy danych
Ten film wideo i przewodnik krok po kroku zawierają wprowadzenie do Code First projektowania dla istniejącej bazy danych. Code First umożliwia zdefiniowanie modelu przy użyciu klas C @ no__t-0 lub VB.Net. Opcjonalnie można wykonać dodatkową konfigurację przy użyciu atrybutów klas i właściwości lub przy użyciu interfejsu API Fluent.

## <a name="watch-the-video"></a>Obejrzyj wideo
Ten film wideo jest [teraz dostępny w witrynie Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).

## <a name="pre-requisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik, musisz mieć zainstalowany **program Visual Studio 2012** lub **Visual Studio 2013** .

Wymagana jest również wersja **6,1** (lub nowsza) **Entity Framework Tools programu Visual Studio** . Aby uzyskać informacje na temat instalowania najnowszej wersji Entity Framework Tools, zobacz [pobierz Entity Framework](~/ef6/fundamentals/install.md) .

## <a name="1-create-an-existing-database"></a>1. Tworzenie istniejącej bazy danych

Zwykle w przypadku określania docelowej istniejącej bazy danych zostanie ona już utworzona, ale w tym instruktażu musimy utworzyć bazę danych do uzyskania dostępu.

Przyjrzyjmy się i wygenerujemy bazę danych.

-   Otwórz program Visual Studio
-   **Widok-&gt; Eksplorator serwera**
-   Kliknij prawym przyciskiem myszy pozycję **połączenia danych-&gt; Dodaj połączenie...**
-   Jeśli nie masz połączenia z bazą danych **Eksplorator serwera** przed wybraniem **Microsoft SQL Server** jako źródła danych

    ![Wybieranie źródła danych](~/ef6/media/selectdatasource.png)

-   Nawiąż połączenie z wystąpieniem programu LocalDB i wprowadź nazwę bazy danych na potrzeby prowadzenia **blogów**

    ![Połączenie LocalDB](~/ef6/media/localdbconnection.png)

-   Wybierz pozycję **OK** . pojawi się monit z prośbą o utworzenie nowej bazy danych, a następnie wybierz pozycję **tak** .

    ![Okno dialogowe Tworzenie bazy danych](~/ef6/media/createdatabasedialog.png)

-   Nowa baza danych będzie teraz wyświetlana w Eksplorator serwera, kliknij ją prawym przyciskiem myszy, a następnie wybierz pozycję **nowe zapytanie** .
-   Skopiuj następujące SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Execute (wykonaj** ).

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

Aby zachować prostotę, możemy utworzyć podstawową aplikację konsolową, która używa Code First do uzyskiwania dostępu do danych:

-   Otwórz program Visual Studio
-   **Plik-&gt; nowy-&gt; projektu...**
-   Wybierz pozycję **Windows** z menu po lewej stronie i **aplikacji konsolowej**
-   Wprowadź **CodeFirstExistingDatabaseSample** jako nazwę
-   Wybierz **przycisk OK**

 

## <a name="3-reverse-engineer-model"></a>3. Odtwórz Model

Zamierzamy skorzystać z Entity Framework Tools dla programu Visual Studio, aby pomóc nam wygenerować kod początkowy do mapowania na bazę danych. Narzędzia te generują tylko kod, który można również wpisać ręcznie, jeśli wolisz.

-   **Projekt-&gt; Dodaj nowy element...**
-   Wybierz pozycję **dane** z menu po lewej stronie, a następnie **ADO.NET Entity Data Model**
-   Wprowadź **BloggingContext** jako nazwę, a następnie kliknij przycisk **OK** .
-   Spowoduje to uruchomienie **kreatora Entity Data Model**
-   Wybierz **Code First z bazy danych** i kliknij przycisk **dalej** .

    ![Jeden CFE Kreatora](~/ef6/media/wizardonecfe.png)

-   Wybierz połączenie z bazą danych utworzoną w pierwszej sekcji i kliknij przycisk **dalej** .

    ![Kreator dwóch CFE](~/ef6/media/wizardtwocfe.png)

-   Kliknij pole wyboru obok pozycji **tabele** , aby zaimportować wszystkie tabele, a następnie kliknij przycisk **Zakończ** .

    ![Kreator trzech CFE](~/ef6/media/wizardthreecfe.png)

Po zakończeniu procesu odtwarzania liczba elementów zostanie dodana do projektu, przyjrzyjmy się dodaniu.

### <a name="configuration-file"></a>Plik konfiguracji

Plik App. config został dodany do projektu, ten plik zawiera parametry połączenia do istniejącej bazy danych.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*You'll Zauważ, że niektóre inne ustawienia w pliku konfiguracji są również domyślne ustawienia EF, które informują Code First o tworzeniu baz danych. Ponieważ mapowanie do istniejącej bazy danych, to ustawienie zostanie zignorowane w naszej aplikacji.*

### <a name="derived-context"></a>Kontekst pochodny

Do projektu Dodano klasę **BloggingContext** . Kontekst reprezentuje sesję z bazą danych, umożliwiając nam wykonywanie zapytań i zapisywanie danych.
Kontekst ujawnia **nieogólnymi @ no__t-1TEntity @ no__t-2** dla każdego typu w naszym modelu. Zauważ również, że Konstruktor domyślny wywołuje konstruktora podstawowego przy użyciu składni **name =** . Informuje to Code First, że parametry połączenia do użycia dla tego kontekstu powinny zostać załadowane z pliku konfiguracji.

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

*You powinna zawsze używać składni **name =** w przypadku używania parametrów połączenia w pliku konfiguracji. Gwarantuje to, że jeśli parametry połączenia nie są obecne, Entity Framework będzie zgłaszany zamiast tworzenia nowej bazy danych według Konwencji.*

### <a name="model-classes"></a>Klasy modelu

Na koniec do projektu dodano także **blog** i Klasa **post** . Są to klasy domeny, które składają się na nasz model. Zobaczysz adnotacje danych zastosowane do klas, aby określić konfigurację, w której konwencje Code First nie były wyrównane z strukturą istniejącej bazy danych. Na przykład zobaczysz adnotację **StringLength** w **blog.Name** i **blogu. URL** , ponieważ ma ona maksymalną długość **200** w bazie danych (Code First domyślnym jest użycie długości Maximun obsługiwanej przez dostawcę bazy danych — **nvarchar (max)** w SQL Server).

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

## <a name="4-reading--writing-data"></a>4. Odczytywanie & zapisywania danych

Teraz, gdy mamy już model, który jest używany do uzyskiwania dostępu do niektórych danych. Zaimplementuj metodę **Main** w **program.cs** , jak pokazano poniżej. Ten kod tworzy nowe wystąpienie naszego kontekstu, a następnie używa go do wstawienia nowego **bloga**. Następnie używa zapytania LINQ do pobrania wszystkich **blogów** z bazy danych uporządkowane alfabetycznie według **tytułu**.

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

Teraz możesz uruchomić aplikację i przetestować ją.

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>Co zrobić, jeśli moja baza danych ulegnie zmianie?

Kreator Code First do bazy danych służy do generowania zestawu punktów początkowych klas, które można następnie dostosować i zmodyfikować. Jeśli schemat bazy danych ulegnie zmianie, można ręcznie edytować klasy lub wykonać inny inżynier odtwarzania w celu zastąpienia klas.

## <a name="using-code-first-migrations-to-an-existing-database"></a>Używanie Migracje Code First do istniejącej bazy danych

Jeśli chcesz użyć Migracje Code First z istniejącą bazą danych, zobacz [migracje Code First do istniejącej bazy danych](~/ef6/modeling/code-first/migrations/existing-database.md).

## <a name="summary"></a>Podsumowanie

W tym instruktażu oglądamy Code First opracowywanie przy użyciu istniejącej bazy danych. Użyto Entity Framework Tools dla programu Visual Studio w celu odtworzenia zestawu klas, które są zamapowane na bazę danych i może służyć do przechowywania i pobierania danych.
