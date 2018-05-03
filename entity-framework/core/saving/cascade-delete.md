---
title: Usuwanie - EF Core kaskadowe
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: 0fc8929c56d4c657b7fb1e3c8e4b1a71659220c9
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
---
# <a name="cascade-delete"></a>Usuwanie kaskadowe

Usuwanie kaskadowe jest zwykle używany w terminologii bazy danych w przypadku cech, który umożliwia usunięcie wiersza automatycznie wyzwalać usunięcie powiązane wiersze. Ściśle pojęciem objętych zachowania delete EF Core jest automatyczne usuwanie obiektu podrzędnego w przypadku relacji do elementu nadrzędnego zostały Przerwano — jest to często nazywane "Usuwanie oddzielone".

Podstawowe EF implementuje kilka różnych usuwania zachowań i umożliwia konfigurację zachowania delete poszczególnych relacji. Podstawowe EF implementuje również automatycznie konfigurować domyślne przydatne usunięcie zachowania dla każdej relacji na podstawie Konwencji [requiredness relacji](../modeling/relationships.md#required-and-optional-relationships).

## <a name="delete-behaviors"></a>Usuń zachowania
Usuń zachowania są zdefiniowane w *DeleteBehavior* modułu wyliczającego wpisz i mogą zostać przekazane do *OnDelete* interfejsu API fluent do kontroli czy usunięcie jednostki nadrzędne podmiot zabezpieczeń lub severing programu Relacja podmioty zależne od/podrzędny musi mieć efektem ubocznym WE podmioty zależne od/podrzędny.

Istnieją trzy czynności, które EF można wykonać po usunięciu jednostki nadrzędne podmiot zabezpieczeń lub relacji do elementu podrzędnego jest Przerwano:
* Podrzędne/zależnych mogą zostać usunięte.
* Można ustawić wartości kluczy obcych dziecka na wartość null
* Obiekt podrzędny nie jest zmieniany

> [!NOTE]  
> Zachowanie delete skonfigurowane w modelu podstawowej EF jest stosowany tylko wtedy, gdy główna jednostka zostanie usunięta za pomocą EF Core i podmioty zależne są ładowane do pamięci (np. do śledzonych zależnych). Odpowiednie zachowanie cascade należy się, że ustawienia w bazie danych, aby upewnić się, dane, które nie jest śledzony przez kontekst ma niezbędnych działań, które są stosowane. Jeśli używasz EF Core utworzyć bazę danych, to zachowanie w kaskadowego będzie Instalator automatycznie.

Dla drugiej akcji powyższe ustawienie wartości klucza obcego do wartości null jest nieprawidłowy w przypadku klucza obcego nie dopuszcza wartości null. (Wartości null klucz obcy jest odpowiednikiem wymaganej relacji). W takich przypadkach EF Core śledzi, że właściwość klucza obcego została oznaczona jako null aż po wywołaniu metody SaveChanges po tym czasie jest zwracany wyjątek, ponieważ zmiana nie może zostać utrwalona w bazie danych. Jest to podobne do pobierania naruszenie ograniczenia z bazy danych.

Istnieją cztery usunąć zachowania, wymienione w poniższych tabelach. W przypadku relacji opcjonalny (wartość null klucz obcy) on _jest_ można zapisać wartości null wartości klucza obcego, co powoduje następujące skutki:

| Nazwa zachowania               | Wpływ na zależne od/podrzędny w pamięci    | Wpływ na zależne od/podrzędny w bazie danych  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Kaskadowo**                 | Jednostki są usuwane.                   | Jednostki są usuwane.                   |
| **ClientSetNull** (domyślna) | Właściwości klucza obcego są ustawione na wartość null | Brak                                   |
| **SetNull**                 | Właściwości klucza obcego są ustawione na wartość null | Właściwości klucza obcego są ustawione na wartość null |
| **Ogranicz**                | Brak                                   | Brak                                   |

Wymagane relacje (klucza obcego nie dopuszcza wartości null) jest _nie_ można zapisać wartości null wartości klucza obcego, co powoduje następujące skutki:

| Nazwa zachowania         | Wpływ na zależne od/podrzędny w pamięci | Wpływ na zależne od/podrzędny w bazie danych |
|:----------------------|:------------------------------------|:--------------------------------------|
| **CASCADE** (domyślna) | Jednostki są usuwane.                | Jednostki są usuwane.                  |
| **ClientSetNull**     | Zgłasza SaveChanges                  | Brak                                  |
| **SetNull**           | Zgłasza SaveChanges                  | Zgłasza SaveChanges                    |
| **Ogranicz**          | Brak                                | Brak                                  |

W tabelach powyżej *Brak* może spowodować naruszenie ograniczenia. Na przykład jeśli obiekt principal/podrzędny jest usuwany, ale nie podjęto żadnej akcji można zmienić klucza obcego zależne od/podrzędnego, następnie bazy danych prawdopodobnie zgłosi na SaveChanges z powodu naruszenia ograniczenia obcego.

Na wysokim poziomie:
* Jeśli masz jednostek, które nie mogą istnieć bez klasy nadrzędnej, a EF automatyzującą automatycznie usuwania elementów podrzędnych, a następnie użyć *Cascade*.
  * Jednostek, które nie mogą istnieć bez elementu nadrzędnego zwykle należy użyć wymagane relacji, dla którego *Cascade* jest ustawieniem domyślnym.
* Jeśli masz jednostek, które mogą lub nie może mieć elementu nadrzędnego, a EF automatyzującą nulling się klucz obcy dla Ciebie, a następnie użyć *ClientSetNull*
  * Jednostki, które może istnieć bez elementu nadrzędnego zwykle należy użyć relacji opcjonalne, dla którego *ClientSetNull* jest ustawieniem domyślnym.
  * Jeśli chcesz, aby bazę danych do też spróbować propagację wartości null do kluczy obcych podrzędnych nawet po obiektu podrzędnego nie został załadowany, następnie użyj *SetNull*. Jednak należy pamiętać, bazy danych musi obsługiwać to, czy Konfigurowanie bazy danych, takich jak to może spowodować inne ograniczenia, które w praktyce często sprawia, że ta opcja niepraktyczne. Jest to dlaczego *SetNull* nie jest domyślnym.
* Jeśli nie chcesz jądra EF kiedykolwiek automatycznie usuwać jednostki lub null limit klucza obcego automatycznie, a następnie użyj *Ogranicz*. Uwaga to wymaga, że kod synchronizowania jednostek podrzędnych i ich wartości kluczy obcych ręcznie w przeciwnym razie ograniczenia wyjątki będą zgłaszane.

> [!NOTE]
> W podstawowej EF, w odróżnieniu od EF6 efekty kaskadowych odbywa się natychmiast, ale zamiast tego tylko wtedy, gdy jest wywoływana metody SaveChanges.

> [!NOTE]  
> **Zmiany w programie EF Core 2.0:** w poprzednich wersjach *Ogranicz* spowodowałoby opcjonalne właściwości klucza obcego w śledzonych jednostek zależnych, należy ustawić wartości null i został domyślnie Usuń zachowanie w przypadku relacji opcjonalne. W programie EF Core 2.0 *ClientSetNull* wprowadzono w celu reprezentowania tego zachowania i stał się wartością domyślną opcjonalne relacji. Zachowanie *Ogranicz* została dostosowana do nigdy nie ma żadnych efektów ubocznych na jednostek zależnych.

## <a name="entity-deletion-examples"></a>Przykłady usunięcie jednostki

Poniższy kod jest częścią [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) który można pobrać i uruchomić. Przykład pokazuje, co się stanie dla każdego zachowanie Usuń relacje zarówno opcjonalne i wymagane po usunięciu jednostki nadrzędnej.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Przejdźmy każdej zmiany, aby zrozumieć, co dzieje się.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade z relacją wymagany lub opcjonalny

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* Blog jest oznaczone jako usunięte
* Wpisy początkowo pozostać niezmieniony, ponieważ kaskady odbywa się do metody SaveChanges
* Metody SaveChanges wysyła usuwa zarówno zależności/elementy podrzędne (ogłoszeń), a następnie principal/nadrzędnego (blog)
* Po zapisaniu wszystkich jednostek są odłączone od teraz zostały usunięte z bazy danych

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull lub DeleteBehavior.SetNull z wymaganą relację

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* Blog jest oznaczone jako usunięte
* Wpisy początkowo pozostać niezmieniony, ponieważ kaskady odbywa się do metody SaveChanges
* Metody SaveChanges próbuje ustawić po klucz OBCY na wartość null, ale to nie działa, ponieważ klucza Obcego nie dopuszcza wartości null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull lub DeleteBehavior.SetNull z opcjonalną relację

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* Blog jest oznaczone jako usunięte
* Wpisy początkowo pozostać niezmieniony, ponieważ kaskady odbywa się do metody SaveChanges
* Metody SaveChanges prób Ustawia klucz OBCY zarówno zależności/podrzędnych (ogłoszeń) na wartość null przed usunięciem principal/nadrzędnego (blog)
* Po zapisaniu principal/nadrzędnego (blog) zostanie usunięty, ale zależności/podrzędnych (ogłoszeń) nadal są śledzone.
* Śledzonych zależności/podrzędnych (ogłoszeń) teraz mieć wartości null klucza Obcego i ich odwołanie do usuniętego principal/nadrzędnego (blog) zostały usunięte.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict z relacją wymagany lub opcjonalny

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* Blog jest oznaczone jako usunięte
* Wpisy początkowo pozostać niezmieniony, ponieważ kaskady odbywa się do metody SaveChanges
* Ponieważ *Ogranicz* informuje EF, aby automatycznie ustawiony na wartość null, klucza Obcego pozostaje inną niż null i zgłasza SaveChanges bez zapisywania

## <a name="delete-orphans-examples"></a>Usuń porzucone przykłady

Poniższy kod jest częścią [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) który można pobrać uruchomienia. Próbka przedstawiono dla każdego zachowanie Usuń relacje zarówno opcjonalne i wymagane jest Przerwano relacji nadrzędny/podmiot zabezpieczeń i jego elementy podrzędne/zależności. W tym przykładzie relacji jest Przerwano przez usunięcie zależności/podrzędnych (ogłoszeń) z właściwością nawigacji kolekcji principal/nadrzędnego (blog). Jednak zachowanie jest takie samo Jeśli odwołania z potomnym zależne od podmiotu/Parent jest zamiast tego pustych wychodzących.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Przejdźmy każdej zmiany, aby zrozumieć, co dzieje się.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade z relacją wymagany lub opcjonalny

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego może być oznaczony jako wartość null
  * Jeśli klucza Obcego nie dopuszcza wartości null, następnie rzeczywistej wartości nie spowoduje zmiany, nawet jeśli jest oznaczony jako wartość null
* Metody SaveChanges wysyła usuwa zależności/podrzędnych (ogłoszeń)
* Po zapisaniu zależności/podrzędnych (ogłoszeń) są odłączone od teraz zostały usunięte z bazy danych

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull lub DeleteBehavior.SetNull z wymaganą relację

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego może być oznaczony jako wartość null
  * Jeśli klucza Obcego nie dopuszcza wartości null, następnie rzeczywistej wartości nie spowoduje zmiany, nawet jeśli jest oznaczony jako wartość null
* Metody SaveChanges próbuje ustawić po klucz OBCY na wartość null, ale to nie działa, ponieważ klucza Obcego nie dopuszcza wartości null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull lub DeleteBehavior.SetNull z opcjonalną relację

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego może być oznaczony jako wartość null
  * Jeśli klucza Obcego nie dopuszcza wartości null, następnie rzeczywistej wartości nie spowoduje zmiany, nawet jeśli jest oznaczony jako wartość null
* Metody SaveChanges Ustawia klucz OBCY zarówno zależności/podrzędnych (ogłoszeń) na wartość null
* Po zapisaniu zależności/podrzędnych (ogłoszeń) teraz mieć wartości null klucza Obcego i ich odwołanie do usuniętego principal/nadrzędnego (blog) zostały usunięte.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict z relacją wymagany lub opcjonalny

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego może być oznaczony jako wartość null
  * Jeśli klucza Obcego nie dopuszcza wartości null, następnie rzeczywistej wartości nie spowoduje zmiany, nawet jeśli jest oznaczony jako wartość null
* Ponieważ *Ogranicz* informuje EF, aby automatycznie ustawiony na wartość null, klucza Obcego pozostaje inną niż null i zgłasza SaveChanges bez zapisywania

## <a name="cascading-to-untracked-entities"></a>Kaskadowa dla nieśledzonych jednostek

Podczas wywoływania *SaveChanges*, cascade Usuń zasady zostaną zastosowane do żadnych jednostek, które są śledzone przez kontekst. Jest to sytuacja we wszystkich przykładach przedstawionych powyżej, tworzonym SQL został wygenerowany, aby usunąć zarówno principal/nadrzędnego (blog) oraz zależności/elementy podrzędne (ogłoszeń):

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Jeśli podmiot zabezpieczeń jest załadowany — tylko na przykład kwerendy nawiązaniem blogu bez `Include(b => b.Posts)` aby obejmować wpisów — następnie SaveChanges wygeneruje jedynie SQL, aby usunąć nadrzędne podmiot zabezpieczeń:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Zależności/podrzędnych (ogłoszeń) zostanie usunięte tylko w przypadku, gdy baza danych ma skonfigurowane odpowiednie zachowanie cascade. Jeśli używasz EF utworzyć bazę danych, to zachowanie w kaskadowego będzie Instalator automatycznie.
