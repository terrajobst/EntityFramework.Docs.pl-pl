---
title: Dzielenie jednostek projektanta — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418463"
---
# <a name="designer-entity-splitting"></a>Dzielenie jednostek projektanta
W tym instruktażu pokazano, jak zmapować typ jednostki na dwie tabele, modyfikując model przy użyciu Entity Framework Designer (Dr Designer). Możesz zmapować jednostkę na wiele tabel, gdy tabele korzystają ze wspólnego klucza. Koncepcje, które mają zastosowanie do mapowania typu jednostki na dwie tabele, można łatwo rozszerzyć w celu mapowania typu jednostki na więcej niż dwie tabele.

Na poniższej ilustracji przedstawiono główne okna, które są używane podczas pracy z programem Dr Designer.

![Projektant EF](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Wymagania wstępne

Visual Studio 2012 lub Visual Studio 2010, Ultimate, Premium, Professional lub Web Express Edition.

## <a name="create-the-database"></a>Tworzenie bazy danych

Serwer bazy danych zainstalowany przy użyciu programu Visual Studio różni się w zależności od zainstalowanej wersji programu Visual Studio:

-   Jeśli używasz programu Visual Studio 2012, zostanie utworzona baza danych LocalDB.
-   Jeśli używasz programu Visual Studio 2010, tworzysz bazę danych SQL Express.

Najpierw utworzymy bazę danych z dwiema tabelami, które zostaną połączone w jedną jednostkę.

-   Otwórz program Visual Studio.
-   **Widok-&gt; Eksplorator serwera**
-   Kliknij prawym przyciskiem myszy pozycję **połączenia danych —&gt; Dodaj połączenie...**
-   Jeśli nie masz połączenia z bazą danych Eksplorator serwera przed wybraniem **Microsoft SQL Server** jako źródła danych
-   Nawiąż połączenie z usługą LocalDB lub SQL Express, w zależności od tego, który z nich jest zainstalowany
-   Wprowadź **EntitySplitting** jako nazwę bazy danych
-   Wybierz pozycję **OK** . pojawi się monit z prośbą o utworzenie nowej bazy danych, a następnie wybierz pozycję **tak** .
-   Nowa baza danych zostanie wyświetlona w Eksplorator serwera
-   Jeśli używasz programu Visual Studio 2012
    -   Kliknij prawym przyciskiem myszy bazę danych w Eksplorator serwera a następnie wybierz pozycję **nowe zapytanie** .
    -   Skopiuj następujące SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Execute (wykonaj** ).
-   Jeśli używasz programu Visual Studio 2010
    -   Wybierz pozycję **dane —&gt; edytor języka Transact SQL —&gt; nowe połączenie zapytania...**
    -   Wprowadź **.\\SQLExpress** jako nazwę serwera, a następnie kliknij przycisk **OK** .
    -   Wybierz bazę danych **EntitySplitting** z listy rozwijanej w górnej części edytora zapytań.
    -   Skopiuj poniższy kod SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Wykonaj SQL**

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a>Tworzenie projektu

-   W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt**.
-   W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **Aplikacja konsolowa** .
-   Wprowadź **MapEntityToTablesSample** jako nazwę projektu, a następnie kliknij przycisk **OK**.
-   Kliknij przycisk **nie** , jeśli zostanie wyświetlony monit o zapisanie zapytania SQL utworzonego w pierwszej sekcji.

## <a name="create-a-model-based-on-the-database"></a>Tworzenie modelu opartego na bazie danych

-   Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element**.
-   Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.
-   W polu Nazwa pliku wprowadź **MapEntityToTablesModel. edmx** , a następnie kliknij przycisk **Dodaj**.
-   W oknie dialogowym Wybierz zawartość modelu wybierz pozycję **Generuj z bazy danych**, a następnie kliknij przycisk **Dalej.**
-   Wybierz połączenie **EntitySplitting** z listy rozwijanej i kliknij przycisk **dalej**.
-   W oknie dialogowym Wybierz obiekty bazy danych zaznacz pole wyboru obok pozycji **tabele** węźle.
    Spowoduje to dodanie wszystkich tabel z bazy danych **EntitySplitting** do modelu.
-   Kliknij przycisk **Zakończ**.

Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu.

## <a name="map-an-entity-to-two-tables"></a>Mapowanie jednostki na dwie tabele

W tym kroku będziemy aktualizować typ podmiotu **osoby** , aby łączyć dane z **osób** i tabel **PersonInfo** .

-   Wybierz  **e-mail** i właściwości **telefonu** jednostki **PersonInfo **i naciśnij klawisze **Ctrl + X** .
-   Wybierz jednostkę **osoby **i naciśnij klawisze **Ctrl + V** .
-   Na powierzchni projektowej wybierz jednostkę **PersonInfo** i naciśnij przycisk **Usuń** na klawiaturze.
-   Kliknij przycisk **nie** , gdy zostanie wyświetlony monit o usunięcie tabeli **PersonInfo** z modelu, zamierzamy zamapować ją na jednostkę **osoby** .

    ![Usuń tabele](~/ef6/media/deletetables.png)

Następne kroki wymagają okna **szczegóły mapowania** . Jeśli nie widzisz tego okna, kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **szczegóły mapowania**.

-   Wybierz typ jednostki  **osoba** , a następnie kliknij pozycję **&lt;Dodaj tabelę lub widok&gt;**  w oknie **szczegóły mapowania** .
-   Z listy rozwijanej wybierz pozycję **PersonInfo ** .
    Okno **szczegóły mapowania** zostało zaktualizowane z domyślnymi mapowaniami kolumn, ale są one bardzo szczegółowe w naszym scenariuszu.

Typ jednostki  **osoby** jest teraz mapowany do tabel **osoby** i **PersonInfo** .

![Mapowanie 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a>Korzystanie z modelu

-   Wklej następujący kod w metodzie Main.

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   Skompiluj i uruchom aplikację.

Następujące instrukcje języka T-SQL zostały wykonane względem bazy danych w wyniku uruchomienia tej aplikacji. 

-   Dwie poniższe instrukcje **INSERT** zostały wykonane w wyniku wykonywania kontekstu. Metody SaveChanges (). Pobierają one dane z podmiotu **osoby** i dzielą je między **osoby** i tabele **PersonInfo** .

    ![Wstaw 1](~/ef6/media/insert1.png)

    ![Wstaw 2](~/ef6/media/insert2.png)
-   Następujący **wybór** został wykonany w wyniku wyliczenia osób w bazie danych. Łączy dane od **osoby** i tabeli **PersonInfo** .

    ![Wybierz pozycję](~/ef6/media/select.png)
