---
title: Projektant przestrzenny-EF — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: a9c54fbc14dd02ce5d4d91449a0d5f9e72f7f0f7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418540"
---
# <a name="spatial---ef-designer"></a>Projektant przestrzenny-EF
> [!NOTE]
> **EF5 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 5. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.

Film wideo i przewodnik krok po kroku przedstawiają sposób mapowania typów przestrzennych przy użyciu Entity Framework Designer. Pokazano również, jak używać zapytania LINQ, aby znaleźć odległość między dwiema lokalizacjami.

Ten Instruktaż będzie używać Model First do tworzenia nowej bazy danych, ale można również użyć programu Dr Designer z przepływem pracy [Database First](~/ef6/modeling/designer/workflows/database-first.md) , aby mapować do istniejącej bazy danych.

Obsługa typów przestrzennych została wprowadzona w Entity Framework 5. Należy pamiętać, że aby korzystać z nowych funkcji, takich jak typ przestrzenny, wyliczenia i funkcje zwracające tabelę, należy określić wartość docelową .NET Framework 4,5. Program Visual Studio 2012 Domyślnie kieruje .NET 4,5.

Aby można było używać typów danych przestrzennych, należy również użyć dostawcy Entity Framework z obsługą przestrzenną. Aby uzyskać więcej informacji, zobacz [Obsługa dostawcy dla typów przestrzennych](~/ef6/fundamentals/providers/spatial-support.md) .

Istnieją dwa główne typy danych przestrzennych: Geografia i geometria. Typ danych Geografia przechowuje dane ellipsoidal (na przykład współrzędne geograficzne i Długość geograficzna). Typ danych geometrii reprezentuje układ współrzędnych Euclidean (płaski).

## <a name="watch-the-video"></a>Obejrzyj film
W tym filmie wideo przedstawiono sposób mapowania typów przestrzennych przy użyciu Entity Framework Designer. Pokazano również, jak używać zapytania LINQ, aby znaleźć odległość między dwiema lokalizacjami.

**Przedstawione przez**: Julia Kornich

**Wideo**: [wmv](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (zip)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik, musisz mieć zainstalowaną wersję Visual Studio 2012, Ultimate, Premium, Professional lub Web Express Edition.

## <a name="set-up-the-project"></a>Konfigurowanie projektu

1.  Otwórz program Visual Studio 2012
2.  W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt** .
3.  W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **konsoli**
4.  Wprowadź **SpatialEFDesigner** jako nazwę projektu, a następnie kliknij przycisk **OK** .

## <a name="create-a-new-model-using-the-ef-designer"></a>Tworzenie nowego modelu przy użyciu narzędzia Dr Designer

1.  Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element** .
2.  Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.
3.  W polu Nazwa pliku wprowadź **UniversityModel. edmx** , a następnie kliknij przycisk **Dodaj** .
4.  Na stronie kreatora Entity Data Model wybierz pozycję **pusty model** w oknie dialogowym Wybierz zawartość modelu
5.  Kliknij przycisk **Zakończ**

Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu.

Kreator wykonuje następujące czynności:

-   Generuje plik EnumTestModel. edmx, który definiuje model koncepcyjny, model magazynu i mapowanie między nimi. Ustawia właściwość przetwarzania artefaktu metadanych pliku. edmx na osadzony w zestawie danych wyjściowych, aby wygenerowane pliki metadanych zostały osadzone w zestawie.
-   Dodaje odwołanie do następujących zestawów: EntityFramework, system. ComponentModel. DataAnnotations i system. Data. Entity.
-   Tworzy pliki UniversityModel.tt i UniversityModel.Context.tt i dodaje je do pliku. edmx. Te pliki szablonów T4 generują kod, który definiuje typ pochodny DbContext i typy POCO, które są mapowane na jednostki w modelu. edmx

## <a name="add-a-new-entity-type"></a>Dodaj nowy typ jednostki

1.  Kliknij prawym przyciskiem myszy pusty obszar na powierzchni projektowej, a następnie wybierz polecenie **dodaj&gt; jednostki**, pojawi się okno dialogowe Nowa jednostka
2.  Określ nazwę typu dla **Uniwersytetu** i określ **UniversityID** dla nazwy właściwości klucza, pozostaw typ jako **Int32**
3.  Kliknij przycisk **OK**.
4.  Kliknij prawym przyciskiem myszy jednostkę i wybierz polecenie **Dodaj nową-&gt; Właściwość skalarna**
5.  Zmień nazwę nowej właściwości na **nazwę**
6.  Dodaj kolejną Właściwość skalarną i zmień jej nazwę na **lokalizację** Otwórz okno właściwości i Zmień typ nowej właściwości na **Geografia**
7.  Zapisz model i skompiluj projekt
    > [!NOTE]
    > Podczas kompilowania w Lista błędów mogą pojawić się ostrzeżenia dotyczące niezamapowanych jednostek i skojarzeń. Te ostrzeżenia można zignorować, ponieważ po wybraniu opcji wygenerowania bazy danych z modelu te błędy zostaną odrzucone.

## <a name="generate-database-from-model"></a>Generuj bazę danych na podstawie modelu

Teraz możemy wygenerować bazę danych opartą na modelu.

1.  Kliknij prawym przyciskiem myszy puste miejsce na Entity Designer powierzchni i wybierz polecenie **Generuj bazę danych na podstawie modelu**
2.  Zostanie wyświetlone okno dialogowe Wybieranie połączenia danych w Kreatorze generowania bazy danych kliknij przycisk **nowe połączenie** , określ **(LocalDB)\\mssqllocaldb** dla nazwy serwera i **University** dla bazy danych, a następnie kliknij przycisk **OK** .
3.  Zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz utworzyć nową bazę danych, kliknij przycisk **tak**.
4.  Kliknij przycisk **dalej** , aby Kreator tworzenia bazy danych wygenerował język definicji danych (DDL) służący do tworzenia bazy danych, wygenerowany kod DDL jest wyświetlany w oknie dialogowym Podsumowanie i ustawienia Zanotuj, że kod DDL nie zawiera definicji tabeli, która jest mapowana na typ wyliczenia
5.  Kliknij przycisk **Zakończ** kliknięcie przycisku Zakończ nie powoduje wykonania skryptu DDL.
6.  Kreator tworzenia bazy danych wykonuje następujące czynności: otwiera plik **UniversityModel. edmx. SQL** w edytorze T-SQL generuje schemat magazynu i sekcje mapowania pliku edmx dodaje informacje o parametrach połączenia do pliku App. config
7.  Kliknij prawym przyciskiem myszy w edytorze T-SQL i wybierz polecenie **Wykonaj** okno dialogowe łączenie z serwerem, wprowadź informacje o połączeniu z kroku 2 i kliknij pozycję **Połącz** .
8.  Aby wyświetlić wygenerowany schemat, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksplorator obiektów SQL Server i wybierz polecenie **Odśwież** .

## <a name="persist-and-retrieve-data"></a>Utrwalanie i pobieranie danych

Otwórz plik Program.cs, w którym jest zdefiniowana Metoda Main. Dodaj następujący kod do funkcji main.

Kod dodaje dwa nowe obiekty University do kontekstu. Właściwości przestrzenne są inicjowane przy użyciu metody DbGeography. FromText. Punkt geografii reprezentowany jako WellKnownText jest przenoszona do metody. Następnie kod zapisuje dane. Następnie zapytanie LINQ, które zwraca obiekt University, gdzie jego lokalizacja znajduje się najbliżej określonej lokalizacji, jest konstruowany i wykonywany.

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
    {
        Name = "School of Fine Art",
        Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
    });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                                orderby u.Location.Distance(myLocation)
                                select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

Skompiluj i uruchom aplikację. Program tworzy następujące dane wyjściowe:

```console
The closest University to you is: School of Fine Art.
```

Aby wyświetlić dane w bazie danych, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksplorator obiektów SQL Server i wybierz polecenie **Odśwież**. Następnie kliknij prawym przyciskiem myszy w tabeli i wybierz polecenie **Wyświetl dane**.

## <a name="summary"></a>Podsumowanie

W tym instruktażu przedstawiono sposób mapowania typów przestrzennych przy użyciu Entity Framework Designer i sposobu używania typów przestrzennych w kodzie. 
