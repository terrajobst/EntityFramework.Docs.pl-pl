---
title: Najpierw — model EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 3dd0eba29619f09995d7009dd29462c14bde98c4
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251144"
---
# <a name="model-first"></a>Najpierw modelu
W tym przewodniku krok po kroku i wideo zawierają wprowadzenie do rozwoju pierwszego modelu używający narzędzia Entity Framework. Model umożliwia najpierw utworzyć nowy model przy użyciu programu Entity Framework Designer, a następnie wygenerować schemat bazy danych z modelu. Model jest przechowywany w pliku EDMX (z rozszerzeniem edmx) i można wyświetlać i edytować w Projektancie Entity Framework. Klasy, które możesz korzystać z aplikacji są generowane automatycznie z pliku EDMX.

## <a name="watch-the-video"></a>Obejrzyj wideo
W tym przewodniku krok po kroku i wideo zawierają wprowadzenie do rozwoju pierwszego modelu używający narzędzia Entity Framework. Model umożliwia najpierw utworzyć nowy model przy użyciu programu Entity Framework Designer, a następnie wygenerować schemat bazy danych z modelu. Model jest przechowywany w pliku EDMX (z rozszerzeniem edmx) i można wyświetlać i edytować w Projektancie Entity Framework. Klasy, które możesz korzystać z aplikacji są generowane automatycznie z pliku EDMX.

**Osoba prezentująca**: [Rowan Miller](http://romiller.com/)

**Film wideo**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Musisz mieć program Visual Studio 2010 lub Visual Studio 2012 są zainstalowane w tym przewodniku.

Jeśli używasz programu Visual Studio 2010, należy również mieć [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) zainstalowane.

## <a name="1-create-the-application"></a>1. Tworzenie aplikacji

Aby zachować ich prostotę zamierzamy utworzyć aplikację konsoli podstawowe, która używa pierwszego modelu do dostępu do danych:

-   Otwórz program Visual Studio
-   **Plik —&gt; New -&gt; projektu...**
-   Wybierz **Windows** menu po lewej stronie i **aplikacji konsoli**
-   Wprowadź **ModelFirstSample** jako nazwę
-   Wybierz **OK**

## <a name="2-create-model"></a>2. Tworzenie modelu

Zamierzamy korzystania z programu Entity Framework Designer, który wchodzi w skład programu Visual Studio, aby utworzyć nasz model.

-   **Projekt —&gt; Dodaj nowy element...**
-   Wybierz **danych** menu po lewej stronie i następnie **ADO.NET Entity Data Model**
-   Wprowadź **BloggingModel** jako nazwę i kliknij przycisk **OK**, zostanie uruchomiony Kreator modelu Entity Data Model
-   Wybierz **pusty Model** i kliknij przycisk **Zakończ**

    ![Utwórz pusty Model](~/ef6/media/createemptymodel.png)

Entity Framework Designer jest otwierany przy użyciu modelu puste. Można teraz rozpocząć dodawanie jednostek, właściwości i skojarzenia do modelu.

-   Kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **właściwości**
-   Zmiana okna właściwości **nazwa kontenera jednostki** do **BloggingContext**
    *jest to nazwa pochodnej kontekst, który zostanie wygenerowany dla Ciebie kontekstu reprezentuje sesję z bazą danych, dzięki czemu nam zapytania i Zapisz dane*
-   Kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Dodaj nowy -&gt; jednostki...**
-   Wprowadź **Blog** jako nazwę podmiotu i **BlogId** jako nazwę klucza i kliknij przycisk **OK**

    ![Dodaj jednostkę Blog](~/ef6/media/addblogentity.png)

-   Kliknij prawym przyciskiem myszy na nową jednostkę na projekt powierzchni i wybierz **Dodaj nowy -&gt; właściwość skalarną**, wprowadź **nazwa** jako nazwę właściwości.
-   Powtórz ten proces, aby dodać **adresu Url** właściwości.
-   Kliknij prawym przyciskiem myszy **adresu Url** właściwość projekt powierzchni i wybierz **właściwości**, zmiana okna właściwości **Nullable** ustawienie **True** 
     *Dzięki temu można zapisać w blogu do bazy danych bez przypisywania go na adres Url*
-   Przy użyciu technik, możesz po prostu dzięki modelom uczenia, Dodaj **wpis** jednostki o **PostId** właściwości klucza
-   Dodaj **tytuł** i **zawartości** właściwości skalarne można **wpis** jednostki

Skoro już mamy kilka jednostek, nadszedł czas na dodawanie skojarzenia (lub relacji) między nimi.

-   Kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Dodaj nowy -&gt; skojarzenie...**
-   Wprowadzić jeden z końców relacji wskazują **Blog** z wartością liczebności równą **jeden** i innych punktu końcowego do **wpis** z wartością liczebności równą **wiele** 
     *Oznacza to, że blogu zawiera wiele wpisów i wpis należy do jednego Blog*
-   Upewnij się, **dodawania właściwości klucza obcego do jednostki "Post"** pole jest zaznaczone, a kliknij **OK**

    ![Dodaj skojarzenie MF](~/ef6/media/addassociationmf.png)

W efekcie powstał prosty model, który możemy Generuj z bazy danych i umożliwia odczytywanie i zapisywanie danych.

![Początkowa modelu](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Dodatkowe kroki w programie Visual Studio 2010

Jeśli pracujesz w programie Visual Studio 2010 istnieją pewne dodatkowe czynności, które należy wykonać uaktualnienie do najnowszej wersji programu Entity Framework. Uaktualnienie jest ważne, ponieważ daje ona dostęp do ulepszone powierzchni interfejsu API, który jest znacznie łatwiejsze do użycia, a także najnowsze poprawki.

Najpierw up, musimy pobrać najnowszą wersję programu Entity Framework z NuGet.

-   **Project —&gt; Zarządzaj pakietami NuGet... ** 
     *Jeśli nie masz **Zarządzaj pakietami NuGet... ** opcji, należy zainstalować [najnowszej wersji pakietu NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*
-   Wybierz **Online** kartę
-   Wybierz **EntityFramework** pakietu
-   Kliknij przycisk **instalacji**

Następnie należy zamienić nasz model, aby wygenerować kod, który korzysta z interfejsu API typu DbContext, który został wprowadzony w nowszych wersjach programu Entity Framework.

-   Kliknij prawym przyciskiem myszy na puste miejsce modelu w Projektancie platformy EF, a następnie wybierz pozycję **Dodaj element generowanie kodu...**
-   Wybierz **szablonów Online** z menu po lewej stronie i wyszukaj **DbContext**
-   Wybierz EF **5.x Generator DbContext dla języka C\#**, wprowadź **BloggingModel** jako nazwę i kliknij przycisk **Dodaj**

    ![Szablon typu DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a>3. Generowanie bazy danych

Biorąc pod uwagę nasz model, platformy Entity Framework można obliczyć schemat bazy danych, które pomogą nam do przechowywania i pobierania danych przy użyciu modelu.

Serwer bazy danych, który został zainstalowany przy użyciu programu Visual Studio różni się zależnie od wersji programu Visual Studio zostały zainstalowane:

-   Jeśli używasz programu Visual Studio 2010 zostanie utworzona baza danych programu SQL Express.
-   Jeśli używasz programu Visual Studio 2012, a następnie zostanie utworzona [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) bazy danych.

Rozpocznijmy i wygenerować bazę danych.

-   Kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Generuj z bazy danych z modelu...**
-   Kliknij przycisk **nowe połączenie...** a następnie określ LocalDB lub SQL Express, w zależności od instalowanej wersji programu Visual Studio, którego używasz, wprowadź **ModelFirst.Blogging** jako nazwa bazy danych.

    ![Połączenie LocalDB MF](~/ef6/media/localdbconnectionmf.png)

    ![Połączenia programu SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   Wybierz **OK** i uzyskasz, jeśli chcesz utworzyć nową bazę danych, wybierz opcję **tak**
-   Wybierz **dalej** i Entity Framework Designer obliczy skryptu, aby utworzyć schemat bazy danych
-   Gdy skrypt zostanie wyświetlona, kliknij przycisk **Zakończ** i skrypt zostanie dodany do projektu i otwarte
-   Kliknij prawym przyciskiem myszy skrypt i wybierz pozycję **Execute**, zostanie wyświetlony monit, aby określić bazę danych, aby nawiązać połączenie, podaj LocalDB lub SQL Server Express, w zależności od instalowanej wersji programu Visual Studio, którego używasz

## <a name="4-reading--writing-data"></a>4. Odczytywanie i zapisywanie danych

Teraz, gdy model, nadszedł czas na potrzeby dostępu do niektórych danych. Klasy użyjemy na potrzeby dostępu do danych są generowane automatycznie na podstawie pliku EDMX.

*Ten zrzut ekranu pochodzi z programu Visual Studio 2012, jeśli używasz programu Visual Studio 2010 BloggingModel.tt i BloggingModel.Context.tt plików będzie bezpośrednio w ramach projektu, a nie zagnieżdżony w pliku EDMX.*

![Wygenerowane klasy](~/ef6/media/generatedclasses.png)

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

## <a name="5-dealing-with-model-changes"></a>5. Obsługa zmiany modelu

Teraz nadszedł czas, aby wprowadzić pewne zmiany na nasz model, jeśli możemy wprowadzić te zmiany, należy również zaktualizować schemat bazy danych.

Rozpoczniemy przez dodanie nowego obiektu użytkownika w naszym modelu.

-   Dodaj nową **użytkownika** nazwa jednostki za pomocą **Username** jako nazwę klucza i **ciąg** jako typ właściwości klucza

    ![Dodaj jednostki użytkownika](~/ef6/media/adduserentity.png)

-   Kliknij prawym przyciskiem myszy **Username** właściwość projekt powierzchni i wybierz **właściwości**, we właściwościach okna zmiany **MaxLength** ustawienie **50 ** 
     *To ogranicza dane, które mogą być przechowywane w nazwa użytkownika używana do 50 znaków*
-   Dodaj **DisplayName** właściwości skalarne **użytkownika** jednostki

W efekcie powstał aktualizowanego modelu i jesteśmy gotowi zaktualizować bazę danych, aby obsłużyć nasz nowy typ jednostki użytkownika.

-   Kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Generuj z bazy danych z modelu...** , Platformy entity Framework obliczy skrypt, aby ponownie utworzyć schemat oparty na aktualizowanego modelu.
-   Kliknij przycisk **Zakończ**
-   Może odbierać ostrzeżeń o zastąpienie istniejącego skryptu języka DDL i mapowania i magazynu części modelu, kliknij przycisk **tak** dla obu tych ostrzeżeń
-   Zaktualizowano skrypt SQL w celu utworzenia bazy danych jest otwarty  
    *Skrypt, który jest generowany będzie porzucić wszystkie istniejące tabele i następnie ponownie Utwórz schemat od podstaw. To może działać na potrzeby lokalnego programowania, ale nie jest wygodną wypychania zmian do bazy danych, która została już wdrożona. Jeśli potrzebujesz opublikować zmiany w bazie danych, która została już wdrożona, konieczne będzie edytowanie skryptu lub użyj narzędzia do porównywania schematu do obliczania skryptów migracji.*
-   Kliknij prawym przyciskiem myszy skrypt i wybierz pozycję **Execute**, zostanie wyświetlony monit, aby określić bazę danych, aby nawiązać połączenie, podaj LocalDB lub SQL Server Express, w zależności od instalowanej wersji programu Visual Studio, którego używasz

## <a name="summary"></a>Podsumowanie

W tym przewodniku, który przyjrzeliśmy się pierwszy Model programowania, które umożliwiło nam Tworzenie modelu w Projektancie platformy EF, a następnie wygenerować bazę danych z tego modelu. Następnie użyliśmy modelu do odczytu i zapisu pewne dane z bazy danych. Na koniec mamy zaktualizowany modelu i tworzony ponownie schematu bazy danych, zgodny z modelem.
