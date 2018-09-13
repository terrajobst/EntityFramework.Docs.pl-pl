---
title: Najpierw kod do nowej bazy danych — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: 6572574ad36094ac0960c429cfa8606b6aebb492
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490754"
---
# <a name="code-first-to-a-new-database"></a>Najpierw kod do nowej bazy danych
W tym przewodniku krok po kroku i wideo zawierają wprowadzenie do rozwiązania deweloperskiego Code First dla nowej bazy danych. Ten scenariusz obejmuje przeznaczonych dla bazy danych, która nie istnieje i utworzy Code First lub pustą bazę danych tej Code First doda nowe tabele, aby. Kod umożliwia najpierw zdefiniować model za pomocą języka C\# lub klasy VB.Net. Opcjonalnie można wykonać dodatkowe czynności konfiguracyjne przy użyciu atrybutów klas i właściwości lub za pomocą interfejsu API fluent.

## <a name="watch-the-video"></a>Obejrzyj wideo
To wideo zawiera wprowadzenie do rozwiązania deweloperskiego Code First dla nowej bazy danych. Ten scenariusz obejmuje przeznaczonych dla bazy danych, która nie istnieje i utworzy Code First lub pustą bazę danych tej Code First doda nowe tabele, aby. Najpierw kod pozwala na zdefiniowanie modelu przy użyciu klas języka C# lub VB.Net. Opcjonalnie można wykonać dodatkowe czynności konfiguracyjne przy użyciu atrybutów klas i właściwości lub za pomocą interfejsu API fluent.

**Osoba prezentująca**: [Rowan Miller](http://romiller.com/)

**Film wideo**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Musisz mieć co najmniej programu Visual Studio 2010 lub Visual Studio 2012 są zainstalowane w tym przewodniku.

Jeśli używasz programu Visual Studio 2010, należy również mieć [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) zainstalowane.

## <a name="1-create-the-application"></a>1. Tworzenie aplikacji

Aby zachować ich prostotę zamierzamy utworzyć aplikację podstawowa konsola, która używa Code First do dostępu do danych.

-   Otwórz program Visual Studio
-   **Plik —&gt; New -&gt; projektu...**
-   Wybierz **Windows** menu po lewej stronie i **aplikacji konsoli**
-   Wprowadź **CodeFirstNewDatabaseSample** jako nazwę
-   Wybierz **OK**

## <a name="2-create-the-model"></a>2. Tworzenie modelu

Czynnością jest zdefiniowanie bardzo prosty model przy użyciu klas. Wystarczy zdefiniować je w pliku Program.cs, ale w rzeczywistej aplikacji, w Twoich zajęciach się będzie podzielić na osobne pliki i potencjalnie oddzielnego projektu.

Poniżej definicji klasy programu w pliku Program.cs Dodaj następujące dwie klasy.

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

Można zauważyć, że wprowadzamy dwie właściwości nawigacji (Blog.Posts i Post.Blog) wirtualnego. Dzięki temu funkcja powolne ładowanie programu Entity Framework. Powolne ładowanie oznacza, że zawartość te właściwości zostaną automatycznie załadowane z bazy danych przy próbie uzyskiwać do nich dostęp.

## <a name="3-create-a-context"></a>3. Utwórz kontekst

Teraz nadszedł czas, aby zdefiniować pochodnej kontekst, który reprezentuje sesję z bazą danych, dzięki czemu nam zapytania i Zapisz dane. Definiujemy kontekstu, który pochodzi z System.Data.Entity.DbContext i udostępnia wpisane DbSet&lt;TEntity&gt; dla każdej klasy w naszym modelu.

Zaczynamy teraz użyć typów z programu Entity Framework, dlatego musimy Dodaj pakiet NuGet platformy EntityFramework.

-   **Project —&gt; Zarządzaj pakietami NuGet...**
    Uwaga: Jeśli nie masz **Zarządzaj pakietami NuGet...** opcja, należy zainstalować [najnowszej wersji pakietu NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)
-   Wybierz **Online** kartę
-   Wybierz **EntityFramework** pakietu
-   Kliknij przycisk **instalacji**

Dodaj instrukcję using poufności informacji dotyczące System.Data.Entity w górnej części pliku Program.cs.

``` csharp
using System.Data.Entity;
```

Poniższe klasy wpisu w pliku Program.cs Dodaj następującym kontekstem pochodnych.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

Oto Pełna lista Program.cs powinien teraz zawartość.

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

To już cały kod, której potrzebujesz do rozpoczęcia przechowywania i pobierania danych. Oczywiście istnieje znacznej dzieje się w tle, a firma Microsoft będzie Przyjrzyj się, w momencie, ale pierwszym umożliwia sprawdzenie w działaniu.

## <a name="4-reading--writing-data"></a>4. Odczytywanie i zapisywanie danych

Implementuje metody Main w pliku Program.cs, jak pokazano poniżej. Ten kod tworzy nowe wystąpienie nasz kontekst i używa go do wstawiania nowego bloga. Następnie używa zapytania LINQ, aby pobrać wszystkie blogi z bazy danych uporządkowana w kolejności alfabetycznej według tytułu.

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
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a>Gdzie są moje dane?

Zgodnie z Konwencją DbContext utworzył bazę danych dla Ciebie.

-   Jeśli lokalne wystąpienie programu SQL Express jest dostępny (instalowany domyślnie w programie Visual Studio 2010) następnie Code First została utworzona baza danych w tym wystąpieniu
-   Jeśli program SQL Express nie jest dostępny, a następnie Code First będzie próbują i użyć [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (instalowany domyślnie w programie Visual Studio 2012)
-   Baza danych jest nazwana w pełni kwalifikowaną nazwę pochodnego kontekst, w tym przypadku, który jest **CodeFirstNewDatabaseSample.BloggingContext**

Są to po prostu domyślnych Konwencji istnieją różne sposoby, aby zmienić bazę danych, która używa Code First, więcej informacji znajduje się w **jak DbContext wykrywa Model i połączenie z bazą danych** tematu.
Możesz nawiązać połączenie tej bazy danych za pomocą Eksploratora serwera w programie Visual Studio

-   **Widok —&gt; Eksploratora serwera**
-   Kliknij prawym przyciskiem myszy **połączeń danych** i wybierz **Dodaj połączenie...**
-   Jeśli nie jest połączona z bazą danych za pomocą Eksploratora serwera przed, musisz wybrać programu Microsoft SQL Server jako źródło danych

    ![Wybierz źródło danych](~/ef6/media/selectdatasource.png)

-   Łączenie się z LocalDB lub SQL Express, w zależności od tego, który z nich został zainstalowany

Firma Microsoft jest teraz Zbadaj schematu utworzonego Code First.

![Schemat początkowy](~/ef6/media/schemainitial.png)

Które klasy do uwzględnienia w modelu, analizując właściwości DbSet, które zdefiniowaliśmy opracowaną w DbContext. Następnie używa domyślnego zestawu Code First konwencje do określenia nazwy tabel i kolumn, określanie typów danych, znajdowanie, kluczy podstawowych, itp. W dalszej części tego przewodnika, omówimy sposób można zastąpić tych konwencji.

## <a name="5-dealing-with-model-changes"></a>5. Obsługa zmiany modelu

Teraz nadszedł czas, aby wprowadzić pewne zmiany na nasz model, jeśli możemy wprowadzić te zmiany, należy również zaktualizować schemat bazy danych. W tym celu użyjemy do użycia funkcję migracje Code First ani migracji w skrócie.

Migracja pozwala mieć uporządkowany zestaw kroków, które opisują sposób uaktualnienia (i obniżenia poziomu) naszych schemat bazy danych. Każdy z tych kroków nazywana migracji, zawiera pewne kod, który opisano zmiany, które mają być stosowane. 

Pierwszym krokiem jest włączenie migracje Code First dla naszych BloggingContext.

-   **Narzędzia —&gt; Menedżer pakietów biblioteki -&gt; Konsola Menedżera pakietów**
-   Uruchom **Enable-Migrations** polecenia w konsoli Menedżera pakietów
-   Nowy folder migracji został dodany do naszych projektu, który zawiera dwa elementy:
    -   **Configuration.cs** — ten plik zawiera ustawienia, używających migracje migracji BloggingContext. Nie trzeba zmieniać niczego w ramach tego przewodnika, ale poniżej przedstawiono, w którym można określić przestrzeń nazw zmieniają się dane inicjatora, dostawcy rejestru dla innych baz danych, czy migracje są generowane w itp.
    -   **&lt;Sygnatura czasowa&gt;\_InitialCreate.cs** — jest to pierwszy migracji, reprezentuje zmiany, które zostały już zastosowane do bazy danych, zrób to przed pustej bazy danych na taki, który zawiera tabele blogów i wpisów . Mimo że umożliwialiśmy Code First automatycznie utworzyć te tabele, skoro mamy wybranych do migracji, które zostały przekonwertowane do migracji. Kod najpierw również zarejestrował w naszej bazie danych lokalnych, czy migracja została już zainstalowana. Sygnatury czasowej na nazwę pliku służy do ustalania kolejności celów.

    Teraz możemy wprowadzić zmiany w naszym modelu, Dodaj właściwość adres Url do klasy Blog:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   Uruchom **AddUrl migracji Dodaj** polecenia w konsoli Menedżera pakietów.
    Polecenie Dodaj migracji sprawdza zmiany od ostatniego migracji i scaffolds nowej migracji ze wszystkimi zmianami, które znajdują się. Firma Microsoft może nazwij migracje; w takim przypadku wywołania migracji "AddUrl".
    Utworzony szkielet kodu mówi, że musimy dodać kolumnę adres Url, który może pomieścić ciąg danych, do dbo. Blogi tabeli. Jeśli to konieczne, można będziemy edytować utworzony szkielet kodu, ale nie jest to wymagane w tym przypadku.

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

-   Uruchom **Update-Database** polecenia w konsoli Menedżera pakietów. To polecenie będzie dotyczyć wszelkie oczekujące migracji bazy danych. Nasze migracji InitialCreate została już zainstalowana, migracje będą po prostu zastosować naszej nowej migracji AddUrl.
    Porada: Możesz użyć **— pełne** Przełącz podczas wywoływania Update-Database, aby zobaczyć, SQL Server, który jest wykonywana w bazie danych.

Nowa kolumna adres Url jest dodany do blogów tabeli w bazie danych:

![Schemat adresu URL](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. Adnotacje danych

Do tej pory został właśnie umożliwialiśmy EF odnajdywanie modelu przy użyciu jego domyślnych Konwencji, ale będą występować sytuacje, gdy nie są zgodne z konwencjami naszych zajęć i musimy wykonać dalszą część konfigurowania. Dostępne są dwie opcje dla tego; omówimy adnotacje danych w tej sekcji, a następnie wygodnego interfejsu API w następnej sekcji.

-   Dodajmy klasy użytkownika w naszym modelu

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Należy również dodać zestaw do nasz kontekst pochodne

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   Jeśli firma Microsoft podjęto próbę dodania migracji otrzymamy wynik powiedzenie błąd "*obiektu EntityType"User"nie ma zdefiniowanego klucza. Klucz jest definiowany dla tego obiektu EntityType."* ponieważ EF nie ma możliwości, wiedząc, że nazwa użytkownika powinna mieć klucz podstawowy dla użytkownika.
-   Zamierzamy korzystanie z adnotacji danych teraz musimy dodać, przy użyciu instrukcji w górnej części pliku Program.cs

```
using System.ComponentModel.DataAnnotations;
```

-   Teraz dodawać adnotacje do właściwości nazwy użytkownika do identyfikowania, czy jest klucz podstawowy

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Użyj **AddUser migracji Dodaj** polecenia do tworzenia szkieletu migracji, aby zastosować te zmiany do bazy danych
-   Uruchom **Update-Database** polecenie, aby zastosować nową migrację do bazy danych

Teraz zostanie dodana nowa tabela, do bazy danych:

![Schemat z użytkownikami](~/ef6/media/schemawithusers.png)

Pełna lista adnotacje obsługiwane przez EF jest:

-   [KeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [StringLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [ConcurrencyCheckAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [TimestampAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [ComplexTypeAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [ColumnAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [TableAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [InversePropertyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [ForeignKeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [DatabaseGeneratedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [NotMappedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. Interfejs Fluent API

W poprzedniej sekcji przyjrzeliśmy się przy użyciu adnotacji danych do uzupełnienia lub zastąpić, co zostało wykryte przez Konwencję. Inny sposób, aby skonfigurować model jest za pośrednictwem interfejsu API fluent Code First.

Większość konfiguracji modelu można zrobić za pomocą prostych danych adnotacji. Interfejs fluent API jest bardziej zaawansowany sposób określania Konfiguracja modelu, który obejmuje wszystko, co dodatkowo zrobić adnotacje danych na niektórych bardziej zaawansowanych konfiguracji nie jest możliwe przy użyciu adnotacji danych. Adnotacje danych i wygodnego interfejsu API mogą być używane razem.

Dostęp do interfejsu API fluent zastąpienia metody OnModelCreating w DbContext. Załóżmy, że Chcieliśmy, aby zmienić nazwę kolumny, która User.DisplayName znajduje się w celu wyświetlenia\_nazwy.

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

-   Użyj **ChangeDisplayName migracji Dodaj** polecenia do tworzenia szkieletu migracji, aby zastosować te zmiany do bazy danych.
-   Uruchom **Update-Database** polecenie, aby zastosować nową migrację do bazy danych.

W kolumnie Nazwa wyświetlana została zmieniona na wyświetlanie\_nazwy:

![Schemat o nazwie wyświetlanej zmieniona](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>Podsumowanie

W tym przewodniku przyjrzeliśmy się rozwiązania deweloperskiego Code First przy użyciu nowej bazy danych. Firma Microsoft zdefiniowany model przy użyciu klasy, a następnie ten model umożliwia tworzenie bazy danych i przechowywać i pobierać dane. Gdy baza danych została utworzona użyliśmy migracje Code First można zmienić schematu jako nasz model ewoluował. Widzieliśmy również sposób konfigurowania model przy użyciu adnotacji danych i interfejsu API Fluent.
