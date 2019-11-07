---
title: Kaskadowe usuwanie EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: 51c8b6f4517a3f87821ed1e4e2d60549e06ed39d
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656061"
---
# <a name="cascade-delete"></a>Usuwanie kaskadowe

Funkcja usuwania kaskadowego jest często używana w terminologii bazy danych do opisywania cechy, która umożliwia usunięcie wiersza w celu automatycznego wyzwalania usuwania powiązanych wierszy. Silnie powiązane koncepcje objęte również EF Core zachowaniem usunięcia to automatyczne usunięcie jednostki podrzędnej, gdy jest ona relacją z elementem nadrzędnym — jest to często nazywane "usuwaniem sierotów".

EF Core implementuje kilka różnych zachowań usuwania i umożliwia konfigurację zachowań poszczególnych relacji. EF Core również implementuje konwencje, które automatycznie skonfigurują domyślne zachowania usuwania dla każdej relacji w zależności od [wymaganej relacji](../modeling/relationships.md#required-and-optional-relationships).

## <a name="delete-behaviors"></a>Usuwanie zachowań

Zachowania usuwania są zdefiniowane w typie modułu wyliczającego *DeleteBehavior* i mogą być przenoszone do interfejsu API Fluent przy użyciu metody *onDelete* , aby określić, czy usunięcie podmiotu zabezpieczeń/jednostki nadrzędnej lub nawiązanie relacji z jednostkami zależnymi/podrzędnymi powinno mieć efekt uboczny dla jednostek zależnych/podrzędnych.

Istnieją trzy akcje, które można wykonać, gdy jednostka główna/nadrzędna jest usuwana lub relacja do elementu podrzędnego jest poważna:

* Można usunąć element podrzędny/zależny
* Wartości klucza obcego dziecka można ustawić na wartość null.
* Element podrzędny pozostaje niezmieniony

> [!NOTE]  
> Zachowanie usuwania skonfigurowane w modelu EF Core jest stosowane tylko wtedy, gdy jednostka główna jest usuwana przy użyciu EF Core i jednostki zależne są ładowane w pamięci (czyli dla śledzonych elementów zależnych). Odpowiednie zachowanie kaskadowe musi być konfiguracją w bazie danych, aby upewnić się, że dane, które nie są śledzone przez kontekst, mają zastosowane wymagane działanie. Jeśli używasz EF Core do tworzenia bazy danych, to zachowanie kaskadowe zostanie skonfigurowane.

Dla drugiej czynności powyżej ustawienie wartości klucza obcego na null jest nieprawidłowe, jeśli klucz obcy nie dopuszcza wartości null. (Klucz obcy niedopuszczający wartości null jest równoważny z wymaganą relacją). W takich przypadkach EF Core śledzi, że właściwość klucza obcego została oznaczona jako null do momentu wywołania metody SaveChanges, podczas gdy wyjątek jest zgłaszany, ponieważ zmiana nie może zostać utrwalona w bazie danych. Jest to podobne do uzyskiwania naruszenia ograniczenia z bazy danych.

Istnieją cztery zachowania dotyczące usuwania, jak pokazano w poniższej tabeli.

### <a name="optional-relationships"></a>Opcjonalne relacje

W przypadku opcjonalnej relacji (klucz obcy dopuszczający wartość null _) można zapisać_ wartość null klucza obcego, co spowoduje następujące skutki:

| Nazwa zachowania               | Efekt zależny/podrzędny w pamięci    | Efekt zależny/podrzędny w bazie danych  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Cascade**                 | Jednostki zostały usunięte                   | Jednostki zostały usunięte                   |
| **ClientSetNull** (domyślnie) | Właściwości klucza obcego są ustawione na wartość null. | Brak                                   |
| **SetNull**                 | Właściwości klucza obcego są ustawione na wartość null. | Właściwości klucza obcego są ustawione na wartość null. |
| **Ograniczone**                | Brak                                   | Brak                                   |

### <a name="required-relationships"></a>Wymagane relacje

W przypadku wymaganych relacji (klucz obcy niedopuszczający wartości null) _nie_ można zapisać wartości null klucza obcego, co spowoduje następujące skutki:

| Nazwa zachowania         | Efekt zależny/podrzędny w pamięci | Efekt zależny/podrzędny w bazie danych |
|:----------------------|:------------------------------------|:--------------------------------------|
| **Kaskada** (domyślnie) | Jednostki zostały usunięte                | Jednostki zostały usunięte                  |
| **ClientSetNull**     | Metody SaveChanges zgłasza                  | Brak                                  |
| **SetNull**           | Metody SaveChanges zgłasza                  | Metody SaveChanges zgłasza                    |
| **Ograniczone**          | Brak                                | Brak                                  |

W podanych powyżej tabelach *żaden* z nich może spowodować naruszenie ograniczenia. Na przykład, jeśli jednostka główna/podrzędna jest usuwana, ale nie jest podejmowana żadna akcja w celu zmiany klucza obcego elementu zależnego/podrzędnego, baza danych prawdopodobnie zgłosi się na metody SaveChanges z powodu naruszenia ograniczenia obcego.

Na wysokim poziomie:

* Jeśli masz jednostki, które nie mogą istnieć bez elementu nadrzędnego, i chcesz, aby program Dr zadbać o automatyczne usunięcie elementów podrzędnych, a następnie użyj opcji *kaskadowych*.
  * Jednostki, które nie mogą istnieć bez elementu nadrzędnego zwykle korzystają z wymaganych relacji, dla których *Kaskada* jest wartością domyślną.
* Jeśli masz jednostki, które mogą lub nie mają elementu nadrzędnego, i chcesz, aby EF zadbać o wyzerowanie klucza obcego, a następnie użyj *ClientSetNull*
  * Jednostki, które mogą istnieć bez elementu nadrzędnego zwykle używają relacji opcjonalnych, dla których *ClientSetNull* jest wartością domyślną.
  * Jeśli chcesz, aby baza danych mogła próbować propagować wartości null do podrzędnych kluczy obcych, nawet gdy jednostka podrzędna nie została załadowana, użyj właściwości *SetNull*. Należy jednak pamiętać, że ta baza danych programu musi obsługiwać ten program, a Konfiguracja bazy danych może spowodować inne ograniczenia, które w praktyce często powodują, że ta opcja jest nieprzydatna. To dlatego, że *SetNull* nie jest wartością domyślną.
* Jeśli nie chcesz, aby EF Core kiedykolwiek usunąć jednostki automatycznie lub wyzerować klucz obcy automatycznie, użyj opcji *Ogranicz*. Należy zauważyć, że wymaga to, aby kod utrzymywać jednostki podrzędne i ich wartości klucza obcego w synchronizacji ręcznie w przeciwnym razie zostaną zgłoszone wyjątki ograniczenia.

> [!NOTE]
> W EF Core, w przeciwieństwie do EF6, efekty kaskadowe nie są wykonywane natychmiast, ale tylko wtedy, gdy jest wywoływana metody SaveChanges.

> [!NOTE]  
> **Zmiany w EF Core 2,0:** W poprzednich wersjach *ograniczenie* mogłoby spowodować, że właściwości opcjonalnego klucza obcego w śledzonych jednostkach zależnych mają być ustawione na wartość null, i było domyślnym zachowaniem usuwania dla relacji opcjonalnych. W EF Core 2,0 został wprowadzony *ClientSetNull* do reprezentowania tego zachowania i stał się wartością domyślną dla relacji opcjonalnych. Zachowanie *ograniczenia* zostało dostosowane do wartości nigdy nie mają żadnych efektów ubocznych w jednostkach zależnych.

## <a name="entity-deletion-examples"></a>Przykłady usuwania jednostek

Poniższy kod jest częścią [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) , którą można pobrać i uruchomić. Przykład pokazuje, co się stanie w przypadku każdego zachowania usuwania zarówno dla relacji opcjonalnych, jak i wymaganych, gdy jednostka nadrzędna jest usuwana.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Zapoznaj się z informacjami o tym, co się dzieje.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior. Kaskada z wymaganą lub opcjonalną relacją

```console
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* Blog jest oznaczony jako usunięty
* Wpisy pozostają początkowo niezmienione, ponieważ kaskady nie są wykonywane do metody SaveChanges
* Metody SaveChanges wysyła usunięcia zarówno dla elementów zależnych, jak i podrzędnych (wpisów), a następnie podmiotu zabezpieczeń/elementu nadrzędnego (blog)
* Po zapisaniu wszystkie jednostki są odłączone, ponieważ zostały usunięte z bazy danych

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior. ClientSetNull lub DeleteBehavior. SetNull z wymaganą relacją

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* Blog jest oznaczony jako usunięty
* Wpisy pozostają początkowo niezmienione, ponieważ kaskady nie są wykonywane do metody SaveChanges
* Metody SaveChanges próbuje ustawić wartość null dla wpisu klucza obcego, ale to nie powiodło się, ponieważ obcy nie dopuszcza wartości null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior. ClientSetNull lub DeleteBehavior. SetNull z opcjonalną relacją

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* Blog jest oznaczony jako usunięty
* Wpisy pozostają początkowo niezmienione, ponieważ kaskady nie są wykonywane do metody SaveChanges
* Metody SaveChanges próbuje przed usunięciem podmiotu zabezpieczeń/elementu nadrzędnego (ogłoszenia) do wartości null
* Po zapisaniu podmiot zabezpieczeń/nadrzędne (blog) zostanie usunięty, ale zależności/elementy podrzędne (wpisy) są nadal śledzone
* Śledzone zależności/elementy podrzędne (wpisy) mają teraz wartości klucza obcego i ich odwołanie do usuniętego podmiotu zabezpieczeń/nadrzędnego (blog) zostało usunięte

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior. Ogranicz z wymaganą lub opcjonalną relacją

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* Blog jest oznaczony jako usunięty
* Wpisy pozostają początkowo niezmienione, ponieważ kaskady nie są wykonywane do metody SaveChanges
* Ponieważ wartość *ograniczenia* powoduje, że EF nie ustawia automatycznie klucza obcego na null, pozostaje nierówna null i metody SaveChanges zgłasza bez zapisywania

## <a name="delete-orphans-examples"></a>Usuń przykłady oddzielonych

Poniższy kod jest częścią [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) , którą można pobrać i uruchomić. Przykład pokazuje, co się stanie w przypadku każdego zachowania usuwania zarówno dla relacji opcjonalnych, jak i wymaganych, gdy relacja między obiektem nadrzędnym/podmiotem zabezpieczeń a jego elementami podrzędnymi/zależnymi jest poważna. W tym przykładzie relacja jest porzucana przez usunięcie elementów zależnych/podrzędnych (wpisów) z właściwości nawigacji kolekcji na serwerze głównym/nadrzędnym (blog). Zachowanie jest jednak takie samo, jeśli odwołanie od elementu zależnego/podrzędnego do podmiotu zabezpieczeń/nadrzędne jest w zamian wartością null.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Zapoznaj się z informacjami o tym, co się dzieje.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior. Kaskada z wymaganą lub opcjonalną relacją

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* Wpisy są oznaczane jako zmodyfikowane, ponieważ powoduje to odłączenie relacji, co spowodowało oznaczenie klucza obcego jako null
  * Jeśli obcy nie dopuszcza wartości null, wartość rzeczywista nie zmieni się, mimo że jest oznaczona jako null
* Metody SaveChanges wysyła usunięcia dla elementów zależnych/podrzędnych (wpisów)
* Po zapisaniu elementy zależne/podrzędne (wpisy) są odłączone, ponieważ zostały usunięte z bazy danych.

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior. ClientSetNull lub DeleteBehavior. SetNull z wymaganą relacją

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* Wpisy są oznaczane jako zmodyfikowane, ponieważ powoduje to odłączenie relacji, co spowodowało oznaczenie klucza obcego jako null
  * Jeśli obcy nie dopuszcza wartości null, wartość rzeczywista nie zmieni się, mimo że jest oznaczona jako null
* Metody SaveChanges próbuje ustawić wartość null dla wpisu klucza obcego, ale to nie powiodło się, ponieważ obcy nie dopuszcza wartości null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior. ClientSetNull lub DeleteBehavior. SetNull z opcjonalną relacją

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* Wpisy są oznaczane jako zmodyfikowane, ponieważ powoduje to odłączenie relacji, co spowodowało oznaczenie klucza obcego jako null
  * Jeśli obcy nie dopuszcza wartości null, wartość rzeczywista nie zmieni się, mimo że jest oznaczona jako null
* Metody SaveChanges ustawia wartość klucza obcego obu zależności/elementów podrzędnych (wpisów) na null
* Po zapisaniu elementy zależne/podrzędne (wpisy) mają teraz wartości klucza obcego i ich odwołanie do usuniętego podmiotu zabezpieczeń/nadrzędnego (blog) zostało usunięte

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior. Ogranicz z wymaganą lub opcjonalną relacją

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* Wpisy są oznaczane jako zmodyfikowane, ponieważ powoduje to odłączenie relacji, co spowodowało oznaczenie klucza obcego jako null
  * Jeśli obcy nie dopuszcza wartości null, wartość rzeczywista nie zmieni się, mimo że jest oznaczona jako null
* Ponieważ wartość *ograniczenia* powoduje, że EF nie ustawia automatycznie klucza obcego na null, pozostaje nierówna null i metody SaveChanges zgłasza bez zapisywania

## <a name="cascading-to-untracked-entities"></a>Kaskadowanie do nieśledzonych jednostek

Po wywołaniu *metody SaveChanges*reguły usuwania kaskadowego będą stosowane do wszystkich jednostek, które są śledzone przez kontekst. Jest to sytuacja we wszystkich przykładach przedstawionych powyżej, co oznacza, że program SQL został wygenerowany w celu usunięcia zarówno podmiotu zabezpieczeń, jak i wszystkich elementów zależnych/podrzędnych (wpisów):

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

W przypadku załadowania tylko podmiotu zabezpieczeń — na przykład gdy kwerenda zostanie wykonana dla blogu bez `Include(b => b.Posts)` do dołączenia wpisów, a następnie metody SaveChanges będzie generować tylko SQL w celu usunięcia podmiotu zabezpieczeń/elementu nadrzędnego:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Zależności/elementy podrzędne (wpisy) zostaną usunięte tylko wtedy, gdy baza danych ma skonfigurowane odpowiednie zachowanie kaskadowe. Jeśli utworzysz bazę danych za pomocą programu EF, to zachowanie kaskadowe zostanie skonfigurowane.
