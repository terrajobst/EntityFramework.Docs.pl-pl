---
title: Entity Framework Designer skrótów klawiaturowych - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
caps.latest.revision: 3
ms.openlocfilehash: 438a9368c5594243ddf06eee3c12d3eb100ff2da
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912713"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Skróty klawiaturowe projektanta programu Entity Framework
Ta strona zawiera listę skróty klawiaturowe, które są dostępne na różnych ekranach narzędzi Entity Framework Tools for Visual Studio.

## <a name="adonet-entity-data-model-wizard"></a>Kreator modelu danych jednostki ADO.NET

### <a name="step-one-choose-model-contents"></a>Jeden krok: Wybierz zawartość modelu

![WizardOne](~/ef6/media/wizardone.png)

| Skrót  | Akcja                                                     | Uwagi                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **Alt + n** | Przenieś do następnego ekranu                                        | Nie jest dostępna dla wszystkich wyborów zawartość modelu. |
| **ALT + f** | Zakończ pracę kreatora                                              | Nie jest dostępna dla wszystkich wyborów zawartość modelu. |
| **ALT + w** | Przełącza widok "co modelu może zawierać?" okienko. |                                                     |

### <a name="step-two-choose-your-connection"></a>Krok 2: Wybierz połączenie

![WizardTwo](~/ef6/media/wizardtwo.png)

| Skrót  | Akcja                                                     | Uwagi                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **Alt + n** | Przenieś do następnego ekranu                                        |                                                         |
| **ALT + p** | Przenieś do poprzedniego ekranu                                    |                                                         |
| **ALT + w** | Przełącza widok "co modelu może zawierać?" okienko. |                                                         |
| **Alt + c** | Otwórz okno "Właściwości połączenia"                    | Umożliwia określenie nowego połączenia z bazą danych. |
| **Alt + e** | Wyklucz poufne dane z parametrów połączenia          |                                                         |
| **ALT + i** | Dołączenia danych poufnych w parametrach połączenia            |                                                         |
| **Alt + s** | Ustaw opcję "Zapisz ustawienia połączenia w pliku App.Config" |                                                         |

### <a name="step-three-choose-your-version"></a>Krok 3: Wybierz swoją wersję

![WizardThree](~/ef6/media/wizardthree.png)

| Skrót  | Akcja                                             | Uwagi                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **Alt + n** | Przenieś do następnego ekranu                                |                                                                                       |
| **ALT + p** | Przenieś do poprzedniego ekranu                            |                                                                                       |
| **ALT + w** | Przełącz fokus na wybór wersji platformy Entity Framework | Umożliwia określenie innej wersji programu Entity Framework do użycia w projekcie. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>Krok 4: Wybierz obiekty bazy danych i ustawień

![WizardFour](~/ef6/media/wizardfour.png)

| Skrót  | Akcja                                                                                    | Uwagi                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **ALT + f** | Zakończ pracę kreatora                                                                             |                                                                     |
| **ALT + p** | Przenieś do poprzedniego ekranu                                                                   |                                                                     |
| **ALT + w** | Przełącza widok okienko wyboru obiektu bazy danych                                            | Umożliwia określenie obiektów bazy danych do odtworzenia odtwarzane.    |
| **Alt + s** | Przełącz "Pluralize lub końcówek nazw generowanych obiektów" opcja                       |                                                                     |
| **ALT + k** | Ustaw opcję "Dołącz kolumny klucza obcego w modelu"                              | Nie jest dostępna dla wszystkich wyborów zawartość modelu.                 |
| **ALT + i** | Ustaw opcję "Importuj wybrane procedur przechowywanych i funkcji do modelu entity" | Nie jest dostępna dla wszystkich wyborów zawartość modelu.                 |
| **ALT + m** | Przełącza fokus do pola tekstowego "Model Namespace"                                        | Nie jest dostępna dla wszystkich wyborów zawartość modelu.                 |
| **Miejsce** | Przełącz zaznaczenie elementu                                                               | Jeśli element ma elementy podrzędne, wszystkie elementy podrzędne zostaną także przełączać |
| **Po lewej stronie**  | Zwiń drzewa podrzędnego                                                                       |                                                                     |
| **Po prawej stronie** | Rozwiń drzewo podrzędne                                                                         |                                                                     |
| **W górę**    | Przejdź do poprzedniego elementu w drzewie                                                      |                                                                     |
| **W dół**  | Przejdź do następnego elementu w drzewie                                                          |                                                                     |

## <a name="ef-designer-surface"></a>Powierzchnia projektowa EF

![DesignerSurface](~/ef6/media/designersurface.png)

| Skrót                                                                                | Akcja                      | Uwagi                                                                                                                                                                                                                           |
|:----------------------------------------------------------------------------------------|:----------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Wprowadź/miejsca**                                                                         | Przełącz zaznaczenie            | Włącza lub wyłącza wybór obiektu z fokusem.                                                                                                                                                                                     |
| **ESC**                                                                                 | Anuluj zaznaczenie            | Anuluje bieżące zaznaczenie.                                                                                                                                                                                                  |
| **CTRL + A**                                                                            | Zaznacz wszystko                  | Wybiera wszystkie kształty na powierzchni projektowej.                                                                                                                                                                                   |
| **Strzałka w górę**                                                                            | Przenieś w górę                     | Przenosi wybrany jednostki się przyrostu jednej siatce. <br/> Jeśli na liście, przenosi do poprzedniego ograniczonym element równorzędny.                                                                                                                        |
| **Strzałka w dół**                                                                          | Przenieś w dół                   | Przenosi wybrany jednostki przyrostu jednej siatce w dół. <br/> Jeśli na liście, przechodzi do następnego ograniczonym element równorzędny.                                                                                                                          |
| **Strzałka w lewo**                                                                          | Przenieś w lewo                   | Przenosi wybrany jednostki przyrostu po lewej stronie w jednej siatce. <br/> Jeśli na liście, przenosi do poprzedniego ograniczonym element równorzędny.                                                                                                                      |
| **Strzałka w prawo**                                                                         | Przesuń w prawo                  | Przenosi wybrany przyrostu siatki jeden jednostki. <br/> Jeśli na liście, przechodzi do następnego ograniczonym element równorzędny.                                                                                                                         |
| **Shift + Strzałka w lewo**                                                                  | Rozmiar kształtu w lewo             | Zmniejsza szerokość wybraną jednostkę, z przyrostem jednej siatce.                                                                                                                                                                 |
| **Shift + Strzałka w prawo**                                                                 | Rozmiar kształtu w prawo            | Zwiększa szerokość wybranej jednostki przyrostu jednej siatce.                                                                                                                                                               |
| **Strona główna**                                                                                | Pierwszy elementu równorzędnego                  | Przenosi fokus i wyboru, aby pierwszy obiekt na powierzchni projektowej, w tym samym poziomie elementów równorzędnych.                                                                                                                                     |
| **End**                                                                                 | Ostatniego elementu równorzędnego                   | Przenosi fokus i wybór, ostatni obiekt na powierzchni projektowej, w tym samym poziomie elementów równorzędnych.                                                                                                                                      |
| **Ctrl + Home**                                                                         | Pierwszy elementu równorzędnego (focus)          | Tak samo jak pierwszy elementu równorzędnego, ale przenosi fokus zamiast przenoszenia fokusu i wybór.                                                                                                                                                      |
| **CTRL + End**                                                                          | Ostatniego elementu równorzędnego (focus)           | Takie same jak ostatniej komunikacji równorzędnej, ale przenosi fokus zamiast przenoszenia fokusu i wybór.                                                                                                                                                       |
| **Karta**                                                                                 | Dalej elementu równorzędnego                   | Przenosi fokus i zaznaczenie do następnego obiektu na powierzchni projektowej, w tym samym poziomie elementów równorzędnych.                                                                                                                                      |
| **Shift + Tab**                                                                           | Poprzednich elementów równorzędnych               | Przenosi fokus i zaznaczenie do poprzedniego obiektu na powierzchni projektowej, w tym samym poziomie elementów równorzędnych.                                                                                                                                  |
| **ALT + klawisze Ctrl + Tab**                                                                        | Dalej elementu równorzędnego (focus)           | Takie same jak dalej komunikacji równorzędnej, ale przenosi fokus zamiast przenoszenia fokusu i wybór.                                                                                                                                                       |
| **ALT + klawisze Ctrl + Shift + Tab**                                                                  | Poprzednich elementów równorzędnych (focus)       | Tak samo jak w poprzednich elementów równorzędnych, ale przenosi fokus zamiast przenoszenia fokusu i wybór.                                                                                                                                                   |
| **&lt;**                                                                                | Ascend                      | Przechodzi do następnego obiektu na projekt powierzchni jeden poziom wyżej w hierarchii. Jeśli nie istnieją żadne kształty powyżej tego kształtu w hierarchii (czyli obiekt jest umieszczany bezpośrednio na powierzchni projektowej), diagram zostanie wybrany. |
| **&gt;**                                                                                | Jest elementem podrzędnym elementu                     | Przechodzi do następnego obiektu zawarte na projekt powierzchni jeden poziom poniżej tego w hierarchii. W przypadku przechowywany obiekt, który nie jest pusta.                                                                          |
| **CTRL + &lt;**                                                                         | Ascend (focus)              | Takie same jak ascend polecenia, ale przenosi fokus bez zaznaczenia.                                                                                                                                                                      |
| **CTRL + &gt;**                                                                         | Jest elementem podrzędnym elementu (focus)             | Takie same jak jest elementem podrzędnym elementu polecenia, ale przenosi fokus bez zaznaczenia.                                                                                                                                                                     |
| **Shift + End**                                                                         | Należy wykonać, aby połączone         | Z jednostki przenosi do jednostki podłączone do tej jednostki.                                                                                                                                                           |
| **Del**                                                                                 | Usuwanie                      | Usuń obiekt lub łącznika z diagramu.                                                                                                                                                                                 |
| **Dodatki**                                                                                 | Insert                      | Dodaje nową właściwość do jednostki, po wybraniu nagłówek przedziału "Scalar właściwości" lub samej właściwości.                                                                                                       |
| **Strona w górę**                                                                               | Diagram przewiń w górę           | Przewija powierzchni projektowej, w przyrostach co równa 75% wysokość powierzchni projektowej aktualnie widoczne.                                                                                                                |
| **Strona w dół**                                                                             | Diagram przewiń w dół         | Przewija widok powierzchni projektowej w dół.                                                                                                                                                                                                |
| **SHIFT + strona w dół**                                                                     | Diagram przewiń w prawo        | Przewija powierzchni projektowej po prawej stronie.                                                                                                                                                                                        |
| **SHIFT + strona w górę**                                                                       | Diagram przewiń w lewo         | Przewija powierzchni projektowej po lewej stronie.                                                                                                                                                                                         |
| **F2**                                                                                  | Przejdź do trybu edycji             | Standardowa skrót klawiaturowy do wprowadzenia trybu edycji dla kontrolki tekstu.                                                                                                                                                           |
| **SHIFT + F10**                                                                         | Wyświetla menu skrótów.       | Standardowa skrót klawiaturowy do wyświetlania wybranego elementu menu skrótów.                                                                                                                                                      |
| **Kliknięcie lewym przyciskiem Control + Shift + myszy**  <br/> **Kontrolka + Shift + kółka myszy do przodu**  | Powiększenie semantyczne w            | Powiększa się w obszarze widoku diagramu pod kursorem myszy.                                                                                                                                                             |
| **Kliknij prawym przyciskiem myszy formant + Shift + myszy** <br/> **Kontrolka + Shift + kółka myszy do tyłu** | Powiększenie semantyczne Out           | OtherContextMenus.MicrosoftDataEntityDesignContext.Select.BaseType OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Entity                                                                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Property** <br/> **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Subtype**                        | OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAll                     | OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAssociation                                                                                                                                                                                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinDiagram** <br/> **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinModelBrowser**                       | OtherContextMenus.MicrosoftDataEntityDesignContext.StoredProcedureMapping                    | OtherContextMenus.MicrosoftDataEntityDesignContext.TableMapping OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Entity                                                                                        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.UpdateModelfromDatabase**                  | OtherContextMenus.MicrosoftDataEntityDesignContext.Validate                   | OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.10 OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.100                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.125**                                                              | OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.150 | OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.200                                                                                                                                                           |

## <a name="mapping-details-window"></a>OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.25

![OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.300](~/ef6/media/mappingdetailsshortcuts.png)

| Skrót                  | Akcja         | Uwagi                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **Karta**                   | OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.33 | OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.400                                                                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.50**            | Nawigacja     | OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.66 OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.75 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.Custom** <br/> **Miejsce** | Wybierz         | OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomIn                                                                                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomOut**      | OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomtoFit      | View.EntityDataModelBrowser                                                                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.Custom**                 | View.EntityDataModelMappingDetails    | Wybiera element na liście rozwijanej.                                                                                               |
| **ESC**                   | Zamknij listę     | Zamyka listy rozwijanej.                                                                                                              |

## <a name="visual-studio-navigation"></a>Nawigacja w programie Visual Studio

Entity Framework dostarcza również akcje, które mogą mieć niestandardowych skrótów klawiaturowych mapowane (nie skróty są mapowane domyślnie). Aby utworzyć te skróty niestandardowe, kliknij menu Narzędzia, a następnie opcje.  W obszarze środowiska wybierz opcję klawiatury.  Przewiń w dół na liście w środku dopiero po przeprowadzeniu wybranie żądanego polecenia, wprowadź skrót, w polu tekstowym "Naciśnij klawisze skrótu" i kliknij pozycję przypisania. Skróty możliwe są następujące:

| Skrót                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Add.ComplexProperty.ComplexTypes**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddCodeGenerationItem**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddFunctionImport**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.AddEnumType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Association**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexProperty**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.FunctionImport**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Inheritance**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.NavigationProperty**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ScalarProperty**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNewDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddtoDiagram**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Close**                                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Collapse**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ConverttoEnum**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.CollapseAll**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExpandAll**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExportasImage**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.LayoutDiagram**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Edit**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.EntityKey**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Expand**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.FunctionImportMapping**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GenerateDatabasefromModel**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GoToDefinition**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.ShowGrid**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.SnaptoGrid**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.IncludeRelated**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MappingDetails**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ModelBrowser**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveDiagramstoSeparateFile**              |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down5**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToBottom**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToTop**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up5**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MovetonewDiagram**                        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Open**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.MovetoNewComplexType**           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.Rename**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.RemovefromDiagram**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Rename**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayName**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayNameandType** |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.BaseType**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Property**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Subtype**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAll**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAssociation**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinModelBrowser**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.StoredProcedureMapping**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.TableMapping**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.UpdateModelfromDatabase**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Validate**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.10**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.100**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.125**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.150**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.200**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.25**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.300**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.33**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.400**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.50**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.66**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.75**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.Custom**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomIn**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomOut**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomtoFit**                          |
| **View.EntityDataModelBrowser**                                                                |
| **View.EntityDataModelMappingDetails**                                                         |
