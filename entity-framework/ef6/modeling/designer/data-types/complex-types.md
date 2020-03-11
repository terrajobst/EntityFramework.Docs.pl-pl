---
title: Typy złożone — Dr Designer — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418652"
---
# <a name="complex-types---ef-designer"></a>Typy złożone — Projektant EF
W tym temacie przedstawiono sposób mapowania typów złożonych o Entity Framework Designer (program Dr Designer) i sposób wykonywania zapytań dotyczących jednostek, które zawierają właściwości typu złożonego.

Na poniższej ilustracji przedstawiono główne okna, które są używane podczas pracy z programem Dr Designer.

![Projektant EF](~/ef6/media/efdesigner.png)

> [!NOTE]
> Podczas budowania modelu koncepcyjnego w Lista błędów mogą pojawić się ostrzeżenia dotyczące niezamapowanych jednostek i skojarzeń. Te ostrzeżenia można zignorować, ponieważ po wybraniu opcji wygenerowania bazy danych z modelu te błędy zostaną odrzucone.

## <a name="what-is-a-complex-type"></a>Co to jest typ złożony

Typy złożone są nieskalarnymi właściwościami typów jednostek, które umożliwiają organizowanie właściwości skalarnych w obrębie jednostek. Podobnie jak jednostki, typy złożone składają się z właściwości skalarnych lub innych właściwości typu złożonego.

Podczas pracy z obiektami, które reprezentują typy złożone, należy pamiętać o następujących kwestiach:

-   Typy złożone nie mają kluczy i dlatego nie mogą występować niezależnie. Typy złożone mogą istnieć tylko jako właściwości typów jednostek lub innych typów złożonych.
-   Typy złożone nie mogą uczestniczyć w skojarzeniach i nie mogą zawierać właściwości nawigacji.
-   Właściwości typu złożonego nie mogą mieć **wartości null**. **InvalidOperationException **występuje, gdy zostanie wywołana **DbContext. metody SaveChanges** i zostanie napotkany obiekt złożony o wartości null. Właściwości skalarne obiektów złożonych mogą mieć **wartość null**.
-   Typy złożone nie mogą dziedziczyć z innych typów złożonych.
-   Typ złożony należy zdefiniować jako **klasę**. 
-   EF wykrywa zmiany elementów członkowskich w obiekcie typu złożonego, gdy wywoływana jest **DbContext. DetectChanges** . Entity Framework wywołania **DetectChanges** automatycznie po wywołaniu następujących elementów członkowskich: **nieogólnymi. Find**, **nieogólnymi. Local**, **nieogólnymi. Remove**, **Nieogólnymi. Add**, **nieogólnymi. Attach**, **DbContext. metody SaveChanges**, **DbContext. GetValidationErrors**, **DbContext. entry**, **DbChangeTracker. wpisy**.

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a>Refaktoryzacja właściwości jednostki do nowego typu złożonego

Jeśli masz już jednostkę w modelu koncepcyjnym, możesz chcieć refaktoryzacji niektórych właściwości do właściwości typu złożonego.

Na powierzchni projektanta zaznacz jedną lub więcej właściwości (z wyłączeniem właściwości nawigacji) jednostki, a następnie kliknij prawym przyciskiem myszy i wybierz pozycję **Refaktoryzacja —&gt; Przenieś do nowego typu złożonego**.

![Refaktoryzacja](~/ef6/media/refactor.png)

Nowy typ złożony z wybranymi właściwościami zostanie dodany do **przeglądarki modeli**. Typ złożony ma przydaną nazwę domyślną.

Właściwość złożona nowo utworzonego typu zastępuje wybrane właściwości. Wszystkie mapowania właściwości są zachowywane.

![Refaktoryzacja 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a>Utwórz nowy typ złożony

Można również utworzyć nowy typ złożony, który nie zawiera właściwości istniejącej jednostki.

Kliknij prawym przyciskiem myszy folder **typy złożone** w przeglądarce modelu, wskaż **typ złożony typu AddNew.** . Alternatywnie możesz wybrać folder **typy złożone** i nacisnąć klawisz **INSERT** na klawiaturze.

![Dodaj nowy typ złożony](~/ef6/media/addnewcomplextype.png)

Nowy typ złożony zostanie dodany do folderu z nazwą domyślną. Teraz możesz dodawać właściwości do typu.

## <a name="add-properties-to-a-complex-type"></a>Dodawanie właściwości do typu złożonego

Właściwości typu złożonego mogą być typami skalarnymi lub istniejącymi typami złożonymi. Jednak właściwości typu złożonego nie mogą mieć odwołań cyklicznych. Na przykład typ złożony **OnsiteCourseDetails** nie może mieć właściwości typu złożonego **OnsiteCourseDetails**.

Możesz dodać właściwość do typu złożonego na dowolnym z wymienionych poniżej sposobów.

-   W przeglądarce modelu kliknij prawym przyciskiem myszy typ złożony, wskaż polecenie **Dodaj**, a następnie wskaż **właściwość skalarną** lub **Właściwość złożoną**, a następnie wybierz żądany typ właściwości. Alternatywnie możesz wybrać typ złożony, a następnie nacisnąć klawisz **Insert** na klawiaturze.  

    ![Dodawanie właściwości do typu złożonego](~/ef6/media/addpropertiestocomplextype.png)

    Nowa właściwość zostanie dodana do typu złożonego z nazwą domyślną.

- Oraz

-   Kliknij prawym przyciskiem myszy Właściwość jednostki na powierzchni **projektanta EF** i wybierz polecenie **Kopiuj**, a następnie kliknij prawym przyciskiem myszy typ złożony w **przeglądarce modelu** i wybierz polecenie **Wklej**.

## <a name="rename-a-complex-type"></a>Zmień nazwę typu złożonego

W przypadku zmiany nazwy typu złożonego wszystkie odwołania do tego typu są aktualizowane w całym projekcie.

-   Powoli kliknij dwukrotnie typ złożony w **przeglądarce modeli**.
    Nazwa zostanie wybrana i w trybie edycji.

- Oraz

-   Kliknij prawym przyciskiem myszy typ złożony w **przeglądarce modelu** i wybierz polecenie **Zmień nazwę**.

- Oraz

-   Wybierz typ złożony w przeglądarce modelu i naciśnij klawisz F2.

- Oraz

-   Kliknij prawym przyciskiem myszy typ złożony w **przeglądarce modelu** i wybierz polecenie **Właściwości**. Edytuj nazwę w oknie **właściwości** .

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a>Dodaj istniejący typ złożony do jednostki i zamapuj jego właściwości na kolumny tabeli

1.  Kliknij prawym przyciskiem myszy jednostkę, wskaż polecenie **Dodaj nowy**i wybierz polecenie **złożona Właściwość**.
    Do jednostki zostanie dodana właściwość typu złożonego o nazwie domyślnej. Domyślny typ (wybrany z istniejących typów złożonych) jest przypisany do właściwości.
2.  Przypisz żądany typ do właściwości w oknie **właściwości** .
    Po dodaniu właściwości typu złożonego do jednostki należy zamapować jej właściwości na kolumny tabeli.
3.  Kliknij prawym przyciskiem myszy typ jednostki na powierzchni projektowej lub w **przeglądarce modelu** a następnie wybierz pozycję **mapowania tabeli**.
    Mapowania tabeli są wyświetlane w oknie **szczegóły mapowania** .
4.  Rozwiń węzeł **mapy, aby &lt;nazwę tabeli&gt;** węźle .
    Zostanie wyświetlony węzeł **mapowania kolumn** .
5.  Rozwiń węzeł **mapowania kolumn** .
    Zostanie wyświetlona lista wszystkich kolumn w tabeli. Domyślne właściwości (jeśli istnieją), do których mapy kolumn są wyświetlane w nagłówku **Value/Property** .
6.  Wybierz kolumnę, którą chcesz zmapować, a następnie kliknij prawym przyciskiem myszy odpowiednią **wartość/właściwość** pole.
    Zostanie wyświetlona lista rozwijana wszystkich właściwości skalarnych.
7.  Wybierz odpowiednią właściwość.

    ![Mapowanie typu złożonego](~/ef6/media/mapcomplextype.png)

8.  Powtórz kroki 6 i 7 dla każdej kolumny tabeli.

>[!NOTE]
> Aby usunąć mapowanie kolumny, wybierz kolumnę, którą chcesz zmapować, a następnie kliknij pole **wartość/właściwość** . Następnie wybierz pozycję **Usuń** z listy rozwijanej.

## <a name="map-a-function-import-to-a-complex-type"></a>Mapowanie funkcji importowania do typu złożonego

Importy funkcji są oparte na procedurach składowanych. Aby zmapować funkcję importu do typu złożonego, kolumny zwracane przez odpowiednią procedurę składowaną muszą być zgodne z właściwościami typu złożonego w liczbie i muszą mieć typy magazynów zgodne z typami właściwości.

-   Kliknij dwukrotnie zaimportowaną funkcję, która ma zostać zmapowana do typu złożonego.

    ![Importy funkcji](~/ef6/media/functionimports.png)

-   Wypełnij ustawienia dla nowego importu funkcji w następujący sposób:
    -   Określ procedurę składowaną, dla której tworzysz import funkcji w polu **nazwa procedury składowanej** . To pole jest listą rozwijaną, która wyświetla wszystkie procedury składowane w modelu magazynu.
    -   Określ nazwę importu funkcji w polu  **Nazwa importu funkcji** .
    -   Wybierz opcję **złożone** jako zwracany typ, a następnie określ konkretny typ zwracany złożone, wybierając odpowiedni typ z listy rozwijanej.

        ![Edytuj import funkcji](~/ef6/media/editfunctionimport.png)

-   Kliknij przycisk **OK**.
    Wpis importowania funkcji jest tworzony w modelu koncepcyjnym.

### <a name="customize-column-mapping-for-function-import"></a>Dostosuj Mapowanie kolumn dla importu funkcji

-   Kliknij prawym przyciskiem myszy funkcję importowania w przeglądarce modelu i wybierz pozycję **Mapowanie importu funkcji**.
    Zostanie wyświetlone okno **szczegóły mapowania** i wyświetlane jest mapowanie domyślne dla importu funkcji. Strzałki oznaczają mapowania między wartościami kolumn i wartościami właściwości. Domyślnie nazwy kolumn są takie same jak nazwy właściwości typu złożonego. Domyślne nazwy kolumn są wyświetlane w kolorze szarym.
-   W razie potrzeby zmień nazwy kolumn tak, aby były zgodne z nazwami kolumn zwracanymi przez procedurę składowaną, która odnosi się do importowania funkcji.

## <a name="delete-a-complex-type"></a>Usuń typ złożony

Po usunięciu typu złożonego typ jest usuwany z modelu koncepcyjnego i mapowania dla wszystkich wystąpień tego typu są usuwane. Odwołania do tego typu nie są jednak aktualizowane. Na przykład jeśli jednostka ma właściwość typu złożonego typu ComplexType1, a ComplexType1 jest usuwana w **przeglądarce modelu**, odpowiednia właściwość jednostki nie jest aktualizowana. Model nie zostanie sprawdzony, ponieważ zawiera jednostkę, która odwołuje się do usuniętego typu złożonego. Można zaktualizować lub usunąć odwołania do usuniętych typów złożonych przy użyciu Entity Designer.

-   Kliknij prawym przyciskiem myszy typ złożony w przeglądarce modelu i wybierz polecenie **Usuń**.

- Oraz

-   W przeglądarce modeli wybierz typ złożony i naciśnij klawisz DELETE na klawiaturze.

## <a name="query-for-entities-containing-properties-of-complex-type"></a>Zapytanie dotyczące jednostek zawierających właściwości typu złożonego

Poniższy kod pokazuje, jak wykonać zapytanie, które zwraca kolekcję obiektów typu jednostki, które zawierają właściwość typu złożonego.

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
