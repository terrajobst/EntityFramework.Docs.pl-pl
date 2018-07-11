---
title: Konwersje wartości - programu EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 5bfb6111ac450db91f3f1a7074a924a1c8400ce7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949095"
---
# <a name="value-conversions"></a>Konwersje wartości

> [!NOTE]  
> Ta funkcja jest nowa w programie EF Core 2.1.

Konwertery wartości Zezwalaj na wartości właściwości, które ma zostać przekonwertowany, podczas odczytu / zapisu do bazy danych. Ta konwersja może być z jedną wartość na inny tego samego typu (na przykład ciągi, szyfrowanie) lub wartość jednego typu z wartością innego typu (na przykład konwertowania wartości wyliczenia do i z ciągów w bazie danych.)

## <a name="fundamentals"></a>Podstawy

Konwertery wartości są określane na podstawie `ModelClrType` i `ProviderClrType`. Typ modelu jest typ architektury .NET właściwości typu jednostki. Typ dostawcy jest typ architektury .NET zrozumiała dla dostawcy bazy danych. Na przykład, aby zapisać wyliczenia jako ciągi w bazie danych, typ modelu jest typem wyliczenia, a typ dostawcy jest `String`. Te dwa typy może być taki sam.

Konwersje są definiowane za pomocą dwóch `Func` drzew wyrażeń: jeden z `ModelClrType` do `ProviderClrType` , a druga z `ProviderClrType` do `ModelClrType`. Drzewa wyrażeń są używane i może być kompilowane do kodu dostępu do bazy danych podczas konwersji wydajne. W przypadku złożonych konwersji drzewa wyrażeń może być proste wywołanie metody, która wykonuje konwersję.

## <a name="configuring-a-value-converter"></a>Konfigurowanie konwertera wartości

Konwersje wartości są definiowane na właściwości w OnModelCreating z Twojego DbContext. Na przykład należy wziąć pod uwagę typem wyliczenia i jednostek zdefiniowany jako:
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
Następnie można zdefiniować konwersje w OnModelCreating do przechowywania wartości wyliczenia jako ciągi (na przykład "Osłów", "Muł",...) w bazie danych:
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
> A `null` wartość nigdy nie zostaną przekazane do konwertera wartości. To ułatwia wykonanie konwersji i umożliwia im być współużytkowany przez właściwości dopuszcza wartości null i nie dopuszcza wartości null.

## <a name="the-valueconverter-class"></a>Klasa elementu ValueConverter

Wywoływanie `HasConversion` jak wspomniano powyżej, spowoduje to utworzenie `ValueConverter` wystąpienia i ustaw jego właściwości. `ValueConverter` Zamiast tego mogą być tworzone w sposób jawny. Na przykład:
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Może to być przydatne, gdy wiele właściwości, użyj tej samej konwersji.

> [!NOTE]  
> Obecnie nie ma możliwości można określić w jednym miejscu, że dla każdej właściwości danego typu musi używać tego samego konwertera wartości. Ta funkcja zostanie uwzględniony w przyszłej wersji.

## <a name="built-in-converters"></a>Wbudowane konwertery

EF Core dostarczany z zestawem wstępnie zdefiniowane `ValueConverter` znalezione w klasy `Microsoft.EntityFrameworkCore.Storage.ValueConversion` przestrzeni nazw. Są to:
* `BoolToZeroOneConverter` — Bool na zero i jeden
* `BoolToStringConverter` — Bool do ciągów, takich jak "Y" i "N"
* `BoolToTwoValuesConverter` — Bool do dowolnych dwóch wartości.
* `BytesToStringConverter` -Tablica bajtów na ciąg kodowany w formacie Base64
* `CastingConverter` -Konwersje, które wymagają rzutowania języka Csharp
* `CharToStringConverter` -Char ciąg pojedynczy znak
* `DateTimeOffsetToBinaryConverter` — DateTimeOffset na wartość 64-bitowych zakodowane w formacie pliku binarnego
* `DateTimeOffsetToBytesConverter` — DateTimeOffset do tablicy typu byte
* `DateTimeOffsetToStringConverter` — DateTimeOffset ciąg
* `DateTimeToBinaryConverter` — Data i godzina wartości 64-bitowe, w tym DateTimeKind
* `DateTimeToStringConverter` — Data i godzina ciąg
* `DateTimeToTicksConverter` -Data i godzina impulsów
* `EnumToNumberConverter` -Wyliczenie numer podstawowy
* `EnumToStringConverter` — Enum ciąg
* `GuidToBytesConverter` — Identyfikator Guid do tablicy typu byte
* `GuidToStringConverter` — Identyfikator Guid ciąg
* `NumberToBytesConverter` -Dowolna wartość liczbowa do tablicy typu byte
* `NumberToStringConverter` -Dowolna wartość liczbową, ciąg
* `StringToBytesConverter` -Ciągu UTF8 bajtów
* `TimeSpanToStringConverter` — TimeSpan ciąg
* `TimeSpanToTicksConverter` -TimeSpan impulsów

Należy zauważyć, że `EnumToStringConverter` znajduje się na tej liście. Oznacza to, że nie ma potrzeby określić konwersji jawnie, jak pokazano powyżej. Zamiast tego można użyć konwertera wbudowane:
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Należy pamiętać, że wbudowane konwertery są bezstanowe, a więc pojedyncze wystąpienie może być bezpiecznie współużytkowane przez wiele właściwości.

## <a name="pre-defined-conversions"></a>Wstępnie zdefiniowane konwersje

W przypadku typowych konwersji dla których istnieje wbudowany konwerter nie ma potrzeby jawnie określić konwertera. Zamiast tego po prostu skonfigurować należy używać typu dostawcy i platforma EF automatycznie użyje odpowiedniego konwertera kompilacji. Wyliczenie w celu konwersji ciągów są używane jako przykład powyżej, ale EF faktycznie wykona to automatycznie, jeśli skonfigurowano typ dostawcy:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
Ten sam efekt można osiągnąć przez jawne określenie typu kolumny. Na przykład, jeśli nie zdefiniowano typu jednostki, takie jak tak:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Następnie wartości wyliczenia zostaną zapisane jako ciągi w bazie danych bez dalszej konfiguracji w OnModelCreating.

## <a name="limitations"></a>Ograniczenia

Istnieje kilka znanych bieżących ograniczeń systemu konwersji wartości:
* Jak wspomniano powyżej, `null` nie może zostać przekonwertowany.
* Obecnie nie ma możliwości się konwersję z jedną właściwość wiele kolumn lub na odwrót.
* Korzystanie z konwersji wartości mogą mieć wpływ na możliwość translacji wyrażeń do bazy danych SQL platformy EF Core. W takich przypadkach zostanie zarejestrowane ostrzeżenie.
Usunięcie tych ograniczeń jest rozważane w przyszłej wersji.
