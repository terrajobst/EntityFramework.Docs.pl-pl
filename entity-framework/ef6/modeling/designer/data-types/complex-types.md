---
title: Typy złożone - projektancie platformy EF - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: 2a516bd14131fd035a4d005e0fdf140f7ff4d65f
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250832"
---
# <a name="complex-types---ef-designer"></a>Typy złożone - projektancie platformy EF
W tym temacie pokazano, jak do mapowania typów złożonych z Entity Framework Designer (Projektant EF) oraz sposób wykonywania zapytań w przypadku jednostek, które zawierają właściwości typu złożonego.

Na poniższej ilustracji przedstawiono główne systemu windows, które są używane podczas pracy z projektancie platformy EF.

![Projektancie platformy EF](~/ef6/media/efdesigner.png)

> [!NOTE]
> Podczas tworzenia modelu koncepcyjnego ostrzeżenia dotyczące podmiotów niezmapowanych i skojarzenia może pojawić się na liście błędów. Można zignorować te ostrzeżenia, ponieważ po dokonaniu wyboru wygenerować bazę danych z modelu, błędy znikną.

## <a name="what-is-a-complex-type"></a>Co to jest typem złożonym

Typy złożone są właściwościami nieskalarnego typów jednostek, które umożliwiają właściwości skalarne, które mają być organizowane w ramach jednostek. Podobnie jak jednostki typy złożone składają się z właściwości skalarne właściwości lub innego typu złożonego.

Podczas pracy z obiektami, które reprezentują typy złożone, należy pamiętać o następujących czynności:

-   Typy złożone nie ma kluczy i dlatego nie mogą istnieć niezależnie. Typy złożone może istnieć tylko jako właściwości typów jednostek lub inne typy złożone.
-   Typy złożone nie mogą uczestniczyć w skojarzenia i nie może zawierać właściwości nawigacji.
-   Właściwości typu złożonego nie mogą być **null**. ** InvalidOperationException ** występuje, gdy **DbContext.SaveChanges** nosi nazwę i obiekt złożony null zostanie osiągnięty. Właściwości skalarne obiektów złożonych może być **null**.
-   Typy złożone nie może dziedziczyć z innych typów złożonych.
-   Należy zdefiniować typ złożony jako **klasy**. 
-   EF wykrywa zmiany do elementów członkowskich w obiekcie typu złożonego przy **DbContext.DetectChanges** jest wywoływana. Wywołuje platformy Entity Framework **metody DetectChanges** automatycznie, kiedy są wywoływane następujące składowe: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a>Refaktoryzacji właściwości jednostki na nowy typ złożony

Jeśli masz już jednostki w modelu koncepcyjnego może zajść potrzeba refaktoryzacji niektórych właściwości na właściwość typu złożonego.

Na powierzchni projektowej, wybierz jeden lub więcej właściwości (z wyjątkiem właściwości nawigacji) jednostki, następnie kliknij prawym przyciskiem myszy i wybierz **Refaktoryzuj -&gt; przenieść do nowego typu złożonego**.

![Refaktoryzacja](~/ef6/media/refactor.png)

Nowy typ złożony z wybranymi właściwościami zostanie dodany do **przeglądarka modeli**. Typ złożony jest nadawana nazwa domyślna.

Właściwości złożonej typu nowo utworzony zastępuje wybranych właściwości. Wszystkie mapowania właściwości są zachowywane.

![Refaktoryzuj 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a>Utwórz nowy typ złożony

Można również utworzyć nowy typ złożony, który nie zawiera właściwości istniejącej jednostki.

Kliknij prawym przyciskiem myszy **typy złożone** folderu w przeglądarce modelu wskaż **typu złożonego działają funkcje AddNew...** . Alternatywnie, można wybrać **typy złożone** folder, a następnie naciśnij klawisz **Wstaw** kluczowe na klawiaturze.

![Dodaj typ złożony nowe](~/ef6/media/addnewcomplextype.png)

Nowy typ złożony jest dodawany do folderu o domyślnej nazwie. Teraz można dodać właściwości do typu.

## <a name="add-properties-to-a-complex-type"></a>Dodaj właściwości do typu złożonego

Typy skalarne lub istniejące typy złożone, może być właściwości typu złożonego. Jednak właściwości typu złożonego nie może mieć odwołania cykliczne. Na przykład typ złożony **OnsiteCourseDetails** nie może mieć właściwość typu złożonego **OnsiteCourseDetails**.

Można dodać właściwość, typ złożony sposoby wymienione poniżej.

-   Kliknij prawym przyciskiem myszy typ złożony w przeglądarce modelu, wskaż opcję **Dodaj**, następnie wskaż **właściwość skalarną** lub **właściwość typu złożonego**, następnie wybierz typ żądanej właściwości. Alternatywnie można wybrać typ złożony, a następnie naciśnij klawisz **Wstaw** kluczowe na klawiaturze.  

    ![Dodaj właściwości do typu złożonego](~/ef6/media/addpropertiestocomplextype.png)

    Nowa właściwość jest dodawany do typu złożonego o domyślnej nazwie.

- LUB —

-   Kliknij prawym przyciskiem myszy właściwość jednostek **projektancie platformy EF** powierzchni i wybierz **kopiowania**, kliknij prawym przyciskiem myszy typ złożony, w **przeglądarka modeli** i wybierz  **Wklej**.

## <a name="rename-a-complex-type"></a>Zmień nazwę typu złożonego

Po zmianie nazwy typu złożonego, wszystkie odwołania do typu są aktualizowane w całym projekcie.

-   Wolno kliknij dwukrotnie typu złożonego w **przeglądarka modeli**.
    Nazwa zostanie wybrana i w trybie edycji.

- LUB —

-   Kliknij prawym przyciskiem myszy typ złożony, w **przeglądarka modeli** i wybierz **Zmień nazwę**.

- LUB —

-   Wybierz typ złożony w przeglądarce modelu, a następnie naciśnij klawisz F2.

- LUB —

-   Kliknij prawym przyciskiem myszy typ złożony, w **przeglądarka modeli** i wybierz **właściwości**. Edytuj nazwę w **właściwości** okna.

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a>Dodawanie istniejącego typu złożonego do jednostki i zamapuj jego właściwości do kolumn tabeli

1.  Kliknij prawym przyciskiem myszy jednostkę, wskaż opcję **Dodaj nowe**i wybierz **właściwość typu złożonego**.
    Właściwość typu złożonego o domyślnej nazwie zostanie dodany do jednostki. Domyślny typ (masz wybrane z istniejących typów złożonych) jest przypisywane do właściwości.
2.  Przypisz żądany typ właściwości w **właściwości** okna.
    Po dodaniu właściwość typu złożonego do jednostki, jego właściwości musi być mapowane na kolumny w tabeli.
3.  Kliknij prawym przyciskiem myszy typ jednostki na powierzchni projektowej lub w **przeglądarka modeli** i wybierz **mapowania tabel**.
    Mapowania tabel są wyświetlane w **szczegóły mapowania** okna.
4.  Rozwiń **mapuje &lt;nazwy tabeli&gt;**  węzła.
    A **mapowania kolumn** węzeł jest dostępny.
5.  Rozwiń **mapowania kolumn** węzła.
    Zostanie wyświetlona lista wszystkich kolumn w tabeli. Domyślne właściwości (jeśli istnieje) który mapowania kolumn są wyświetlane w obszarze **wartość/właściwości** nagłówka.
6.  Wybierz kolumnę, którą chcesz zamapować, a następnie kliknij prawym przyciskiem myszy odpowiedni **wartość/właściwości** pola.
    Jest wyświetlana lista rozwijana lista właściwości skalarne.
7.  Wybierz odpowiednie właściwości.

    ![Mapowanie typu złożonego](~/ef6/media/mapcomplextype.png)

8.  Powtórz kroki 6 i 7 dla każdej kolumny.

>[!NOTE]
> Aby usunąć mapowanie kolumn, wybierz kolumnę, którą chcesz zamapować, a następnie kliknij przycisk **wartość/właściwości** pola. Następnie wybierz **Usuń** z listy rozwijanej.

## <a name="map-a-function-import-to-a-complex-type"></a>Importowanie funkcji mapowania typu złożonego

Importy funkcji są oparte na procedury składowane. Aby mapować importowanie funkcji typu złożonego, kolumny zwracane przez odpowiednie procedury składowanej musi odpowiadać właściwości typu złożonego w liczbie i musi mieć typy magazynu, które są zgodne z typami właściwości.

-   Kliknij dwukrotnie importowanych funkcja, która ma być mapowany do typu złożonego.

    ![Imports — funkcja](~/ef6/media/functionimports.png)

-   Wypełnij ustawienia dla nowego importowanie funkcji w następujący sposób:
    -   Określ procedury składowanej, dla którego tworzysz importowanie funkcji, w **Nazwa procedury składowanej** pola. To pole jest listy rozwijanej, który wyświetla wszystkie procedury przechowywane w modelu magazynu.
    -   Określ nazwę funkcji importu w **nazwy importowanie funkcji** pola.
    -   Wybierz **złożonych** jako zwracany typ, a następnie określ określonych zwracany typ złożony, wybierając odpowiedni typ z listy rozwijanej.

        ![Edytuj Import — funkcja](~/ef6/media/editfunctionimport.png)

-   Kliknij przycisk **OK**.
    W modelu koncepcyjnym jest tworzony wpis importu funkcji.

### <a name="customize-column-mapping-for-function-import"></a>Dostosuj kolumny mapowania dla importu — funkcja

-   Kliknij prawym przyciskiem myszy importowanie funkcji w przeglądarce modelu, a następnie wybierz pozycję **Importowanie mapowania funkcji**.
    **Szczegóły mapowania** oknie wyświetlane wraz z domyślnego mapowania do importu funkcji. Strzałki wskazują mapowania między wartości w kolumnie i wartości właściwości. Domyślnie nazwy kolumn są zakłada się, że taka sama jak nazwy właściwości typu złożonego. Domyślne nazwy kolumn są wyświetlane w kolorze szarym.
-   W razie potrzeby zmień nazwy kolumn do dopasowania nazwy kolumn, które są zwracane przez procedurę składowaną, która odnosi się do importu funkcji.

## <a name="delete-a-complex-type"></a>Usuń typ złożony

Jeśli usuniesz to typ złożony, typ zostanie usunięta z modelu koncepcyjnego i mapowania dla wszystkich wystąpień tego typu są usuwane. Odwołania do typu nie są aktualizowane. Na przykład, jeśli określona jednostka ma właściwość typu złożonego typu ComplexType1 i ComplexType1 zostanie usunięty z **przeglądarka modeli**, odpowiednie właściwości jednostki nie jest aktualizowana. Model nie zostanie przeprowadzona Weryfikacja, ponieważ zawiera jednostki, do której odwołuje się do usuniętego typu złożonego. Można zaktualizować lub usunąć odwołania do usuniętych typów złożonych przy użyciu narzędzia Projektant jednostki.

-   Kliknij prawym przyciskiem myszy typ złożony w przeglądarce modelu, a następnie wybierz pozycję **Usuń**.

- LUB —

-   Wybierz typ złożony w przeglądarce modelu i na klawiaturze naciśnij klawisz Delete.

## <a name="query-for-entities-containing-properties-of-complex-type"></a>Zapytań dotyczących jednostek zawierający właściwości typu złożonego

Poniższy kod przedstawia sposób wykonywania zapytania, które zwraca kolekcję jednostek typu obiektów, które zawierają właściwość typu złożonego.

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
