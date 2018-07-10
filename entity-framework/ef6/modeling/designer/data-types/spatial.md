---
title: Przestrzenne EF6 - projektancie platformy EF -
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
caps.latest.revision: 3
ms.openlocfilehash: 2332818275763fd90274f426b7fced4c3c14ac17
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914131"
---
# <a name="spatial---ef-designer"></a>Przestrzenne - projektancie platformy EF
> [!NOTE]
> **EF5 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 5. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.

Przewodnik krok po kroku i wideo pokazuje, jak do mapowania typów przestrzennych z programu Entity Framework Designer. Ilustruje też sposób używania zapytania LINQ można znaleźć odległość między dwiema lokalizacjami.

W tym przewodniku użyje pierwszego modelu do utworzenia nowej bazy danych, ale może również służyć projektancie platformy EF z [Database First](~/ef6/modeling/designer/workflows/database-first.md) przepływu pracy do mapowania istniejącej bazy danych.

Obsługa przestrzenne typu została wprowadzona w programie Entity Framework 5. Należy pamiętać, że aby korzystać z nowych funkcji, takich jak typ przestrzennych, wyliczeń i funkcji z wartościami przechowywanymi w tabeli, należy wskazać .NET Framework 4.5. Program Visual Studio 2012 jest przeznaczony dla platformy .NET 4.5 domyślnie.

Używanie typów danych przestrzennych, należy również użyć dostawcy środowiska Entity Framework, który obsługuje przestrzennych. Zobacz [Obsługa dostawców dla typów przestrzennych](~/ef6/fundamentals/providers/spatial-support.md) Aby uzyskać więcej informacji.

Istnieją dwa typy danych przestrzennych głównego: geometry i położenia geograficznego. Typ danych Geografia przechowuje dane ellipsoidal (na przykład współrzędne geograficzne GPS koordynuje). Typ danych Geometria reprezentuje euklidesowa współrzędnych (płaski).

## <a name="watch-the-video"></a>Obejrzyj wideo
To wideo pokazuje, jak do mapowania typów przestrzennych z programu Entity Framework Designer. Ilustruje też sposób używania zapytania LINQ można znaleźć odległość między dwiema lokalizacjami.

**Osoba prezentująca**: Julia Kornich

**Film wideo**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Musisz być w wersji programu Visual Studio 2012 Ultimate, Premium, Professional i Web Express zainstalowany w celu przeprowadzenia tego instruktażu.

## <a name="set-up-the-project"></a>Konfigurowanie projektu

1.  Otwórz program Visual Studio 2012
2.  Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**
3.  W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu
4.  Wprowadź **SpatialEFDesigner** jako nazwę projektu i kliknij przycisk **OK**

## <a name="create-a-new-model-using-the-ef-designer"></a>Utwórz nowy Model przy użyciu projektanta EF

1.  Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wskaż opcję **Dodaj**, a następnie kliknij przycisk **nowy element**
2.  Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów
3.  Wprowadź **UniversityModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**
4.  Na stronie Kreator modelu Entity Data Model wybierz **pusty Model** w oknie dialogowym Wybierz zawartość modelu
5.  Kliknij przycisk **Zakończ**

Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu.

Kreator wykonuje następujące czynności:

-   Generuje plik EnumTestModel.edmx, który definiuje modelu koncepcyjnego, model magazynu i mapowanie między nimi. Ustawia właściwość metadanych artefaktów przetwarzania pliku edmx osadzania w zestawie danych wyjściowych, więc pliki metadanych wygenerowanych Pobierz osadzone w zestawie.
-   Dodanie odwołania do następujących zestawów: platformy EntityFramework, System.ComponentModel.DataAnnotations i System.Data.Entity.
-   Tworzy pliki UniversityModel.tt i UniversityModel.Context.tt i dodaje je w pliku edmx. Te pliki szablonów T4 generowania kodu, który definiuje typu DbContext pochodnych i POCO typów, które mapują jednostek w modelu edmx

## <a name="add-a-new-entity-type"></a>Dodaj nowy typ jednostki

1.  Kliknij prawym przyciskiem myszy pusty obszar powierzchni projektu, wybierz opcję **Add -&gt; jednostki**, pojawi się okno dialogowe nowej jednostki
2.  Określ **University** dla typu nazwy i określ **UniversityID** dla danej nazwy właściwości klucza, pozostaw typ jako **Int32**
3.  Kliknij przycisk **OK**
4.  Kliknij prawym przyciskiem myszy jednostkę, a następnie wybierz pozycję **Dodaj nowy -&gt; właściwość skalarną**
5.  Zmień nazwę nowej właściwości **nazwy**
6.  Dodawanie innej skalarne właściwości i zmień jej nazwę na **lokalizacji** Otwórz okno właściwości, a następnie zmień typ nowej właściwości **lokalizacji geograficznej**
7.  Zapisz model i skompiluj projekt
    > [!NOTE]
    > W przypadku tworzenia, ostrzeżenia dotyczące podmiotów niezmapowanych i skojarzenia może pojawić się na liście błędów. Można zignorować te ostrzeżenia, ponieważ po Wybierzmy wygenerować bazę danych z modelu, błędy znikną.

## <a name="generate-database-from-model"></a>Generuj bazę danych z modelu

Teraz możemy wygenerować bazę danych, która jest oparta na modelu.

1.  Kliknij prawym przyciskiem myszy pusty obszar w Projektancie jednostki powierzchni i wybierz **Generuj z bazy danych z modelu**
2.  Wybierz połączenie danych dialogowym Generuj Kreatora bazy danych jest wyświetlany, kliknij przycisk **nowe połączenie** przycisk Określ **(localdb)\\mssqllocaldb** dla nazwy serwera i  **Uniwersytet** bazy danych, a następnie kliknij przycisk **OK**
3.  Okno z pytaniem, jeśli chcesz utworzyć nową bazę danych będą się pojawiać, kliknij przycisk **tak**.
4.  Kliknij przycisk **dalej** i Kreatora tworzenia bazy danych generuje języka definicji danych (DDL) dla tworzenia bazy danych wygenerowanej języka DDL są wyświetlane w Podsumowanie i okna dialogowego pole Uwaga dotycząca ustawień, który DDL nie zawiera definicji dla tabelę, która mapuje do typu wyliczenia
5.  Kliknij przycisk **Zakończ** kliknięcie przycisku Zakończ nie wykonuje skrypt języka DDL.
6.  Kreator tworzenia bazy danych wykonuje następujące czynności: Otwiera **UniversityModel.edmx.sql** w Edytor T-SQL generuje sekcje schematu i mapowania magazynu EDMX pliku informacji o parametrach połączenia usług AD DS do pliku App.config
7.  Kliknij prawym przyciskiem myszy w edytorze języka T-SQL i wybierz pozycję **Execute** nawiązać połączenie z serwerem w oknie dialogowym wyświetlony, podaj informacje o połączeniu z kroku 2, a następnie kliknij polecenie **Connect**
8.  Aby wyświetlić wygenerowany schemat, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksploratorze obiektów SQL Server, a następnie wybierz **odświeżania**

## <a name="persist-and-retrieve-data"></a>Utrwalanie i pobieranie danych

Otwórz plik Program.cs, w którym jest zdefiniowana metoda Main. Dodaj następujący kod do funkcji Main.

Ten kod dodaje dwa nowe obiekty University do kontekstu. Właściwości przestrzenne są inicjowane za pomocą metody DbGeography.FromText. Punkt lokalizacji geograficznej, reprezentowane jako WellKnownText jest przekazywany do metody. Kod następnie zapisuje dane. Następnie zapytania LINQ, która zwraca obiekt University, gdzie jego lokalizacja jest najbardziej zbliżony do określonej lokalizacji jest tworzony i wykonywane.

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

Skompilować i uruchomić aplikację. Program generuje następujące wyniki:

```
The closest University to you is: School of Fine Art.
```

Aby wyświetlić dane w bazie danych, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksploratorze obiektów SQL Server, a następnie wybierz **Odśwież**. Kliknięcie prawym przyciskiem myszy tabelę i wybierz pozycję **dane widoku**.

## <a name="summary"></a>Podsumowanie

W tym przewodniku zobaczyliśmy, jak do mapowania typów przestrzennych, za pomocą programu Entity Framework Designer oraz jak używać typów przestrzennych w kodzie. 
