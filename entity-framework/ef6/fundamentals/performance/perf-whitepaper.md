---
title: Zagadnienia dotyczące wydajności dla EF4, EF5 i EF6-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: 07eb605f0d39f0c1bcfe781540525180f0dd0b22
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181666"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a>Zagadnienia dotyczące wydajności dla EF 4, 5 i 6
Przez David Obando, Eric Dettinger i inne

Publikacj Kwiecień 2012

Ostatnia aktualizacja: 2014 maja

------------------------------------------------------------------------

## <a name="1-introduction"></a>1. Wprowadzenie

Struktury mapowania relacyjnego obiektów są wygodnym sposobem zapewnienia abstrakcji dostępu do danych w aplikacji zorientowanej obiektowo. W przypadku aplikacji .NET zalecana firma Microsoft ma Entity Framework. W przypadku jakichkolwiek streszczeń, wydajność może być istotna.

Niniejszy dokument został zapisany, aby pokazać zagadnienia dotyczące wydajności podczas tworzenia aplikacji przy użyciu Entity Framework, aby dać deweloperom pomysł Entity Framework wewnętrznych algorytmów, które mogą wpływać na wydajność, oraz zapewnić wskazówki dotyczące badania i Poprawa wydajności w aplikacjach korzystających z Entity Framework. Istnieje kilka dobrych tematów dotyczących wydajności już dostępnych w sieci Web. Ponadto podjęto próbę przeprowadzenia tych zasobów, jeśli jest to możliwe.

Wydajność to Lewa sekcja. Ten oficjalny dokument jest przeznaczony dla zasobów, które ułatwiają podejmowanie decyzji dotyczących wydajności dla aplikacji korzystających z Entity Framework. Dodaliśmy pewne metryki testów w celu zademonstrowania wydajności, ale te metryki nie zamierzą bezwzględnych wskaźników wydajności, które będą widoczne w aplikacji.

W praktyce w tym dokumencie przyjęto, że Entity Framework 4 jest uruchamiany na platformie .NET 4,0 i Entity Framework 5 i 6 są uruchamiane w ramach platformy .NET 4,5. Wiele ulepszeń wydajności Entity Framework 5 znajduje się w podstawowych składnikach dostarczanych z platformą .NET 4,5.

Entity Framework 6 to wersja poza pasmem i nie zależy od składników Entity Framework dostarczanych z platformą .NET. Entity Framework 6 działa na platformach .NET 4,0 i .NET 4,5 oraz oferuje dużą wydajność dla tych, którzy nie zostali uaktualnioni z platformy .NET 4,0, ale chcą mieć najnowsze Entity Framework BITS w swojej aplikacji. W tym dokumencie znajduje się Entity Framework 6, odnosi się do najnowszej wersji dostępnej w momencie zapisu: wersja 6.1.0.

## <a name="2-cold-vs-warm-query-execution"></a>2. Zimny a Wykonywanie zapytania ciepłego

Podczas pierwszego wykonywania zapytania dotyczącego danego modelu, Entity Framework wykonuje wiele pracy w tle, aby załadować i zweryfikować model. Często odwołujemy się do pierwszego zapytania jako "zimne".  Dalsze zapytania dotyczące już załadowanego modelu są znane jako zapytania "grzane" i są znacznie szybsze.

Przyjrzyjmy się ogólnemu w miejscu, gdzie podczas wykonywania zapytania przy użyciu Entity Framework i zobacz, w jaki sposób poprawiamy działania w Entity Framework 6.

**Pierwsze wykonanie zapytania — zimna kwerenda**

| Kod zapisy użytkownika                                                                                     | Action                    | Wpływ na wydajność EF4                                                                                                                                                                                                                                                                                                                                                                                                        | Wpływ na wydajność EF5                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Wpływ na wydajność EF6                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Tworzenie kontekstu          | Średni                                                                                                                                                                                                                                                                                                                                                                                                                        | Średni                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Małe                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Tworzenie wyrażenia zapytania | Małe                                                                                                                                                                                                                                                                                                                                                                                                                           | Małe                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Małe                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | Wykonanie zapytania LINQ      | -Ładowanie metadanych: Wysoki, ale w pamięci podręcznej <br/> -Wyświetl generowanie: Prawdopodobnie bardzo wysokie, ale w pamięci podręcznej <br/> -Obliczanie parametrów: Średni <br/> -Tłumaczenie zapytania: Średni <br/> -Materializer generacji: Średni, ale buforowany <br/> -Wykonywanie zapytania dotyczącego bazy danych: Potencjalnie wysoka <br/> + Połączenie. Otwórz <br/> + Command. ExecuteReader <br/> + DataReader.Read <br/> Materializację obiektu: Średni <br/> -Wyszukiwanie tożsamości: Średni | -Ładowanie metadanych: Wysoki, ale w pamięci podręcznej <br/> -Wyświetl generowanie: Prawdopodobnie bardzo wysokie, ale w pamięci podręcznej <br/> -Obliczanie parametrów: Małe <br/> -Tłumaczenie zapytania: Średni, ale buforowany <br/> -Materializer generacji: Średni, ale buforowany <br/> -Wykonywanie zapytania dotyczącego bazy danych: Potencjalnie wysoka (lepsze zapytania w niektórych sytuacjach) <br/> + Połączenie. Otwórz <br/> + Command. ExecuteReader <br/> + DataReader.Read <br/> Materializację obiektu: Średni <br/> -Wyszukiwanie tożsamości: Średni | -Ładowanie metadanych: Wysoki, ale w pamięci podręcznej <br/> -Wyświetl generowanie: Średni, ale buforowany <br/> -Obliczanie parametrów: Małe <br/> -Tłumaczenie zapytania: Średni, ale buforowany <br/> -Materializer generacji: Średni, ale buforowany <br/> -Wykonywanie zapytania dotyczącego bazy danych: Potencjalnie wysoka (lepsze zapytania w niektórych sytuacjach) <br/> + Połączenie. Otwórz <br/> + Command. ExecuteReader <br/> + DataReader.Read <br/> Materializację obiektu: Średni (szybszy niż EF5) <br/> -Wyszukiwanie tożsamości: Średni |
| `}`                                                                                                  | Połączenie. Zamknij          | Małe                                                                                                                                                                                                                                                                                                                                                                                                                           | Małe                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Małe                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


**Drugie wykonanie zapytania — grzane zapytanie**

| Kod zapisy użytkownika                                                                                     | Action                    | Wpływ na wydajność EF4                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Wpływ na wydajność EF5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Wpływ na wydajność EF6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Tworzenie kontekstu          | Średni                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Średni                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Małe                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Tworzenie wyrażenia zapytania | Małe                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Małe                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Małe                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | Wykonanie zapytania LINQ      | -Wyszukiwanie ~~ładowania~~ metadanych: ~~Wysoki, ale w pamięci podręcznej~~ Małą <br/> -Wyszukiwanie ~~generowania~~ widoku: ~~Prawdopodobnie bardzo wysokie, ale w pamięci podręcznej~~ Małą <br/> -Obliczanie parametrów: Średni <br/> -Wyszukiwanie ~~tłumaczenia~~ zapytania: Średni <br/> -Materializer ~~generacji~~ : ~~Średni, ale buforowany~~ Małą <br/> -Wykonywanie zapytania dotyczącego bazy danych: Potencjalnie wysoka <br/> + Połączenie. Otwórz <br/> + Command. ExecuteReader <br/> + DataReader.Read <br/> Materializację obiektu: Średni <br/> -Wyszukiwanie tożsamości: Średni | -Wyszukiwanie ~~ładowania~~ metadanych: ~~Wysoki, ale w pamięci podręcznej~~ Małą <br/> -Wyszukiwanie ~~generowania~~ widoku: ~~Prawdopodobnie bardzo wysokie, ale w pamięci podręcznej~~ Małą <br/> -Obliczanie parametrów: Małe <br/> -Wyszukiwanie ~~tłumaczenia~~ zapytania: ~~Średni, ale buforowany~~ Małą <br/> -Materializer ~~generacji~~ : ~~Średni, ale buforowany~~ Małą <br/> -Wykonywanie zapytania dotyczącego bazy danych: Potencjalnie wysoka (lepsze zapytania w niektórych sytuacjach) <br/> + Połączenie. Otwórz <br/> + Command. ExecuteReader <br/> + DataReader.Read <br/> Materializację obiektu: Średni <br/> -Wyszukiwanie tożsamości: Średni | -Wyszukiwanie ~~ładowania~~ metadanych: ~~Wysoki, ale w pamięci podręcznej~~ Małą <br/> -Wyszukiwanie ~~generowania~~ widoku: ~~Średni, ale buforowany~~ Małą <br/> -Obliczanie parametrów: Małe <br/> -Wyszukiwanie ~~tłumaczenia~~ zapytania: ~~Średni, ale buforowany~~ Małą <br/> -Materializer ~~generacji~~ : ~~Średni, ale buforowany~~ Małą <br/> -Wykonywanie zapytania dotyczącego bazy danych: Potencjalnie wysoka (lepsze zapytania w niektórych sytuacjach) <br/> + Połączenie. Otwórz <br/> + Command. ExecuteReader <br/> + DataReader.Read <br/> Materializację obiektu: Średni (szybszy niż EF5) <br/> -Wyszukiwanie tożsamości: Średni |
| `}`                                                                                                  | Połączenie. Zamknij          | Małe                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Małe                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Małe                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


Istnieje kilka sposobów zmniejszenia kosztów wydajności zarówno w przypadku zapytań zimnych, jak i ciepłej. Zapoznaj się z nimi w poniższej sekcji. Zapoznaj się z tym, jak zmniejszyć koszt ładowania modelu w zimnych zapytaniach, używając wstępnie wygenerowanych widoków, które powinny pomóc w zmniejszeniu wydajności podczas generowania widoku. W przypadku zapytań ciepłej będziemy obejmować buforowanie planu zapytania, brak zapytań śledzenia i różne opcje wykonywania zapytania.

### <a name="21-what-is-view-generation"></a>2,1 co to jest generacja widoku?

Aby zrozumieć, jaka jest generacja widoku, należy najpierw zrozumieć, co to jest "widoki mapowania". Widoki mapowania są wykonywalnymi reprezentacjami przekształceń określonych w mapowaniu dla każdego zestawu jednostek i skojarzenia. Wewnętrznie te widoki mapowania przyjmują kształt CQTs (kanoniczne drzewa zapytań). Istnieją dwa typy widoków mapowania:

-   Widoki zapytań: reprezentuje transformację niezbędną do przechodzenia ze schematu bazy danych do modelu koncepcyjnego.
-   Aktualizuj widoki: reprezentuje transformację niezbędną do przechodzenia z modelu koncepcyjnego do schematu bazy danych.

Należy pamiętać, że model koncepcyjny może się różnić od schematu bazy danych na różne sposoby. Na przykład jedna tabela może być używana do przechowywania danych dla dwóch różnych typów jednostek. Dziedziczenie i nieuproszczone mapowania odgrywają rolę w złożoności widoków mapowania.

Proces przetwarzania tych widoków na podstawie specyfikacji mapowania to to, co wywołujemy generowanie widoku. Generowanie widoku może być wykonywane dynamicznie podczas ładowania modelu lub w czasie kompilacji przy użyciu "wstępnie wygenerowanych widoków"; drugi z nich jest serializowany w postaci instrukcji Entity SQL do pliku C @ no__t-0 lub VB.

Po wygenerowaniu widoków są one również weryfikowane. Z punktu widzenia wydajności zdecydowana większość kosztów generowania widoku jest w rzeczywistości walidacją widoków, które zapewniają, że połączenia między jednostkami mają sens i mają prawidłową Kardynalność dla wszystkich obsługiwanych operacji.

Gdy wykonywane jest zapytanie nad zestawem jednostek, zapytanie jest połączone z odpowiednim widokiem zapytania, a wynik tej kompozycji jest uruchamiany za pośrednictwem kompilatora planu, aby utworzyć reprezentację zapytania, które może zrozumieć magazyn zapasowy. W przypadku SQL Server końcowy wynik tej kompilacji będzie instrukcją SELECT języka T-SQL. Podczas pierwszego wykonywania aktualizacji przez zestaw jednostek widok aktualizacji jest uruchamiany przez podobny proces, aby przekształcić go w Instrukcje DML dla docelowej bazy danych.

### <a name="22-factors-that-affect-view-generation-performance"></a>2,2 czynników wpływających na wydajność generowania widoku

Krok generowania widoku wydajności nie tylko zależy od rozmiaru modelu, ale również od tego, jak jest on połączony z modelem. Jeśli dwie jednostki są połączone za pośrednictwem łańcucha dziedziczenia lub skojarzenia, są one znane jako połączone. Podobnie, jeśli dwie tabele są połączone za pośrednictwem klucza obcego, są one połączone. Wraz ze wzrostem liczby połączonych jednostek i tabel w schematach zwiększa się koszt generowania widoku.

Algorytm używany do generowania i weryfikowania widoków jest wykładniczy w najgorszym przypadku, chociaż używamy pewnych optymalizacji, aby usprawnić ten proces. Największe czynniki wpływające negatywnie na wydajność są następujące:

-   Rozmiar modelu, odnoszący się do liczby jednostek i ilości skojarzeń między tymi jednostkami.
-   Złożoność modelu, w tym dziedziczenie obejmujące dużą liczbę typów.
-   Używanie niezależnych skojarzeń zamiast skojarzeń kluczy obcych.

W przypadku małych modeli prosta koszt może być wystarczająco mały, aby nie bother przy użyciu wstępnie wygenerowanych widoków. W miarę wzrostu rozmiaru modelu i stopnia złożoności dostępnych jest kilka opcji zmniejszania kosztów generowania i walidacji widoku.

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a>2,3 użycie wstępnie wygenerowanych widoków w celu zmniejszenia czasu ładowania modelu

Aby uzyskać szczegółowe informacje na temat używania wstępnie wygenerowanych widoków w Entity Framework 6 odwiedź [wstępnie wygenerowane widoki mapowania](~/ef6/fundamentals/performance/pre-generated-views.md)

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools-community-edition"></a>2.3.1 wstępnie wygenerowane widoki przy użyciu Entity Framework narzędzia do zarządzania wersjami Community

Aby wygenerować widoki modeli EDMX i Code First, można użyć [wersji Community Tools programu Entity Framework 6](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) , klikając prawym przyciskiem myszy plik klasy modelu i używając menu Entity Framework, aby wybrać polecenie "Generuj widoki". Entity Framework narzędzia do zarządzania wersjami Professional działają tylko w kontekstach pochodnych DbContext.

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a>2.3.2 użycie wstępnie wygenerowanych widoków z modelem utworzonym przez EDMGen

EDMGen to narzędzie, które jest dostarczane z platformą .NET i współpracuje z Entity Framework 4 i 5, ale nie z Entity Framework 6. EDMGen umożliwia wygenerowanie pliku modelu, warstwy obiektu i widoków z wiersza polecenia. Jednym z danych wyjściowych będzie plik widoków w wybranym języku, VB lub C @ no__t-0. Jest to plik kodu zawierający Entity SQL fragmenty kodu dla każdego zestawu jednostek. Aby włączyć wstępnie wygenerowane widoki, wystarczy dołączyć plik do projektu.

Jeśli ręcznie wprowadzisz zmiany do plików schematu dla modelu, konieczne będzie ponowne wygenerowanie pliku widoków. Można to zrobić, uruchamiając EDMGen z flagą **/Mode: ViewGeneration** .

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a>2.3.3, jak używać wstępnie wygenerowanych widoków z plikiem EDMX

Można również użyć EDMGen do wygenerowania widoków dla pliku EDMX — w poprzednim temacie opisano, jak dodać do tego celu zdarzenie sprzed kompilacji, ale jest to skomplikowane, a w niektórych przypadkach nie jest to możliwe. Zwykle łatwiej jest używać szablonu T4 do generowania widoków, gdy model znajduje się w pliku edmx.

Blog zespołu programu ADO.NET ma wpis, który opisuje sposób używania szablon T4 do generowania widoku ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>). Ten wpis obejmuje szablon, który można pobrać i dodać do projektu. Szablon został zapisany dla pierwszej wersji Entity Framework, więc nie gwarantujemy pracy z najnowszymi wersjami Entity Framework. Można jednak pobrać bardziej aktualny zestaw szablonów generacji widoku dla Entity Framework 4 i 5from galerię programu Visual Studio:

-   VB.NET: \< @ NO__T-1
-   C @ NO__T-0: \< @ NO__T-2

Jeśli używasz platformy Entity Framework 6 można uzyskać widok szablony T4 generacji z galerii Visual Studio na \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.

### <a name="24-reducing-the-cost-of-view-generation"></a>2,4 zmniejszenie kosztów generowania widoku

Użycie wstępnie wygenerowanych widoków przenosi koszt generowania widoku z ładowania modelu (czasu wykonywania) do czasu projektowania. Chociaż zwiększa to wydajność uruchamiania w czasie wykonywania, nadal będziesz mieć problemy z generowaniem widoku podczas opracowywania. Istnieje kilka dodatkowych lew, które mogą pomóc w zmniejszeniu kosztów generowania widoku zarówno w czasie kompilacji, jak i w czasie wykonywania.

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a>2.4.1 użycie skojarzenia klucza obcego w celu zmniejszenia kosztów generowania widoku

Napotkano wiele przypadków, w których zmiana skojarzeń w modelu z niezależnych skojarzeń z kluczami obcym znacznie poprawiła czas poświęcony na generowanie widoku.

Aby zademonstrować to ulepszenie, Wygenerowano dwie wersje modelu systemu Navision przy użyciu EDMGen. *Uwaga: Aby zapoznać się z opisem modelu systemu Navision, zobacz Dodatek C.* Model systemu Navision jest interesujący dla tego ćwiczenia z powodu bardzo dużej liczby jednostek i relacji między nimi.

Jedna wersja tego bardzo dużego modelu została wygenerowana przy użyciu skojarzeń kluczy obcych, a druga została wygenerowana z niezależnymi skojarzeniami. Następnie przekroczyć czas trwania generowania widoków dla każdego modelu. Entity Framework 5 test użył metody GenerateViews () z klasy EntityViewGenerator do wygenerowania widoków, podczas gdy test Entity Framework 6 użył metody GenerateViews () z klasy StorageMappingItemCollection. Dzieje się tak z powodu restrukturyzacji kodu, która wystąpiła w bazie kodu Entity Framework 6.

Przy użyciu Entity Framework 5, generowanie widoku dla modelu z użyciem kluczy obcych trwało 65 minut na maszynie laboratoryjnej. Jest to nieznane, jak długo zostałyby wykonane w celu wygenerowania widoków dla modelu, który używa niezależnych skojarzeń. Pozostawiłem test uruchomiony przez ponad miesiąc przed ponownym uruchomieniem maszyny w naszym laboratorium w celu zainstalowania comiesięcznych aktualizacji.

Przy użyciu Entity Framework 6, generowanie widoku dla modelu z kluczami obcymi trwało 28 sekund na tej samej maszynie laboratoryjnej. Generowanie widoku dla modelu, który używa niezależnych skojarzeń zajęło 58 sekund. Ulepszenia wykonywane w Entity Framework 6 w swoim kodzie generacji widoku oznaczają, że wiele projektów nie potrzebuje wstępnie wygenerowanych widoków w celu uzyskania krótszych czasu uruchamiania.

Ważne jest, aby zwrócić uwagę, że wstępnie generowane widoki w Entity Framework 4 i 5 można wykonać za pomocą EDMGen lub Entity Framework narzędzia do zarządzania. Generowanie widoku Entity Framework 6 można przeprowadzić za pomocą narzędzi Entity Framework lub programowo, zgodnie z opisem w [wstępnie wygenerowanych widokach mapowania](~/ef6/fundamentals/performance/pre-generated-views.md).

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a>2.4.1.1, jak używać kluczy obcych zamiast niezależnych skojarzeń

W przypadku korzystania z EDMGen lub Entity Designer w programie Visual Studio, domyślnie otrzymujesz FKs i tylko jedno pole wyboru lub flagę wiersza polecenia, aby przełączać się między FKs i IAs.

Jeśli masz duży model Code First, użycie niezależnych skojarzeń będzie miało ten sam wpływ na generowanie widoku. Można uniknąć tego wpływu, dołączając właściwości klucza obcego klas dla obiektów zależnych, chociaż niektórzy deweloperzy rozważą to zanieczyszczenie modelu obiektów. Można znaleźć więcej informacji na ten temat w \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.

| W przypadku korzystania z      | Zrób to                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Entity Designer | Po dodaniu skojarzenia między dwiema jednostkami upewnij się, że masz ograniczenie referencyjne. Więzy referencyjne informują Entity Framework o użyciu kluczy obcych zamiast niezależnych skojarzeń. Aby uzyskać więcej informacji, odwiedź stronę \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>. |
| EDMGen          | W przypadku generowania plików z bazy danych przy użyciu programu EDMGen klucze obce będą przestrzegane i dodawane do modelu. Aby uzyskać więcej informacji na temat różnych opcji udostępnianych przez EDMGen odwiedź stronę [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx).                           |
| Code First      | Zapoznaj się z sekcją "Konwencja relacji" tematu [Code First Konwencji](~/ef6/modeling/code-first/conventions/built-in.md) , aby uzyskać informacje na temat dołączania właściwości klucza obcego do obiektów zależnych podczas korzystania z Code First.                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a>2.4.2 przeniesienie modelu do oddzielnego zestawu

Gdy model zostanie uwzględniony bezpośrednio w projekcie aplikacji, a widoki są generowane za pomocą zdarzenia przed kompilacją lub szablonu T4, generowanie i walidacja widoku są przeprowadzane zawsze wtedy, gdy projekt zostanie odbudowany, nawet jeśli model nie został zmieniony. Jeśli przeniesiesz model do oddzielnego zestawu i odwołujesz się do niego z projektu aplikacji, możesz wprowadzić inne zmiany w aplikacji bez konieczności ponownego kompilowania projektu zawierającego model.

*Uwaga:*  when przenieść model do oddzielnych zestawów Pamiętaj, aby skopiować parametry połączenia dla modelu do pliku konfiguracji aplikacji projektu klienta.

#### <a name="243-disable-validation-of-an-edmx-based-model"></a>2.4.3 Wyłącz weryfikację modelu opartego na edmx

Modele EDMX są weryfikowane w czasie kompilacji, nawet jeśli model nie został zmieniony. Jeśli Twój model został już zweryfikowany, możesz pominąć walidację w czasie kompilacji, ustawiając właściwość "Weryfikuj przy kompilacji" na wartość false w oknie właściwości. Gdy zmienisz mapowanie lub model, możesz tymczasowo ponownie włączyć weryfikację, aby zweryfikować zmiany.

Należy zauważyć, że wprowadzono ulepszenia wydajności w Entity Framework Designer dla Entity Framework 6, a koszt "Walidacja w kompilacji" jest znacznie mniejszy niż w poprzednich wersjach projektanta.

## <a name="3-caching-in-the-entity-framework"></a>3 buforowanie w Entity Framework

Entity Framework ma następujące formy buforowania:

1.  Buforowanie obiektów — obiekt ObjectStateManager wbudowany w wystąpienie obiektu ObjectContext śledzi w pamięci pamięć obiektów, które zostały pobrane przy użyciu tego wystąpienia. Jest to również nazywane pamięcią podręczną pierwszego poziomu.
2.  Buforowanie planu zapytania — ponowne użycie wygenerowanego magazynu, gdy zapytanie jest wykonywane więcej niż raz.
3.  Buforowanie metadanych — udostępnia metadane dla modelu w różnych połączeniach z tym samym modelem.

Oprócz pamięci podręcznych, które są dostępne w ramach usługi Box, można również użyć specjalnego rodzaju dostawcy danych ADO.NET, znanego jako dostawca otoki, w celu rozbudowania Entity Framework z pamięcią podręczną dla wyników pobranych z bazy danych, znanej również jako buforowanie drugiego poziomu.

### <a name="31-object-caching"></a>Buforowanie obiektów 3,1

Domyślnie, gdy jednostka jest zwracana w wynikach zapytania, tuż przed Dr materializuje, obiekt ObjectContext sprawdzi, czy jednostka z tym samym kluczem została już załadowana do jego obiektu ObjectStateManager. Jeśli jednostka z tymi samymi kluczami już istnieje, będzie uwzględniać ją w wynikach zapytania. Mimo że Dr nadal będzie wysyłać zapytanie względem bazy danych, takie zachowanie może ominąć wiele kosztów materializacji jednostki.

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a>3.1.1 pobieranie jednostek z pamięci podręcznej obiektów przy użyciu funkcji DbContext Find

W przeciwieństwie do zwykłego zapytania, Metoda Find w Nieogólnymi (interfejsy API dołączone po raz pierwszy w EF 4,1) przeprowadzi wyszukiwanie w pamięci przed nawet wygenerowaniem zapytania względem bazy danych. Należy pamiętać, że dwa różne wystąpienia obiektu ObjectContext będą miały dwa różne wystąpienia obiektu ObjectStateManager, co oznacza, że mają osobne pamięci podręczne obiektów.

Znajdź używa wartości klucza podstawowego, aby spróbować znaleźć jednostkę śledzoną przez kontekst. Jeśli jednostka nie znajduje się w kontekście, zapytanie zostanie wykonane i ocenione względem bazy danych, a wartość null jest zwracana, jeśli jednostka nie zostanie znaleziona w kontekście lub w bazie danych. Należy zauważyć, że funkcja Find zwraca również jednostki, które zostały dodane do kontekstu, ale nie zostały jeszcze zapisane w bazie danych.

W przypadku korzystania z funkcji Znajdź należy wziąć pod uwagę wydajność. Wywołania tej metody domyślnie wyzwalają weryfikację pamięci podręcznej obiektów w celu wykrycia zmian, które nadal oczekują na zatwierdzenie w bazie danych. Ten proces może być bardzo kosztowny, jeśli istnieje bardzo duża liczba obiektów w pamięci podręcznej obiektów lub wykres dużego obiektu dodawany do pamięci podręcznej obiektów, ale można go również wyłączyć. W niektórych przypadkach można postrzegać kolejność o wielkości różnicy w wywołaniu metody Find po wyłączeniu zmian autowykrywania. Jeszcze drugi porządek wielkości jest postrzegany, gdy obiekt rzeczywiście znajduje się w pamięci podręcznej, a kiedy obiekt musi zostać pobrany z bazy danych. Oto przykładowy wykres z miarami wykonywanymi przy użyciu niektórych mikrotestów porównawczych wyrażonych w milisekundach, z obciążeniem jednostek 5000:

.NET ![4,5 Skala logarytmiczna](~/ef6/media/net45logscale.png ".NET 4,5 — Skala logarytmiczna")

Przykład wyszukiwania z wyłączonymi zmianami autowykrywania:

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

Co należy wziąć pod uwagę podczas korzystania z metody Find:

1.  Jeśli obiekt nie znajduje się w pamięci podręcznej, korzyści z znalezienia są negacji, ale składnia jest nadal łatwiejsza niż kwerenda według klucza.
2.  Jeśli funkcja autowykrywania zmian jest włączona, koszt metody Find może wzrosnąć o jeden porządek wielkości lub nawet więcej w zależności od złożoności modelu i liczby jednostek w pamięci podręcznej obiektów.

Należy również pamiętać, że funkcja Znajdź zwraca tylko jednostkę, której szukasz, i nie ładuje jej automatycznie, jeśli nie znajdują się jeszcze w pamięci podręcznej obiektów. Jeśli musisz pobrać skojarzone jednostki, możesz użyć zapytania przez klucz z ładowaniem eager. Aby uzyskać więcej informacji, zobacz **8,1 opóźnione ładowanie a Eager ładowania @ no__t-0.

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a>3.1.2 problemy z wydajnością, gdy pamięć podręczna obiektów ma wiele jednostek

Pamięć podręczna obiektów ułatwia zwiększenie ogólnej reakcji Entity Framework. Jednak w przypadku, gdy pamięć podręczna obiektów ma załadowane bardzo duże ilości jednostek, może mieć wpływ na niektóre operacje, takie jak dodawanie, usuwanie, Znajdowanie, wprowadzanie, metody SaveChanges i inne. W szczególności operacje wyzwalające wywołanie DetectChanges będą mieć negatywny wpływ na bardzo duże pamięci podręczne obiektów. DetectChanges synchronizuje Graf obiektów z menedżerem stanu obiektów, a jego wydajność zostanie określona bezpośrednio przez rozmiar grafu obiektów. Aby uzyskać więcej informacji na temat DetectChanges, zobacz [śledzenie zmian w jednostkach poco](https://msdn.microsoft.com/library/dd456848.aspx).

W przypadku korzystania z Entity Framework 6 deweloperzy mogą wywołać metodę AddRange i RemoveRange bezpośrednio w Nieogólnymi, a nie iterację w kolekcji i wywołać Dodawanie raz dla każdego wystąpienia. Zaletą korzystania z metod Range jest to, że koszt usługi DetectChanges jest płatny tylko raz dla całego zestawu jednostek, a nie raz na każdą dodaną jednostkę.

### <a name="32-query-plan-caching"></a>Buforowanie planu zapytania 3,2

Gdy zapytanie jest wykonywane po raz pierwszy, przechodzi przez kompilator wewnętrznego planu, aby przetłumaczyć zapytanie koncepcyjne na polecenie magazynu (na przykład T-SQL, który jest wykonywany w przypadku uruchomienia względem SQL Server).  Jeśli buforowanie planu zapytania jest włączone, przy następnym wykonywaniu zapytania polecenie magazynu jest pobierane bezpośrednio z pamięci podręcznej planu zapytania na potrzeby wykonywania, pomijając kompilator planu.

Pamięć podręczna planu zapytania jest współdzielona przez wystąpienia obiektu ObjectContext w ramach tego samego elementu AppDomain. Nie trzeba przytrzymać wystąpienia obiektu ObjectContext, aby można było korzystać z buforowania planu zapytania.

#### <a name="321-some-notes-about-query-plan-caching"></a>3.2.1 niektóre uwagi dotyczące buforowania planu zapytania

-   Pamięć podręczna planu zapytania jest udostępniana dla wszystkich typów zapytań: Entity SQL, LINQ to Entities i obiekty CompiledQuery.
-   Domyślnie buforowanie planu zapytania jest włączone dla zapytań Entity SQL, niezależnie od tego, czy są wykonywane za pomocą EntityCommand, czy ObjectQuery. Jest on również domyślnie włączony dla zapytań LINQ to Entities w Entity Framework na platformie .NET 4,5 i w Entity Framework 6.
    -   Buforowanie planu zapytania można wyłączyć, ustawiając właściwość EnablePlanCaching (w EntityCommand lub ObjectQuery) na wartość false. Na przykład:
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
-   W przypadku zapytań parametrycznych zmiana wartości parametru będzie nadal trafiać na zbuforowane zapytanie. Ale zmiana aspektów parametru (na przykład size, Precision lub Scale) spowoduje, że zostanie osiągnięty inny wpis w pamięci podręcznej.
-   W przypadku korzystania z Entity SQL ciąg zapytania jest częścią klucza. Zmiana zapytania w ogóle spowoduje powstanie różnych wpisów w pamięci podręcznej, nawet jeśli zapytania są funkcjonalnie równoważne. Obejmuje to zmiany wielkości liter lub białych znaków.
-   W przypadku korzystania z LINQ zapytanie jest przetwarzane w celu wygenerowania części klucza. Zmiana wyrażenia LINQ spowoduje wygenerowanie innego klucza.
-   Mogą być stosowane inne ograniczenia techniczne; Zobacz autokompilowane zapytania, aby uzyskać więcej szczegółów.

#### <a name="322-cache-eviction-algorithm"></a>3.2.2 algorytm wykluczenia pamięci podręcznej

Zrozumienie, jak działa wewnętrzny algorytm pomoże Ci ustalić, kiedy należy włączyć lub wyłączyć buforowanie planu zapytania. Algorytm oczyszczania jest następujący:

1.  Gdy pamięć podręczna zawiera określoną liczbę wpisów (800), uruchamiamy czasomierz, który okresowo (raz na minutę) czyść pamięć podręczną.
2.  Podczas czyszczenia pamięci podręcznej wpisy są usuwane z pamięci podręcznej na LFRU (ostatnio używane). Ten algorytm pobiera zarówno liczbę trafień, jak i wiek do konta podczas wybierania wpisów, które zostały wysunięte.
3.  Po zakończeniu każdego odchylenia pamięci podręcznej pamięć podręczna zawiera 800 wpisów.

Wszystkie wpisy pamięci podręcznej są traktowane równomiernie podczas określania wpisów do wykluczenia. Oznacza to, że polecenie magazynu dla CompiledQuery ma tę samą szansę wykluczenia jako polecenie magazynu dla zapytania Entity SQL.

Należy zauważyć, że czasomierz wykluczenia pamięci podręcznej jest uruchamiany, gdy w pamięci podręcznej znajdują się 800 jednostek, ale pamięć podręczna jest uruchamiana dopiero po upływie 60 sekund od momentu uruchomienia tego czasomierza. Oznacza to, że przez maksymalnie 60 sekund pamięć podręczna może być coraz większa.

#### <a name="323-test-metrics-demonstrating-query-plan-caching-performance"></a>3.2.3 metryki testów ukazujące wydajność buforowania planu zapytania

Aby zademonstrować efekt buforowania planu zapytania względem wydajności aplikacji, przeprowadzono test, w którym wykonano kilka Entity SQL zapytań względem modelu systemu Navision. Zapoznaj się z załącznikiem opis modelu systemu Navision i typy zapytań, które zostały wykonane. W tym teście najpierw wykonujemy iterację na liście zapytań i wykonują każdy raz raz, aby dodać je do pamięci podręcznej (Jeśli buforowanie jest włączone). Ten krok jest niepełny. Następnie uśpienie głównego wątku przez ponad 60 sekund, aby umożliwić czyszczenie pamięci podręcznej; na koniec wykonujemy iterację na liście po raz drugi, aby wykonać buforowane zapytania. Ponadto pamięć podręczna planu SQL Server jest opróżniana przed wykonaniem każdego zestawu zapytań, dzięki czemu czasy uzyskiwane dokładnie odzwierciedlają korzyść przydaną przez pamięć podręczną planu zapytania.

##### <a name="3231-test-results"></a>3.2.3.1 Wyniki testów

| Testowanie                                                                   | EF5 Brak pamięci podręcznej | EF5 w pamięci podręcznej | EF6 Brak pamięci podręcznej | EF6 w pamięci podręcznej |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| Wyliczanie wszystkich zapytań 18723                                          | 124          | 125,4      | 124,3        | 125,3      |
| Unikanie odchylenia (tylko pierwsze zapytania 800, niezależnie od złożoności)  | 41,7         | 5.5        | 40.5         | 5.4        |
| Tylko zapytania AggregatingSubtotals (łącznie 178), które unikają wycierania | 39,5         | 4.5        | 38,1         | 4.6        |

*Wszystkie czasy w sekundach.*

Dobry — podczas wykonywania wielu odrębnych zapytań (na przykład dynamicznie tworzonych zapytań) buforowanie nie jest pomocne, a wynikiem operacji opróżniania pamięci podręcznej może być zachowanie zapytań, które byłyby korzystne dla najwyższego użycia planu.

Zapytania AggregatingSubtotals są najbardziej skomplikowane dla zapytań, które przetestowały. Zgodnie z oczekiwaniami, bardziej skomplikowane jest zapytanie, tym więcej korzyści będzie można znaleźć w temacie buforowanie planu zapytania.

Ponieważ CompiledQuery jest naprawdę zapytania LINQ z buforowanym planem, porównanie CompiledQuery i równoważnej kwerendy Entity SQL powinna mieć podobne wyniki. W rzeczywistości, jeśli aplikacja ma wiele zapytań Entity SQL dynamicznych, wypełnianie pamięci podręcznej przy użyciu zapytań spowoduje również skuteczną CompiledQueries "dekompilowanie" po opróżnieniu z pamięci podręcznej. W tym scenariuszu wydajność można ulepszyć, wyłączając buforowanie w zapytaniach dynamicznych w celu określenia priorytetów CompiledQueries. Jeszcze lepszym rozwiązaniem jest ponowne napisanie aplikacji w celu używania zapytań parametrycznych zamiast zapytań dynamicznych.

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a>3,3 przy użyciu CompiledQuery, aby zwiększyć wydajność przy użyciu zapytań LINQ

Nasze testy wskazują, że korzystanie z usługi CompiledQuery może przynieść do 7% przez skompilowane przez siebie zapytania LINQ. oznacza to, że poświęcimy 7% mniej czasu na wykonanie kodu ze stosu Entity Frameworkowego; nie oznacza to, że aplikacja będzie o 7% szybsza. Ogólnie mówiąc, koszt pisania i konserwacji obiektów CompiledQuery w EF 5,0 może nie być cenny w porównaniu z korzyściami. Przebieg może się różnić, dlatego należy skorzystać z tej opcji, jeśli projekt wymaga dodatkowej wypychania. Należy zauważyć, że CompiledQueries są zgodne tylko z modelami pochodnymi ObjectContext i nie są zgodne z modelami pochodnymi DbContext.

Aby uzyskać więcej informacji na temat tworzenia i wywoływania CompiledQuery, zobacz [skompilowane zapytania (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).

W przypadku korzystania z CompiledQuery należy wziąć pod uwagę dwa kwestie, a mianowicie wymagania dotyczące korzystania z wystąpień statycznych i problemów z możliwością redagowania. Poniżej znajduje się szczegółowy opis tych dwóch zagadnień.

#### <a name="331-use-static-compiledquery-instances"></a>3.3.1 użycie statycznych wystąpień CompiledQuery

Ponieważ Kompilowanie zapytania LINQ jest czasochłonnym procesem, nie chcemy go wykonywać za każdym razem, gdy będziemy musieli pobrać dane z bazy danych. Wystąpienia CompiledQuery umożliwiają kompilowanie i uruchamianie wielu razy, ale należy zachować ostrożność i zadawać, aby ponownie używać tego samego wystąpienia CompiledQuery za każdym razem, zamiast kompilować go w trybie failover i ponownie. Użycie statycznych elementów członkowskich do przechowywania wystąpień CompiledQuery będzie konieczne; w przeciwnym razie nie zobaczysz żadnej korzyści.

Załóżmy na przykład, że strona ma następującą treść metody, aby obsłużyć wyświetlanie produktów dla wybranej kategorii:

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

W takim przypadku na bieżąco utworzysz nowe wystąpienie CompiledQuery przy każdym wywołaniu metody. Zamiast wyświetlać zalety wydajności przez pobranie polecenia Zapisz z pamięci podręcznej planu zapytania, CompiledQuery przejdzie przez kompilator planu za każdym razem, gdy tworzone jest nowe wystąpienie. W rzeczywistości będzie to zanieczyszczenie pamięci podręcznej planu zapytania przy użyciu nowego wpisu CompiledQuery przy każdym wywołaniu metody.

Zamiast tego należy utworzyć wystąpienie statyczne skompilowanego zapytania, aby można było wywołać to samo skompilowane zapytanie za każdym razem, gdy wywoływana jest metoda. Jednym ze sposobów jest dodanie wystąpienia CompiledQuery jako elementu członkowskiego kontekstu obiektu.  Następnie można zwiększyć czytelność, uzyskując dostęp do CompiledQuery za pomocą metody pomocnika:

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

Ta metoda pomocnika zostałaby wywołana w następujący sposób:

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-composing-over-a-compiledquery"></a>3.3.2 redagowanie w CompiledQuery

Możliwość tworzenia wszystkich zapytań LINQ jest niezwykle przydatna. w tym celu po prostu wywołaj metodę po interfejsie IQueryable, takim jak *Skip ()* lub *Count ()* . Jednak zasadniczo zwraca nowy obiekt IQueryable. Nie ma nic, aby nie było możliwe, aby nie zatrzymywać od firmy CompiledQuery, że spowoduje to wygenerowanie nowego obiektu IQueryable, który wymaga ponownego przekazania kompilatora planu.

Niektóre składniki będą używały złożonych obiektów IQueryable do włączenia zaawansowanych funkcji. Na przykład ASP. Widok GridView sieci może być powiązany z danymi z obiektem IQueryable za pośrednictwem właściwości SelectMethod. W widoku GridView utworzysz ten obiekt IQueryable, aby umożliwić sortowanie i stronicowanie w modelu danych. Jak widać, użycie CompiledQuery dla widoku GridView nie spowoduje pojawienia się skompilowanego zapytania, ale spowoduje wygenerowanie nowej autokompilowanego zapytania.

Jedno miejsce, w którym można w tym celu wykonać w przypadku dodawania filtrów progresywnych do zapytania. Załóżmy na przykład, że masz stronę klienci z kilkoma listami rozwijanymi dla filtrów opcjonalnych (na przykład Country i OrdersCount). Te filtry można redagować na podstawie wyników CompiledQuery, ale to spowoduje, że nowe zapytanie przejdzie przez kompilator planu przy każdym jego wykonaniu.

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

 Aby uniknąć tej ponownej kompilacji, możesz ponownie napisać CompiledQuery, aby uwzględnić możliwe filtry:

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

Które zostałyby wywołane w interfejsie użytkownika, np.:

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

 W tym miejscu jest to polecenie wygenerowany magazyn, które będzie miało zawsze filtry z sprawdzeniami null, ale te wartości powinny być dość proste, aby serwer bazy danych mógł zoptymalizować:

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a>buforowanie metadanych 3,4

Entity Framework obsługuje również buforowanie metadanych. Jest to zasadniczo buforowanie informacji o typie i informacje mapowania typu "na typ do bazy danych" między różnymi połączeniami z tym samym modelem. Pamięć podręczna metadanych jest unikatowa dla domeny aplikacji.

#### <a name="341-metadata-caching-algorithm"></a>w algorytmie buforowania metadanych

1.  Informacje o metadanych dla modelu są przechowywane w obiekt ItemCollection dla każdego EntityConnectionu.
    -   Jako notatka boczna istnieją różne obiekty obiekt ItemCollection dla różnych części modelu. Na przykład StoreItemCollections zawiera informacje o modelu bazy danych; ObjectItemCollection zawiera informacje o modelu danych; EdmItemCollection zawiera informacje o modelu koncepcyjnym.

2.  Jeśli dwa połączenia używają tych samych parametrów połączenia, będą współużytkować to samo wystąpienie obiekt ItemCollection.
3.  Funkcja równoważna funkcjonalnie, ale różne ciągi połączenia mogą powodować różne metadane. Tokenize ciągi połączeń, więc po prostu zmiana kolejności tokenów powinna spowodować, że metadane udostępnione. Ale dwa parametry połączenia, które wydaje się funkcjonalnie takie same, mogą nie być oceniane jako identyczne po tokenizacji.
4.  Obiekt ItemCollection jest okresowo sprawdzana pod kątem użycia. Jeśli okaże się, że nie uzyskano ostatnio dostępu do obszaru roboczego, zostanie on oznaczony do oczyszczenia przy następnym wyczyszczeniu pamięci podręcznej.
5.  Tylko utworzenie EntityConnection spowoduje utworzenie pamięci podręcznej metadanych (mimo że kolekcje elementów w niej nie zostaną zainicjowane do momentu otwarcia połączenia). Ten obszar roboczy pozostanie w pamięci do momentu, aż algorytm buforowania ustali, że nie jest używany.

Zespół Doradczy klientów zapisane wpis w blogu, który opisuje zawierający odwołanie do obiektu ItemCollection w celu uniknięcia "wycofywania", korzystając z dużych modeli: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a>3.4.2 relację między buforowaniem metadanych a buforowaniem planu zapytania

Wystąpienie pamięci podręcznej planu zapytania jest przechowywane w obiekt ItemCollection obiektu MetadataWorkspace. Oznacza to, że polecenia magazynu w pamięci podręcznej będą używane do wykonywania zapytań dotyczących kontekstu wystąpienia przy użyciu danego obiektu MetadataWorkspace. Oznacza to również, że jeśli istnieją dwa parametry połączeń, które są nieco inne i nie pasują po tokenizowanie, będą dostępne różne wystąpienia pamięci podręcznej planu zapytania.

### <a name="35-results-caching"></a>Buforowanie wyników 3,5

Dzięki buforowaniu wyników (nazywanej także "buforowaniem drugiego poziomu") można zachować wyniki zapytań w lokalnej pamięci podręcznej. Podczas wykonywania zapytania należy najpierw sprawdzić, czy wyniki są dostępne lokalnie przed wykonaniem zapytania względem magazynu. Podczas gdy buforowanie wyników nie jest bezpośrednio obsługiwane przez Entity Framework, można dodać pamięć podręczną drugiego poziomu przy użyciu dostawcy otoki. Przykładem dostawcy zawijania z pamięci podręcznej drugiego poziomu jest [Entity Framework Alachisofta pamięć podręczna drugiego poziomu oparta na NCache](https://www.alachisoft.com/ncache/entity-framework.html).

Ta implementacja buforowania drugiego poziomu jest funkcją wstrzykiwaną, która ma miejsce po obliczeniu wyrażenia LINQ (i funcletized), a plan wykonywania zapytania jest obliczany lub pobierany z pamięci podręcznej pierwszego poziomu. Pamięć podręczna drugiego poziomu będzie następnie przechowywać tylko wyniki nieprzetworzonej bazy danych, więc potok materializację nadal zostanie wykonany.

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a>3.5.1 dodatkowe informacje dotyczące buforowania wyników w ramach dostawcy zawijania

-   Julie Lerman zapisał "pamięć podręczną drugiego poziomu w Entity Framework i Windows Azure" w witrynie MSDN, która obejmuje jak zaktualizować przykładowego dostawcę otoki do korzystania z pamięci podręcznej systemu Windows Server AppFabric: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)
-   Jeśli pracujesz z Entity Framework 5, blog zespołu ma wpis, w której opisano Rozpoczynanie pracy z pamięci podręcznej dostawcy programu Entity Framework 5: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>. Zawiera również szablon T4, który pomaga zautomatyzować Dodawanie buforowania drugiego poziomu do projektu.

## <a name="4-autocompiled-queries"></a>4 autokompilowane zapytania

Gdy zapytanie jest wydawane dla bazy danych przy użyciu Entity Framework, musi przejść przez serię kroków, zanim rzeczywiście materializacji wyniki; jednym z tych kroków jest kompilacja zapytania. Wiadomo, że Entity SQL zapytania mają dobrą wydajność, ponieważ są one automatycznie buforowane, więc drugi lub trzeci czas wykonywania tego samego zapytania może pominąć kompilator planu i zamiast tego użyć buforowanego planu.

Entity Framework 5 wprowadzono również automatyczne buforowanie dla zapytań LINQ to Entities. W poprzednich wersjach Entity Framework tworzenia CompiledQuery w celu przyspieszenia działania była powszechną gwarancją, ponieważ spowodowałoby to przeprowadzenie LINQ to Entities zapytania w pamięci podręcznej. Ponieważ buforowanie jest teraz wykonywane automatycznie bez użycia CompiledQuery, wywoływana jest funkcja "autokompilowane zapytania". Aby uzyskać więcej informacji na temat pamięci podręcznej planu zapytania i jej Mechanics, zobacz buforowanie planu zapytania.

Entity Framework wykrywa, kiedy zapytanie wymaga ponownej kompilacji, i robi to, gdy zapytanie jest wywoływane, nawet jeśli zostało skompilowane wcześniej. Typowe warunki, które powodują ponowną kompilację zapytania, to:

-   Zmiana MergeOption skojarzonego z zapytaniem. Zapytanie buforowane nie zostanie użyte, a następnie kompilator planu zostanie uruchomiony ponownie, a nowo utworzony plan zostanie zapisany w pamięci podręcznej.
-   Zmiana wartości ContextOptions. UseCSharpNullComparisonBehavior. Ten sam efekt jest taki sam jak zmiana MergeOption.

Inne warunki mogą uniemożliwić korzystanie z pamięci podręcznej przez zapytanie. Typowe przykłady to:

-   Za pomocą interfejsu IEnumerable @ no__t-0T @ no__t-1. Zawiera @ no__t-2 @ no__t-3 (T wartość).
-   Korzystanie z funkcji, które generują zapytania ze stałymi.
-   Używanie właściwości niemapowanego obiektu.
-   Łączenie zapytania z innym zapytaniem, które wymaga ponownej kompilacji.

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a>4,1 przy użyciu interfejsu IEnumerable @ no__t-0T @ no__t-1. Zawiera wartość @ no__t-2T @ no__t-3 (T wartość)

Entity Framework nie buforuje zapytań, które wywołują interfejs IEnumerable @ no__t-0T @ no__t-1. Zawiera element @ no__t-2T @ no__t-3 (T Value) względem kolekcji w pamięci, ponieważ wartości kolekcji są uznawane za nietrwałe. Następujące przykładowe zapytanie nie zostanie zapisane w pamięci podręcznej, więc będzie zawsze przetwarzane przez kompilator planu:

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

Należy zauważyć, że rozmiar interfejsu IEnumerable, z którym jest wykonywane, określa, jak szybko lub jak wolno kompilować zapytanie. Wydajność może znacznie pogorszyć się podczas korzystania z dużych kolekcji, takich jak pokazane w powyższym przykładzie.

Entity Framework 6 zawiera optymalizacje w sposób, w jaki interfejs IEnumerable @ no__t-0T @ no__t-1. Zawiera @ no__t-2T @ no__t-3 (T wartość) działa podczas wykonywania zapytań. Wygenerowany kod SQL jest znacznie szybszy do tworzenia i bardziej czytelny, a w większości przypadków jest również wykonywany szybciej na serwerze.

### <a name="42-using-functions-that-produce-queries-with-constants"></a>4,2 użycie funkcji generujących zapytania ze stałymi

Operatory Skip (), Take (), Contains () i DefautIfEmpty () LINQ nie generują zapytań SQL z parametrami, ale zamiast tego przechodzą wartości do nich jako stałe. Z tego powodu zapytania, które mogłyby w przeciwnym razie być takie same, kończą się zanieczyszczeniem pamięci podręcznej planu zapytania, zarówno na stosie EF, jak i na serwerze bazy danych, i nie są ponownie wykorzystywane, chyba że te same stałe są używane w kolejnym wykonaniu zapytania. Na przykład:

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

W tym przykładzie, za każdym razem, gdy to zapytanie jest wykonywane z inną wartością dla identyfikatora, zapytanie zostanie skompilowane do nowego planu.

W szczególności należy zwrócić uwagę na użycie pomijania i podjąć podczas wykonywania stronicowania. W EF6 te metody mają Przeciążenie lambda, które efektywnie czynią buforowanym planem zapytań, ponieważ program EF może przechwytywać zmienne przesłane do tych metod i przetłumaczać je na parametry SqlParameters. Pozwala to również zachować oczyszczarkę pamięci podręcznej, ponieważ w przeciwnym razie każde zapytanie o inną stałą dla pozycji Pomiń i zrób spowoduje uzyskanie własnego wpisu pamięci podręcznej planu zapytania.

Rozważmy poniższy kod, który jest optymalny, ale jest przeznaczony tylko do exemplify tej klasy zapytań:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Szybsza wersja tego samego kodu będzie wymagała wywołania pominięcia z wyrażeniem lambda:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Drugi fragment kodu może działać do 11% szybciej, ponieważ ten sam plan zapytania jest używany przy każdym uruchomieniu zapytania, co oszczędza czas procesora i zapobiega zanieczyszczaniu pamięci podręcznej zapytań. Ponadto, ponieważ parametr do pominięcia znajduje się w zamknięciu, kod może wyglądać następująco:

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a>4,3 przy użyciu właściwości niemapowanego obiektu

Gdy zapytanie używa właściwości niemapowanego typu obiektu jako parametru, zapytanie nie zostanie zapisane w pamięci podręcznej. Na przykład:

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

W tym przykładzie Załóżmy, że Klasa unzamapowanytype nie jest częścią modelu Entity. To zapytanie można łatwo zmienić, aby nie używało niemapowanego typu, a zamiast tego użyć zmiennej lokalnej jako parametru do zapytania:

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

W takim przypadku zapytanie będzie mogło być dostępne w pamięci podręcznej i będzie korzystać z pamięci podręcznej planu zapytania.

### <a name="44-linking-to-queries-that-require-recompiling"></a>4,4 Łączenie z zapytaniami wymagającymi ponownego kompilowania

Zgodnie z powyższym przykładem, jeśli masz drugie zapytanie, które opiera się na zapytaniu, które musi zostać ponownie skompilowane, całe drugie zapytanie zostanie również ponownie skompilowane. Oto przykład ilustrujący ten scenariusz:

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

Przykładem jest ogólny, ale ilustruje to, jak łączenie z firstQuery powoduje, że secondQuery nie można uzyskać pamięci podręcznej. Jeśli firstQuery nie był zapytaniem wymagającym ponownej kompilacji, secondQuery zostałyby zapisane w pamięci podręcznej.

## <a name="5-notracking-queries"></a>5 zapytań NoTracking

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a>5,1 wyłączanie śledzenia zmian w celu ograniczenia kosztów zarządzania stanem

Jeśli jesteś w scenariuszu tylko do odczytu i chcesz uniknąć narzutów ładowania obiektów do obiektu ObjectStateManager, możesz wydać zapytania "Brak śledzenia".  Śledzenie zmian można wyłączyć na poziomie zapytania.

Należy pamiętać, że wyłączenie śledzenia zmian pozwala skutecznie wyłączyć pamięć podręczną obiektów. Podczas wykonywania zapytania o jednostkę nie można pominąć materializację przez ściąganie poprzednio wykorzystanych wyników zapytania z obiektu ObjectStateManager. Jeśli wykonujesz wielokrotnie zapytania dotyczące tych samych jednostek w tym samym kontekście, możesz ostatecznie zobaczyć korzyść wydajności z włączenia śledzenia zmian.

Podczas wykonywania zapytań za pomocą wystąpień obiektu ObjectContext, ObjectQuery i ObjectSet zapamiętają MergeOption po jej ustawieniu, a zapytania, które są tworzone na nich, będą dziedziczyć efektywną MergeOption zapytania nadrzędnego. W przypadku korzystania z DbContext śledzenia można wyłączyć, wywołując modyfikator AsNoTracking () na Nieogólnymi.

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a>5.1.1 wyłączenie śledzenia zmian dla zapytania podczas korzystania z DbContext

Można przełączyć tryb zapytania na NoTracking poprzez łańcuch wywołania metody AsNoTracking () w zapytaniu. W przeciwieństwie do ObjectQuery, klasy Nieogólnymi i DBQuery w interfejsie API DbContext nie mają właściwości mutable dla MergeOption.

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a>5.1.2 Wyłączenie śledzenia zmian na poziomie zapytania przy użyciu obiektu ObjectContext

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a>5.1.3 wyłączenie śledzenia zmian dla całego zestawu jednostek przy użyciu obiektu ObjectContext

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a>5,2 metryki testów ukazujące korzyść wydajności dla zapytań NoTracking

W tym teście Przyjrzyjmy się kosztowi wypełniania obiektu ObjectStateManager, porównując śledzenie z zapytaniami NoTracking model systemu Navision. Zapoznaj się z załącznikiem opis modelu systemu Navision i typy zapytań, które zostały wykonane. W tym teście wykonujemy iterację na liście zapytań i wykonują każde jeden raz. Uruchomiono dwie odmiany testu, raz z zapytania NoTracking i raz z domyślną opcją scalania "AppendOnly". Każda zmiana została uruchomiona 3 razy i ma wartość średnia dla przebiegów. Między testami czyścimy pamięć podręczną zapytań na SQL Server i zmniejszamy bazę danych tempdb, uruchamiając następujące polecenia:

1.  POLECENIE DBCC DROPCLEANBUFFERS
2.  POLECENIE DBCC FREEPROCCACHE
3.  DBCC SHRINKDATABASE (tempdb, 0)

Wyniki testów, mediana nad 3 uruchomieniami:

|                        | BRAK ŚLEDZENIA — ZESTAW ROBOCZY | BEZ ŚLEDZENIA — CZAS | TYLKO DOŁĄCZANIE — ZESTAW ROBOCZY | TYLKO DOŁĄCZ — CZAS |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| **Entity Framework 5** | 460361728                 | 1163536 MS         | 596545536                 | 1273042 MS         |
| **Entity Framework 6** | 647127040                 | 190228 MS          | 832798720                 | 195521 MS          |

Program Entity Framework 5 będzie miał mniejsze ilości pamięci na końcu uruchomienia niż Entity Framework 6. Dodatkowa pamięć używana przez Entity Framework 6 to wynik dodatkowych struktur pamięci i kodu, który umożliwia korzystanie z nowych funkcji i lepszą wydajność.

W przypadku korzystania z obiektu ObjectStateManager istnieje również wyraźna różnica w pamięci. Entity Framework 5 zwiększono swoje rozmiary o 30% podczas śledzenia wszystkich jednostek, z których korzystamy z bazy danych. Entity Framework 6 zwiększono jego rozmiary o 28% w tym czasie.

W czasie Entity Framework 6 przeprowadzi Entity Framework 5 w tym teście o duży margines. Entity Framework 6 zakończył test w około 16% czasu zużyty przez Entity Framework 5. Ponadto Entity Framework 5 trwa o 9% więcej czasu, gdy obiekt ObjectStateManager jest używany. W porównaniu Entity Framework 6 jest używany przez 3% więcej czasu przy użyciu obiektu ObjectStateManager.

## <a name="6-query-execution-options"></a>6 opcji wykonywania zapytania

Entity Framework oferuje kilka różnych sposobów wykonywania zapytań. Zapoznaj się z następującymi opcjami, porównaj zalety i wady każdego z nich i sprawdź ich charakterystykę wydajności:

-   LINQ to Entities.
-   Brak śledzenia LINQ to Entities.
-   Entity SQL w ObjectQuery.
-   Entity SQL w EntityCommand.
-   ExecuteStoreQuery.
-   SqlQuery.
-   CompiledQuery.

### <a name="61-linq-to-entities-queries"></a>6,1 zapytań LINQ to Entities

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

**Formaty**

-   Odpowiednie dla operacji CUD.
-   W pełni materiałowe obiekty.
-   Najprostszym sposobem pisać składnią wbudowaną w język programowania.
-   Dobra wydajność.

**Wada**

-   Niektóre ograniczenia techniczne, takie jak:
    -   Wzorce używające DefaultIfEmpty dla zapytań SPRZĘŻENIa zewnętrznego powodują bardziej skomplikowane zapytania niż proste instrukcje zewnętrznego SPRZĘŻENIa w Entity SQL.
    -   Nadal nie można używać takich jak z ogólnym dopasowaniem do wzorca.

### <a name="62-no-tracking-linq-to-entities-queries"></a>6,2 Brak śledzenia zapytań LINQ to Entities

Gdy kontekst dziedziczy:

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

Gdy kontekst dziedziczy DbContext:

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

**Formaty**

-   Zwiększona wydajność przez zwykłe zapytania LINQ.
-   W pełni materiałowe obiekty.
-   Najprostszym sposobem pisać składnią wbudowaną w język programowania.

**Wada**

-   Nieodpowiednie dla operacji CUD.
-   Niektóre ograniczenia techniczne, takie jak:
    -   Wzorce używające DefaultIfEmpty dla zapytań SPRZĘŻENIa zewnętrznego powodują bardziej skomplikowane zapytania niż proste instrukcje zewnętrznego SPRZĘŻENIa w Entity SQL.
    -   Nadal nie można używać takich jak z ogólnym dopasowaniem do wzorca.

Należy zauważyć, że zapytania, które właściwości skalarne projektu nie są śledzone, nawet jeśli nie jest określone NoTracking. Na przykład:

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

To konkretne zapytanie nie określa jawnie elementu NoTracking, ale ponieważ nie materializacji typu, który jest znany przez menedżera stanu obiektów, wówczas materiałowy wynik nie jest śledzony.

### <a name="63-entity-sql-over-an-objectquery"></a>6,3 Entity SQL w ObjectQuery

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

**Formaty**

-   Odpowiednie dla operacji CUD.
-   W pełni materiałowe obiekty.
-   Obsługuje buforowanie planu zapytania.

**Wada**

-   Obejmuje ciągi kwerend tekstowych, które są bardziej podatne na błędy użytkownika niż konstrukcje zapytań wbudowane w język.

### <a name="64-entity-sql-over-an-entity-command"></a>6,4 Entity SQL za pomocą polecenia Entity

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

**Formaty**

-   Obsługuje buforowanie planu zapytania w programie .NET 4,0 (buforowanie planu jest obsługiwane przez wszystkie inne typy zapytań w programie .NET 4,5).

**Wada**

-   Obejmuje ciągi kwerend tekstowych, które są bardziej podatne na błędy użytkownika niż konstrukcje zapytań wbudowane w język.
-   Nieodpowiednie dla operacji CUD.
-   Wyniki nie są automatycznie materiałowe i muszą zostać odczytane z czytnika danych.

### <a name="65-sqlquery-and-executestorequery"></a>6,5 sqlQuery i ExecuteStoreQuery

SqlQuery w bazie danych:

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

SqlQuery w Nieogólnymi:

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

**Formaty**

-   Zazwyczaj najszybszą wydajność, ponieważ kompilator planu jest pomijany.
-   W pełni materiałowe obiekty.
-   Odpowiednie dla operacji CUD, gdy są używane z Nieogólnymi.

**Wada**

-   Zapytanie jest tekstowe i podatne na błędy.
-   Zapytanie jest powiązane z określonym zapleczem przy użyciu semantyki magazynu zamiast semantyki koncepcyjnej.
-   Gdy jest obecny dziedziczenie, zapytanie Handcrafted musi uwzględnić warunki mapowania dla żądanego typu.

### <a name="66-compiledquery"></a>6,6 CompiledQuery

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

**Formaty**

-   Zapewnia wzrost wydajności do 7% w porównaniu do zwykłych zapytań LINQ.
-   W pełni materiałowe obiekty.
-   Odpowiednie dla operacji CUD.

**Wada**

-   Zwiększona złożoność i narzuty związane z programowaniem.
-   Zwiększenie wydajności jest tracone podczas redagowania na skompilowanym zapytaniu.
-   Niektóre zapytania LINQ nie mogą być zapisywane jako CompiledQuery — na przykład projekcje typów anonimowych.

### <a name="67-performance-comparison-of-different-query-options"></a>Porównanie wydajności 6,7 różnych opcji zapytania

Proste mikrotesty, w których nie upłynął limit czasu tworzenia kontekstu, zostały wprowadzone do testu. Mierzy zapytania o 5000 razy dla zestawu niebuforowanych jednostek w środowisku kontrolowanym. Te liczby mają być pobierane z ostrzeżeniem: nie odzwierciedlają rzeczywistej liczby wyprodukowanej przez aplikację, ale zamiast tego są bardzo precyzyjne pomiary, jaka jest różnica wydajności, gdy porównywane są różne opcje zapytania jabłka do jabłek, z wyłączeniem kosztów tworzenia nowego kontekstu.

| BIEŻĄCO  | Testowanie                                 | Czas (MS) | Memory (Pamięć)   |
|:----|:-------------------------------------|:----------|:---------|
| EF5 | Obiekt ObjectContext ESQL                   | 2414      | 38801408 |
| EF5 | Zapytanie ObjectContext LINQ             | 2692      | 38277120 |
| EF5 | Brak śledzenia zapytania DbContext LINQ     | 2818      | 41840640 |
| EF5 | Zapytanie DbContext LINQ                 | 2930      | 41771008 |
| EF5 | Zapytanie ObjectContext LINQ bez śledzenia | 3013      | 38412288 |
|     |                                      |           |          |
| EF6 | Obiekt ObjectContext ESQL                   | 2059      | 46039040 |
| EF6 | Zapytanie ObjectContext LINQ             | 3074      | 45248512 |
| EF6 | Brak śledzenia zapytania DbContext LINQ     | 3125      | 47575040 |
| EF6 | Zapytanie DbContext LINQ                 | 3420      | 47652864 |
| EF6 | Zapytanie ObjectContext LINQ bez śledzenia | 3593      | 45260800 |

![EF5 mikroporównawcze, 5000 iteracji](~/ef6/media/ef5micro5000warm.png)

![EF6 mikroporównawcze, 5000 iteracji](~/ef6/media/ef6micro5000warm.png)

Mikrotesty są bardzo poufne dla małych zmian w kodzie. W takim przypadku różnica między kosztami Entity Framework 5 i Entity Framework 6 jest spowodowana dodaniem [przechwycenia](~/ef6/fundamentals/logging-and-interception.md) i zmian [transakcyjnych](~/ef6/saving/transactions.md). Te mikrotestowe numery, jednak stanowią wzmocną wizję do bardzo małego fragmentu Entity Framework. Rzeczywiste scenariusze zapytań ciepłej nie powinny mieć zastosowania regresji wydajności podczas uaktualniania z Entity Framework 5 do Entity Framework 6.

Aby porównać rzeczywistą wydajność różnych opcji zapytania, utworzyliśmy 5 oddzielnych odmian testowych, w których używamy innej opcji zapytania, aby wybrać wszystkie produkty, których nazwa kategorii to "napoje". Każda iteracja obejmuje koszt tworzenia kontekstu, a koszt materializacji wszystkich zwracanych jednostek. 10 iteracji jest wykonywanych przed upływem sumy 1000 iteracji czasowych. Wyświetlane wyniki to mediana, wykonywana z 5 przebiegów każdego testu. Aby uzyskać więcej informacji, zobacz dodatek B, który zawiera kod dla testu.

| BIEŻĄCO  | Testowanie                                        | Czas (MS) | Memory (Pamięć)   |
|:----|:--------------------------------------------|:----------|:---------|
| EF5 | ObjectContext Entity — polecenie                | 621       | 39350272 |
| EF5 | Zapytanie SQL DbContext w bazie danych             | 825       | 37519360 |
| EF5 | Zapytanie magazynu ObjectContext                   | 878       | 39460864 |
| EF5 | Zapytanie ObjectContext LINQ bez śledzenia        | 969       | 38293504 |
| EF5 | Zapytanie jednostki obiektu ObjectContext programu SQL using | 1089      | 38981632 |
| EF5 | Zapytanie skompilowane obiektu ObjectContext                | 1099      | 38682624 |
| EF5 | Zapytanie ObjectContext LINQ                    | 1152      | 38178816 |
| EF5 | Brak śledzenia zapytania DbContext LINQ            | 1208      | 41803776 |
| EF5 | Zapytanie SQL DbContext dotyczące Nieogólnymi                | 1414      | 37982208 |
| EF5 | Zapytanie DbContext LINQ                        | 1574      | 41738240 |
|     |                                             |           |          |
| EF6 | ObjectContext Entity — polecenie                | 480       | 47247360 |
| EF6 | Zapytanie magazynu ObjectContext                   | 493       | 46739456 |
| EF6 | Zapytanie SQL DbContext w bazie danych             | 614       | 41607168 |
| EF6 | Zapytanie ObjectContext LINQ bez śledzenia        | 684       | 46333952 |
| EF6 | Zapytanie jednostki obiektu ObjectContext programu SQL using | 767       | 48865280 |
| EF6 | Zapytanie skompilowane obiektu ObjectContext                | 788       | 48467968 |
| EF6 | Brak śledzenia zapytania DbContext LINQ            | 878       | 47554560 |
| EF6 | Zapytanie ObjectContext LINQ                    | 953       | 47632384 |
| EF6 | Zapytanie SQL DbContext dotyczące Nieogólnymi                | 1023      | 41992192 |
| EF6 | Zapytanie DbContext LINQ                        | 1290      | 47529984 |


![Iteracje EF5 ciepłych zapytań 1000](~/ef6/media/ef5warmquery1000.png)

![Iteracje EF6 ciepłych zapytań 1000](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> W celu zapewnienia kompletności dodaliśmymy odmianę, w której wykonujemy Entity SQL zapytanie na EntityCommand. Jednak ze względu na to, że wyniki nie są istotne dla takich zapytań, porównanie nie jest koniecznie jabłek do jabłek. Test obejmuje bliskie przybliżenie do materializacji, aby spróbować uzyskać bardziej atrakcyjny wynik porównania.

W tym celu należy wykonać Entity Framework 6 Entity Framework 5 z powodu ulepszeń wydajności dla kilku części stosu, takich jak znacznie jaśniejsze inicjalizacje DbContext i szybsze wyszukiwanie no__t-0T @ no__t-1.

## <a name="7-design-time-performance-considerations"></a>7 zagadnienia dotyczące wydajności w czasie projektowania

### <a name="71-inheritance-strategies"></a>7,1 strategie dziedziczenia

W przypadku korzystania z Entity Framework jest stosowana strategia dziedziczenia. Entity Framework obsługuje 3 podstawowe typy dziedziczenia i ich kombinacje:

-   Tabela na hierarchię (TPH) — gdzie poszczególne ustawienia dziedziczenia są mapowane na tabelę z kolumną rozróżniacza, aby wskazać, który konkretny typ w hierarchii jest reprezentowany w wierszu.
-   Tabela na typ (TPT) — gdzie każdy typ ma własną tabelę w bazie danych; tabele podrzędne definiują tylko kolumny, które nie zawierają tabeli nadrzędnej.
-   Tabela na klasę (TPC) — gdzie każdy typ ma własną pełną tabelę w bazie danych; tabele podrzędne definiują wszystkie pola, włącznie z tymi zdefiniowanymi w typach nadrzędnych.

Jeśli model używa dziedziczenia TPT, generowane zapytania będą bardziej skomplikowane niż te, które są generowane z innymi strategiami dziedziczenia, co może spowodować dłuższe czasy wykonywania w sklepie.  Zazwyczaj generowanie zapytań w modelu TPT i zmaterializowania obiektów powstających zajmuje więcej czasu.

Zobacz "zagadnienia dotyczące wydajności podczas korzystania z dziedziczenia TPT (Tabela na typ) w Entity Framework" w blogu MSDN: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.

#### <a name="711-avoiding-tpt-in-model-first-or-code-first-applications"></a>7.1.1 unikanie TPT w aplikacjach Model First lub Code First

Gdy tworzysz model dla istniejącej bazy danych, która ma schemat TPT, nie masz wielu opcji. Ale podczas tworzenia aplikacji przy użyciu Model First lub Code First należy unikać dziedziczenia TPT w przypadku problemów z wydajnością.

W przypadku korzystania z Model First w Kreatorze Entity Designer zostanie TPT do dowolnego dziedziczenia w modelu. Jeśli chcesz przełączyć się do strategii TPH dziedziczenia z pierwszego modelu, można używać "jednostki projektanta bazy danych generowania Power Pack" dostępne z galerii Visual Studio ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).

Przy użyciu Code First do konfigurowania mapowania modelu z dziedziczeniem, EF domyślnie będzie używać TPH, dlatego wszystkie jednostki w hierarchii dziedziczenia zostaną zmapowane do tej samej tabeli. Zobacz sekcję "Mapowanie z interfejs Fluent API" artykułu "Kodu pierwszy w jednostki Framework4.1" w MSDN Magazine ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) Aby uzyskać więcej informacji.

### <a name="72-upgrading-from-ef4-to-improve-model-generation-time"></a>7,2 uaktualnienie z EF4 w celu poprawienia czasu generowania modelu

Specyficzna dla SQL Server poprawa algorytmu, który generuje warstwę magazynu (SSDL) modelu, jest dostępna w Entity Framework 5 i 6 oraz jako aktualizacja Entity Framework 4, gdy jest zainstalowany program Visual Studio 2010 SP1. Poniższe wyniki testów przedstawiają poprawę podczas generowania bardzo dużego modelu, w tym przypadku modelu systemu Navision. Aby uzyskać szczegółowe informacje na ten temat, zobacz Dodatek C.

Model zawiera 1005 zestawów jednostek i 4227 zestawów skojarzeń.

| Konfigurowanie                              | Podział zużytego czasu                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Visual Studio 2010, Entity Framework 4     | Generowanie SSDL: 2 godz. 27 min <br/> Generowanie mapowania: 1 sekunda <br/> Generowanie CSDL: 1 sekunda <br/> Generowanie ObjectLayer: 1 sekunda <br/> Generowanie widoku: 2 godz. 14 min |
| Visual Studio 2010 z dodatkiem SP1, Entity Framework 4 | Generowanie SSDL: 1 sekunda <br/> Generowanie mapowania: 1 sekunda <br/> Generowanie CSDL: 1 sekunda <br/> Generowanie ObjectLayer: 1 sekunda <br/> Generowanie widoku: 1 godz. 53 min   |
| Visual Studio 2013, Entity Framework 5     | Generowanie SSDL: 1 sekunda <br/> Generowanie mapowania: 1 sekunda <br/> Generowanie CSDL: 1 sekunda <br/> Generowanie ObjectLayer: 1 sekunda <br/> Generowanie widoku: 65 minut    |
| Visual Studio 2013, Entity Framework 6     | Generowanie SSDL: 1 sekunda <br/> Generowanie mapowania: 1 sekunda <br/> Generowanie CSDL: 1 sekunda <br/> Generowanie ObjectLayer: 1 sekunda <br/> Generowanie widoku: 28 sekund.   |


Należy zauważyć, że podczas generowania SSDL, obciążenie jest niemal całkowicie wykorzystane na SQL Server, podczas gdy komputer deweloperski klienta oczekuje na wyniki z serwera. Przetwarzający powinny szczególnie poprawić te ulepszenia. Warto również zauważyć, że zasadniczo cały koszt generowania modelu odbywa się teraz.

### <a name="73-splitting-large-models-with-database-first-and-model-first"></a>7,3 dzielenie dużych modeli przy użyciu Database First i Model First

W miarę wzrostu rozmiaru modelu powierzchnia projektanta zostaje zapełniony i trudno używać. Zazwyczaj rozważamy model z ponad 300 jednostkami, które są zbyt duże, aby efektywnie korzystać z projektanta. Następujący wpis w blogu opisuje kilka opcji do dzielenia dużych modeli: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.

Wpis został zapisany dla pierwszej wersji Entity Framework, ale kroki nadal mają zastosowanie.

### <a name="74-performance-considerations-with-the-entity-data-source-control"></a>7,4 zagadnienia dotyczące wydajności związane z kontrolą źródła danych jednostki

Widzimy przypadki w przypadku wielowątkowych testów wydajnościowych i obciążeniowych, w których wydajność aplikacji sieci Web przy użyciu formantu EntityDataSource pogorszy się. Podstawową przyczyną jest to, że obiekt EntityDataSource wielokrotnie wywołuje obiekt MetadataWorkspace. LoadFromAssembly na zestawach, do których odwołuje się aplikacja sieci Web, aby odnaleźć typy, które mają być używane jako jednostki.

Rozwiązaniem jest ustawienie ContextTypeName obiektu EntityDataSource do nazwy typu klasy pochodnej ObjectContext. Powoduje to wyłączenie mechanizmu, który skanuje wszystkie przywoływane zestawy dla typów jednostek.

Ustawienie pola ContextTypeName uniemożliwia również wystąpienie problemu funkcjonalnego, w którym obiekt EntityDataSource w programie .NET 4,0 zgłasza ReflectionTypeLoadException, gdy nie może załadować typu z zestawu za pomocą odbicia. Ten problem został rozwiązany w programie .NET 4,5.

### <a name="75-poco-entities-and-change-tracking-proxies"></a>7,5 jednostek POCO i serwerów proxy śledzenia zmian

Entity Framework umożliwia korzystanie z niestandardowych klas danych razem z modelem danych bez wprowadzania jakichkolwiek modyfikacji klas danych. Oznacza to, że można użyć "zwykłych" obiektów CLR (POCO), takich jak istniejące obiekty domeny, z modelem danych. Te klasy danych POCO (nazywane również obiektami trwałości-ignorujących), które są mapowane na jednostki, które są zdefiniowane w modelu danych, obsługują większość tych samych zachowań zapytania, INSERT, Update i DELETE jako typy jednostek, które są generowane przez narzędzia Entity Data Model.

Entity Framework może również tworzyć klasy proxy pochodzące z typów POCO, które są używane do włączania funkcji, takich jak ładowanie z opóźnieniem i automatyczne śledzenie zmian w jednostkach POCO. Twoich zajęciach POCO muszą spełniać określone wymagania, aby umożliwić Entity Framework użyć serwerów proxy, zgodnie z opisem w tym miejscu: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).

Serwery proxy śledzenia szansy powiadomień będą powiadamiać menedżera stanu obiektów za każdym razem, gdy jego wartość zostanie zmieniona, więc Entity Framework wie o rzeczywistym stanie jednostek przez cały czas. Jest to realizowane przez dodanie zdarzeń powiadomień do treści metod metody ustawiającej właściwości i posiadanie przetwarzania takich zdarzeń przez menedżera stanu obiektów. Należy pamiętać, że utworzenie jednostki proxy będzie zazwyczaj droższe niż utworzenie jednostki POCO innej niż proxy z powodu dodanego zestawu zdarzeń utworzonych przez Entity Framework.

Gdy jednostka POCO nie ma serwera proxy śledzenia zmian, można znaleźć zmiany, porównując zawartość jednostek z kopią poprzedniego zapisanego stanu. To głębokie porównanie będzie długotrwałym procesem, gdy istnieje wiele jednostek w Twoim kontekście lub gdy jednostki mają bardzo dużą ilość właściwości, nawet jeśli żadna z nich nie zmieniła się od czasu ostatniego porównania.

Podsumowanie: podczas tworzenia serwera proxy śledzenia zmian zostanie wypłacona wydajność, ale śledzenie zmian pomoże przyspieszyć proces wykrywania zmian, gdy jednostki będą mieć wiele właściwości lub jeśli w modelu istnieje wiele jednostek. W przypadku jednostek z małą liczbą właściwości, w których ilość jednostek nie rośnie zbyt wiele, posiadanie serwerów proxy śledzenia zmian może nie być dużo korzystne.

## <a name="8-loading-related-entities"></a>8\. ładowanie powiązanych jednostek

### <a name="81-lazy-loading-vs-eager-loading"></a>8,1 ładowania z opóźnieniem a Ładowanie eager

Entity Framework oferuje kilka różnych sposobów ładowania jednostek, które są powiązane z jednostką docelową. Na przykład podczas wykonywania zapytania dotyczącego produktów istnieją różne sposoby ładowania powiązanych zamówień do menedżera stanu obiektów. Z punktu widzenia wydajności największe pytanie, które należy wziąć pod uwagę podczas ładowania powiązanych jednostek, będzie używać ładowania z opóźnieniem lub ładowania eager.

Podczas ładowania eager powiązane jednostki są ładowane wraz z zestawem jednostek docelowych. Używasz instrukcji include w zapytaniu, aby wskazać, które powiązane jednostki mają zostać umieszczone.

W przypadku używania ładowania z opóźnieniem początkowe zapytanie jest tylko w docelowym zestawie jednostek. Jednak za każdym razem, gdy uzyskujesz dostęp do właściwości nawigacji, w sklepie zostanie wystawione inne zapytanie w celu załadowania jednostki powiązanej.

Po załadowaniu jednostki wszelkie dalsze zapytania dotyczące jednostki będą ładować je bezpośrednio z menedżera stanu obiektów, niezależnie od tego, czy używasz ładowania z opóźnieniem, czy ładowania eager.

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a>8,2 jak wybrać między ładowaniem z opóźnieniem i ładowaniem eager

Ważne jest, aby zrozumieć różnicę między ładowaniem z opóźnieniem i ładowaniem eager, dzięki czemu można wybrać właściwy wybór dla aplikacji. Pomoże to w ocenie kompromisu między wieloma żądaniami w bazie danych a pojedynczym żądaniem, które może zawierać duży ładunek. Może być konieczne użycie eager ładowania w niektórych częściach aplikacji i załadowanie z opóźnieniem w innych częściach.

Przykładem tego, co dzieje się na wystawie, Załóżmy, że chcesz wysyłać zapytania dotyczące klientów, którzy mieszkają w Wielkiej Brytanii i ich liczbie zamówień.

**Używanie ładowania eager**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

**Używanie ładowania z opóźnieniem**

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

Podczas ładowania eager należy wydać pojedyncze zapytanie zwracające wszystkich klientów i wszystkie zamówienia. Polecenie Store wygląda następująco:

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

W przypadku korzystania z ładowania z opóźnieniem należy początkowo wydać następujące zapytanie:

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

Za każdym razem, gdy użytkownik uzyskuje dostęp do właściwości nawigacji Orders klienta, w sklepie zostanie wystawione inne zapytanie, takie jak następujące:

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

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a>8.2.1 ładowanie z opóźnieniem a eager ładowanie Ściągawka arkusza

Nie ma takiego znaczenia, aby można było wybrać eager ładowanie i ładowanie z opóźnieniem. Spróbuj najpierw zrozumieć różnice między obiema strategiami, aby można było dobrze uzyskać świadomą decyzję; należy również rozważyć, czy kod pasuje do żadnego z następujących scenariuszy:

| Scenariusz                                                                    | Nasza sugestia                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Czy musisz uzyskać dostęp do wielu właściwości nawigacji z pobranych jednostek? | **Nie** — obie opcje będą prawdopodobnie. Jeśli jednak ładunek nie jest zbyt duży, mogą wystąpić korzyści z wydajności przy użyciu ładowania eager, ponieważ wymaga to mniejszej liczby podróży sieciowych w celu zmaterializowania obiektów. <br/> <br/> **Tak** — Jeśli chcesz uzyskać dostęp do wielu właściwości nawigacji z jednostek, możesz to zrobić za pomocą wielu instrukcji include w zapytaniu z eager ładowaniem. Im więcej jednostek zostanie uwzględnionych, tym większy ładunek zostanie zwrócony przez zapytanie. Po dołączeniu trzech lub większej liczby jednostek do zapytania Rozważ przełączenie na ładowanie z opóźnieniem. |
| Czy wiesz dokładnie, jakie dane będą potrzebne w czasie wykonywania?                   | Pobieranie z opóźnieniem będzie lepszym rozwiązaniem. W przeciwnym razie możesz zakończyć wykonywanie zapytań dotyczących danych, które nie są potrzebne. <br/> <br/> **Tak** — ładowanie eager jest prawdopodobnie najlepszym trafieniem; ułatwi to szybsze ładowanie całych zestawów. Jeśli zapytanie wymaga pobrania bardzo dużej ilości danych i będzie zbyt wolne, spróbuj wykonać ładowanie z opóźnieniem.                                                                                                                                                                                                                                                       |
| Czy kod wykonywany jest daleko od bazy danych? (zwiększone opóźnienie sieci)  | **Nie** — gdy opóźnienie sieci nie jest problemem, użycie ładowania z opóźnieniem może uprościć kod. Należy pamiętać, że topologia aplikacji może się zmieniać, dlatego nie należy podejmować żadnych bliskości bazy danych. <br/> <br/> **Tak** — w przypadku problemu z siecią możesz zdecydować, co lepiej pasuje do danego scenariusza. Zwykle ładowanie eager będzie lepszym rozwiązaniem, ponieważ wymaga mniejszej liczby rejsów.                                                                                                                                                                                                      |


#### <a name="822-performance-concerns-with-multiple-includes"></a>8.2.2 problemy z wydajnością wielu obejmuje

Gdy będziemy słyszeć pytania dotyczące wydajności, które obejmują problemy związane z czasem odpowiedzi serwera, źródłem problemu często są zapytania z wieloma instrukcjami INCLUDE. Chociaż w tym obiekty pokrewne w zapytaniu są zaawansowane, ważne jest, aby zrozumieć, co się dzieje w ramach okładek.

Wykonywanie zapytania z wieloma instrukcjami include w tym czasie zajmuje stosunkowo dużo czasu, aby przejść przez nasz kompilator wewnętrznego planu, aby utworzyć polecenie magazynu. Większość tego czasu poświęca próbę optymalizacji wyniku zapytania. Polecenie wygenerowany magazyn będzie zawierać sprzężenie zewnętrzne lub Unię dla każdej z nich, w zależności od mapowania. Takie zapytania spowodują, że w ramach jednego ładunku są nawiązane duże połączone wykresy, które będą acerbate wszelkie problemy z przepustowością, zwłaszcza gdy istnieje wiele nadmiarowości w ładunku (na przykład gdy na przechodzenie jest używanych wielu poziomów dołączania) skojarzenia w kierunku "jeden do wielu").

Można sprawdzić przypadki, w których zapytania zwracają nadmiernie duże ładunki, uzyskując dostęp do bazowego TSQL zapytania przy użyciu ToTraceString i wykonując polecenie Store w SQL Server Management Studio, aby zobaczyć rozmiar ładunku. W takich przypadkach można spróbować zmniejszyć liczbę instrukcji include w zapytaniu, aby po prostu wprowadzić potrzebne dane. Lub może być możliwe przerwanie zapytania w krótszej sekwencji podzapytań, na przykład:

**Przed usunięciem zapytania:**

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

**Po przeprowadzeniu przerwania zapytania:**

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

Ta funkcja będzie działać tylko na śledzonych zapytaniach, ponieważ korzystamy z możliwości automatycznego wykonywania rozpoznawania tożsamości i naprawiania skojarzenia.

Podobnie jak w przypadku ładowania z opóźnieniem, kompromis będzie więcej zapytań dla mniejszych ładunków. Można również użyć projekcji poszczególnych właściwości, aby jawnie wybierać tylko potrzebne dane z poszczególnych jednostek, ale nie będzie można ładować jednostek w tym przypadku, a aktualizacje nie będą obsługiwane.

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a>8.2.3 obejście, aby uzyskać pobieranie z opóźnieniem właściwości

Entity Framework obecnie nie obsługuje ładowania z opóźnieniem właściwości skalarnych lub złożonych. Jednak w przypadkach, gdy istnieje tabela zawierająca duży obiekt, taki jak obiekt BLOB, można użyć podziału tabeli, aby oddzielić duże właściwości do osobnej jednostki. Załóżmy na przykład, że masz tabelę produktów, która zawiera kolumnę zdjęć varbinary. Jeśli nie ma często potrzeby uzyskiwania dostępu do tej właściwości w zapytaniach, można użyć podziału tabeli, aby przenieść tylko te części jednostki, która jest zwykle potrzebna. Jednostka reprezentująca zdjęcie produktu zostanie załadowana tylko wtedy, gdy jest to konieczne.

Dobre zasób, który pokazuje, jak włączyć dzielenia tabeli jest "Tabela podział w programie Entity Framework" Gil Fink wpis w blogu: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.

## <a name="9-other-considerations"></a>9 inne zagadnienia

### <a name="91-server-garbage-collection"></a>Odzyskiwanie pamięci serwera 9,1

Niektórzy użytkownicy mogą napotkać rywalizacje o zasoby, które ograniczają równoległość, której oczekuje, gdy moduł wyrzucania elementów bezużytecznych nie jest prawidłowo skonfigurowany. Za każdym razem, gdy program EF jest używany w scenariuszu wielowątkowym lub w dowolnej aplikacji, która jest podobna do systemu po stronie serwera, upewnij się, że włączono odzyskiwanie pamięci serwera. Jest to realizowane za pomocą prostego ustawienia w pliku konfiguracyjnym aplikacji:

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

Powinno to zmniejszyć rywalizację o wątki i zwiększyć przepływność nawet o 30% w scenariuszach zapełnionych przez procesor CPU. Ogólnie rzecz biorąc, należy zawsze testować sposób działania aplikacji przy użyciu klasycznego wyrzucania elementów bezużytecznych (który jest lepiej dostosowany do scenariuszy interfejsu użytkownika i klienta), a także do wyrzucania elementów bezużytecznych serwera.

### <a name="92-autodetectchanges"></a>9,2 AutoDetectChanges

Jak wspomniano wcześniej, Entity Framework mogą pokazać problemy z wydajnością, gdy pamięć podręczna obiektów ma wiele jednostek. Niektóre operacje, takie jak dodawanie, usuwanie, Znajdowanie, wprowadzanie i metody SaveChanges, wyzwalają wywołania do DetectChanges, które mogą zużywać dużą ilość czasu procesora w zależności od rozmiaru pamięci podręcznej obiektów. Przyczyną takiego działania jest fakt, że pamięć podręczna obiektów i Menedżer stanu obiektów próbują zachować się jak najszybciej w przypadku każdej operacji wykonywanej do kontekstu, aby wygenerowane dane były poprawne w ramach szerokiej gamy scenariuszy.

Ogólnie rzecz biorąc, dobrym sposobem jest pozostawienie Entity Framework automatycznego wykrywania zmian w całym cyklu życia aplikacji. Jeśli Twój Scenariusz ma negatywny wpływ na duże użycie procesora CPU, a Twoje profile wskazują, że przyczyna jest wywołaniem DetectChanges, rozważ tymczasowe wyłączenie AutoDetectChanges w poufnej części kodu:

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

Przed wyłączeniem AutoDetectChanges warto zrozumieć, że może to spowodować utratę możliwości śledzenia pewnych informacji o zmianach wprowadzonych w jednostkach przez Entity Framework. Jeśli są obsługiwane nieprawidłowo, może to spowodować niespójność danych w aplikacji. Aby uzyskać więcej informacji na temat wyłączania AutoDetectChanges, przeczytaj \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.

### <a name="93-context-per-request"></a>kontekst 9,3 na żądanie

Konteksty Entity Framework są przeznaczone do użycia jako wystąpienia krótkoterminowe w celu zapewnienia optymalnej wydajności. Należy oczekiwać, że konteksty są krótkie i odrzucane, a jako takie zostały zaimplementowane jako bardzo lekkie i w miarę możliwości wykorzystują metadane. W scenariuszach sieci Web ważne jest, aby mieć to na uwadze i nie mieć kontekstu dłużej niż czas trwania pojedynczego żądania. Podobnie w przypadku scenariuszy innych niż sieci Web kontekst powinien zostać odrzucony w oparciu o zrozumienie różnych poziomów buforowania w Entity Framework. Ogólnie mówiąc, jeden z nich powinien unikać wystąpienia kontekstu w całym cyklu życia aplikacji, a także kontekstów dla wątków i kontekstów statycznych.

### <a name="94-database-null-semantics"></a>Semantyka null bazy danych 9,4

Entity Framework domyślnie generuje kod SQL, który ma semantykę porównania C @ no__t-0 o wartości null. Rozważmy następujące przykładowe zapytanie:

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

W tym przykładzie porównamy wiele zmiennych wartości null z właściwościami dopuszczanymi do wartości null w jednostce, takich jak IDDostawcy i CenaJednostkowa. Wygenerowane dane SQL dla tego zapytania spowodują, że wartość parametru jest taka sama jak wartość kolumny, lub jeśli oba parametry i kolumny mają wartość null. Spowoduje to ukrycie sposobu, w jaki serwer bazy danych obsługuje wartości null i zapewni spójne środowisko C @ no__t-0 null dla różnych dostawców baz danych. Z drugiej strony wygenerowany kod jest bitowym zawiłe i może nie działać prawidłowo, gdy ilość porównań w instrukcji WHERE zapytania rośnie do dużej liczby.

Jednym ze sposobów postępowania z tą sytuacją jest użycie semantyki bazy danych o wartości null. Należy zauważyć, że może to być nieznacznie zachowywać się inaczej w przypadku semantyki o wartości null @ no__t-0, ponieważ teraz Entity Framework generuje prostszy kod SQL, który uwidacznia sposób, w jaki aparat bazy danych będzie obsługiwał wartości null. Semantyki o wartości null bazy danych można aktywować dla kontekstu z jednym wierszem konfiguracji z konfiguracją kontekstu:

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

W przypadku zapytań o małe i średnie rozmiary nie będą wyświetlane zauważalne ulepszenia wydajności podczas korzystania z semantyki o wartości null bazy danych, ale różnica będzie bardziej zauważalna w przypadku zapytań z dużą liczbą potencjalnych porównań o wartości null.

W przykładzie powyższego zapytania różnica wydajności była mniejsza niż 2% w przypadku mikrotestu działającego w środowisku kontrolowanym.

### <a name="95-async"></a>9,5 Async

Entity Framework 6 wprowadzono obsługę operacji asynchronicznych w przypadku uruchamiania programu .NET 4,5 lub nowszego. W większości przypadków aplikacje, które mają rywalizację dotyczącą we/wy, będą korzystać z funkcji asynchronicznego wykonywania zapytań i zapisywania. Jeśli aplikacja nie pogorszy się z rywalizacją we/wy, Użycie Async będzie w najlepszym przypadku wykonywane synchronicznie i zwracać wynik w tym samym czasie co w przypadku wywołania synchronicznego lub w najgorszym przypadku, po prostu Opóźnij wykonywanie do zadania asynchronicznego i Dodaj dodatkowe Tim e do ukończenia Twojego scenariusza.

Instrukcje dotyczące sposobu asynchronicznego programowania pracy, która pomoże przy wyborze rozwiązania, jeśli async poprawi wydajność aplikacji odwiedzanych przez użytkownika [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx). Aby uzyskać więcej informacji na temat używania operacji asynchronicznych na Entity Framework, zobacz [Async Query i Zapisz @ no__t-1.

### <a name="96-ngen"></a>9,6 NGEN

Entity Framework 6 nie jest domyślną instalacją programu .NET Framework. W związku z tym zestawy Entity Framework nie są domyślnie NGEN, co oznacza, że cały kod Entity Framework podlega tym samym kosztom JIT'ing, co każdy inny zestaw MSIL. Może to spowodować spadek wydajności podczas tworzenia i uruchamiania aplikacji w środowiskach produkcyjnych. Aby zmniejszyć koszty procesora CPU i pamięci JIT'ing, zaleca się, aby program NGEN Entity Framework obrazy odpowiednio do potrzeb. Aby uzyskać więcej informacji na temat ulepszania wydajności uruchamiania Entity Framework 6 z programem NGEN, zobacz [Poprawianie wydajności uruchamiania za pomocą narzędzia NGen](~/ef6/fundamentals/performance/ngen.md).

### <a name="97-code-first-versus-edmx"></a>9,7 Code First w porównaniu z EDMX

Entity Framework przyczyny niezgodności między programowaniem zorientowanym na obiekt i relacyjnymi bazami danych przez posiadanie reprezentacji w pamięci modelu koncepcyjnego (obiektów), schematu magazynu (bazy danych) i mapowania między tymi. Te metadane są nazywane Entity Data Model lub EDM jako krótkie. Z tego modelu EDM Entity Framework będą dziedziczyć widoki w celu przeprowadzenia komunikacji między danymi z obiektów znajdujących się w pamięci a bazą danych.

Gdy Entity Framework jest używany z plikiem EDMX, który formalnie Określa model koncepcyjny, schemat magazynu i mapowanie, wówczas etap ładowania modelu musi sprawdzić, czy model EDM jest poprawny (na przykład upewnić się, że nie ma żadnych mapowań), a następnie Wygeneruj widoki, a następnie sprawdź poprawność widoków i przygotuj te metadane do użycia. Tylko wtedy możliwe jest wykonanie zapytania lub zapisanie nowych danych w magazynie danych.

Code First podejście to, w swoim serca, zaawansowanego generatora Entity Data Model. Entity Framework musi utworzyć modelu EDM z dostarczonego kodu; robi to przez analizowanie klas objętych modelem, stosowanie Konwencji i Konfigurowanie modelu za pośrednictwem interfejsu API Fluent. Po skompilowaniu modelu EDM Entity Framework zasadniczo zachowuje się tak samo, jak w projekcie znajduje się plik EDMX. W ten sposób Kompilowanie modelu z Code First dodaje dodatkową złożoność, która tłumaczy na wolniejszy czas uruchamiania dla Entity Framework w porównaniu z EDMX. Koszt jest w pełni zależny od rozmiaru i złożoności tworzonego modelu.

W przypadku korzystania z EDMX i Code First należy pamiętać, że elastyczność wprowadzona przez Code First zwiększa koszt kompilowania modelu po raz pierwszy. Jeśli aplikacja może wytrzymać koszt tego obciążenia po raz pierwszy, zazwyczaj Code First będzie preferowanym sposobem przejścia.

## <a name="10-investigating-performance"></a>10 badanie wydajności

### <a name="101-using-the-visual-studio-profiler"></a>10,1 przy użyciu profilera programu Visual Studio

Jeśli masz problemy z wydajnością Entity Framework, możesz użyć profilera, takiego jak wbudowany w program Visual Studio, aby zobaczyć, gdzie Twoja aplikacja spędza swoją godzinę. To narzędzie, firma Microsoft służącego do generowania wykresów kołowych "Eksplorowanie wydajności programu ADO.NET Entity Framework — część 1" wpis w blogu ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) ukazują, gdzie Entity Framework spędza czas podczas wykonywania kwerend ścieżce nieaktywnej i bez wyłączania zasilania.

Wpis "Entity Framework profilowania przy użyciu programu Visual Studio 2010 Profiler" został zapisany przez dane i modelowanie zespołu Doradczego ds. klienta pokazuje rzeczywisty przykład sposobu użycia profilera do zbadania problemu z wydajnością.  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>. Ten wpis został zapisany dla aplikacji systemu Windows. Jeśli zachodzi potrzeba profilowania aplikacji sieci Web, narzędzia Windows Performance Recorder (WP) i Windows Performance Analyzer (WPA) mogą działać lepiej niż w przypadku programu Visual Studio. WPR i WPA są częścią zestawu narzędzi wydajności Windows, który jest dołączony do Windows Assessment and Deployment Kit ( [http://www.microsoft.com/download/details.aspx?id=39982](https://www.microsoft.com/download/details.aspx?id=39982)).

### <a name="102-applicationdatabase-profiling"></a>10,2 Profilowanie aplikacji/bazy danych

Narzędzia, takie jak Profiler wbudowany w program Visual Studio, informują o tym, gdzie Twoja aplikacja jest w trakcie.  Dostępny jest inny typ profilera służący do przeprowadzania dynamicznej analizy uruchomionej aplikacji w środowisku produkcyjnym lub przedprodukcyjnym w zależności od potrzeb, a także wyszukuje typowe pułapek i antywzorce dostępu do bazy danych.

Dwa komercyjnego profilowania są Profiler Framework jednostki ( \<http://efprof.com>) i ORMProfiler ( \<http://ormprofiler.com>).

Jeśli aplikacja jest aplikacją MVC używającą Code First, można użyć MiniProfiler StackExchange. Scott Hanselman opis tego narzędzia w jego blog znajduje się na: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.

Aby uzyskać więcej informacji na temat profilowania działania bazy danych aplikacji, zobacz artykuł dotyczący usługi Julie Lerman w witrynie MSDN Magazine zatytułowany [profilowanie działania bazy danych w Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).

### <a name="103-database-logger"></a>Rejestrator bazy danych 10,3

W przypadku korzystania z programu Entity Framework 6 należy również rozważyć użycie wbudowanych funkcji rejestrowania. Właściwość baza danych kontekstu może być zainstruuja, aby rejestrować swoje działania za pomocą prostej konfiguracji jednowierszowej:

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

W tym przykładzie działanie bazy danych zostanie zarejestrowane w konsoli programu, ale właściwości dziennika można skonfigurować do wywoływania akcji @ no__t-0string @ no__t-1 delegata.

Jeśli chcesz włączyć rejestrowanie bazy danych bez ponownego kompilowania i używasz Entity Framework 6,1 lub nowszej, możesz to zrobić przez dodanie interceptora w pliku Web. config lub App. config aplikacji.

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

Aby uzyskać więcej informacji na temat dodawania rejestrowanie bez konieczności ponownego kompilowania przejdź do pozycji \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.

## <a name="11-appendix"></a>11 dodatek

### <a name="111-a-test-environment"></a>Środowisko testowe 11,1 A.

W tym środowisku jest stosowana konfiguracja 2-maszynowa z bazą danych na oddzielnym komputerze od aplikacji klienckiej. Maszyny znajdują się w tym samym stojaku, więc opóźnienie sieci jest stosunkowo małe, ale bardziej realistyczne niż środowisko pojedynczej maszyny.

#### <a name="1111-app-server"></a>Serwer aplikacji 11.1.1

##### <a name="11111-software-environment"></a>Środowisko oprogramowania 11.1.1.1

-   Środowisko oprogramowania Entity Framework 4
    -   Nazwa systemu operacyjnego: Windows Server 2008 R2 Enterprise SP1.
    -   Visual Studio 2010 — wersja Ultimate.
    -   Visual Studio 2010 z dodatkiem SP1 (tylko w przypadku niektórych porównań).
-   Entity Framework 5 i 6 środowiska oprogramowania
    -   Nazwa systemu operacyjnego: Windows 8,1 Enterprise
    -   Visual Studio 2013 — wersja Ultimate.

##### <a name="11112-hardware-environment"></a>Środowisko sprzętowe 11.1.1.2

-   Podwójny procesor:     Intel (R) Xeon (R) procesora CPU L5520 W3530 @ 2,27 GHz, 2261 Mhz8 GHz, 4 rdzenie, 84 procesorów logicznych.
-   2412 GB RamRAM.
-   dysk 136 GB SCSI250GB SATA 7200 RPM WŁĄCZONĄ/s podzielony na 4 partycje.

#### <a name="1112-db-server"></a>Serwer 11.1.2 DB

##### <a name="11121-software-environment"></a>Środowisko oprogramowania 11.1.2.1

-   Nazwa systemu operacyjnego: Windows Server 2008 R28.1 Enterprise SP1.
-   SQL Server 2008 R22012.

##### <a name="11122-hardware-environment"></a>Środowisko sprzętowe 11.1.2.2

-   Pojedynczy procesor: Intel (R) Xeon (R) procesora CPU L5520 @ 2,27 GHz, 2261 MhzES-1620 0 @ 3.60 GHz, 4 rdzenie, 8 procesorów logicznych.
-   824 GB RamRAM.
-   dysk 465 GB ATA500GB SATA 7200 RPM 6 GB/s podzielony na 4 partycje.

### <a name="112-b-query-performance-comparison-tests"></a>11,2 B. testy porównawcze wydajności zapytań

Model Northwind został użyty do wykonania tych testów. Zostało ono wygenerowane na podstawie bazy danych za pomocą narzędzia Entity Framework Designer. Następnie Poniższy kod został użyty do porównania wydajności opcji wykonywania zapytania:

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

### <a name="113-c-navision-model"></a>Model 11,3 C. Navision

Baza danych systemu Navision to duża baza danych służąca do pokazania systemu Microsoft Dynamics — NAV. Wygenerowany model koncepcyjny zawiera 1005 zestawów jednostek i 4227 zestawów skojarzeń. Model używany w teście ma wartość "Flat" — nie dodano żadnego dziedziczenia.

#### <a name="1131-queries-used-for-navision-tests"></a>zapytania 11.3.1 używane dla testów systemu Navision

Lista zapytań używana z modelem systemu Navision zawiera trzy kategorie Entity SQL zapytań:

##### <a name="11311-lookup"></a>11.3.1.1, wyszukiwanie

Proste zapytanie wyszukiwania bez agregacji

-   Liczbą 16232
-   Przykład:

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312singleaggregating"></a>11.3.1.2 SingleAggregating

Normalne zapytanie analizy biznesowej z wieloma agregacjami, ale bez sum częściowych (pojedyncze zapytanie)

-   Liczbą 2313
-   Przykład:

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

Gdzie MDF @ no__t-0SessionLogin @ no__t-1Time @ no__t-2Max () jest zdefiniowany w modelu jako:

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313aggregatingsubtotals"></a>11.3.1.3 AggregatingSubtotals

Zapytanie analizy biznesowej z agregacjami i sumami częściowymi (za pośrednictwem UNION ALL)

-   Liczbą 178
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
