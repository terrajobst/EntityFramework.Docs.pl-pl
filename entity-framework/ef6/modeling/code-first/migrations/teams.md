---
title: Migracje Code First na środowiska zespołowe - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 4c2d9a95-de6f-4e97-9738-c1f8043eff69
ms.openlocfilehash: 42f52e63fd6cfc1f02d6a721594f4a161eea9a7b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997302"
---
# <a name="code-first-migrations-in-team-environments"></a>Migracje Code First na środowiska zespołowe
> [!NOTE]
> W tym artykule przyjęto założenie, że wiesz, jak użyć migracje Code First w podstawowych scenariuszy. Jeśli tego nie zrobisz, a następnie należy odczytać [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) przed kontynuowaniem.

## <a name="grab-a-coffee-you-need-to-read-this-whole-article"></a>Pobierz kawy, musisz przeczytaj ten artykuł, całe

Problemów w środowiskach zespołu są głównie wokół scalanie migracji po dwóch programistów wygenerowały migracji w swoich lokalnych kod podstawowy. Kroki, aby rozwiązać te są całkiem proste, wymagają one może być pełny opis sposobu działania migracji. Proszę nie po prostu przejść od razu na końcu — Poświęć chwilę Aby odczytać cały artykuł, aby upewnić się, że możesz pomyślnie.

## <a name="some-general-guidelines"></a>Ogólne wskazówki

Przed rozpoczęciem omawiania jak zarządzać scalania migracje generowane przez wielu deweloperów, poniżej przedstawiono ogólne wskazówki, które ukierunkowane na powodzenie.

### <a name="each-team-member-should-have-a-local-development-database"></a>Każdy członek zespołu powinien mieć lokalny rozwój bazy danych

Zastosowań migracje  **\_ \_MigrationsHistory** tabelę do przechowywania, jakie migracji zostały zastosowane do bazy danych. Jeśli masz wielu deweloperów generowania różne migracje podczas próby pod kątem tej samej bazy danych (i udostępnianie w ten sposób  **\_ \_MigrationsHistory** tabeli) migracje zamierza uzyskać bardzo mylące.

Oczywiście w przypadku członków zespołu, którzy nie są Generowanie migracje występuje problem, nie pozwól, aby udostępnić bazę danych centralnej rozwoju.

### <a name="avoid-automatic-migrations"></a>Należy unikać automatycznej migracji

Mierzenie to, że automatycznej migracji początkowo wyglądają dobrze w środowiskach zespołu, ale w rzeczywistości po prostu nie działają. Jeśli chcesz wiedzieć Dlaczego, Zachowaj czytania — Jeśli nie, następnie można przejść do następnej sekcji.

Automatyczne migracji umożliwia zostały zaktualizowane w celu dopasowania bieżącego modelu bez konieczności generowanie kodu plików (migracje oparte na kodzie) schemat bazy danych. Automatyczne migracje będzie bardzo dobrze pracować w środowisku zespołowym, jeśli tylko nigdy nie użył ich i nigdy nie generowane wszystkie migracje oparte na kodzie. Problem polega na czy automatycznej migracji są ograniczone i nie obsługują wielu operacji — zmienia nazwę kolumny właściwości/przenoszenia danych do innej tabeli itp. Do obsługi tych scenariuszy, na końcu generowania migracje oparte na kod (i edytowania utworzony szkielet kodu), które są zmieszane Between zmiany, które są obsługiwane przez automatyczne migracji. Dzięki temu niemal na niemożliwe do scalania zmian, gdy dwa deweloperzy ewidencjonują migracji.

## <a name="screencasts"></a>Zrzuty ekranu

Jeśli zrzut ekranu niż przeczytaj ten artykuł będzie wolisz obejrzeć, następujące dwa filmy wideo obejmuje tę samą zawartość, jak w tym artykule.

### <a name="video-one-migrations---under-the-hood"></a>Wideo 1: "Migracje - Under the Hood"

[Ten zrzut ekranu](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) opisano sposób migracji śledzi i używa informacji o modelu, aby wykrywać zmiany modelu.

### <a name="video-two-migrations---team-environments"></a>Dwa wideo: "Migracje - środowiska zespołowe"

Opierając się na koncepcji z poprzednim filmu wideo, [ten zrzut ekranu](http://channel9.msdn.com/blogs/ef/migrations-team-environments) omówiono zagadnienia, które pojawiają się w środowisku zespołowym oraz sposoby ich rozwiązywania.

## <a name="understanding-how-migrations-works"></a>Zrozumienie sposobu działania migracji

Klawisz, aby pomyślnie przy użyciu migracji w środowisku zespołowym, to podstawowy zrozumienie, jak migracje śledzi i używa informacji o modelu do wykrycia zmiany modelu.

### <a name="the-first-migration"></a>Pierwszej migracji

Po pierwszej migracji możesz dodać do projektu, możesz uruchomić podobny **migracji Dodaj pierwszy** w konsoli Menedżera pakietów. Kroki wysokiego poziomu, które wykonuje to polecenie jest na ilustracji poniżej.

![FirstMigration](~/ef6/media/firstmigration.png)

Bieżący model jest obliczana w kodzie (1). Obiekty bazy danych wymagane jest następnie obliczana z użyciem różnią się modelu (2) — ponieważ jest to pierwszy migracji modelu różnią się jedynie używa pusty model do porównania. Wymagane zmiany są przekazywane do generatora kodu, aby skompilować kod Wymagana migracja (3), która jest następnie dodawana do rozwiązania programu Visual Studio (4).

Oprócz kodu rzeczywistą migrację, który jest przechowywany w pliku głównego kodu migracje generuje również niektórych dodatkowych plików z kodem. Te pliki są metadane, który jest używany przez migracje i nie coś, co należy edytować. Jeden z tych plików jest plik zasobów (.resx) zawiera migawkę tego modelu w czasie migracji została wygenerowana. Zobaczysz, jak jest on używany w następnym kroku.

W tym momencie należy prawdopodobnie uruchomić **Update-Database** Aby zastosować zmiany do bazy danych, a następnie przejdź dotyczących implementowania innych obszarów aplikacji.

### <a name="subsequent-migrations"></a>Kolejne migracje

Później możesz wrócić i wprowadzić pewne zmiany na modelu — w tym przykładzie dodamy **adresu Url** właściwości **blogu**. Następnie może wydać polecenie takich jak **AddUrl migracji Dodaj** zmienia się do tworzenia szkieletu migracji, aby zastosować odpowiednie bazy danych. Kroki wysokiego poziomu, które wykonuje to polecenie jest na ilustracji poniżej.

![SecondMigration](~/ef6/media/secondmigration.png)

Podobnie jak wcześniej bieżący model jest obliczana na podstawie kodu (1). Tym razem istnieją migracji istniejących więc w poprzednim modelu są pobierane z najnowszych migracji (2). Te dwa modele są diffed można znaleźć wymaganych zmian w bazie danych (3), a następnie proces kończy się tak jak poprzednio.

Ten sam proces jest używany dla dalszych migracji, które dodajesz do projektu.

### <a name="why-bother-with-the-model-snapshot"></a>Dlaczego odblokowane z migawką modelu?

Możesz się zastanawiać, dlaczego EF bothers z migawką modelu — Dlaczego się nie tylko bazy danych. Jeśli tak, Czytaj dalej. Jeśli użytkownik nie chce można pominąć tę sekcję.

Istnieje wiele możliwych przyczyn, które EF przechowuje migawki modelu wokół:

-   Umożliwia ona bazy danych w celu odstają od modelu platformy EF. Tych zmian bezpośrednio w bazie danych lub utworzony szkielet kodu można zmienić w migracji w taki sposób, aby wprowadzić zmiany. Poniżej przedstawiono kilka przykładów tego w praktyce:
    -   Aby dodać kolumnę do jednej lub kilku tabel wstawione i zaktualizowane, ale nie chcesz uwzględnić te kolumny w modelu platformy EF. Jeśli migracja przyjrzano się bazy danych stale może spróbować usunąć te kolumny, za każdym razem, gdy działanie migracji. Przy użyciu migawki modelu, EF tylko wykryje uzasadnione zmiany w modelu.
    -   Chcesz zmienić treść funkcji procedurę składowaną, która jest używana w przypadku aktualizacji do uwzględnienia niektórych rejestrowania. Jeśli migracji przyjrzano tę procedurę składowaną z bazy danych będzie stale spróbuj i przywrócić jego definicji, który oczekuje, że EF. Za pomocą migawki modelu, EF będzie tylko tworzenia szkieletu kodu lub zmieniać procedury składowanej w przypadku zmiany kształtu procedurę opisaną w modelu platformy EF.
    -   Te same zasady mają zastosowanie do dodawania dodatkowych indeksów, w tym dodatkowe tabele w bazie danych, mapowanie EF na widok bazy danych, który znajduje się za pośrednictwem tabeli itp.
-   Modelu platformy EF zawiera więcej niż tylko kształt bazy danych. Posiadanie cały model umożliwia migracji wyświetlić informacje dotyczące właściwości i klasy w modelu oraz sposobu mapowania ich na kolumn i tabel. Informacje te pozwalają migracje się bardziej inteligentne w kodzie, który go szkielety mechanizmów. Na przykład jeśli zmienisz nazwę kolumny, która właściwość mapuje do migracji może wykrywać zmiany nazwy, zobaczysz, że jest tę samą właściwość — coś, co nie można przeprowadzić, jeśli masz tylko schemat bazy danych. 

## <a name="what-causes-issues-in-team-environments"></a>Co powoduje, że problemy w środowiskach zespołu

Przepływ pracy omówione w poprzednich działa sekcja doskonałe, podczas pracy nad aplikacją jednego dewelopera. Działa dobrze w środowisku zespołowym, jeśli jesteś jedyną osobą, zmiany wprowadzone w modelu. W tym scenariuszu można wprowadzić zmiany w modelu, generowania migracje i przesyłanie ich do kontroli źródła. Inni deweloperzy mogą synchronizowanie zmian i uruchamiać **Update-Database** zastosować zmian schematu.

Problemy z Rozpocznij wystąpić w przypadku wielu deweloperów wprowadzanie zmian modelu platformy EF i przesyłanie do kontroli źródła, w tym samym czasie. Nie posiada EF jest najwyższej klasy można scalić migracji lokalnej za pomocą migracji, które innemu deweloperowi zostało przesłane do kontroli źródła, ponieważ Ostatnia synchronizacja.

## <a name="an-example-of-a-merge-conflict"></a>Przykładem konfliktu scalania

Pierwszy Przyjrzyjmy się konkretny przykład konfliktu scalania. Będziemy dalej na przykład, który przyjrzeliśmy się wcześniej. Jako początkowy punkt możemy założyć zmian z poprzedniej sekcji zostały zaewidencjonowane przez dewelopera, oryginalnym. Będziesz Śledzimy dwa deweloperom, jak oni wprowadzić zmiany do kodu podstawowego

Firma Microsoft będzie śledzić modelu platformy EF migracjach do większej liczby zmian. Punkt początkowy zarówno deweloperów zsynchronizowanych repozytorium kontroli źródła, jak pokazano na poniższym rysunku.

![StartingPoint](~/ef6/media/startingpoint.png)

Deweloper \#1 i deweloperów \#2 teraz sprawia, że pewne zmiany do modelu platformy EF w swoich lokalnych kod podstawowy. Deweloper \#dodaje 1 **ocena** właściwości **Blog** — i generuje **AddRating** migracji w celu zastosowania zmian w bazie danych. Deweloper \#2 dodaje **czytelnicy** właściwości **Blog** — i generuje odpowiedni **AddReaders** migracji. Uruchom zarówno deweloperów **Update-Database**, aby zastosować zmiany w swoich lokalnych baz danych, a następnie kontynuuj, tworzenia aplikacji.

> [!NOTE]
> Migracje mają prefiks sygnaturę czasową, dzięki czemu nasze grafika przedstawia, migracja AddReaders z deweloperów \#2 pochodzi po migracji AddRating Developer \#1. Czy dla deweloperów \#1 lub \#2 generowane migracji pierwszej sprawia, że nie ma wpływu problemów pracy w zespole lub procesu scalania ich, które omówimy w następnej sekcji.

![LocalChanges](~/ef6/media/localchanges.png)

To dzień, szczęście Deweloper \#1 po ich wprowadzeniu przedstawią ich zmiany. Ponieważ żaden inny użytkownik zaewidencjonował ponieważ one zsynchronizowane z własnym repozytorium, ich zmiany może przesłać tylko bez przeprowadzania żadnych scalania.

![Prześlij](~/ef6/media/submit.png)

Teraz nadszedł czas na dewelopera \#2, aby przesłać. Nie są one takie szczęście. Ponieważ ktoś inny zostało przesłane zmiany, ponieważ są synchronizowane, należy ściągnąć zmiany i scalania. System kontroli źródła prawdopodobnie będzie automatycznie scalić zmiany na poziomie kodu, ponieważ są one bardzo proste. Stan dla deweloperów \#2 użytkownika lokalnego repozytorium po synchronizacji jest przedstawiony na poniższym rysunku. 

![Ściągnij](~/ef6/media/pull.png)

W tym testowanie Developer \#2 można uruchomić **Update-Database** wykryje nowe **AddRating** migracji (nie został zastosowany do deweloperów \#2 bazy danych) i zastosować je. Teraz **ocena** kolumna zostanie dodana do **blogi** tabeli i bazy danych jest zsynchronizowany z modelu.

Istnieje jednak kilka problemów:

1.  Mimo że **Update-Database** zastosuje **AddRating** migracji również zgłosi Ostrzeżenie: *nie można zaktualizować bazy danych, aby dopasować w bieżącym modelu, ponieważ istnieją oczekujące zmiany i Automatyczna migracja jest wyłączona...*
    Problem polega na to, że migawka modelu przechowywane w ostatniej migracji (**AddReader**) brakuje **ocena** właściwość **blogu** (ponieważ nie było częścią modelu po Migracja została wygenerowana). Kod najpierw wykrywa, że model w ostatniej migracji nie jest zgodny z bieżącym modelu i wywołuje ostrzeżenie.
2.  Uruchomiona jest aplikacja mogłoby spowodować InvalidOperationException o treści "*modelu kopii kontekstu"BloggingContext"została zmieniona od czasu utworzenia bazy danych. Należy wziąć pod uwagę przy użyciu migracje Code First w aktualizacji bazy danych..."*
    Ponownie problem polega na migawki modelu, przechowywane w ostatniej migracji nie jest zgodna bieżącego modelu.
3.  Na koniec będzie oczekujemy, że działa **migracji Dodaj** teraz wygeneruje pusty migracji (ponieważ nie wprowadzono żadnych zmian do zastosowania do bazy danych). Ale ponieważ migracje porównuje bieżący model na jeden z ostatnich migracji (czyli Brak **ocena** właściwość) on zostanie faktycznie tworzenia szkieletu innego **AddColumn** wywołanie do dodania w **Ocena** kolumny. Oczywiście, ta migracja może zakończyć się niepowodzeniem podczas **Update-Database** ponieważ **ocena** kolumna już istnieje.

## <a name="resolving-the-merge-conflict"></a>Rozwiązywanie konfliktu scalania

Dobra wiadomość jest, że nie jest zbyt trudne do przeciwdziałania scalanie ręczne — pod warunkiem, że zrozumienie sposobu działania migracji. Jeśli zostały pominięte w sekcji do tej sekcji... Niestety musisz przejść wstecz i czytania dalszej części tego artykułu, najpierw!

Dostępne są dwie opcje najprostsza polega na generowaniu puste migracji, który ma poprawne bieżący model jako migawka. Drugą opcją jest aktualizacja migawki w ostatniej migracji ma poprawny model migawki. Drugą opcją jest nieco trudniejsze i nie można użyć w każdym scenariuszu, ale jest również bardziej przejrzysty, ponieważ nie wymaga, dodanie dodatkowych migracji.

### <a name="option-1-add-a-blank-merge-migration"></a>Opcja 1: Dodaj migrację puste scalania

W przypadku tej opcji, wygenerowanie pustego migracji wyłącznie w celu umożliwienia się, że najnowsze migracji ma w nim przechowywane migawki poprawny model.

Ta opcja może służyć niezależnie od wartości która wygenerowała ostatniej migracji. W przykładzie, możemy wykonywano Developer \#2 jest zwracając szczególną uwagę scalania i ich wystąpienia, aby wygenerować ostatniej migracji. Ale te same kroki mogą być używane, jeśli deweloper \#1 generowane ostatniej migracji. Te kroki mają zastosowanie, jeśli wiele migracji są zaangażowani — firma Microsoft została właśnie zostało spojrzenie na dwa w celu uproszczenia.

Następujący proces może służyć w tym podejściu, rozpoczynając od godziny, o których należy pamiętać, że masz zmiany, które muszą zostać zsynchronizowane z kontroli źródła.

1.  Upewnij się, że wszelkie zmiany oczekujące modelu w kodzie lokalnych, zostały zapisane w migracji. Ten krok zapewnia, że nie przegap wszelkie uzasadnione zmiany, kiedy nastąpi moment, aby wygenerować pusty migracji.
2.  Synchronizowanie z kontrolą źródła.
3.  Uruchom **Update-Database** do zastosowania nowej migracji, które zostały zaewidencjonowane innym deweloperom.
    **
    *Uwaga: *** Jeśli nie dostaniesz żadnych ostrzeżeń polecenia Update-Database nie było żadnych nowych migracje od innych deweloperów i trzeba wykonywać dalszych scalania.*
4.  Uruchom **migracji Dodaj &lt;wybierz\_\_nazwa&gt; — IgnoreChanges** (na przykład **scalania migracji Dodaj — IgnoreChanges**). To generuje migracji za pomocą wszystkich metadanych (w tym migawki bieżącego modelu), ale będzie ignorować wszelkie zmiany wykrycia podczas porównywania bieżący model z migawką w ostatnim migracji (co oznacza, Pobierz pusty **się** i **Dół** metody).
5.  Kontynuować tworzenie lub Prześlij do kontroli źródła (po wykonywania z testów jednostkowych oczywiście).

W tym miejscu jest stan dla deweloperów \#2 użytkownika lokalnego kodu bazowego po zakończeniu korzystania z tej metody.

![MergeMigration](~/ef6/media/mergemigration.png)

### <a name="option-2-update-the-model-snapshot-in-the-last-migration"></a>Opcja 2: Zaktualizuj migawkę modelu w ostatniej migracji

Ta opcja jest bardzo podobna do opcji 1, ale usuwa dodatkowe puste migracji —, ponieważ umożliwia prawdzie, kto chce, aby pliki dodatkowego kodu w swoich rozwiązaniach.

**To podejście jest możliwe tylko, jeśli istnieje tylko w podstawowym kod lokalny najnowszych migracji, a nie jeszcze został przesłany do kontroli źródła (na przykład, jeśli ostatni migracji został wygenerowany przez użytkownika, wykonując scalanie)**. Edytować metadane migracji, które inni deweloperzy już zastosowane do swojej bazy danych rozwoju — lub nawet co gorsza stosowane do produkcyjnej bazy danych — może spowodować nieoczekiwane działania niepożądane. W procesie zamierzamy wycofać migrację ostatni w naszym lokalnej bazy danych, a następnie ponownie zastosuj go ze zaktualizowanymi metadanymi.

Podczas ostatniej migracji konieczne po prostu znajdować się w lokalnym kod podstawowy, nie ma żadnych ograniczeń liczbę lub kolejność migracji, które ją kontynuować. Może istnieć wiele migracji z wielu różnych deweloperów i Zastosuj te same czynności — firma Microsoft została właśnie zostało spojrzenie na dwa w celu uproszczenia.

Następujący proces może służyć w tym podejściu, rozpoczynając od godziny, o których należy pamiętać, że masz zmiany, które muszą zostać zsynchronizowane z kontroli źródła.

1.  Upewnij się, że wszelkie zmiany oczekujące modelu w kodzie lokalnych, zostały zapisane w migracji. Ten krok zapewnia, że nie przegap wszelkie uzasadnione zmiany, kiedy nastąpi moment, aby wygenerować pusty migracji.
2.  Synchronizowanie z kontrolą źródła.
3.  Uruchom **Update-Database** do zastosowania nowej migracji, które zostały zaewidencjonowane innym deweloperom.
    **
    *Uwaga: *** Jeśli nie dostaniesz żadnych ostrzeżeń polecenia Update-Database nie było żadnych nowych migracje od innych deweloperów i trzeba wykonywać dalszych scalania.*
4.  Uruchom **Update-Database-TargetMigration &lt;drugi\_ostatniego\_migracji&gt;**  (w przykładzie, możemy wykonywano takie rozwiązanie byłoby **aktualizacji bazy danych — TargetMigration AddRating**). To role bazy danych z powrotem do stanu drugiego ostatnie migracji — skutecznie "bez stosowania" ostatniej migracji z bazy danych.
    **
    *Uwaga: *** ten krok jest wymagany, aby bezpiecznie edytować metadane migracji, ponieważ metadane są również przechowywane w \_ \_MigrationsHistoryTable bazy danych. Jest to, dlaczego tej opcji należy używać tylko, jeśli ostatni migracji jest tylko w lokalnej bazie kodu. Jeśli innych baz danych było ostatniej migracji, które są stosowane również masz do nich wycofać, a następnie ponownie zastosuj ostatniej migracji w celu zaktualizowania metadanych.* 
5.  Uruchom **migracji Dodaj &lt;pełną\_nazwa\_tym\_sygnatura czasowa\_z\_ostatniego\_migracji** &gt; (w tym przykładzie Firma Microsoft wykonywano powinien to być podobny do **201311062215252 migracji Dodaj\_AddReaders**).
    **
    *Uwaga: *** muszą zawierać sygnaturę czasową, aby migracje wie, którą chcesz edytować istniejące migracji, a nie tworzenia szkieletów nową.*
    Spowoduje to zaktualizowanie metadanych dla ostatniej migracji do dopasowania w bieżącym modelu. Po wykonaniu polecenia, ale jest to dokładnie co chcesz otrzymasz następujące ostrzeżenie. "*Tylko kodu projektanta do migracji" 201311062215252\_AddReaders został ponownie utworzony szkielet. Aby ponownie utworzyć szkielet całego migracji, należy użyć parametru - Force. "*
6.  Uruchom **Update-Database** Ponowne zgłoszenie chęci najnowszych migracji ze zaktualizowanymi metadanymi.
7.  Kontynuować tworzenie lub Prześlij do kontroli źródła (po wykonywania z testów jednostkowych oczywiście).

W tym miejscu jest stan dla deweloperów \#2 użytkownika lokalnego kodu bazowego po zakończeniu korzystania z tej metody.

![UpdatedMetadata](~/ef6/media/updatedmetadata.png)

## <a name="summary"></a>Podsumowanie

Istnieją niektóre wyzwania, korzystając z migracje Code First w środowisko pracy dla zespołu. Jednak podstawową wiedzę na temat sposobu działania migracji i niektóre proste podejścia do rozwiązywania konfliktów scalania ułatwiają przezwyciężyć te wyzwania.

Podstawowe problem jest nieprawidłowa z metadanych najnowszych migracji. Powoduje to Code First nieprawidłowo wykrywa, że bieżący model i schemat bazy danych nie są zgodne i tworzenia szkieletu niepoprawny kod w następnej migracji. Takiej sytuacji można rozwiązać przez generowanie puste migracji za pomocą modelu poprawne lub zaktualizowanie metadanych w najnowszych migracji.
