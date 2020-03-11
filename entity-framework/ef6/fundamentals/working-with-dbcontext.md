---
title: Praca z DbContext-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416323"
---
# <a name="working-with-dbcontext"></a>Praca z klasą DbContext

Aby można było używać Entity Framework do wykonywania zapytań, wstawiania, aktualizowania i usuwania danych przy użyciu obiektów .NET, należy najpierw [utworzyć model](~/ef6/modeling/index.md) , który mapuje jednostki i relacje zdefiniowane w modelu do tabel w bazie danych.

Po utworzeniu modelu Klasa podstawowa, z którą współdziała aplikacja, jest `System.Data.Entity.DbContext` (często określana jako Klasa kontekstu). Można użyć DbContext skojarzonego z modelem, aby:
- Zapisywanie i wykonywanie zapytań   
- Zmaterializowania wyniki zapytania jako obiekty jednostek
- Śledzenie zmian wprowadzonych w tych obiektach
- Utrwalanie zmian obiektów w bazie danych
- Powiązywanie obiektów w kontrolkach z pamięcią

Na tej stronie przedstawiono wskazówki dotyczące zarządzania klasą kontekstową.  

## <a name="defining-a-dbcontext-derived-class"></a>Definiowanie klasy pochodnej DbContext  

Zalecanym sposobem pracy z kontekstem jest zdefiniowanie klasy, która dziedziczy z DbContext i uwidacznia właściwości Nieogólnymi, które reprezentują kolekcje określonych jednostek w kontekście. Jeśli pracujesz z programem Dr Designer, kontekst zostanie wygenerowany dla Ciebie. Jeśli pracujesz z Code First, zazwyczaj Napisz kontekst samodzielnie.  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

Po umieszczeniu kontekstu należy wykonać zapytanie o, dodać (przy użyciu metod `Add` lub `Attach`) lub usunąć (przy użyciu `Remove`) jednostki w kontekście za pośrednictwem tych właściwości. Uzyskiwanie dostępu do właściwości `DbSet` w obiekcie kontekstu reprezentuje zapytanie początkowe zwracające wszystkie jednostki określonego typu. Należy pamiętać, że tylko uzyskanie dostępu do właściwości nie spowoduje wykonania zapytania. Zapytanie jest wykonywane, gdy:  

- Jest on wyliczany przez instrukcję `foreach` (C#) lub `For Each` (Visual Basic).  
- Jest on wyliczany przez operację kolekcji, taką jak `ToArray`, `ToDictionary`lub `ToList`.  
- Operatory LINQ, takie jak `First` lub `Any`, są określone w najbardziej zewnętrznej części zapytania.  
- Wywoływana jest jedna z następujących metod: `Load` Metoda rozszerzenia, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`i `DbSet<T>.Find`, jeśli jednostka z określonym kluczem nie została już załadowana w kontekście.  

## <a name="lifetime"></a>Okres istnienia  

Okres istnienia kontekstu rozpoczyna się, gdy wystąpienie jest tworzone i kończące się, gdy wystąpienie zostanie usunięte lub nie zostało pobrane jako elementy bezużyteczne. Użyj **polecenia using** , jeśli chcesz, aby wszystkie zasoby, które zostaną usunięte przez formant kontekstu na końcu bloku. Gdy używasz **polecenia using**, kompilator automatycznie tworzy blok try/finally i wywołuje metodę Dispose w bloku **finally** .  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

Poniżej przedstawiono ogólne wytyczne dotyczące okresu istnienia kontekstu:  

- Podczas pracy z aplikacjami sieci Web Użyj wystąpienia kontekstu dla żądania.  
- Podczas pracy z Windows Presentation Foundation (WPF) lub Windows Forms, użyj wystąpienia kontekstu na formularz. Dzięki temu można korzystać z funkcji śledzenia zmian udostępnianej przez kontekst.  
- Jeśli wystąpienie kontekstu jest tworzone przez kontener iniekcji zależności, zwykle jest to odpowiedzialność za kontener do usunięcia kontekstu.
- Jeśli kontekst jest tworzony w kodzie aplikacji, należy pamiętać o usunięciu kontekstu, gdy nie jest już wymagany.  
- Podczas pracy z długotrwałym kontekstem należy wziąć pod uwagę następujące kwestie:  
    - Podczas ładowania większej liczby obiektów i ich odwołań do pamięci, użycie pamięci przez kontekst może szybko ulec zwiększeniu. Może to spowodować problemy z wydajnością.  
    - Kontekst nie jest bezpieczny dla wątków, dlatego nie powinien być współużytkowany w wielu wątkach wykonujących wykonywane czynności.
    - Jeśli wyjątek powoduje, że kontekst jest w stanie niemożliwym do odzyskania, cała aplikacja może zakończyć działanie.  
    - Prawdopodobieństwo uruchamiania w ramach problemów związanych z współbieżnością zwiększa się w miarę przerwy między czasem, gdy dane są badane i aktualizowane.  

## <a name="connections"></a>Połączenia  

Domyślnie, kontekst zarządza połączeniami z bazą danych. Kontekst otwiera i zamyka połączenia zgodnie z wymaganiami. Na przykład kontekst otwiera połączenie w celu wykonania zapytania, a następnie zamyka połączenie po przetworzeniu wszystkich zestawów wyników.  

Istnieją przypadki, gdy chcesz mieć większą kontrolę nad tym, kiedy połączenie zostanie otwarte i zamknięte. Na przykład podczas pracy z SQL Server Compact często zaleca się zachowanie oddzielnego otwartego połączenia z bazą danych przez okres istnienia aplikacji w celu zwiększenia wydajności. Można zarządzać tym procesem ręcznie przy użyciu właściwości `Connection`.  
