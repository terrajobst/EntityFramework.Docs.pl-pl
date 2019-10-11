---
title: Database First — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: d40cff4ddccf43a394ef4f244653372a5a89b05a
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182451"
---
# <a name="database-first"></a>Database First
Ten film wideo i przewodnik krok po kroku zawierają wprowadzenie do Database First opracowywania przy użyciu Entity Framework. Database First umożliwia odtwarzanie modelu z istniejącej bazy danych. Model jest przechowywany w pliku EDMX (rozszerzenie EDMX) i można go przeglądać i edytować w Entity Framework Designer. Klasy, z którymi można korzystać w aplikacji, są generowane automatycznie na podstawie pliku EDMX.

## <a name="watch-the-video"></a>Obejrzyj wideo
To wideo zawiera wprowadzenie do Database First tworzenia przy użyciu Entity Framework. Database First umożliwia odtwarzanie modelu z istniejącej bazy danych. Model jest przechowywany w pliku EDMX (rozszerzenie EDMX) i można go przeglądać i edytować w Entity Framework Designer. Klasy, z którymi można korzystać w aplikacji, są generowane automatycznie na podstawie pliku EDMX.

**Przedstawione przez**: [Rowan Miller](https://romiller.com/)

**Film wideo**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik, musisz mieć zainstalowany co najmniej program Visual Studio 2010 lub Visual Studio 2012.

W przypadku korzystania z programu Visual Studio 2010 należy również zainstalować pakiet [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .

 

## <a name="1-create-an-existing-database"></a>1. Tworzenie istniejącej bazy danych

Zwykle w przypadku określania docelowej istniejącej bazy danych zostanie ona już utworzona, ale w tym instruktażu musimy utworzyć bazę danych do uzyskania dostępu.

Serwer bazy danych zainstalowany przy użyciu programu Visual Studio różni się w zależności od zainstalowanej wersji programu Visual Studio:

-   Jeśli używasz programu Visual Studio 2010, tworzysz bazę danych SQL Express.
-   Jeśli używasz programu Visual Studio 2012, zostanie utworzona baza danych [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .

 

Przyjrzyjmy się i wygenerujemy bazę danych.

-   Otwórz program Visual Studio
-   **Widok-&gt; Eksplorator serwera**
-   Kliknij prawym przyciskiem myszy pozycję **połączenia danych-&gt; Dodaj połączenie...**
-   Jeśli nie masz połączenia z bazą danych Eksplorator serwera przed wybraniem Microsoft SQL Server jako źródła danych

    ![Wybieranie źródła danych](~/ef6/media/selectdatasource.png)

-   Połącz się z usługą LocalDB lub SQL Express, w zależności od tego, który został zainstalowany, a następnie wprowadź **DatabaseFirst. blog** jako nazwę bazy danych

    ![DF połączenia SQL Express](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDB połączenia DF](~/ef6/media/localdbconnectiondf.png)

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
```

## <a name="2-create-the-application"></a>2. Tworzenie aplikacji

Aby zachować prostotę, możemy utworzyć podstawową aplikację konsolową, która używa Database First do uzyskiwania dostępu do danych:

-   Otwórz program Visual Studio
-   **Plik-&gt; nowy-&gt; projektu...**
-   Wybierz pozycję **Windows** z menu po lewej stronie i **aplikacji konsolowej**
-   Wprowadź **DatabaseFirstSample** jako nazwę
-   Wybierz **przycisk OK**

 

## <a name="3-reverse-engineer-model"></a>3. Odtwórz Model

Będziemy używać Entity Framework Designer, które są dołączone jako część programu Visual Studio, aby utworzyć nasz model.

-   **Projekt-&gt; Dodaj nowy element...**
-   Wybierz pozycję **dane** z menu po lewej stronie, a następnie **ADO.NET Entity Data Model**
-   Wprowadź **BloggingModel** jako nazwę, a następnie kliknij przycisk **OK** .
-   Spowoduje to uruchomienie **kreatora Entity Data Model**
-   Wybierz pozycję **Generuj z bazy danych** i kliknij przycisk **dalej** .

    ![Krok 1 Kreatora](~/ef6/media/wizardstep1.png)

-   Wybierz połączenie z bazą danych utworzoną w pierwszej sekcji, wprowadź **BloggingContext** jako nazwę parametrów połączenia i kliknij przycisk **dalej** .

    ![Krok 2 Kreatora](~/ef6/media/wizardstep2.png)

-   Kliknij pole wyboru obok pozycji "tabele", aby zaimportować wszystkie tabele, a następnie kliknij przycisk Zakończ.

    ![Krok 3 Kreatora](~/ef6/media/wizardstep3.png)

 

Po zakończeniu procesu odtwarzania nowy model zostanie dodany do projektu i otwarty do wyświetlania w Entity Framework Designer. Plik App. config został również dodany do projektu i zawiera szczegółowe informacje o połączeniu dla bazy danych.

![Model początkowy](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Dodatkowe kroki w programie Visual Studio 2010

Jeśli pracujesz w programie Visual Studio 2010, należy wykonać kilka dodatkowych kroków, aby przeprowadzić uaktualnienie do najnowszej wersji programu Entity Framework. Aktualizacja jest ważna, ponieważ zapewnia dostęp do ulepszonej powierzchni interfejsu API, która jest znacznie łatwiejsza w użyciu, a także najnowszych poprawek błędów.

Najpierw należy uzyskać najnowszą wersję Entity Framework z narzędzia NuGet.

-   **Projekt — &gt; Zarządzaj pakietami NuGet...** 
    *Jeśli nie masz opcji **Zarządzaj pakietami NuGet...** należy zainstalować [najnowszą wersję programu NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) *
-   Wybierz kartę **online**
-   Wybierz pakiet **EntityFramework**
-   Kliknij przycisk **Instaluj**

Następnie musimy wymienić nasz model, aby wygenerować kod, który korzysta z interfejsu API DbContext, który został wprowadzony w nowszych wersjach Entity Framework.

-   Kliknij prawym przyciskiem myszy pusty punkt w modelu w programie Dr Designer i wybierz polecenie **Dodaj element generowania kodu...**
-   Wybierz pozycję **Szablony online** z menu po lewej stronie i Wyszukaj w usłudze **DbContext**
-   Wybierz pozycję Dr **5. x DbContext generator dla języka C @ no__t-1**, wprowadź **BloggingModel** jako nazwę i kliknij przycisk **Dodaj** .

    ![Szablon DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a>4. Odczytywanie & zapisywania danych

Teraz, gdy mamy już model, który jest używany do uzyskiwania dostępu do niektórych danych. Klasy, które będą używane do uzyskiwania dostępu do danych są automatycznie generowane na podstawie pliku EDMX.

*Ten zrzut ekranu pochodzi z programu Visual Studio 2012, jeśli używasz programu Visual Studio 2010 pliki BloggingModel.tt i BloggingModel.Context.tt będą znajdować się bezpośrednio w projekcie, a nie zagnieżdżone w pliku EDMX.*

![Wygenerowane klasy DF](~/ef6/media/generatedclassesdf.png)

 

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
 

## <a name="5-dealing-with-database-changes"></a>5. Rozpatrywanie zmian w bazie danych

Teraz można wprowadzić pewne zmiany w naszym schemacie bazy danych, po wprowadzeniu tych zmian należy zaktualizować nasz model, aby odzwierciedlić te zmiany.

Pierwszym krokiem jest wprowadzenie pewnych zmian w schemacie bazy danych. Zamierzamy dodać tabelę użytkowników do schematu.

-   Kliknij prawym przyciskiem myszy bazę danych **DatabaseFirst. Blogi** w Eksplorator serwera i wybierz pozycję **nowe zapytanie** .
-   Skopiuj następujące SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Execute (wykonaj** ).

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

Teraz, gdy schemat został zaktualizowany, należy zaktualizować model o te zmiany.

-   Kliknij prawym przyciskiem myszy pusty punkt w modelu w programie Dr Designer i wybierz polecenie "Aktualizuj model z bazy danych...", co spowoduje uruchomienie Kreatora aktualizacji
-   Na karcie dodawanie kreatora aktualizacji zaznacz pole wyboru obok pozycji tabele, co oznacza, że chcemy dodać nowe tabele ze schematu.
    Karta *The Refresh zawiera wszystkie istniejące tabele w modelu, które będą sprawdzane pod kątem zmian podczas aktualizacji. Karty Usuń pokazują wszystkie tabele, które zostały usunięte ze schematu i również zostaną usunięte z modelu w ramach aktualizacji. Informacje dotyczące tych dwóch kart są wykrywane automatycznie i udostępniane wyłącznie do celów informacyjnych. nie można zmienić żadnych ustawień.*

    ![Kreator odświeżania](~/ef6/media/refreshwizard.png)

-   Kliknij przycisk Zakończ w Kreatorze aktualizacji

 

Model zostanie zaktualizowany w celu uwzględnienia nowej jednostki użytkownika, która jest mapowana na tabelę użytkowników dodaną do bazy danych.

![Zaktualizowano model](~/ef6/media/modelupdated.png)

## <a name="summary"></a>Podsumowanie

W tym instruktażu oglądamy Database First opracowywanie, które umożliwiają nam tworzenie modelu w programie Dr Designer na podstawie istniejącej bazy danych. Następnie korzystamy z tego modelu, aby odczytywać i zapisywać dane z bazy danych. Na koniec Zaktualizowaliśmy model w celu odzwierciedlenia zmian wprowadzonych w schemacie bazy danych.
