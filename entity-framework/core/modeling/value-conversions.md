---
title: Konwersje wartości - EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 3e97c05a87ad9b4817c03f446031ea6c74704f5b
ms.sourcegitcommit: 605e42232854ce44bae09624a6eebc35b8e2473b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/16/2018
---
# <a name="value-conversions"></a>Konwersje wartości

> [!NOTE]  
> Ta funkcja jest nowa w programie EF Core 2.1.

Konwertery wartości Zezwalaj wartości właściwości, które ma zostać przekonwertowane, gdy odczyt lub zapis do bazy danych. Ta konwersja może być z jedną wartość na inny tego samego typu (na przykład ciągi szyfrowania) lub wartość jednego typu wartości innego typu (na przykład konwertowanie wartości wyliczenia do i z ciągów w bazie danych.)

## <a name="fundamentals"></a>Podstawy

Konwertery wartości są określone w zakresie `ModelClrType` i `ProviderClrType`. Typ modelu jest typ architektury .NET właściwości w typ jednostki. Typ dostawcy jest typ architektury .NET rozpoznawany przez dostawcę bazy danych. Na przykład aby zapisać wyliczenia jako ciągi w bazie danych, typu modelu jest typ wyliczenia i typ dostawcy jest `String`. Tych dwóch typów może być taka sama.

Konwersje są definiowane za pomocą dwóch `Func` drzew wyrażeń: jeden z `ModelClrType` do `ProviderClrType` , a druga z `ProviderClrType` do `ModelClrType`. Drzewa wyrażeń są używane, dzięki czemu mogą być kompilowane do kodu dostępu do bazy danych dla konwersji wydajne. Podczas konwersji złożonych drzewa wyrażenia może być prosty wywołanie do metody, która wykonuje konwersję.

## <a name="configuring-a-value-converter"></a>Konfigurowanie konwerter wartości

Konwersje wartości są definiowane na właściwości w OnModelCreating elementu z typu DbContext. Rozważmy na przykład typ wyliczenia i jednostek zdefiniowany jako:
```Csharp
public class Rider
{
    public int Id { get; set; }
    public EquineBeast Mount { get; set; }
}

public enum EquineBeast
{
    Donkey,
    Mule,
    Horse,
    Unicorn
}
```
Następnie można zdefiniować konwersje w OnModelCreating do przechowywania wartości wyliczenia jako ciągi (np. "Osłów", "Muł",...) w bazie danych:
```Csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Rider>()
        .Property(e => e.Mount)
        .HasConversion(
            v => v.ToString(),
            v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));
}
```
> [!NOTE]  
> A `null` wartość nigdy nie będą przekazywane do konwertera wartości. To ułatwia wykonania konwersji i umożliwia ich udostępnianie między właściwości wartości null i nie dopuszcza wartości null.

## <a name="the-valueconverter-class"></a>Klasa ValueConverter

Wywoływanie `HasConversion` zgodnie z powyższym utworzy `ValueConverter` wystąpienia i ustawiać właściwości. `ValueConverter` Zamiast tego można jawnie utworzyć. Na przykład:
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Może to być przydatne, gdy wielu właściwości Użyj tego samego konwersji.

> [!NOTE]  
> Nie istnieje obecnie sposób określić w jednym miejscu, że dla każdej właściwości danego typu muszą używać tego samego konwerter wartości. Ta funkcja zostanie uwzględniony w przyszłości.

## <a name="built-in-converters"></a>Konwertery wbudowane

Wstępnie zdefiniowana Core EF jest dostarczany z zestawem `ValueConverter` znalezione w klasy `Microsoft.EntityFrameworkCore.Storage.ValueConversion` przestrzeni nazw. Są to:
* `BoolToZeroOneConverter` — Bool do zera do jednego
* `BoolToStringConverter` — Bool do ciągów, takich jak "Y" i "N"
* `BoolToTwoValuesConverter` — Bool do każdej dwóch wartości
* `BytesToStringConverter` -Tablica bajtowych na ciąg kodowany w formacie Base64
* `CastingConverter` -Konwersje, które wymagają tylko rzutowanie Csharp
* `CharToStringConverter` -Char na ciąg pojedynczy znak
* `DateTimeOffsetToBinaryConverter` — DateTimeOffset wartość binarnie 64-bitowych
* `DateTimeOffsetToBytesConverter` — DateTimeOffset do tablicy typu byte
* `DateTimeOffsetToStringConverter` — DateTimeOffset ciąg
* `DateTimeToBinaryConverter` — DateTime tym DateTimeKind wartość 64-bitowych
* `DateTimeToStringConverter` — DateTime ciąg
* `DateTimeToTicksConverter` — DateTime aby znaczniki osi
* `EnumToNumberConverter` — Enum numer podstawowej
* `EnumToStringConverter` — Enum ciąg
* `GuidToBytesConverter` — Identyfikator Guid do tablicy typu byte
* `GuidToStringConverter` — Identyfikator Guid na ciąg
* `NumberToBytesConverter` -Dowolne wartości liczbowe do tablicy typu byte
* `NumberToStringConverter` -Wszelkie wartości liczbowej na ciąg
* `StringToBytesConverter` -Ciągu UTF8 bajtów
* `TimeSpanToStringConverter` — TimeSpan ciąg
* `TimeSpanToTicksConverter` -TimeSpan znaczników

Zwróć uwagę, że `EnumToStringConverter` znajduje się na tej liście. Oznacza to, że nie istnieje potrzeba określone jawnie, jak pokazano powyżej konwersji. Zamiast tego można użyć konwertera wbudowany:
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Zauważ, że bezstanowe są wbudowane konwertery i dlatego pojedyncze wystąpienie może być bezpiecznie współużytkowane przez wiele właściwości.

## <a name="pre-defined-conversions"></a>Konwersje wstępnie zdefiniowane

W przypadku typowych konwersji dla których istnieje konwerter wbudowanych jest niepotrzebna, aby jawnie określić konwerter. Zamiast tego po prostu skonfigurować typ dostawcy należy używać i EF będą automatycznie używać odpowiedniego konwertera kompilacji. Wyliczenia do konwersji ciągów są używane jako przykład powyżej, ale EF faktycznie wykona to automatyczne skonfigurowanie typu dostawcy:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
Ten sam efekt można osiągnąć, jawnie określając typ kolumny. Na przykład, jeśli zdefiniowano typ jednostki, takich jak tak:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Następnie wartości wyliczenia są zapisywane jako ciągi w bazie danych bez dalszej konfiguracji w OnModelCreating.

## <a name="limitations"></a>Ograniczenia

Istnieje kilka znane ograniczenia bieżącego systemu umożliwić konwersję wartości:
* Jak wspomniano powyżej, `null` nie może zostać przekonwertowany.
* Nie istnieje obecnie sposób się konwersji właściwości jeden do wielu kolumn lub na odwrót.
* Użyj konwersji wartości mogą mieć wpływ na możliwość EF Core translacji wyrażenia do bazy danych SQL. Ostrzeżenie jest rejestrowane w takich przypadkach.
Usunięcie tych ograniczeń jest rozważane w przyszłości.
