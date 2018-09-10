---
title: Dziedziczenie projektanta TPH - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 1eb935414b20d6e93e9d470ccc845bc13626ed3a
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250847"
---
# <a name="designer-tph-inheritance"></a>Dziedziczenie TPH projektanta
Ten przewodnik krok po kroku pokazano, jak zaimplementować Tabela wg hierarchii (TPH) dziedziczenia w model koncepcyjny za pomocą funkcji Entity Framework Designer (Projektant EF). Dziedziczenie TPH używa jednej tabeli bazy danych do przechowywania danych dla wszystkich typów jednostek w hierarchii dziedziczenia.

W ramach tego przewodnika firma Microsoft będzie zmapowana tabeli osób do trzech typów jednostek: osoba (typ podstawowy), uczniów (pochodzi od osoby) i przez instruktorów (pochodzi od osoby). Utworzymy model koncepcyjny z bazy danych (Database First) i następnie zmienić model, który ma implementują dziedziczenie TPH przy użyciu projektanta EF.

Można zamapować na dziedziczenie TPH, za pomocą funkcji First modelu, ale trzeba napisać własne przepływu pracy generowanie bazy danych, którą jest złożony. Następnie można przypisać ten przepływ pracy ma **generowanie bazy danych w przepływie pracy** właściwości w Projektancie platformy EF. Alternatywą łatwiej jest używać Code First.

## <a name="other-inheritance-options"></a>Inne opcje dziedziczenia

Tabela wg typu (TPT) jest inny typ dziedziczenia, w którym oddzielnych tabel w bazie danych są mapowane do jednostek, które uczestniczą w dziedziczeniu.  Aby uzyskać informacje o mapowaniu tabeli na typ dziedziczenia z projektancie platformy EF, zobacz [dziedziczenia TPT projektancie platformy EF](~/ef6/modeling/designer/inheritance/tpt.md).

Dziedziczenie typu tabeli na konkretny (TPC) i wzory mieszane dziedziczenia są obsługiwane przez środowisko uruchomieniowe programu Entity Framework, ale nie są obsługiwane w Projektancie platformy EF. Jeśli chcesz użyć TPC lub mieszane dziedziczenia, masz dwie opcje: Użyj Code First lub ręczna Edycja pliku EDMX. Jeśli użytkownik chce pracować z pliku EDMX, okno Szczegóły mapowania, które zostaną wprowadzone w "trybie awaryjnym" i nie można zmienić mapowania za pomocą projektanta.

## <a name="prerequisites"></a>Wymagania wstępne

W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:

- Najnowszą wersję programu Visual Studio.
- [Przykładowej bazy danych School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Konfigurowanie projektu

-   Otwórz program Visual Studio 2012.
-   Wybierz **plikach&gt; New -&gt; projektu**
-   W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu.
-   Wprowadź **TPHDBFirstSample** jako nazwę.
-   Wybierz **OK**.

## <a name="create-a-model"></a>Tworzenie modelu

-   Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, a następnie wybierz pozycję **Add -&gt; nowy element**.
-   Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów.
-   Wprowadź **TPHModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**.
-   W oknie dialogowym Wybierz zawartość modelu, wybierz **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.
-   Kliknij przycisk **nowe połączenie**.
    W oknie dialogowym właściwości połączenia, wprowadź nazwę serwera (na przykład **(localdb)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz **School** nazwy bazy danych, a następnie Kliknij przycisk **OK**.
    Okno dialogowe Wybierz połączenie danych jest aktualizowana ustawienie połączenia bazy danych.
-   W oknie dialogowym Wybierz obiekty bazy danych w węźle tabel, wybierz **osoby** tabeli.
-   Kliknij przycisk **Zakończ**.

Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu. Wszystkie obiekty, które zostały wybrane w oknie dialogowym Wybierz obiekty bazy danych są dodawane do modelu.

Oznacza to sposób, w jaki **osoby** tabela wygląda w bazie danych.

![Tabela osoby](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>Implementowanie Tabela wg hierarchii dziedziczenia

**Osoby** tabela ma **dyskryminatora** kolumny, która może mieć jedną z dwóch wartości: "Student" i "Instruktora". W zależności od wartości **osoby** tabeli zostaną zmapowane do **uczniów** jednostki lub **przez instruktorów** jednostki. **Osoby** tabela ma także dwie kolumny **DataZatrudnienia** i **EnrollmentDate**, które muszą być **dopuszczającego wartość null** ponieważ osoba nie może być dla uczniów i pod kierunkiem instruktora w tym samym czasie, (a przynajmniej nie w tym przewodniku).

### <a name="add-new-entities"></a>Dodaj nowe jednostki

-   Dodaj nową jednostkę.
    Aby to zrobić, kliknij prawym przyciskiem myszy pusty obszar powierzchni projektu programu Entity Framework Designer, a następnie wybierz **Add -&gt;jednostki**.
-   Typ **przez instruktorów** dla **nazwa jednostki** i wybierz **osoby** z listy rozwijanej dla **typ podstawowy**.
-   Kliknij przycisk **OK**.
-   Dodaj inną nową jednostkę. Typ **uczniów** dla **nazwa jednostki** i wybierz **osoby** z listy rozwijanej dla **typ podstawowy**.

Dwóch nowych typów jednostki zostały dodane do powierzchni projektowej. Strzałka wskazuje z nowych typów jednostek do **osoby** typu jednostki; oznacza to, że **osoby** jest typem podstawowym dla nowych typów jednostek.

-   Kliknij prawym przyciskiem myszy **DataZatrudnienia** właściwość **osoby** jednostki. Wybierz **Wytnij** (lub użyj klawisza Ctrl-X).
-   Kliknij prawym przyciskiem myszy **przez instruktorów** jednostki, a następnie wybierz pozycję **Wklej** (lub użyj klawisza Ctrl-V).
-   Kliknij prawym przyciskiem myszy **DataZatrudnienia** właściwości i wybierz pozycję **właściwości**.
-   W **właściwości** oknie **Nullable** właściwości **false**.
-   Kliknij prawym przyciskiem myszy **EnrollmentDate** właściwość **osoby** jednostki. Wybierz **Wytnij** (lub użyj klawisza Ctrl-X).
-   Kliknij prawym przyciskiem myszy **uczniów** jednostki, a następnie wybierz pozycję **Wklej (lub użyj klawiszy Ctrl-V klucza).**
-   Wybierz **EnrollmentDate** właściwości i ustaw **Nullable** właściwości **false**.
-   Wybierz **osoby** typu jednostki. W **właściwości** oknie jego **abstrakcyjne** właściwości **true**.
-   Usuń **dyskryminatora** właściwość **osoby**. Powodów, dla którego należy usunąć jest szczegółowo opisane w poniższej sekcji.

### <a name="map-the-entities"></a>Mapuj jednostki

-   Kliknij prawym przyciskiem myszy **przez instruktorów** i wybierz **mapowania tabeli.**
    W oknie Szczegóły mapowania wybrano jednostki przez instruktorów.
-   Kliknij przycisk **&lt;Dodaj tabelę lub widok&gt;** w **szczegóły mapowania** okna.
    **&lt;Dodaj tabelę lub widok&gt;** pole staje się listy rozwijanej tabel lub widoków, które można mapować wybranej jednostki.
-   Wybierz **osoby** z listy rozwijanej.
-   **Szczegóły mapowania** okna jest aktualizowane przy użyciu domyślnego mapowania kolumn oraz opcję dodawania warunku.
-   Kliknij pozycję  **&lt;Dodaj warunek&gt;**.
    **&lt;Dodaj warunek&gt;** pole staje się listy rozwijanej, kolumn, dla których można ustawić warunki.
-   Wybierz **dyskryminatora** z listy rozwijanej.
-   W **Operator** kolumny **szczegóły mapowania** wybierz = z listy rozwijanej.
-   W **wartość/właściwości** wpisz **przez instruktorów**. Wynik końcowy powinien wyglądać następująco:

    ![Szczegóły mapowania](~/ef6/media/mappingdetails2.png)

-   Powtórz te kroki dla **uczniów** typu jednostki, ale Utwórz warunek równa **uczniów** wartość.  
    *Dlatego Chcieliśmy, aby usunąć **dyskryminatora** właściwość, jest, ponieważ nie można zamapować kolumny tabeli więcej niż jeden raz. Ta kolumna będzie służyć warunkowego mapowania, więc nie można używać właściwości mapowania także. Jedynym sposobem może służyć w obu przypadkach, gdy warunek będzie używać **Is Null** lub **Is Not Null** porównania.*

Tabela wg hierarchii dziedziczenia jest teraz implementowane.

![Końcowe TPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>Użyj modelu

Otwórz **Program.cs** pliku gdzie **Main** metoda jest zdefiniowana. Wklej następujący kod do **Main** funkcji. Kod wykonuje trzy zapytania. Pierwsze zapytanie powoduje zwrócenie wszystkich **osoby** obiektów. Drugie zapytanie używa **OfType** metodę, aby zwrócić **przez instruktorów** obiektów. Trzecie zapytanie używa **OfType** metodę, aby zwrócić **uczniów** obiektów.

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
