---
title: Zagadnienia dotyczące wydajności dla EF4, EF5 i EF6
author: divega
ms.date: 2016-10-23
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: f71a13ec06ad46259b3f33216367723b53314a5c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996751"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a>Zagadnienia dotyczące wydajności na platformie EF, 4, 5 i 6
David Obando, Eric Dettinger i inne osoby

Data publikacji: Kwietnia 2012

Ostatnia aktualizacja: maj 2014

------------------------------------------------------------------------

## <a name="1-introduction"></a>1. Wprowadzenie

Mapowania obiektowo-relacyjny struktury to wygodny sposób zapewnienia klasą abstrakcyjną dla dostępu do danych w aplikacji, zorientowane obiektowo. W przypadku aplikacji .NET zalecaną przez firmę Microsoft jest Obiektowo Entity Framework. Wszelkie abstrakcji jednak wydajności mogą stać się problemem.

Ten oficjalny dokument został zapisany do wyświetlenia zagadnienia związane z wydajnością podczas tworzenia aplikacji przy użyciu platformy Entity Framework, aby zaoferować deweloperom pomysł wewnętrzne algorytmy Entity Framework, które mogą wpłynąć na wydajność i zapewnienie porady dotyczące badania i poprawa wydajności w aplikacjach korzystających z programu Entity Framework. Istnieje wiele dobrych tematy dotyczące wydajności już dostępne w sieci web, a także staraliśmy, wskazując do tych zasobów, jeśli jest to możliwe.

Wydajność jest trudne tematu. Ten oficjalny dokument jest przeznaczony jako zasób ułatwiające wprowadzeniu wydajności związane z decyzje dotyczące aplikacji korzystających z programu Entity Framework. Wprowadzono niektóre metryki testu, aby zademonstrować wydajność, ale te metryki nie są przeznaczone jako bezwzględne wskaźników wydajności, które będą wyświetlane w aplikacji.

Ze względów praktycznych w tym dokumencie przyjęto założenie, Entity Framework 4 jest uruchamiany program .NET 4.0 i Entity Framework 5 i 6 są uruchamiane w ramach platformy .NET 4.5. Wiele ulepszeń wydajności dla programu Entity Framework 5 znajdują się w podstawowe składniki, które są dostarczane za pomocą platformy .NET 4.5.

Entity Framework 6 jest poza pasmem wersji i nie zależy od składników platformy Entity Framework, które są dostarczane za pomocą platformy .NET. Entity Framework 6 działają zarówno w przypadku programu .NET 4.0, jak i .NET 4.5 i zaoferować korzyść duża wydajność dla użytkowników, którzy jeszcze nie uaktualniono z programu .NET 4.0, ale ma najnowsze elementy platformy Entity Framework w aplikacjach. Gdy ten dokument jest wspomniany Entity Framework 6, odwołuje się do najnowszej wersji, dostępnym w momencie pisania tego dokumentu: wersja 6.1.0.

## <a name="2-cold-vs-warm-query-execution"></a>2. Zimnych programu vs. Wykonywanie zapytania bez wyłączania zasilania

Podczas pierwszego dowolne zapytanie jest wykonywane względem danego modelu Entity Framework jest dużo pracy w tle, aby załadować i sprawdzania poprawności modelu. Często nazywamy to pierwsze zapytanie jako zapytanie "zimnymi".  Dodatkowe zapytania względem modelu już załadowana są określane jako "ciepłych" zapytań i jest znacznie szybsze.

Teraz możesz ogólny widok, w której jest zużywany czas podczas wykonywania zapytania za pomocą programu Entity Framework i zobacz, gdzie poprawiamy rzeczy w Entity Framework 6.

**Pierwsze wykonanie zapytania — zimnych zapytania**

| Zapisy użytkownika kodu                                                                                     | Akcja                    | EF4 Wpływ na wydajność                                                                                                                                                                                                                                                                                                                                                                                                        | EF5 Wpływ na wydajność                                                                                                                                                                                                                                                                                                                                                                                                                                                    | EF6 Wpływ na wydajność                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Tworzenie kontekstu          | Średni                                                                                                                                                                                                                                                                                                                                                                                                                        | Średni                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Niska                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Tworzenie wyrażenia kwerendy | Niska                                                                                                                                                                                                                                                                                                                                                                                                                           | Niska                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Niska                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | Wykonywanie zapytania LINQ      | -Ładowanie metadanych: wysoki, ale pamięci podręcznej <br/> — Wyświetlanie generacji: potencjalnie bardzo wysoka, ale pamięci podręcznej <br/> -Parametr oceny: średni <br/> -Zapytania tłumaczenia: średni <br/> -Generowanie materializer: średnie, ale pamięci podręcznej <br/> — Wykonywanie kwerend bazy danych: potencjalnie dużego <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materializacja obiektu: średni <br/> -Wyszukiwanie identity: średni | -Ładowanie metadanych: wysoki, ale pamięci podręcznej <br/> — Wyświetlanie generacji: potencjalnie bardzo wysoka, ale pamięci podręcznej <br/> -Parametr oceny: Niski <br/> -Zapytania tłumaczenia: średnie, ale pamięci podręcznej <br/> -Generowanie materializer: średnie, ale pamięci podręcznej <br/> — Wykonywanie kwerend bazy danych: potencjalnie dużego (lepsze zapytania w niektórych sytuacjach) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materializacja obiektu: średni <br/> -Wyszukiwanie identity: średni | -Ładowanie metadanych: wysoki, ale pamięci podręcznej <br/> — Wyświetlanie generacji: średnie, ale pamięci podręcznej <br/> -Parametr oceny: Niski <br/> -Zapytania tłumaczenia: średnie, ale pamięci podręcznej <br/> -Generowanie materializer: średnie, ale pamięci podręcznej <br/> — Wykonywanie kwerend bazy danych: potencjalnie dużego (lepsze zapytania w niektórych sytuacjach) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materializacja obiektu: średni (szybciej niż EF5) <br/> -Wyszukiwanie identity: średni |
| `}`                                                                                                  | Connection.Close          | Niska                                                                                                                                                                                                                                                                                                                                                                                                                           | Niska                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Niska                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


**Drugiego wykonywania zapytania — zapytania bez wyłączania zasilania**

| Zapisy użytkownika kodu                                                                                     | Akcja                    | EF4 Wpływ na wydajność                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | EF5 Wpływ na wydajność                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | EF6 Wpływ na wydajność                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Tworzenie kontekstu          | Średni                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Średni                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Niska                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Tworzenie wyrażenia kwerendy | Niska                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Niska                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Niska                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | Wykonywanie zapytania LINQ      | -Metadanych ~~ładowania~~ wyszukiwania: ~~wysoka, ale pamięci podręcznej~~ niski <br/> — Wyświetlanie ~~generowania~~ wyszukiwania: ~~potencjalnie bardzo wysoka, lecz buforowane~~ niski <br/> -Parametr oceny: średni <br/> -Zapytania ~~tłumaczenia~~ wyszukiwania: średni <br/> -Materializer ~~generowania~~ wyszukiwania: ~~średnie, ale pamięci podręcznej~~ niski <br/> — Wykonywanie kwerend bazy danych: potencjalnie dużego <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materializacja obiektu: średni <br/> -Wyszukiwanie identity: średni | -Metadanych ~~ładowania~~ wyszukiwania: ~~wysoka, ale pamięci podręcznej~~ niski <br/> — Wyświetlanie ~~generowania~~ wyszukiwania: ~~potencjalnie bardzo wysoka, lecz buforowane~~ niski <br/> -Parametr oceny: Niski <br/> -Zapytania ~~tłumaczenia~~ wyszukiwania: ~~średnie, ale pamięci podręcznej~~ niski <br/> -Materializer ~~generowania~~ wyszukiwania: ~~średnie, ale pamięci podręcznej~~ niski <br/> — Wykonywanie kwerend bazy danych: potencjalnie dużego (lepsze zapytania w niektórych sytuacjach) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materializacja obiektu: średni <br/> -Wyszukiwanie identity: średni | -Metadanych ~~ładowania~~ wyszukiwania: ~~wysoka, ale pamięci podręcznej~~ niski <br/> — Wyświetlanie ~~generowania~~ wyszukiwania: ~~średnie, ale pamięci podręcznej~~ niski <br/> -Parametr oceny: Niski <br/> -Zapytania ~~tłumaczenia~~ wyszukiwania: ~~średnie, ale pamięci podręcznej~~ niski <br/> -Materializer ~~generowania~~ wyszukiwania: ~~średnie, ale pamięci podręcznej~~ niski <br/> — Wykonywanie kwerend bazy danych: potencjalnie dużego (lepsze zapytania w niektórych sytuacjach) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materializacja obiektu: średni (szybciej niż EF5) <br/> -Wyszukiwanie identity: średni |
| `}`                                                                                                  | Connection.Close          | Niska                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Niska                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Niska                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


Istnieje kilka sposobów, aby zmniejszyć koszt wydajności zapytań, ciepło i zimno, a firma Microsoft będzie Przyjrzyj się one w poniższej sekcji. W szczególności Zapoznamy się obniżyć koszty ładowania zimnych zapytań przy użyciu wstępnie wygenerowanych widoków, które należy zmniejszyć wydajność problemy doświadczenie podczas generowania widoku modelu. Dla zapytań bez wyłączania zasilania omówimy, buforowanie planu zapytania, nie śledzenia zapytań i opcje wykonanie innego zapytania.

### <a name="21-what-is-view-generation"></a>2.1 Generowanie widoku co to jest?

Aby zrozumieć, jakie widoku Generowanie jest, firma Microsoft musisz najpierw zrozumieć, co to są "Mapowanie widoków". Widoki mapowania są reprezentacji pliku wykonywalnego przekształcenia określony w mapowaniu dla każdego zestawu jednostek i skojarzenia. Wewnętrznie te widoki mapowania przybrać CQTs (canonical zapytania drzewa). Istnieją dwa rodzaje widokach mapowania:

-   Widoki kwerendę: reprezentują one to konieczne, można przejść od schematu bazy danych do modelu koncepcyjnego transformacji.
-   Aktualizowanie widoków: reprezentuje przekształcenie konieczne przechodzenie z modelu koncepcyjnego do schematu bazy danych.

Należy pamiętać, że model koncepcyjny różnić się od schematu bazy danych na różne sposoby. Na przykład jeden pojedynczej tabeli mogą służyć do przechowywania danych dla dwóch typów jednostek innej. Dziedziczenie i mapowania nietrywialnymi pełnić rolę, w złożoność widokach mapowania.

Proces przetwarzania tych widoków, na podstawie specyfikacji mapowanie to tak zwany generowania widoku. Generowanie widoku albo korzystać z miejsca dynamicznie podczas ładowania modelu lub w czasie kompilacji, używając "wstępnie wygenerowanych widoków"; te ostatnie są serializowane w postaci instrukcji języka SQL jednostki, C\# lub plik VB.

Po wygenerowaniu widoków, one również są weryfikowane. Z punktu widzenia wydajności większość koszt generowania widoku jest faktycznie weryfikacji widoków, który zapewnia, że połączenia między jednostkami sens i mają Kardynalność prawidłowe dla wszystkich obsługiwanych operacji.

Podczas wykonywania zapytania za pośrednictwem zestawu jednostek zapytania jest połączony z odpowiedniego widoku zapytania, a wynik tej kompozycji jest uruchamiany przez kompilator planu, aby utworzyć reprezentacji zapytanie, które może zrozumieć magazyn zapasowy. Dla programu SQL Server ostateczny wynik tej kompilacji będzie instrukcję języka T-SQL ZAZNACZYĆ. Po raz pierwszy wykonać aktualizację za pośrednictwem zestawu jednostek, widok aktualizacji jest uruchamiane za pomocą podobnej do przekształcania go w instrukcji DML dla docelowej bazy danych.

### <a name="22-factors-that-affect-view-generation-performance"></a>2.2 czynniki, które mają wpływ na wydajność generowania widoku

Wydajność krok generowania widoku zależy nie tylko rozmiar modelu, ale także na połączonych jak model jest. Jeśli dwie jednostki są połączone za pośrednictwem łańcuch dziedziczenia lub skojarzenia, są one określane jako podłączone. Podobnie jeśli dwie tabele są połączone za pomocą klucza obcego, są one połączone. Jak zwiększyć liczbę połączonych jednostek, jak i tabele w swoje schematy, generowanie widoku koszt zwiększa się.

Algorytmu, używanego do generowania i zweryfikować widoków jest wykładniczego w najgorszym przypadku, chociaż używamy niektóre optymalizacje tego. Największych czynniki, które wydaje się, że negatywnie wpłynąć na wydajność są następujące:

-   Rozmiar modelu odnoszące się do liczby jednostek i ilość skojarzenia między tymi jednostkami.
-   Złożoność modelu, specjalnie dziedziczenia obejmujące wiele typów.
-   Użycie niezależnych skojarzeń, zamiast skojarzeń klucza obcego.

W przypadku małych, prostych modeli kosztów może być wystarczająco mała, aby nie odblokowane za pomocą wstępnie wygenerowanych widoków. Jak zwiększyć rozmiar modelu i złożoność, dostępnych jest kilka opcji, które można zmniejszyć koszt generowania widoku i sprawdzania poprawności.

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a>2.3 widoków Pre-Generated przy użyciu modelu zmniejszyć czas ładowania

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools"></a>2.3.1 wstępnie wygenerowanych widoków za pomocą narzędzi Entity Framework Power Tools.

Możesz też rozważyć, za pomocą narzędzi Entity Framework Power Tools do generowania widoków plików EDMX i Code First modeli kliknij prawym przyciskiem myszy plik klasy modelu, a następnie wybierz pozycję "Generuj widoki" przy użyciu menu platformy Entity Framework. Entity Framework Power Tools działają tylko w kontekstach pochodzi od typu DbContext i znajduje się w temacie \< http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d>.

Aby uzyskać więcej informacji na temat korzystania z wstępnie wygenerowanych widoków w programie Entity Framework 6 odwiedź [Pre-Generated mapowanie widoków](~/ef6/fundamentals/performance/pre-generated-views.md).

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a>2.3.2 sposób używania wstępnie wygenerowanych widoków z modelem, który został utworzony przez EDMGen

EDMGen to narzędzie jest dostarczany za pomocą platformy .NET, która działa z programu Entity Framework 4 i 5, ale nie z programu Entity Framework 6. EDMGen umożliwia generowanie pliku modelu warstwy obiektu i widoków z wiersza polecenia. Jedną z danych wyjściowych będzie plik widoków w języku wybranym języku VB lub C\#. Jest to plik kodu zawierający fragmenty jednostki SQL dla każdego zestawu jednostek. Aby włączyć wstępnie wygenerowanych widoków, po prostu Dołącz plik w projekcie.

Jeśli ręcznie wprowadzić zmiany plików schematów dla modelu, należy ponownie wygenerować plik widoków. Można to zrobić, uruchamiając EDMGen z **/mode:ViewGeneration** flagi.

Aby dalsze informacje, zobacz [jak: Pre-Generate widoków, aby poprawić wydajność zapytań](https://msdn.microsoft.com/library/bb896240.aspx).

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a>2.3.3 jak widoków Pre-Generated za pomocą pliku EDMX

Można również użyć EDMGen można wygenerować widoków dla pliku EDMX — wcześniej odwołania temacie w witrynie MSDN zawiera opis sposobu dodawania Zdarzenie sprzed kompilacji, w tym -, ale jest to skomplikowane i istnieją przypadki, gdy nie jest możliwe. Ogólnie łatwiej szablon T4 umożliwia generowanie widoków, gdy model znajduje się w pliku edmx.

Blog zespołu programu ADO.NET ma wpis, który opisuje sposób używania szablon T4 do generowania widoku ( \< http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>). Ten wpis zawiera szablon, który może być pobrane i dodane do projektu. Szablon został napisany dla pierwszej wersji programu Entity Framework, więc one nie są gwarantowane najnowsze wersje platformy Entity Framework. Jednakże można pobrać bardziej aktualny zestaw szablonów generowania widoku Entity Framework 4 i 5from galerii Visual Studio:

-   VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d>
-   C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d>

Jeśli używasz platformy Entity Framework 6 można uzyskać widok szablony T4 generacji z galerii Visual Studio na \< http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.

#### <a name="234-how-to-use-pre-generated-views-with-a-code-first-model"></a>2.3.4 jak używać widoków Pre-Generated model Code First

Istnieje również możliwość użycia wstępnie wygenerowanych widoków z projektem Code First. Entity Framework Power Tools ma możliwość generowania pliku widoków w projekcie Code First. Entity Framework Power Tools można znaleźć w galerii Visual Studio na \< http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d/>.

### <a name="24-reducing-the-cost-of-view-generation"></a>2.4 obniżyć koszty generowania widoku

Za pomocą wstępnie wygenerowanych widoków przenosi koszt generowania widoku ładowania modelu (czas wykonywania) czas kompilacji. Gdy poprawia to wydajność uruchamiania w środowisku uruchomieniowym, będzie nadal występować ból generowania widoku podczas programowania. Istnieje kilka dodatkowe wskazówki, które mogą pomóc zmniejszyć koszt generowania widoku, zarówno w czasie kompilacji i w czasie wykonywania.

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a>2.4.1 za pomocą skojarzeń klucza obcego, aby zmniejszyć koszt generowania widoku

Zaobserwowaliśmy liczba przypadków, w której przełączanie skojarzeń w modelu z niezależnym skojarzenia obcego skojarzenia klucza znacznie ulepszona czas potrzebny do generowania widoku.

Aby zademonstrować to ulepszenie, firma Microsoft generowane dwie wersje modelu Navision przy użyciu EDMGen. *Uwaga: seeappendix Cfor opis modelu Navision.* Navision model jest interesujące dla tego ćwiczenia z powodu ich dużej ilości jednostek i relacji między nimi.

Jedną wersję tego modelu bardzo dużych został wygenerowany z użyciem obcego skojarzenia kluczy i innych został wygenerowany z użyciem niezależnych skojarzenia. Następnie timed się, jak długo można wygenerować widoków dla każdego modelu. Jednostki Framework5 test używał GenerateViews() metody z klasy EntityViewGenerator, można wygenerować widoków, podczas testu Entity Framework 6 GenerateViews() metody z klasy obiekt StorageMappingItemCollection. To z powodu restrukturyzacji kod, który wystąpił w bazie kodu podlegającej Entity Framework 6.

Za pomocą programu Entity Framework 5, widok generacji dla modelu przy użyciu kluczy obcych trwało 65 minut w komputerze laboratoryjnym. Wiadomo jak długo zajęłoby do generowania widoków dla modelu, który używane niezależnie od skojarzenia. Pozostawiliśmy test uruchomiony w ciągu miesiąca, zanim komputer został ponownie uruchomiony w nasze laboratorium, aby zainstalować comiesięcznych aktualizacji.

Widok generacji dla modelu przy użyciu kluczy obcych przy użyciu platformy Entity Framework 6, zajęło 28 sekundach w tym samym komputerze laboratoryjnym. Widok generacji dla modelu, który używa niezależnych skojarzeń zajęło s 58. Ulepszenia jej kodu generowania widoku poświęconej Entity Framework 6 oznaczają, że w przypadku wielu projektów nie będzie już konieczne wstępnie wygenerowanych widoków, aby uzyskać krótszy czas uruchamiania.

Jest to ważne uwagi, który wstępnie generowanie widoków w programie Entity Framework 4 i 5 może odbywać się przy użyciu EDMGen lub narzędzi Entity Framework Power Tools. Entity Framework 6 widoku generowania może odbywać się za pomocą narzędzi Entity Framework Power Tools lub programowo, zgodnie z opisem w [Pre-Generated mapowanie widoków](~/ef6/fundamentals/performance/pre-generated-views.md).

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a>2.4.1.1 jak do używania kluczy obcych zamiast niezależnych skojarzenia

Korzystając z EDMGen lub Projektant jednostki w programie Visual Studio, otrzymasz FKs domyślnie, ale zajmuje tylko OK pojedynczej flagi pola wyboru lub wiersza polecenia można przełączać się między FKs i IAs.

W przypadku dużych model Code First przy użyciu niezależnych skojarzenia mają ten sam efekt na generowania widoku. Możesz uniknąć wpływ przez dołączenie klucza obcego właściwości klasy dla obiektów zależnych, chociaż niektórzy deweloperzy będą należy wziąć pod uwagę ten element, aby być zanieczyszczenie ich modelu obiektów. Można znaleźć więcej informacji na ten temat w \< http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.

| Korzystając z      | Zrób to                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Projektant ekranu | Po dodaniu skojarzenia między dwiema jednostkami, upewnij się, że masz ograniczenia referencyjnego. Ograniczenia referencyjne Poinformuj Entity Framework do używania kluczy obcych zamiast niezależnych skojarzenia. Aby uzyskać więcej informacji, odwiedź stronę \< http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>. |
| EDMGen          | Generowanie plików z bazy danych za pomocą EDMGen, klucze obce będą przestrzegane i dodana do modelu, w związku z tym. Aby uzyskać więcej informacji na temat różnych opcji udostępnianych przez EDMGen odwiedź stronę [ http://msdn.microsoft.com/library/bb387165.aspx ](https://msdn.microsoft.com/library/bb387165.aspx).                           |
| Najpierw kod      | Zobacz sekcję "Konwencji relacji" [pierwszy konwencje związane z](~/ef6/modeling/code-first/conventions/built-in.md) zawiera informacje dotyczące sposobu uwzględniania właściwości klucza obcego na obiekty zależne, gdy za pomocą funkcji Code First.                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a>2.4.2 przenoszenie modelu w osobnym zestawie

Gdy model znajduje się bezpośrednio w projekcie aplikacji i generowanie widoków przez zdarzenie sprzed kompilacji lub szablon T4, generowania widoku i sprawdzanie poprawności nastąpi zawsze wtedy, gdy projekt zostanie ponownie skompilowany, nawet jeśli nie można zmienić modelu. Jeśli przenoszenie modelu w osobnym zestawie i odwoływać się do niego z projektu aplikacji, można zapisać wprowadzić inne zmiany aplikacji bez konieczności ponownie skompiluj projekt zawierający modelu.

*Uwaga:* podczas przenoszenia modelu do oddzielnych zestawów Pamiętaj o skopiowaniu parametrów połączenia dla modelu w pliku konfiguracyjnym aplikacji projektu klienta.

#### <a name="243-disable-validation-of-an-edmx-based-model"></a>2.4.3 wyłączyć sprawdzanie poprawności modelu opartego na edmx

Modele EDMX są weryfikowane w czasie kompilacji, nawet jeśli model jest bez zmian. Jeśli model została już zweryfikowana, można pominąć sprawdzanie poprawności w czasie kompilacji przez ustawienie właściwości "Sprawdzanie poprawności w kompilacji" na wartość false w oknie dialogowym właściwości. Po zmianie z mapowania lub modelu, można tymczasowo ponownie włączyć sprawdzanie poprawności, aby zweryfikować zmiany.

Należy pamiętać, że wprowadzono ulepszenia wydajności do programu Entity Framework Designer dla programu Entity Framework 6 i koszt "Sprawdzanie poprawności w kompilacji" jest znacznie niższa niż w poprzednich wersjach projektanta.

## <a name="3-caching-in-the-entity-framework"></a>3 buforowania w programie Entity Framework

Entity Framework zawiera następujące rodzaje wbudowanej w pamięci podręcznej:

1.  Obiekt z pamięci podręcznej — obiekt ObjectStateManager wbudowaną wystąpienie obiektu ObjectContext śledzi w pamięci obiektów, które zostały pobrane przy użyciu tego wystąpienia. To jest również nazywany pierwszego poziomu w pamięci podręcznej.
2.  Buforowanie planu zapytania — ponowne użycie polecenia magazynu wygenerowany, gdy zapytanie jest wykonywane więcej niż jeden raz.
3.  Metadane w pamięci podręcznej — Udostępnianie metadanych dla modelu w różnych połączeń do tego samego modelu.

Oprócz pamięci podręczne, które EF zapewnia gotowych specjalny rodzaj dostawcy danych ADO.NET, znane jako dostawcy opakowujące aplikacje można również rozszerzyć Entity Framework z pamięcią podręczną zawiera wyniki pobrane z bazy danych, nazywany również pamięć podręczna drugiego poziomu.

### <a name="31-object-caching"></a>3.1 obiektu pamięci podręcznej

Domyślnie gdy jednostka jest zwracany w wynikach zapytania, przed EF materializuje, Obiekt ObjectContext sprawdzi, jeśli jednostki z tym samym kluczu został już załadowany w jego obiekcie ObjectStateManager. Jeśli jednostki z tych samych kluczy znajduje się już EF będzie dołączyć wyniki zapytania. Mimo że EF nadal będzie wystawiać zapytań w bazie danych, to zachowanie można pominąć większość koszt materializowanie jednostki wiele razy.

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a>3.1.1 pobieranie jednostek z pamięci podręcznej obiektów korzystania z funkcji znajdowania typu DbContext

W przeciwieństwie do regularnych zapytania metody Find w DbSet (interfejsy API uwzględnione po raz pierwszy w EF 4.1) będzie wykonywać wyszukiwania w pamięci przed wystawieniem nawet zapytanie w bazie danych. Należy zauważyć, że dwa różne wystąpienia obiektu ObjectContext dwóch różnych wystąpień obiektu ObjectStateManager, co oznacza, do których mają oddzielny obiekt w pamięci podręcznej.

Znajdź używa wartość klucza podstawowego do podejmą próbę odnalezienia śledzone przez kontekst jednostki. Jeśli jednostki nie znajduje się w kontekście następnie wykonywane i oceniane w bazie danych zapytania i zwracana jest wartość null, jeśli jednostka nie zostanie odnaleziona w kontekście lub w bazie danych. Należy pamiętać, że znajdowanie zwraca również wartość jednostek, które zostały dodane do kontekstu, ale nie zostały zapisane w bazie danych.

Brak jest brany pod uwagę wydajności do wykonania podczas korzystania z funkcji znajdowania. Wywołania do tej metody, domyślnie wyzwoli weryfikacji obiektu pamięci podręcznej w celu wykrycia zmian, które wciąż oczekują na zatwierdzenie w bazie danych. Ten proces może zająć bardzo kosztowny w przypadku bardzo dużej liczby obiektów w pamięci podręcznej obiektów lub wykresie dużego obiektu dodawane do pamięci podręcznej obiektów, ale można również zostaną wyłączone. W niektórych przypadkach mogą postrzegać przez rząd wielkości różnicy podczas wywoływania Znajdź metodę po wyłączeniu automatycznego wykrywania zmian. Jeszcze drugi rząd wielkości jest traktowany, gdy obiekt jest rzeczywiście w pamięci podręcznej, a gdy obiekt ma być pobierane z bazy danych. Oto przykładowy Graf za pomocą pomiarów dokonanych przy użyciu niektóre z naszych microbenchmarks wyrażony w milisekundach, wynosi 5000 jednostek:

![Net45LogScale](~/ef6/media/net45logscale.png ".NET 4.5 - skali logarytmicznej")

Przykład Znajdź ze zmianami auto-detect wyłączone:

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

Co należy wziąć pod uwagę podczas korzystania z metody Znajdź jest:

1.  Jeśli obiekt nie jest w pamięci podręcznej z zalet wyszukiwania jest ujemna, ale składnia jest nadal jest prostsze niż zapytania według klucza.
2.  Jeśli automatyczne wykrywanie zmian jest włączona może zwiększyć koszt metody Find, co o rząd wielkości i jeszcze bardziej w zależności od złożoności modelu i ilość jednostek w pamięci podręcznej obiektu.

Ponadto należy pamiętać, że znaleźć tylko zwraca obiekt, do którego szukasz, i go nie automatycznie ładowania jego skojarzone jednostki, jeśli nie są jeszcze w pamięci podręcznej obiektów. Jeśli musisz pobrać skojarzone jednostki można użyć zapytania według klucza przy użyciu wczesne ładowanie. Aby uzyskać więcej informacji, zobacz **8.1 powolne ładowanie programu vs. Wczesne ładowanie**.

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a>3.1.2 problemy z wydajnością, gdy pamięć podręczna obiekt zawiera wiele jednostek

Obiektu pamięci podręcznej pomaga zwiększyć ogólną szybkość reakcji Entity Framework. Jednak po pamięci podręcznej obiektów zawiera bardzo dużą ilość jednostek załadowane, który może wpływać na niektórych operacji, takich jak dodawanie, usuwanie, znajdź wpis, SaveChanges i wiele innych. W szczególności operacje, które wyzwala wywołanie metody DetectChanges będzie negatywny wpływ bardzo dużych obiektów w pamięci podręcznej. Metody DetectChanges synchronizuje wykres obiektu z obiektu state manager i spowoduje jego wydajności, określany bezpośrednio przez rozmiar wykresu obiektu. Aby uzyskać więcej informacji na temat metody DetectChanges zobacz [śledzenie zmian w jednostkach obiektów POCO](https://msdn.microsoft.com/library/dd456848.aspx).

Korzystając z platformy Entity Framework 6, deweloperzy mają możliwość wywoływania wywoływania metody AddRange i RemoveRange bezpośrednio na DbSet, zamiast Iterowanie w kolekcji i wywoływania Dodaj jeden raz dla każdego wystąpienia. Zaletą używania metody range jest koszt metody DetectChanges tylko raz płatnych dla całego zestawu jednostek, a nie raz dla każdej jednostki dodano.

### <a name="32-query-plan-caching"></a>3.2 buforowanie planu zapytania za pomocą

Zapytanie jest wykonywane, po raz pierwszy, go przechodzi przez kompilator wewnętrzny plan do tłumaczenia koncepcyjny zapytanie na polecenia magazynu (na przykład T-SQL, który jest wykonywany po uruchomieniu testów programu SQL Server).  Jeśli włączone jest buforowanie planu zapytania, przy następnym zapytanie jest wykonywane sklepu polecenia są pobierane bezpośrednio z pamięci podręcznej planu zapytania do wykonania, z pominięciem kompilatora planu.

Pamięci podręcznej planu zapytania jest współużytkowany przez obiekt ObjectContext wystąpienia w ramach tej samej domenie aplikacji. Nie należy przechowywać na wystąpienie obiektu ObjectContext do korzystania z buforowanie planu zapytania.

#### <a name="321-some-notes-about-query-plan-caching"></a>3.2.1 kilka uwag dotyczących buforowanie planu zapytania

-   Pamięci podręcznej planu zapytania jest udostępniany dla wszystkie typy zapytań: Entity SQL, składnik LINQ to Entities i CompiledQuery obiektów.
-   Domyślnie buforowanie planu zapytania jest włączona dla zapytań jednostki SQL, czy wykonywane za pośrednictwem EntityCommand lub ObjectQuery. Jego jest również domyślnie włączone dla programu LINQ do zapytań jednostki w Entity Framework w .NET 4.5 i Entity Framework 6
    -   Buforowanie planu zapytania, może być wyłączone przez ustawienie wartości false dla właściwości EnablePlanCaching (na EntityCommand lub ObjectQuery). Na przykład:
``` csharp
                    var query = from customer in context.Customer
                                where customer.CustomerId == id
                                select new
                                {
                                    customer.CustomerId,
                                    customer.Name
                                };
                    ObjectQuery oQuery = query as ObjectQuery;
                    oQuery.EnablePlanCaching = false;
```
-   W zapytaniach parametrycznych zmiana wartości parametru nadal będzie trafień pamięci podręcznej zapytań. Jednak zmiana parametru aspektami (na przykład rozmiaru, dokładności lub skali) spowoduje osiągnięcie inny wpis w pamięci podręcznej.
-   Podczas korzystania z jednostki SQL, ciąg zapytania jest częścią klucza. Zmiana zapytanie na wszystkich spowoduje wpisy w pamięci podręcznej różne, nawet jeśli zapytania są funkcjonalnie równoważne. Obejmuje to zmiany wielkości liter lub była białym znakiem.
-   Podczas korzystania z LINQ, zapytania są przetwarzane do generowania część klucza. Zmiana wyrażenia LINQ w związku z tym wygeneruje inny klucz.
-   Inne ograniczenia techniczne mogą zastosować; Aby uzyskać więcej informacji, zobacz Autocompiled zapytania.

#### <a name="322------cache-eviction-algorithm"></a>3.2.2 algorytm eksmisji pamięci podręcznej

Zrozumienie, jak działa wewnętrznego algorytmu pomoże Ci zorientować się, aby włączyć lub wyłączyć buforowanie planu zapytania. Algorytm oczyszczania jest w następujący sposób:

1.  Gdy pamięć podręczna zawiera określona liczba wpisów (800), na początek czasomierz okresowo (jeden raz na minutę) wrzucając pamięci podręcznej.
2.  W trakcie symulacji pamięci podręcznej wpisy są usuwane z pamięci podręcznej na LFRU (najmniej często — ostatnio używane) podstawy. Ten algorytm uwzględnia liczbę trafień i wieku przy podejmowaniu decyzji, które wpisy są odrzucane.
3.  Na koniec każdego czyszczenia pamięci podręcznej pamięć podręczna zawiera ponownie 800 wpisów.

Wszystkie wpisy pamięci podręcznej są traktowani jednakowo podczas ustalania, które wpisy do wykluczenia. Oznacza to, że polecenie magazynu dla CompiledQuery ma ten sam prawdopodobieństwo eksmisji jako polecenie magazynu zapytania SQL jednostki.

Należy zauważyć, że czasomierza eksmisji pamięci podręcznej rozpocznie się w przypadku 800 jednostkami w pamięci podręcznej, ale w pamięci podręcznej tylko przechwytywana 60 sekund, po uruchomieniu tego czasomierza. Oznacza to, że do 60 sekund pamięci podręcznej może rosnąć dość duży.

#### <a name="323-------test-metrics-demonstrating-query-plan-caching-performance"></a>3.2.3 test metryki ukazujące planu zapytania, buforowanie wydajności

Aby zaprezentować efekt planu zapytania, buforowanie na wydajność aplikacji, wykonane testu których firma Microsoft wykonywane liczby zapytań SQL jednostki w modelu Navision. Zobacz dodatek opis modelu Navision i typów kwerend, które zostały wykonane. W tym teście możemy najpierw iteracji przez listę zapytań i wykonywane każdego z nich raz, aby dodać je do pamięci podręcznej (jeśli jest włączone buforowanie). Ten krok jest untimed. Następnie możemy uśpienia wątku głównego ponad 60 sekund umożliwić buforowanie sprawdzaniu została wykonana; na koniec możemy wykonać iterację czasu listy 2 do wykonywania zapytań pamięci podręcznej. Ponadto on pamięci podręcznej planu programu SQL Server jest opróżniany przed wykonaniem każdego zestawu zapytań, aby przypadków, gdy uzyskany dokładnie odzwierciedlają korzyści przez pamięć podręczną planu zapytań.

##### <a name="3231-------test-results"></a>3.2.3.1 wyniki testu

| Test                                                                   | EF5 Brak pamięci podręcznej | EF5 pamięci podręcznej | EF6 Brak pamięci podręcznej | EF6 pamięci podręcznej |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| Wyliczanie wszystkich zapytań 18723                                          | 124          | 125.4      | 124.3        | 125.3      |
| Unikanie odchylenia (tylko pierwszy 800 zapytań, niezależnie od tego, co do złożoności)  | 41.7         | 5.5        | 40.5         | 5.4        |
| Po prostu zapytania AggregatingSubtotals (178 razem — w celu uniknięcia odchylenia) | 39.5         | 4.5        | 38.1         | 4.6        |

*Cały czas w sekundach.*

Autorskie — w przypadku wykonywania wiele różnych zapytań (na przykład tworzone dynamicznie zapytań), buforowania nie pomoże, a wynikowy opróżniania pamięci podręcznej można zachować zapytań, które używającym najbardziej buforowanie planu faktycznie korzystanie z niego.

Zapytania AggregatingSubtotals są najbardziej złożonych zapytań, które przetestowaliśmy za pomocą. Zgodnie z oczekiwaniami, tym bardziej złożone jest zapytanie, więcej korzyści, zobaczysz ze buforowanie planu zapytania.

Ponieważ CompiledQuery jest naprawdę zapytania LINQ z jego planem pamięci podręcznej, porównanie CompiledQuery a równoważne zapytań jednostki SQL powinna mieć podobne wyniki. W rzeczywistości Jeśli aplikacja ma wiele zapytań jednostki SQL dynamicznych, wypełnienie pamięci podręcznej za pomocą zapytań również skutecznie spowoduje CompiledQueries "dekompilować", gdy są one opróżniane z pamięci podręcznej. W tym scenariuszu można poprawić wydajność, wyłączenie buforowania na zapytania dynamiczne, aby określić priorytety CompiledQueries. Jeszcze lepiej oczywiście, byłoby ponownego zapisywania aplikacji Używanie zapytań sparametryzowanych zamiast zapytań dynamicznych.

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a>3.3 za pomocą CompiledQuery poprawianie wydajności za pomocą zapytań LINQ

Nasze testy wykażą, że za pomocą CompiledQuery może przynieść korzyści % 7 za pośrednictwem autocompiled zapytań LINQ; oznacza to, że musisz wykonać pewne czynności 7% mniej czasu na wykonywanie kodu ze stosu Entity Framework; nie oznacza to, że Twoja aplikacja będzie 7% szybciej. Ogólnie rzecz biorąc koszt napisaniem i obsługą CompiledQuery obiektów w programie EF 5.0 może nie być warte problemy w porównaniu do korzyści. Przebieg może się różnić w, więc wykonuje tę opcję, jeśli Twój projekt wymaga dodatkowego wypychania. Należy pamiętać, że CompiledQueries tylko są zgodne z klasy pochodnej ObjectContext modeli i nie jest zgodna z modelami pochodzi od typu DbContext.

Aby uzyskać więcej informacji na temat tworzenia i wywoływania CompiledQuery, zobacz [zapytania skompilowane (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).

Istnieją dwie kwestie, które należy wykonać, korzystając z CompiledQuery, a mianowicie wymóg dotyczący używania statycznych wystąpień oraz problemy, że zawierają dzięki możliwości tworzenia. W tym miejscu poniżej szczegółowe wyjaśnienie tych dwóch zagadnień.

#### <a name="331-------use-static-compiledquery-instances"></a>3.3.1 używać statycznych wystąpień CompiledQuery

Ponieważ kompilowanie zapytania LINQ jest czasochłonne, nie chcemy zrobić to za każdym razem, gdy będziemy musieli pobierać dane z bazy danych. Wystąpienia CompiledQuery pozwalają na raz skompilować i uruchomić wiele razy, ale trzeba należy zachować ostrożność i nabywania ponownego używania tego samego wystąpienia CompiledQuery za każdym razem, zamiast wielokrotnie zestawiania. Konieczność użycia statyczne elementy członkowskie do przechowywania wystąpień CompiledQuery; w przeciwnym razie nie będziesz widzieć żadnych korzyści.

Na przykład załóżmy, że Twoja strona zawiera następujące treści metody do obsługi wyświetlania produktów dla wybranej kategorii:

``` csharp
    // Warning: this is the wrong way of using CompiledQuery
    using (NorthwindEntities context = new NorthwindEntities())
    {
        string selectedCategory = this.categoriesList.SelectedValue;

        var productsForCategory = CompiledQuery.Compile<NorthwindEntities, string, IQueryable<Product>>(
            (NorthwindEntities nwnd, string category) =>
                nwnd.Products.Where(p => p.Category.CategoryName == category)
        );

        this.productsGrid.DataSource = productsForCategory.Invoke(context, selectedCategory).ToList();
        this.productsGrid.DataBind();
    }

    this.productsGrid.Visible = true;
```

W takim przypadku utworzysz nowe wystąpienie CompiledQuery na bieżąco za każdym razem, gdy metoda jest wywoływana. Zamiast zobaczyć korzyści wydajności, pobierając polecenie magazynu z pamięci podręcznej planu zapytania, CompiledQuery będzie przejście przez kompilator planu, za każdym razem, gdy tworzone jest nowe wystąpienie. W rzeczywistości można będzie mieć zanieczyszczenie pamięci podręcznej planu zapytania z nowym wpisem CompiledQuery za każdym razem, gdy metoda jest wywoływana.

Zamiast tego chcesz utworzyć wystąpienie statyczne kompilowanym zapytaniu, więc wywoływane tego samego zapytania skompilowane za każdym razem, gdy metoda jest wywoływana. Jednym ze sposobów to przez dodanie wystąpienia CompiledQuery jako członek kontekstu obiektów.  Następnie można wprowadzić rzeczy nieco testu czyszczenia po zalogowaniu się za pośrednictwem metody pomocnika do CompiledQuery:

``` csharp
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IEnumerable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
            );

        public IEnumerable<Product> GetProductsForCategory(string categoryName)
        {
            return productsForCategoryCQ.Invoke(this, categoryName).ToList();
        }
```

Ta metoda pomocnika będzie można wywołać w następujący sposób:

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-------composing-over-a-compiledquery"></a>3.3.2 redagowania za pośrednictwem CompiledQuery

Możliwość tworzenia za pośrednictwem dowolnego zapytania LINQ jest niezwykle przydatna funkcja; Aby to zrobić, możesz po prostu wywołać metodę po element IQueryable takich jak *Skip()* lub *Count()*. Jednak sposób więc zasadniczo zwraca nowy obiekt IQueryable. Nie ma nic do uniemożliwić Ci z technicznego punktu widzenia redagowania za pośrednictwem CompiledQuery, w ten sposób spowoduje, że Generowanie nowego obiektu IQueryable, wymaga przechodzącego przez kompilator planu ponownie.

Spowoduje, że niektóre składniki użytkowania IQueryable złożone obiekty, aby włączyć zaawansowane funkcje. Na przykład, ASP. GridView NET firmy może być powiązane z danymi do obiektu IQueryable za pomocą właściwości metody SelectMethod. Kontrolki GridView zostanie następnie tworzą za pośrednictwem tego obiektu IQueryable umożliwia sortowanie i stronicowanie za pośrednictwem modelu danych. Jak widać, za pomocą CompiledQuery dla widoku GridView nie osiągnie kompilowanym zapytaniu, ale wygeneruje nowe zapytanie autocompiled.

Zespół Doradczy klientów to omówiono w nim ich "Potencjalnych wydajności problemy z skompilowany LINQ zapytanie ponownie kompiluje" w blogu: <http://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/potential-performance-issues-with-compiled-linq-query-re-compiles.aspx>.

Jedno miejsce, gdzie może wystąpić ten jest podczas dodawania filtrów stopniowego do zapytania. Na przykład załóżmy, że masz strony klienci z kilku list rozwijanych opcjonalne filtry (na przykład, kraj i OrdersCount). Filtry te można utworzyć za pośrednictwem wyników IQueryable CompiledQuery, ale takie działanie spowoduje w nowym zapytaniu przechodzenia przez kompilator planu, za każdym razem, aby uruchomić go.

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployee();

        if (this.orderCountFilterList.SelectedItem.Value != defaultFilterText)
        {
            int orderCount = int.Parse(orderCountFilterList.SelectedValue);
            myCustomers = myCustomers.Where(c => c.Orders.Count > orderCount);
        }

        if (this.countryFilterList.SelectedItem.Value != defaultFilterText)
        {
            myCustomers = myCustomers.Where(c => c.Address.Country == countryFilterList.SelectedValue);
        }

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Aby uniknąć tego ponownej kompilacji, można ponownie napisać CompiledQuery uwzględnienie możliwych filtrów:

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

Którego będzie można wywołać w interfejsie użytkownika, takich jak:

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        int? countFilter = (this.orderCountFilterList.SelectedIndex == 0) ?
            (int?)null :
            int.Parse(this.orderCountFilterList.SelectedValue);

        string countryFilter = (this.countryFilterList.SelectedIndex == 0) ?
            null :
            this.countryFilterList.SelectedValue;

        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployeeWithFilters(
                countFilter, countryFilter);

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Kosztem w tym miejscu to polecenie wygenerowanego magazynu będzie ono mieć zawsze filtry z sprawdzanie wartości null, ale powinny być stosunkowo proste dla serwera bazy danych w celu optymalizacji:

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a>3.4 buforowanie metadanych

Entity Framework obsługuje także buforowanie metadanych. To jest zasadniczo buforowanie informacji o typie i informacje dotyczące mapowania typu w bazie danych w różnych połączeń do tego samego modelu. Pamięć podręczna metadanych jest unikatowa dla każdej domeny aplikacji.

#### <a name="341-metadata-caching-algorithm"></a>3.4.1 pamięć podręczna metadanych algorytmu

1.  Informacje o metadanych dla modelu, znajduje się w obiektu ItemCollection każdy obiekt EntityConnection.
    -   Jako notatka boczna istnieją różne obiekty ItemCollection dla różnych części modelu. Na przykład StoreItemCollections zawiera informacje o modelu bazy danych; ObjectItemCollection zawiera informacje o modelu danych; EdmItemCollection zawiera informacje o modelu koncepcyjnego.

2.  Jeśli dwa połączenia używają tych samych parametrach połączenia, będą miały to samo wystąpienie ItemCollection.
3.  Parametry połączenia funkcjonalnie równoważne, ale różnych w formie tekstu może spowodować innych metadanych w pamięci podręcznej. Firma Microsoft tokenizację parametry połączenia, więc po prostu zmieniając kolejność tokenów powinna być rozwiązywana WE udostępnionych metadanych. Jednak dwa ciągi połączeń, które wydają się funkcjonalne może nie zostać ocenione jako identyczne po tokenizacji.
4.  ItemCollection jest okresowo sprawdzane pod kątem użycia. Jeśli okaże się, że obszar roboczy nie uzyska dostępu niedawno, zostanie ona oznaczona na oczyszczenie na następny czyszczenia pamięci podręcznej.
5.  Jedynie tworzenie EntityConnection spowoduje, że pamięć podręczna metadanych, ma zostać utworzony (chociaż kolekcji elementów, które w nim nie zostaną zainicjowane, dopóki nie jest otwarte połączenie). Ten obszar roboczy pozostanie w pamięci, dopóki buforowania algorytm okaże się, że nie jest "w użyciu".

Zespół Doradczy klientów zapisane wpis w blogu, który opisuje zawierający odwołanie do obiektu ItemCollection w celu uniknięcia "wycofywania", korzystając z dużych modeli: \< http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a>3.4.2 relacji między buforowanie metadanych i buforowanie planu zapytania

Wystąpienie pamięci podręcznej planu zapytania, znajduje się w obiekcie MetadataWorkspace ItemCollection typów magazynu. Oznacza to, że polecenia magazynu pamięci podręcznej stosowanych w odniesieniu do zapytań dla dowolnego kontekstu tworzone przy użyciu danego obiektu MetadataWorkspace. Oznacza to również, że jeśli masz dwa ciągi połączeń są nieco inne, które nie są zgodne po tokenizowanie, użytkownik będzie miał inne zapytanie, planowanie wystąpienia pamięci podręcznej.

### <a name="35-results-caching"></a>3.5 wyniki buforowania

Z wynikami buforowania (znany także jako "second-level buforowanie") należy dysponować wyniki zapytań w lokalnej pamięci podręcznej. Wydając kwerendę, najpierw zobaczysz przypadku wyniki są dostępne lokalnie przed zapytania względem magazynu. Gdy wyniki z pamięci podręcznej nie są bezpośrednio obsługiwane przez program Entity Framework, jest możliwość dodania drugiego poziomu pamięci podręcznej przy użyciu dostawcy zawijania. Przykład dostawcy zawijania z pamięcią podręczną drugiego poziomu jest Alachisoft firmy [Entity Framework drugi poziom Cache oparta na NCache](http://www.alachisoft.com/ncache/entity-framework.html).

Ta implementacja pamięć podręczna drugiego poziomu jest wprowadzony funkcje, które odbywa się po ocenie wyrażenie LINQ (i funcletized) i planu wykonywania zapytania jest obliczana lub pobrane z pierwszego poziomu pamięci podręcznej. Pamięć podręczna drugiego poziomu następnie zapisze tylko wyniki pierwotne bazy danych, dlatego potok materializacja nadal wykonuje później.

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a>3.5.1 dodatkowe informacje dotyczące wyników z pamięci podręcznej za pomocą dostawcy zawijania

-   Julie Lerman został zapisany w artykule MSDN "Second-Level buforowania w Entity Framework i Windows Azure", o tym, jak można zaktualizować dostawcy zawijania przykładowe pamięci podręcznej programu AppFabric systemu Windows Server: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)
-   Jeśli pracujesz z Entity Framework 5, blog zespołu ma wpis, w której opisano Rozpoczynanie pracy z pamięci podręcznej dostawcy programu Entity Framework 5: \< http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>. Zawiera on również szablon T4, które ułatwiają Automatyzowanie dodanie 2. buforowanie na poziomie projektu.

## <a name="4-autocompiled-queries"></a>4 Autocompiled zapytań

Podczas generowania zapytania względem bazy danych przy użyciu platformy Entity Framework, jego musi ona przejść serię kroków przed faktycznie materializowanie wyniki. jeden taki krok nie jest kompilowanie zapytania. Znanych zapytań jednostki SQL ma dobrą wydajność, jak automatycznie są buforowane, więc drugi lub trzeci czas wykonania tego samego zapytania, można pominąć kompilatora planu i zamiast tego użyj buforowanego planu.

Entity Framework 5 wprowadzono buforowania automatycznego dla programu LINQ do zapytań jednostki także. W poprzednich wersjach programu Entity Framework CompiledQuery, aby przyspieszyć tworzenie wydajność była powszechną praktyką dzięki temu upewnisz się LINQ do kwerendy jednostek podlega buforowaniu. Ponieważ buforowanie teraz odbywa się automatycznie bez użycia CompiledQuery, nazywamy tę funkcję "autocompiled zapytania". Aby uzyskać więcej informacji o pamięci podręcznej planu zapytania i jego mechanics Zobacz buforowanie planu zapytania.

Wykrywa platformy Entity Framework, gdy zapytanie wymaga, aby ponownie skompilowana, a nie tak, gdy zapytanie jest wywoływana, nawet wtedy, gdy miała zostać skompilowany przed. Typowe warunki, które powodują zapytania do ponownej kompilacji są:

-   Zmiana MergeOption skojarzonej z zapytaniem. Pamięci podręcznej zapytania nie będą używane, zamiast tego kompilator plan zostaną ponownie uruchomione i nowo utworzonego planu pobiera buforowany.
-   Zmiana wartości ContextOptions.UseCSharpNullComparisonBehavior. Możesz uzyskać ten sam efekt jak zmiana MergeOption.

Inne warunki może uniemożliwić korzystanie z pamięci podręcznej przez zapytanie. Typowe przykłady to:

-   Za pomocą interfejsu IEnumerable&lt;T&gt;. Zawiera&lt;&gt;(wartość T).
-   Za pomocą funkcji, które generują zapytania za pomocą stałych.
-   Korzystanie z właściwości obiektu nie jest zamapowany.
-   Łączenie zapytania do innego zapytania, który wymaga, aby ponownie skompilowana.

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a>4.1 przy użyciu interfejsu IEnumerable&lt;T&gt;. Zawiera&lt;T&gt;(wartość T)

Entity Framework, nie będzie buforować zapytań, które wywołują IEnumerable&lt;T&gt;. Zawiera&lt;T&gt;(T wartości) względem kolekcji w pamięci, ponieważ wartości kolekcji są traktowane jako nietrwałe. Poniższe przykładowe zapytanie nie będzie zapisywane, dzięki czemu będzie on przetworzony przez kompilator plan:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var query = context.MyEntities
                    .Where(entity => ids.Contains(entity.Id));

    var results = query.ToList();
    ...
}
```

Należy zauważyć, że rozmiar IEnumerable, względem której zawiera jest wykonywane zapytanie określa, jak szybko lub wolno jest kompilowana. Może to spowodować obniżenie wydajności znacznie korzystając z dużych kolekcjach, takiego jak pokazano w powyższym przykładzie.

Entity Framework 6 zawiera optymalizacje w sposobie IEnumerable&lt;T&gt;. Zawiera&lt;T&gt;(wartość T) działa, gdy zapytania są wykonywane. Kod SQL, który jest generowany jest znacznie szybszy, aby wygenerować i bardziej czytelny i w większości przypadków jest również wykonywana szybciej na serwerze.

### <a name="42-using-functions-that-produce-queries-with-constants"></a>4.2 przy użyciu funkcji, które generują zapytania za pomocą stałych

Operatory Skip(), Take(), Contains() i DefautIfEmpty() LINQ nie tworzą zapytania SQL z parametrami, ale zamiast tego umieść wartości przekazane do nich jako stałe. W związku z tym zapytań, które w przeciwnym razie mogą być identyczne zakończenia się zanieczyszczenie zapytanie plan pamięci podręcznej, zarówno na stosie EF, jak i na serwerze bazy danych, a nie uzyskać reutilized, chyba że tych samych stałych są używane podczas wykonywania kolejnych zapytań. Na przykład:

``` csharp
var id = 10;
...
using (var context = new MyContext())
{
    var query = context.MyEntities.Select(entity => entity.Id).Contains(id);

    var results = query.ToList();
    ...
}
```

W tym przykładzie każdym razem, gdy to zapytanie jest wykonywane, podając inną wartość dla identyfikatora kwerendy zostanie skompilowany w nowym planie.

W szczególności zwróć uwagę na korzystanie z Skip i Take podczas ustalania stronicowania. W EF6 metody te mają przeciążenia lambda, które skutecznie sprawia, że plan pamięci podręcznej zapytań do wielokrotnego użytku ponieważ EF można przechwytywać zmienne przekazywane do tych metod i tłumaczyć je na SQLparameters. Pomaga to również zachowywać pamięci podręcznej bardziej przejrzysty, ponieważ w przeciwnym razie każdego zapytania z inną stałą Skip i Take otrzymamy swój własny wpis pamięci podręcznej planu zapytania.

Należy wziąć pod uwagę następujący kod, który jest nieoptymalne, ale jest przeznaczone wyłącznie do spróbujemy tej klasy zapytania:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Szybsze wersję tego samego kodu obejmowałaby wywoływanie Pomiń z wyrażenia lambda:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i \< count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Drugi fragment kodu może działać 11% szybciej, ponieważ jest używany ten sam plan zapytania, za każdym razem, gdy zapytanie jest uruchomione, pozwala zaoszczędzić czas procesora CPU, co pozwala uniknąć zanieczyszczenie pamięć podręczną zapytań. Ponadto ponieważ parametru do pomijania jest zamknięcie kod także wygląda to teraz:

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a>4.3 przy użyciu właściwości obiektu bez zamapowane

Podczas zapytania jest używana właściwości typu-zamapowany obiekt jako parametr, a następnie zapytanie będzie nie Pobieranie pamięci podręcznej. Na przykład:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();

    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myObject.MyProperty)
                select entity;

   var results = query.ToList();
    ...
}
```

W tym przykładzie założono, że klasa NonMappedType nie jest częścią modelu jednostki. To zapytanie można łatwo zmienić nie używają typu nie są mapowane, i zamiast tego użyć zmiennej lokalnej jako parametru zapytania:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();
    var myValue = myObject.MyProperty;
    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myValue)
                select entity;

    var results = query.ToList();
    ...
}
```

W tym przypadku zapytania będą mogli uzyskać pamięci podręcznej i będą mogli korzystać z pamięci podręcznej planu zapytania.

### <a name="44-linking-to-queries-that-require-recompiling"></a>4.4 — łączenie zapytań, które wymagają ponownej kompilacji

Tym samym przykładzie jak wyżej Jeśli masz drugiego zapytania, która opiera się na zapytaniach, który musi być ponownie kompilowane, cały drugiego zapytania będzie również ponownie kompilowana. Oto przykład, aby zilustrować ten scenariusz:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var firstQuery = from entity in context.MyEntities
                        where ids.Contains(entity.Id)
                        select entity;

    var secondQuery = from entity in context.MyEntities
                        where firstQuery.Any(otherEntity => otherEntity.Id == entity.Id)
                        select entity;

    var results = secondQuery.ToList();
    ...
}
```

W przykładzie przedstawiono ogólny, ale ilustruje, jak łączenie firstQuery powoduje secondQuery będą mogli uzyskać pamięci podręcznej. Jeśli firstQuery nie była kwerendę, która wymaga ponownej kompilacji, następnie secondQuery mogłoby być buforowane.

## <a name="5-notracking-queries"></a>Zapytania NoTracking 5

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a>5.1 wyłączenie śledzenie zmian, aby zmniejszyć koszty zarządzania stanu

Jeśli jesteś w scenariuszu tylko do odczytu i chcesz uniknąć zadań ładowania obiektów w obiekcie ObjectStateManager, możesz odpytywać "Bez śledzenia".  Można wyłączyć śledzenia zmian na poziomie zapytania.

Pamiętaj jednak, że, wyłączając możesz śledzenia zmian są efektywne wyłączenie pamięci podręcznej obiektów. Po wykonaniu zapytania dotyczącego jednostki, firma Microsoft nie można pominąć materializacja przez pobieranie wyników zapytania wcześniej zmaterializowanego w obiekcie ObjectStateManager. Jeśli są wielokrotnie wykonywaniem zapytań dotyczących tych samych jednostek na tym samym kontekście, można napotkać faktycznie wydajności korzyść z włączenia śledzenia zmian.

Podczas wykonywania zapytania za pomocą obiektu ObjectContext, gdy jest ustawiona i zapytania, które składają się na nich będzie dziedziczyć skuteczne MergeOption zapytania nadrzędnego wystąpienia ObjectQuery i obiektu ObjectSet zapamięta MergeOption. Korzystając z typu DbContext, wywołując modyfikator AsNoTracking() na DbSet można wyłączyć śledzenia.

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a>5.1.1 wyłączenie śledzenia zmian dla zapytania przy użyciu typu DbContext

Aby przełączyć tryb zapytania na NoTracking, łańcuch wywołanie metody AsNoTracking() w zapytaniu. W odróżnieniu od ObjectQuery DbSet i DbQuery klasy w interfejsie API DbContext braku modyfikowalną właściwość MergeOption.

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a>5.1.2 wyłączenie śledzenia na poziomie zapytania, przy użyciu obiektu ObjectContext zmian

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a>5.1.3 wyłączenie śledzenia zmian dla całej jednostki można ustawić przy użyciu obiektu ObjectContext

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52-test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a>5.2 metryki testu ukazujące korzyści w zakresie wydajności kwerend NoTracking

W tym teście spojrzymy kosztem wypełnianie obiekt ObjectStateManager porównując śledzenia zapytań NoTracking Navision modelu. Zobacz dodatek opis modelu Navision i typów kwerend, które zostały wykonane. W tym teście możemy iteracji przez listę zapytań i wykonać jeden raz każdego z nich. Uruchomiliśmy dwie odmiany testu, drugi raz z NoTracking zapytań i jeden raz z domyślną opcję scalania "TylkoDołącz". Przeprowadziliśmy poszczególnych odmian 3 razy i wykonać wartości średniej przebiegów. Między testy możemy Wyczyść pamięć podręczną zapytań w programie SQL Server i zmniejszyć tempdb, uruchamiając następujące polecenia:

1.  POLECENIE DBCC DROPCLEANBUFFERS
2.  POLECENIE DBCC FREEPROCCACHE
3.  DBCC SHRINKDATABASE (bazy danych tempdb, 0)

Wyniki, mediana ponad 3 przebiegów testów:

|                        | NIE ŚLEDZENIA — ZESTAW ROBOCZY | BRAK ŚLEDZENIA — GODZINA | DOŁĄCZ DO TYLKO — ZESTAW ROBOCZY | DOŁĄCZ TYLKO — GODZINA |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| **Entity Framework 5** | 460361728                 | 1163536 ms         | 596545536                 | 1273042 ms         |
| **Entity Framework 6** | 647127040                 | 190228 ms          | 832798720                 | 195521 ms          |

Entity Framework 5 mają mniejsze zużycie pamięci na koniec uruchom niż Entity Framework 6. Dodatkowej pamięci używane przez program Entity Framework 6 jest wynikiem struktur więcej pamięci i kod, który umożliwia deweloperom nowe funkcje i lepszą wydajność.

Istnieje również wyczyść różnica w zużycie pamięci, korzystając z obiektu ObjectStateManager. Gdy rejestrowanie informacji o wszystkich jednostek, które firma Microsoft zmaterializowanego z bazy danych, platformy Entity Framework 5 zwiększyć jego rozmiaru o 30%. Entity Framework 6 zwiększyć jego rozmiaru, 28% sytuacji.

W czasie platformy Entity Framework 6 przewyższa stosowane przekształcania Entity Framework 5 w tym teście przez duże margines. Entity Framework 6 test zakończył się w około 16% czasu używany przez Entity Framework 5. Ponadto Entity Framework 5 czasochłonne 9% więcej ukończone, gdy jest używany obiekt ObjectStateManager. W odróżnieniu od platformy Entity Framework 6 używa więcej czasu, korzystając z obiekt ObjectStateManager % 3.

## <a name="6-query-execution-options"></a>6 opcje wykonywania zapytań

Entity Framework oferuje kilka różnych sposobów, aby wykonać zapytanie. Firma Microsoft będzie zapoznaj się z następujących opcji, porównaj zalet i wad każdego z nich i sprawdzić ich charakterystyk wydajności:

-   Składnik LINQ to Entities.
-   Brak śledzenia LINQ to Entities.
-   Jednostka SQL przez ObjectQuery.
-   Jednostka SQL przez EntityCommand.
-   ExecuteStoreQuery.
-   SqlQuery.
-   CompiledQuery.

### <a name="61-------linq-to-entities-queries"></a>6.1 zapytaniach składnika LINQ to Entities

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

**Specjaliści**

-   Odpowiedni dla operacje CUD.
-   W pełni zmaterializowany obiektów.
-   Najprostszą do zapisu przy użyciu składni wbudowane w języku programowania.
-   Dobrą wydajność.

**Wady**

-   Niektórych ograniczeń technicznych, takich jak:
    -   Wzory DefaultIfEmpty zapytań OUTER JOIN powoduje bardziej złożone zapytania niż proste instrukcje OUTER JOIN w języku SQL jednostki.
    -   Nadal nie można użyć NOTACJI z dopasowaniem wzorca ogólnego.

### <a name="62-------no-tracking-linq-to-entities-queries"></a>6.2. Brak śledzenia LINQ do zapytań jednostki

Gdy kontekstu pochodzi ObjectContext:

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

Gdy kontekstu pochodzi DbContext:

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

**Specjaliści**

-   Zwiększono wydajność przez regularne zapytań LINQ.
-   W pełni zmaterializowany obiektów.
-   Najprostszą do zapisu przy użyciu składni wbudowane w języku programowania.

**Wady**

-   Nie nadaje się do operacje CUD.
-   Niektórych ograniczeń technicznych, takich jak:
    -   Wzory DefaultIfEmpty zapytań OUTER JOIN powoduje bardziej złożone zapytania niż proste instrukcje OUTER JOIN w języku SQL jednostki.
    -   Nadal nie można użyć NOTACJI z dopasowaniem wzorca ogólnego.

Należy pamiętać, zapytania, które właściwości skalarne projektu nie są śledzone, nawet jeśli nie określono NoTracking. Na przykład:

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

To określone zapytanie nie jawnie określone, jest NoTracking, ale ponieważ nie jest materializowanie typ, który ma znane przez menedżera stanu obiektu następnie zmaterializowanym wyniku nie jest śledzone.

### <a name="63-------entity-sql-over-an-objectquery"></a>6.3 jednostki SQL przez ObjectQuery

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

**Specjaliści**

-   Odpowiedni dla operacje CUD.
-   W pełni zmaterializowany obiektów.
-   Buforowanie planu zapytania obsługuje.

**Wady**

-   Obejmuje ciągi zapytań tekstowych, które są bardziej podatne na błędy użytkowników niż konstrukcje zapytań wbudowane w języku.

### <a name="64-------entity-sql-over-an-entity-command"></a>6.4 jednostki SQL przez polecenie jednostki

``` csharp
EntityCommand cmd = eConn.CreateCommand();
cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
{
    while (reader.Read())
    {
        // manually 'materialize' the product
    }
}
```

**Specjaliści**

-   Obsługuje zapytania, buforowanie planu w programie .NET 4.0 (buforowanie planu jest obsługiwany przez wszystkie inne typy zapytań w .NET 4.5).

**Wady**

-   Obejmuje ciągi zapytań tekstowych, które są bardziej podatne na błędy użytkowników niż konstrukcje zapytań wbudowane w języku.
-   Nie nadaje się do operacje CUD.
-   Wyniki nie są automatycznie zmaterializowany i musi być odczytywana z czytnika danych.

### <a name="65-------sqlquery-and-executestorequery"></a>6.5 SqlQuery i ExecuteStoreQuery

SqlQuery w bazie danych:

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

SqlQuery na DbSet:

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

ExecyteStoreQuery:

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

**Specjaliści**

-   Ogólnie największą wydajność, ponieważ kompilator plan jest pomijany.
-   W pełni zmaterializowany obiektów.
-   Odpowiedni dla operacje CUD w przypadku używania z DbSet.

**Wady**

-   Zapytanie jest tekstową i występowania błędów.
-   Zapytanie jest powiązany określonych wewnętrznej bazy danych przy użyciu semantyki magazynu zamiast semantyki pojęć.
-   W przypadku dziedziczenia jest obecny, handcrafted zapytania trzeba uwzględnić warunki mapowania żądanego typu.

### <a name="66-------compiledquery"></a>6.6 CompiledQuery

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

**Specjaliści**

-   Udostępnia do poprawy wydajności 7% w porównaniu z regularnych zapytań LINQ.
-   W pełni zmaterializowany obiektów.
-   Odpowiedni dla operacje CUD.

**Wady**

-   Większą złożoność i koszty programowania.
-   Zwiększenie wydajności zostaną utracone podczas redagowania na podstawie kompilowanym zapytaniu.
-   Nie można zapisać niektórych zapytań LINQ jako CompiledQuery — na przykład projekcje typów anonimowych.

### <a name="67-------performance-comparison-of-different-query-options"></a>6.7 Porównanie wydajności opcji inne zapytanie

Proste microbenchmarks, której nie upłynął tworzenie kontekstu pojawiły się do testu. Firma Microsoft mierzy zapytań 5000 razy dla zestawu jednostek nie są buforowane w kontrolowanym środowisku. Numery te są pobierane z ostrzeżeniem: nie odzwierciedlają wartości rzeczywistych generowany przez aplikację, ale zamiast tego są one bardzo dokładny pomiar większość różnic w wydajności jest porównaniu różne opcje zapytań jabłka do jabłek, z wyłączeniem koszt utworzenia nowego kontekstu.

| EF  | Test                                 | Czas (ms) | Pamięć   |
|:----|:-------------------------------------|:----------|:---------|
| EF5 | ObjectContext ESQL                   | 2414      | 38801408 |
| EF5 | Zapytania Linq ObjectContext             | 2692      | 38277120 |
| EF5 | Linq typu DbContext zapytania nie śledzenia     | 2818      | 41840640 |
| EF5 | Zapytania Linq typu DbContext                 | 2930      | 41771008 |
| EF5 | Linq do obiektu ObjectContext zapytania nie śledzenia | 3013      | 38412288 |
|     |                                      |           |          |
| EF6 | ObjectContext ESQL                   | 2059      | 46039040 |
| EF6 | Zapytania Linq ObjectContext             | 3074      | 45248512 |
| EF6 | Linq typu DbContext zapytania nie śledzenia     | 3125      | 47575040 |
| EF6 | Zapytania Linq typu DbContext                 | 3420      | 47652864 |
| EF6 | Linq do obiektu ObjectContext zapytania nie śledzenia | 3593      | 45260800 |

![EF5Micro5000Warm](~/ef6/media/ef5micro5000warm.png)

![EF6Micro5000Warm](~/ef6/media/ef6micro5000warm.png)

Microbenchmarks są bardzo wrażliwe na niewielkie zmiany w kodzie. W tym przypadku różnicy między kosztów Entity Framework 5 i Entity Framework 6 są spowodowane przez dodanie [przejmowanie](~/ef6/fundamentals/logging-and-interception.md) i [transakcyjnych ulepszenia](~/ef6/saving/transactions.md). Te numery microbenchmarks są jednak namnożonego przetwarzania do bardzo małego fragmentu działanie programu Entity Framework. Rzeczywiste scenariusze dostępu do ciepłych zapytania nie powinien zostać wyświetlony regresji wydajności podczas uaktualniania programu Entity Framework 5 do programu Entity Framework 6.

Aby porównać wydajność rzeczywistych opcje inne zapytanie, utworzyliśmy 5 odmiany osobnego badania, gdzie możemy opcja inne zapytanie umożliwia wybranie wszystkich produktów, których nazwa kategorii jest "Beverages". Każda iteracja obejmuje koszt tworzenia kontekstu, a koszt materializowanie wszystkie zwrócone jednostki. 10 iteracji są uruchamiane untimed przed przełączeniem sumę upłynął limit czasu 1000 iteracji. Wyniki wyświetlane są także jej mediana Uruchom pobranego z 5 uruchomienia każdego testu. Aby uzyskać więcej informacji zobacz dodatek B, który zawiera kod dla testu.

| EF  | Test                                        | Czas (ms) | Pamięć   |
|:----|:--------------------------------------------|:----------|:---------|
| EF5 | Polecenie ObjectContext jednostki                | 621       | 39350272 |
| EF5 | Kontekst DbContext zapytanie Sql w bazie danych             | 825       | 37519360 |
| EF5 | Query Store ObjectContext                   | 878       | 39460864 |
| EF5 | Linq do obiektu ObjectContext zapytania nie śledzenia        | 969       | 38293504 |
| EF5 | Obiekt ObjectContext jednostki Sql za pomocą obiektu Query | 1089      | 38981632 |
| EF5 | Obiekt ObjectContext kompilowanym zapytaniu.                | 1099      | 38682624 |
| EF5 | Zapytania Linq ObjectContext                    | 1152      | 38178816 |
| EF5 | Linq typu DbContext zapytania nie śledzenia            | 1208      | 41803776 |
| EF5 | Zapytanie Sql DbContext na DbSet                | 1414      | 37982208 |
| EF5 | Zapytania Linq typu DbContext                        | 1574      | 41738240 |
|     |                                             |           |          |
| EF6 | Polecenie ObjectContext jednostki                | 480       | 47247360 |
| EF6 | Query Store ObjectContext                   | 493       | 46739456 |
| EF6 | Kontekst DbContext zapytanie Sql w bazie danych             | 614       | 41607168 |
| EF6 | Linq do obiektu ObjectContext zapytania nie śledzenia        | 684       | 46333952 |
| EF6 | Obiekt ObjectContext jednostki Sql za pomocą obiektu Query | 767       | 48865280 |
| EF6 | Obiekt ObjectContext kompilowanym zapytaniu.                | 788       | 48467968 |
| EF6 | Linq typu DbContext zapytania nie śledzenia            | 878       | 47554560 |
| EF6 | Zapytania Linq ObjectContext                    | 953       | 47632384 |
| EF6 | Zapytanie Sql DbContext na DbSet                | 1023      | 41992192 |
| EF6 | Zapytania Linq typu DbContext                        | 1290      | 47529984 |


![EF5WarmQuery1000](~/ef6/media/ef5warmquery1000.png)

![EF6WarmQuery1000](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> Aby informacje były kompletne dodaliśmy odmiany, gdzie możemy wykonać kwerendy SQL jednostki EntityCommand. Jednak ponieważ wyniki nie są zmaterializowanego takich zapytań, porównanie niekoniecznie jabłek do jabłka. Test obejmuje bliskie zbliżenia do materializowanie próby porównywania bardziej sprawiedliwa.

W tym przypadku end-to-end Entity Framework 6 przewyższa stosowane przekształcania Entity Framework 5 ze względu na ulepszenia wydajności wprowadzone na kilka części stosu, w tym znacznie jaśniejszy inicjowania typu DbContext i szybsze MetadataCollection&lt;T&gt; wyszukiwania.

## <a name="7-design-time-performance-considerations"></a>7 zagadnienia dotyczące wydajności czasu projektowania

### <a name="71-------inheritance-strategies"></a>7.1 strategie dziedziczenia

Inną ważną kwestią wydajności podczas korzystania z programu Entity Framework jest strategia dziedziczenia, których używasz. Entity Framework obsługuje 3 typy podstawowe dziedziczenia i ich kombinacje:

-   Tabela na hierarchii (TPH) — gdzie każdy dziedziczenia zestaw mapy do tabeli z kolumną dyskryminatora, aby wskazać, które określonego typu w hierarchii jest jest reprezentowana w wierszu.
-   Tabela według typu (TPT) — gdzie każdy typ ma własną tabelę w bazie danych. tabele podrzędne definiować tylko kolumny, które nie zawiera tabeli nadrzędnej.
-   Tabela na klasę (TPC) — gdzie każdy typ ma swój własny pełną tabelę w bazie danych. tabele podrzędne definiują ich pól, takich jak te zdefiniowane typów nadrzędnych.

Jeśli model korzysta z dziedziczenia TPT, zapytania, które są generowane będzie bardziej skomplikowane niż te, które są generowane z innymi strategiami dziedziczenia, które mogą powstać w dłuższym czasie wykonywania w sklepie.  Zwykle będzie trwać dłużej, do generowania zapytań za pośrednictwem modelu TPT i do zmaterializowania obiektów wynikowych.

Zobacz "zagadnienia dotyczące wydajności podczas korzystania z dziedziczenia TPT (Tabela na typ) w Entity Framework" w blogu MSDN: \< http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.

#### <a name="711-------avoiding-tpt-in-model-first-or-code-first-applications"></a>7.1.1 unikanie TPT w aplikacjach pierwszego modelu lub Code First

Podczas tworzenia modelu przez istniejącą bazę danych, która ma schemat TPT nie masz wiele opcji. Jednak podczas tworzenia aplikacji przy użyciu modelu pierwszej lub Code First, należy unikać TPT dziedziczenia dla problemów z wydajnością.

Gdy używasz pierwszego modelu w Kreatorze Projektant jednostki, otrzymasz TPT wszystkie dziedziczenia w modelu. Jeśli chcesz przełączyć się do strategii TPH dziedziczenia z pierwszego modelu, można używać "jednostki projektanta bazy danych generowania Power Pack" dostępne z galerii Visual Studio ( \< http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).

Za pomocą Code First skonfiguruj mapowanie modelu za pomocą dziedziczenia, platforma EF użyje TPH domyślnie, dlatego wszystkie jednostki w hierarchii dziedziczenia zostaną zmapowane do tej samej tabeli. Zobacz sekcję "Mapowanie z interfejs Fluent API" artykułu "Kodu pierwszy w jednostki Framework4.1" w MSDN Magazine ( [ http://msdn.microsoft.com/magazine/hh126815.aspx ](https://msdn.microsoft.com/magazine/hh126815.aspx)) Aby uzyskać więcej informacji.

### <a name="72-------upgrading-from-ef4-to-improve-model-generation-time"></a>7.2 uaktualniasz EF4 w celu generowania modelu czasu

Ulepszanie specyficzne dla programu SQL Server do algorytmu, który generuje warstwę magazynu (SSDL) modelu jest dostępne w programie Entity Framework 5 i 6 i jako aktualizacja programu Entity Framework 4, po zainstalowaniu programu Visual Studio 2010 z dodatkiem SP1. Następujące wyniki testów pokazują ulepszanie podczas generowania modelu bardzo dużych, w tym przypadku modelu Navision. Aby uzyskać więcej informacji na ten temat, zobacz dodatku C.

W modelu danych zestawy jednostek 1005 4227 zestawów skojarzeń.

| Konfiguracja                              | Podział wykorzystany czas                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Program Visual Studio 2010, platformy Entity Framework 4     | Generowanie SSDL: 2 hr 27 min <br/> Generowanie mapowania: 1 sekundę <br/> Generowanie CSDL: 1 sekundę <br/> Generowanie ObjectLayer: 1 sekundę <br/> Generowanie widoku: 2 godz. 14 min |
| Visual Studio 2010 z dodatkiem SP1, platformy Entity Framework 4 | Generowanie SSDL: 1 sekundę <br/> Generowanie mapowania: 1 sekundę <br/> Generowanie CSDL: 1 sekundę <br/> Generowanie ObjectLayer: 1 sekundę <br/> Generowania widoku: 1 min 53 hr   |
| Visual Studio 2013, platformy Entity Framework 5     | Generowanie SSDL: 1 sekundę <br/> Generowanie mapowania: 1 sekundę <br/> Generowanie CSDL: 1 sekundę <br/> Generowanie ObjectLayer: 1 sekundę <br/> Generowanie widoku: 65 minut    |
| Visual Studio 2013, platformy Entity Framework 6     | Generowanie SSDL: 1 sekundę <br/> Generowanie mapowania: 1 sekundę <br/> Generowanie CSDL: 1 sekundę <br/> Generowanie ObjectLayer: 1 sekundę <br/> Generowanie widoku: 28 sekundach.   |


Warto zauważyć, że podczas generowania SSDL, obciążenia prawie całkowicie odbywa się na programu SQL Server, gdy oczekuje na komputerze deweloperskim klienta bezczynny wyniki, aby wrócić z serwera. Przetwarzający należy szczególnie wdzięczni za to ulepszenie. Warto również zauważyć, że zasadniczo cały koszt generowania modelu odbywa się podczas generowania widoku teraz.

### <a name="73-------splitting-large-models-with-database-first-and-model-first"></a>7.3 najpierw dzielenia dużych modeli z bazą danych i modelu pierwszy

W miarę zwiększania rozmiaru modelu powierzchni projektanta staje się przepełniony i trudne w użyciu. Firma Microsoft zwykle należy wziąć pod uwagę modelu z ponad 300 jednostek zbyt duży, aby skutecznie używać projektanta. Następujący wpis w blogu opisuje kilka opcji do dzielenia dużych modeli: \< http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.

Wpis został napisany dla pierwszej wersji programu Entity Framework, ale te kroki nadal mają zastosowanie.

### <a name="74-------performance-considerations-with-the-entity-data-source-control"></a>7.4 zagadnienia dotyczące wydajności z kontrolą źródła danych jednostki

Zobaczyliśmy przypadków w wielowątkowych wydajności i testy obciążeniowe, gdzie wydajność aplikacji sieci web za pomocą kontroli EntityDataSource pogorszy znacznie. Podstawową przyczyną jest to, że EntityDataSource wielokrotnie wywołuje MetadataWorkspace.LoadFromAssembly na zestawy przywoływane przez aplikację sieci Web, aby dowiedzieć się, typ, który będzie służyć jako jednostki.

Rozwiązanie jest równa ContextTypeName z EntityDataSource nazwę typu klasy pochodnej obiektu ObjectContext. Wyłącza mechanizm, który skanuje wszystkich przywoływanych zestawach typów jednostek.

Ponadto ustawienie pola ContextTypeName zapobiega to problem z funkcjonalnością gdzie EntityDataSource w programie .NET 4.0 zgłasza wyjątku ReflectionTypeLoadException, gdy nie może załadować typu z zestawu przy użyciu odbicia. Ten problem został rozwiązany w .NET 4.5.

### <a name="75-------poco-entities-and-change-tracking-proxies"></a>7.5 jednostki POCO i serwery proxy śledzenia zmian

Entity Framework pozwala używać klas niestandardowych danych wraz z modelu danych bez żadnych modyfikacji klas danych, samodzielnie. Oznacza to, że za pomocą obiektów CLR "zwykły stary" (POCO), takie jak istniejące obiekty domeny, modelu danych. Te POCO klas danych (znany także jako zakresu trwałość obiektów), które są mapowane do jednostek, które są zdefiniowane w modelu danych, obsługują większości tego samego zapytania, wstawianie, aktualizowanie i usuwanie zachowania jako typy jednostek, które są generowane przez narzędzia modelu Entity Data Model.

Entity Framework można również tworzyć klasy pochodne typy POCO, w których są używane, jeśli chcesz włączyć funkcje, takie jak powolne ładowanie i automatyczne śledzenie zmian w jednostki POCO klasy w serwera proxy. Twoich zajęciach POCO muszą spełniać określone wymagania, aby umożliwić Entity Framework użyć serwerów proxy, zgodnie z opisem w tym miejscu: [ http://msdn.microsoft.com/library/dd468057.aspx ](https://msdn.microsoft.com/library/dd468057.aspx).

Serwery proxy śledzenia szansy powiadomi przez menedżera stanu obiektu każdorazowo dowolne z właściwości jednostki ma swoją wartość, więc Entity Framework zna rzeczywistego stanu jednostki przez cały czas. Odbywa się to przez dodanie zdarzenia powiadomień do treści metod ustawiających właściwości, a ponieważ Menedżer stanu obiektu przetwarzania takie zdarzenia. Należy zauważyć, że tworzenie proxy jednostka będzie najczęściej być droższe niż tworzenia jednostki POCO-proxy z powodu dodano zestaw zdarzenia utworzone przez program Entity Framework.

Podczas jednostki POCO nie ma serwera proxy śledzenia zmian, zmiany zostaną znalezione, porównując zawartość jednostki względem kopię poprzedniego zapisanego stanu. To szczegółowe porównanie staną się długotrwałym procesem w przypadku wielu jednostek w kontekście użytkownika, lub gdy jednostek bardzo dużą ilość właściwości, nawet jeśli żaden z nich zmianie od czasu ostatniego porównania miało miejsce.

W podsumowaniu: zapłacisz wydajności trafień, podczas tworzenia serwera proxy śledzenia zmian, ale śledzenie zmian mogą pomóc przyspieszyć proces wykrywania zmian, jeśli obiekty wiele właściwości, gdy masz wiele jednostek w modelu. Dla jednostek z mniejszą liczbą właściwości, których ilość jednostek nie rozwój zbyt dużo masz serwery proxy śledzenia zmian nie może być wiele korzyści.

## <a name="8-loading-related-entities"></a>8 ładowanie powiązanych jednostek

### <a name="81-lazy-loading-vs-eager-loading"></a>8.1 powolne ładowanie programu vs. Wczesne ładowanie

Entity Framework oferuje kilka różnych sposobów, które można załadować jednostek, które są powiązane z jednostki docelowej. Na przykład, kiedy wykonujesz zapytanie dotyczące produktów, istnieją różne sposoby powiązane zamówienia zostaną załadowane do menedżera stanu obiektu. Z punktu widzenia wydajności będzie największych pytania, na które należy wziąć pod uwagę podczas ładowania powiązanych jednostek, powolne ładowanie lub Eager ładowania.

Korzystając z ładowania Eager, powiązanych jednostek jest ładowany wraz z Twojej docelowy zestaw jednostek. Używasz instrukcji dołączania w zapytaniu do wskazania, który związane z jednostkami, z którymi chcesz przełączyć.

Podczas korzystania z opóźnieniem ładowania, początkowego zapytania może dopiero w docelowym zestawem jednostek. Jednak zawsze wtedy, gdy uzyskujesz dostęp do właściwości nawigacji, inne zapytanie jest wystawiony na podstawie magazynu można załadować obiektu pokrewnego.

Po załadowaniu jednostki dodatkowe pytania dla jednostki załaduje go bezpośrednio z poziomu Menedżera stanu obiektów, zarówno w przypadku korzystania z opóźnieniem ładowania lub wczesne ładowanie.

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a>8.2 jak dokonać wyboru między powolne ładowanie i Eager ładowania

Ważne jest, zrozumieć różnicę między powolne ładowanie i ładowanie Eager, dzięki czemu można było odpowiednim wyborem dla twojej aplikacji. Dzięki temu łatwiej ocenić zależnościami między wiele żądań względem bazy danych w porównaniu z pojedynczego żądania, który może stanowić duże ładunku. Może być odpowiednie do użycia w innych częściach wczesne ładowanie w niektórych części aplikacji i ładowanie z opóźnieniem.

Na przykład o tym, co dzieje się pod maską Załóżmy, że chcesz wykonać zapytanie dla klientów, którzy mieszkają w Wielkiej Brytanii i ich liczba zamówień.

**Za pomocą wczesne ładowanie**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

**Przy użyciu ładowania z opóźnieniem**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    context.ContextOptions.LazyLoadingEnabled = true;

    //Notice that the Include method call is missing in the query
    var ukCustomers = context.Customers.Where(c => c.Address.Country == "UK");

    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

Korzystając z wczesne ładowanie, będzie wystawiać pojedyncze zapytanie, które zwraca wszystkich klientów i zamówienia. Polecenie magazynu wygląda następująco:

``` SQL
SELECT
[Project1].[C1] AS [C1],
[Project1].[CustomerID] AS [CustomerID],
[Project1].[CompanyName] AS [CompanyName],
[Project1].[ContactName] AS [ContactName],
[Project1].[ContactTitle] AS [ContactTitle],
[Project1].[Address] AS [Address],
[Project1].[City] AS [City],
[Project1].[Region] AS [Region],
[Project1].[PostalCode] AS [PostalCode],
[Project1].[Country] AS [Country],
[Project1].[Phone] AS [Phone],
[Project1].[Fax] AS [Fax],
[Project1].[C2] AS [C2],
[Project1].[OrderID] AS [OrderID],
[Project1].[CustomerID1] AS [CustomerID1],
[Project1].[EmployeeID] AS [EmployeeID],
[Project1].[OrderDate] AS [OrderDate],
[Project1].[RequiredDate] AS [RequiredDate],
[Project1].[ShippedDate] AS [ShippedDate],
[Project1].[ShipVia] AS [ShipVia],
[Project1].[Freight] AS [Freight],
[Project1].[ShipName] AS [ShipName],
[Project1].[ShipAddress] AS [ShipAddress],
[Project1].[ShipCity] AS [ShipCity],
[Project1].[ShipRegion] AS [ShipRegion],
[Project1].[ShipPostalCode] AS [ShipPostalCode],
[Project1].[ShipCountry] AS [ShipCountry]
FROM ( SELECT
      [Extent1].[CustomerID] AS [CustomerID],
       [Extent1].[CompanyName] AS [CompanyName],
       [Extent1].[ContactName] AS [ContactName],
       [Extent1].[ContactTitle] AS [ContactTitle],
       [Extent1].[Address] AS [Address],
       [Extent1].[City] AS [City],
       [Extent1].[Region] AS [Region],
       [Extent1].[PostalCode] AS [PostalCode],
       [Extent1].[Country] AS [Country],
       [Extent1].[Phone] AS [Phone],
       [Extent1].[Fax] AS [Fax],
      1 AS [C1],
       [Extent2].[OrderID] AS [OrderID],
       [Extent2].[CustomerID] AS [CustomerID1],
       [Extent2].[EmployeeID] AS [EmployeeID],
       [Extent2].[OrderDate] AS [OrderDate],
       [Extent2].[RequiredDate] AS [RequiredDate],
       [Extent2].[ShippedDate] AS [ShippedDate],
       [Extent2].[ShipVia] AS [ShipVia],
       [Extent2].[Freight] AS [Freight],
       [Extent2].[ShipName] AS [ShipName],
       [Extent2].[ShipAddress] AS [ShipAddress],
       [Extent2].[ShipCity] AS [ShipCity],
       [Extent2].[ShipRegion] AS [ShipRegion],
       [Extent2].[ShipPostalCode] AS [ShipPostalCode],
       [Extent2].[ShipCountry] AS [ShipCountry],
      CASE WHEN ([Extent2].[OrderID] IS NULL) THEN CAST(NULL AS int) ELSE 1 END AS [C2]
      FROM  [dbo].[Customers] AS [Extent1]
      LEFT OUTER JOIN [dbo].[Orders] AS [Extent2] ON [Extent1].[CustomerID] = [Extent2].[CustomerID]
      WHERE N'UK' = [Extent1].[Country]
)  AS [Project1]
ORDER BY [Project1].[CustomerID] ASC, [Project1].[C2] ASC
```

Podczas korzystania z opóźnieniem ładowania, będzie początkowo wydawać następujące zapytanie:

``` SQL
SELECT
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[CompanyName] AS [CompanyName],
[Extent1].[ContactName] AS [ContactName],
[Extent1].[ContactTitle] AS [ContactTitle],
[Extent1].[Address] AS [Address],
[Extent1].[City] AS [City],
[Extent1].[Region] AS [Region],
[Extent1].[PostalCode] AS [PostalCode],
[Extent1].[Country] AS [Country],
[Extent1].[Phone] AS [Phone],
[Extent1].[Fax] AS [Fax]
FROM [dbo].[Customers] AS [Extent1]
WHERE N'UK' = [Extent1].[Country]
```

I zawsze możesz uzyskać dostęp do właściwości nawigacji zamówienia klienta względem magazynu wystawiono innego zapytania podobne do następujących:

``` SQL
exec sp_executesql N'SELECT
[Extent1].[OrderID] AS [OrderID],
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[EmployeeID] AS [EmployeeID],
[Extent1].[OrderDate] AS [OrderDate],
[Extent1].[RequiredDate] AS [RequiredDate],
[Extent1].[ShippedDate] AS [ShippedDate],
[Extent1].[ShipVia] AS [ShipVia],
[Extent1].[Freight] AS [Freight],
[Extent1].[ShipName] AS [ShipName],
[Extent1].[ShipAddress] AS [ShipAddress],
[Extent1].[ShipCity] AS [ShipCity],
[Extent1].[ShipRegion] AS [ShipRegion],
[Extent1].[ShipPostalCode] AS [ShipPostalCode],
[Extent1].[ShipCountry] AS [ShipCountry]
FROM [dbo].[Orders] AS [Extent1]
WHERE [Extent1].[CustomerID] = @EntityKeyValue1',N'@EntityKeyValue1 nchar(5)',@EntityKeyValue1=N'AROUT'
```

Aby uzyskać więcej informacji, zobacz [ładowanie powiązanych obiektów](https://msdn.microsoft.com/library/bb896272.aspx).

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a>8.2.1 powolne ładowanie i ładowanie Eager — ściągawka

Brak coś takiego jak opracowanie wybór wczesne ładowanie, a ładowanie z opóźnieniem. Spróbuj najpierw zrozumieć różnice między dwóch strategii, więc możecie dobrze świadomych decyzji; należy również rozważyć, jeśli kod pasuje do dowolnego z następujących scenariuszy:

| Scenariusz                                                                    | Nasze sugestii                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Potrzebujesz uzyskać dostęp do wielu właściwości nawigacji z pobrano jednostek? | **Nie** — prawdopodobnie wykona obie opcje. Jednak jeśli ładunek, którą chodzi o przeniesienie zapytanie nie jest zbyt duży, może wystąpić korzyści wydajności za pomocą wczesne ładowanie, co będzie wymagać mniej sieci rund do zmaterializowania obiektów. <br/> <br/> **Tak** — Jeśli potrzebujesz uzyskać dostęp do wielu właściwości nawigacji z jednostkami, jak, za pomocą wielu instrukcji #include w zapytaniu z wczesne ładowanie. Uwzględnić więcej jednostek, większy ładunek zapytanie zwróci. Po uwzględnieniu trzech lub więcej jednostek do zapytania, należy wziąć pod uwagę przełączenie na leniwy ładowania. |
| Czy wiesz, jakie dane będą dokładnie potrzebne w czasie wykonywania?                   | **Nie** -powolne ładowanie będą lepsze dla Ciebie. W przeciwnym razie użytkownik może pozostać wykonywaniem zapytań dotyczących danych, nie będą potrzebne. <br/> <br/> **Tak** — Eager, ładowanie prawdopodobnie jest najlepszym sposobem; aby ułatwić szybsze ładowanie całym zestawie. Jeśli zapytanie wymaga pobierania bardzo dużych ilości danych, a ten staje się zbyt wolno, a następnie spróbuj leniwy zamiast tego podczas ładowania.                                                                                                                                                                                                                                                       |
| Kod wykonuje dalekie od wartości bazy danych (opóźnienie sieci zwiększone)  | **Nie** — w przypadku opóźnienie sieci nie jest problemem, za pomocą powolne ładowanie może uprościć kod. Należy pamiętać, że topologię aplikacji, mogą ulec zmianie, więc nie odległości bazy danych dla przyznane. <br/> <br/> **Tak** — w przypadku sieci jest problemem, tylko można zdecydować, co lepiej dopasować się do danego scenariusza. Zazwyczaj wczesne ładowanie jest lepiej, ponieważ wymaga mniejszej liczby rund.                                                                                                                                                                                                      |


#### <a name="822-------performance-concerns-with-multiple-includes"></a>8.2.2 problemów z wydajnością przy użyciu wielu obejmuje

Wzięliśmy to pytania wydajności, które obejmują problemów czas odpowiedzi serwera, źródłem problemu jest często zapytania z wieloma instrukcjami Include. Łącznie z powiązanymi obiektami w zapytaniu jest zaawansowanych, jest ważne, aby zrozumieć, co się dzieje w sposób niewidoczny.

Trwa stosunkowo długo dla zapytania z wieloma instrukcjami Include w nim przechodzić przez kompilator naszego wewnętrznego planu w taki sposób, aby wygenerować polecenia magazynu. Większość tego czasu jest poświęcony próby zoptymalizowania wynikowe zapytanie. Polecenie wygenerowanego magazynu będzie zawierać Outer Join lub Unii, dla każdego dołączenia, w zależności od Twojego mapowania. Kwerend, takich jak ta zostanie wyświetlone w dużych wykresów połączonych ze swojej bazy danych w jednym ładunku acerbate problemach przepustowości, szczególnie w przypadku, gdy jest dużo nadmiarowości w ładunku (na przykład, gdy wiele poziomów dołączania są używane do przechodzenia skojarzenia w kierunku jeden do wielu).

Możesz sprawdzić, czy w przypadkach, w którym zapytań jest zwracany ładunków bardzo duże, uzyskując dostęp do podstawowych TSQL dla zapytania, używając ToTraceString i wykonując polecenie magazynu w SQL Server Management Studio, aby wyświetlić rozmiar ładunku. W takich przypadkach można wypróbować, aby zmniejszyć liczbę instrukcji dołączania w zapytaniu tylko Przenieś potrzebne dane. Lub można przerwać zapytania mniejszych sekwencji podzapytań, na przykład:

**Przed przerwaniem kwerendy:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var customers = from c in context.Customers.Include(c => c.Orders)
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

**Po przerywanie zapytania:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var orders = from o in context.Orders
                 where o.Customer.LastName.StartsWith(lastNameParameter)
                 select o;

    orders.Load();

    var customers = from c in context.Customers
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

Funkcja będzie działać tylko na śledzonych zapytań, jak firma Microsoft wprowadza korzystanie z możliwości kontekst ma automatycznie przeprowadzić korektę rozdzielczość i skojarzenie tożsamości.

Podobnie jak w przypadku ładowania z opóźnieniem, kosztem będzie więcej zapytań dla mniejszych ładunków. Umożliwia także projekcje poszczególne właściwości jawnie wybrać tylko potrzebne dane z każdej jednostki, ale można będzie nie być Trwa ładowanie jednostek w tym przypadku, a aktualizacje nie będą obsługiwane.

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a>8.2.3 obejście pozwalające uzyskać powolne ładowanie właściwości

Entity Framework nie obsługuje obecnie powolne ładowanie właściwości skalarne lub zbyt złożone. Jednak w przypadkach, w którym masz tabelę, która obejmuje dużych obiektów, takich jak obiekt BLOB, umożliwia dzielenie tabeli dzielenie dużych właściwości osobne jednostki. Na przykład załóżmy, że masz tabelę produktu, która zawiera kolumnę zdjęcie varbinary. Jeśli często nie trzeba dostęp do tej właściwości w zapytaniach, można użyć tabeli podział w ramach tylko te części jednostki, które zwykle wymagają. Jednostka reprezentująca fotografia produktu tylko zostaną załadowane, gdy potrzebujesz jawnie.

Dobre zasób, który pokazuje, jak włączyć dzielenia tabeli jest "Tabela podział w programie Entity Framework" Gil Fink wpis w blogu: \< http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.

## <a name="9-other-considerations"></a>9 inne zagadnienia

### <a name="91------server-garbage-collection"></a>9.1 wyrzucanie elementów bezużytecznych serwera

Niektórzy użytkownicy mogą wystąpić rywalizacja, który ogranicza równoległości, muszą być oczekiwana, gdy moduł odśmiecania pamięci nie jest poprawnie skonfigurowana. Zawsze, gdy EF jest używany w scenariuszu wielowątkowych, lub w dowolnej aplikacji, który przypomina systemu po stronie serwera, upewnij się włączyć serwer wyrzucania elementów bezużytecznych. Można to zrobić za pomocą proste ustawienie w pliku konfiguracji aplikacji:

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

To powinno zmniejszyć swoje rywalizacji wątków i zwiększa przepustowość sieci nawet o 30% w scenariuszach procesora CPU jest przepełniony. Ogólnie rzecz biorąc zawsze należy sprawdzić, jak aplikacja zachowuje się przy użyciu klasycznego wyrzucania elementów bezużytecznych (który lepiej jest ona dostrojona dla scenariuszy po stronie interfejsu użytkownika i klienta) oraz serwer wyrzucania elementów bezużytecznych.

### <a name="92------autodetectchanges"></a>9.2 AutoDetectChanges do

Jak wspomniano wcześniej, platformy Entity Framework może wyświetlać problemy z wydajnością, gdy pamięć podręczna obiekt zawiera wiele jednostek. Niektóre operacje, takie jak Dodaj, Usuń, wyszukiwanie, zapis i SaveChanges, wyzwalania wywołań do metody DetectChanges, którego może używać dużej ilości procesora CPU, oparte na wielkości pamięci podręcznej obiektów stał się. Przyczyną jest, że pamięci podręcznej obiektów i obiektu stanu Menedżera próbę pozostają zsynchronizowane po każdej operacji wykonywane do kontekstu, aby produkowane danych może być prawidłowe w obszarze szerokiej gamy scenariuszy.

Zazwyczaj jest dobrą praktyką jest pozostawienie programu Entity Framework automatyczna zmiana wykrywanie włączone dla całego cyklu życia aplikacji. Jeśli scenariusz jest negatywny wpływowi wysokie użycie procesora CPU i profilów wskazują, że przyczyna nadmiernego jest wywołanie metody DetectChanges, należy wziąć pod uwagę tymczasowo wyłączając AutoDetectChanges we fragmencie poufnych kod:

``` csharp
try
{
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    ...
}
finally
{
    context.Configuration.AutoDetectChangesEnabled = true;
}
```

Przed wyłączeniem AutoDetectChanges, warto poznać, może to spowodować Entity Framework utracą możliwość śledzenia określonych informacji o zmiany, które pojawiają się na jednostkach. Jeśli obsługiwane nieprawidłowo, może to spowodować niespójność danych w swojej aplikacji. Aby uzyskać więcej informacji na temat wyłączania AutoDetectChanges, przeczytaj \< http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.

### <a name="93------context-per-request"></a>9.3 kontekst na żądanie

Konteksty platformy Entity Framework są przeznaczone do służyć jako środowisko krótkotrwałe wystąpień w celu zapewnienia najbardziej optymalną wydajność. Konteksty powinny być krótkie krótkotrwałe i usuwane i jako takie zostały zaimplementowane do bardzo i reutilize metadanych, jeśli to możliwe. W scenariuszach sieci web należy pamiętać o tym i nie kontekstu dla więcej niż czas trwania pojedynczego żądania. Podobnie w scenariuszach bez sieci web kontekstu powinny zostać odrzucone zależnie od zrozumieć różne poziomy buforowania w programie Entity Framework. Ogólnie rzecz biorąc jeden unikać występowania wystąpienie kontekstu, w całym cyklu życia aplikacji, a także kontekst na wątek i konteksty statyczne.

### <a name="94------database-null-semantics"></a>9.4 semantyka wartości null bazy danych

Entity Framework domyślnie spowoduje wygenerowanie kodu SQL, który ma C\# wartość null, semantyka porównania. Należy wziąć pod uwagę poniższe przykładowe zapytanie:

``` csharp
            int? categoryId = 7;
            int? supplierId = 8;
            decimal? unitPrice = 0;
            short? unitsInStock = 100;
            short? unitsOnOrder = 20;
            short? reorderLevel = null;

            var q = from p incontext.Products
                    wherep.Category.CategoryName == "Beverages"
                          || (p.CategoryID == categoryId
                                || p.SupplierID == supplierId
                                || p.UnitPrice == unitPrice
                                || p.UnitsInStock == unitsInStock
                                || p.UnitsOnOrder == unitsOnOrder
                                || p.ReorderLevel == reorderLevel)
                    select p;

            var r = q.ToList();
```

W tym przykładzie firma Microsoft porównujemy różne zmienne dopuszczającego wartość null, przed nullable właściwości jednostki, takie jak IDDostawcy i UnitPrice. Wygenerowany SQL dla tego zapytania zostanie wyświetlone pytanie, jeśli wartość tego parametru jest taka sama jak wartość kolumny lub parametru i wartości w kolumnach mają wartość null. To spowoduje ukrycie sposób serwer bazy danych obsługuje wartości null i zapewni spójne C\# wartość null, środowisko pochodzących od różnych dostawców innej bazy danych. Z drugiej strony, wygenerowany kod jest nieco zawiłe i mogą nie działać poprawnie, gdy ilość porównania w klauzuli where instrukcji kwerendy zwiększa się do dużej liczby.

Jest jednym ze sposobów, aby rozwiązać ten problem przy użyciu bazy danych, semantyka wartości null. Należy zauważyć, że to może potencjalnie działają inaczej niż w C\# null semantyki od teraz platformy Entity Framework wygeneruje prostsze SQL Server, który uwidacznia sposób aparatu bazy danych obsługuje wartości null. Semantyka wartości null dla bazy danych może być aktywowany na kontekstowe o jeden wiersz jednej konfiguracji względem kontekstowy konfiguracji:

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

Mały, średni rozmiar zapytania nie będą wyświetlane zwiększenie wydajności zauważalne podczas korzystania z bazy danych, semantyka wartości null, ale różnica staną się bardziej zauważalne w zapytaniach z dużą liczbą potencjalnych porównania wartości null.

W powyższym zapytaniu przykład różnicę w wydajności była mniej niż 2% microbenchmark, działające w środowisku kontrolowanym.

### <a name="95------async"></a>9.5 asynchroniczne

Obsługa Framework 6 wprowadzone jednostki operacji asynchronicznych w przypadku uruchamiania na .NET 4.5 lub nowszej. W większości przypadków aplikacji, które mają we/wy związane z rywalizacji o zasoby będą korzystać maksymalnie z zapytania asynchronicznego i operacje zapisywania. Jeśli aplikacja nie odczuwają rywalizacji o zasoby we/wy, użycie async w przypadku najlepszych były uruchamiane synchronicznie i zwracają wynik, w tym samym czasie jako synchroniczne wywołanie lub w najgorszym przypadku, po prostu Odrocz wykonywania zadania asynchronicznego i dodać dodatkowe tim e do wykonania danego scenariusza.

Instrukcje dotyczące sposobu asynchronicznego programowania pracy, która pomoże przy wyborze rozwiązania, jeśli async poprawi wydajność aplikacji odwiedzanych przez użytkownika [ http://msdn.microsoft.com/library/hh191443.aspx ](https://msdn.microsoft.com/library/hh191443.aspx). Aby uzyskać więcej informacji dotyczących używania operacji asynchronicznych na platformie Entity Framework, zobacz [Async zapytania i Zapisz](~/ef6/fundamentals/async.md
).

### <a name="96------ngen"></a>9.6 NGEN

Entity Framework 6 nie jest dostarczany w domyślnej instalacji programu .NET framework. W efekcie zestawów platformy Entity Framework nie są domyślnie, co oznacza, że cały kod platformy Entity Framework podlega tych samych kosztów JIT'ing jako innego zestawu MSIL przez ngen. Może to obniżyć środowisko F5 podczas opracowywania, a także podczas zimnego uruchamiania aplikacji w środowiskach produkcyjnych. W celu zmniejszenia kosztów procesora CPU i pamięci JIT'ing wskazane jest NGEN obrazy platformy Entity Framework, zgodnie z potrzebami. Aby uzyskać więcej informacji na temat sposobu zwiększenia wydajności uruchamiania programu Entity Framework 6 za pomocą narzędzia NGEN, zobacz [zwiększanie wydajności uruchamiania za pomocą narzędzia NGen](~/ef6/fundamentals/performance/ngen.md).

### <a name="97------code-first-versus-edmx"></a>Zbierając 9,7 najpierw kod i EDMX

Entity Framework przyczyny o problemie niezgodności impedancji między programowania dzięki obiektowej i relacyjnymi bazami danych dzięki reprezentację w pamięci modelu koncepcyjnego (obiekty), schemat magazynu (baza danych) i mapowanie między dwa. Te metadane nosi nazwę modelu Entity Data Model lub EDM w skrócie. Z tym EDM Entity Framework pochodzą widoków do przesyłania danych z obiektów w pamięci do bazy danych i kopii.

Gdy Entity Framework jest używany przy użyciu pliku EDMX oznacza formalnie określa modelu koncepcyjnego, schemat magazynu i mapowanie, a następnie etap podczas ładowania modelu ma tylko do sprawdzania, czy EDM jest poprawny (na przykład, upewnij się, Brak Brak mapowań), następnie Generowanie widoków, a następnie zweryfikować widoków i mają te metadane, które są gotowe do użycia. Wykonania może następnie kwerendę, lub tylko nowe dane zapisane w magazynie danych.

Podejścia Code First jest Centrum, zaawansowane generator modelu Entity Data Model. Entity Framework musi produkować EDM z udostępnionego kodu; robi to analizowanie klas, które są zaangażowane w modelu, stosując konwencje i konfigurowanie modelu za pomocą interfejsu API Fluent. Po utworzeniu EDM Entity Framework zasadniczo zachowuje się taki sam sposób, jak będzie miał już obecne w projekcie pliku EDMX. W związku z tym budowania modelu z Code First dodaje dodatkowe złożoność, która przekłada się na wolniejszych czas uruchamiania programu Entity Framework, w porównaniu z konieczności EDMX. Koszt jest całkowicie zależna od rozmiaru i złożoność modelu, który jest konstruowany.

Wybór użycia EDMX a Code First, jest ważne, aby dowiedzieć się, że elastyczność wprowadzone przez rozwiązanie Code First zwiększa koszt budowania modelu po raz pierwszy. Jeśli aplikacja może wytrzymać koszt tego obciążenia po raz pierwszy to zazwyczaj Code First będą preferowany sposób, aby przejść.

## <a name="10-investigating-performance"></a>10 badanie wydajności

### <a name="101-using-the-visual-studio-profiler"></a>10.1 przy użyciu programu Visual Studio Profiler

Jeśli występują problemy z wydajnością za pomocą programu Entity Framework, można użyć profiler, takiego jak wbudowany w program Visual Studio, aby zobaczyć, gdzie aplikacja spędza czas. To narzędzie, firma Microsoft służącego do generowania wykresów kołowych "Eksplorowanie wydajności programu ADO.NET Entity Framework — część 1" wpis w blogu ( \< http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) ukazują, gdzie Entity Framework spędza czas podczas wykonywania kwerend ścieżce nieaktywnej i bez wyłączania zasilania.

Wpis w blogu "Profilowania platformy Entity Framework przy użyciu Profiler 2010 usługi Visual Studio" napisane przez dane i modelowanie zespół doradczy klientów przedstawiono przykład rzeczywistych jak profiler one używane do badania problemów z wydajnością.  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>. Ten wpis został napisany dla aplikacji systemu windows. Jeśli chcesz profilować aplikację sieci web narzędzi rejestratora wydajności Windows (WPR) i Windows Analizator wydajności (WPA) może działać lepiej niż pracy w programie Visual Studio. WPR i WPA są częścią zestawu narzędzi wydajności Windows, który jest dołączony do Windows Assessment and Deployment Kit ( [ http://www.microsoft.com/en-US/download/details.aspx?id=39982 ](https://www.microsoft.com/en-US/download/details.aspx?id=39982)).

### <a name="102-applicationdatabase-profiling"></a>10.2 profilowanie/bazy danych aplikacji

Narzędzia, takie jak profiler wbudowany w program Visual Studio poinformować Cię, w którym aplikacja jest poświęcania czasu.  Inny rodzaj profiler jest dostępny, przeprowadza analizy dynamicznej uruchomionej aplikacji, zarówno w produkcji wstępnej lub w zależności od potrzeb, a szuka typowych pułapek oraz niezalecane wzorce dostępu do bazy danych.

Dwa komercyjnego profilowania są Profiler Framework jednostki ( \< http://efprof.com>) i ORMProfiler ( \< http://ormprofiler.com>).

Jeśli aplikacja to aplikacja MVC za pomocą funkcji Code First, możesz użyć MiniProfiler StackExchange firmy. Scott Hanselman opis tego narzędzia w jego blog znajduje się na: \< http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.

Aby uzyskać więcej informacji na profilowaniu aktywności bazy danych aplikacji, zobacz artykuł w MSDN Magazine Julie Lerman pod tytułem [profilowania działań w bazie danych platformy Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).

### <a name="103-database-logger"></a>10.3 Rejestrator bazy danych

Jeśli używasz platformy Entity Framework 6 należy również rozważyć korzystanie z funkcji wbudowane funkcje rejestrowania. Właściwość bazy danych kontekstu można zobowiązany do rejestrowania swojej działalności za pośrednictwem prostej konfiguracji jednego wiersza:

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

W tym przykładzie działań w bazie danych będą rejestrowane w konsoli, ale można skonfigurować właściwości rejestrowania, aby wywołać dowolną akcję&lt;ciąg&gt; delegować.

Jeśli chcesz włączyć rejestrowanie w bazie danych bez konieczności ponownego kompilowania i korzystania z programu Entity Framework 6.1 lub nowszej, możesz to zrobić, dodając interceptor w pliku web.config lub app.config aplikacji.

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

Aby uzyskać więcej informacji na temat dodawania rejestrowanie bez konieczności ponownego kompilowania przejdź do pozycji \< http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.

## <a name="11-appendix"></a>Dodatek 11

### <a name="111-a-test-environment"></a>11.1 środowisko testowe A.

To środowisko używa ustawień maszyny 2 z bazy danych na osobnym komputerze od aplikacji klienckiej. Maszyny są w tej samej perspektywy regału sprzętowego, więc opóźnienie sieci jest stosunkowo niska, ale bardziej realistycznego niż środowisku pojedynczego komputera.

#### <a name="1111-------app-server"></a>11.1.1 serwer aplikacji

##### <a name="11111------software-environment"></a>11.1.1.1 środowisko oprogramowania

-   Środowisko oprogramowania programu Entity Framework 4
    -   Nazwa systemu operacyjnego: System Windows Server 2008 R2 Enterprise z dodatkiem SP1.
    -   Program Visual Studio 2010 — Ultimate.
    -   Visual Studio 2010 z dodatkiem SP1 (tylko dla niektórych porównania).
-   Środowisko oprogramowania programu Entity Framework 5 i 6
    -   Nazwa systemu operacyjnego: Windows 8.1 Enterprise
    -   Visual Studio 2013 — Ultimate.

##### <a name="11112------hardware-environment"></a>11.1.1.2 środowisko sprzętu

-   Dwurdzeniowy procesor: Intel(R) Xeon(R) L5520 Procesora W3530 @ 2,27 GHz, 2261 Mhz8 GHz, 4 rdzenie, 84 procesorów logicznych.
-   RamRAM 2412 GB.
-   136 GB SCSI250GB SATA 7200 obr. / min 3GB/s dysku podzielić na 4 partycjami.

#### <a name="1112-------db-server"></a>11.1.2 serwer bazy danych

##### <a name="11121------software-environment"></a>11.1.2.1 środowisko oprogramowania

-   Nazwa systemu operacyjnego: Windows Server 2008 R28.1 Enterprise z dodatkiem SP1.
-   SQL Server 2008 R22012.

##### <a name="11122------hardware-environment"></a>11.1.2.2 środowisko sprzętu

-   Pojedynczy procesor: Intel(R) Xeon(R) Procesora L5520 @ 2,27 GHz, 2261 MhzES-1620 0 @ 3,60 GHz, 4 rdzenie, 8 procesorów logicznych.
-   RamRAM 824 GB.
-   465 GB ATA500GB SATA 7200 obr. / min 6GB/s dysku podzielić na 4 partycjami.

### <a name="112------b-query-performance-comparison-tests"></a>11.2 testy porównanie wydajności zapytań B.

Northwind model była używana do wykonywania tych testów. Został on wygenerowany z bazy danych za pomocą projektanta programu Entity Framework. Następujący kod został użyty porównać wydajność opcje wykonywania zapytań:

``` csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Common;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Data.Objects;
using System.Linq;

namespace QueryComparison
{
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
                );

        public IQueryable<Product> InvokeProductsForCategoryCQ(string categoryName)
        {
            return productsForCategoryCQ(this, categoryName);
        }
    }

    public class QueryTypePerfComparison
    {
        private static string entityConnectionStr = @"metadata=res://*/Northwind.csdl|res://*/Northwind.ssdl|res://*/Northwind.msl;provider=System.Data.SqlClient;provider connection string='data source=.;initial catalog=Northwind;integrated security=True;multipleactiveresultsets=True;App=EntityFramework'";

        public void LINQIncludingContextCreation()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTracking()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                context.Products.MergeOption = MergeOption.NoTracking;

                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void CompiledQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                var q = context.InvokeProductsForCategoryCQ("Beverages");
                q.ToList();
            }
        }

        public void ObjectQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
                products.ToList();
            }
        }

        public void EntityCommand()
        {
            using (EntityConnection eConn = new EntityConnection(entityConnectionStr))
            {
                eConn.Open();
                EntityCommand cmd = eConn.CreateCommand();
                cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

                using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
                {
                    List<Product> productsList = new List<Product>();
                    while (reader.Read())
                    {
                        DbDataRecord record = (DbDataRecord)reader.GetValue(0);

                        // 'materialize' the product by accessing each field and value. Because we are materializing products, we won't have any nested data readers or records.
                        int fieldCount = record.FieldCount;

                        // Treat all products as Product, even if they are the subtype DiscontinuedProduct.
                        Product product = new Product();  

                        product.ProductID = record.GetInt32(0);
                        product.ProductName = record.GetString(1);
                        product.SupplierID = record.GetInt32(2);
                        product.CategoryID = record.GetInt32(3);
                        product.QuantityPerUnit = record.GetString(4);
                        product.UnitPrice = record.GetDecimal(5);
                        product.UnitsInStock = record.GetInt16(6);
                        product.UnitsOnOrder = record.GetInt16(7);
                        product.ReorderLevel = record.GetInt16(8);
                        product.Discontinued = record.GetBoolean(9);

                        productsList.Add(product);
                    }
                }
            }
        }

        public void ExecuteStoreQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectResult<Product> beverages = context.ExecuteStoreQuery<Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Database.SqlQuery\<QueryComparison.DbC.Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbSet()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Products.SqlQuery(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void LINQIncludingContextCreationDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTrackingDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var q = context.Products.AsNoTracking().Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }
    }
}
```

### <a name="113-c-navision-model"></a>Model Navision 11,3 C.

Baza danych Navision jest używana do pokazu Microsoft Dynamics — NAV. dużej bazy danych Wygenerowany model koncepcyjny zawiera 1005 jednostki zestawów i zestawów skojarzeń 4227. Model używany w teście jest "stała" — nie dziedziczenia dodano do niego.

#### <a name="1131-queries-used-for-navision-tests"></a>11.3.1 zapytania używany do testów Navision

Lista zapytań, używany przy użyciu modelu Navision zawiera 3 kategorii zapytań jednostki SQL:

##### <a name="11311-lookup"></a>11.3.1.1 wyszukiwanie

Proste wyszukiwanie zapytania nie agregacji

-   Liczba: 16232
-   Przykład:

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312-singleaggregating"></a>11.3.1.2 SingleAggregating

Normalne BI zapytanie o wiele agregacji, ale nie sumy częściowe (jedno zapytanie)

-   Liczba: 2313
-   Przykład:

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

Gdzie MDF\_SessionLogin\_czasu\_Max() jest zdefiniowany w modelu w postaci:

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313-aggregatingsubtotals"></a>11.3.1.3 AggregatingSubtotals

Zapytanie analizy Biznesowej za pomocą agregacji i sumy częściowe (za pośrednictwem wszystkich Unia)

-   Liczba: 178
-   Przykład:

``` xml
  <Query complexity="AggregatingSubtotals">
    <CommandText>
using NavisionFK;
function AmountConsumed(entities Collection([CRONUS_International_Ltd__Zone])) as
(
    Edm.Sum(select value N.Block_Movement FROM entities as E, E.CRONUS_International_Ltd__Bin as N)
)
function AmountConsumed(P1 Edm.Int32) as
(
    AmountConsumed(select value e from NavisionFKContext.CRONUS_International_Ltd__Zone as e where e.Zone_Ranking = P1)
)
----------------------------------------------------------------------------------------------------------------------
(
    select top(10) Zone_Ranking, Cross_Dock_Bin_Zone, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking, E.Cross_Dock_Bin_Zone
)
union all
(
    select top(10) Zone_Ranking, Cast(null as Edm.Byte) as P2, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking
)
union all
{
    Row(Cast(null as Edm.Int32) as P1, Cast(null as Edm.Byte) as P2, AmountConsumed(select value E
                                                                         from NavisionFKContext.CRONUS_International_Ltd__Zone as E
                                                                         where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed))
}</CommandText>
    <Parameters>
      <Parameter Name="MinAmountConsumed" DbType="Int32" Value="10000" />
    </Parameters>
  </Query>
```
