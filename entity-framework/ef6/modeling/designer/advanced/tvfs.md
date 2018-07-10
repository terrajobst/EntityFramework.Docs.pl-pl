---
title: Funkcje zwracające tabelę funkcji (Tvf) - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
caps.latest.revision: 3
ms.openlocfilehash: 7d652725a2655b691b03aa3f43103753fe72ede7
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914314"
---
# <a name="table-valued-functions-tvfs"></a>Funkcje z wartościami przechowywanymi w tabeli (funkcji Tvf)
> [!NOTE]
> **EF5 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 5. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.

Przewodnik krok po kroku i wideo pokazuje, jak mapować zwracających tabelę (funkcji Tvf) za pomocą programu Entity Framework Designer. Ilustruje też sposób wywoływania funkcji TVF w wyniku zapytania LINQ.

Funkcji Tvf są obecnie obsługiwane tylko w bazie danych pierwszego przepływu pracy.

Obsługa funkcji TVF została wprowadzona w programie Entity Framework w wersji 5. Należy pamiętać, że aby korzystać z nowych funkcji, takich jak zwracających tabelę, wyliczeń i typów przestrzennych, należy wskazać .NET Framework 4.5. Program Visual Studio 2012 jest przeznaczony dla platformy .NET 4.5 domyślnie.

Funkcji Tvf są bardzo podobne do procedur składowanych z jedną kluczową różnicą: wynik funkcji TVF jest konfigurowalna. Oznacza to, że wyniki z funkcji TVF mogą być używane w zapytaniu LINQ, ale nie wyników procedury składowanej.

## <a name="watch-the-video"></a>Obejrzyj wideo

**Osoba prezentująca**: Julia Kornich

[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Aby ukończyć ten Instruktaż, musisz:

- Zainstaluj [bazę danych School](~/ef6/resources/school-database.md).

- Posiadania najnowszej wersji programu Visual Studio

## <a name="set-up-the-project"></a>Konfigurowanie projektu

1.  Otwórz program Visual Studio
2.  Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**
3.  W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu
4.  Wprowadź **TVF** jako nazwę projektu i kliknij przycisk **OK**

## <a name="add-a-tvf-to-the-database"></a>Dodawanie funkcji TVF w bazie danych

-   Wybierz **widok —&gt; Eksplorator obiektów SQL Server**
-   Jeśli LocalDB nie znajduje się lista serwerów: kliknij prawym przyciskiem myszy **programu SQL Server** i wybierz **dodawania serwera SQL** Użyj domyślnej **uwierzytelniania Windows** do łączenia się z serwerem LocalDB
-   Rozwiń węzeł LocalDB
-   W węźle bazy danych, kliknij prawym przyciskiem myszy węzeł bazy danych School, a następnie wybierz pozycję **nowe zapytanie...**
-   W edytorze języka T-SQL, Wklej poniższą definicję funkcji TVF

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

-   Kliknij prawym przyciskiem myszy w edytorze języka T-SQL i wybierz pozycję **wykonania**
-   Funkcja GetStudentGradesForCourse zostanie dodany do bazy danych School

 

## <a name="create-a-model"></a>Tworzenie modelu

1.  Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wskaż opcję **Dodaj**, a następnie kliknij przycisk **nowy element**
2.  Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w **szablony** okienko
3.  Wprowadź **TVFModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**
4.  W oknie dialogowym Wybierz zawartość modelu, wybierz **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**
5.  Kliknij przycisk **nowe połączenie** Enter **(localdb)\\mssqllocaldb** w tekście nazwy serwera polu wprowadź **School** bazy danych programu nazwę kliknij **OK**
6.  W polu Wybierz obiekty bazy danych okna dialogowego w polu **tabel** węzeł **osoby**, **StudentGrade**, i **kurs** tabel
7.  Wybierz **GetStudentGradesForCourse** funkcja znajdujący się w folderze **procedur przechowywanych i funkcji** węzła należy pamiętać, że począwszy od programu Visual Studio 2012, Projektant obiektów umożliwia importowanie usługi batch Twoje procedur przechowywanych i funkcji
8.  Kliknij przycisk **Zakończ**
9.  Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu. Wszystkie obiekty, które wybrano w **wybierz obiekty bazy danych** okno dialogowe, są dodawane do modelu.
10. Domyślnie kształt wynik każdego importowanych procedura składowana lub funkcja automatycznie stanie się nowy typ złożony w modelu entity. Ale chcemy mapowania wyniki funkcji GetStudentGradesForCourse jednostki StudentGrade: kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **przeglądarka modeli** w przeglądarce modelu wybierz **Importy funkcja**, a następnie kliknij dwukrotnie **GetStudentGradesForCourse** funkcji w edytować funkcji Importuj okno dialogowe, zaznacz **jednostek** i wybierz polecenie **StudentGrade**

## <a name="persist-and-retrieve-data"></a>Utrwalanie i pobieranie danych

Otwórz plik, w których zdefiniowano metody Main. Dodaj następujący kod do funkcji Main.

Poniższy kod przedstawia sposób tworzenia zapytania, które korzysta z funkcji z wartościami przechowywanymi w tabeli. Zapytanie projektów wyniki na typ anonimowy, który zawiera powiązane tytuł kursu i powiązane uczniom i studentom klasy korporacyjnej, większa lub równa 3.5.

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

Skompilować i uruchomić aplikację. Program generuje następujące wyniki:

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>Podsumowanie

W tym przewodniku zobaczyliśmy, jak mapować zwracających tabelę (funkcji Tvf) za pomocą programu Entity Framework Designer. On również przedstawiono sposób wywoływania funkcji TVF w wyniku zapytania LINQ.
