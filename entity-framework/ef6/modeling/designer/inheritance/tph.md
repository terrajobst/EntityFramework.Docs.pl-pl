---
title: Projektant TPH — dziedziczenie — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418428"
---
# <a name="designer-tph-inheritance"></a>TPH dziedziczenia projektanta
W tym przewodniku krok po kroku pokazano, jak zaimplementować dziedziczenie dla hierarchii (TPH) w modelu koncepcyjnym przy użyciu Entity Framework Designer (program EF Designer). Dziedziczenie TPH używa jednej tabeli bazy danych do przechowywania danych dla wszystkich typów jednostek w hierarchii dziedziczenia.

W tym instruktażu zamapujemy tabelę Person na trzy typy jednostek: Person (typ podstawowy), Student (pochodzi od osoby) i instruktora (pochodzi od osoby). Utworzymy model koncepcyjny na podstawie bazy danych (Database First), a następnie zmienimy model w celu zaimplementowania dziedziczenia TPH przy użyciu narzędzia Dr Designer.

Istnieje możliwość mapowania na dziedziczenie TPH przy użyciu Model First, ale należy napisać własny przepływ pracy generowania bazy danych, który jest skomplikowany. Następnie można przypisać ten przepływ pracy do właściwości **przepływu pracy generowania bazy danych** w programie Dr Designer. Łatwiejszym rozwiązaniem jest użycie Code First.

## <a name="other-inheritance-options"></a>Inne opcje dziedziczenia

Tabela na typ (TPT) jest innym typem dziedziczenia, w którym oddzielne tabele w bazie danych są mapowane na jednostki, które uczestniczą w dziedziczeniu.  Aby uzyskać informacje o sposobach mapowania dziedziczenia na typ tabeli przy użyciu narzędzia Dr Designer, zobacz [dziedziczenie w programie EF Designer TPT](~/ef6/modeling/designer/inheritance/tpt.md).

Dziedziczenie typu "tabela-konkretna" (TPC) i modele dziedziczenia mieszanego są obsługiwane przez środowisko uruchomieniowe Entity Framework, ale nie są obsługiwane przez projektanta EF. Jeśli chcesz użyć programu TPC lub dziedziczenia mieszanego, masz dwie opcje: Użyj Code First lub ręcznie Edytuj plik EDMX. Jeśli zdecydujesz się pracować z plikiem EDMX, okno Szczegóły mapowania zostanie umieszczone w trybie bezpiecznym i nie będzie można użyć projektanta, aby zmienić mapowania.

## <a name="prerequisites"></a>Wymagania wstępne

W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:

- Najnowsza wersja programu Visual Studio.
- [Przykładowa baza danych szkoły](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Konfigurowanie projektu

-   Open Visual Studio 2012.
-   Wybierz pozycję **plik —&gt; nowy&gt; projekt**
-   W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **konsoli** .
-   Wprowadź **TPHDBFirstSample** jako nazwę.
-   Wybierz **przycisk OK**.

## <a name="create-a-model"></a>Tworzenie modelu

-   Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań i wybierz polecenie **dodaj&gt; nowy element**.
-   Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.
-   W polu Nazwa pliku wprowadź **TPHModel. edmx** , a następnie kliknij przycisk **Dodaj**.
-   W oknie dialogowym Wybierz zawartość modelu wybierz pozycję **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.
-   Kliknij pozycję **nowe połączenie**.
    W oknie dialogowym właściwości połączenia wprowadź nazwę serwera (na przykład **(LocalDB)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz  **szkoły** dla nazwy bazy danych, a następnie kliknij przycisk **OK**.
    Okno dialogowe Wybieranie połączenia danych zostanie zaktualizowane przy użyciu ustawienia połączenia z bazą danych.
-   W oknie dialogowym Wybierz obiekty bazy danych w węźle tabele wybierz tabelę **osoba** .
-   Kliknij przycisk **Zakończ**.

Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu. Wszystkie obiekty wybrane w oknie dialogowym Wybierz obiekty bazy danych zostaną dodane do modelu.

Jest to sposób, w jaki tabela **osoba** przeszukuje bazę danych.

![Tabela osób](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>Implementowanie dziedziczenia w hierarchii na poziomie tabeli

Tabela **osób** ma kolumnę **rozróżniacza** , która może mieć jedną z dwóch wartości: "Student" i "instruktor". W zależności od wartości tabeli **Person** zostanie zamapowana na jednostkę **ucznia** lub jednostkę **instruktora** . Tabela **osób** ma także dwie kolumny: **HireDate** i **EnrollmentDate**, które muszą mieć **wartość null** , ponieważ osoba nie może być studentem i instruktorem w tym samym czasie (co najmniej w tym instruktażu).

### <a name="add-new-entities"></a>Dodaj nowe jednostki

-   Dodaj nową jednostkę.
    W tym celu kliknij prawym przyciskiem myszy puste miejsce na powierzchni projektowej Entity Framework Designer i wybierz polecenie **dodaj&gt;jednostkę**.
-   Wpisz **instruktora**  **nazwy jednostki** a następnie wybierz pozycję **osoba** z listy rozwijanej dla **typu podstawowego**.
-   Kliknij przycisk **OK**.
-   Dodaj kolejną nową jednostkę. Wpisz  **uczniów** dla **nazwy jednostki** a następnie wybierz pozycję **osoba** z listy rozwijanej **Typ podstawowy**.

Dodano dwa nowe typy jednostek do powierzchni projektowej. Strzałka wskazuje od nowych typów jednostek do **osoby** typ jednostki; oznacza to, że **osoba** jest typem podstawowym dla nowych typów jednostek.

-   Kliknij prawym przyciskiem myszy Właściwość **HireDate** jednostki  **osoby** . Wybierz pozycję **Wytnij** (lub użyj klawisza Ctrl-X).
-   Kliknij prawym przyciskiem myszy jednostkę  **instruktora** i wybierz polecenie **Wklej** (lub użyj klawisza Ctrl-V).
-   Kliknij prawym przyciskiem myszy Właściwość **HireDate** i wybierz pozycję **Właściwości**.
-   W oknie **właściwości** ustaw właściwość **null** na **wartość false**.
-   Kliknij prawym przyciskiem myszy Właściwość **EnrollmentDate** jednostki  **osoby** . Wybierz pozycję **Wytnij** (lub użyj klawisza Ctrl-X).
-   Kliknij prawym przyciskiem myszy jednostkę **studenta** i wybierz polecenie **Wklej (lub użyj klawisza Ctrl-V).**
-   Wybierz właściwość **EnrollmentDate** i ustaw właściwość  **nullable** na **wartość false**.
-   Wybierz typ jednostki  **osoby** . W oknie **właściwości** Ustaw dla jego właściwości **abstract**  **wartość true**.
-   Usuń właściwość **rozróżniacza** z **osoby**. Powód, który powinien zostać usunięty, został wyjaśniony w następnej sekcji.

### <a name="map-the-entities"></a>Mapowanie jednostek

-   Kliknij prawym przyciskiem myszy **instruktora** i wybierz pozycję **Mapowanie tabeli.**
    W oknie Szczegóły mapowania została wybrana jednostka instruktora.
-   Kliknij **&lt;dodać tabelę lub widok&gt;**  w oknie **szczegóły mapowania** .
     **&lt;dodać tabelę lub widok&gt;**  stanie się listy rozwijanej tabel lub widoków, do których można zamapować wybraną jednostkę.
-   Z listy rozwijanej wybierz pozycję **osoba** .
-   Okno **szczegóły mapowania** zostało zaktualizowane z domyślnymi mapowaniami kolumn i opcją dodawania warunku.
-   Kliknij **&lt;dodać warunek&gt;** .
     **&lt;dodać warunek&gt;**  stanie się listy rozwijanej kolumn, dla których można ustawić warunki.
-   Z listy rozwijanej wybierz pozycję **rozróżniacz** .
-   W kolumnie  **operatora** okna **szczegóły mapowania** wybierz pozycję = z listy rozwijanej.
-   W kolumnie **wartość/właściwość** wpisz **instruktor**. Wynik końcowy powinien wyglądać następująco:

    ![Szczegóły mapowania](~/ef6/media/mappingdetails2.png)

-   Powtórz te kroki dla **ucznia** typ jednostki, ale Oznacz warunek jako wartość **ucznia** .  
    *Powód usunięcia właściwości **rozróżniacza** jest spowodowany tym, że nie można zmapować kolumny tabeli więcej niż raz. Ta kolumna będzie używana na potrzeby mapowania warunkowego, dlatego nie można jej używać również do mapowania właściwości. Jedynym sposobem użycia dla obu, jeśli warunek używa elementu **is o wartości null** lub **nie jest wartością null** porównaniem.*

Dziedziczenie na poziomie tabeli jest teraz zaimplementowane.

![TPH końcowy](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>Korzystanie z modelu

Otwórz plik **program.cs** , w którym jest zdefiniowana Metoda **Main** . Wklej następujący kod do funkcji **Main** . Kod wykonuje trzy zapytania. Pierwsze zapytanie powoduje przywrócenie wszystkich obiektów **osób** . Drugie zapytanie używa metody **OfType** do zwracania obiektów **instruktora** . Trzecie zapytanie używa metody **OfType** do zwracania obiektów **ucznia** .

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
