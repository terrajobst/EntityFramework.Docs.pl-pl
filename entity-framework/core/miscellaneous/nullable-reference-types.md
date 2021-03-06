---
title: Praca z typami odwołań do wartości null — EF Core
author: roji
ms.date: 09/09/2019
ms.assetid: bde4e0ee-fba3-4813-a849-27049323d301
uid: core/miscellaneous/nullable-reference-types
ms.openlocfilehash: c16a475c363320cd18804a4efe78ccae1ae22f0d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416656"
---
# <a name="working-with-nullable-reference-types"></a>Praca z typami odwołań dopuszczających wartości null

C#8 wprowadzono nową funkcję o nazwie [dopuszczającej wartość null](/dotnet/csharp/tutorials/nullable-reference-types), co pozwala na dodawanie adnotacji do typów odwołań wskazujących, czy są one prawidłowe dla nich, aby zawierały wartość null. Jeśli dopiero zaczynasz tę funkcję, zalecamy zapoznanie się z nią, czytając C# dokumenty.

Na tej stronie EF Core wprowadzono pomoc techniczną dla typów referencyjnych dopuszczających wartości null oraz opisano najlepsze rozwiązania dotyczące pracy z nimi.

## <a name="required-and-optional-properties"></a>Właściwości wymagane i opcjonalne

Główna dokumentacja dotycząca wymaganych i opcjonalnych właściwości oraz ich interakcji z typami odwołań dopuszczających wartość null jest stroną [Właściwości wymagane i opcjonalne](xref:core/modeling/entity-properties#required-and-optional-properties) . Zalecamy rozpoczęcie od początku odczytywania tej strony.

> [!NOTE]
> Należy zachować ostrożność podczas włączania typów referencyjnych dopuszczających wartość null w istniejącym projekcie: właściwości typu referencyjnego, które zostały wcześniej skonfigurowane jako opcjonalne, zostaną teraz skonfigurowane zgodnie z wymaganiami, chyba że zostaną jawnie dodane do wartości null. W przypadku zarządzania schematem relacyjnej bazy danych może to spowodować wygenerowanie migracji, co pozwala zmienić wartość null kolumny bazy danych.

## <a name="dbcontext-and-dbset"></a>DbContext i Nieogólnymi

Gdy włączone są typy odwołań dopuszczających wartość C# null, kompilator emituje ostrzeżenia dla każdej niezainicjowanej właściwości, która nie dopuszcza wartości null. W związku z tym typowa Metoda definiowania `DbSet` niedopuszczających wartości null w kontekście spowoduje teraz wygenerowanie ostrzeżenia. Jednak EF Core zawsze inicjuje wszystkie właściwości `DbSet` w typach pochodnych DbContext, dzięki czemu są one gwarantowane, że nigdy nie będzie mieć wartości null, nawet jeśli kompilator nie będzie mógł tego wiedzieć. W związku z tym zaleca się pozostawienie właściwości `DbSet`, które nie dopuszczają wartości null, umożliwiając dostęp do nich bez sprawdzania wartości null — i wyciszenie ostrzeżeń kompilatora przez jawne ustawienie ich jako null za pomocą operatora łagodniejszej null (!):

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/NullableReferenceTypesContext.cs?name=Context&highlight=3-4)]

## <a name="non-nullable-properties-and-initialization"></a>Właściwości i inicjalizacje bez dopuszczania wartości null

Ostrzeżenia kompilatora dla niezainicjowanych typów referencyjnych, które nie dopuszczają wartości null, są również problemem dla zwykłych właściwości w typach jednostek. W naszym przykładzie unikamy tych ostrzeżeń przy użyciu [powiązania konstruktora](xref:core/modeling/constructors), a funkcja, która doskonale sprawdza się w przypadku właściwości, które nie dopuszcza wartości null, gwarantując, że są one zawsze inicjowane. Jednak w niektórych scenariuszach powiązanie konstruktora nie jest opcją: nie można w ten sposób zainicjować właściwości nawigacji, na przykład.

Wymagane właściwości nawigacji stanowią dodatkowe trudności: Chociaż dla danego podmiotu zależnego będzie zawsze istniała zależność, może ona zostać załadowana przez określone zapytanie lub nie może być załadowane w zależności od potrzeb w tym momencie w programie ([Zobacz różne wzorce dotyczące ładowania danych](xref:core/querying/related-data)). W tym samym czasie nie należy wprowadzać tych właściwości do wartości null, ponieważ spowodowałoby to wymuszenie dostępu do nich do sprawdzenia pod kątem wartości null, nawet jeśli są one wymagane.

Jednym ze sposobów postępowania z tymi scenariuszami jest posiadanie właściwości niedopuszczającej wartości null przy użyciu [pola zapasowego](xref:core/modeling/backing-field)dopuszczającego wartość null:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Order.cs?range=10-17)]

Ponieważ właściwość nawigacji nie dopuszcza wartości null, jest konfigurowana wymagana Nawigacja; i tak długo, jak nawigacja zostanie prawidłowo załadowana, zależna będzie dostępna za pośrednictwem właściwości. Jeśli jednak właściwość jest dostępna bez wcześniejszego poprawnego załadowania powiązanej jednostki, zgłaszany jest InvalidOperationException, ponieważ kontrakt interfejsu API został niepoprawnie użyty. Należy pamiętać, że EF musi być skonfigurowany tak, aby zawsze miał dostęp do pola zapasowego, a nie do właściwości, ponieważ polega na możliwym odczytaniu wartości nawet wtedy, gdy ustawienie nie zostanie ustawione; Zapoznaj się z dokumentacją dotyczącą [pól zapasowych](xref:core/modeling/backing-field) , aby to zrobić, i rozważ określenie `PropertyAccessMode.Field`, aby upewnić się, że konfiguracja jest poprawna.

Jako alternatywę Terser można po prostu zainicjować właściwość null przy użyciu operatora null-łagodniejszej (!):

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Order.cs?range=19)]

Rzeczywista wartość null nigdy nie będzie obserwowana, chyba że w wyniku błędu programowania, np. do uzyskania dostępu do właściwości nawigacji bez prawidłowo załadowania powiązanej jednostki.

> [!NOTE]
> Nawigacja kolekcji, która zawiera odwołania do wielu pokrewnych jednostek, nie powinna być zawsze dopuszczana do wartości null. Pusta kolekcja oznacza, że nie istnieją żadne powiązane jednostki, ale sama lista nie powinna mieć wartości null.

## <a name="navigating-and-including-nullable-relationships"></a>Nawigowanie i uwzględnianie relacji dopuszczających wartości null

Podczas pracy z opcjonalnymi relacjami można napotkać ostrzeżenia kompilatora, w których rzeczywisty wyjątek odwołania o wartości null byłby możliwy. Podczas tłumaczenia i wykonywania zapytań LINQ EF Core gwarantuje, że jeśli opcjonalna powiązana jednostka nie istnieje, dowolna Nawigacja do niej zostanie po prostu zignorowana, a nie przerzucana. Jednak kompilator nie rozpoznaje tej EF Core gwarancji i generuje ostrzeżenia tak, jakby kwerenda LINQ była wykonywana w pamięci, z LINQ to Objects. W związku z tym należy użyć operatora null-łagodniejszej (!) do informowania kompilatora, że rzeczywista wartość null nie jest możliwa:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Program.cs?range=46)]

Podobny problem występuje w przypadku uwzględnienia wielu poziomów relacji między opcjonalnymi nawigacjami:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Program.cs?range=36-39&highlight=2)]

Jeśli znajdziesz się w tej partii, a typy jednostek, których dotyczy, są głównie (lub wyłącznie) używane w zapytaniach EF Core, rozważ wprowadzenie właściwości nawigacji, które nie dopuszczają wartości null, i skonfigurowanie ich jako opcjonalnego za pośrednictwem interfejsu API Fluent lub adnotacji danych. Spowoduje to usunięcie wszystkich ostrzeżeń kompilatora z zachowaniem opcjonalnej relacji; Jeśli jednak obiekty są przenoszone poza EF Core, można zaobserwować wartości null, chociaż właściwości są adnotacją jako niedopuszczające wartości null.

## <a name="limitations"></a>Ograniczenia

* Proces odtwarzania nie obsługuje C# [ C# obecnie 8 typów referencyjnych dopuszczających wartość null (NRTs)](/dotnet/csharp/tutorials/nullable-reference-types): EF Core zawsze generuje kod, który zakłada, że ta funkcja jest wyłączona. Na przykład kolumny tekstu dopuszczające wartość null będą szkieletem jako właściwość o typie `string`, a nie `string?`, z interfejsem API Fluent lub adnotacjami danych używanymi do konfigurowania, czy właściwość jest wymagana. Można edytować kod szkieletowy i zastąpić je adnotacjami o C# wartości null. Obsługa tworzenia szkieletów dla typów odwołań do wartości null jest śledzona przez [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520)problemu.
* Publiczna powierzchnia interfejsu API EF Core nie została jeszcze dostosowana do wartości null (Publiczny interfejs API to "null-Oblivious"), co sprawia, że niewygodna jest używana, gdy funkcja NRT jest włączona. Dotyczy to szczególnie operatorów asynchronicznych LINQ uwidocznionych przez EF Core, takich jak [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_). Planujemy rozwiązać ten rozwiązanie w wersji 5,0.
