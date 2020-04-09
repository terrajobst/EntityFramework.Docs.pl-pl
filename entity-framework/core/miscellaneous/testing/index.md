---
title: Komponenty testowe przy użyciu EF Core - EF Core
description: Różne podejścia do testowania aplikacji korzystających z EF Core
author: ajcvickers
ms.date: 03/23/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: b1ab37ebb0a3aae09d5d5b225f746cf83dfba170
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634251"
---
# <a name="testing-code-that-uses-ef-core"></a>Testowanie kodu, który używa EF Core

Testowanie kodu, który uzyskuje dostęp do bazy danych wymaga albo:
* Uruchamianie zapytań i aktualizacji w tym samym systemie bazy danych, który jest używany w produkcji.
* Uruchamianie zapytań i aktualizacji względem innych łatwiejszych w zarządzaniu systemem baz danych.
* Za pomocą testu podwaja lub inny mechanizm, aby uniknąć korzystania z bazy danych w ogóle.

W tym dokumencie przedstawiono kompromisy związane z każdym z tych wyborów i pokazuje, jak EF Core może być używany z każdym podejściem.  

## <a name="all-database-providers-are-not-equal"></a>Wszyscy dostawcy baz danych nie są

Jest bardzo ważne, aby zrozumieć, że EF Core nie jest przeznaczony do abstrakcji każdy aspekt systemu baz danych.
Zamiast tego EF Core jest wspólny zestaw wzorców i pojęć, które mogą być używane z dowolnego systemu bazy danych.
Ef Core dostawców bazy danych następnie warstwy zachowania specyficzne dla bazy danych i funkcji w tej wspólnej struktury.
Dzięki temu każdy system bazy danych, aby zrobić to, co robi najlepiej przy jednoczesnym zachowaniu podobieństwa, w stosownych przypadkach, z innymi systemami bazy danych. 

Zasadniczo oznacza to, że wyłączenie dostawcy bazy danych spowoduje zmianę zachowania EF Core i nie można oczekiwać, że aplikacja będzie działać poprawnie, chyba że jawnie uwzględnia wszystkie różnice w zachowaniu.
Mając na uwadze to, w wielu przypadkach będzie to działać, ponieważ istnieje wysoki stopień podobieństwa między relacyjnymi bazami danych.
To jest dobre i złe.
Dobrze, ponieważ przemieszczanie się między bazami danych może być stosunkowo łatwe.
Źle, ponieważ może dać fałszywe poczucie bezpieczeństwa, jeśli aplikacja nie jest w pełni przetestowany przeciwko nowemu systemowi bazy danych.  

## <a name="approach-1-production-database-system"></a>Podejście 1: System produkcyjnej bazy danych

Jak opisano w poprzedniej sekcji, jedynym sposobem, aby upewnić się, że testujesz to, co działa w produkcji jest użycie tego samego systemu bazy danych.
Na przykład jeśli wdrożona aplikacja używa sql azure, następnie testowanie należy również wykonać przeciwko SQL Azure.

Jednak posiadanie wszystkich testów uruchamiania deweloperów przeciwko sql azure podczas aktywnej pracy nad kodem będzie zarówno powolne, jak i kosztowne.
Ilustruje to główny kompromis związany z tymi podejściami: kiedy należy odejść od systemu produkcyjnej bazy danych, aby poprawić wydajność testów?

Na szczęście w tym przypadku odpowiedź jest dość prosta: użyj lokalnego lub lokalnego programu SQL Server do testowania deweloperów.
SQL Azure i SQL Server są bardzo podobne, więc testowanie sql server jest zwykle uzasadnione kompromisu.
Mając na to na powiedziane, nadal jest mądry, aby uruchomić testy przeciwko samej platformy SQL Azure przed przejściem do produkcji.
 
### <a name="localdb"></a>Localdb 

Wszystkie główne systemy baz danych mają jakąś formę "Developer Edition" do testowania lokalnego.
SQL Server posiada również funkcję o nazwie [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).
Główną zaletą LocalDb jest to, że obraca się wystąpienie bazy danych na żądanie.
Pozwala to uniknąć usługi bazy danych uruchomionej na komputerze, nawet jeśli nie są uruchomione testy.

LocalDb nie jest bez problemów:
* Nie obsługuje wszystkiego, co robi [program SQL Server Developer Edition.](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15)
* Nie jest dostępny w systemie Linux.
* Może to spowodować opóźnienie przy pierwszym uruchomieniu testowym, ponieważ usługa jest spun up.

Osobiście nigdy nie znalazłem problemu z usługą bazy danych działającą na moim komputerze deweloperskim i ogólnie polecam użycie Developer Edition.
Jednak może to być odpowiednie dla niektórych osób, zwłaszcza na mniej wydajnych komputerach deweloperskich.  

## <a name="approach-2-sqlite"></a>Podejście 2: SQLite

EF Core testuje dostawcę programu SQL Server przede wszystkim przez uruchomienie go w przypadku lokalnego wystąpienia programu SQL Server.
Te testy uruchamiają dziesiątki tysięcy zapytań w ciągu kilku minut na szybkim komputerze.
To pokazuje, że przy użyciu systemu prawdziwej bazy danych może być rozwiązaniem wydajne.
Jest to mit, że przy użyciu niektórych lżejszych bazy danych jest jedynym sposobem, aby szybko uruchomić testy.

Mając na to na myśli, co zrobić, jeśli z jakiegokolwiek powodu nie można uruchomić testy przeciwko coś blisko systemu bazy danych produkcji?
Następnym najlepszym wyborem jest użycie czegoś o podobnej funkcjonalności.
Zwykle oznacza to inną relacyjną bazę danych, dla której [SQLite](https://sqlite.org/index.html) jest oczywistym wyborem.

SQLite to dobry wybór, ponieważ:
* Działa w trakcie z aplikacją i tak ma niskie obciążenie.
* Używa prostych, automatycznie utworzonych plików dla baz danych, a więc nie wymaga zarządzania bazami danych.
* Ma tryb w pamięci, który pozwala uniknąć nawet tworzenia plików.

Należy jednak pamiętać, że:
* SqLite inevitability nie obsługuje wszystko, co system produkcyjnej bazy danych nie.
* SQLite będzie zachowywać się inaczej niż system produkcyjnej bazy danych dla niektórych zapytań.

Więc jeśli używasz SQLite do niektórych testów, upewnij się również, aby przetestować przeciwko rzeczywistemu systemowi bazy danych.

Zobacz [Testowanie z SQLite](xref:core/miscellaneous/testing/sqlite) dla EF Core wskazówki dotyczące. 

## <a name="approach-3-the-ef-core-in-memory-database"></a>Podejście 3: Baza danych EF Core w pamięci

EF Core jest wyposażony w bazę danych w pamięci, której używamy do wewnętrznych testów samego ef core.
Ta baza danych na ogół **nie nadaje się jako substytut dla aplikacji testujących, które używają EF Core**. Są to:
* Nie jest to relacyjna baza danych
* Nie obsługuje transakcji
* Nie jest zoptymalizowany pod kątem wydajności

Nic z tego nie jest bardzo ważne podczas testowania ef core wewnętrznych, ponieważ używamy go w szczególności, gdzie baza danych jest bez znaczenia dla testu.
Z drugiej strony te rzeczy wydają się być bardzo ważne podczas testowania aplikacji, która używa EF Core.

## <a name="unit-testing"></a>Testowanie jednostek

Należy wziąć pod uwagę testowanie fragmentu logiki biznesowej, który może być konieczne użycie niektórych danych z bazy danych, ale nie jest z natury testowania interakcji bazy danych.
Jedną z opcji jest użycie [podwójnego testu,](https://en.wikipedia.org/wiki/Test_double) takiego jak makieta lub podróbka.

Używamy podwaja testu do wewnętrznych testów EF Core.
Jednak nigdy nie staramy się wyśmiewać DbContext lub IQueryable.
Jest to trudne, uciążliwe i kruche.
**Nie rób tego.**

Zamiast tego używamy bazy danych w pamięci podczas testowania jednostki coś, co używa DbContext.
W takim przypadku przy użyciu bazy danych w pamięci jest właściwe, ponieważ test nie jest zależny od zachowania bazy danych.
Po prostu nie rób tego, aby przetestować rzeczywiste zapytania bazy danych lub aktualizacje.   

Zobacz [Testowanie z dostawcą w pamięci](xref:core/miscellaneous/testing/in-memory) ef core wskazówki dotyczące korzystania z bazy danych w pamięci do testowania jednostek.
