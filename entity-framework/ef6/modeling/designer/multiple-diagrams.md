---
title: Wiele diagramów na model — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
ms.openlocfilehash: e78b927a147143629ac5b73e23edf439ba6d0264
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418295"
---
# <a name="multiple-diagrams-per-model"></a>Wiele diagramów na model
> [!NOTE]
> **EF5 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 5. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.

Ten film wideo i strona przedstawiają sposób dzielenia modelu na wiele diagramów przy użyciu Entity Framework Designer (program EF Designer). Możesz chcieć użyć tej funkcji, gdy model będzie zbyt duży do wyświetlania lub edycji.

We wcześniejszych wersjach projektanta EF można było tylko jeden diagram dla pliku EDMX. Począwszy od programu Visual Studio 2012, można użyć programu Dr Designer do dzielenia pliku EDMX na wiele diagramów.

## <a name="watch-the-video"></a>Obejrzyj film
W tym filmie wideo pokazano, jak podzielić model na wiele diagramów przy użyciu Entity Framework Designer (program Dr Designer). Możesz chcieć użyć tej funkcji, gdy model będzie zbyt duży do wyświetlania lub edycji.

**Przedstawione przez**: Julia Kornich

**Wideo**: [wmv](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (zip)](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>Omówienie projektanta EF

Podczas tworzenia modelu przy użyciu kreatora Entity Data Model programu EF, plik. edmx zostanie utworzony i dodany do rozwiązania. Ten plik definiuje kształt obiektów i sposób ich mapowania do bazy danych.

Projektant Dr składa się z następujących składników:

-   Powierzchnia projektowania wizualizacji do edycji modelu. Można tworzyć, modyfikować lub usuwać jednostki i skojarzenia.
-    okno **przeglądarki modelu** , które udostępnia widoki drzewa modelu.  Jednostki i ich skojarzenia znajdują się w folderze *\[modelname\]* . Tabele i ograniczenia bazy danych znajdują się w obszarze *\[modelname\]* . Folder przechowywania.
-   Okno **szczegóły mapowania** do wyświetlania i edytowania mapowań. Można mapować typy jednostek lub skojarzenia z tabelami baz danych, kolumnami i procedurami składowanymi. 

Okno powierzchni projektowej wizualizacji jest automatycznie otwierane po zakończeniu pracy Kreatora Entity Data Model. Jeśli przeglądarka modelu nie jest widoczna, kliknij prawym przyciskiem myszy główną powierzchnię projektu i wybierz pozycję **przeglądarka modeli**.

Poniższy zrzut ekranu przedstawia plik. edmx otwarty w projektancie EF. Zrzut ekranu przedstawia powierzchnię projektowania wizualizacji (po lewej stronie) i okna **przeglądarki modelu** (po prawej stronie).

![Dr Designer 2](~/ef6/media/efdesigner2.png)

Aby cofnąć operację wykonywaną w projektancie EF, kliknij przycisk Ctrl-Z.

## <a name="working-with-diagrams"></a>Praca z diagramami

Domyślnie program Dr Designer tworzy jeden diagram o nazwie Diagram1. Jeśli masz diagram o dużej liczbie jednostek i skojarzeń, najprawdopodobniej chcesz podzielić je logicznie. Począwszy od programu Visual Studio 2012, model koncepcyjny można wyświetlić w wielu diagramach.   

Po dodaniu nowych diagramów pojawiają się one w folderze diagramy w oknie Przeglądarka modeli. Aby zmienić nazwę diagramu: Wybierz diagram w oknie Przeglądarka modeli, kliknij jeden raz na nazwie i wpisz nową nazwę.  Możesz również kliknąć prawym przyciskiem myszy nazwę diagramu i wybrać polecenie **Zmień nazwę**.

Nazwa diagramu jest wyświetlana obok nazwy pliku. edmx w edytorze programu Visual Studio. Na przykład Model1. edmx\[Diagram1\].

![DiagramName](~/ef6/media/diagramname.png)

Zawartość diagramów (kształt i kolor jednostek i skojarzeń) są przechowywane w pliku. edmx. diagram. Aby wyświetlić ten plik, wybierz Eksplorator rozwiązań i unfold plik. edmx. 

![DiagramFiles](~/ef6/media/diagramfiles.png)

Nie należy edytować pliku edmx. diagram ręcznie, zawartość tego pliku może zostać zastąpiona przez projektanta EF.
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>Dzielenie jednostek i skojarzeń z nowym diagramem

Można wybrać jednostki na istniejącym diagramie (przytrzymaj klawisz Shift, aby zaznaczyć wiele jednostek). Kliknij prawym przyciskiem myszy, a następnie wybierz pozycję **Przenieś do nowego diagramu**. Nowy diagram zostanie utworzony, a wybrane jednostki i ich skojarzenia są przenoszone do diagramu.

Alternatywnie możesz kliknąć prawym przyciskiem myszy folder diagramy w przeglądarce modelu i wybrać polecenie **Dodaj nowy diagram.** Następnie można przeciągać i upuszczać jednostki z poziomu folderu Entity Types w przeglądarce modelu na powierzchnię projektu.

Można również wyciąć lub skopiować jednostki (za pomocą klawiszy Ctrl-X lub CTRL-C) z jednego diagramu i wkleić je (przy użyciu klawisza Ctrl-V). Jeśli diagram, do którego wklejasz jednostkę, już zawiera jednostkę o tej samej nazwie, zostanie utworzona nowa jednostka i zostanie dodana do modelu.  Na przykład: Diagram2 zawiera jednostkę działu. Następnie należy wkleić inny dział w Diagram2. Jednostka Department1 jest tworzona i dodawana do modelu koncepcyjnego.   

Aby uwzględnić powiązane jednostki w diagramie, Rick jednostki i wybierz pozycję **Dołącz powiązane**. Spowoduje to utworzenie kopii powiązanych jednostek i skojarzeń na określonym diagramie.

## <a name="changing-the-color-of-entities"></a>Zmiana koloru jednostek

Oprócz dzielenia modelu na wiele diagramów można również zmienić kolory jednostek.

Aby zmienić kolor, wybierz jednostkę (lub wiele jednostek) na powierzchni projektowej. Następnie kliknij prawym przyciskiem myszy, a następnie wybierz pozycję **Właściwości**. W okno Właściwości wybierz właściwość **kolor wypełnienia** . Określ kolor przy użyciu prawidłowej nazwy koloru (na przykład Red) lub prawidłowej wartości RGB (na przykład 255, 128, 128). 

![Kolor](~/ef6/media/color.png)

## <a name="summary"></a>Podsumowanie

W tym temacie przedstawiono sposób dzielenia modelu na wiele diagramów, a także sposobu określania innego koloru dla jednostki przy użyciu Entity Framework Designer. 
