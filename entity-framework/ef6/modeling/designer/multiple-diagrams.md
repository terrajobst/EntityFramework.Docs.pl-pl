---
title: Wiele diagramów na modelu — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
ms.openlocfilehash: e78b927a147143629ac5b73e23edf439ba6d0264
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283618"
---
# <a name="multiple-diagrams-per-model"></a>Wiele diagramów na modelu
> [!NOTE]
> **EF5 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 5. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.

Ten film wideo i stronie pokazuje, jak podzielić modelu wiele diagramów przy użyciu narzędzia Entity Framework Designer (Projektant EF). Można użyć tej funkcji, gdy model staje się zbyt duży, aby wyświetlić lub edytować.

We wcześniejszych wersjach programu projektancie platformy EF jednym diagramie można mieć tylko dla pliku EDMX. Począwszy od programu Visual Studio 2012, umożliwia projektancie platformy EF Podziel plik EDMX na wiele diagramów.

## <a name="watch-the-video"></a>Obejrzyj wideo
To wideo pokazuje, jak podzielić modelu wiele diagramów przy użyciu narzędzia Entity Framework Designer (Projektant EF). Można użyć tej funkcji, gdy model staje się zbyt duży, aby wyświetlić lub edytować.

**Osoba prezentująca**: Julia Kornich

**Film wideo**: [WMV](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>Omówienie projektanta EF

Podczas tworzenia modelu przy użyciu Kreatora modelu danych jednostki w Projektancie platformy EF plik edmx jest utworzony i dodany do rozwiązania. Ten plik definiuje kształt jednostek i sposobu mapowania ich na bazie danych.

W Projektancie platformy EF składa się z następujących składników:

-   Powierzchnia projektowania wizualnego służące do edycji modelu. Tworzenia, modyfikowania lub usuwania jednostki i skojarzenia.
-   A **przeglądarka modeli** okna, który zawiera widoki drzewa modelu.  Jednostek i ich skojarzenia, które znajdują się w obszarze *\[ModelName\]* folderu. Tabele bazy danych i ograniczeń, które znajdują się w obszarze  *\[ModelName\]*. Store folder.
-   A **szczegóły mapowania** okna do wyświetlania i edytowania mapowania. Typy jednostek i skojarzenia można mapować do bazy danych tabel, kolumn i procedur składowanych. 

Po zakończeniu działania Kreator modelu Entity Data Model automatycznie otwierania okna powierzchni projektowania wizualnego. Jeśli przeglądarka modelu nie jest widoczna, kliknij prawym przyciskiem myszy główny projekt powierzchni i wybierz **przeglądarka modeli**.

Poniższy zrzut ekranu przedstawia plik edmx otwarty w Projektancie platformy EF. Na zrzucie ekranu przedstawiono powierzchnia projektowania wizualnego (do lewej) i **przeglądarka modeli** okno (z prawej strony).

![Projektancie platformy EF 2](~/ef6/media/efdesigner2.png)

Aby cofnąć operacji w Projektancie platformy EF, kliknij przycisk klawisze Ctrl + Z.

## <a name="working-with-diagrams"></a>Praca z diagramami

Domyślnie w Projektancie platformy EF tworzy jeden diagram o nazwie Diagram1. W przypadku diagramu z dużą liczbą jednostek i skojarzenia będzie najczęściej chcesz chcesz podzielić logicznie. Począwszy od programu Visual Studio 2012, można wyświetlić model koncepcyjny w wiele diagramów.   

W miarę dodawania nowych diagramów pojawiają się w folderze diagramy w oknie przeglądarki modelu. Aby zmienić nazwę diagramu: Wybierz diagram w oknie przeglądarki modelu, kliknij jeden raz na nazwę i wpisz nową nazwę.  Możesz również kliknąć prawym przyciskiem myszy nazwę diagramu i wybierz **Zmień nazwę**.

Nazwa diagramu jest wyświetlany obok nazwy pliku edmx w edytorze programu Visual Studio. Na przykład Model1.edmx\[Diagram1\].

![DiagramName](~/ef6/media/diagramname.png)

Zawartość diagramy (kształt i kolor encji i skojarzeń) jest przechowywana w. edmx.diagram pliku. Aby wyświetlić ten plik, wybierz w Eksploratorze rozwiązań i rozwijania pliku edmx. 

![DiagramFiles](~/ef6/media/diagramfiles.png)

Nie należy edytować. edmx.diagram ręcznie, plik zawartości tego pliku może być zastąpiona przez projektancie platformy EF.
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>Dzielenie jednostek i skojarzenia do nowego diagramu

Możesz wybrać jednostek na diagramie istniejących (naciśnij i przytrzymaj klawisz Shift, aby wybrać wiele jednostek). Kliknij prawym przyciskiem myszy i wybierz pozycję **przenieść do nowego diagramu**. Zostanie utworzony nowy diagram i wybranych obiektów i ich skojarzenia zostaną przeniesione do diagramu.

Alternatywnie, kliknij prawym przyciskiem myszy folder diagramy w przeglądarce modelu i wybierz **Dodaj nowy Diagram.** Można następnie przeciągnij i upuść jednostek jest dostępna z folderu typów jednostek w przeglądarce modelu na powierzchnię projektu.

Można również Wytnij lub Kopiuj jednostki (przy użyciu klawiszy Ctrl-X lub Ctrl-C) z jednym diagramie i wkleić (przy użyciu klawisza Ctrl-V) na drugim. Diagram, w której wklejasz jednostki już zawiera jednostkę o takiej samej nazwie, Nowa jednostka zostanie utworzona i dodana do modelu.  Na przykład: Diagram2 zawiera jednostkę działu. Następnie możesz wkleić innego działu, na Diagram2. Jednostka Department1 zostanie utworzony i dodany do modelu koncepcyjnego.   

Aby dołączyć powiązanych jednostek na diagramie, kliknij rick jednostki i wybierz **obejmują powiązane**. Spowoduje to kopia powiązanych jednostek i skojarzenia na diagramie określony.

## <a name="changing-the-color-of-entities"></a>Zmiana koloru jednostek

Oprócz dzieląc modelu na wiele diagramów, można również zmienić kolory jednostek.

Aby zmienić kolor, wybierz jednostki (lub wiele jednostek) na powierzchni projektowej. Następnie kliknij prawym przyciskiem myszy i wybierz **właściwości**. W oknie dialogowym właściwości wybierz **kolor wypełnienia** właściwości. Określanie koloru za pomocą prawidłową nazwą koloru (na przykład kolor czerwony) albo prawidłowy RGB, (na przykład, 255, 128, 128). 

![Kolor](~/ef6/media/color.png)

## <a name="summary"></a>Podsumowanie

W tym temacie przyjrzeliśmy się jak podzielić modelu wiele diagramów, a także jak określić innego koloru dla jednostki za pomocą programu Entity Framework Designer. 
