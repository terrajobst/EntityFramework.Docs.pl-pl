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
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="838b7-102">Code First procedury składowane INSERT, Update i DELETE</span><span class="sxs-lookup"><span data-stu-id="838b7-102">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="838b7-103">**Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="838b7-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="838b7-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="838b7-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="838b7-105">Domyślnie Code First skonfiguruje wszystkie jednostki do wykonywania poleceń INSERT, Update i DELETE przy użyciu bezpośredniego dostępu do tabeli.</span><span class="sxs-lookup"><span data-stu-id="838b7-105">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="838b7-106">Począwszy od programu EF6, można skonfigurować model Code First, aby korzystał z procedur składowanych dla niektórych lub wszystkich jednostek w modelu.</span><span class="sxs-lookup"><span data-stu-id="838b7-106">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="838b7-107">Podstawowe mapowanie jednostek</span><span class="sxs-lookup"><span data-stu-id="838b7-107">Basic Entity Mapping</span></span>  

<span data-ttu-id="838b7-108">Można zrezygnować z używania procedur składowanych do wstawiania, aktualizowania i usuwania przy użyciu interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="838b7-108">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="838b7-109">Spowoduje to, że Code First użyć pewnych konwencji do skompilowania oczekiwanego kształtu procedur składowanych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="838b7-109">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="838b7-110">Trzy procedury składowane o nazwie **\<type_name\>_Insert**, **\<TYPE_NAME**\>_Update i **\<** type_name\>_Delete (na przykład Blog_Insert, Blog_Update i Blog_Delete).</span><span class="sxs-lookup"><span data-stu-id="838b7-110">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (for example, Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="838b7-111">Nazwy parametrów odpowiadają nazwom właściwości.</span><span class="sxs-lookup"><span data-stu-id="838b7-111">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="838b7-112">Jeśli używasz HasColumnName () lub atrybutu Column, aby zmienić nazwę kolumny dla danej właściwości, ta nazwa będzie używana dla parametrów zamiast nazwy właściwości.</span><span class="sxs-lookup"><span data-stu-id="838b7-112">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="838b7-113">**Procedura składowana INSERT** będzie zawierać parametr dla każdej właściwości, z wyjątkiem tych oznaczonych jako wygenerowany magazyn (tożsamość lub obliczona).</span><span class="sxs-lookup"><span data-stu-id="838b7-113">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="838b7-114">Procedura składowana powinna zwrócić zestaw wyników z kolumną dla każdej wygenerowanej właściwości magazynu.</span><span class="sxs-lookup"><span data-stu-id="838b7-114">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="838b7-115">**Procedura składowana aktualizacji** będzie zawierać parametr dla każdej właściwości, z wyjątkiem tych oznaczonych wzorem wygenerowanym przez magazyn "obliczone".</span><span class="sxs-lookup"><span data-stu-id="838b7-115">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="838b7-116">Niektóre tokeny współbieżności wymagają parametru pierwotnej wartości, zobacz sekcję *tokeny współbieżności* poniżej, aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="838b7-116">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="838b7-117">Procedura składowana powinna zwrócić zestaw wyników z kolumną dla każdej obliczonej właściwości.</span><span class="sxs-lookup"><span data-stu-id="838b7-117">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="838b7-118">**Procedura składowana usuwania** powinna mieć parametr dla wartości klucza jednostki (lub wielu parametrów, jeśli jednostka ma klucz złożony).</span><span class="sxs-lookup"><span data-stu-id="838b7-118">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="838b7-119">Ponadto procedura Delete powinna również mieć parametry dla wszystkich niezależnych kluczy obcych skojarzenia w tabeli docelowej (relacje, które nie mają odpowiednich właściwości klucza obcego zadeklarowanych w jednostce).</span><span class="sxs-lookup"><span data-stu-id="838b7-119">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="838b7-120">Niektóre tokeny współbieżności wymagają parametru pierwotnej wartości, zobacz sekcję *tokeny współbieżności* poniżej, aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="838b7-120">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="838b7-121">Użycie poniższej klasy jako przykładu:</span><span class="sxs-lookup"><span data-stu-id="838b7-121">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="838b7-122">Domyślne procedury składowane to:</span><span class="sxs-lookup"><span data-stu-id="838b7-122">The default stored procedures would be:</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="838b7-123">Zastępowanie ustawień domyślnych</span><span class="sxs-lookup"><span data-stu-id="838b7-123">Overriding the Defaults</span></span>  

<span data-ttu-id="838b7-124">Można przesłonić część lub wszystkie elementy, które zostały skonfigurowane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="838b7-124">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="838b7-125">Można zmienić nazwę co najmniej jednej procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="838b7-125">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="838b7-126">Ten przykład zmienia nazwę tylko procedury składowanej aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="838b7-126">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="838b7-127">W tym przykładzie zmieniane są wszystkie trzy procedury składowane.</span><span class="sxs-lookup"><span data-stu-id="838b7-127">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="838b7-128">W tych przykładach wywołania są połączone łańcuchowo, ale można również użyć składni bloków lambda.</span><span class="sxs-lookup"><span data-stu-id="838b7-128">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

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

<span data-ttu-id="838b7-129">Ten przykład zmienia nazwę parametru właściwości BlogId w procedurze składowanej Update.</span><span class="sxs-lookup"><span data-stu-id="838b7-129">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="838b7-130">Te wywołania są w łańcuchu i mogą być zbudowane.</span><span class="sxs-lookup"><span data-stu-id="838b7-130">These calls are all chainable and composable.</span></span> <span data-ttu-id="838b7-131">Oto przykład, który zmienia nazwy wszystkich trzech procedur składowanych i ich parametrów.</span><span class="sxs-lookup"><span data-stu-id="838b7-131">Here is an example that renames all three stored procedures and their parameters.</span></span>  

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

<span data-ttu-id="838b7-132">Możesz również zmienić nazwę kolumn w zestawie wyników zawierającym wartości wygenerowane przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="838b7-132">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="838b7-133">Relacje bez klucza obcego w klasie (skojarzenia niezależne)</span><span class="sxs-lookup"><span data-stu-id="838b7-133">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="838b7-134">Gdy właściwość klucza obcego jest uwzględniona w definicji klasy, odpowiedni parametr można zmienić w taki sam sposób jak jakakolwiek inna właściwość.</span><span class="sxs-lookup"><span data-stu-id="838b7-134">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="838b7-135">Gdy relacja istnieje bez właściwości klucza obcego w klasie, domyślną nazwą parametru jest **\<navigation_property_name\>_\<primary_key_name\>** .</span><span class="sxs-lookup"><span data-stu-id="838b7-135">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="838b7-136">Na przykład następujące definicje klas spowodują, że Blog_BlogId parametr w procedurach składowanych, aby wstawić i zaktualizować wpisy.</span><span class="sxs-lookup"><span data-stu-id="838b7-136">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="838b7-137">Zastępowanie ustawień domyślnych</span><span class="sxs-lookup"><span data-stu-id="838b7-137">Overriding the Defaults</span></span>  

<span data-ttu-id="838b7-138">Parametry kluczy obcych, które nie są uwzględnione w klasie, można zmienić, dostarczając ścieżkę do właściwości klucza podstawowego do metody parametru.</span><span class="sxs-lookup"><span data-stu-id="838b7-138">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="838b7-139">Jeśli nie masz właściwości nawigacji w jednostce zależnej (tj.</span><span class="sxs-lookup"><span data-stu-id="838b7-139">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="838b7-140">Brak właściwości post. blog, a następnie można użyć metody Association do identyfikowania drugiego końca relacji, a następnie skonfigurowania parametrów, które odpowiadają każdej właściwości klucza.</span><span class="sxs-lookup"><span data-stu-id="838b7-140">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="838b7-141">Tokeny współbieżności</span><span class="sxs-lookup"><span data-stu-id="838b7-141">Concurrency Tokens</span></span>  

<span data-ttu-id="838b7-142">Procedury składowane Update i DELETE mogą również wymagać rozproszenia:</span><span class="sxs-lookup"><span data-stu-id="838b7-142">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="838b7-143">Jeśli jednostka zawiera tokeny współbieżności, procedura składowana może opcjonalnie mieć parametr wyjściowy, który zwraca liczbę wierszy zaktualizowanych/usuniętych (dotyczy wierszy).</span><span class="sxs-lookup"><span data-stu-id="838b7-143">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="838b7-144">Taki parametr musi być skonfigurowany przy użyciu metody RowsAffectedParameter.</span><span class="sxs-lookup"><span data-stu-id="838b7-144">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="838b7-145">Domyślnie EF używa wartości zwracanej z ExecuteNonQuery, aby określić liczbę wierszy, których to dotyczy.</span><span class="sxs-lookup"><span data-stu-id="838b7-145">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="838b7-146">Określenie parametru danych wyjściowych, których to dotyczy, jest przydatne, jeśli w sproc jest wykonywana jakakolwiek logika, która spowoduje, że wartość zwracana przez ExecuteNonQuery jest niepoprawna (z perspektywy EF) na zakończenie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="838b7-146">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="838b7-147">Dla każdego tokenu współbieżności będzie parametr o nazwie **\<property_name\>_original** (na przykład Timestamp_Original).</span><span class="sxs-lookup"><span data-stu-id="838b7-147">For each concurrency token there will be a parameter named **\<property_name\>_Original** (for example, Timestamp_Original ).</span></span> <span data-ttu-id="838b7-148">Zostanie przeniesiona oryginalna wartość tej właściwości — wartość w przypadku zapytania z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="838b7-148">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="838b7-149">Tokeny współbieżności, które są obliczane przez bazę danych, takie jak sygnatury czasowe, będą miały tylko oryginalny parametr wartości.</span><span class="sxs-lookup"><span data-stu-id="838b7-149">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="838b7-150">Nieobliczone właściwości, które są ustawione jako tokeny współbieżności, będą również miały parametr nowej wartości w procedurze Update.</span><span class="sxs-lookup"><span data-stu-id="838b7-150">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="838b7-151">W tym przypadku są stosowane konwencje nazewnictwa już omówione dla nowych wartości.</span><span class="sxs-lookup"><span data-stu-id="838b7-151">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="838b7-152">Przykładem takiego tokenu będzie użycie adresu URL blogu jako tokenu współbieżności, jest wymagana nowa wartość, ponieważ można ją zaktualizować do nowej wartości przez kod (w przeciwieństwie do tokenu sygnatury czasowej, który jest aktualizowany tylko przez bazę danych).</span><span class="sxs-lookup"><span data-stu-id="838b7-152">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="838b7-153">Jest to przykładowa Klasa i Aktualizacja procedury składowanej z tokenem współbieżności sygnatur czasowych.</span><span class="sxs-lookup"><span data-stu-id="838b7-153">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

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

<span data-ttu-id="838b7-154">Poniżej znajduje się przykładowa Klasa i Aktualizacja procedury składowanej z nieobliczanym tokenem współbieżności.</span><span class="sxs-lookup"><span data-stu-id="838b7-154">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="838b7-155">Zastępowanie ustawień domyślnych</span><span class="sxs-lookup"><span data-stu-id="838b7-155">Overriding the Defaults</span></span>  

<span data-ttu-id="838b7-156">Opcjonalnie możesz wprowadzić parametr dotyczący wierszy.</span><span class="sxs-lookup"><span data-stu-id="838b7-156">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="838b7-157">W przypadku obliczanych tokenów współbieżności bazy danych — w której jest przenoszona tylko oryginalna wartość — można po prostu użyć mechanizmu zmiany nazwy parametru standardowego, aby zmienić nazwę parametru oryginalnej wartości.</span><span class="sxs-lookup"><span data-stu-id="838b7-157">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="838b7-158">W przypadku nieobliczanych tokenów współbieżności — w przypadku przekazania zarówno oryginalnej, jak i nowej wartości — można użyć przeciążenia parametru, który umożliwia podanie nazwy dla każdego parametru.</span><span class="sxs-lookup"><span data-stu-id="838b7-158">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="838b7-159">Wiele do wielu relacji</span><span class="sxs-lookup"><span data-stu-id="838b7-159">Many to Many Relationships</span></span>  

<span data-ttu-id="838b7-160">Będziemy używać następujących klas jako przykładu w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="838b7-160">We’ll use the following classes as an example in this section.</span></span>  

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

<span data-ttu-id="838b7-161">Wiele relacji do wielu można mapować na procedury składowane z następującą składnią.</span><span class="sxs-lookup"><span data-stu-id="838b7-161">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="838b7-162">Jeśli nie podano żadnej innej konfiguracji, domyślnie używany jest następujący kształt procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="838b7-162">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="838b7-163">Dwie procedury składowane o nazwie **\<type_one\>\<type_two\>_Insert** i\<type_one\> **\<type_two\>_Delete** (na przykład PostTag_Insert i PostTag_Delete).</span><span class="sxs-lookup"><span data-stu-id="838b7-163">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (for example, PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="838b7-164">Parametry będą wartościami klucza dla każdego typu.</span><span class="sxs-lookup"><span data-stu-id="838b7-164">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="838b7-165">Nazwa każdego parametru, który jest **\<type_name\>_\<property_name\>** (na przykład Post_PostId i Tag_TagId).</span><span class="sxs-lookup"><span data-stu-id="838b7-165">The name of each parameter being **\<type_name\>_\<property_name\>** (for example, Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="838b7-166">Oto przykładowe procedury składowane INSERT i Update.</span><span class="sxs-lookup"><span data-stu-id="838b7-166">Here are example insert and update stored procedures.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="838b7-167">Zastępowanie ustawień domyślnych</span><span class="sxs-lookup"><span data-stu-id="838b7-167">Overriding the Defaults</span></span>  

<span data-ttu-id="838b7-168">Nazwy procedur i parametrów można skonfigurować w podobny sposób do procedur składowanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="838b7-168">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

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
