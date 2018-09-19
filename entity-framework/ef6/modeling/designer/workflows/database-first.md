---
title: Najpierw — bazy danych platformy EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: c81025fe7c3ad6398f003f7be2a3f9f072eec327
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284086"
---
# <a name="database-first"></a>Najpierw bazy danych
W tym przewodniku krok po kroku i wideo zawierają wprowadzenie do tworzenia pierwszej bazy danych przy użyciu platformy Entity Framework. Baza danych najpierw można odtwarzać modelu z istniejącej bazy danych. Model jest przechowywany w pliku EDMX (z rozszerzeniem edmx) i można wyświetlać i edytować w Projektancie Entity Framework. Klasy, które możesz korzystać z aplikacji są generowane automatycznie z pliku EDMX.

## <a name="watch-the-video"></a>Obejrzyj wideo
To wideo zawiera wprowadzenie do tworzenia pierwszej bazy danych przy użyciu platformy Entity Framework. Baza danych najpierw można odtwarzać modelu z istniejącej bazy danych. Model jest przechowywany w pliku EDMX (z rozszerzeniem edmx) i można wyświetlać i edytować w Projektancie Entity Framework. Klasy, które możesz korzystać z aplikacji są generowane automatycznie z pliku EDMX.

**Osoba prezentująca**: [Rowan Miller](http://romiller.com/)

**Film wideo**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Musisz mieć co najmniej programu Visual Studio 2010 lub Visual Studio 2012 są zainstalowane w tym przewodniku.

Jeśli używasz programu Visual Studio 2010, należy również mieć [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) zainstalowane.

 

## <a name="1-create-an-existing-database"></a>1. Utwórz istniejącej bazy danych

Zazwyczaj przeznaczonych do istniejącej bazy danych, zostanie już utworzony, ale w tym przewodniku należy utworzyć bazy danych w celu uzyskania dostępu.

Serwer bazy danych, który został zainstalowany przy użyciu programu Visual Studio różni się zależnie od wersji programu Visual Studio zostały zainstalowane:

-   Jeśli używasz programu Visual Studio 2010 zostanie utworzona baza danych programu SQL Express.
-   Jeśli używasz programu Visual Studio 2012, a następnie zostanie utworzona [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) bazy danych.

 

Rozpocznijmy i wygenerować bazę danych.

-   Otwórz program Visual Studio
-   **Widok —&gt; Eksploratora serwera**
-   Kliknij prawym przyciskiem myszy **połączeń danych -&gt; Dodaj połączenie...**
-   Jeśli nie jest połączona z bazą danych za pomocą Eksploratora serwera przed, musisz wybrać programu Microsoft SQL Server jako źródło danych

    ![Wybierz źródło danych](~/ef6/media/selectdatasource.png)

-   Łączenie się z LocalDB lub SQL Express, w zależności od tego, który z nich został zainstalowany, a następnie wprowadź **DatabaseFirst.Blogging** jako nazwa bazy danych

    ![Połączenia programu SQL Express DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![Połączenie LocalDB DF](~/ef6/media/localdbconnectiondf.png)

-   Wybierz **OK** i uzyskasz, jeśli chcesz utworzyć nową bazę danych, wybierz opcję **tak**

    ![Tworzenie okna dialogowego baza danych](~/ef6/media/createdatabasedialog.png)

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
```

## <a name="2-create-the-application"></a>2. Tworzenie aplikacji

Aby zachować ich prostotę zamierzamy utworzyć aplikację konsoli podstawowe, która używa pierwszej bazy danych do dostępu do danych:

-   Otwórz program Visual Studio
-   **Plik —&gt; New -&gt; projektu...**
-   Wybierz **Windows** menu po lewej stronie i **aplikacji konsoli**
-   Wprowadź **DatabaseFirstSample** jako nazwę
-   Wybierz **OK**

 

## <a name="3-reverse-engineer-model"></a>3. Model odtwarzania

Zamierzamy korzystania z programu Entity Framework Designer, który wchodzi w skład programu Visual Studio, aby utworzyć nasz model.

-   **Projekt —&gt; Dodaj nowy element...**
-   Wybierz **danych** menu po lewej stronie i następnie **ADO.NET Entity Data Model**
-   Wprowadź **BloggingModel** jako nazwę i kliknij przycisk **OK**
-   Spowoduje to uruchomienie **Kreator modelu Entity Data Model**
-   Wybierz **Generuj z bazy danych** i kliknij przycisk **dalej**

    ![Kreator krok 1](~/ef6/media/wizardstep1.png)

-   Wybierz połączenie do bazy danych utworzonej w pierwszej sekcji, wprowadź **BloggingContext** jako nazwa parametrów połączenia i kliknij przycisk **dalej**

    ![Kreator krok 2](~/ef6/media/wizardstep2.png)

-   Kliknij pole wyboru obok "Tabele", aby zaimportować wszystkie tabele i kliknij przycisk "Zakończ"

    ![Kreator krok 3](~/ef6/media/wizardstep3.png)

 

Po zakończeniu procesu odtwarzania nowy model jest dodawane do projektu i otworzona w celu wyświetlania w Projektancie Entity Framework. Dodano również pliku App.config do projektu przy użyciu szczegółów połączenia dla bazy danych.

![Początkowa modelu](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Dodatkowe kroki w programie Visual Studio 2010

Jeśli pracujesz w programie Visual Studio 2010 istnieją pewne dodatkowe czynności, które należy wykonać uaktualnienie do najnowszej wersji programu Entity Framework. Uaktualnienie jest ważne, ponieważ daje ona dostęp do ulepszone powierzchni interfejsu API, który jest znacznie łatwiejsze do użycia, a także najnowsze poprawki.

Najpierw up, musimy pobrać najnowszą wersję programu Entity Framework z NuGet.

-   **Project —&gt; Zarządzaj pakietami NuGet... ** 
     *Jeśli nie masz **Zarządzaj pakietami NuGet... ** opcji, należy zainstalować [najnowszej wersji pakietu NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*
-   Wybierz **Online** kartę
-   Wybierz **EntityFramework** pakietu
-   Kliknij przycisk **instalacji**

Następnie należy zamienić nasz model, aby wygenerować kod, który korzysta z interfejsu API typu DbContext, który został wprowadzony w nowszych wersjach programu Entity Framework.

-   Kliknij prawym przyciskiem myszy na puste miejsce modelu w Projektancie platformy EF, a następnie wybierz pozycję **Dodaj element generowanie kodu...**
-   Wybierz **szablonów Online** z menu po lewej stronie i wyszukaj **DbContext**
-   Wybierz EF **5.x Generator DbContext dla języka C\#**, wprowadź **BloggingModel** jako nazwę i kliknij przycisk **Dodaj**

    ![Szablon typu DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a>4. Odczytywanie i zapisywanie danych

Teraz, gdy model, nadszedł czas na potrzeby dostępu do niektórych danych. Klasy użyjemy na potrzeby dostępu do danych są generowane automatycznie na podstawie pliku EDMX.

*Ten zrzut ekranu pochodzi z programu Visual Studio 2012, jeśli używasz programu Visual Studio 2010 BloggingModel.tt i BloggingModel.Context.tt plików będzie bezpośrednio w ramach projektu, a nie zagnieżdżony w pliku EDMX.*

![Wygenerowane klasy DF](~/ef6/media/generatedclassesdf.png)

 

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
 

## <a name="5-dealing-with-database-changes"></a>5. Zajmowanie się zmian w bazie danych

Teraz nadszedł czas, aby wprowadzić pewne zmiany na naszych schemat bazy danych, gdy firma Microsoft wprowadza te zmiany, należy również zaktualizować nasz model, aby odzwierciedlać wprowadzone zmiany.

Pierwszym krokiem jest wprowadzić pewne zmiany schematu bazy danych. Zamierzamy dodać tabelę użytkowników ze schematem.

-   Kliknij prawym przyciskiem myszy **DatabaseFirst.Blogging** bazy danych w Eksploratorze serwera i wybierz **nowe zapytanie**
-   Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonania**

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

Teraz, gdy schemat jest aktualizowany, nadszedł czas na aktualizowanie modelu z tymi zmianami.

-   Kliknij prawym przyciskiem myszy na puste miejsce w modelu w Projektancie platformy EF i wybierz opcję "Aktualizuj modelu z bazy danych...", co spowoduje uruchomienie Kreatora aktualizacji
-   Na karcie Dodaj Kreatora aktualizacji zaznacz pole wyboru obok tabel oznacza to, że chcemy dodać nowe tabele ze schematu.
    *Na karcie odświeżania pokazuje wszystkie istniejące tabele w modelu, który będzie sprawdzany zmian podczas aktualizacji. Usuń kartach wszystkie tabele zostały usunięte ze schematu, które zostaną także usunięte z modelu, w ramach aktualizacji. Informacje dotyczące tych dwóch kart jest wykrywany automatycznie i jest dostępne wyłącznie do celów informacyjnych, nie można zmienić dowolne ustawienia.*

    ![Odśwież Kreatora](~/ef6/media/refreshwizard.png)

-   Kliknij przycisk Zakończ w Kreatorze aktualizacji

 

Model został zaktualizowany do uwzględnienia nowej jednostki użytkownika, który jest mapowany do tabeli użytkowników, którą dodaliśmy do bazy danych.

![Zaktualizowano modelu](~/ef6/media/modelupdated.png)

## <a name="summary"></a>Podsumowanie

W tym przewodniku, który przyjrzeliśmy się tworzenie pierwszej bazy danych, które umożliwiło nam Tworzenie modelu w Projektancie platformy EF, w oparciu o istniejącą bazę danych. Następnie użyliśmy tego modelu do odczytu i zapisu pewne dane z bazy danych. Na koniec Zaktualizowaliśmy modelu aby odzwierciedlić zmiany wprowadzone do schematu bazy danych.
