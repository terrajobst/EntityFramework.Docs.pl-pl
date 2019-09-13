---
title: Dzielenie tabeli projektanta — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f5e7532e6c0b473d8ce77cbd11e3e673b0af6cbe
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921782"
---
# <a name="designer-table-splitting"></a>Dzielenie tabeli projektanta
W tym instruktażu pokazano, jak zmapować wiele typów jednostek do pojedynczej tabeli, modyfikując model przy użyciu Entity Framework Designer (Dr Designer).

Jednym z powodów użycia dzielenia tabeli jest opóźnienie ładowania niektórych właściwości w przypadku ładowania obiektów przy użyciu opóźnionych obciążeń. Możesz oddzielić właściwości, które mogą zawierać bardzo dużą ilość danych w osobnej jednostce i ładować ją tylko wtedy, gdy jest to wymagane.

Na poniższej ilustracji przedstawiono główne okna, które są używane podczas pracy z programem Dr Designer.

![Projektant EF](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Wymagania wstępne

W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:

- Najnowsza wersja programu Visual Studio.
- [Przykładowa baza danych szkoły](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Konfigurowanie projektu

W tym instruktażu jest używany program Visual Studio 2012.

-   Open Visual Studio 2012.
-   W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt**.
-   W lewym okienku kliknij pozycję Visual C\#, a następnie wybierz szablon Aplikacja konsolowa.
-   Wprowadź **TableSplittingSample** jako nazwę projektu, a następnie kliknij przycisk **OK**.

## <a name="create-a-model-based-on-the-school-database"></a>Tworzenie modelu opartego na bazie danych szkoły

-   Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element**.
-   Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.
-   W polu Nazwa pliku wprowadź **TableSplittingModel. edmx** , a następnie kliknij przycisk **Dodaj**.
-   W oknie dialogowym Wybierz zawartość modelu wybierz pozycję **Generuj z bazy danych**, a następnie kliknij przycisk **Dalej.**
-   Kliknij pozycję nowe połączenie. W oknie dialogowym właściwości połączenia wprowadź nazwę serwera (na przykład **(LocalDB)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz **szkołę** dla nazwy bazy danych, a następnie kliknij przycisk **OK**.
    Okno dialogowe Wybieranie połączenia danych zostanie zaktualizowane przy użyciu ustawienia połączenia z bazą danych.
-   W oknie dialogowym Wybierz obiekty bazy danych unfold węzeł **tabele** i sprawdź tabelę **Person** . Spowoduje to dodanie określonej tabeli do modelu **szkoły** .
-   Kliknij przycisk **Zakończ**.

Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu. Wszystkie obiekty wybrane w oknie dialogowym  **Wybierz obiekty bazy danych**zostaną dodane do modelu.

## <a name="map-two-entities-to-a-single-table"></a>Mapuj dwie jednostki na jedną tabelę

W tej sekcji podzielę jednostkę **osoby** na dwie jednostki, a następnie mapując je do pojedynczej tabeli.

> [!NOTE]
> Jednostka **osoby** nie zawiera żadnych właściwości, które mogą zawierać dużą ilość danych; jest on właśnie używany jako przykład.

-   Kliknij prawym przyciskiem myszy pusty obszar na powierzchni projektowej, wskaż polecenie **Dodaj nowe**, a następnie kliknij pozycję **Jednostka**.
    Zostanie wyświetlone okno dialogowe **Nowa jednostka** .
-   Wpisz **HireInfo** dla **nazwy obiektu** i **PersonID** dla nazwy **właściwości klucza** .
-   Kliknij przycisk **OK**.
-   Nowy typ jednostki jest tworzony i wyświetlany na powierzchni projektowej.
-   Wybierz właściwość **HireDate** typu jednostki **osoba** i naciśnij klawisze **Ctrl + X** .
-   Wybierz jednostkę **HireInfo** i naciśnij klawisze **Ctrl + V** .
-   Utwórz skojarzenie między **osobą** i **HireInfo**. W tym celu kliknij prawym przyciskiem myszy pusty obszar na powierzchni projektowej, wskaż polecenie **Dodaj nowe**, a następnie kliknij pozycję **skojarzenie**.
-   Zostanie wyświetlone okno dialogowe **Dodawanie skojarzenia** . Nazwa **PersonHireInfo** jest domyślnie określona.
-   Określ liczebność **1 (jeden)** na obu końcach relacji.
-   Naciśnij klawisz **OK**.

Następny krok wymaga okna **szczegóły** mapowania. Jeśli nie widzisz tego okna, kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **szczegóły mapowania**.

-   Wybierz typ jednostki **HireInfo** , a następnie kliknij pozycję **&lt;Dodaj tabelę lub&gt;widok** w oknie **szczegóły** mapowania.
-   Wybierz **osobę** z   **&lt;listy rozwijanej Dodaj tabelę lub&gt;widok**. Lista zawiera tabele lub widoki, do których można zamapować wybraną jednostkę.
    Odpowiednie właściwości powinny być domyślnie mapowane.

    ![Mapowanie](~/ef6/media/mapping.png)

-   Wybierz skojarzenie **PersonHireInfo** na powierzchni projektowej.
-   Kliknij prawym przyciskiem myszy skojarzenie na powierzchni projektowej i wybierz polecenie **Właściwości**.
-   W oknie **Właściwości** wybierz właściwość **ograniczenia referencyjne** i kliknij przycisk wielokropka.
-   Wybierz **osobę** z listy rozwijanej **główna** .
-   Naciśnij klawisz **OK**.

 

## <a name="use-the-model"></a>Korzystanie z modelu

-   Wklej następujący kod w metodzie Main.

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
-   Skompiluj i uruchom aplikację.

Następujące instrukcje języka T-SQL zostały wykonane względem bazy danych **szkoły** w wyniku uruchomienia tej aplikacji. 

-   Następujący element **INSERT** został wykonany w wyniku wykonywania kontekstu. Metody SaveChanges () i łączy dane od **osoby** i jednostek **HireInfo**

    ![Insert](~/ef6/media/insert.png)

-   Następujący **wybór** został wykonany w wyniku wykonywania kontekstu. **Persons** . FirstOrDefault () i wybiera tylko kolumny mapowane do osoby

    ![Wybierz 1](~/ef6/media/select1.png)

-   Następujący **wybór** został wykonany w wyniku uzyskania dostępu do właściwości nawigacji ExistingPerson. instruktor i wybiera tylko kolumny mapowane na **HireInfo**

    ![Wybierz 2](~/ef6/media/select2.png)
