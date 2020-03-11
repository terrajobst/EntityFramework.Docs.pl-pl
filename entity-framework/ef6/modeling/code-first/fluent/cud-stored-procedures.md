---
title: Code First Wstawianie, aktualizowanie i usuwanie procedur składowanych — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: bfc56671814aec1965ac054ff901297e5cdbbecb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419088"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>Code First procedury składowane INSERT, Update i DELETE
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.  

Domyślnie Code First skonfiguruje wszystkie jednostki do wykonywania poleceń INSERT, Update i DELETE przy użyciu bezpośredniego dostępu do tabeli. Począwszy od programu EF6, można skonfigurować model Code First, aby korzystał z procedur składowanych dla niektórych lub wszystkich jednostek w modelu.  

## <a name="basic-entity-mapping"></a>Podstawowe mapowanie jednostek  

Można zrezygnować z używania procedur składowanych do wstawiania, aktualizowania i usuwania przy użyciu interfejsu API Fluent.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

Spowoduje to, że Code First użyć pewnych konwencji do skompilowania oczekiwanego kształtu procedur składowanych w bazie danych.  

- Trzy procedury składowane o nazwie **\<type_name\>_Insert**, **\<TYPE_NAME**\>_Update i **\<** type_name\>_Delete (na przykład Blog_Insert, Blog_Update i Blog_Delete).  
- Nazwy parametrów odpowiadają nazwom właściwości.  
  > [!NOTE]
  > Jeśli używasz HasColumnName () lub atrybutu Column, aby zmienić nazwę kolumny dla danej właściwości, ta nazwa będzie używana dla parametrów zamiast nazwy właściwości.  
- **Procedura składowana INSERT** będzie zawierać parametr dla każdej właściwości, z wyjątkiem tych oznaczonych jako wygenerowany magazyn (tożsamość lub obliczona). Procedura składowana powinna zwrócić zestaw wyników z kolumną dla każdej wygenerowanej właściwości magazynu.  
- **Procedura składowana aktualizacji** będzie zawierać parametr dla każdej właściwości, z wyjątkiem tych oznaczonych wzorem wygenerowanym przez magazyn "obliczone". Niektóre tokeny współbieżności wymagają parametru pierwotnej wartości, zobacz sekcję *tokeny współbieżności* poniżej, aby uzyskać szczegółowe informacje. Procedura składowana powinna zwrócić zestaw wyników z kolumną dla każdej obliczonej właściwości.  
- **Procedura składowana usuwania** powinna mieć parametr dla wartości klucza jednostki (lub wielu parametrów, jeśli jednostka ma klucz złożony). Ponadto procedura Delete powinna również mieć parametry dla wszystkich niezależnych kluczy obcych skojarzenia w tabeli docelowej (relacje, które nie mają odpowiednich właściwości klucza obcego zadeklarowanych w jednostce). Niektóre tokeny współbieżności wymagają parametru pierwotnej wartości, zobacz sekcję *tokeny współbieżności* poniżej, aby uzyskać szczegółowe informacje.  

Użycie poniższej klasy jako przykładu:  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

Domyślne procedury składowane to:  

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

Można przesłonić część lub wszystkie elementy, które zostały skonfigurowane domyślnie.  

Można zmienić nazwę co najmniej jednej procedury składowanej. Ten przykład zmienia nazwę tylko procedury składowanej aktualizacji.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

W tym przykładzie zmieniane są wszystkie trzy procedury składowane.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

W tych przykładach wywołania są połączone łańcuchowo, ale można również użyć składni bloków lambda.  

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

Ten przykład zmienia nazwę parametru właściwości BlogId w procedurze składowanej Update.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

Te wywołania są w łańcuchu i mogą być zbudowane. Oto przykład, który zmienia nazwy wszystkich trzech procedur składowanych i ich parametrów.  

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

Możesz również zmienić nazwę kolumn w zestawie wyników zawierającym wartości wygenerowane przez bazę danych.  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>Relacje bez klucza obcego w klasie (skojarzenia niezależne)  

Gdy właściwość klucza obcego jest uwzględniona w definicji klasy, odpowiedni parametr można zmienić w taki sam sposób jak jakakolwiek inna właściwość. Gdy relacja istnieje bez właściwości klucza obcego w klasie, domyślną nazwą parametru jest **\<navigation_property_name\>_\<primary_key_name\>** .  

Na przykład następujące definicje klas spowodują, że Blog_BlogId parametr w procedurach składowanych, aby wstawić i zaktualizować wpisy.  

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

Parametry kluczy obcych, które nie są uwzględnione w klasie, można zmienić, dostarczając ścieżkę do właściwości klucza podstawowego do metody parametru.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

Jeśli nie masz właściwości nawigacji w jednostce zależnej (tj. Brak właściwości post. blog, a następnie można użyć metody Association do identyfikowania drugiego końca relacji, a następnie skonfigurowania parametrów, które odpowiadają każdej właściwości klucza.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>Tokeny współbieżności  

Procedury składowane Update i DELETE mogą również wymagać rozproszenia:  

- Jeśli jednostka zawiera tokeny współbieżności, procedura składowana może opcjonalnie mieć parametr wyjściowy, który zwraca liczbę wierszy zaktualizowanych/usuniętych (dotyczy wierszy). Taki parametr musi być skonfigurowany przy użyciu metody RowsAffectedParameter.  
Domyślnie EF używa wartości zwracanej z ExecuteNonQuery, aby określić liczbę wierszy, których to dotyczy. Określenie parametru danych wyjściowych, których to dotyczy, jest przydatne, jeśli w sproc jest wykonywana jakakolwiek logika, która spowoduje, że wartość zwracana przez ExecuteNonQuery jest niepoprawna (z perspektywy EF) na zakończenie wykonywania.  
- Dla każdego tokenu współbieżności będzie parametr o nazwie **\<property_name\>_original** (na przykład Timestamp_Original). Zostanie przeniesiona oryginalna wartość tej właściwości — wartość w przypadku zapytania z bazy danych.  
    - Tokeny współbieżności, które są obliczane przez bazę danych, takie jak sygnatury czasowe, będą miały tylko oryginalny parametr wartości.  
    - Nieobliczone właściwości, które są ustawione jako tokeny współbieżności, będą również miały parametr nowej wartości w procedurze Update. W tym przypadku są stosowane konwencje nazewnictwa już omówione dla nowych wartości. Przykładem takiego tokenu będzie użycie adresu URL blogu jako tokenu współbieżności, jest wymagana nowa wartość, ponieważ można ją zaktualizować do nowej wartości przez kod (w przeciwieństwie do tokenu sygnatury czasowej, który jest aktualizowany tylko przez bazę danych).  

Jest to przykładowa Klasa i Aktualizacja procedury składowanej z tokenem współbieżności sygnatur czasowych.  

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

Poniżej znajduje się przykładowa Klasa i Aktualizacja procedury składowanej z nieobliczanym tokenem współbieżności.  

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

Opcjonalnie możesz wprowadzić parametr dotyczący wierszy.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

W przypadku obliczanych tokenów współbieżności bazy danych — w której jest przenoszona tylko oryginalna wartość — można po prostu użyć mechanizmu zmiany nazwy parametru standardowego, aby zmienić nazwę parametru oryginalnej wartości.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

W przypadku nieobliczanych tokenów współbieżności — w przypadku przekazania zarówno oryginalnej, jak i nowej wartości — można użyć przeciążenia parametru, który umożliwia podanie nazwy dla każdego parametru.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>Wiele do wielu relacji  

Będziemy używać następujących klas jako przykładu w tej sekcji.  

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

Wiele relacji do wielu można mapować na procedury składowane z następującą składnią.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

Jeśli nie podano żadnej innej konfiguracji, domyślnie używany jest następujący kształt procedury składowanej.  

- Dwie procedury składowane o nazwie **\<type_one\>\<type_two\>_Insert** i\<type_one\> **\<type_two\>_Delete** (na przykład PostTag_Insert i PostTag_Delete).  
- Parametry będą wartościami klucza dla każdego typu. Nazwa każdego parametru, który jest **\<type_name\>_\<property_name\>** (na przykład Post_PostId i Tag_TagId).

Oto przykładowe procedury składowane INSERT i Update.  

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

Nazwy procedur i parametrów można skonfigurować w podobny sposób do procedur składowanych jednostek.  

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
