---
title: Usuwanie — EF Core kaskadowe
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: 2c50d94aafb3788761efc4225b6340a8e0da712d
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911565"
---
# <a name="cascade-delete"></a>Usuwanie kaskadowe

Usuwanie kaskadowe jest najczęściej używany w terminologii bazy danych do opisania cechę, która umożliwia usunięcie wiersza automatycznie wyzwalać usunięcie powiązane wiersze. Ściśle powiązanych koncepcji, również pasuje do żadnego zachowań usuwania programu EF Core jest automatyczne usuwanie obiektu podrzędnego, gdy relacji do elementu nadrzędnego zostały oddzielone — jest to często nazywane "Usuwanie porzucone".

EF Core implementuje kilka różnych delete zachowań i umożliwia konfigurację zachowania Usuń poszczególne relacji. EF Core implementuje również konwencje, które automatycznie konfigurują zachowania delete przydatne domyślne dla każdej relacji na podstawie [requiredness relacji](../modeling/relationships.md#required-and-optional-relationships).

## <a name="delete-behaviors"></a>Usuń zachowania
Usuń zachowania są zdefiniowane w *DeleteBehavior* moduł wyliczający typu i mogą być przekazywane do *OnDelete* wygodnego interfejsu API do kontroli czy usunięcie jednostki jednostki/element nadrzędny lub severing programu Relacja do podmiotów zależnych od ustawień lokalnych/podrzędny musi mieć efekt uboczny w jednostek zależnych od ustawień lokalnych/podrzędny.

Dostępne są trzy akcje, które EF można podjąć, gdy jednostka jednostki/nadrzędna została usunięta lub jest oddzielone relacji do elementu podrzędnego:
* Można je usunąć podrzędnych/zależnych od ustawień lokalnych
* Wartości klucza obcego elementu podrzędnego można ustawić na wartość null
* Element podrzędny nie jest zmieniany

> [!NOTE]  
> Zachowanie dotyczące usuwania konfigurowane w modelu platformy EF Core są stosowane tylko w sytuacji, gdy jednostki głównej jest usuwana za pomocą programu EF Core i jednostki zależne są ładowane do pamięci (np. do śledzonych elementów zależnych). Odpowiednie zachowania kaskadowe musi być Instalator w bazie danych, aby upewnić się, dane, które nie jest śledzony przez kontekst ma niezbędnych działań, które są stosowane. Jeśli użyjesz programu EF Core do utworzenia bazy danych tego zachowania kaskadowe będzie Instalatora dla Ciebie.

Dla drugiego z powyższej akcji ustawienie wartości klucza obcego o wartości null jest nieprawidłowa w przypadku klucza obcego nie dopuszcza wartości null. (Dopuszcza klucz obcy jest odpowiednikiem wymaganej relacji). W takich przypadkach programu EF Core śledzi, czy właściwość klucza obcego została oznaczona jako wartości null do momentu SaveChanges jest wywoływana, co jest zgłaszany wyjątek, ponieważ zmiana nie może zostać utrwalona w bazie danych. Jest to podobne do pobierania naruszenie ograniczenia z bazy danych.

Istnieją cztery Usuń zachowań, zgodnie z opisem w poniższych tabelach.

### <a name="optional-relationships"></a>Opcjonalne relacji
W przypadku relacji opcjonalne (dopuszcza wartości null z kluczem obcym) on _jest_ można zapisać wartości null wartości klucza obcego, co powoduje następujące skutki:

| Nazwa zachowania               | Wpływ na zależnych od ustawień lokalnych/podrzędny w pamięci    | Wpływ na zależnych od ustawień lokalnych/podrzędnej bazy danych  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Kaskadowe**                 | Jednostki są usuwane.                   | Jednostki są usuwane.                   |
| **ClientSetNull** (opcja domyślna) | Właściwości klucza obcego są ustawione na wartość null | Brak                                   |
| **SetNull**                 | Właściwości klucza obcego są ustawione na wartość null | Właściwości klucza obcego są ustawione na wartość null |
| **Ograniczenia**                | Brak                                   | Brak                                   |

### <a name="required-relationships"></a>Wymagane relacje
W przypadku relacji (innych niż null z kluczem obcym), wymagane jest _nie_ można zapisać wartości null wartości klucza obcego, co powoduje następujące skutki:

| Nazwa zachowania         | Wpływ na zależnych od ustawień lokalnych/podrzędny w pamięci | Wpływ na zależnych od ustawień lokalnych/podrzędnej bazy danych |
|:----------------------|:------------------------------------|:--------------------------------------|
| **Kaskadowe** (opcja domyślna) | Jednostki są usuwane.                | Jednostki są usuwane.                  |
| **ClientSetNull**     | Zgłasza SaveChanges                  | Brak                                  |
| **SetNull**           | Zgłasza SaveChanges                  | Zgłasza SaveChanges                    |
| **Ograniczenia**          | Brak                                | Brak                                  |

W tabelach powyżej *Brak* może doprowadzić do naruszenia ograniczenia. Na przykład jeśli jednostka jednostki/podrzędny jest usuwana, lecz nie podjęto żadnej akcji można zmienić klucza obcego z zależnych od ustawień lokalnych/podrzędny, następnie bazie danych prawdopodobnie zgłosi na SaveChanges z powodu naruszenia ograniczenia obcego.

Na wysokim poziomie:
* Jeśli masz jednostek, które nie mogą istnieć bez klasy nadrzędnej, a EF automatyzującą automatycznego usuwania elementy podrzędne, a następnie użyć *Cascade*.
  * Jednostki, które nie mogą znajdować się bez nadrzędnej zwykle należy na użytek wymaganych relacji, które *Cascade* jest ustawieniem domyślnym.
* Jeśli masz jednostek, które mogą lub nie może mieć elementu nadrzędnego i EF, aby zadbać o nulling się klucz obcy dla Ciebie, a następnie użyć *ClientSetNull*
  * Jednostek, które może istnieć bez nadrzędnej zwykle należy użyć opcjonalne relacji, dla którego *ClientSetNull* jest ustawieniem domyślnym.
  * Jeśli mają również próbować nawet propagację wartości null do podrzędnych klucze obce w bazie danych po jednostce podrzędnej nie został załadowany, następnie za pomocą *SetNull*. Jednak pamiętaj, że baza danych musi obsługiwać to Konfigurowanie bazy danych, np. to może spowodować inne ograniczenia, co w praktyce często sprawia, że ta opcja niepraktyczne. Dlatego *SetNull* nie jest ustawieniem domyślnym.
* Jeśli nie chcesz programu EF Core kiedykolwiek automatycznie Usuń jednostkę lub wartość null out klucza obcego, automatycznie, a następnie użyć *Ogranicz*. Należy pamiętać, że wymaga to, że Twój kod Synchronizuj podrzędnych jednostek i ich wartości klucza obcego ręcznie w przeciwnym razie ograniczenie wyjątki zostanie wygenerowany.

> [!NOTE]
> W programie EF Core, w odróżnieniu od EF6 efekty kaskadowych nie nawiązały natychmiast, ale zamiast tego tylko wtedy, gdy jest wywoływana SaveChanges.

> [!NOTE]  
> **Zmiany w programie EF Core 2.0:** w poprzednich wersjach *Ogranicz* spowodowałoby opcjonalne właściwości klucza obcego w śledzonych jednostki zależne, należy ustawić wartości null i zostało domyślne zachowanie dla relacji opcjonalne przy usuwaniu. W programie EF Core 2.0 *ClientSetNull* została wprowadzona do reprezentowania tego zachowania i stało się domyślnie w przypadku relacji opcjonalne. Zachowanie *Ogranicz* została dostosowana do nigdy nie ma żadnych efektów ubocznych na jednostki zależne.

## <a name="entity-deletion-examples"></a>Przykłady usuwania jednostki

Poniższy kod jest częścią [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , można pobrać i uruchomić. Przykład pokazuje, co się stanie dla każdego zachowanie dotyczące usuwania dla relacji wymagane i opcjonalne, po usunięciu obiektu nadrzędnego.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Przejdźmy teraz przez poszczególnych odmian, aby zrozumieć, co się dzieje.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade z relacją wymagane lub opcjonalne

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

* Blog jest oznaczony jako usunięty
* Wpisy początkowo pozostać Unchanged, ponieważ kaskady tak się nie stanie do momentu SaveChanges
* SaveChanges wysyła usuwa zarówno zależności/podrzędnych (wpisy), a następnie jednostkę/element nadrzędny (blog)
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

* Blog jest oznaczony jako usunięty
* Wpisy początkowo pozostać Unchanged, ponieważ kaskady tak się nie stanie do momentu SaveChanges
* SaveChanges podejmuje próbę ustawienia wpisu klucza Obcego na wartość null, ale to nie powiodło się ponieważ klucza Obcego nie dopuszcza wartości null

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

* Blog jest oznaczony jako usunięty
* Wpisy początkowo pozostać Unchanged, ponieważ kaskady tak się nie stanie do momentu SaveChanges
* Próby SaveChanges ustawia klucza Obcego z obu zależności/elementów podrzędnych (ogłoszeń) o wartości null przed usunięciem jednostki/element nadrzędny (blog)
* Po zapisaniu jednostki/element nadrzędny (blog) zostanie usunięty, ale zależności/elementy podrzędne (ogłoszeń) nadal są śledzone.
* Śledzone zależności/elementy podrzędne (ogłoszeń) ma wartości null klucza Obcego, a ich odwołania do jednostki/nadrzędnych usunięte (blog) została usunięta.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict z relacją wymagane lub opcjonalne

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

* Blog jest oznaczony jako usunięty
* Wpisy początkowo pozostać Unchanged, ponieważ kaskady tak się nie stanie do momentu SaveChanges
* Ponieważ *Ogranicz* informuje EF, aby automatycznie ustawić klucza Obcego o wartości null, pozostaje inną niż null, i SaveChanges zgłasza bez zapisywania

## <a name="delete-orphans-examples"></a>Usuń porzucone przykłady

Poniższy kod jest częścią [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , można pobrać i uruchomić. Przykład pokazuje, co się stanie dla każdego zachowanie dotyczące usuwania dla relacji wymagane i opcjonalne po relacji między nadrzędnym/jednostki i jego elementy podrzędne/dependents jest oddzielone. W tym przykładzie relacji jest oddzielone przez usunięcie zależności/elementy podrzędne (ogłoszeń) z właściwości nawigacji kolekcji jednostki/nadrzędnej (blog). Jednak to zachowanie jest taka sama Jeśli odwołanie z zależnych od ustawień lokalnych/podrzędny do podmiotu zabezpieczeń/element nadrzędny jest zamiast tego pustych.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Przejdźmy teraz przez poszczególnych odmian, aby zrozumieć, co się dzieje.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade z relacją wymagane lub opcjonalne

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

* Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego być oznaczony jako wartość null
  * W przypadku klucza Obcego nie dopuszcza wartości null, następnie wartość rzeczywista nie zmieni, nawet jeśli nie jest oznaczona jako wartości null
* SaveChanges wysyła usuwa dla zależności/dzieci (wpisów)
* Po zapisaniu zależności/elementy podrzędne (ogłoszeń) są odłączone od teraz zostały usunięte z bazy danych

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

* Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego być oznaczony jako wartość null
  * W przypadku klucza Obcego nie dopuszcza wartości null, następnie wartość rzeczywista nie zmieni, nawet jeśli nie jest oznaczona jako wartości null
* SaveChanges podejmuje próbę ustawienia wpisu klucza Obcego na wartość null, ale to nie powiodło się ponieważ klucza Obcego nie dopuszcza wartości null

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

* Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego być oznaczony jako wartość null
  * W przypadku klucza Obcego nie dopuszcza wartości null, następnie wartość rzeczywista nie zmieni, nawet jeśli nie jest oznaczona jako wartości null
* SaveChanges ustawia klucza Obcego z obu zależności/elementów podrzędnych (ogłoszeń) o wartości null
* Po zapisaniu zależności/elementy podrzędne (ogłoszeń) ma wartości null klucza Obcego, a ich odwołania do jednostki/nadrzędnych usunięte (blog) została usunięta.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict z relacją wymagane lub opcjonalne

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

* Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego być oznaczony jako wartość null
  * W przypadku klucza Obcego nie dopuszcza wartości null, następnie wartość rzeczywista nie zmieni, nawet jeśli nie jest oznaczona jako wartości null
* Ponieważ *Ogranicz* informuje EF, aby automatycznie ustawić klucza Obcego o wartości null, pozostaje inną niż null, i SaveChanges zgłasza bez zapisywania

## <a name="cascading-to-untracked-entities"></a>Kaskadowe nieśledzone jednostek

Gdy wywołujesz *SaveChanges*, reguły usuwanie kaskadowe zostaną zastosowane do dowolnej jednostki, które są śledzone przez kontekst. Jest to sytuacja we wszystkich przykładach pokazano powyżej, co jest Dlaczego SQL został wygenerowany można usunąć jednostki/element nadrzędny (blog) i wszystkie zależności/elementy podrzędne (ogłoszeń):

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Jeśli tylko podmiot zabezpieczeń jest ładowany — na przykład, gdy zapytania są tworzone dla bloga bez `Include(b => b.Posts)` też dołączyć wpisy — następnie SaveChanges będą generowane tylko SQL, aby usunąć jednostkę/element nadrzędny:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Zależności/elementy podrzędne (ogłoszeń) tylko zostaną usunięte, jeśli baza danych ma odpowiedni zachowania kaskadowe skonfigurowane. Jeśli używasz programu EF utworzyć bazę danych, zachowania kaskadowe będzie Instalatora dla Ciebie.
