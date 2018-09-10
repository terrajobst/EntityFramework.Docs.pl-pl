---
title: Tabela projektanta dzielenia - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f07aeb0aa679f6fa8131c667ac808f17c3f03f20
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250988"
---
# <a name="designer-table-splitting"></a>Tabela projektanta dzielenia
Ten poradnik pokazuje jak mapować wielu typów jednostek do pojedynczej tabeli, modyfikując model przy użyciu narzędzia Entity Framework Designer (Projektant EF).

Jeden powodów, dla którego chcesz użyć tabeli podział opóźnia ładowanie niektórych właściwości podczas korzystania z opóźnieniem, ładowania do ładowania obiektów. Można oddzielić właściwości, które mogą zawierać bardzo duże ilości danych na osobne jednostki i tylko załadować je, gdy jest to wymagane.

Na poniższej ilustracji przedstawiono główne systemu windows, które są używane podczas pracy z projektancie platformy EF.

![Projektancie platformy EF](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Wymagania wstępne

W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:

- Najnowszą wersję programu Visual Studio.
- [Przykładowej bazy danych School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Konfigurowanie projektu

Ten przewodnik korzysta z programu Visual Studio 2012.

-   Otwórz program Visual Studio 2012.
-   Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**.
-   W okienku po lewej stronie kliknij Visual C\#, a następnie wybierz szablon Aplikacja konsoli.
-   Wprowadź **TableSplittingSample** jako nazwę projektu i kliknij przycisk **OK**.

## <a name="create-a-model-based-on-the-school-database"></a>Tworzenie modelu, w oparciu o bazę danych School

-   Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wskaż opcję **Dodaj**, a następnie kliknij przycisk **nowy element**.
-   Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów.
-   Wprowadź **TableSplittingModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**.
-   W oknie dialogowym Wybierz zawartość modelu, wybierz **Generuj z bazy danych**, a następnie kliknij przycisk **dalej.**
-   Kliknij przycisk nowe połączenie. W oknie dialogowym właściwości połączenia, wprowadź nazwę serwera (na przykład **(localdb)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz **School** nazwy bazy danych, a następnie Kliknij przycisk **OK**.
    Okno dialogowe Wybierz połączenie danych jest aktualizowana ustawienie połączenia bazy danych.
-   W oknie dialogowym Wybierz obiekty bazy danych należy ujawniać **tabel** węzła i wyboru **osoby** tabeli. Spowoduje to dodanie określonej tabeli, aby **School** modelu.
-   Kliknij przycisk **Zakończ**.

Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu. Wszystkie obiekty, które wybrano w **wybierz obiekty bazy danych** okno dialogowe, są dodawane do modelu.

## <a name="map-two-entities-to-a-single-table"></a>Dwie jednostki mapy do pojedynczej tabeli

W tej sekcji zostanie podzielona **osoby** jednostki do dwóch jednostek i zamapować je do jednej tabeli.

> [!NOTE]
> **Osoby** jednostka nie zawiera żadnych właściwości, które mogą zawierać dużą ilość danych; jest używany tylko jako przykład.

-   Kliknij prawym przyciskiem myszy pusty obszar powierzchni projektu, wskaż opcję **Dodaj nowe**i kliknij przycisk **jednostki**.
    **Nowa jednostka** pojawi się okno dialogowe.
-   Typ **HireInfo** dla **nazwa jednostki** i **PersonID** dla **właściwości klucza** nazwy.
-   Kliknij przycisk **OK**.
-   Nowy typ jednostki jest tworzone i wyświetlane na powierzchni projektowej.
-   Wybierz **DataZatrudnienia** właściwość **osoby** typu jednostki, a następnie naciśnij klawisz **Ctrl + X** kluczy.
-   Wybierz **HireInfo** jednostki, a następnie naciśnij klawisz **klawisze Ctrl + V** kluczy.
-   Tworzenie skojarzenia między **osoby** i **HireInfo**. Aby to zrobić, kliknij prawym przyciskiem myszy pusty obszar powierzchni projektu, wskaż opcję **Dodaj nowe**i kliknij przycisk **skojarzenie**.
-   **Dodawanie skojarzenia** pojawi się okno dialogowe. **PersonHireInfo** domyślnie zostanie podana nazwa.
-   Określać liczebność **1(One)** po obu stronach relacji.
-   Naciśnij klawisz **OK**.

Następny krok wymaga **szczegóły mapowania** okna. Jeśli nie widzisz tego okna, kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **szczegóły mapowania**.

-   Wybierz **HireInfo** typu jednostki i kliknij przycisk **&lt;Dodaj tabelę lub widok&gt;** w **szczegóły mapowania** okna.
-   Wybierz **osoby** z **&lt;Dodaj tabelę lub widok&gt;** pole listy rozwijanej. Lista zawiera tabele lub widoki, które można mapować wybranej jednostki.
    Odpowiednie właściwości powinny być mapowane domyślnie.

    ![Mapowanie](~/ef6/media/mapping.png)

-   Wybierz **PersonHireInfo** skojarzenia na powierzchni projektowej.
-   Kliknij prawym przyciskiem myszy asocjacji na projekt powierzchni i wybierz **właściwości**.
-   W **właściwości** wybierz **ograniczenia referencyjne** właściwości i kliknij przycisk z wielokropkiem.
-   Wybierz **osoby** z **jednostki** listy rozwijanej.
-   Naciśnij klawisz **OK**.

 

## <a name="use-the-model"></a>Użyj modelu

-   Wklej następujący kod dla metody Main.

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   Skompilować i uruchomić aplikację.

Poniższe instrukcje języka T-SQL zostały wykonane na **School** bazy danych w wyniku działania tej aplikacji. 

-   Następujące **Wstaw** zostało wykonane w wyniku wykonania z kontekstu. SaveChanges() i łączy dane z **osoby** i **HireInfo** jednostek

    ![Insert](~/ef6/media/insert.png)

-   Następujące **wybierz** zostało wykonane w wyniku wykonania z kontekstu. People.FirstOrDefault() i wybiera tylko potrzebne kolumny mapowane na **osoby**

    ![Wybierz 1](~/ef6/media/select1.png)

-   Następujące **wybierz** zostało wykonane w wyniku uzyskiwania dostępu do existingPerson.Instructor właściwość nawigacji z wybiera tylko potrzebne kolumny, które są mapowane na **HireInfo**

    ![Wybierz opcję 2](~/ef6/media/select2.png)
