---
title: Dziedziczenie projektanta TPT - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 68980fa89446940b8b7f5f73c519d38e727a9039
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996351"
---
# <a name="designer-tpt-inheritance"></a>Dziedziczenie TPT projektanta
Ten przewodnik krok po kroku pokazano, jak zaimplementować Tabela wg typu (TPT) dziedziczenia w modelu przy użyciu narzędzia Entity Framework Designer (Projektant EF). Tabela wg typu dziedziczenia używa osobnej tabeli w bazie danych do przechowywania danych dla właściwości dziedziczone i właściwości klucza dla każdego typu w hierarchii dziedziczenia.

W tym przewodniku, firma Microsoft będzie zmapowana **kurs** (typ podstawowy) **OnlineCourse** (pochodzi z kursu) i **OnsiteCourse** (pochodzi od klasy **kurs**) jednostki do tabel z takich samych nazwach. Utworzymy model z bazy danych i następnie zmienić model, który ma implementują dziedziczenie TPT.

Można również uruchomić z pierwszego modelu i następnie wygenerować bazę danych z modelu. Projektancie platformy EF używa strategii TPT domyślnie, a więc wszystkie dziedziczenia w modelu będą mapowane do oddzielnych tabel.

## <a name="other-inheritance-options"></a>Inne opcje dziedziczenia

Tabela wg hierarchii (TPH) jest inny typ dziedziczenia, w której jedna baza danych tabela jest używana do przechowywania danych dla wszystkich typów jednostek w hierarchii dziedziczenia.  Aby uzyskać informacje o mapowaniu Tabela wg hierarchii dziedziczenia z Projektanta obiektów, zobacz [dziedziczenia TPH projektancie platformy EF](~/ef6/modeling/designer/inheritance/tph.md). 

Należy pamiętać, że tabeli na na konkretny typ dziedziczenia (TPC) i dziedziczenie mieszane modeli są obsługiwane przez środowisko uruchomieniowe programu Entity Framework, ale nie są obsługiwane w Projektancie platformy EF. Jeśli chcesz użyć TPC lub mieszane dziedziczenia, masz dwie opcje: Użyj Code First lub ręczna Edycja pliku EDMX. Jeśli użytkownik chce pracować z pliku EDMX, okno Szczegóły mapowania, które zostaną wprowadzone w "trybie awaryjnym" i nie można zmienić mapowania za pomocą projektanta.

## <a name="prerequisites"></a>Wymagania wstępne

W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:

- Najnowszą wersję programu Visual Studio.
- [Przykładowej bazy danych School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Konfigurowanie projektu

-   Otwórz program Visual Studio 2012.
-   Wybierz **plikach&gt; New -&gt; projektu**
-   W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu.
-   Wprowadź **TPTDBFirstSample** jako nazwę.
-   Wybierz **OK**.

## <a name="create-a-model"></a>Tworzenie modelu

-   Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, a następnie wybierz pozycję **Add -&gt; nowy element**.
-   Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów.
-   Wprowadź **TPTModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**.
-   W oknie dialogowym Wybierz zawartość modelu, wybierz pozycję ** Generuj z bazy danych, a następnie kliknij przycisk **dalej**.
-   Kliknij przycisk **nowe połączenie**.
    W oknie dialogowym właściwości połączenia, wprowadź nazwę serwera (na przykład **(localdb)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz **School** nazwy bazy danych, a następnie Kliknij przycisk **OK**.
    Okno dialogowe Wybierz połączenie danych jest aktualizowana ustawienie połączenia bazy danych.
-   W oknie dialogowym Wybierz obiekty bazy danych w węźle tabel, wybierz **działu**, **kursu, OnlineCourse i OnsiteCourse** tabel.
-   Kliknij przycisk **Zakończ**.

Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu. Wszystkie obiekty, które zostały wybrane w oknie dialogowym Wybierz obiekty bazy danych są dodawane do modelu.

## <a name="implement-table-per-type-inheritance"></a>Implementowanie dziedziczenia tabelę według typu

-   Na powierzchni projektowej kliknij prawym przyciskiem myszy **OnlineCourse** typu jednostki, a następnie wybierz **właściwości**.
-   W **właściwości** okna, ustaw właściwość typu Base **kurs**.
-   Kliknij prawym przyciskiem myszy **OnsiteCourse** typu jednostki, a następnie wybierz **właściwości**.
-   W **właściwości** okna, ustaw właściwość typu Base **kurs**.
-   Kliknij prawym przyciskiem myszy skojarzenia (wiersz) między **OnlineCourse** i **kurs** typów jednostek.
    Wybierz **usunięte z modelu**.
-   Kliknij prawym przyciskiem myszy skojarzenie między **OnsiteCourse** i **kurs** typów jednostek.
    Wybierz **usunięte z modelu**.

Firma Microsoft usunie teraz **CourseID** właściwość **OnlineCourse** i **OnsiteCourse** ponieważ te klasy dziedziczą **CourseID** z **kurs** typ podstawowy.

-   Kliknij prawym przyciskiem myszy **CourseID** właściwość **OnlineCourse** typu jednostki, a następnie wybierz **usunięte z modelu**.
-   Kliknij prawym przyciskiem myszy **CourseID** właściwość **OnsiteCourse** typu jednostki, a następnie wybierz **usunięte z modelu**
-   Tabela wg typu dziedziczenia jest teraz implementowane.

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a>Użyj modelu

Otwórz **Program.cs** pliku gdzie **Main** metoda jest zdefiniowana. Wklej następujący kod do **Main** funkcji. Kod wykonuje trzy zapytania. Pierwsze zapytanie powoduje zwrócenie wszystkich **kursów** związane z określonej dział. Drugie zapytanie używa **OfType** metodę, aby zwrócić **OnlineCourses** związane z określonej dział. Zwraca trzecie zapytanie **OnsiteCourses**.

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
