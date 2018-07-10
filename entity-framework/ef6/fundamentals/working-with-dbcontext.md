---
title: Praca z DbContext - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
caps.latest.revision: 3
ms.openlocfilehash: 865c9883ce25f405a173791df4e46b98550cd41f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912047"
---
# <a name="working-with-dbcontext"></a>Praca z typu DbContext

Aby można było używać programu Entity Framework do zapytania, wstawiania, aktualizowania i usuwania danych, używając obiektów platformy .NET, należy najpierw [utworzyć Model](~/ef6/modeling/index.md) która mapuje jednostek i relacji, które są zdefiniowane w modelu służącym do tabel w bazie danych.

Po utworzeniu modelu, jest podstawową klasą aplikacji współdziała z `System.Data.Entity.DbContext` (często określanymi jako klasy kontekstu). Można użyć typu DbContext powiązanych z modelu do:
- Pisanie i wykonywania zapytań   
- Zmaterializowania wyników zapytania jako obiekty jednostki
- Śledź zmiany wprowadzone do tych obiektów
- Utrwalanie zmian obiektów na bazie danych
- Powiązanie obiektów w pamięci na kontrolkę interfejsu użytkownika

Ta strona udostępnia wskazówki na temat zarządzania z klasy kontekstu.  

## <a name="defining-a-dbcontext-derived-class"></a>Definiowanie klasy pochodne typu DbContext  

Zalecany sposób pracy z kontekstu jest Definiowanie klasy, która pochodzi od typu DbContext i udostępnia właściwości DbSet, które reprezentują kolekcji określonej jednostki w kontekście. Jeśli pracujesz w Projektancie platformy EF kontekście zostanie wygenerowany dla Ciebie. Jeśli pracujesz z Code First, zwykle napiszesz kontekście samodzielnie.  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

Po utworzeniu kontekstu, należy wyszukać, Dodaj (przy użyciu `Add` lub `Attach` metody) lub Usuń (przy użyciu `Remove`) jednostki w kontekście za pomocą tych właściwości. Uzyskiwanie dostępu do `DbSet` właściwość obiektu kontekstu reprezentują począwszy od zapytania, które zwraca wszystkich jednostek określonego typu. Należy zauważyć, że tylko dostęp do właściwości nie będą wykonywane zapytania. Zapytanie jest wykonywane gdy:  

- Są wyliczane przez `foreach` (C#) lub `For Each` — instrukcja (Visual Basic).  
- Jego są wyliczane przez operację kolekcji takich jak `ToArray`, `ToDictionary`, lub `ToList`.  
- Operatory LINQ, takich jak `First` lub `Any` są określone w najbardziej zewnętrznej część zapytania.  
- Noszą nazwę jednej z następujących metod: `Load` metody rozszerzenia `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, i `DbSet<T>.Find`, jeśli jednostki z określonym kluczem nie znajduje się już ładowane w kontekście.  

## <a name="lifetime"></a>Okres istnienia  

Okres istnienia w kontekście rozpoczyna się, gdy wystąpienie zostanie utworzona i kończy się, gdy wystąpienie jest usunięty lub zebranych elementów bezużytecznych. Użyj **przy użyciu** Jeśli chcesz, aby wszystkie zasoby, które kontekstu kontrolki usuwana na końcu bloku. Kiedy używasz **przy użyciu**, kompilator automatycznie tworzy blok try/finally i wywołuje metodę dispose w **na koniec** bloku.  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

Poniżej przedstawiono ogólne wskazówki przy podejmowaniu decyzji o okresie istnienia w kontekście:  

- Podczas pracy z aplikacjami sieci Web, należy użyć wystąpienia kontekstu na żądanie.  
- Podczas pracy z Windows Presentation Foundation (WPF) lub Windows Forms, należy użyć wystąpienia kontekstu dla formularza. Dzięki temu można korzystać z funkcji śledzenia zmian zapewnia tego kontekstu.  
- Jeśli wystąpienie kontekstu jest tworzony przez kontener iniekcji zależności, zazwyczaj przyczyną jest odpowiedzialność kontenera można zlikwidować kontekstu.
- Jeśli kontekst jest tworzony w kodzie aplikacji, pamiętaj, aby dysponować kontekstu, gdy nie jest już wymagany.  
- Podczas pracy z kontekstem długoterminowych należy rozważyć następujące kwestie:  
    - Jak załadować więcej obiektów i ich odwołania do pamięci, zmniejszenie zużycia pamięci kontekstu może szybko wzrosnąć. Może to spowodować problemy z wydajnością.  
    - Kontekst nie jest bezpieczna dla wątków, w związku z tym go nie może być współużytkowany przez wiele wątków jednocześnie wykonywania pracy.
    - Jeśli wyjątek powoduje, że kontekst będzie w stanie nieodwracalny, może rozwiązać niniejszą całej aplikacji.  
    - Ryzyko związane z współbieżności problemy podczas zwiększyć wraz ze wzrostem natężenia przerwy między czasem, gdy dane są badane i zaktualizowane.  

## <a name="connections"></a>Połączenia  

Domyślnie w kontekście zarządza połączeń z bazą danych. Kontekst otwiera i zamyka połączenia, zgodnie z potrzebami. Na przykład kontekście otwiera połączenie, aby wykonać zapytanie, a następnie zamyka połączenia, po przetworzeniu wszystkich zestawów wyników.  

Istnieją przypadki, gdy użytkownik chce mieć większą kontrolę nad, gdy połączenie otwiera i zamyka. Na przykład podczas pracy z programu SQL Server Compact, często zaleca się zachować oddzielne Otwieranie połączenia z bazą danych dla cyklu życia aplikacji w celu zwiększenia wydajności. Ten proces można zarządzać ręcznie za pomocą `Connection` właściwości.  
