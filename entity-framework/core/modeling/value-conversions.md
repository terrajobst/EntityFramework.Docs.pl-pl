---
title: Konwersje wartości — EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 93774bc1bc3887f982faeac151825a6643c1107c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654782"
---
# <a name="value-conversions"></a><span data-ttu-id="aade8-102">Konwersje wartości</span><span class="sxs-lookup"><span data-stu-id="aade8-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="aade8-103">Ta funkcja jest nowa w EF Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="aade8-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="aade8-104">Konwertery wartości umożliwiają konwersję wartości właściwości podczas odczytywania z lub zapisywania do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="aade8-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="aade8-105">Ta konwersja może być z jednej wartości na inną tego samego typu (na przykład szyfrowanie ciągów) lub z wartości jednego typu do wartości innego typu (na przykład konwertowanie wartości wyliczenia na i z ciągów w bazie danych).</span><span class="sxs-lookup"><span data-stu-id="aade8-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="aade8-106">Podstawy</span><span class="sxs-lookup"><span data-stu-id="aade8-106">Fundamentals</span></span>

<span data-ttu-id="aade8-107">Konwertery wartości są określone w zakresie `ModelClrType` i `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="aade8-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="aade8-108">Typ modelu jest typem .NET właściwości w typie jednostki.</span><span class="sxs-lookup"><span data-stu-id="aade8-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="aade8-109">Typ dostawcy to typ .NET zrozumiały dla dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="aade8-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="aade8-110">Na przykład, aby zapisać wyliczenia jako ciągi w bazie danych, typ modelu jest typem wyliczenia, a typ dostawcy to `String`.</span><span class="sxs-lookup"><span data-stu-id="aade8-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="aade8-111">Te dwa typy mogą być takie same.</span><span class="sxs-lookup"><span data-stu-id="aade8-111">These two types can be the same.</span></span>

<span data-ttu-id="aade8-112">Konwersje są definiowane przy użyciu dwóch `Func` drzew wyrażeń: jeden z `ModelClrType` do `ProviderClrType`, a drugi z `ProviderClrType` do `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="aade8-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="aade8-113">Drzewa wyrażeń są używane, aby mogły być kompilowane do kodu dostępu do bazy danych w celu wydajnej konwersji.</span><span class="sxs-lookup"><span data-stu-id="aade8-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="aade8-114">W przypadku złożonych konwersji drzewo wyrażenia może być prostym wywołaniem metody, która wykonuje konwersję.</span><span class="sxs-lookup"><span data-stu-id="aade8-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="aade8-115">Konfigurowanie konwertera wartości</span><span class="sxs-lookup"><span data-stu-id="aade8-115">Configuring a value converter</span></span>

<span data-ttu-id="aade8-116">Konwersje wartości są definiowane we właściwościach w `OnModelCreating` `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="aade8-116">Value conversions are defined on properties in the `OnModelCreating` of your `DbContext`.</span></span> <span data-ttu-id="aade8-117">Rozważmy na przykład Wyliczenie i typ jednostki zdefiniowane jako:</span><span class="sxs-lookup"><span data-stu-id="aade8-117">For example, consider an enum and entity type defined as:</span></span>

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

<span data-ttu-id="aade8-118">Następnie konwersje można definiować w `OnModelCreating`, aby przechowywać wartości wyliczenia jako ciągi (na przykład "Donkey", "Mule",...) w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="aade8-118">Then conversions can be defined in `OnModelCreating` to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>

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
> <span data-ttu-id="aade8-119">Wartość `null` nigdy nie będzie przenoszona do konwertera wartości.</span><span class="sxs-lookup"><span data-stu-id="aade8-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="aade8-120">Dzięki temu implementacja konwersji jest łatwiejsza i umożliwia udostępnienie jej przez właściwości dopuszczające wartości null i niedopuszczające wartości null.</span><span class="sxs-lookup"><span data-stu-id="aade8-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="aade8-121">Klasa ValueConverter</span><span class="sxs-lookup"><span data-stu-id="aade8-121">The ValueConverter class</span></span>

<span data-ttu-id="aade8-122">Wywołanie `HasConversion`, jak pokazano powyżej, utworzy wystąpienie `ValueConverter` i ustawi je we właściwości.</span><span class="sxs-lookup"><span data-stu-id="aade8-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="aade8-123">Zamiast tego można jawnie utworzyć `ValueConverter`.</span><span class="sxs-lookup"><span data-stu-id="aade8-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="aade8-124">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="aade8-124">For example:</span></span>

``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

<span data-ttu-id="aade8-125">Może to być przydatne, gdy wiele właściwości używa tej samej konwersji.</span><span class="sxs-lookup"><span data-stu-id="aade8-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="aade8-126">Obecnie nie ma możliwości określenia w jednym miejscu, że każda Właściwość danego typu musi używać tego samego konwertera wartości.</span><span class="sxs-lookup"><span data-stu-id="aade8-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="aade8-127">Ta funkcja zostanie uwzględniona w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="aade8-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="aade8-128">Wbudowane konwertery</span><span class="sxs-lookup"><span data-stu-id="aade8-128">Built-in converters</span></span>

<span data-ttu-id="aade8-129">EF Core dostarcza z zestawem wstępnie zdefiniowanych klas `ValueConverter`, które znajdują się w przestrzeni nazw `Microsoft.EntityFrameworkCore.Storage.ValueConversion`.</span><span class="sxs-lookup"><span data-stu-id="aade8-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="aade8-130">Są to:</span><span class="sxs-lookup"><span data-stu-id="aade8-130">These are:</span></span>

* <span data-ttu-id="aade8-131">`BoolToZeroOneConverter`-bool do zero i jeden</span><span class="sxs-lookup"><span data-stu-id="aade8-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="aade8-132">`BoolToStringConverter`-bool do ciągów, takich jak "Y" i "N"</span><span class="sxs-lookup"><span data-stu-id="aade8-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="aade8-133">`BoolToTwoValuesConverter`-bool do dowolnych dwóch wartości</span><span class="sxs-lookup"><span data-stu-id="aade8-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="aade8-134">`BytesToStringConverter`-bajtowa tablica do ciągu zakodowanego algorytmem Base64</span><span class="sxs-lookup"><span data-stu-id="aade8-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="aade8-135">Konwersje `CastingConverter`, które wymagają tylko rzutowania typu</span><span class="sxs-lookup"><span data-stu-id="aade8-135">`CastingConverter` - Conversions that require only a type cast</span></span>
* <span data-ttu-id="aade8-136">ciąg znaków `CharToStringConverter`-char do pojedynczego znaku</span><span class="sxs-lookup"><span data-stu-id="aade8-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="aade8-137">`DateTimeOffsetToBinaryConverter`-DateTimeOffset do zakodowanej binarnie wartości 64-bitowej</span><span class="sxs-lookup"><span data-stu-id="aade8-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="aade8-138">`DateTimeOffsetToBytesConverter`-DateTimeOffset do tablicy typu Byte</span><span class="sxs-lookup"><span data-stu-id="aade8-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="aade8-139">`DateTimeOffsetToStringConverter`-DateTimeOffset do ciągu</span><span class="sxs-lookup"><span data-stu-id="aade8-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="aade8-140">wartość `DateTimeToBinaryConverter`-DateTime do 64-bitowej, w tym DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="aade8-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="aade8-141">`DateTimeToStringConverter`-DateTime do String</span><span class="sxs-lookup"><span data-stu-id="aade8-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="aade8-142">`DateTimeToTicksConverter`-DateTime do taktów</span><span class="sxs-lookup"><span data-stu-id="aade8-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="aade8-143">`EnumToNumberConverter`-Wyliczenie do numeru bazowego</span><span class="sxs-lookup"><span data-stu-id="aade8-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="aade8-144">`EnumToStringConverter` — Wyliczenie na ciąg</span><span class="sxs-lookup"><span data-stu-id="aade8-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="aade8-145">`GuidToBytesConverter`-GUID do tablicy typu Byte</span><span class="sxs-lookup"><span data-stu-id="aade8-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="aade8-146">`GuidToStringConverter`-GUID do ciągu</span><span class="sxs-lookup"><span data-stu-id="aade8-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="aade8-147">`NumberToBytesConverter` — dowolna wartość liczbowa do tablicy typu Byte</span><span class="sxs-lookup"><span data-stu-id="aade8-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="aade8-148">`NumberToStringConverter` — dowolna wartość liczbowa do ciągu</span><span class="sxs-lookup"><span data-stu-id="aade8-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="aade8-149">`StringToBytesConverter`-String do bajtów UTF8</span><span class="sxs-lookup"><span data-stu-id="aade8-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="aade8-150">`TimeSpanToStringConverter`-TimeSpan do ciągu</span><span class="sxs-lookup"><span data-stu-id="aade8-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="aade8-151">`TimeSpanToTicksConverter`-TimeSpan do taktów</span><span class="sxs-lookup"><span data-stu-id="aade8-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="aade8-152">Zwróć uwagę, że `EnumToStringConverter` znajduje się na liście.</span><span class="sxs-lookup"><span data-stu-id="aade8-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="aade8-153">Oznacza to, że nie ma potrzeby jawnego określania konwersji, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="aade8-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="aade8-154">Zamiast tego wystarczy użyć wbudowanego konwertera:</span><span class="sxs-lookup"><span data-stu-id="aade8-154">Instead, just use the built-in converter:</span></span>

``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

<span data-ttu-id="aade8-155">Należy zauważyć, że wszystkie wbudowane konwertery są bezstanowe i dlatego pojedyncze wystąpienie może być bezpiecznie udostępnione przez wiele właściwości.</span><span class="sxs-lookup"><span data-stu-id="aade8-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="aade8-156">Wstępnie zdefiniowane konwersje</span><span class="sxs-lookup"><span data-stu-id="aade8-156">Pre-defined conversions</span></span>

<span data-ttu-id="aade8-157">W przypadku typowych konwersji, dla których istnieje wbudowany konwerter, nie ma potrzeby jawnego określania konwertera.</span><span class="sxs-lookup"><span data-stu-id="aade8-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="aade8-158">Zamiast tego wystarczy skonfigurować typ dostawcy, który ma być używany, a EF będzie automatycznie używać odpowiedniego konwertera wbudowanego.</span><span class="sxs-lookup"><span data-stu-id="aade8-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate built-in converter.</span></span> <span data-ttu-id="aade8-159">Wyliczenia do konwersji ciągów są używane w powyższym przykładzie, ale Dr w rzeczywistości automatycznie, jeśli typ dostawcy jest skonfigurowany:</span><span class="sxs-lookup"><span data-stu-id="aade8-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>

``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

<span data-ttu-id="aade8-160">Tę samą wartość można osiągnąć przez jawne określenie typu kolumny.</span><span class="sxs-lookup"><span data-stu-id="aade8-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="aade8-161">Na przykład, jeśli typ jednostki jest zdefiniowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="aade8-161">For example, if the entity type is defined like so:</span></span>

``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

<span data-ttu-id="aade8-162">Wartości wyliczeniowe zostaną zapisane jako ciągi w bazie danych bez żadnej dalszej konfiguracji w `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="aade8-162">Then the enum values will be saved as strings in the database without any further configuration in `OnModelCreating`.</span></span>

## <a name="limitations"></a><span data-ttu-id="aade8-163">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="aade8-163">Limitations</span></span>

<span data-ttu-id="aade8-164">Istnieje kilka znanych bieżących ograniczeń systemu konwersji wartości:</span><span class="sxs-lookup"><span data-stu-id="aade8-164">There are a few known current limitations of the value conversion system:</span></span>

* <span data-ttu-id="aade8-165">Jak wspomniano powyżej, nie można przekonwertować `null`.</span><span class="sxs-lookup"><span data-stu-id="aade8-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="aade8-166">Obecnie nie ma możliwości rozłożenia konwersji jednej właściwości na wiele kolumn lub na odwrót.</span><span class="sxs-lookup"><span data-stu-id="aade8-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="aade8-167">Użycie konwersji wartości może mieć wpływ na możliwość EF Core przetłumaczania wyrażeń na SQL.</span><span class="sxs-lookup"><span data-stu-id="aade8-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="aade8-168">W takich przypadkach zostanie zarejestrowane ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="aade8-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="aade8-169">Usunięcie tych ograniczeń jest brane pod uwagę w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="aade8-169">Removal of these limitations is being considered for a future release.</span></span>
