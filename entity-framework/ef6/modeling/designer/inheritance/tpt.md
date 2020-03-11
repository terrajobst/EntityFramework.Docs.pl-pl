---
title: Projektant TPT — dziedziczenie — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418211"
---
# <a name="designer-tpt-inheritance"></a>TPT dziedziczenia projektanta
W tym przewodniku krok po kroku pokazano, jak zaimplementować dziedziczenie według typu (TPT) w modelu przy użyciu Entity Framework Designer (program EF Designer). Dziedziczenie na poziomie tabeli używa oddzielnej tabeli w bazie danych, aby zachować dane dla właściwości niedziedziczonych i właściwości klucza dla każdego typu w hierarchii dziedziczenia.

W tym instruktażu zamapujemy **kurs** (typ podstawowy), **OnlineCourse** (pochodzą z kursu) i **OnsiteCourse** (pochodzą od **kursów**) do tabel o tych samych nazwach. Utworzymy model z bazy danych, a następnie zmienimy model w celu zaimplementowania dziedziczenia TPT.

Możesz również rozpocząć od Model First, a następnie wygenerować bazę danych na podstawie modelu. Projektant EF domyślnie korzysta z strategii TPT, dlatego każde dziedziczenie w modelu zostanie zmapowane do oddzielnych tabel.

## <a name="other-inheritance-options"></a>Inne opcje dziedziczenia

Tabela na hierarchię (TPH) jest innym typem dziedziczenia, w którym jedna tabela bazy danych jest używana do przechowywania danych dla wszystkich typów jednostek w hierarchii dziedziczenia.  Aby uzyskać informacje o sposobach mapowania dziedziczenia na poziomie tabeli przy użyciu Entity Designer, zobacz [dziedziczenie w programie EF Designer TPH](~/ef6/modeling/designer/inheritance/tph.md). 

Należy zauważyć, że w środowisku uruchomieniowym Entity Framework są obsługiwane dziedziczenie typu tabeli dla konkretnych typów (TPC) i modele dziedziczenia mieszanego, ale nie są obsługiwane przez projektanta EF. Jeśli chcesz użyć programu TPC lub dziedziczenia mieszanego, masz dwie opcje: Użyj Code First lub ręcznie Edytuj plik EDMX. Jeśli zdecydujesz się pracować z plikiem EDMX, okno Szczegóły mapowania zostanie umieszczone w trybie bezpiecznym i nie będzie można użyć projektanta, aby zmienić mapowania.

## <a name="prerequisites"></a>Wymagania wstępne

W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:

- Najnowsza wersja programu Visual Studio.
- [Przykładowa baza danych szkoły](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Konfigurowanie projektu

-   Open Visual Studio 2012.
-   Wybierz pozycję **plik —&gt; nowy&gt; projekt**
-   W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **konsoli** .
-   Wprowadź **TPTDBFirstSample** jako nazwę.
-   Wybierz **przycisk OK**.

## <a name="create-a-model"></a>Tworzenie modelu

-   Kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań i wybierz polecenie **dodaj&gt; nowy element**.
-   Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.
-   W polu Nazwa pliku wprowadź **TPTModel. edmx** , a następnie kliknij przycisk **Dodaj**.
-   W oknie dialogowym Wybierz zawartość modelu wybierz pozycję ** Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.
-   Kliknij pozycję **nowe połączenie**.
    W oknie dialogowym właściwości połączenia wprowadź nazwę serwera (na przykład **(LocalDB)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz  **szkoły** dla nazwy bazy danych, a następnie kliknij przycisk **OK**.
    Okno dialogowe Wybieranie połączenia danych zostanie zaktualizowane przy użyciu ustawienia połączenia z bazą danych.
-   W oknie dialogowym Wybierz obiekty bazy danych w węźle tabele wybierz tabele **dział**, **kurs, OnlineCourse i OnsiteCourse** .
-   Kliknij przycisk **Zakończ**.

Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu. Wszystkie obiekty wybrane w oknie dialogowym Wybierz obiekty bazy danych zostaną dodane do modelu.

## <a name="implement-table-per-type-inheritance"></a>Implementowanie dziedziczenia na poziomie tabeli

-   Na powierzchni projektowej kliknij prawym przyciskiem myszy typ jednostki **OnlineCourse** i wybierz polecenie **Właściwości**.
-   W oknie **Właściwości** ustaw właściwość typ podstawowy na **kurs**.
-   Kliknij prawym przyciskiem myszy typ jednostki **OnsiteCourse** i wybierz polecenie **Właściwości**.
-   W oknie **Właściwości** ustaw właściwość typ podstawowy na **kurs**.
-   Kliknij prawym przyciskiem myszy skojarzenie (wiersz) między typami jednostek **OnlineCourse** i **kurs** .
    Wybierz pozycję **Usuń z modelu**.
-   Kliknij prawym przyciskiem myszy skojarzenie między typami jednostek **OnsiteCourse** i **kurs** .
    Wybierz pozycję **Usuń z modelu**.

Usuniemy teraz Właściwość **CourseID** z **OnlineCourse** i **OnsiteCourse** , ponieważ te klasy dziedziczą **CourseID** z typu podstawowego **kursu** .

-   Kliknij prawym przyciskiem myszy Właściwość **CourseID** typu jednostki **OnlineCourse** , a następnie wybierz pozycję **Usuń z modelu**.
-   Kliknij prawym przyciskiem myszy Właściwość **CourseID** typu jednostki **OnsiteCourse** , a następnie wybierz pozycję **Usuń z modelu** .
-   Dziedziczenie dla typu tabeli jest teraz zaimplementowane.

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a>Korzystanie z modelu

Otwórz plik **program.cs** , w którym jest zdefiniowana Metoda **Main** . Wklej następujący kod do funkcji **Main** . Kod wykonuje trzy zapytania. Pierwsze zapytanie powoduje przywrócenie wszystkich **kursów** związanych z określonym działem. Drugie zapytanie używa metody **OfType** do zwracania **OnlineCourses** związanych z określonym działem. Trzecie zapytanie zwraca **OnsiteCourses**.

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
