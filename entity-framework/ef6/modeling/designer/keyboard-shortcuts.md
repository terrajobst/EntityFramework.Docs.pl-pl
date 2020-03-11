---
title: Entity Framework Designer skróty klawiaturowe — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
ms.openlocfilehash: c75eafcca0863faa1ad64202e98b61832827377c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418519"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Skróty klawiaturowe Entity Framework Designer
Ta strona zawiera listę shorcuts z klawiatury, które są dostępne na różnych ekranach Entity Framework Tools dla programu Visual Studio.

## <a name="adonet-entity-data-model-wizard"></a>Kreator Entity Data Model ADO.NET

### <a name="step-one-choose-model-contents"></a>Krok 1: Wybierz zawartość modelu

![Kreator jeden](~/ef6/media/wizardone.png)

| Skrót  | Akcja                                                     | Uwagi                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **Alt + n** | Przejdź do następnego ekranu                                        | Niedostępne dla wszystkich wyborów zawartości modelu. |
| **Alt + f** | Zakończ pracę z kreatorem                                              | Niedostępne dla wszystkich wyborów zawartości modelu. |
| **Alt + w** | Zmień fokus na "jaki powinien zawierać model?". okno. |                                                     |

### <a name="step-two-choose-your-connection"></a>Krok 2: Wybierz połączenie

![Kreator — dwa](~/ef6/media/wizardtwo.png)

| Skrót  | Akcja                                                     | Uwagi                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **Alt + n** | Przejdź do następnego ekranu                                        |                                                         |
| **Alt + p** | Przenieś do poprzedniego ekranu                                    |                                                         |
| **Alt + w** | Zmień fokus na "jaki powinien zawierać model?". okno. |                                                         |
| **Alt + c** | Otwórz okno "właściwości połączenia".                    | Umożliwia zdefiniowanie nowego połączenia z bazą danych. |
| **Alt + e** | Wyklucz dane poufne z parametrów połączenia          |                                                         |
| **Alt + i** | Uwzględnij dane poufne w parametrach połączenia            |                                                         |
| **Alt + s** | Przełącz opcję "Zapisz ustawienia połączenia w pliku App. config" |                                                         |

### <a name="step-three-choose-your-version"></a>Krok trzeci. Wybieranie wersji

![Kreator — trzy](~/ef6/media/wizardthree.png)

| Skrót  | Akcja                                             | Uwagi                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **Alt + n** | Przejdź do następnego ekranu                                |                                                                                       |
| **Alt + p** | Przenieś do poprzedniego ekranu                            |                                                                                       |
| **Alt + w** | Przełącz fokus na wybór Entity Framework wersji | Umożliwia określenie innej wersji Entity Framework do użycia w projekcie. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>Krok 4. Wybieranie obiektów i ustawień bazy danych

![Kreator 4](~/ef6/media/wizardfour.png)

| Skrót  | Akcja                                                                                    | Uwagi                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **Alt + f** | Zakończ pracę z kreatorem                                                                             |                                                                     |
| **Alt + p** | Przenieś do poprzedniego ekranu                                                                   |                                                                     |
| **Alt + w** | Przełącz fokus do okienka wyboru obiektów bazy danych                                            | Umożliwia określenie obiektów bazy danych, które mają być odtwarzane.    |
| **Alt + s** | Przełączenie opcji "pluralize lub nazwom generated names"                       |                                                                     |
| **Alt + k** | Przełącz opcję "Uwzględnij kolumny klucza obcego w modelu"                              | Niedostępne dla wszystkich wyborów zawartości modelu.                 |
| **Alt + i** | Przełącz opcję "Importuj wybrane procedury składowane i funkcje do modelu jednostki" | Niedostępne dla wszystkich wyborów zawartości modelu.                 |
| **Alt + m** | Przełącza fokus do pola tekstowego "model przestrzeni nazw"                                        | Niedostępne dla wszystkich wyborów zawartości modelu.                 |
| **Odstęp** | Przełącz zaznaczenie elementu                                                               | Jeśli element ma elementy podrzędne, wszystkie elementy podrzędne zostaną również przełączane |
| **Lewym**  | Zwiń drzewo potomne                                                                       |                                                                     |
| **Kliknij** | Rozwiń drzewo potomne                                                                         |                                                                     |
| **Konfigurowanie**    | Przejdź do poprzedniego elementu w drzewie                                                      |                                                                     |
| **Notuj**  | Przejdź do następnego elementu w drzewie                                                          |                                                                     |

## <a name="ef-designer-surface"></a>Powierzchnia projektanta Dr

![Powierzchnia projektanta](~/ef6/media/designersurface.png)

| Skrót                                                                                | Akcja                      | Uwagi                                                                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Spacja/ENTER**                                                                         | Przełącz zaznaczenie            | Przełącza zaznaczenie obiektu fokusem.                                                                                                                                                                                         |
| **ESC**                                                                                 | Anuluj zaznaczenie            | Anuluje bieżące zaznaczenie.                                                                                                                                                                                                      |
| **Ctrl + A**                                                                            | Zaznacz wszystko                  | Zaznacza wszystkie kształty na powierzchni projektowej.                                                                                                                                                                                       |
| **Strzałka w górę**                                                                            | Przenieś w górę                     | Przenosi wybraną jednostkę w górę o jedną siatkę w górę. <br/> Jeśli na liście, przenosi do poprzedniego podpola elementu równorzędnego.                                                                                                                            |
| **Strzałka w dół**                                                                          | Przenieś w dół                   | Przenosi wybraną jednostkę w dół o jeden przyrost siatki. <br/> Jeśli na liście, przenosi do następnego podpola równorzędnego.                                                                                                                              |
| **Strzałka w lewo**                                                                          | Przenieś w lewo                   | Przesuwa wybraną jednostkę w lewo o jeden przyrost siatki. <br/> Jeśli na liście, przenosi do poprzedniego podpola elementu równorzędnego.                                                                                                                          |
| **Strzałka w prawo**                                                                         | Przenieś w prawo                  | Przesuwa wybraną jednostkę o jeden przyrost w dół. <br/> Jeśli na liście, przenosi do następnego podpola równorzędnego.                                                                                                                             |
| **Shift + Strzałka w lewo**                                                                  | Kształt rozmiaru — w lewo             | Zmniejsza szerokość wybranej jednostki o jeden przyrost siatki.                                                                                                                                                                     |
| **Shift + Strzałka w prawo**                                                                 | Kształt rozmiaru — prawy            | Zwiększa szerokość wybranej jednostki o jeden przyrost siatki.                                                                                                                                                                   |
| **Mowa**                                                                                | Pierwszy element równorzędny                  | Przesuwa fokus i zaznaczenie do pierwszego obiektu na powierzchni projektowej na tym samym poziomie elementu równorzędnego.                                                                                                                                         |
| **Zakończenie**                                                                                 | Ostatni element równorzędny                   | Przenosi fokus i zaznaczenie do ostatniego obiektu na powierzchni projektowej na tym samym poziomie elementu równorzędnego.                                                                                                                                          |
| **Ctrl + Home**                                                                         | Pierwszy element równorzędny (fokus)          | Taki sam jak pierwszy element równorzędny, ale przenosi fokus zamiast przesuwania fokusu i wyboru.                                                                                                                                                          |
| **Ctrl + End**                                                                          | Ostatni element równorzędny (fokus)           | Analogicznie jak ostatni element równorzędny, ale przenosi fokus zamiast przesuwania fokusu i wyboru.                                                                                                                                                           |
| **Tabulator**                                                                                 | Następny element równorzędny                   | Przenosi fokus i zaznaczenie do następnego obiektu na powierzchni projektowej na tym samym poziomie elementu równorzędnego.                                                                                                                                          |
| **Shift + Tab**                                                                           | Poprzedni element równorzędny               | Przenosi fokus i zaznaczenie do poprzedniego obiektu na powierzchni projektowej na tym samym poziomie elementu równorzędnego.                                                                                                                                      |
| **Alt + Ctrl + Tab**                                                                        | Następny element równorzędny (fokus)           | Analogicznie jak następny element równorzędny, ale przenosi fokus, zamiast przenosić fokus i wybór.                                                                                                                                                           |
| **Alt + Ctrl + Shift + Tab**                                                                  | Poprzedni element równorzędny (fokus)       | Taki sam jak poprzedni element równorzędny, ale przenosi fokus zamiast przesuwania fokusu i wyboru.                                                                                                                                                       |
| **&lt;**                                                                                | Wg                      | Przenosi do następnego obiektu na powierzchni projektowej o jeden poziom wyżej w hierarchii. Jeśli nie ma kształtów powyżej tego kształtu w hierarchii (oznacza to, że obiekt jest umieszczony bezpośrednio na powierzchni projektowej), diagram jest zaznaczony. |
| **&gt;**                                                                                | Elementy                     | Przenosi do następnego zawartego obiektu na powierzchni projektowej o jeden poziom poniżej tego, w hierarchii. Jeśli nie ma żadnego obiektu, to nie jest.                                                                              |
| **Ctrl + &lt;**                                                                         | Ascend (fokus)              | Analogicznie jak polecenie Ascend, ale przenosi fokus bez zaznaczania.                                                                                                                                                                          |
| **Ctrl + &gt;**                                                                         | Dolny (fokus)             | Analogicznie jak polecenie podrzędne, ale przenosi fokus bez zaznaczania.                                                                                                                                                                         |
| **Shift + End**                                                                         | Śledź połączenie         | Z jednostki program przenosi do jednostki, z którą jest połączona ta jednostka.                                                                                                                                                               |
| **Usunięcie**                                                                                 | Usuń                      | Usuń obiekt lub łącznik z diagramu.                                                                                                                                                                                     |
| **Stawk**                                                                                 | Insert                      | Dodaje nową właściwość do jednostki w przypadku wybrania nagłówka przedziału "właściwości skalarne" lub samej właściwości.                                                                                                           |
| **PG up**                                                                               | Przewiń o diagram w górę           | Przewija powierzchnię projektu, w przyrostach równej 75% wysokości obecnie widocznej powierzchni projektowej.                                                                                                                    |
| **PG Strzałka w dół**                                                                             | Przewiń diagram w dół         | Przewija powierzchnię projektu w dół.                                                                                                                                                                                                    |
| **Shift + Strzałka w dół**                                                                     | Przewiń do prawej        | Przewija powierzchnię projektu w prawo.                                                                                                                                                                                            |
| **Shift + PG up**                                                                       | Przewiń do lewej diagram         | Przewija powierzchnię projektu w lewo.                                                                                                                                                                                             |
| **F2**                                                                                  | Przejdź do trybu edycji             | Standardowy skrót klawiaturowy do wprowadzania trybu edycji dla kontrolki tekstowej.                                                                                                                                                               |
| **Shift + F10**                                                                         | Wyświetl menu skrótów       | Standardowy skrót klawiaturowy do wyświetlania menu skrótów wybranego elementu.                                                                                                                                                          |
| **Kontrolka + Shift + Strzałka w lewo**  <br/> **Kontrolka + Shift + kółka myszy do przodu**  | Powiększenie semantyczne            | Powiększa w obszarze widoku diagramu pod wskaźnikiem myszy.                                                                                                                                                                 |
| **Control + Shift + Strzałka w prawo kliknięcia** <br/> **Control + Shift + kółka myszy do tyłu** | Powiększenie semantyczne           | Powiększa się z obszaru widoku diagramu pod wskaźnikiem myszy. Umożliwia ponowne wyśrodkowanie diagramu w przypadku pomniejszenia rozmiaru bieżącego centrum diagramów.                                                                          |
| **Control + Shift + ' + '** <br/> **Kontrolka + kółka myszy do przodu**                        | Powiększ                     | Powiększa się w środku widoku diagramu.                                                                                                                                                                                         |
| **Control + Shift + '-'** <br/> **Control + kółka myszy do tyłu**                       | Pomniejsz                    | Powiększa się z klikniętego obszaru widoku diagramu. Umożliwia ponowne wyśrodkowanie diagramu w przypadku pomniejszenia rozmiaru bieżącego centrum diagramów.                                                                                            |
| **Kontrolka + Shift + Rysuj prostokąt z lewym przyciskiem myszy w dół**                  | Obszar powiększenia                   | Powiększa się do środka w wybranym obszarze. Po przytrzymaniu klawiszy Control + Shift zobaczysz, że kursor zmieni się w lupę, która pozwala na zdefiniowanie obszaru do powiększania.                        |
| **Klucz menu kontekstowego + 'M '**                                                              | Otwórz okno szczegółów mapowania | Otwiera okno Szczegóły mapowania, aby edytować mapowania dla wybranej jednostki                                                                                                                                                               |

## <a name="mapping-details-window"></a>Mapowanie okna Szczegóły

![Skróty szczegółów mapowania](~/ef6/media/mappingdetailsshortcuts.png)

| Skrót                  | Akcja         | Uwagi                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **Tabulator**                   | Przełącz kontekst | Przełącza między głównym obszarem okna a paskiem narzędzi po lewej stronie                                                                     |
| **Klawisze strzałek**            | Nawigacja     | Przenieś w górę i w dół wiersze lub w prawo i w lewo między kolumnami w obszarze okna głównego. Przechodź między przyciskami na pasku narzędzi po lewej stronie. |
| **Wejść** <br/> **Odstęp** | Wybierz pozycję         | Wybiera przycisk na pasku narzędzi po lewej stronie.                                                                                          |
| **Alt + Strzałka w dół**      | Otwórz listę      | Lista rozwijana, jeśli wybrano komórkę z listą rozwijaną.                                                                     |
| **Wejść**                 | Wybór listy    | Wybiera element na liście rozwijanej.                                                                                               |
| **ESC**                   | Zamknij listę     | Zamyka listę rozwijaną.                                                                                                              |

## <a name="visual-studio-navigation"></a>Nawigacja programu Visual Studio

Entity Framework dostarcza również wielu akcji, które mogą mieć zamapowane niestandardowe skróty klawiaturowe (nie są domyślnie mapowane żadne skróty). Aby utworzyć niestandardowe skróty, kliknij menu Narzędzia, a następnie opcje.  W obszarze środowisko wybierz pozycję Klawiatura.  Przewiń w dół listę w środku do momentu wybrania żądanego polecenia, wprowadź skrót w polu tekstowym "naciśnij klawisze skrótów", a następnie kliknij przycisk Przypisz. Możliwe są następujące skróty:

| Skrót                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Add. ComplexProperty. ComplexTypes**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddCodeGenerationItem**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddFunctionImport**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. AddEnumType**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. Association**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. ComplexProperty**                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. ComplexType**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. Entity**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. FunctionImport**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. dziedziczenia**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. NavigationProperty**               |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. element scalarproperty**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNewDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddtoDiagram**                            |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Close**                                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zwijanie**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ConverttoEnum**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. diagram. Zwiń wszystko**                     |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. diagram. Rozwiń wszystko**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. diagram. ExportasImage**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. diagram. LayoutDiagram**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Edit**                                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. EntityKey**                               |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. expand**                                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. FunctionImportMapping**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GenerateDatabasefromModel**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GoToDefinition**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Grid. ShowGrid**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Grid. SnaptoGrid**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.IncludeRelated**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MappingDetails**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. element ModelBrowser**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveDiagramstoSeparateFile**              |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MoveProperties. Down**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down5**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToBottom**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToTop**                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MoveProperties. up**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up5**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MovetonewDiagram**                        |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Open**                                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Refaktoryzacja. MovetoNewComplexType**           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Refaktoryzacja. Zmień nazwę**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.RemovefromDiagram**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Rename**                                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ScalarPropertyFormat. DisplayName**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayNameandType** |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. BaseType**                         |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. Entity**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. Property**                         |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. Subtype**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAll**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAssociation**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinModelBrowser**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.StoredProcedureMapping**                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. elementu TableMapping**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.UpdateModelfromDatabase**                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Validate**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 10**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 100**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 125**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 150**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 200**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 25**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 300**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 33**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 400**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 50**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 66**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 75**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. Custom**                             |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. zoom**                             |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. ZoomOut**                            |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. ZoomtoFit**                          |
| **Widok. EntityDataModelBrowser**                                                                |
| **Widok. EntityDataModelMappingDetails**                                                         |
