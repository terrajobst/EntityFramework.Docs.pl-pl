---
title: Obsługa Enum - projektancie platformy EF - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: d4c5528c4dc13ab7189421feebf84c2cb2f4b2bb
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995640"
---
# <a name="enum-support---ef-designer"></a>Pomoc techniczna dla wyliczenia — projektancie platformy EF
> [!NOTE]
> **EF5 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 5. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.

W tym przewodniku krok po kroku i wideo pokazuje, jak w typach wyliczeniowych za pomocą programu Entity Framework Designer. Ilustruje też sposób używania typów wyliczeniowych w zapytaniu programu LINQ.

W tym przewodniku użyje pierwszego modelu do utworzenia nowej bazy danych, ale może również służyć projektancie platformy EF z [Database First](~/ef6/modeling/designer/workflows/database-first.md) przepływu pracy do mapowania istniejącej bazy danych.

Obsługa typu wyliczeniowego została wprowadzona w Entity Framework 5. Aby korzystać z nowych funkcji, takich jak typy wyliczeniowe, typy danych przestrzennych i funkcji z wartościami przechowywanymi w tabeli, należy wskazać .NET Framework 4.5. Program Visual Studio 2012 jest przeznaczony dla platformy .NET 4.5 domyślnie.

W programie Entity Framework, wyliczenie może mieć następujące typy bazowe: **bajtów**, **Int16**, **Int32**, **Int64** , lub **SByte**.

## <a name="watch-the-video"></a>Obejrzyj wideo
To wideo pokazuje, jak w typach wyliczeniowych za pomocą programu Entity Framework Designer. Ilustruje też sposób używania typów wyliczeniowych w zapytaniu programu LINQ.

**Osoba prezentująca**: Julia Kornich

**Film wideo**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Musisz być w wersji programu Visual Studio 2012 Ultimate, Premium, Professional i Web Express zainstalowany w celu przeprowadzenia tego instruktażu.

## <a name="set-up-the-project"></a>Konfigurowanie projektu

1.  Otwórz program Visual Studio 2012
2.  Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**
3.  W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu
4.  Wprowadź **EnumEFDesigner** jako nazwę projektu i kliknij przycisk **OK**

## <a name="create-a-new-model-using-the-ef-designer"></a>Utwórz nowy Model przy użyciu projektanta EF

1.  Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wskaż opcję **Dodaj**, a następnie kliknij przycisk **nowy element**
2.  Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów
3.  Wprowadź **EnumTestModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**
4.  Na stronie Kreator modelu Entity Data Model wybierz **pusty Model** w oknie dialogowym Wybierz zawartość modelu
5.  Kliknij przycisk **Zakończ**

Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu.

Kreator wykonuje następujące czynności:

-   Generuje plik EnumTestModel.edmx, który definiuje modelu koncepcyjnego, model magazynu i mapowanie między nimi. Ustawia właściwość metadanych artefaktów przetwarzania pliku edmx osadzania w zestawie danych wyjściowych, więc pliki metadanych wygenerowanych Pobierz osadzone w zestawie.
-   Dodanie odwołania do następujących zestawów: platformy EntityFramework, System.ComponentModel.DataAnnotations i System.Data.Entity.
-   Tworzy pliki EnumTestModel.tt i EnumTestModel.Context.tt i dodaje je w pliku edmx. Te pliki szablonów T4 wygenerować kod, który definiuje typu DbContext pochodnych i POCO typów, które mapują jednostek w modelu edmx.

## <a name="add-a-new-entity-type"></a>Dodaj nowy typ jednostki

1.  Kliknij prawym przyciskiem myszy pusty obszar powierzchni projektu, wybierz opcję **Add -&gt; jednostki**, pojawi się okno dialogowe nowej jednostki
2.  Określ **działu** dla typu nazwy i określ **DepartmentID** dla danej nazwy właściwości klucza, pozostaw typ jako **Int32**
3.  Kliknij przycisk **OK**
4.  Kliknij prawym przyciskiem myszy jednostkę, a następnie wybierz pozycję **Dodaj nowy -&gt; właściwość skalarną**
5.  Zmień nazwę nowej właściwości **nazwy**
6.  Zmień typ nowej właściwości **Int32** (domyślnie przez nową właściwość jest typu String) Aby zmienić typ, Otwórz okno właściwości, a następnie zmień typ właściwości **Int32**
7.  Dodawanie innej skalarne właściwości i zmień jej nazwę na **budżetu**, Zmień typ na **dziesiętna**

## <a name="add-an-enum-type"></a>Dodaj typ wyliczeniowy

1.  W Entity Framework Designer kliknij prawym przyciskiem myszy nazwę właściwości, wybierz **przekonwertować wyliczenia**

    ![ConvertToEnum](~/ef6/media/converttoenum.png)

2.  W **Dodaj wyliczenie** okna dialogowego wpisz **DepartmentNames** dla nazwy typu wyliczenia, Zmień typ podstawowy do **Int32**, a następnie dodaj następujące elementy członkowskie do typu: angielski, Matematyczne i oszczędnościami, jakie daje

    ![AddEnumType](~/ef6/media/addenumtype.png)

3.  Naciśnij klawisz **OK**
4.  Zapisz model i skompiluj projekt
    > [!NOTE]
    > W przypadku tworzenia, ostrzeżenia dotyczące podmiotów niezmapowanych i skojarzenia może pojawić się na liście błędów. Można zignorować te ostrzeżenia, ponieważ po Wybierzmy wygenerować bazę danych z modelu, błędy znikną.

Jeśli przyjrzymy się w oknie właściwości, zauważysz, że typ właściwości Nazwa została zmieniona na **DepartmentNames** i typ wyliczeniowy nowo dodanych została dodana do listy typów.

Po przełączeniu do okna przeglądarki modelu pojawi się, czy typ został również dodany do węzła typach wyliczeniowych.

![Element ModelBrowser](~/ef6/media/modelbrowser.png)

>[!NOTE]
> Można również dodać nowe typy wyliczenia z tego okna, klikając prawym przyciskiem myszy i wybierając **Dodaj typ wyliczeniowy**. Po utworzeniu typu pojawi się na liście typów i będzie można skojarzyć z właściwością

## <a name="generate-database-from-model"></a>Generuj bazę danych z modelu

Teraz możemy wygenerować bazę danych, która jest oparta na modelu.

1.  Kliknij prawym przyciskiem myszy pusty obszar w Projektancie jednostki powierzchni i wybierz **Generuj z bazy danych z modelu**
2.  Wybierz połączenie danych dialogowym Generuj Kreatora bazy danych jest wyświetlany, kliknij przycisk **nowe połączenie** przycisk Określ **(localdb)\\mssqllocaldb** dla nazwy serwera i  **EnumTest** bazy danych, a następnie kliknij przycisk **OK**
3.  Okno z pytaniem, jeśli chcesz utworzyć nową bazę danych będą się pojawiać, kliknij przycisk **tak**.
4.  Kliknij przycisk **dalej** i Kreatora tworzenia bazy danych generuje języka definicji danych (DDL) dla tworzenia bazy danych wygenerowanej języka DDL są wyświetlane w Podsumowanie i okna dialogowego pole Uwaga dotycząca ustawień, który DDL nie zawiera definicji dla tabelę, która mapuje do typu wyliczenia
5.  Kliknij przycisk **Zakończ** kliknięcie przycisku Zakończ nie wykonuje skrypt języka DDL.
6.  Kreator tworzenia bazy danych wykonuje następujące czynności: Otwiera **EnumTest.edmx.sql** w Edytor T-SQL generuje sekcje schematu i mapowania magazynu EDMX pliku informacji o parametrach połączenia usług AD DS do pliku App.config
7.  Kliknij prawym przyciskiem myszy w edytorze języka T-SQL i wybierz pozycję **Execute** nawiązać połączenie z serwerem w oknie dialogowym wyświetlony, podaj informacje o połączeniu z kroku 2, a następnie kliknij polecenie **Connect**
8.  Aby wyświetlić wygenerowany schemat, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksploratorze obiektów SQL Server, a następnie wybierz **odświeżania**

## <a name="persist-and-retrieve-data"></a>Utrwalanie i pobieranie danych

Otwórz plik Program.cs, w którym jest zdefiniowana metoda Main. Dodaj następujący kod do funkcji Main. Ten kod dodaje nowy obiekt działu do kontekstu. Następnie zapisuje dane. Kod wykonuje też zapytanie LINQ, które zwraca działu, gdzie nazwa jest DepartmentNames.English.

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Skompilować i uruchomić aplikację. Program generuje następujące wyniki:

```
DepartmentID: 1 Name: English
```

Aby wyświetlić dane w bazie danych, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksploratorze obiektów SQL Server, a następnie wybierz **Odśwież**. Kliknięcie prawym przyciskiem myszy tabelę i wybierz pozycję **dane widoku**.

## <a name="summary"></a>Podsumowanie

W tym przewodniku zobaczyliśmy, jak do mapowania typów wyliczenia przy użyciu programu Entity Framework Designer oraz jak używać wyliczenia w kodzie. 
