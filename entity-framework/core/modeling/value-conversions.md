---
title: Konwersje wartości — EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 93774bc1bc3887f982faeac151825a6643c1107c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417203"
---
# <a name="value-conversions"></a>Konwersje wartości

> [!NOTE]  
> Ta funkcja jest nowa na platformie EF Core 2.1.

Konwertery wartości umożliwiają konwersję wartości właściwości podczas odczytywania z lub zapisywania do bazy danych. Ta konwersja może być z jednej wartości na inną tego samego typu (na przykład szyfrowanie ciągów) lub z wartości jednego typu do wartości innego typu (na przykład konwertowanie wartości wyliczenia na i z ciągów w bazie danych).

## <a name="fundamentals"></a>Podstawy

Konwertery wartości są określone w zakresie `ModelClrType` i `ProviderClrType`. Typ modelu jest typem .NET właściwości w typie jednostki. Typ dostawcy to typ .NET zrozumiały dla dostawcy bazy danych. Na przykład, aby zapisać wyliczenia jako ciągi w bazie danych, typ modelu jest typem wyliczenia, a typ dostawcy to `String`. Te dwa typy mogą być takie same.

Konwersje są definiowane przy użyciu dwóch `Func` drzew wyrażeń: jeden z `ModelClrType` do `ProviderClrType`, a drugi z `ProviderClrType` do `ModelClrType`. Drzewa wyrażeń są używane, aby mogły być kompilowane do kodu dostępu do bazy danych w celu wydajnej konwersji. W przypadku złożonych konwersji drzewo wyrażenia może być prostym wywołaniem metody, która wykonuje konwersję.

## <a name="configuring-a-value-converter"></a>Konfigurowanie konwertera wartości

Konwersje wartości są definiowane we właściwościach w `OnModelCreating` `DbContext`. Rozważmy na przykład Wyliczenie i typ jednostki zdefiniowane jako:

``` csharp
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

Następnie konwersje można definiować w `OnModelCreating`, aby przechowywać wartości wyliczenia jako ciągi (na przykład "Donkey", "Mule",...) w bazie danych:

``` csharp
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
> Wartość `null` nigdy nie będzie przenoszona do konwertera wartości. Dzięki temu implementacja konwersji jest łatwiejsza i umożliwia udostępnienie jej przez właściwości dopuszczające wartości null i niedopuszczające wartości null.

## <a name="the-valueconverter-class"></a>Klasa ValueConverter

Wywołanie `HasConversion`, jak pokazano powyżej, utworzy wystąpienie `ValueConverter` i ustawi je we właściwości. Zamiast tego można jawnie utworzyć `ValueConverter`. Na przykład:

``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

Może to być przydatne, gdy wiele właściwości używa tej samej konwersji.

> [!NOTE]  
> Obecnie nie ma możliwości określenia w jednym miejscu, że każda Właściwość danego typu musi używać tego samego konwertera wartości. Ta funkcja zostanie uwzględniona w przyszłej wersji.

## <a name="built-in-converters"></a>Wbudowane konwertery

EF Core dostarcza z zestawem wstępnie zdefiniowanych klas `ValueConverter`, które znajdują się w przestrzeni nazw `Microsoft.EntityFrameworkCore.Storage.ValueConversion`. Są to:

* `BoolToZeroOneConverter`-bool do zero i jeden
* `BoolToStringConverter`-bool do ciągów, takich jak "Y" i "N"
* `BoolToTwoValuesConverter`-bool do dowolnych dwóch wartości
* `BytesToStringConverter`-bajtowa tablica do ciągu zakodowanego algorytmem Base64
* Konwersje `CastingConverter`, które wymagają tylko rzutowania typu
* ciąg znaków `CharToStringConverter`-char do pojedynczego znaku
* `DateTimeOffsetToBinaryConverter`-DateTimeOffset do zakodowanej binarnie wartości 64-bitowej
* `DateTimeOffsetToBytesConverter`-DateTimeOffset do tablicy typu Byte
* `DateTimeOffsetToStringConverter`-DateTimeOffset do ciągu
* wartość `DateTimeToBinaryConverter`-DateTime do 64-bitowej, w tym DateTimeKind
* `DateTimeToStringConverter`-DateTime do String
* `DateTimeToTicksConverter`-DateTime do taktów
* `EnumToNumberConverter`-Wyliczenie do numeru bazowego
* `EnumToStringConverter` — Wyliczenie na ciąg
* `GuidToBytesConverter`-GUID do tablicy typu Byte
* `GuidToStringConverter`-GUID do ciągu
* `NumberToBytesConverter` — dowolna wartość liczbowa do tablicy typu Byte
* `NumberToStringConverter` — dowolna wartość liczbowa do ciągu
* `StringToBytesConverter`-String do bajtów UTF8
* `TimeSpanToStringConverter`-TimeSpan do ciągu
* `TimeSpanToTicksConverter`-TimeSpan do taktów

Zwróć uwagę, że `EnumToStringConverter` znajduje się na liście. Oznacza to, że nie ma potrzeby jawnego określania konwersji, jak pokazano powyżej. Zamiast tego wystarczy użyć wbudowanego konwertera:

``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

Należy zauważyć, że wszystkie wbudowane konwertery są bezstanowe i dlatego pojedyncze wystąpienie może być bezpiecznie udostępnione przez wiele właściwości.

## <a name="pre-defined-conversions"></a>Wstępnie zdefiniowane konwersje

W przypadku typowych konwersji, dla których istnieje wbudowany konwerter, nie ma potrzeby jawnego określania konwertera. Zamiast tego wystarczy skonfigurować typ dostawcy, który ma być używany, a EF będzie automatycznie używać odpowiedniego konwertera wbudowanego. Wyliczenia do konwersji ciągów są używane w powyższym przykładzie, ale Dr w rzeczywistości automatycznie, jeśli typ dostawcy jest skonfigurowany:

``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

Tę samą wartość można osiągnąć przez jawne określenie typu kolumny. Na przykład, jeśli typ jednostki jest zdefiniowany w następujący sposób:

``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

Wartości wyliczeniowe zostaną zapisane jako ciągi w bazie danych bez żadnej dalszej konfiguracji w `OnModelCreating`.

## <a name="limitations"></a>Ograniczenia

Istnieje kilka znanych bieżących ograniczeń systemu konwersji wartości:

* Jak wspomniano powyżej, nie można przekonwertować `null`.
* Obecnie nie ma możliwości rozłożenia konwersji jednej właściwości na wiele kolumn lub na odwrót.
* Użycie konwersji wartości może mieć wpływ na możliwość EF Core przetłumaczania wyrażeń na SQL. W takich przypadkach zostanie zarejestrowane ostrzeżenie.
Usunięcie tych ograniczeń jest brane pod uwagę w przyszłej wersji.
