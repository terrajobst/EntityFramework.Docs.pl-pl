---
title: Pierwsze wstawienie kodu, aktualizowanie i usuwanie procedur składowanych - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
caps.latest.revision: 3
ms.openlocfilehash: 1f100ed888abd98df83c80d0de2086cfb1ba7b4f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949082"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>Pierwsze wstawienie kodu, aktualizowanie i usuwanie procedur składowanych
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.  

Domyślnie program Code First służy do konfigurowania wszystkich jednostek, aby wykonać Wstawianie, aktualizowanie i usuwanie poleceń przy użyciu bezpośredniego dostępu do tabel. Uruchamianie platformy EF6 można skonfigurować przez model Code First i używanie procedur składowanych w przypadku niektórych lub wszystkich jednostek w modelu.  

## <a name="basic-entity-mapping"></a>Mapowania jednostki podstawowe  

Możesz zdecydować się na korzystanie z procedur składowanych do wstawiania, aktualizacji i usunąć za pomocą interfejsu API Fluent.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

W ten sposób spowoduje, że Code First na potrzeby niektóre konwencje kompilacji oczekiwanego kształt procedur składowanych w bazie danych.  

- Trzy procedury składowane o nazwie  **\<type_name\>_Wstaw**,  **\<type_name\>_aktualizuj** i  **\<type_ Nazwa\>_Usuń** (na przykład Blog_Insert i Blog_Update Blog_Delete).  
- Nazwy parametrów odpowiadają nazwy właściwości.  
  > [!NOTE]
  > Jeśli używasz HasColumnName() lub atrybut kolumny można zmienić nazwy kolumny dla danej właściwości ta nazwa jest używana dla parametrów zamiast nazwy właściwości.  
- **Procedura składowana insert** będzie mieć parametr dla każdej właściwości, z wyjątkiem tych oznaczone jako wygenerowane (tożsamość lub obliczona). Procedura składowana powinien zwrócić zestawu wyników z kolumny dla każdej właściwości wygenerowane.  
- **Procedura składowana aktualizacji** będzie mieć parametr dla każdej właściwości, z wyjątkiem tych oznaczone wzorzec wygenerowane "Obliczane". Niektóre tokeny współbieżności wymaga parametru dla oryginalnej wartości, zobacz *tokeny współbieżności* sekcji poniżej, aby uzyskać szczegółowe informacje. Procedura składowana powinien zwrócić zestawu wyników z kolumny dla każdej właściwości obliczanej.  
- **Usuń procedurę składowaną** powinien mieć parametr wartość klucza jednostki (lub wiele parametrów, jeśli jednostka ma klucz złożony). Ponadto procedury usuwania powinny mieć również parametry żadnych kluczy obcych skojarzenia niezależnie od tabeli docelowej (relacje, które nie mają odpowiednich właściwości klucza obcego zadeklarowane w jednostce). Niektóre tokeny współbieżności wymaga parametru dla oryginalnej wartości, zobacz *tokeny współbieżności* sekcji poniżej, aby uzyskać szczegółowe informacje.  

Na przykład, przy użyciu następującej klasy:  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

Wartość domyślna, który będzie procedur składowanych:  

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS BlogId
END
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId;
CREATE PROCEDURE [dbo].[Blog_Delete]  
  @BlogId int  
AS  
  DELETE FROM [dbo].[Blogs]
  WHERE BlogId = @BlogId
```  

### <a name="overriding-the-defaults"></a>Zastępowanie ustawień domyślnych  

Można zastąpić część lub całość co zostało skonfigurowane domyślnie.  

Można zmienić nazwę jednej lub kilku procedur składowanych. Ten przykład zmienia nazwę procedury składowanej aktualizacji tylko.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

Ten przykład zmienia nazwę wszystkich trzech procedur składowanych.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

W tych przykładach wywołania są ze sobą w sposób, ale możesz również użyć składni bloku lambda.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    {  
      s.Update(u => u.HasName("modify_blog"));  
      s.Delete(d => d.HasName("delete_blog"));  
      s.Insert(i => i.HasName("insert_blog"));  
    });
```  

Ten przykład zmienia nazwę parametru dla właściwości BlogId na procedury składowanej aktualizacji.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

Te wywołania są wszystkie chainable i konfigurowalna. Oto przykład, który zmienia nazwę wszystkich trzech procedur przechowywanych i ich parametrów.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")  
                   .Parameter(b => b.BlogId, "blog_id")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url"))  
     .Delete(d => d.HasName("delete_blog")  
                   .Parameter(b => b.BlogId, "blog_id"))  
     .Insert(i => i.HasName("insert_blog")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url")));
```  

Można również zmienić nazwy kolumn w zestawie wyników, zawierającą wartości z bazy danych, wygenerowane.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures(s =>
    s.Insert(i => i.Result(b => b.BlogId, "generated_blog_identity")));
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS generated_blog_id
END
```  

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>Relacje bez klucza obcego w klasie (niezależnie od skojarzeń)  

Gdy właściwość klucza obcego jest uwzględniony w definicji klasy, odpowiedniego parametru można zmieniać nazwy w taki sam sposób jak inne właściwości. Gdy istnieje relacja bez właściwości klucza obcego w klasie, domyślna nazwa parametru jest  **\<navigation_property_name\>_\<primary_key_name\>**.  

Na przykład następujące definicje klas spowodowałoby parametr Blog_BlogId jest oczekiwany w procedurach przechowywanych do wstawiania i aktualizowania wpisów.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }

  public List<Post> Posts { get; set; }  
}  

public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public Blog Blog { get; set; }  
}
```  

### <a name="overriding-the-defaults"></a>Zastępowanie ustawień domyślnych  

Możesz zmienić parametry dla kluczy obcych, które nie są uwzględnione w klasie, podając ścieżkę do właściwość klucza podstawowego do parametru metody.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

Jeśli nie masz właściwości nawigacji jednostki zależne (tj.) nie właściwości Post.Blog) możesz użyć metody skojarzenia, aby zidentyfikować drugiej stronie relacji, a następnie skonfigurować parametry, które odpowiadają każdej z właściwości kluczy.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>Tokeny współbieżności  

Aktualizowanie i usuwanie przechowywane procedury może być również konieczne przeciwdziałania współbieżności:  

- Jeśli jednostka zawiera tokeny współbieżności, procedury składowanej mogą opcjonalnie mieć parametr wyjściowy, która zwraca liczbę wierszy, zaktualizowane lub usunięte (wierszy). Taki parametr musi być skonfigurowany przy użyciu metody RowsAffectedParameter.  
Domyślnie EF używa wartość zwrotną z elementu ExecuteNonQuery, aby określić, ile wierszy została zmieniona. Określanie parametru wyjściowego odnośnych wierszy jest przydatne, jeśli po wykonaniu dowolnej logiki w swojej procedury sproc, które mogłyby spowodować wartość zwracaną ExecuteNonQuery jest niepoprawny (z perspektywy firmy EF) na końcu wykonywania.  
- Dla każdego współbieżności będzie token ma parametr o nazwie  **\<property_name\>_Original** (na przykład Timestamp_Original). To zostaną przekazane oryginalnej wartości tej właściwości – wartości po otrzymaniu kwerendy od bazy danych.  
    - Tokeny współbieżności, które są obliczane przez bazy danych — takich jak sygnatury czasowe — będzie miał tylko oryginalny parametru wartości.  
    - Obliczane inne niż właściwości, które są ustawione jako tokeny współbieżności Ponadto będziesz mieć parametr nową wartość w ramach procedury aktualizacji. Używa konwencji nazewnictwa już omówiono nowe wartości. Przykładem takiego tokenu będzie przy użyciu adresu URL blogu jako tokenem współbieżności, nowa wartość jest wymagana, ponieważ to mogą być aktualizowane na nową wartość w kodzie (w przeciwieństwie do token sygnatury czasowej, które są aktualizowane tylko przez bazę danych).  

Jest to przykład klasy i aktualizować procedury składowanej z tokenem współbieżności sygnatury czasowej.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
  [Timestamp]
  public byte[] Timestamp { get; set; }
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Timestamp_Original rowversion  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Timestamp] = @Timestamp_Original
```  

Oto przykład klasy i aktualizować procedury składowanej przy użyciu tokenu nieobliczaną współbieżności.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  [ConcurrencyCheck]
  public string Url { get; set; }  
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Url_Original nvarchar(max),
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Url] = @Url_Original
```  

### <a name="overriding-the-defaults"></a>Zastępowanie ustawień domyślnych  

Opcjonalnie możesz wprowadzić parametr odnośnych wierszy.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

Tokeny współbieżności bazy danych jest obliczana — gdzie oryginalną wartość jest przekazywana — po prostu umożliwia parametr standardowe, zmiana nazwy mechanizm Zmień nazwę parametru, aby uzyskać oryginalną wartość.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

Tokeny współbieżności nieobliczaną — gdzie zarówno oryginalne i nowe wartości są przekazywane — możesz użyć przeciążenia parametr, który pozwala podać nazwę dla każdego parametru.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>Wiele do wielu relacji  

Użyjemy następujących klas jako przykład w tej sekcji.  

``` csharp
public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public List<Tag> Tags { get; set; }  
}  

public class Tag  
{  
  public int TagId { get; set; }  
  public string TagName { get; set; }  

  public List<Post> Posts { get; set; }  
}
```  

Wiele do wielu relacji można mapować do procedur składowanych z następującą składnią.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

Jeśli zostanie podana żadna inna konfiguracja domyślnie jest używany następujący kształt procedury składowanej.  

- Dwie procedury składowane o nazwie  **\<type_one\>\<type_two\>_Wstaw** i  **\<type_one\>\<type_two \>_Usuń** (na przykład PostTag_Insert i PostTag_Delete).  
- Parametry będą kluczowe wartości dla każdego typu. Nazwa każdego parametru jest **\<type_name\>_\<property_name\>** (na przykład Post_PostId i Tag_TagId).

Poniżej przedstawiono przykład wstawiania i aktualizowania procedur składowanych.  

``` SQL
CREATE PROCEDURE [dbo].[PostTag_Insert]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  INSERT INTO [dbo].[Post_Tags] (Post_PostId, Tag_TagId)   
  VALUES (@Post_PostId, @Tag_TagId)
CREATE PROCEDURE [dbo].[PostTag_Delete]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  DELETE FROM [dbo].[Post_Tags]    
  WHERE Post_PostId = @Post_PostId AND Tag_TagId = @Tag_TagId
```  

### <a name="overriding-the-defaults"></a>Zastępowanie ustawień domyślnych  

Nazwy procedury i parametr można skonfigurować w sposób podobny do jednostki przechowywanej procedur.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.HasName("add_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id"))  
     .Delete(d => d.HasName("remove_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id")));
```  
