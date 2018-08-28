---
title: Projektanta CUD procedury składowane - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 7a3176e1057816dd11ced5fc545aa3baa672bd03
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993892"
---
# <a name="designer-cud-stored-procedures"></a>Procedury składowane CUD projektanta
Ten przewodnik krok po kroku pokazano, jak mapować tworzenia\\Wstawianie, aktualizowanie i usuwanie operacji (CUD) typu jednostki do procedur składowanych, za pomocą funkcji Entity Framework Designer (Projektant EF).  Domyślnie platforma Entity Framework automatycznie generuje instrukcje SQL dla operacje CUD, ale można również mapować procedur składowanych do tych operacji.  

Należy pamiętać, że Code First platformy nie obsługuje mapowania do procedury składowanej lub funkcji. Jednak można wywoływać procedury składowanej lub funkcji za pomocą metody System.Data.Entity.DbSet.SqlQuery. Na przykład:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>Uwagi dotyczące mapowania operacje CUD do procedur składowanych

Mapowanie operacje CUD do procedur składowanych, następujące kwestie: 

-   Jeśli mapowanie jeden operacje CUD do procedury składowanej, mapować wszystkie z nich. Jeśli nie należy mapować wszystkie trzy, niezamapowane operacji zakończy się niepowodzeniem, jeśli wykonywane i **UpdateException** zostanie zgłoszony.
-   Każdy parametr procedury składowanej musi być mapowane do właściwości jednostki.
-   Jeśli serwer generuje wartość klucza podstawowego dla wstawionego wiersza, możesz zamapować tę wartość do właściwości klucza jednostki. W poniższym przykładzie **InsertPerson** procedura składowana ma zwracać jako część zestawu wyników procedury składowanej nowo utworzony klucz podstawowy. Klucz podstawowy jest mapowany na klucz jednostki (**PersonID**) przy użyciu **&lt;Dodaj powiązań wyników&gt;** funkcji projektancie platformy EF.
-   Zapisane wywołania procedur są mapowane 1:1 jednostki w modelu koncepcyjnym. Na przykład w przypadku zaimplementowania Hierarchia dziedziczenia w modelu koncepcyjnym i następnie mapy CUD procedur składowanych dla **nadrzędnego** (podstawowy) i **podrzędnych** jednostki (pochodnego), zapisywanie **Podrzędnych** zmian będzie wywoływać tylko **podrzędnych**przez procedury składowane, nie spowodują uruchomienia **nadrzędnego**użytkownika przechowywane wywołań procedur.

## <a name="prerequisites"></a>Wymagania wstępne

W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:

- Najnowszą wersję programu Visual Studio.
- [Przykładowej bazy danych School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Konfigurowanie projektu

-   Otwórz program Visual Studio 2012.
-   Wybierz **plikach&gt; New -&gt; projektu**
-   W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu.
-   Wprowadź **CUDSProcsSample** jako nazwę.
-   Wybierz **OK**.

## <a name="create-a-model"></a>Tworzenie modelu

-   Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, a następnie wybierz pozycję **Add -&gt; nowy element**.
-   Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów.
-   Wprowadź **CUDSProcs.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**.
-   W oknie dialogowym Wybierz zawartość modelu, wybierz **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.
-   Kliknij przycisk **nowe połączenie**. W oknie dialogowym właściwości połączenia, wprowadź nazwę serwera (na przykład **(localdb)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz **School** nazwy bazy danych, a następnie Kliknij przycisk **OK**.
    Okno dialogowe Wybierz połączenie danych jest aktualizowana ustawienie połączenia bazy danych.
-   W polu Wybierz obiekty bazy danych okna dialogowego w polu **tabel** węzeł **osoby** tabeli.
-   Zaznacz również następujących procedur składowanych w obszarze **procedur przechowywanych i funkcji** węzła: **DeletePerson**, **InsertPerson**, i **UpdatePerson** . 
-   Począwszy od programu Visual Studio 2012, projektancie platformy EF obsługuje importu zbiorczego procedur składowanych. **Importowanie wybranych procedur przechowywanych i funkcji do modelu jednostki** jest zaznaczone domyślnie. Ponieważ w tym przykładzie firma Microsoft ma procedur składowanych, które Wstawianie, aktualizowanie i usuwanie typów jednostek, firma Microsoft nie chcesz zaimportować je, a następnie będzie Usuń zaznaczenie tego pola wyboru. 

    ![ImportSProcs](~/ef6/media/importsprocs.jpg)

-   Kliknij przycisk **Zakończ**.
    Zostanie wyświetlona projektancie platformy EF, oferujący powierzchnię projektową do edycji modelu.

## <a name="map-the-person-entity-to-stored-procedures"></a>Mapuj jednostki osoby do procedur składowanych

-   Kliknij prawym przyciskiem myszy **osoby** typu jednostki, a następnie wybierz **mapowanie procedur przechowywanych**.
-   Procedura składowana mapowania są wyświetlane w **szczegóły mapowania** okna.
-   Kliknij przycisk  **&lt;wybierz opcję Wstaw funkcja&gt;**.
    Pole staje się listy rozwijanej, procedur składowanych w modelu magazynu, które mogą być mapowane na typy jednostek w modelu koncepcyjnym.
    Wybierz **InsertPerson** z listy rozwijanej.
-   Domyślne mapowania między parametrów procedury składowanej i właściwości obiektu są wyświetlane. Należy pamiętać, że strzałki wskazują kierunek mapowania: wartości właściwości są dostarczane do parametrów procedury składowanej.
-   Kliknij przycisk  **&lt;Dodaj powiązanie wynik&gt;**.
-   Typ **NewPersonID**, nazwa parametru zwrócony przez **InsertPerson** procedury składowanej. Upewnij się, że nie wpisz spacji wiodących i końcowych.
-   Naciśnij klawisz **wprowadź**.
-   Domyślnie **NewPersonID** jest mapowany na klucz jednostki **PersonID**. Należy pamiętać, że strzałka wskazuje kierunek mapowania: wartość kolumny wynik jest dostarczany do właściwości.

    ![MappingDetails](~/ef6/media/mappingdetails.png)

-   Kliknij przycisk **&lt;wybierz funkcję Update&gt;** i wybierz **UpdatePerson** z wyświetlonej listy rozwijanej.
-   Domyślne mapowania między parametrów procedury składowanej i właściwości obiektu są wyświetlane.
-   Kliknij przycisk **&lt;funkcji wybierz pozycję Usuń&gt;** i wybierz **DeletePerson** z wyświetlonej listy rozwijanej.
-   Domyślne mapowania między parametrów procedury składowanej i właściwości obiektu są wyświetlane.

Wstawianie, aktualizowanie i usuwanie operacji **osoby** typu jednostki są teraz mapowane na procedury składowane.

Jeśli chcesz włączyć współbieżności sprawdzania podczas aktualizowania lub usuwania jednostki za pomocą procedur składowanych, użyj jednej z następujących opcji:

-   Użyj **dane wyjściowe** parametru, aby zwrócić liczbę wierszy z procedury składowanej i sprawdź, których to dotyczy **parametr na wiersze** pole wyboru obok nazwy parametru. Jeśli wartość zwracana wynosi zero, gdy operacja jest wywoływana, [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) zostanie zgłoszony.
-   Sprawdź **Użyj oryginalnej wartości** pole wyboru obok właściwości, którą chcesz używać sprawdzania współbieżności. Próba aktualizacji wartości właściwości, która pierwotnie została odczytana z bazy danych będzie używany podczas zapisywania danych z powrotem do bazy danych. Jeśli wartość jest niezgodna z wartością w bazie danych **OptimisticConcurrencyException** zostanie zgłoszony.

## <a name="use-the-model"></a>Użyj modelu

Otwórz **Program.cs** pliku gdzie **Main** metoda jest zdefiniowana. Dodaj następujący kod do funkcji Main.

Ten kod tworzy nową **osoby** obiektu, następnie aktualizuje obiekt, a następnie usuwa obiekt.         

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

-   Skompilować i uruchomić aplikację. Program generuje następujące dane wyjściowe *
    >[!NOTE]
> PersonID został wygenerowany automatycznie przez serwer, więc najprawdopodobniej zobaczysz liczbę różnych *

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Jeśli pracujesz z programu Visual Studio w wersji Ultimate, umożliwia Intellitrace za pomocą debugera zobacz instrukcje SQL, które są wykonywane.

![IntelliTrace](~/ef6/media/intellitrace.png)
