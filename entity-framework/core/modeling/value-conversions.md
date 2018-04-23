---
title: Konwersje wartości - EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 329d2757059462468ca30772d37789343c03ba7b
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
# <a name="value-conversions"></a><span data-ttu-id="bfc08-102">Konwersje wartości</span><span class="sxs-lookup"><span data-stu-id="bfc08-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="bfc08-103">Ta funkcja jest nowa w programie EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="bfc08-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="bfc08-104">Konwertery wartości Zezwalaj wartości właściwości, które ma zostać przekonwertowane, gdy odczyt lub zapis do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bfc08-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="bfc08-105">Ta konwersja może być z jedną wartość na inny tego samego typu (na przykład ciągi szyfrowania) lub wartość jednego typu wartości innego typu (na przykład konwertowanie wartości wyliczenia do i z ciągów w bazie danych.)</span><span class="sxs-lookup"><span data-stu-id="bfc08-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="bfc08-106">Podstawy</span><span class="sxs-lookup"><span data-stu-id="bfc08-106">Fundamentals</span></span>

<span data-ttu-id="bfc08-107">Konwertery wartości są określone w zakresie `ModelClrType` i `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="bfc08-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="bfc08-108">Typ modelu jest typ architektury .NET właściwości w typ jednostki.</span><span class="sxs-lookup"><span data-stu-id="bfc08-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="bfc08-109">Typ dostawcy jest typ architektury .NET rozpoznawany przez dostawcę bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bfc08-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="bfc08-110">Na przykład aby zapisać wyliczenia jako ciągi w bazie danych, typu modelu jest typ wyliczenia i typ dostawcy jest `String`.</span><span class="sxs-lookup"><span data-stu-id="bfc08-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="bfc08-111">Tych dwóch typów może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="bfc08-111">These two types can be the same.</span></span>

<span data-ttu-id="bfc08-112">Konwersje są definiowane za pomocą dwóch `Func` drzew wyrażeń: jeden z `ModelClrType` do `ProviderClrType` , a druga z `ProviderClrType` do `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="bfc08-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="bfc08-113">Drzewa wyrażeń są używane, dzięki czemu mogą być kompilowane do kodu dostępu do bazy danych dla konwersji wydajne.</span><span class="sxs-lookup"><span data-stu-id="bfc08-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="bfc08-114">Podczas konwersji złożonych drzewa wyrażenia może być prosty wywołanie do metody, która wykonuje konwersję.</span><span class="sxs-lookup"><span data-stu-id="bfc08-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="bfc08-115">Konfigurowanie konwerter wartości</span><span class="sxs-lookup"><span data-stu-id="bfc08-115">Configuring a value converter</span></span>

<span data-ttu-id="bfc08-116">Konwersje wartości są definiowane na właściwości w OnModelCreating elementu z typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="bfc08-116">Value conversions are defined on properties in the OnModelCreating of your DbContext.</span></span> <span data-ttu-id="bfc08-117">Rozważmy na przykład typ wyliczenia i jednostek zdefiniowany jako:</span><span class="sxs-lookup"><span data-stu-id="bfc08-117">For example, consider an enum and entity type defined as:</span></span>
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
<span data-ttu-id="bfc08-118">Następnie można zdefiniować konwersje w OnModelCreating do przechowywania wartości wyliczenia jako ciągi (np. "Osłów", "Muł",...) w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="bfc08-118">Then conversions can be defined in OnModelCreating to store the enum values as strings (e.g. "Donkey", "Mule", ...) in the database:</span></span>
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
> <span data-ttu-id="bfc08-119">A `null` wartość nigdy nie będą przekazywane do konwertera wartości.</span><span class="sxs-lookup"><span data-stu-id="bfc08-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="bfc08-120">To ułatwia wykonania konwersji i umożliwia ich udostępnianie między właściwości wartości null i nie dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="bfc08-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="bfc08-121">Klasa ValueConverter</span><span class="sxs-lookup"><span data-stu-id="bfc08-121">The ValueConverter class</span></span>

<span data-ttu-id="bfc08-122">Wywoływanie `HasConversion` zgodnie z powyższym utworzy `ValueConverter` wystąpienia i ustawiać właściwości.</span><span class="sxs-lookup"><span data-stu-id="bfc08-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="bfc08-123">`ValueConverter` Zamiast tego można jawnie utworzyć.</span><span class="sxs-lookup"><span data-stu-id="bfc08-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="bfc08-124">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="bfc08-124">For example:</span></span>
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="bfc08-125">Może to być przydatne, gdy wielu właściwości Użyj tego samego konwersji.</span><span class="sxs-lookup"><span data-stu-id="bfc08-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="bfc08-126">Nie istnieje obecnie sposób określić w jednym miejscu, że dla każdej właściwości danego typu muszą używać tego samego konwerter wartości.</span><span class="sxs-lookup"><span data-stu-id="bfc08-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="bfc08-127">Ta funkcja zostanie uwzględniony w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="bfc08-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="bfc08-128">Konwertery wbudowane</span><span class="sxs-lookup"><span data-stu-id="bfc08-128">Built-in converters</span></span>

<span data-ttu-id="bfc08-129">Wstępnie zdefiniowana Core EF jest dostarczany z zestawem `ValueConverter` znalezione w klasy `Microsoft.EntityFrameworkCore.Storage.Converters` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="bfc08-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.Converters` namespace.</span></span> <span data-ttu-id="bfc08-130">Są to:</span><span class="sxs-lookup"><span data-stu-id="bfc08-130">These are:</span></span>
* <span data-ttu-id="bfc08-131">`BoolToZeroOneConverter` — Bool do zera do jednego</span><span class="sxs-lookup"><span data-stu-id="bfc08-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="bfc08-132">`BoolToStringConverter` — Bool do ciągów, takich jak "Y" i "N"</span><span class="sxs-lookup"><span data-stu-id="bfc08-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="bfc08-133">`BoolToTwoValuesConverter` — Bool do każdej dwóch wartości</span><span class="sxs-lookup"><span data-stu-id="bfc08-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="bfc08-134">`BytesToStringConverter` -Tablica bajtowych na ciąg kodowany w formacie Base64</span><span class="sxs-lookup"><span data-stu-id="bfc08-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="bfc08-135">`CastingConverter` -Konwersje, które wymagają tylko rzutowanie Csharp</span><span class="sxs-lookup"><span data-stu-id="bfc08-135">`CastingConverter` - Conversions that require only a Csharp cast</span></span>
* <span data-ttu-id="bfc08-136">`CharToStringConverter` -Char na ciąg pojedynczy znak</span><span class="sxs-lookup"><span data-stu-id="bfc08-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="bfc08-137">`DateTimeOffsetToBinaryConverter` — DateTimeOffset wartość binarnie 64-bitowych</span><span class="sxs-lookup"><span data-stu-id="bfc08-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="bfc08-138">`DateTimeOffsetToBytesConverter` — DateTimeOffset do tablicy typu byte</span><span class="sxs-lookup"><span data-stu-id="bfc08-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="bfc08-139">`DateTimeOffsetToStringConverter` — DateTimeOffset ciąg</span><span class="sxs-lookup"><span data-stu-id="bfc08-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="bfc08-140">`DateTimeToBinaryConverter` — DateTime tym DateTimeKind wartość 64-bitowych</span><span class="sxs-lookup"><span data-stu-id="bfc08-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="bfc08-141">`DateTimeToStringConverter` — DateTime ciąg</span><span class="sxs-lookup"><span data-stu-id="bfc08-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="bfc08-142">`DateTimeToTicksConverter` — DateTime aby znaczniki osi</span><span class="sxs-lookup"><span data-stu-id="bfc08-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="bfc08-143">`EnumToNumberConverter` — Enum numer podstawowej</span><span class="sxs-lookup"><span data-stu-id="bfc08-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="bfc08-144">`EnumToStringConverter` — Enum ciąg</span><span class="sxs-lookup"><span data-stu-id="bfc08-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="bfc08-145">`GuidToBytesConverter` — Identyfikator Guid do tablicy typu byte</span><span class="sxs-lookup"><span data-stu-id="bfc08-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="bfc08-146">`GuidToStringConverter` — Identyfikator Guid na ciąg</span><span class="sxs-lookup"><span data-stu-id="bfc08-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="bfc08-147">`NumberToBytesConverter` -Dowolne wartości liczbowe do tablicy typu byte</span><span class="sxs-lookup"><span data-stu-id="bfc08-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="bfc08-148">`NumberToStringConverter` -Wszelkie wartości liczbowej na ciąg</span><span class="sxs-lookup"><span data-stu-id="bfc08-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="bfc08-149">`StringToBytesConverter` -Ciągu UTF8 bajtów</span><span class="sxs-lookup"><span data-stu-id="bfc08-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="bfc08-150">`TimeSpanToStringConverter` — TimeSpan ciąg</span><span class="sxs-lookup"><span data-stu-id="bfc08-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="bfc08-151">`TimeSpanToTicksConverter` -TimeSpan znaczników</span><span class="sxs-lookup"><span data-stu-id="bfc08-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="bfc08-152">Zwróć uwagę, że `EnumToStringConverter` znajduje się na tej liście.</span><span class="sxs-lookup"><span data-stu-id="bfc08-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="bfc08-153">Oznacza to, że nie istnieje potrzeba określone jawnie, jak pokazano powyżej konwersji.</span><span class="sxs-lookup"><span data-stu-id="bfc08-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="bfc08-154">Zamiast tego można użyć konwertera wbudowany:</span><span class="sxs-lookup"><span data-stu-id="bfc08-154">Instead, just use the built-in converter:</span></span>
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="bfc08-155">Zauważ, że bezstanowe są wbudowane konwertery i dlatego pojedyncze wystąpienie może być bezpiecznie współużytkowane przez wiele właściwości.</span><span class="sxs-lookup"><span data-stu-id="bfc08-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="bfc08-156">Konwersje wstępnie zdefiniowane</span><span class="sxs-lookup"><span data-stu-id="bfc08-156">Pre-defined conversions</span></span>

<span data-ttu-id="bfc08-157">W przypadku typowych konwersji dla których istnieje konwerter wbudowanych jest niepotrzebna, aby jawnie określić konwerter.</span><span class="sxs-lookup"><span data-stu-id="bfc08-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="bfc08-158">Zamiast tego po prostu skonfigurować typ dostawcy należy używać i EF będą automatycznie używać odpowiedniego konwertera kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bfc08-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate build-in converter.</span></span> <span data-ttu-id="bfc08-159">Wyliczenia do konwersji ciągów są używane jako przykład powyżej, ale EF faktycznie wykona to automatyczne skonfigurowanie typu dostawcy:</span><span class="sxs-lookup"><span data-stu-id="bfc08-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="bfc08-160">Ten sam efekt można osiągnąć, jawnie określając typ kolumny.</span><span class="sxs-lookup"><span data-stu-id="bfc08-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="bfc08-161">Na przykład, jeśli zdefiniowano typ jednostki, takich jak tak:</span><span class="sxs-lookup"><span data-stu-id="bfc08-161">For example, if the entity type is defined like so:</span></span>
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="bfc08-162">Następnie wartości wyliczenia są zapisywane jako ciągi w bazie danych bez dalszej konfiguracji w OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="bfc08-162">Then the enum values will be saved as strings in the database without any further configuration in OnModelCreating.</span></span>

## <a name="limitations"></a><span data-ttu-id="bfc08-163">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="bfc08-163">Limitations</span></span>

<span data-ttu-id="bfc08-164">Istnieje kilka znane ograniczenia bieżącego systemu umożliwić konwersję wartości:</span><span class="sxs-lookup"><span data-stu-id="bfc08-164">There are a few known current limitations of the value convertion system:</span></span>
* <span data-ttu-id="bfc08-165">Jak wspomniano powyżej, `null` nie może zostać przekonwertowany.</span><span class="sxs-lookup"><span data-stu-id="bfc08-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="bfc08-166">Nie istnieje obecnie sposób się konwersji właściwości jeden do wielu kolumn lub na odwrót.</span><span class="sxs-lookup"><span data-stu-id="bfc08-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="bfc08-167">Użyj konwersji wartości mogą mieć wpływ na możliwość EF Core translacji wyrażenia do bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="bfc08-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="bfc08-168">Ostrzeżenie jest rejestrowane w takich przypadkach.</span><span class="sxs-lookup"><span data-stu-id="bfc08-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="bfc08-169">Usunięcie tych ograniczeń jest rozważane w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="bfc08-169">Removal of these limitations is being considered for a future release.</span></span>
