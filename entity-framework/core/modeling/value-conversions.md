---
title: Konwersje wartości - programu EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 2a1956221ecc920feba796e4d95cc97259e89c53
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152513"
---
# <a name="value-conversions"></a><span data-ttu-id="873a3-102">Konwersje wartości</span><span class="sxs-lookup"><span data-stu-id="873a3-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="873a3-103">Ta funkcja jest nowa na platformie EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="873a3-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="873a3-104">Konwertery wartości Zezwalaj na wartości właściwości, które ma zostać przekonwertowany, podczas odczytu / zapisu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="873a3-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="873a3-105">Ta konwersja może być z jedną wartość na inny tego samego typu (na przykład ciągi, szyfrowanie) lub wartość jednego typu z wartością innego typu (na przykład konwertowania wartości wyliczenia do i z ciągów w bazie danych.)</span><span class="sxs-lookup"><span data-stu-id="873a3-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="873a3-106">Podstawy</span><span class="sxs-lookup"><span data-stu-id="873a3-106">Fundamentals</span></span>

<span data-ttu-id="873a3-107">Konwertery wartości są określane na podstawie `ModelClrType` i `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="873a3-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="873a3-108">Typ modelu jest typ architektury .NET właściwości typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="873a3-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="873a3-109">Typ dostawcy jest typ architektury .NET zrozumiała dla dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="873a3-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="873a3-110">Na przykład, aby zapisać wyliczenia jako ciągi w bazie danych, typ modelu jest typem wyliczenia, a typ dostawcy jest `String`.</span><span class="sxs-lookup"><span data-stu-id="873a3-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="873a3-111">Te dwa typy może być taki sam.</span><span class="sxs-lookup"><span data-stu-id="873a3-111">These two types can be the same.</span></span>

<span data-ttu-id="873a3-112">Konwersje są definiowane za pomocą dwóch `Func` drzew wyrażeń: jeden z `ModelClrType` do `ProviderClrType` , a druga z `ProviderClrType` do `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="873a3-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="873a3-113">Drzewa wyrażeń są używane i może być kompilowane do kodu dostępu do bazy danych podczas konwersji wydajne.</span><span class="sxs-lookup"><span data-stu-id="873a3-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="873a3-114">W przypadku złożonych konwersji drzewa wyrażeń może być proste wywołanie metody, która wykonuje konwersję.</span><span class="sxs-lookup"><span data-stu-id="873a3-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="873a3-115">Konfigurowanie konwertera wartości</span><span class="sxs-lookup"><span data-stu-id="873a3-115">Configuring a value converter</span></span>

<span data-ttu-id="873a3-116">Konwersje wartości są zdefiniowane na właściwości w `OnModelCreating` z Twojego `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="873a3-116">Value conversions are defined on properties in the `OnModelCreating` of your `DbContext`.</span></span> <span data-ttu-id="873a3-117">Na przykład należy wziąć pod uwagę typem wyliczenia i jednostek zdefiniowany jako:</span><span class="sxs-lookup"><span data-stu-id="873a3-117">For example, consider an enum and entity type defined as:</span></span>
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
<span data-ttu-id="873a3-118">Następnie konwersje można zdefiniować w `OnModelCreating` do przechowywania wartości wyliczenia jako ciągi (na przykład "Osłów", "Muł",...) w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="873a3-118">Then conversions can be defined in `OnModelCreating` to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>
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
> <span data-ttu-id="873a3-119">A `null` wartość nigdy nie zostaną przekazane do konwertera wartości.</span><span class="sxs-lookup"><span data-stu-id="873a3-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="873a3-120">To ułatwia wykonanie konwersji i umożliwia im być współużytkowany przez właściwości dopuszcza wartości null i nie dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="873a3-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="873a3-121">Klasa elementu ValueConverter</span><span class="sxs-lookup"><span data-stu-id="873a3-121">The ValueConverter class</span></span>

<span data-ttu-id="873a3-122">Wywoływanie `HasConversion` jak wspomniano powyżej, spowoduje to utworzenie `ValueConverter` wystąpienia i ustaw jego właściwości.</span><span class="sxs-lookup"><span data-stu-id="873a3-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="873a3-123">`ValueConverter` Zamiast tego mogą być tworzone w sposób jawny.</span><span class="sxs-lookup"><span data-stu-id="873a3-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="873a3-124">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="873a3-124">For example:</span></span>
``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="873a3-125">Może to być przydatne, gdy wiele właściwości, użyj tej samej konwersji.</span><span class="sxs-lookup"><span data-stu-id="873a3-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="873a3-126">Obecnie nie ma możliwości można określić w jednym miejscu, że dla każdej właściwości danego typu musi używać tego samego konwertera wartości.</span><span class="sxs-lookup"><span data-stu-id="873a3-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="873a3-127">Ta funkcja zostanie uwzględniony w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="873a3-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="873a3-128">Wbudowane konwertery</span><span class="sxs-lookup"><span data-stu-id="873a3-128">Built-in converters</span></span>

<span data-ttu-id="873a3-129">EF Core dostarczany z zestawem wstępnie zdefiniowane `ValueConverter` znalezione w klasy `Microsoft.EntityFrameworkCore.Storage.ValueConversion` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="873a3-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="873a3-130">Są to:</span><span class="sxs-lookup"><span data-stu-id="873a3-130">These are:</span></span>
* <span data-ttu-id="873a3-131">`BoolToZeroOneConverter` — Bool na zero i jeden</span><span class="sxs-lookup"><span data-stu-id="873a3-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="873a3-132">`BoolToStringConverter` — Bool do ciągów, takich jak "Y" i "N"</span><span class="sxs-lookup"><span data-stu-id="873a3-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="873a3-133">`BoolToTwoValuesConverter` — Bool do dowolnych dwóch wartości.</span><span class="sxs-lookup"><span data-stu-id="873a3-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="873a3-134">`BytesToStringConverter` -Tablica bajtów na ciąg kodowany w formacie Base64</span><span class="sxs-lookup"><span data-stu-id="873a3-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="873a3-135">`CastingConverter` -Konwersje, które wymagają rzutowanie typu</span><span class="sxs-lookup"><span data-stu-id="873a3-135">`CastingConverter` - Conversions that require only a type cast</span></span>
* <span data-ttu-id="873a3-136">`CharToStringConverter` -Char ciąg pojedynczy znak</span><span class="sxs-lookup"><span data-stu-id="873a3-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="873a3-137">`DateTimeOffsetToBinaryConverter` — DateTimeOffset na wartość 64-bitowych zakodowane w formacie pliku binarnego</span><span class="sxs-lookup"><span data-stu-id="873a3-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="873a3-138">`DateTimeOffsetToBytesConverter` — DateTimeOffset do tablicy typu byte</span><span class="sxs-lookup"><span data-stu-id="873a3-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="873a3-139">`DateTimeOffsetToStringConverter` — DateTimeOffset ciąg</span><span class="sxs-lookup"><span data-stu-id="873a3-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="873a3-140">`DateTimeToBinaryConverter` — Data i godzina wartości 64-bitowe, w tym DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="873a3-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="873a3-141">`DateTimeToStringConverter` — Data i godzina ciąg</span><span class="sxs-lookup"><span data-stu-id="873a3-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="873a3-142">`DateTimeToTicksConverter` -Data i godzina impulsów</span><span class="sxs-lookup"><span data-stu-id="873a3-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="873a3-143">`EnumToNumberConverter` -Wyliczenie numer podstawowy</span><span class="sxs-lookup"><span data-stu-id="873a3-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="873a3-144">`EnumToStringConverter` — Enum ciąg</span><span class="sxs-lookup"><span data-stu-id="873a3-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="873a3-145">`GuidToBytesConverter` — Identyfikator Guid do tablicy typu byte</span><span class="sxs-lookup"><span data-stu-id="873a3-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="873a3-146">`GuidToStringConverter` — Identyfikator Guid ciąg</span><span class="sxs-lookup"><span data-stu-id="873a3-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="873a3-147">`NumberToBytesConverter` -Dowolna wartość liczbowa do tablicy typu byte</span><span class="sxs-lookup"><span data-stu-id="873a3-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="873a3-148">`NumberToStringConverter` -Dowolna wartość liczbową, ciąg</span><span class="sxs-lookup"><span data-stu-id="873a3-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="873a3-149">`StringToBytesConverter` -Ciągu UTF8 bajtów</span><span class="sxs-lookup"><span data-stu-id="873a3-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="873a3-150">`TimeSpanToStringConverter` — TimeSpan ciąg</span><span class="sxs-lookup"><span data-stu-id="873a3-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="873a3-151">`TimeSpanToTicksConverter` -TimeSpan impulsów</span><span class="sxs-lookup"><span data-stu-id="873a3-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="873a3-152">Należy zauważyć, że `EnumToStringConverter` znajduje się na tej liście.</span><span class="sxs-lookup"><span data-stu-id="873a3-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="873a3-153">Oznacza to, że nie ma potrzeby określić konwersji jawnie, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="873a3-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="873a3-154">Zamiast tego można użyć konwertera wbudowane:</span><span class="sxs-lookup"><span data-stu-id="873a3-154">Instead, just use the built-in converter:</span></span>
``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="873a3-155">Należy pamiętać, że wbudowane konwertery są bezstanowe, a więc pojedyncze wystąpienie może być bezpiecznie współużytkowane przez wiele właściwości.</span><span class="sxs-lookup"><span data-stu-id="873a3-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="873a3-156">Wstępnie zdefiniowane konwersje</span><span class="sxs-lookup"><span data-stu-id="873a3-156">Pre-defined conversions</span></span>

<span data-ttu-id="873a3-157">W przypadku typowych konwersji dla których istnieje wbudowany konwerter nie ma potrzeby jawnie określić konwertera.</span><span class="sxs-lookup"><span data-stu-id="873a3-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="873a3-158">Zamiast tego po prostu skonfigurować należy używać typu dostawcy i platforma EF automatycznie użyje odpowiedni konwerter wbudowanych.</span><span class="sxs-lookup"><span data-stu-id="873a3-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate built-in converter.</span></span> <span data-ttu-id="873a3-159">Wyliczenie w celu konwersji ciągów są używane jako przykład powyżej, ale EF faktycznie wykona to automatycznie, jeśli skonfigurowano typ dostawcy:</span><span class="sxs-lookup"><span data-stu-id="873a3-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="873a3-160">Ten sam efekt można osiągnąć przez jawne określenie typu kolumny.</span><span class="sxs-lookup"><span data-stu-id="873a3-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="873a3-161">Na przykład, jeśli nie zdefiniowano typu jednostki, takie jak tak:</span><span class="sxs-lookup"><span data-stu-id="873a3-161">For example, if the entity type is defined like so:</span></span>
``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="873a3-162">Następnie wartości wyliczenia, które zostaną zapisane jako ciągi w bazie danych bez dalszej konfiguracji w `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="873a3-162">Then the enum values will be saved as strings in the database without any further configuration in `OnModelCreating`.</span></span>

## <a name="limitations"></a><span data-ttu-id="873a3-163">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="873a3-163">Limitations</span></span>

<span data-ttu-id="873a3-164">Istnieje kilka znanych bieżących ograniczeń systemu konwersji wartości:</span><span class="sxs-lookup"><span data-stu-id="873a3-164">There are a few known current limitations of the value conversion system:</span></span>
* <span data-ttu-id="873a3-165">Jak wspomniano powyżej, `null` nie może zostać przekonwertowany.</span><span class="sxs-lookup"><span data-stu-id="873a3-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="873a3-166">Obecnie nie ma możliwości się konwersję z jedną właściwość wiele kolumn lub na odwrót.</span><span class="sxs-lookup"><span data-stu-id="873a3-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="873a3-167">Korzystanie z konwersji wartości mogą mieć wpływ na możliwość translacji wyrażeń do bazy danych SQL platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="873a3-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="873a3-168">W takich przypadkach zostanie zarejestrowane ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="873a3-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="873a3-169">Usunięcie tych ograniczeń jest rozważane w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="873a3-169">Removal of these limitations is being considered for a future release.</span></span>
