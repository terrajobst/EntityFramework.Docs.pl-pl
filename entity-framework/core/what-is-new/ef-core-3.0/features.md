---
title: Nowe funkcje w EF Core 3,0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 528733d6eec33de2c9538541a6ed5be704b9d433
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005556"
---
# <a name="new-features-included-in-ef-core-30"></a>Nowe funkcje zawarte w EF Core 3,0

Poniższa lista zawiera najważniejsze nowe funkcje planowane dla EF Core 3,0.

EF Core 3,0 to główna wersja i zawiera także liczne istotne [zmiany](xref:core/what-is-new/ef-core-3.0/breaking-changes), które są ULEPSZENIAMI interfejsu API, które mogą mieć negatywny wpływ na istniejące aplikacje.  

## <a name="linq-improvements"></a>Ulepszenia LINQ 

LINQ pozwala pisać zapytania bazy danych bez opuszczania wybranego języka, korzystając z informacji o typie rozbudowanym do oferowania funkcji IntelliSense i sprawdzania typu w czasie kompilacji.
Jednak LINQ pozwala także pisać nieograniczoną liczbę skomplikowanych kwerend zawierających dowolne wyrażenia (wywołania metod lub operacje).
Obsługa wszystkich tych kombinacji zawsze było znaczącym wyzwaniem dla dostawców LINQ.
W EF Core 3,0 wprowadziliśmy nasze implementację LINQ, aby umożliwić tłumaczenie większej liczby wyrażeń na SQL, generowanie wydajnych zapytań w większej liczbie przypadków, aby zapobiec wykryciu niewydajnych zapytań i ułatwić stopniowe wprowadzanie nowych zapytań możliwości i wydajność improvementswithout istniejące aplikacje i dostawcy danych.

### <a name="client-evaluation"></a>Ocena klienta

Główna zmiana projektu w EF Core 3,0 musi zrobić, w jaki sposób obsługuje wyrażenia LINQ, których nie można przetłumaczyć na SQL lub parametry:

W kilku pierwszych wersjach EF Core po prostu ustalić, jakie fragmenty zapytania można przetłumaczyć na SQL i wykonać pozostałe zapytanie na kliencie.
Ten typ wykonywania po stronie klienta może być pożądany w niektórych sytuacjach, ale w wielu innych przypadkach może to spowodować niewydajne zapytania.
Na przykład jeśli EF Core 2,2 nie może przetłumaczyć predykatu `Where()` w wywołaniu, wykonał instrukcję SQL bez filtru, Odczytaj wszystkie wiersze z bazy danych, a następnie przefiltruje je w pamięci.
To może być akceptowalne, jeśli baza danych zawiera niewielką liczbę wierszy, ale może powodować znaczne problemy z wydajnością lub nawet niepowodzenie aplikacji, jeśli baza danych zawiera dużą liczbę lub kilka wierszy.
W EF Core 3,0 ograniczamy ocenę klienta tylko w przypadku projekcji najwyższego poziomu (ostatnie wywołanie do `Select()`).
Gdy EF Core 3,0 wykrywa wyrażenia, których nie można przetłumaczyć w innym miejscu w zapytaniu, zgłasza wyjątek czasu wykonania.

## <a name="cosmos-db-support"></a>Obsługa Cosmos DB 

Dostawca Cosmos DB dla EF Core umożliwia deweloperom znającym model programu EF programowanie, aby łatwo kierować Azure Cosmos DB jako bazę danych aplikacji.
Celem jest, aby niektóre zalety Cosmos DB, takich jak dystrybucja globalna, "zawsze włączone" dostępność, elastyczna skalowalność i małe opóźnienia, jeszcze bardziej dostępne dla deweloperów platformy .NET.
Dostawca umożliwi korzystanie z większości funkcji EF Core, takich jak automatyczne śledzenie zmian, LINQ i konwersje wartości, względem interfejsu API SQL w programie Cosmos DB.

## <a name="c-80-support"></a>C#Obsługa 8,0

EF Core 3,0 wykorzystuje niektóre nowe funkcje w C# 8,0:

### <a name="asynchronous-streams"></a>Strumienie asynchroniczne

Asynchroniczne wyniki zapytania są teraz udostępniane przy użyciu nowego interfejsu `IAsyncEnumerable<T>` standardowego i mogą być używane przy `await foreach`użyciu programu.

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Proccess(o);
} 
```

### <a name="nullable-reference-types"></a>Typy referencyjne dopuszczające wartość null 

Gdy ta nowa funkcja jest włączona w kodzie, EF Core może przyczynić się do wartości null właściwości typów refrence (typów pierwotnych, takich jak ciąg lub właściwości nawigacji) w celu określenia wartości null kolumn i relacji w bazie danych.

## <a name="interception"></a>Przejmowanie

Nowy interfejs API przechwytywania w EF Core 3,0 umożliwia programowo przestrzeganie i modyfikowanie wyniku operacji bazy danych niskiego poziomu, które występują w ramach normalnej operacji EF Core, takich jak otwieranie połączeń, transakcje initating i wykonywanie poleceń. 

## <a name="reverse-engineering-of-database-views"></a>Odtwarzanie widoków bazy danych

Typy jednostek bez kluczy (wcześniej znanych jako [typy zapytań](xref:core/modeling/query-types)) reprezentują dane, które można odczytać z bazy danych, ale nie można ich zaktualizować.
Ta cecha sprawia, że są one doskonałą zaletą mapowania widoków bazy danych w większości scenariuszy, dzięki czemu tworzymy typy jednostek bez kluczy podczas odtwarzania widoków bazy danych.

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne

Począwszy od EF Core 3,0, jeśli `OrderDetails` jest `Order` własnością lub jawnie zamapowana na tę samą tabelę, będzie możliwe dodanie `Order` bez `OrderDetails` i wszystkich `OrderDetails` właściwości, z wyjątkiem tego, że klucz podstawowy zostanie zmapowany na kolumny dopuszczające wartość null.

Podczas wykonywania zapytania, EF Core zostanie ustawiona `OrderDetails` na `null` , Jeśli którakolwiek z wymaganych właściwości nie ma wartości lub jeśli nie ma żadnych wymaganych właściwości poza kluczem podstawowym i wszystkie właściwości są `null`.

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a>Dr 6,3 na platformie .NET Core

Firma Microsoft zdaje sobie sprawę, że w wielu istniejących aplikacjach są używane poprzednie wersje EF, a ich przenoszenie do EF Core tylko w celu wykorzystania platformy .NET Core może czasami wymagać znacznego wysiłku.
Z tego powodu została włączona wersja newewst programu EF 6 do uruchamiania na platformie .NET Core 3,0.
Istnieją pewne ograniczenia, na przykład:
- Nowi dostawcy muszą współpracować z platformą .NET Core
- Obsługa przestrzenna z SQL Server nie będzie włączona

## <a name="postponed-features"></a>Funkcje odroczone

Niektóre funkcje początkowo planowane dla EF Core 3,0 zostały odroczone do przyszłych wersji: 

- Możliwość ingore części modelu w migracjach, śledzonych przez [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).
- Jednostki zbioru właściwości, śledzone przez dwa oddzielne problemy: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) informacji o jednostkach typu udostępnionego i [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) o obsłudze mapowania właściwości indeksowanych.
