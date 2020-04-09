---
title: Kaskadowe usuwanie - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: 6e92b869d691d0224abf1997d9eb7ea035489c5d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417615"
---
# <a name="cascade-delete"></a>Usuwanie kaskadowe

Kaskadowe usuwanie jest często używane w terminologii bazy danych do opisania cechy, która umożliwia usunięcie wiersza, aby automatycznie wyzwolić usunięcie powiązanych wierszy. Ściśle powiązaną koncepcją również objęte ef core usuwania zachowań jest automatyczne usunięcie jednostki podrzędnej, gdy jest to relacja z elementem nadrzędnym został odcięty - jest to powszechnie znane jako "usuwanie sierot".

EF Core implementuje kilka różnych zachowań usuwania i umożliwia konfigurację zachowań usuwania poszczególnych relacji. EF Core implementuje również konwencje, które automatycznie konfigurują przydatne domyślne zachowania usuwania dla każdej relacji na podstawie [wymaganej relacji](../modeling/relationships.md#required-and-optional-relationships).

## <a name="delete-behaviors"></a>Usuwanie zachowań

Zachowania delete są zdefiniowane w *DeleteBehavior* typ wyliczania i mogą być przekazywane do *OnDelete płynnie* interfejsu API do kontrolowania, czy usunięcie jednostki podmiotu głównego/nadrzędnego lub zerwanie relacji do jednostek zależnych/podrzędnych powinny mieć wpływ uboczny na jednostki zależne/podrzędne.

Istnieją trzy akcje EF można podjąć, gdy jednostka podmiotu głównego/nadrzędnego jest usuwany lub relacji z elementami podrzędnymi jest zerwana:

* Dziecko/pozostające na utrzymaniu można usunąć
* Wartości klucza obcego dziecka można ustawić na wartość null
* Dziecko pozostaje niezmienione

> [!NOTE]  
> Zachowanie usuwania skonfigurowane w modelu EF Core jest stosowane tylko wtedy, gdy jednostka główna jest usuwana przy użyciu EF Core i encje zależne są ładowane do pamięci (czyli dla śledzonych zależności). Odpowiednie zachowanie kaskadowe musi być skonfigurowane w bazie danych, aby upewnić się, że dane, które nie są śledzone przez kontekst, mają zastosowaną niezbędną akcję. Jeśli używasz EF Core do utworzenia bazy danych, to zachowanie kaskadowe zostanie skonfigurowane dla Ciebie.

W przypadku drugiej powyższej akcji ustawienie wartości klucza obcego na null jest nieprawidłowe, jeśli klucz obcy nie jest nullable. (Klucz obcy nienastępny do null jest odpowiednikiem wymaganej relacji). W takich przypadkach EF Core śledzi, że właściwość klucza obcego została oznaczona jako null, dopóki SaveChanges jest wywoływana, w którym czasie wyjątek jest zgłaszany, ponieważ zmiana nie może być utrwalone do bazy danych. Jest to podobne do uzyskania naruszenia ograniczeń z bazy danych.

Istnieją cztery zachowania usuwania, wymienione w poniższych tabelach.

### <a name="optional-relationships"></a>Opcjonalne relacje

W przypadku relacji opcjonalnych (nullable foreign key) możliwe _jest_ zapisanie wartości klucza obcego null, co powoduje następujące efekty:

| Nazwa zachowania               | Wpływ na zależne/dziecko w pamięci    | Wpływ na zależne/podrzędne w bazie danych  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Kaskadowo**                 | Jednostki są usuwane                   | Jednostki są usuwane                   |
| **ClientSetNull** (domyślnie) | Właściwości klucza obcego są ustawione na wartość null | Brak                                   |
| **Setnull**                 | Właściwości klucza obcego są ustawione na wartość null | Właściwości klucza obcego są ustawione na wartość null |
| **Ograniczyć**                | Brak                                   | Brak                                   |

### <a name="required-relationships"></a>Wymagane relacje

Dla wymaganych relacji (klucz obcy nienastępny do null) _nie_ jest możliwe zapisanie wartości klucza obcego null, co powoduje następujące efekty:

| Nazwa zachowania         | Wpływ na zależne/dziecko w pamięci | Wpływ na zależne/podrzędne w bazie danych |
|:----------------------|:------------------------------------|:--------------------------------------|
| **Kaskada** (domyślna) | Jednostki są usuwane                | Jednostki są usuwane                  |
| **KlientSetNull**     | SaveChanges rzuca                  | Brak                                  |
| **Setnull**           | SaveChanges rzuca                  | SaveChanges rzuca                    |
| **Ograniczyć**          | Brak                                | Brak                                  |

W tabelach powyżej *None* może spowodować naruszenie ograniczeń. Na przykład jeśli jednostka podmiotu dublownik/dziecko zostanie usunięta, ale nie zostanie podjęta żadna akcja w celu zmiany klucza obcego zależności/dziecka, baza danych prawdopodobnie zostanie usunięta na SaveChanges z powodu naruszenia ograniczeń zagranicznych.

Na wysokim poziomie:

* Jeśli masz encje, które nie mogą istnieć bez nadrzędnego i chcesz, aby EF zajmował się automatycznym usuwaniem elementów podrzędnych, użyj *kaskadowego*.
  * Jednostki, które nie mogą istnieć bez jednostki nadrzędnej zwykle korzystają z wymaganych relacji, dla których *Kaskada* jest wartością domyślną.
* Jeśli masz jednostki, które mogą lub nie mogą mieć nadrzędnego, i chcesz EF dbać o unieważnienie klucza obcego dla Ciebie, a następnie użyć *ClientSetNull*
  * Jednostki, które mogą istnieć bez jednostki nadrzędnej zwykle korzystają z opcjonalnych relacji, dla których *ClientSetNull* jest wartością domyślną.
  * Jeśli chcesz, aby baza danych próbowała również propagować wartości null do podrzędnych kluczy obcych, nawet jeśli jednostka podrzędna nie jest załadowana, użyj *SetNull*. Należy jednak pamiętać, że baza danych musi obsługiwać to i konfigurowanie bazy danych w ten sposób może spowodować inne ograniczenia, które w praktyce często sprawia, że ta opcja niepraktyczne. Dlatego *SetNull* nie jest ustawieniem domyślnym.
* Jeśli nie chcesz, aby EF Core kiedykolwiek automatycznie usuwał encję lub automatycznie wyzerowywał klucz obcy, użyj opcji *Ogranicz*. Należy zauważyć, że wymaga to, aby kod przechowywać jednostki podrzędne i ich wartości klucza obcego w synchronizacji ręcznie w przeciwnym razie wyjątki ograniczenia zostaną odrzucone.

> [!NOTE]
> W EF Core, w przeciwieństwie do EF6, efekty kaskadowe nie zdarzają się natychmiast, ale zamiast tego tylko wtedy, gdy savechanges jest wywoływana.

> [!NOTE]  
> **Zmiany w EF Core 2.0:** W poprzednich wersjach *Restrict* spowodowałoby opcjonalne właściwości klucza obcego w śledzonych jednostek zależnych, które mają być ustawione na wartość null i było domyślne zachowanie usuwania dla relacji opcjonalnych. W EF Core 2.0 *ClientSetNull* został wprowadzony do reprezentowania tego zachowania i stał się domyślny dla relacji opcjonalnych. Zachowanie *Restrict* został dostosowany do nigdy nie mają żadnych skutków ubocznych na jednostki zależne.

## <a name="entity-deletion-examples"></a>Przykłady usuwania encji

Poniższy kod jest częścią [próbki,](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) które można pobrać i uruchomić. Przykład pokazuje, co się dzieje dla każdego zachowania usuwania dla relacji opcjonalnych i wymaganych, gdy jednostka nadrzędna jest usuwana.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Przejdźmy przez każdą odmianę, aby zrozumieć, co się dzieje.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>UsuńBehavior.Kaskada z wymaganą lub opcjonalną relacją

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
* Posty początkowo pozostają niezmienione, ponieważ kaskady nie zdarzają się do czasu zmiany SaveChanges
* SaveChanges wysyła usuwa zarówno na utrzymaniu /dzieci (posty), a następnie główny/nadrzędny (blog)
* Po zapisaniu wszystkie jednostki są odłączone, ponieważ zostały usunięte z bazy danych

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>UsuńBehavior.ClientSetNull lub DeleteBehavior.SetNull z wymaganą relacją

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
* Posty początkowo pozostają niezmienione, ponieważ kaskady nie zdarzają się do czasu zmiany SaveChanges
* SaveChanges próbuje ustawić wpis FK na null, ale to nie powiedzie się, ponieważ FK nie jest nullable

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>UsuńBehavior.ClientSetNull lub DeleteBehavior.SetNull z opcjonalną relacją

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
* Posty początkowo pozostają niezmienione, ponieważ kaskady nie zdarzają się do czasu zmiany SaveChanges
* SaveChanges próbuje ustawia FK obu utrzymaniu /podstawowych (postów) do wartości null przed usunięciem głównego/nadrzędnego (blog)
* Po zapisaniu główny/nadrzędny (blog) jest usuwany, ale osoby pozostające na utrzymaniu/dzieci (posty) są nadal śledzone
* Śledzone osoby zależne/podrzędne (posty) mają teraz wartości null FK, a ich odwołanie do usuniętego głównego zobowiązanego/nadrzędnego (bloga) zostało usunięte

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>UsuńBehavior.Ogranicz z wymaganą lub opcjonalną relacją

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
* Posty początkowo pozostają niezmienione, ponieważ kaskady nie zdarzają się do czasu zmiany SaveChanges
* Ponieważ *Restrict* nakazuje EF, aby nie ustawiał automatycznie wartości null, pozostaje bez wartości null, a SaveChanges rzuca bez zapisywania

## <a name="delete-orphans-examples"></a>Usuwanie przykładów sierot

Poniższy kod jest częścią [próbki,](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) które można pobrać i uruchomić. Przykład pokazuje, co się dzieje dla każdego zachowania usuwania dla relacji opcjonalnych i wymaganych, gdy relacja między nadrzędnym/głównym i jego podstawowych/zależnych jest zerwana. W tym przykładzie relacja jest zerwana przez usunięcie osób zależnych/elementów podrzędnych (wpisów) z właściwości nawigacji kolekcji w głównym/nadrzędnym (blogu). Jednak zachowanie jest taka sama, jeśli odwołanie od zależne/podrzędne do głównego zobowiązanego/nadrzędnego jest zamiast tego anulowane.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Przejdźmy przez każdą odmianę, aby zrozumieć, co się dzieje.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>UsuńBehavior.Kaskada z wymaganą lub opcjonalną relacją

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

* Wpisy są oznaczone jako Zmodyfikowane, ponieważ zerwanie relacji spowodowało, że FK została oznaczona jako null
  * Jeśli FK nie jest nullable, a następnie rzeczywista wartość nie zmieni się, nawet jeśli jest oznaczony jako null
* SaveChanges wysyła usuwa dla osób pozostających na utrzymaniu /dzieci (posty)
* Po zapisaniu osoby pozostające na utrzymaniu/dzieci (wpisy) są odłączone, ponieważ zostały usunięte z bazy danych

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>UsuńBehavior.ClientSetNull lub DeleteBehavior.SetNull z wymaganą relacją

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

* Wpisy są oznaczone jako Zmodyfikowane, ponieważ zerwanie relacji spowodowało, że FK została oznaczona jako null
  * Jeśli FK nie jest nullable, a następnie rzeczywista wartość nie zmieni się, nawet jeśli jest oznaczony jako null
* SaveChanges próbuje ustawić wpis FK na null, ale to nie powiedzie się, ponieważ FK nie jest nullable

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>UsuńBehavior.ClientSetNull lub DeleteBehavior.SetNull z opcjonalną relacją

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

* Wpisy są oznaczone jako Zmodyfikowane, ponieważ zerwanie relacji spowodowało, że FK została oznaczona jako null
  * Jeśli FK nie jest nullable, a następnie rzeczywista wartość nie zmieni się, nawet jeśli jest oznaczony jako null
* SaveChanges ustawia FK obu osób pozostających na utrzymaniu/dzieci (postów) na wartość null
* Po zapisaniu osoby pozostające na utrzymaniu/dzieci (posty) mają teraz wartości null FK, a ich odwołanie do usuniętego głównego zobowiązanego/nadrzędnego (bloga) zostało usunięte

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>UsuńBehavior.Ogranicz z wymaganą lub opcjonalną relacją

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

* Wpisy są oznaczone jako Zmodyfikowane, ponieważ zerwanie relacji spowodowało, że FK została oznaczona jako null
  * Jeśli FK nie jest nullable, a następnie rzeczywista wartość nie zmieni się, nawet jeśli jest oznaczony jako null
* Ponieważ *Restrict* nakazuje EF, aby nie ustawiał automatycznie wartości null, pozostaje bez wartości null, a SaveChanges rzuca bez zapisywania

## <a name="cascading-to-untracked-entities"></a>Kaskadowe do nieśledzonych encji

Podczas *wywoływania SaveChanges*reguły usuwania kaskadowego zostaną zastosowane do wszystkich jednostek, które są śledzone przez kontekst. Jest to sytuacja we wszystkich przykładach pokazanych powyżej, dlatego SQL został wygenerowany, aby usunąć zarówno głównego/nadrzędnego (blog) i wszystkich osób zależnych / elementów podrzędnych (postów):

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Jeśli tylko podmiot zabezpieczeń jest załadowany - na przykład, gdy `Include(b => b.Posts)` kwerenda jest dla bloga bez również wpisy - a następnie SaveChanges będzie generować tylko SQL, aby usunąć głównego/nadrzędnego:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Osoby pozostające na utrzymaniu/dzieci (wpisy) zostaną usunięte tylko wtedy, gdy baza danych ma skonfigurowane odpowiednie zachowanie kaskadowe. Jeśli do utworzenia bazy danych jest używana funkcja EF, to zachowanie kaskadowe zostanie skonfigurowane.
