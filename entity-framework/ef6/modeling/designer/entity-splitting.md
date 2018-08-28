---
title: Dzielenie projektanta jednostek — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: 214561f0a0381bced3ceae0b6acfcd45f5dd65c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995622"
---
# <a name="designer-entity-splitting"></a>Projektant jednostki dzielenia
Ten poradnik pokazuje jak do mapowania typu encji dwie tabele, modyfikując model przy użyciu narzędzia Entity Framework Designer (Projektant EF). Jednostki można zamapować na wiele tabel, tabele udostępniania wspólny klucz. Pojęcia, które są stosowane do mapowania typu jednostki z dwiema tabelami są łatwo rozszerzyć mapowania typu jednostki w więcej niż dwie tabele.

Na poniższej ilustracji przedstawiono główne systemu windows, które są używane podczas pracy z projektancie platformy EF.

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Wymagania wstępne

Program Visual Studio 2012 lub Visual Studio 2010, Ultimate, Premium, Professional lub Web Express edition.

## <a name="create-the-database"></a>Tworzenie bazy danych

Serwer bazy danych, który został zainstalowany przy użyciu programu Visual Studio różni się zależnie od wersji programu Visual Studio zostały zainstalowane:

-   Jeśli używasz programu Visual Studio 2012, a następnie będzie utworzenie bazy danych LocalDB.
-   Jeśli używasz programu Visual Studio 2010 zostanie utworzona baza danych programu SQL Express.

Najpierw utworzymy bazę danych z dwoma tabelami, które będziemy połączyć do pojedynczej jednostki.

-   Otwórz program Visual Studio
-   **Widok —&gt; Eksploratora serwera**
-   Kliknij prawym przyciskiem myszy **połączeń danych -&gt; Dodaj połączenie...**
-   Jeśli nie jest połączona z bazą danych za pomocą Eksploratora serwera, zanim będzie konieczne wybranie **programu Microsoft SQL Server** jako źródło danych
-   Łączenie się z LocalDB lub SQL Express, w zależności od tego, który z nich został zainstalowany
-   Wprowadź **EntitySplitting** jako nazwa bazy danych
-   Wybierz **OK** i uzyskasz, jeśli chcesz utworzyć nową bazę danych, wybierz opcję **tak**
-   Nowa baza danych będzie teraz wyświetlany w Eksploratorze serwera
-   Jeśli używasz programu Visual Studio 2012
    -   Kliknij prawym przyciskiem myszy w bazie danych w Eksploratorze serwera i wybierz **nowe zapytanie**
    -   Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonania**
-   Jeśli używasz programu Visual Studio 2010
    -   Wybierz **dane —&gt; języka Transact SQL edytor —&gt; nowego połączenia zapytania...**
    -   Wprowadź **.\\ SQLEXPRESS** jako nazwę serwera i kliknij przycisk **OK**
    -   Wybierz **EntitySplitting** bazy danych z listy rozwijanej w górnej części edytora zapytań
    -   Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonaj instrukcję SQL**

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

-   Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**.
-   W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **aplikację Konsolową** szablonu.
-   Wprowadź **MapEntityToTablesSample** jako nazwę projektu i kliknij przycisk **OK**.
-   Kliknij przycisk **nie** Jeśli zostanie wyświetlony monit, aby zapisać zapytanie SQL utworzonego w pierwszej sekcji.

## <a name="create-a-model-based-on-the-database"></a>Tworzenie modelu na podstawie bazy danych

-   Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wskaż opcję **Dodaj**, a następnie kliknij przycisk **nowy element**.
-   Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów.
-   Wprowadź **MapEntityToTablesModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**.
-   W oknie dialogowym Wybierz zawartość modelu, wybierz **Generuj z bazy danych**, a następnie kliknij przycisk **dalej.**
-   Wybierz **EntitySplitting** połączenia z listy w dół i kliknij przycisk **dalej**.
-   W oknie dialogowym Wybierz obiekty bazy danych, zaznacz pole wyboru obok pozycji **tabel** węzła.
    Spowoduje to dodanie wszystkich tabel z **EntitySplitting** bazy danych do modelu.
-   Kliknij przycisk **Zakończ**.

Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu.

## <a name="map-an-entity-to-two-tables"></a>Mapuj jednostki z dwiema tabelami

W tym kroku zaktualizujemy **osoby** typu jednostki, łączyć dane z **osoby** i **PersonInfo** tabel.

-   Wybierz **E-mail** i **Phone** właściwości ** PersonInfo ** jednostki, a następnie naciśnij klawisz **Ctrl + X** kluczy.
-   Wybierz pozycję ** osoby ** jednostki, a następnie naciśnij klawisz **klawisze Ctrl + V** kluczy.
-   Na powierzchni projektowej wybierz **PersonInfo** jednostki, a następnie naciśnij klawisz **Usuń** przycisku na klawiaturze.
-   Kliknij przycisk **nie** po wyświetleniu monitu, jeśli chcesz usunąć **PersonInfo** tabelę z modelu, firma Microsoft zamiar zamapować na **osoby** jednostki.

    ![DeleteTables](~/ef6/media/deletetables.png)

Następne kroki wymagają **szczegóły mapowania** okna. Jeśli nie widzisz tego okna, kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **szczegóły mapowania**.

-   Wybierz **osoby** typu jednostki i kliknij przycisk **&lt;Dodaj tabelę lub widok&gt;** w **szczegóły mapowania** okna.
-   Wybierz pozycję ** PersonInfo ** z listy rozwijanej.
    **Szczegóły mapowania** okno zostanie zaktualizowana przy użyciu domyślnego mapowania kolumn, są w dobrym stanie w naszym scenariuszu.

**Osoby** typ jednostki jest obecnie zmapowane do **osoby** i **PersonInfo** tabel.

![Mapping2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a>Użyj modelu

-   Wklej następujący kod dla metody Main.

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

-   Skompilować i uruchomić aplikację.

Poniższe instrukcje języka T-SQL były wykonywane w bazie danych, w wyniku działania tej aplikacji. 

-   Następujące dwa **Wstaw** instrukcje zostały wykonane w wyniku wykonania z kontekstu. SaveChanges(). Przyjmują, dane z **osoby** jednostki i podziel go między **osoby** i **PersonInfo** tabel.

    ![Insert1](~/ef6/media/insert1.png)

    ![Insert2](~/ef6/media/insert2.png)
-   Następujące **wybierz** zostało wykonane w wyniku wyliczanie osób w bazie danych. Łączy ona dane z **osoby** i **PersonInfo** tabeli.

    ![Wybierz](~/ef6/media/select.png)
