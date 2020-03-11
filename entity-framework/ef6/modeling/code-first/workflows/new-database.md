---
title: Code First do nowej bazy danych — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: d540fc6e84049f345ae22998f94c309e0be73fc3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418813"
---
# <a name="code-first-to-a-new-database"></a>Code First do nowej bazy danych
Ten film wideo i przewodnik krok po kroku zawierają wprowadzenie do Code First tworzenia elementów docelowych dla nowej bazy danych. Ten scenariusz obejmuje bazę danych, która nie istnieje i Code First utworzy, lub pustą bazę danych, która Code First doda nowe tabele do programu. Code First pozwala definiować model przy użyciu klas C\# lub VB.Net. Można opcjonalnie wykonać dodatkową konfigurację przy użyciu atrybutów klas i właściwości albo za pomocą interfejsu API Fluent.

## <a name="watch-the-video"></a>Obejrzyj film
Ten film wideo zawiera wprowadzenie do Code First tworzenia elementów docelowych dla nowej bazy danych. Ten scenariusz obejmuje bazę danych, która nie istnieje i Code First utworzy, lub pustą bazę danych, która Code First doda nowe tabele do programu. Code First pozwala definiować model przy użyciu C# klas lub VB.NET. Można opcjonalnie wykonać dodatkową konfigurację przy użyciu atrybutów klas i właściwości albo za pomocą interfejsu API Fluent.

**Przedstawione przez**: [Rowan Miller](https://romiller.com/)

**Wideo**: [wmv](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (zip)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik, musisz mieć zainstalowany co najmniej program Visual Studio 2010 lub Visual Studio 2012.

W przypadku korzystania z programu Visual Studio 2010 należy również zainstalować pakiet [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .

## <a name="1-create-the-application"></a>1. Utwórz aplikację

Aby zachować prostotę, możemy utworzyć podstawową aplikację konsolową, która używa Code First do uzyskiwania dostępu do danych.

-   Otwórz program Visual Studio.
-   **Plik —&gt; nowy&gt; projekt...**
-   Wybierz pozycję **Windows** z menu po lewej stronie i **aplikacji konsolowej**
-   Wprowadź **CodeFirstNewDatabaseSample** jako nazwę
-   Kliknij przycisk **OK**

## <a name="2-create-the-model"></a>2. Utwórz model

Zdefiniujmy bardzo prosty model przy użyciu klas. Definiujemy je w pliku Program.cs, ale w prawdziwej aplikacji można podzielić klasy na osobne pliki i potencjalnie oddzielny projekt.

Poniżej definicji klasy programu w Program.cs Dodaj następujące dwie klasy.

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

Zauważymy, że są one wirtualne z zastosowaniem dwóch właściwości nawigacji (blog. post i post. blog). Dzięki temu funkcja ładowania z opóźnieniem Entity Framework. Ładowanie z opóźnieniem oznacza, że zawartość tych właściwości zostanie automatycznie załadowana z bazy danych przy próbie uzyskania dostępu do nich.

## <a name="3-create-a-context"></a>3. Tworzenie kontekstu

Teraz można zdefiniować kontekst pochodny, który reprezentuje sesję z bazą danych, umożliwiając nam wykonywanie zapytań i zapisywanie danych. Definiujemy kontekst, który pochodzi od elementu System. Data. Entity. DbContext i uwidacznia typ Nieogólnymi&lt;&gt; dla każdej klasy w naszym modelu.

Teraz zaczynamy używać typów z Entity Framework, więc musimy dodać pakiet NuGet EntityFramework.

-   **Projekt —&gt; zarządzać pakietami NuGet...**
    Uwaga: Jeśli nie masz **zarządzania pakietami NuGet...** Opcja należy zainstalować [najnowszą wersję programu NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)
-   Wybierz kartę **online**
-   Wybierz pakiet **EntityFramework**
-   Kliknij przycisk **Instaluj**

Dodaj instrukcję using dla elementu System. Data. Entity w górnej części Program.cs.

``` csharp
using System.Data.Entity;
```

Pod klasą post w Program.cs Dodaj następujący kontekst pochodny.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

Poniżej znajduje się kompletna lista elementów, które powinny być zawarte w Program.cs.

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

To wszystko, co jest potrzebne do rozpoczęcia przechowywania i pobierania danych. Oczywiście w tle jest dość dużo miejsca, a w tej chwili zajmiemy się tym, ale po raz pierwszy zobaczymy ją w działaniu.

## <a name="4-reading--writing-data"></a>4. odczytywanie & zapisywania danych

Zaimplementuj metodę Main w Program.cs, jak pokazano poniżej. Ten kod tworzy nowe wystąpienie naszego kontekstu, a następnie używa go do wstawienia nowego bloga. Następnie używa zapytania LINQ do pobrania wszystkich blogów z bazy danych uporządkowane alfabetycznie według tytułu.

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
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a>Gdzie są moje dane?

Według Konwencji DbContext utworzyła bazę danych.

-   Jeśli lokalne wystąpienie programu SQL Express jest dostępne (domyślnie instalowane z programem Visual Studio 2010), Code First utworzyć bazę danych w tym wystąpieniu
-   Jeśli program SQL Express jest niedostępny, Code First będzie próbować korzystać z [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (instalowany domyślnie z programem Visual Studio 2012).
-   Baza danych jest nazywana po w pełni kwalifikowaną nazwą kontekstu pochodnego, w naszym przypadku jest to **CodeFirstNewDatabaseSample. BloggingContext**

Są to tylko konwencje domyślne i istnieją różne sposoby zmiany bazy danych używanej przez Code First. więcej informacji można znaleźć w temacie **jak DbContext odnajduje model i połączenie z bazą danych** .
Możesz połączyć się z tą bazą danych przy użyciu Eksplorator serwera w programie Visual Studio

-   **Widok-&gt; Eksplorator serwera**
-   Kliknij prawym przyciskiem myszy pozycję **połączenia danych** i wybierz polecenie **Dodaj połączenie...**
-   Jeśli nie masz połączenia z bazą danych Eksplorator serwera przed wybraniem Microsoft SQL Server jako źródła danych

    ![Wybierz źródło danych](~/ef6/media/selectdatasource.png)

-   Nawiąż połączenie z usługą LocalDB lub SQL Express, w zależności od tego, który z nich jest zainstalowany

Teraz można sprawdzić schemat, który Code First utworzony.

![Schemat początkowy](~/ef6/media/schemainitial.png)

Program DbContext pracował, jakie klasy należy uwzględnić w modelu, przeglądając zdefiniowane przez nas właściwości Nieogólnymi. Następnie używa domyślnego zestawu Konwencji Code First, aby określić nazwy tabel i kolumn, określić typy danych, znaleźć klucze podstawowe itd. W dalszej części tego instruktażu zawarto informacje na temat sposobu przesłania tych konwencji.

## <a name="5-dealing-with-model-changes"></a>5. postępowanie z zmianami modelu

Teraz można wprowadzić pewne zmiany w modelu, gdy wprowadzimy te zmiany, należy również zaktualizować schemat bazy danych. W tym celu użyjemy funkcji o nazwie Migracje Code First lub migracji na krótko.

Migracja pozwala nam na posiadanie uporządkowanego zestawu kroków opisujących sposób uaktualniania (i obniżania poziomu) naszego schematu bazy danych. Każda z tych kroków, znana jako migracja, zawiera kod opisujący zmiany, które mają zostać zastosowane. 

Pierwszym krokiem jest włączenie Migracje Code First dla naszych BloggingContext.

-   **Narzędzia — Menedżer pakietów&gt; Library —&gt; konsoli Menedżera pakietów**
-   Uruchamianie polecenia **enable-migrations** w konsoli Menedżera pakietów
-   Do naszego projektu dodano nowy folder migracji zawierający dwa elementy:
    -   **Configuration.cs** — ten plik zawiera ustawienia, które będą używane przez migracje na potrzeby migrowania BloggingContext. Nie musimy niczego zmienić w tym instruktażu, ale w tym miejscu możesz określić dane dotyczące inicjatora, zarejestrować dostawców dla innych baz danych, zmienić przestrzeń nazw, w której są generowane migracje itp.
    -   **&lt;sygnatura czasowa&gt;\_InitialCreate.cs** — jest to Twoja pierwsza migracja. reprezentuje ona zmiany, które zostały już zastosowane do bazy danych w celu przejęcia ich przez pustą bazę danych, która zawiera tabele blogów i wpisów. Mimo że Code First automatycznie utworzyć te tabele dla nas, teraz, gdy przejdziemy do migracji, zostały one przekonwertowane do migracji. Code First został również zarejestrowany w lokalnej bazie danych, że ta migracja została już zastosowana. Sygnatura czasowa w nazwie pliku jest używana do określania kolejności.

    Teraz Wprowadźmy zmiany w naszym modelu, dodając Właściwość adresu URL do klasy bloga:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   Uruchom polecenie **Add-Migration AddUrl** w konsoli Menedżera pakietów.
    Polecenie Add-Migration sprawdza zmiany od momentu ostatniej migracji i tworzy szkielet nowej migracji wraz ze wszystkimi znalezionymi zmianami. Możemy nadać nazwę migracji; w tym przypadku wywołujemy migrację "AddUrl".
    Kod szkieletu oznacza, że musimy dodać kolumnę adresu URL, która może przechowywać dane ciągu do właściciela bazy danych. Tabela blogów. W razie potrzeby możemy edytować kod szkieletowy, ale nie jest to wymagane w tym przypadku.

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   Uruchom polecenie **Update-Database** w konsoli Menedżera pakietów. To polecenie spowoduje zastosowanie wszelkich oczekujących migracji do bazy danych programu. Nasza migracja InitialCreate została już zastosowana, dlatego migracje będą dotyczyły właśnie nowej migracji AddUrl.
    Porada: w przypadku wywołania metody Update-Database można użyć przełącznika **– verbose** , aby zobaczyć, które SQL jest wykonywane względem bazy danych.

Nowa kolumna adresu URL zostanie dodana do tabeli blogów w bazie danych:

![Schemat z adresem URL](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. adnotacje dotyczące danych

Z tego powodu tylko firma Dr odnajduje model przy użyciu jego Konwencji domyślnych, ale istnieje dużo czasu, gdy nasze klasy nie przestrzegają Konwencji i będziemy mogli przeprowadzić dalszą konfigurację. Dostępne są dwie opcje: przejrzyjemy adnotacje dotyczące danych w tej sekcji, a następnie interfejs API Fluent w następnej sekcji.

-   Dodajmy klasę użytkownika do naszego modelu

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Musimy również dodać zestaw do naszego kontekstu pochodnego

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   Jeśli podjęto próbę dodania migracji, wystąpił błąd informujący,*że nie zdefiniowano klucza "EntityType" User ". Zdefiniuj klucz dla tego elementu EntityType. "* ponieważ EF nie ma możliwości znajomości, że nazwa użytkownika powinna być kluczem podstawowym dla użytkownika.
-   Teraz będziemy używać adnotacji danych, więc musimy dodać instrukcję using w górnej części Program.cs

```csharp
using System.ComponentModel.DataAnnotations;
```

-   Teraz Dodaj adnotację do właściwości username, aby określić, że jest kluczem podstawowym

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Użycie polecenia **Add-Migration adduser** do tworzenia szkieletu migracji w celu zastosowania tych zmian do bazy danych
-   Uruchom polecenie **Update-Database** , aby zastosować nową migrację do bazy danych

Nowa tabela jest teraz dodawana do bazy danych:

![Schemat z użytkownikami](~/ef6/media/schemawithusers.png)

Pełna lista adnotacji obsługiwanych przez EF to:

-   [Atrybut atrybutu](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [StringLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthattribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [ConcurrencyCheckAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [TimestampAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [ComplexTypeattribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [ColumnAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [Tabelaattribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [InversePropertyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [ForeignKeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [DatabaseGeneratedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [NotMappedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. interfejs API Fluent

W poprzedniej sekcji oglądamy używanie adnotacji danych do uzupełniania lub zastępowania elementów wykrytych w ramach Konwencji. Innym sposobem skonfigurowania modelu jest za pośrednictwem interfejsu API usługi Code First Fluent.

Większość konfiguracji modelu można wykonać przy użyciu prostych adnotacji danych. Interfejs API Fluent to bardziej zaawansowany sposób określania konfiguracji modelu, która obejmuje wszystkie elementy, które mogą wykonywać adnotacje danych, a także bardziej zaawansowaną konfigurację, która nie jest możliwa w przypadku adnotacji danych. Adnotacje danych i interfejs API Fluent mogą być używane razem.

Aby uzyskać dostęp do interfejsu API Fluent, Zastąp metodę OnModelCreating w kontekście DbContext. Załóżmy, że chcemy zmienić nazwę kolumny, która jest przechowywana przez User. DisplayName w celu wyświetlenia nazwy\_.

-   Zastąp metodę OnModelCreating na BloggingContext następującym kodem

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   Użyj polecenia **Add-Migration ChangeDisplayName** , aby przeprowadzić szkielet migracji w celu zastosowania tych zmian w bazie danych.
-   Uruchom polecenie **Update-Database** , aby zastosować nową migrację do bazy danych programu.

Nazwa kolumny DisplayName została zmieniona na nazwę wyświetlaną\_:

![Zmieniono nazwę schematu z nazwą wyświetlaną](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>Podsumowanie

W tym instruktażu oglądamy Code First opracowywanie przy użyciu nowej bazy danych. Definiujemy model przy użyciu klas, a następnie używa tego modelu do tworzenia bazy danych i przechowywania i pobierania danych. Po utworzeniu bazy danych używamy Migracje Code First, aby zmienić schemat w miarę rozwoju modelu. Przedstawiono również sposób konfigurowania modelu przy użyciu adnotacji danych i interfejsu API Fluent.
