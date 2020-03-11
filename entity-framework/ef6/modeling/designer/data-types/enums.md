---
title: Enum support-Dr Designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 92a763b84a04d3ce7ec0853ef2a4852356cf7997
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418568"
---
# <a name="enum-support---ef-designer"></a>Obsługa Wyliczenie-Dr Designer
> [!NOTE]
> **EF5 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 5. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.

Ten film wideo i przewodnik krok po kroku pokazują, jak używać typów wyliczeniowych z Entity Framework Designer. Pokazano również, jak używać wyliczeń w zapytaniu LINQ.

Ten Instruktaż będzie używać Model First do tworzenia nowej bazy danych, ale można również użyć programu Dr Designer z przepływem pracy [Database First](~/ef6/modeling/designer/workflows/database-first.md) , aby mapować do istniejącej bazy danych.

Obsługa wyliczenia została wprowadzona w Entity Framework 5. Aby korzystać z nowych funkcji, takich jak wyliczenia, typy danych przestrzennych i funkcje zwracające tabelę, należy określić wartość docelową .NET Framework 4,5. Program Visual Studio 2012 Domyślnie kieruje .NET 4,5.

W Entity Framework Wyliczenie może mieć następujące typy podstawowe: **Byte**, **Int16**, **Int32**, **Int64** **lub**bajty.

## <a name="watch-the-video"></a>Obejrzyj wideo
W tym filmie wideo pokazano, jak używać typów wyliczeniowych z Entity Framework Designer. Pokazano również, jak używać wyliczeń w zapytaniu LINQ.

**Przedstawione przez**: Julia Kornich

**Wideo**: [wmv](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (zip)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik, musisz mieć zainstalowaną wersję Visual Studio 2012, Ultimate, Premium, Professional lub Web Express Edition.

## <a name="set-up-the-project"></a>Konfigurowanie projektu

1.  Otwórz program Visual Studio 2012
2.  W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt** .
3.  W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **konsoli**
4.  Wprowadź **EnumEFDesigner** jako nazwę projektu, a następnie kliknij przycisk **OK** .

## <a name="create-a-new-model-using-the-ef-designer"></a>Tworzenie nowego modelu przy użyciu narzędzia Dr Designer

1.  Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element** .
2.  Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.
3.  W polu Nazwa pliku wprowadź **EnumTestModel. edmx** , a następnie kliknij przycisk **Dodaj** .
4.  Na stronie kreatora Entity Data Model wybierz pozycję **pusty model** w oknie dialogowym Wybierz zawartość modelu
5.  Kliknij przycisk **Zakończ**

Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu.

Kreator wykonuje następujące czynności:

-   Generuje plik EnumTestModel. edmx, który definiuje model koncepcyjny, model magazynu i mapowanie między nimi. Ustawia właściwość przetwarzania artefaktu metadanych pliku. edmx na osadzony w zestawie danych wyjściowych, aby wygenerowane pliki metadanych zostały osadzone w zestawie.
-   Dodaje odwołanie do następujących zestawów: EntityFramework, system. ComponentModel. DataAnnotations i system. Data. Entity.
-   Tworzy pliki EnumTestModel.tt i EnumTestModel.Context.tt i dodaje je do pliku. edmx. Te pliki szablonów T4 generują kod, który definiuje typ pochodny DbContext i typy POCO, które są mapowane na jednostki w modelu. edmx.

## <a name="add-a-new-entity-type"></a>Dodaj nowy typ jednostki

1.  Kliknij prawym przyciskiem myszy pusty obszar na powierzchni projektowej, a następnie wybierz polecenie **dodaj&gt; jednostki**, pojawi się okno dialogowe Nowa jednostka
2.  Określ **dział** dla nazwy typu i określ **DepartmentID** dla nazwy właściwości klucza, pozostaw typ jako **Int32**
3.  Kliknij przycisk **OK**.
4.  Kliknij prawym przyciskiem myszy jednostkę i wybierz polecenie **Dodaj nową-&gt; Właściwość skalarna**
5.  Zmień nazwę nowej właściwości na **nazwę**
6.  Zmień typ nowej właściwości na **Int32** (domyślnie Nowa właściwość jest typu String) Aby zmienić typ, Otwórz okno właściwości i Zmień Właściwość Type na **Int32**
7.  Dodaj kolejną Właściwość skalarną i zmień jej nazwę na **budżet**, Zmień typ na **dziesiętny**

## <a name="add-an-enum-type"></a>Dodawanie typu wyliczeniowego

1.  W Entity Framework Designer kliknij prawym przyciskiem myszy Właściwość Name, wybierz pozycję **Konwertuj na Wyliczenie** .

    ![Konwertuj na Wyliczenie](~/ef6/media/converttoenum.png)

2.  W oknie dialogowym **Dodawanie wyliczenia** wpisz **DepartmentNames** dla nazwy typu wyliczenia, Zmień typ podstawowy na **Int32**, a następnie Dodaj następujące elementy członkowskie do typu: angielski, matematyczny i ekonomia

    ![Dodaj typ wyliczenia](~/ef6/media/addenumtype.png)

3.  Naciśnij przycisk **OK**
4.  Zapisz model i skompiluj projekt
    > [!NOTE]
    > Podczas kompilowania w Lista błędów mogą pojawić się ostrzeżenia dotyczące niezamapowanych jednostek i skojarzeń. Te ostrzeżenia można zignorować, ponieważ po wybraniu opcji wygenerowania bazy danych z modelu te błędy zostaną odrzucone.

Jeśli zobaczysz okno Właściwości, zobaczysz, że typ właściwości Nazwa została zmieniona na **DepartmentNames** , a nowo dodany typ wyliczeniowy został dodany do listy typów.

Po przełączeniu do okna przeglądarki modelu zobaczysz, że typ został również dodany do węzła typy wyliczeniowe.

![Przeglądarka modeli](~/ef6/media/modelbrowser.png)

>[!NOTE]
> Możesz również dodać nowe typy wyliczeniowe z tego okna, klikając prawym przyciskiem myszy i wybierając pozycję **Dodaj typ wyliczeniowy**. Po utworzeniu typu zostanie on wyświetlony na liście typów i będzie można skojarzyć z właściwością

## <a name="generate-database-from-model"></a>Generuj bazę danych na podstawie modelu

Teraz możemy wygenerować bazę danych opartą na modelu.

1.  Kliknij prawym przyciskiem myszy puste miejsce na Entity Designer powierzchni i wybierz polecenie **Generuj bazę danych na podstawie modelu**
2.  Zostanie wyświetlone okno dialogowe Wybieranie połączenia danych w Kreatorze generowania bazy danych. kliknij przycisk **nowe połączenie** , określ **(LocalDB)\\mssqllocaldb** dla bazy danych i **EnumTest** , a następnie kliknij przycisk **OK** .
3.  Zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz utworzyć nową bazę danych, kliknij przycisk **tak**.
4.  Kliknij przycisk **dalej** , aby Kreator tworzenia bazy danych wygenerował język definicji danych (DDL) służący do tworzenia bazy danych, wygenerowany kod DDL jest wyświetlany w oknie dialogowym Podsumowanie i ustawienia Zanotuj, że kod DDL nie zawiera definicji tabeli, która jest mapowana na typ wyliczenia
5.  Kliknij przycisk **Zakończ** kliknięcie przycisku Zakończ nie powoduje wykonania skryptu DDL.
6.  Kreator tworzenia bazy danych wykonuje następujące czynności: otwiera plik **EnumTest. edmx. SQL** w edytorze T-SQL generuje schemat magazynu i sekcje mapowania pliku edmx dodaje informacje o parametrach połączenia do pliku App. config
7.  Kliknij prawym przyciskiem myszy w edytorze T-SQL i wybierz polecenie **Wykonaj** okno dialogowe łączenie z serwerem, wprowadź informacje o połączeniu z kroku 2 i kliknij pozycję **Połącz** .
8.  Aby wyświetlić wygenerowany schemat, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksplorator obiektów SQL Server i wybierz polecenie **Odśwież** .

## <a name="persist-and-retrieve-data"></a>Utrwalanie i pobieranie danych

Otwórz plik Program.cs, w którym jest zdefiniowana Metoda Main. Dodaj następujący kod do funkcji main. Kod dodaje nowy obiekt działu do kontekstu. Następnie zapisuje dane. Kod wykonuje również zapytanie LINQ, które zwraca dział, w którym nazwa jest DepartmentNames. English.

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

Skompiluj i uruchom aplikację. Program tworzy następujące dane wyjściowe:

```console
DepartmentID: 1 Name: English
```

Aby wyświetlić dane w bazie danych, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksplorator obiektów SQL Server i wybierz polecenie **Odśwież**. Następnie kliknij prawym przyciskiem myszy w tabeli i wybierz polecenie **Wyświetl dane**.

## <a name="summary"></a>Podsumowanie

W tym instruktażu przedstawiono sposób mapowania typów wyliczeniowych przy użyciu Entity Framework Designer i sposobu używania wyliczeń w kodzie. 
