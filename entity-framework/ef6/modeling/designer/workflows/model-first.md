---
title: Model First — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 1b37805beb3d33f0b6dad2577a8abb3ea8f7b1e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418106"
---
# <a name="model-first"></a>Model First
Ten film wideo i przewodnik krok po kroku zawierają wprowadzenie do Model First opracowywania przy użyciu Entity Framework. Model First umożliwia utworzenie nowego modelu przy użyciu Entity Framework Designer, a następnie wygenerowanie schematu bazy danych z modelu. Model jest przechowywany w pliku EDMX (rozszerzenie EDMX) i można go przeglądać i edytować w Entity Framework Designer. Klasy, z którymi można korzystać w aplikacji, są generowane automatycznie na podstawie pliku EDMX.

## <a name="watch-the-video"></a>Obejrzyj film
Ten film wideo i przewodnik krok po kroku zawierają wprowadzenie do Model First opracowywania przy użyciu Entity Framework. Model First umożliwia utworzenie nowego modelu przy użyciu Entity Framework Designer, a następnie wygenerowanie schematu bazy danych z modelu. Model jest przechowywany w pliku EDMX (rozszerzenie EDMX) i można go przeglądać i edytować w Entity Framework Designer. Klasy, z którymi można korzystać w aplikacji, są generowane automatycznie na podstawie pliku EDMX.

**Przedstawione przez**: [Rowan Miller](https://romiller.com/)

**Wideo**: [wmv](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik, musisz mieć zainstalowany program Visual Studio 2010 lub Visual Studio 2012.

W przypadku korzystania z programu Visual Studio 2010 należy również zainstalować pakiet [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .

## <a name="1-create-the-application"></a>1. Utwórz aplikację

Aby zachować prostotę, możemy utworzyć podstawową aplikację konsolową, która używa Model First do uzyskiwania dostępu do danych:

-   Otwórz program Visual Studio.
-   **Plik —&gt; nowy&gt; projekt...**
-   Wybierz pozycję **Windows** z menu po lewej stronie i **aplikacji konsolowej**
-   Wprowadź **ModelFirstSample** jako nazwę
-   Kliknij przycisk **OK**

## <a name="2-create-model"></a>2. Utwórz model

Będziemy używać Entity Framework Designer, które są dołączone jako część programu Visual Studio, aby utworzyć nasz model.

-   **Projekt —&gt; Dodaj nowy element...**
-   Wybierz pozycję **dane** z menu po lewej stronie, a następnie **ADO.NET Entity Data Model**
-   Wprowadź **BloggingModel** jako nazwę, a następnie kliknij przycisk **OK**. spowoduje to uruchomienie Kreatora Entity Data Model
-   Wybierz pozycję **pusty model** i kliknij przycisk **Zakończ** .

    ![Utwórz pusty model](~/ef6/media/createemptymodel.png)

Entity Framework Designer jest otwarty z pustym modelem. Teraz możemy rozpocząć dodawanie jednostek, właściwości i skojarzeń do modelu.

-   Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Właściwości**
-   W okno Właściwości zmienić **nazwę kontenera jednostki** na **BloggingContext**
    *jest to nazwa kontekstu pochodnego, który zostanie wygenerowany dla Ciebie, kontekst reprezentuje sesję z bazą danych, umożliwiając nam wykonywanie zapytań i zapisywanie danych*
-   Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Dodaj nową&gt; jednostki...**
-   Wprowadź **blog** jako nazwę jednostki i **BlogId** jako nazwę klucza, a następnie kliknij przycisk **OK** .

    ![Dodaj jednostkę blogu](~/ef6/media/addblogentity.png)

-   Kliknij prawym przyciskiem myszy nową jednostkę na powierzchni projektowej i wybierz polecenie **Dodaj nową&gt; Właściwość skalarna**, wprowadź **nazwę** jako nazwę właściwości.
-   Powtórz ten proces, aby dodać właściwość **adresu URL** .
-   Kliknij prawym przyciskiem myszy Właściwość **adres URL** na powierzchni projektowej i wybierz polecenie **właściwości**, w okno właściwości zmienić ustawienie **wartości null** na **true** ,
    *to umożliwia zapisanie blogu w bazie danych bez przypisywania go do adresu URL*
-   Korzystając z technik, które właśnie uczysz, Dodaj jednostkę **post** z właściwością klucza **PostId**
-   Dodawanie właściwości skalarnych **tytułu** i **zawartości** do jednostki **post**

Teraz, gdy mamy kilka jednostek, czas na dodanie skojarzenia (lub relacji) między nimi.

-   Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Dodaj nowe&gt; skojarzenie...**
-   Utwórz jeden koniec punktu relacji do **blogu** z liczebność **jednego** i drugiego punktu końcowego, aby **ogłosić** z liczebność **wielu**
    *oznacza to, że blog zawiera wiele ogłoszeń i wpis należy do jednego bloga*
-   Upewnij się, że pole **Dodaj właściwości klucza obcego do elementu "post"** jest zaznaczone, a następnie kliknij przycisk **OK** .

    ![Dodaj skojarzenie MF](~/ef6/media/addassociationmf.png)

Mamy teraz prosty model umożliwiający wygenerowanie bazy danych i używanie jej do odczytu i zapisu danych.

![Model początkowy](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Dodatkowe kroki w programie Visual Studio 2010

Jeśli pracujesz w programie Visual Studio 2010, należy wykonać kilka dodatkowych kroków, aby przeprowadzić uaktualnienie do najnowszej wersji programu Entity Framework. Aktualizacja jest ważna, ponieważ zapewnia dostęp do ulepszonej powierzchni interfejsu API, która jest znacznie łatwiejsza w użyciu, a także najnowszych poprawek błędów.

Najpierw należy uzyskać najnowszą wersję Entity Framework z narzędzia NuGet.

-   **Projekt —&gt; zarządzać pakietami NuGet...** 
    , *Jeśli nie masz opcji **Zarządzaj pakietami NuGet...** należy zainstalować [najnowszą wersję programu NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) *
-   Wybierz kartę **online**
-   Wybierz pakiet **EntityFramework**
-   Kliknij przycisk **Instaluj**

Następnie musimy wymienić nasz model, aby wygenerować kod, który korzysta z interfejsu API DbContext, który został wprowadzony w nowszych wersjach Entity Framework.

-   Kliknij prawym przyciskiem myszy pusty punkt w modelu w programie Dr Designer i wybierz polecenie **Dodaj element generowania kodu...**
-   Wybierz pozycję **Szablony online** z menu po lewej stronie i Wyszukaj w usłudze **DbContext**
-   Wybierz pozycję Dr **5. x DbContext generator dla C\#** , wprowadź **BloggingModel** jako nazwę, a następnie kliknij przycisk **Dodaj** .

    ![Szablon DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a>3. Generowanie bazy danych

Mając nasz model, Entity Framework może obliczyć schemat bazy danych, który umożliwi nam przechowywanie i pobieranie danych przy użyciu modelu.

Serwer bazy danych zainstalowany przy użyciu programu Visual Studio różni się w zależności od zainstalowanej wersji programu Visual Studio:

-   Jeśli używasz programu Visual Studio 2010, tworzysz bazę danych SQL Express.
-   Jeśli używasz programu Visual Studio 2012, zostanie utworzona baza danych [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .

Przyjrzyjmy się i wygenerujemy bazę danych.

-   Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Generuj bazę danych na podstawie modelu...**
-   Kliknij pozycję **nowe połączenie...** i określ LocalDB lub SQL Express, w zależności od używanej wersji programu Visual Studio, wprowadź **ModelFirst. blog** jako nazwę bazy danych.

    ![LocalDB połączenie MF](~/ef6/media/localdbconnectionmf.png)

    ![Połączenie SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   Wybierz pozycję **OK** . pojawi się monit z prośbą o utworzenie nowej bazy danych, a następnie wybierz pozycję **tak** .
-   Wybierz pozycję **dalej** , a Entity Framework Designer będzie obliczać skrypt, aby utworzyć schemat bazy danych
-   Po wyświetleniu skryptu kliknij przycisk **Zakończ** , a skrypt zostanie dodany do projektu i otwarty
-   Kliknij prawym przyciskiem myszy skrypt i wybierz polecenie **Wykonaj**, zostanie wyświetlony monit o określenie bazy danych, z którą chcesz nawiązać połączenie, określ LocalDB lub SQL Server Express, w zależności od używanej wersji programu Visual Studio

## <a name="4-reading--writing-data"></a>4. odczytywanie & zapisywania danych

Teraz, gdy mamy już model, który jest używany do uzyskiwania dostępu do niektórych danych. Klasy, które będą używane do uzyskiwania dostępu do danych są automatycznie generowane na podstawie pliku EDMX.

*Ten zrzut ekranu pochodzi z programu Visual Studio 2012, jeśli używasz programu Visual Studio 2010 pliki BloggingModel.tt i BloggingModel.Context.tt będą znajdować się bezpośrednio w projekcie, a nie zagnieżdżone w pliku EDMX.*

![Wygenerowane klasy](~/ef6/media/generatedclasses.png)

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

## <a name="5-dealing-with-model-changes"></a>5. postępowanie z zmianami modelu

Teraz można wprowadzić pewne zmiany w modelu, gdy wprowadzimy te zmiany, należy również zaktualizować schemat bazy danych.

Zaczniemy od dodania nowej jednostki użytkownika do naszego modelu.

-   Dodaj nową nazwę jednostki **użytkownika** z nazwą **użytkownika** jako nazwę klucza i **ciąg** jako typ właściwości klucza

    ![Dodaj jednostkę użytkownika](~/ef6/media/adduserentity.png)

-   Kliknij prawym przyciskiem myszy Właściwość **username** na powierzchni projektowej i wybierz polecenie **właściwości**, w okno właściwości zmienić ustawienie **MaxLength** na **50**
    *to ogranicza dane, które mogą być przechowywane w nazwie użytkownika do 50 znaków*
-   Dodawanie właściwości skalarnej **DisplayName** do jednostki **użytkownika**

Mamy już zaktualizowany model i jesteśmy gotowi do zaktualizowania bazy danych, aby pomieścić nasz nowy typ jednostki użytkownika.

-   Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Generuj bazę danych na podstawie modelu...** , Entity Framework obliczy skrypt, aby odtworzyć schemat w oparciu o zaktualizowany model.
-   Kliknij przycisk **Zakończ**
-   Można otrzymywać ostrzeżenia o zastępowaniu istniejącego skryptu DDL oraz o mapowaniu i magazynowaniu elementów modelu, a następnie kliknąć przycisk **tak** dla obu tych ostrzeżeń
-   Zaktualizowany skrypt SQL służący do tworzenia bazy danych jest otwarty dla Ciebie  
    *Wygenerowany skrypt spowoduje porzucenie wszystkich istniejących tabel, a następnie ponowne utworzenie schematu od podstaw. Może to współdziałać z programowaniem lokalnym, ale nie jest zdolny do wypychania zmian w bazie danych, która została już wdrożona. Jeśli musisz opublikować zmiany w bazie danych, która została już wdrożona, musisz edytować skrypt lub użyć narzędzia do porównywania schematów, aby obliczyć skrypt migracji.*
-   Kliknij prawym przyciskiem myszy skrypt i wybierz polecenie **Wykonaj**, zostanie wyświetlony monit o określenie bazy danych, z którą chcesz nawiązać połączenie, określ LocalDB lub SQL Server Express, w zależności od używanej wersji programu Visual Studio

## <a name="summary"></a>Podsumowanie

W tym instruktażu oglądamy Model First opracowywanie, które umożliwiają utworzenie modelu w programie Dr Designer, a następnie wygenerowanie bazy danych z tego modelu. Następnie korzystamy z modelu, aby odczytywać i zapisywać dane z bazy danych. Na koniec Zaktualizowaliśmy model, a następnie ponownie utworzony schemat bazy danych w celu dopasowania go do modelu.
