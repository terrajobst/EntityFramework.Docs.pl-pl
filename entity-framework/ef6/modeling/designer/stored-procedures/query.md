---
title: Procedury składowane zapytania projektanta — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 2e0092b526278597e8477d47eeb642598647bb91
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182482"
---
# <a name="designer-query-stored-procedures"></a>Procedury składowane zapytania projektanta
W tym przewodniku krok po kroku pokazano, jak używać Entity Framework Designer (programu EF Designer) do importowania procedur składowanych do modelu, a następnie wywoływać zaimportowane procedury składowane w celu pobrania wyników. 

Należy pamiętać, że Code First nie obsługuje mapowania na procedury składowane lub funkcje. Można jednak wywołać procedury składowane lub funkcje przy użyciu metody System. Data. Entity. Nieogólnymi. sqlQuery. Na przykład:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>Wymagania wstępne

W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:

- Najnowsza wersja programu Visual Studio.
- [Przykładowa baza danych szkoły](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Konfigurowanie projektu

-   Open Visual Studio 2012.
-   Wybierz pozycję **plik —&gt; nowy&gt; projekt**
-   W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **konsoli** .
-   Wprowadź **EFwithSProcsSample** jako nazwę.
-   Wybierz **przycisk OK**.

## <a name="create-a-model"></a>Tworzenie modelu

-   Kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań i wybierz polecenie **dodaj&gt; nowy element**.
-   Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.
-   W polu Nazwa pliku wprowadź **EFwithSProcsModel. edmx** , a następnie kliknij przycisk **Dodaj**.
-   W oknie dialogowym Wybierz zawartość modelu wybierz pozycję **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.
-   Kliknij pozycję **nowe połączenie**.  
    W oknie dialogowym właściwości połączenia wprowadź nazwę serwera (na przykład **(LocalDB)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz  **szkoły** dla nazwy bazy danych, a następnie kliknij przycisk **OK**.  
    Okno dialogowe Wybieranie połączenia danych zostanie zaktualizowane przy użyciu ustawienia połączenia z bazą danych.
-   W oknie dialogowym Wybierz obiekty bazy danych zaznacz pole wyboru **tabele** , aby zaznaczyć wszystkie tabele.  
    Należy również wybrać następujące procedury składowane w węźle **procedury składowane i funkcje** : **GetStudentGrades** i **getdepartmentname**. 

    ![Import](~/ef6/media/import.jpg)

    *Począwszy od programu Visual Studio 2012, program Dr Designer obsługuje zbiorcze Importowanie procedur składowanych. **Importowane wybrane procedury składowane i funkcje do modelu theentity** są domyślnie zaznaczone.*
-   Kliknij przycisk **Zakończ**.

Domyślnie kształt wynik każdej zaimportowanej procedury składowanej, która zwraca więcej niż jedną kolumnę, automatycznie stanie się nowym typem złożonym. W tym przykładzie chcemy zmapować wyniki funkcji **GetStudentGrades** na jednostkę **StudentGrade** i wyniki elementu **getdepartmentname** na **none** (**Brak** jest wartością domyślną).

W celu zaimportowania funkcji w celu zwrócenia typu jednostki kolumny zwracane przez odpowiednią procedurę składowaną muszą dokładnie pasować do właściwości skalarnych zwracanego typu jednostki. Import funkcji może również zwracać kolekcje typów prostych, typów złożonych lub bez wartości.

-   Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz pozycję **przeglądarka modeli**.
-   W oknie **przeglądarka modelu**wybierz pozycję **Importy funkcji**, a następnie kliknij dwukrotnie funkcję **GetStudentGrades** .
-   W oknie dialogowym Edytowanie importu funkcji wybierz pozycję **jednostki** a następnie wybierz pozycję **StudentGrade**.  
    ***Import funkcji to** pole wyboru w górnej części okna dialogowego **Importy funkcji** umożliwia zamapowanie na funkcje do redagowania. Jeśli to pole wyboru jest zaznaczone, na liście rozwijanej **procedura składowana/nazwa funkcji** będą wyświetlane tylko funkcje, które można tworzyć (funkcje z wartościami przechowywanymi w tabeli). Jeśli to pole nie zostanie zaznaczone, na liście zostaną wyświetlone tylko funkcje, których nie można tworzyć.*

## <a name="use-the-model"></a>Korzystanie z modelu

Otwórz plik **program.cs** , w którym jest zdefiniowana Metoda **Main** . Dodaj następujący kod do funkcji main.

Kod wywołuje dwie procedury składowane: **GetStudentGrades** (zwraca **StudentGrades** dla określonych *StudentID*) i **getdepartmentname** (zwraca nazwę działu w parametrze danych wyjściowych).  

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

Skompiluj i uruchom aplikację. Program tworzy następujące dane wyjściowe:

```console
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>Parametry wyjściowe
-----------------

Jeśli są używane parametry wyjściowe, ich wartości nie będą dostępne, dopóki wyniki nie zostaną całkowicie odczytane. Jest to spowodowane zachowaniem podstawowego elementu DbDataReader. zobacz [pobieranie danych za pomocą elementu DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) , aby uzyskać więcej szczegółów.
