---
title: Projektant zapytań procedury składowane - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 04478ea1c8cd43a7ba4ee788e464992af3de7f64
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283904"
---
# <a name="designer-query-stored-procedures"></a>Procedury składowane projektanta zapytań
Ten przewodnik krok po kroku pokazano, jak używać Entity Framework Designer (Projektant EF) do zaimportowania procedur składowanych do modelu, a następnie wywołać importowanych procedur składowanych do pobierania wyników. 

Należy pamiętać, że Code First platformy nie obsługuje mapowania do procedury składowanej lub funkcji. Jednak można wywoływać procedury składowanej lub funkcji za pomocą metody System.Data.Entity.DbSet.SqlQuery. Na przykład:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>Wymagania wstępne

W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:

- Najnowszą wersję programu Visual Studio.
- [Przykładowej bazy danych School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Konfigurowanie projektu

-   Otwórz program Visual Studio 2012.
-   Wybierz **plikach&gt; New -&gt; projektu**
-   W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu.
-   Wprowadź **EFwithSProcsSample** jako nazwę.
-   Wybierz **OK**.

## <a name="create-a-model"></a>Tworzenie modelu

-   Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Add -&gt; nowy element**.
-   Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów.
-   Wprowadź **EFwithSProcsModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**.
-   W oknie dialogowym Wybierz zawartość modelu, wybierz **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.
-   Kliknij przycisk **nowe połączenie**.  
    W oknie dialogowym właściwości połączenia, wprowadź nazwę serwera (na przykład **(localdb)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz **School** nazwy bazy danych, a następnie Kliknij przycisk **OK**.  
    Okno dialogowe Wybierz połączenie danych jest aktualizowana ustawienie połączenia bazy danych.
-   W oknie dialogowym Wybierz obiekty bazy danych sprawdź **tabel** pole wyboru, aby wszystkie tabele.  
    Zaznacz również następujących procedur składowanych w obszarze **procedur przechowywanych i funkcji** węzła: **GetStudentGrades** i **GetDepartmentName**. 

    ![{1&gt;Importuj&lt;1}](~/ef6/media/import.jpg)

    *Począwszy od programu Visual Studio 2012, projektancie platformy EF obsługuje importu zbiorczego procedur składowanych. **Importowanie wybranych procedur przechowywanych i funkcji do modelu theentity** jest zaznaczone domyślnie.*
-   Kliknij przycisk **Zakończ**.

Domyślnie kształt wynik każdego importowanych procedura składowana lub funkcja, która zwraca więcej niż jedną kolumnę automatycznie stanie się nowy typ złożony. W tym przykładzie chcemy zamapować wyniki **GetStudentGrades** funkcja **StudentGrade** jednostki oraz wyniki **GetDepartmentName** do **Brak** (**Brak** jest wartością domyślną).

Importowanie funkcji zwracać typ jednostki kolumny zwracane przez odpowiednie procedury składowanej muszą dokładnie odpowiadać właściwości skalarne typu jednostki zwracanego. Importowanie funkcji może również zwracać kolekcji typów prostych, typów złożonych lub brak wartości.

-   Kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **przeglądarka modeli**.
-   W **przeglądarka modeli**, wybierz opcję **Importy funkcji**, a następnie kliknij dwukrotnie **GetStudentGrades** funkcji.
-   W oknie dialogowym Edytowanie importowanie funkcji, wybierz **jednostek** i wybierz polecenie **StudentGrade**.  
    ***Importowanie funkcji jest konfigurowalna** pole wyboru w górnej części **Importy funkcji** okno dialogowe umożliwia mapowanie konfigurowalna funkcji. Jeśli zaznaczysz to pole tylko konfigurowalna funkcje (zwracających tabelę), będą wyświetlane w **procedury składowanej / nazwa funkcji** listy rozwijanej. Jeśli to pole wyboru nie jest zaznaczone, tylko funkcje — konfigurowalna będą wyświetlane na liście.*

## <a name="use-the-model"></a>Użyj modelu

Otwórz **Program.cs** pliku gdzie **Main** metoda jest zdefiniowana. Dodaj następujący kod do funkcji Main.

Kod wywołuje dwie procedury składowane: **GetStudentGrades** (zwraca **StudentGrades** dla określonego *StudentId*) i **GetDepartmentName** (zwraca nazwę działu parametr wyjściowy).  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

Skompilować i uruchomić aplikację. Program generuje następujące wyniki:

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>Parametry wyjściowe
-----------------

Jeśli używane są parametry wyjściowe, ich wartości nie będzie dostępne, dopóki wyniki zostały całkowicie odczytane. Jest to spowodowane bazowego zachowanie obiekt DbDataReader, zobacz [podczas pobierania danych przy użyciu elementu DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) Aby uzyskać więcej informacji.
