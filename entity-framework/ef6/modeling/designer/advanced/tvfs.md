---
title: Funkcje o wartościach tabelowych (TVFs) — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 35684196dcd7b708a8feeb1eca3096e8d4e555ec
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182537"
---
# <a name="table-valued-functions-tvfs"></a>Funkcje z wartościami przechowywanymi w tabeli (TVFs)
> [!NOTE]
> **EF5 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 5. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.

Film wideo i przewodnik krok po kroku przedstawiają sposób mapowania funkcji z wartościami przechowywanymi w tabeli (TVFs) przy użyciu Entity Framework Designer. Przedstawiono w nim również sposób wywoływania TVF z zapytania LINQ.

TVFs są obecnie obsługiwane tylko w przepływie pracy Database First.

Obsługa TVF została wprowadzona w Entity Framework w wersji 5. Należy pamiętać, że aby korzystać z nowych funkcji, takich jak funkcje z wartościami przechowywanymi w tabeli, wyliczenia i typy przestrzenne, należy określić wartość docelową .NET Framework 4,5. Program Visual Studio 2012 Domyślnie kieruje .NET 4,5.

TVFs są bardzo podobne do procedur składowanych z jedną różnicą kluczową: wynik TVF można utworzyć. Oznacza to, że wyniki z TVF można użyć w zapytaniu LINQ, podczas gdy nie można wykonać wyników procedury składowanej.

## <a name="watch-the-video"></a>Obejrzyj wideo

**Przedstawione przez**: Julia Kornich

[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik, musisz wykonać następujące czynności:

- Zainstaluj [szkołę bazy danych](~/ef6/resources/school-database.md).

- Mieć najnowszą wersję programu Visual Studio

## <a name="set-up-the-project"></a>Konfigurowanie projektu

1.  Otwórz program Visual Studio
2.  W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt** .
3.  W lewym okienku kliknij pozycję **Visual C @ no__t-1**, a następnie wybierz szablon **konsoli**
4.  Wprowadź **TVF** jako nazwę projektu, a następnie kliknij przycisk **OK** .

## <a name="add-a-tvf-to-the-database"></a>Dodawanie TVF do bazy danych

-   Wybierz pozycję **Widok-&gt; Eksplorator obiektów SQL Server**
-   Jeśli LocalDB nie znajduje się na liście serwerów: Kliknij prawym przyciskiem myszy **SQL Server** i wybierz polecenie **Dodaj SQL Server** Użyj domyślnego **uwierzytelniania systemu Windows** do nawiązania połączenia z serwerem LocalDB
-   Rozwiń węzeł LocalDB
-   W węźle bazy danych kliknij prawym przyciskiem myszy węzeł baza danych szkoły i wybierz polecenie **nowe zapytanie...**
-   W edytorze T-SQL wklej następującą definicję TVF

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   Kliknij prawym przyciskiem myszy w edytorze T-SQL i wybierz polecenie **Execute (wykonaj** ).
-   Funkcja GetStudentGradesForCourse jest dodawana do bazy danych szkoły

 

## <a name="create-a-model"></a>Tworzenie modelu

1.  Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element** .
2.  Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku **Szablony** .
3.  W polu Nazwa pliku wprowadź **TVFModel. edmx** , a następnie kliknij przycisk **Dodaj** .
4.  W oknie dialogowym Wybierz zawartość modelu wybierz pozycję **Generuj z bazy danych**, a następnie kliknij przycisk **dalej** .
5.  Kliknij pozycję **nowe połączenie** wprowadź **(LocalDB) \\mssqllocaldb** w polu tekstowym nazwa serwera wprowadź **Szkoła** For Nazwa bazy danych kliknij przycisk **OK** .
6.  W oknie dialogowym Wybierz obiekty bazy danych w obszarze **tabele** Node wybierz **osobę**, **StudentGrade**i **kurs**@no__t — 5tables
7.  Wybierz funkcję **GetStudentGradesForCourse** znajdującą się w obszarze **procedury składowane i funkcje** node Uwaga, która rozpoczyna się od programu Visual Studio 2012, Entity Designer umożliwia zbiorcze Importowanie procedur i funkcji przechowywanych
8.  Kliknij przycisk **Zakończ** .
9.  Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu. Wszystkie obiekty wybrane w polu **Wybierz obiekty bazy danych** dialog są dodawane do modelu.
10. Domyślnie kształt wynik każdej zaimportowanej procedury lub funkcji składowanej automatycznie stanie się nowym typem złożonym w modelu jednostki. Jednak chcemy zmapować wyniki funkcji GetStudentGradesForCourse na jednostkę StudentGrade: Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz pozycję **przeglądarka modelu** w przeglądarce modelu, wybierz opcję **Importy funkcji**, a następnie kliknij dwukrotnie funkcję **GetStudentGradesForCourse** w oknie dialogowym Edytowanie importu funkcji, wybierz pozycję **jednostki** .  and wybierz **StudentGrade**

## <a name="persist-and-retrieve-data"></a>Utrwalanie i pobieranie danych

Otwórz plik, w którym jest zdefiniowana Metoda Main. Dodaj następujący kod do funkcji main.

Poniższy kod ilustruje sposób tworzenia zapytania korzystającego z funkcji zwracającej tabelę. Zapytanie projektuje wyniki do typu anonimowego, który zawiera powiązane z nim tytuł kursu i pokrewnych uczniów o klasie wyższej lub równej 3,5.

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

Skompiluj i uruchom aplikację. Program tworzy następujące dane wyjściowe:

```console
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>Podsumowanie

W tym instruktażu przedstawiono sposób mapowania funkcji zwracającej tabelę (TVFs) przy użyciu Entity Framework Designer. Przedstawiono w nim również sposób wywoływania TVF z zapytania LINQ.
