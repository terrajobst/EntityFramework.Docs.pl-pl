---
title: CUD Designer — procedury składowane — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813560"
---
# <a name="designer-cud-stored-procedures"></a>Procedury składowane CUD projektanta

W tym przewodniku krok po kroku pokazano, jak zmapować operacje Create @ no__t-0insert, Update i Delete (CUD) typu jednostki na procedury składowane przy użyciu Entity Framework Designer (program EF Designer).  Domyślnie Entity Framework automatycznie generuje instrukcje SQL dla operacji CUD, ale można również mapować procedury składowane na te operacje.  

Należy pamiętać, że Code First nie obsługuje mapowania na procedury składowane lub funkcje. Można jednak wywołać procedury składowane lub funkcje przy użyciu metody System. Data. Entity. Nieogólnymi. sqlQuery. Na przykład:

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>Zagadnienia dotyczące mapowania operacji CUD na procedury składowane

Podczas mapowania operacji CUD na procedury składowane są stosowane następujące zagadnienia:

- W przypadku mapowania jednej z CUD operacji do procedury składowanej należy zamapować wszystkie z nich. Jeśli nie wszystkie trzy, niezamapowane operacje zakończą się niepowodzeniem, jeśli zostaną wykonane i zostanie zgłoszony wyjątek **updateexception** will.
- Należy zmapować każdy parametr procedury składowanej na właściwości obiektu.
- Jeśli serwer generuje wartość klucza podstawowego dla wstawionego wiersza, należy zmapować tę wartość z powrotem na właściwość klucza jednostki. W poniższym przykładzie procedura **InsertPerson** stored zwróci nowo utworzony klucz podstawowy w ramach zestawu wyników procedury składowanej. Klucz podstawowy jest mapowany na klucz jednostki (**PersonID**) przy użyciu **powiązań wyników &lt;Add @ no__t-3** feature programu EF Designer.
- Wywołania procedury składowanej są mapowane 1:1 z jednostkami w modelu koncepcyjnym. Na przykład w przypadku zaimplementowania hierarchii dziedziczenia w modelu koncepcyjnym, a następnie zamapowania procedur składowanych CUD dla jednostek **nadrzędnych** (podstawowych) i **podrzędnych** (pochodnych), zapisanie zmian **podrzędnych** spowoduje wywołanie **elementu podrzędnego**" s procedury składowane nie będą wyzwalać wywołań procedur przechowywanych **elementu nadrzędnego**.

## <a name="prerequisites"></a>Wymagania wstępne

W celu wykonania instrukcji w tym przewodniku potrzebne są następujące elementy:

- Najnowsza wersja programu Visual Studio.
- [Przykładowa baza danych szkoły](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Konfigurowanie projektu

- Open Visual Studio 2012.
- Wybierz **plik-&gt; nowy-&gt; projektu**
- W lewym okienku kliknij pozycję **Visual C @ no__t-1**, a następnie wybierz szablon **konsoli** .
- Wprowadź **CUDSProcsSample** AS nazwę.
- Wybierz **przycisk OK**.

## <a name="create-a-model"></a>Tworzenie modelu

- Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań i wybierz polecenie **dodaj &gt; nowy element**.
- Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.
- W polu Nazwa pliku wprowadź **CUDSProcs. edmx** , a następnie kliknij przycisk **Dodaj**.
- W oknie dialogowym Wybierz zawartość modelu wybierz pozycję **Generuj z bazy danych**, a następnie kliknij przycisk **dalej**.
- Kliknij pozycję **nowe połączenie**. W oknie dialogowym właściwości połączenia wprowadź nazwę serwera (na przykład **(LocalDB)\\mssqllocaldb**), wybierz metodę uwierzytelniania, wpisz **szkołę** dla nazwy bazy danych, a następnie kliknij przycisk **OK**.
    Okno dialogowe Wybieranie połączenia danych zostanie zaktualizowane przy użyciu ustawienia połączenia z bazą danych.
- W oknie dialogowym Wybierz obiekty bazy danych w obszarze **tabele** node wybierz tabelę **osoba** .
- Należy również wybrać następujące procedury składowane w węźle **procedury składowane i funkcje** : **DeletePerson**, **InsertPerson**i **UpdatePerson**.
- Począwszy od programu Visual Studio 2012, program Dr Designer obsługuje zbiorcze Importowanie procedur składowanych. **Importowane procedury składowane i funkcje do modelu jednostki** są domyślnie zaznaczone. Ponieważ w tym przykładzie mamy procedury składowane, które wstawiają, aktualizują i usuwają typy jednostek, nie chcemy ich zaimportować i usuń zaznaczenie tego pola wyboru.

    ![Importuj elementy S](~/ef6/media/importsprocs.jpg)

- Kliknij przycisk **Zakończ**.
    Zostanie wyświetlony Projektant EF, który zapewnia powierzchnię projektową do edycji modelu.

## <a name="map-the-person-entity-to-stored-procedures"></a>Mapuj jednostkę osoby na procedury składowane

- Kliknij prawym przyciskiem myszy **osobę**@no__t 1entity typ i wybierz pozycję **mapowanie procedury składowanej**.
- Mapowania procedury składowanej pojawiają się w **szczegółach mapowania** window.
- Kliknij **&lt;Select Wstaw funkcję @ no__t-2**.
    Pole stanie się listą rozwijaną procedur składowanych w modelu magazynu, które mogą być mapowane na typy jednostek w modelu koncepcyjnym.
    Wybierz pozycję **InsertPerson** from listę rozwijaną.
- Wyświetlane są domyślne mapowania między parametrami procedury składowanej a właściwościami jednostki. Należy zauważyć, że strzałki wskazują kierunek mapowania: Wartości właściwości są dostarczane do parametrów procedury składowanej.
- Kliknij **@no__t — 1Add wyniku powiązania @ no__t-2**.
- Wpisz **NewPersonID**, nazwę parametru zwróconego przez procedurę **InsertPerson** stored. Pamiętaj, aby nie wpisywać spacji wiodących lub końcowych.
- Naciśnij klawisz **Enter**.
- Domyślnie **NewPersonID** Is mapowany na klucz jednostki **PersonID**. Należy zauważyć, że strzałka wskazuje kierunek mapowania: Wartość kolumny wynik jest dostarczana do właściwości.

    ![Szczegóły mapowania](~/ef6/media/mappingdetails.png)

- Kliknij pozycję **&lt;Select Update Function @ no__t-2** And select **UpdatePerson** from listy rozwijanej.
- Wyświetlane są domyślne mapowania między parametrami procedury składowanej a właściwościami jednostki.
- Kliknij kolejno pozycje **&lt;Select Delete Function @ no__t-2** And select **DeletePerson** from listy rozwijanej.
- Wyświetlane są domyślne mapowania między parametrami procedury składowanej a właściwościami jednostki.

Operacje INSERT, Update i DELETE **osoby**@no__t — typ 1entity są teraz mapowane na procedury składowane.

Jeśli chcesz włączyć sprawdzanie współbieżności podczas aktualizowania lub usuwania jednostki za pomocą procedur składowanych, użyj jednej z następujących opcji:

- Użyj parametru **wyjściowego** , aby zwrócić liczbę odnośnych wierszy z procedury składowanej, a następnie sprawdź **parametr dotyczący wierszy** checkbox obok nazwy parametru. Jeśli zwracana wartość jest równa zero, gdy operacja jest wywoływana, zostanie zgłoszony  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will.
- Zaznacz pole wyboru **Użyj oryginalnej wartości** obok właściwości, która ma być używana do sprawdzania współbieżności. Po próbie aktualizacji wartość właściwości, która pierwotnie odczytana z bazy danych, zostanie użyta podczas zapisywania danych z powrotem do bazy danych. Jeśli wartość nie jest zgodna z wartością w bazie danych, zostanie zgłoszony **OptimisticConcurrencyException** will.

## <a name="use-the-model"></a>Korzystanie z modelu

Otwórz plik **program.cs** , w którym jest zdefiniowana Metoda **Main** . Dodaj następujący kod do funkcji main.

Kod tworzy nowy obiekt **osoby** , a następnie aktualizuje obiekt, a wreszcie usuwa obiekt.

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

- Skompiluj i uruchom aplikację. Program tworzy następujące dane wyjściowe *

> [!NOTE]
> PersonID jest generowana automatycznie przez serwer, więc najprawdopodobniej zobaczysz inną liczbę *

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Jeśli pracujesz z wersją Ultimate programu Visual Studio, możesz użyć IntelliTrace z debugerem, aby wyświetlić instrukcje SQL, które są wykonywane.

![IntelliTrace](~/ef6/media/intellitrace.png)
