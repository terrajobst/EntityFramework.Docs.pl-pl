---
title: Testowanie za pomocą własnych testów wartości podwójnej precyzji - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 2158dc73585c2720e7293096b0478c73edf522d9
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490912"
---
# <a name="testing-with-your-own-test-doubles"></a><span data-ttu-id="23504-102">Testowanie za pomocą własnych testów, wartości podwójnej precyzji</span><span class="sxs-lookup"><span data-stu-id="23504-102">Testing with your own test doubles</span></span>
> [!NOTE]
> <span data-ttu-id="23504-103">**EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="23504-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="23504-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="23504-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="23504-105">Podczas pisania testów dla aplikacji często jest pożądane, aby uniknąć osiągnięcia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="23504-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="23504-106">Entity Framework można to osiągnąć, tworząc kontekst — w przypadku zachowanie zdefiniowane przez testy — korzystającej z danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="23504-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="23504-107">Opcje tworzenia testu wartości podwójnej precyzji</span><span class="sxs-lookup"><span data-stu-id="23504-107">Options for creating test doubles</span></span>  

<span data-ttu-id="23504-108">Istnieją dwa różne podejścia, które mogą służyć do tworzenia w pamięci wersję kontekstu.</span><span class="sxs-lookup"><span data-stu-id="23504-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="23504-109">**Tworzenie własnych testów, wartości podwójnej precyzji** — ta strategia polega na pisanie własnego kontekstu i DbSets implementacji w pamięci.</span><span class="sxs-lookup"><span data-stu-id="23504-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="23504-110">Zapewnia wysoki poziom kontroli nad jak zachowują się klasy, ale może obejmować pisanie i będącej właścicielem rozsądnym kodu.</span><span class="sxs-lookup"><span data-stu-id="23504-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="23504-111">**Umożliwia tworzenie testów wartości podwójnej precyzji pozorowania framework** — za pomocą pozorowania framework (na przykład Moq) może mieć implementacji w pamięci, kontekstu i zestawy tworzone dynamicznie w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="23504-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="23504-112">W tym artykule poradzi sobie z tworzeniem własnych testów double.</span><span class="sxs-lookup"><span data-stu-id="23504-112">This article will deal with creating your own test double.</span></span> <span data-ttu-id="23504-113">Uzyskać informacje o używaniu pozorowania framework zobacz [testowanie za pomocą struktury pozorowanie](mocking.md).</span><span class="sxs-lookup"><span data-stu-id="23504-113">For information on using a mocking framework see [Testing with a Mocking Framework](mocking.md).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="23504-114">Testowanie za pomocą wersji pre-EF6</span><span class="sxs-lookup"><span data-stu-id="23504-114">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="23504-115">Kod przedstawiony w tym artykule jest zgodny z platformy EF6.</span><span class="sxs-lookup"><span data-stu-id="23504-115">The code shown in this article is compatible with EF6.</span></span> <span data-ttu-id="23504-116">Testowanie za pomocą EF5 i starszych wersji dla [testowanie za pomocą kontekstu fałszywe](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="23504-116">For testing with EF5 and earlier version see [Testing with a Fake Context](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="23504-117">Ograniczenia dotyczące wartości podwójnej precyzji EF testu w pamięci</span><span class="sxs-lookup"><span data-stu-id="23504-117">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="23504-118">Test w pamięci wartości podwójnej precyzji, może być dobrym sposobem zapewnienia poziomu zasięg bitów aplikacji korzystających z programu EF testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="23504-118">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="23504-119">Jednak w ten sposób używasz LINQ to Objects do wykonywania zapytań dotyczących danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="23504-119">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="23504-120">Może to spowodować, że inaczej niż tłumaczenie zapytań SQL, która jest uruchamiana względem bazy danych przy użyciu programu EF firmy dostawcy LINQ (LINQ to Entities).</span><span class="sxs-lookup"><span data-stu-id="23504-120">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="23504-121">Przykładem takiej różnicy Trwa ładowanie powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="23504-121">One example of such a difference is loading related data.</span></span> <span data-ttu-id="23504-122">Jeśli tworzysz szereg blogi każdy z powiązanymi wpisy, a następnie korzystając z danych w pamięci dla każdego bloga zawsze zostaną załadowane pokrewnych wpisów.</span><span class="sxs-lookup"><span data-stu-id="23504-122">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="23504-123">Jednak podczas uruchamiania w bazie danych dane tylko zostanie załadowany Jeśli używana jest metoda Include.</span><span class="sxs-lookup"><span data-stu-id="23504-123">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="23504-124">Z tego powodu zaleca się zawsze zawierać pewien stopień end-to-end testy (oprócz testy jednostkowe) w celu zapewnienia działania usługi aplikacji poprawnie względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="23504-124">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="23504-125">Zgodnie z tego artykułu</span><span class="sxs-lookup"><span data-stu-id="23504-125">Following along with this article</span></span>  

<span data-ttu-id="23504-126">Ten artykuł zawiera kompletny kod ofert, kopiowane do programu Visual Studio, aby z niego skorzystać, jeśli chcesz.</span><span class="sxs-lookup"><span data-stu-id="23504-126">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="23504-127">Najłatwiej utworzyć **projektu testu jednostkowego** i należy do obiektu docelowego **.NET Framework 4.5** do wykonania w sekcjach, korzystających z async.</span><span class="sxs-lookup"><span data-stu-id="23504-127">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="creating-a-context-interface"></a><span data-ttu-id="23504-128">Tworzenie interfejsu kontekstu</span><span class="sxs-lookup"><span data-stu-id="23504-128">Creating a context interface</span></span>  

<span data-ttu-id="23504-129">Zamierzamy Przyjrzyj się testowanie to usługa, która korzysta z programu EF modelu.</span><span class="sxs-lookup"><span data-stu-id="23504-129">We're going to look at testing a service that makes use of an EF model.</span></span> <span data-ttu-id="23504-130">Aby można było zastąpić nasz kontekst EF wersją w pamięci do testowania, zdefiniujemy interfejs zostaną imeplement nasz kontekst EF (oraz jej w pamięci podwójnej precyzji).</span><span class="sxs-lookup"><span data-stu-id="23504-130">In order to be able to replace our EF context with an in-memory version for testing, we'll define an interface that our EF context (and it's in-memory double) will imeplement.</span></span>  

<span data-ttu-id="23504-131">Usługi, którą użyjemy do przetestowania spowoduje zapytania i modyfikowanie danych za pomocą właściwości DbSet nasz kontekst i również wywołać funkcję SaveChanges wypychania zmian do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="23504-131">The service we are going to test will query and modify data using the DbSet properties of our context and also call SaveChanges to push changes to the database.</span></span> <span data-ttu-id="23504-132">Możemy więc dołączania te elementy członkowskie w interfejsie.</span><span class="sxs-lookup"><span data-stu-id="23504-132">So we're including these members on the interface.</span></span>  

``` csharp
using System.Data.Entity;

namespace TestingDemo
{
    public interface IBloggingContext
    {
        DbSet<Blog> Blogs { get; }
        DbSet<Post> Posts { get; }
        int SaveChanges();
    }
}
```  

## <a name="the-ef-model"></a><span data-ttu-id="23504-133">Modelu platformy EF</span><span class="sxs-lookup"><span data-stu-id="23504-133">The EF model</span></span>  

<span data-ttu-id="23504-134">Usługa użyjemy do przetestowania, korzysta z programu EF modelu składa się z BloggingContext i klas blogu i Post.</span><span class="sxs-lookup"><span data-stu-id="23504-134">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="23504-135">Ten kod został wygenerowany przez projektanta programu EF lub model Code First.</span><span class="sxs-lookup"><span data-stu-id="23504-135">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext, IBloggingContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }
        public string Url { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }
}
```  

### <a name="implementing-the-context-interface-with-the-ef-designer"></a><span data-ttu-id="23504-136">Implementowanie interfejsu kontekstu za pomocą projektanta EF</span><span class="sxs-lookup"><span data-stu-id="23504-136">Implementing the context interface with the EF Designer</span></span>  

<span data-ttu-id="23504-137">Należy pamiętać, że nasz kontekst implementuje interfejs IBloggingContext.</span><span class="sxs-lookup"><span data-stu-id="23504-137">Note that our context implements the IBloggingContext interface.</span></span>  

<span data-ttu-id="23504-138">Jeśli używasz Code First można edytować kontekst bezpośrednio do implementacji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="23504-138">If you are using Code First then you can edit your context directly to implement the interface.</span></span> <span data-ttu-id="23504-139">Jeśli używasz projektancie platformy EF następnie należy edytować szablon T4, który generuje kontekstu.</span><span class="sxs-lookup"><span data-stu-id="23504-139">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="23504-140">Otwórz \<nazwa_modelu\>. Plik context.TT, który jest zagnieżdżony w przypadku pliku edmx, znajdź następujący fragment kodu i Dodaj w interfejsie, jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="23504-140">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the interface as shown.</span></span>  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="23504-141">Usługi w celu zbadania</span><span class="sxs-lookup"><span data-stu-id="23504-141">Service to be tested</span></span>  

<span data-ttu-id="23504-142">Aby zademonstrować, testowanie za pomocą testu w pamięci wartości podwójnej precyzji zamierzamy zapisywać dla BlogService kilka testów.</span><span class="sxs-lookup"><span data-stu-id="23504-142">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="23504-143">Usługa jest w stanie tworzenie nowych blogów (AddBlog) i zwraca wszystkie blogi uporządkowane według nazwy (GetAllBlogs).</span><span class="sxs-lookup"><span data-stu-id="23504-143">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="23504-144">Oprócz GetAllBlogs udostępniliśmy również metodę, która asynchronicznie pobierze wszystkie blogi uporządkowane według nazwy (GetAllBlogsAsync).</span><span class="sxs-lookup"><span data-stu-id="23504-144">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private IBloggingContext _context;

        public BlogService(IBloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = new Blog { Name = name, Url = url };
            _context.Blogs.Add(blog);
            _context.SaveChanges();

            return blog;
        }

        public List<Blog> GetAllBlogs()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return query.ToList();
        }

        public async Task<List<Blog>> GetAllBlogsAsync()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return await query.ToListAsync();
        }
    }
}
```  

<span data-ttu-id="23504-145"><a name="creating-the-in-memory-test-doubles"/> ## Podwaja się tworzenie testu w pamięci</span><span class="sxs-lookup"><span data-stu-id="23504-145"><a name="creating-the-in-memory-test-doubles"/> ## Creating the in-memory test doubles</span></span>  

<span data-ttu-id="23504-146">Teraz gdy mamy rzeczywiste modelu platformy EF i usługa, która służy nadszedł czas na tworzenie testów w pamięci double, firma Microsoft można używać do testowania.</span><span class="sxs-lookup"><span data-stu-id="23504-146">Now that we have the real EF model and the service that can use it, it's time to create the in-memory test double that we can use for testing.</span></span> <span data-ttu-id="23504-147">Utworzyliśmy testu TestContext double na nasz kontekst.</span><span class="sxs-lookup"><span data-stu-id="23504-147">We've created a TestContext test double for our context.</span></span> <span data-ttu-id="23504-148">W wartości podwójnej precyzji testu, otrzymujemy wybierz zachowanie, chcemy, aby można było obsługiwać testy użyjemy do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="23504-148">In test doubles we get to choose the behavior we want in order to support the tests we are going to run.</span></span> <span data-ttu-id="23504-149">W tym przykładzie po prostu wychwycimy liczba przypadków, gdy jest wywoływana SaveChanges, ale może zawierać dowolną logikę wymaganą wymaganego do weryfikacji scenariuszy, które testujesz.</span><span class="sxs-lookup"><span data-stu-id="23504-149">In this example we're just capturing the number of times SaveChanges is called, but you can include whatever logic is needed to verify the scenario you are testing.</span></span>  

<span data-ttu-id="23504-150">Utworzyliśmy również TestDbSet, zapewniająca DbSet implementację w pamięci.</span><span class="sxs-lookup"><span data-stu-id="23504-150">We've also created a TestDbSet that provides an in-memory implementation of DbSet.</span></span> <span data-ttu-id="23504-151">Udostępniamy pełny implementacja dla wszystkich metod na DbSet (z wyjątkiem znaleźć), ale musisz wdrożyć elementów członkowskich, które będą używać Twojego scenariusza testu.</span><span class="sxs-lookup"><span data-stu-id="23504-151">We've provided a complete implemention for all the methods on DbSet (except for Find), but you only need to implement the members that your test scenario will use.</span></span>  

<span data-ttu-id="23504-152">TestDbSet sprawia, że użycie niektórych innych klas infrastruktury, które wprowadziliśmy, aby upewnić się, że zapytania asynchroniczne mogą być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="23504-152">TestDbSet makes use of some other infrastructure classes that we've included to ensure that async queries can be processed.</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class TestContext : IBloggingContext
    {
        public TestContext()
        {
            this.Blogs = new TestDbSet<Blog>();
            this.Posts = new TestDbSet<Post>();
        }

        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
        public int SaveChangesCount { get; private set; }
        public int SaveChanges()
        {
            this.SaveChangesCount++;
            return 1;
        }
    }

    public class TestDbSet<TEntity> : DbSet<TEntity>, IQueryable, IEnumerable<TEntity>, IDbAsyncEnumerable<TEntity>
        where TEntity : class
    {
        ObservableCollection<TEntity> _data;
        IQueryable _query;

        public TestDbSet()
        {
            _data = new ObservableCollection<TEntity>();
            _query = _data.AsQueryable();
        }

        public override TEntity Add(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Remove(TEntity item)
        {
            _data.Remove(item);
            return item;
        }

        public override TEntity Attach(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Create()
        {
            return Activator.CreateInstance<TEntity>();
        }

        public override TDerivedEntity Create<TDerivedEntity>()
        {
            return Activator.CreateInstance<TDerivedEntity>();
        }

        public override ObservableCollection<TEntity> Local
        {
            get { return _data; }
        }

        Type IQueryable.ElementType
        {
            get { return _query.ElementType; }
        }

        Expression IQueryable.Expression
        {
            get { return _query.Expression; }
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<TEntity>(_query.Provider); }
        }

        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IEnumerator<TEntity> IEnumerable<TEntity>.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IDbAsyncEnumerator<TEntity> IDbAsyncEnumerable<TEntity>.GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<TEntity>(_data.GetEnumerator());
        }
    }

    internal class TestDbAsyncQueryProvider<TEntity> : IDbAsyncQueryProvider
    {
        private readonly IQueryProvider _inner;

        internal TestDbAsyncQueryProvider(IQueryProvider inner)
        {
            _inner = inner;
        }

        public IQueryable CreateQuery(Expression expression)
        {
            return new TestDbAsyncEnumerable<TEntity>(expression);
        }

        public IQueryable<TElement> CreateQuery<TElement>(Expression expression)
        {
            return new TestDbAsyncEnumerable<TElement>(expression);
        }

        public object Execute(Expression expression)
        {
            return _inner.Execute(expression);
        }

        public TResult Execute<TResult>(Expression expression)
        {
            return _inner.Execute<TResult>(expression);
        }

        public Task<object> ExecuteAsync(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute(expression));
        }

        public Task<TResult> ExecuteAsync<TResult>(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute<TResult>(expression));
        }
    }

    internal class TestDbAsyncEnumerable<T> : EnumerableQuery<T>, IDbAsyncEnumerable<T>, IQueryable<T>
    {
        public TestDbAsyncEnumerable(IEnumerable<T> enumerable)
            : base(enumerable)
        { }

        public TestDbAsyncEnumerable(Expression expression)
            : base(expression)
        { }

        public IDbAsyncEnumerator<T> GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<T>(this.AsEnumerable().GetEnumerator());
        }

        IDbAsyncEnumerator IDbAsyncEnumerable.GetAsyncEnumerator()
        {
            return GetAsyncEnumerator();
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<T>(this); }
        }
    }

    internal class TestDbAsyncEnumerator<T> : IDbAsyncEnumerator<T>
    {
        private readonly IEnumerator<T> _inner;

        public TestDbAsyncEnumerator(IEnumerator<T> inner)
        {
            _inner = inner;
        }

        public void Dispose()
        {
            _inner.Dispose();
        }

        public Task<bool> MoveNextAsync(CancellationToken cancellationToken)
        {
            return Task.FromResult(_inner.MoveNext());
        }

        public T Current
        {
            get { return _inner.Current; }
        }

        object IDbAsyncEnumerator.Current
        {
            get { return Current; }
        }
    }
}
```  

### <a name="implementing-find"></a><span data-ttu-id="23504-153">Implementowanie wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="23504-153">Implementing Find</span></span>  

<span data-ttu-id="23504-154">Metoda wyszukiwania jest trudne do zaimplementowania w ogólny sposób.</span><span class="sxs-lookup"><span data-stu-id="23504-154">The Find method is difficult to implement in a generic fashion.</span></span> <span data-ttu-id="23504-155">Jeśli musisz przetestować kod, który sprawia, że użyj metody Find, który najłatwiej utworzyć test znaleźć DbSet dla każdego z typów jednostek, które muszą być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="23504-155">If you need to test code that makes use of the Find method it is easiest to create a test DbSet for each of the entity types that need to support find.</span></span> <span data-ttu-id="23504-156">Następnie można napisać logikę, aby znaleźć konkretny typ jednostki, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="23504-156">You can then write logic to find that particular type of entity, as shown below.</span></span>  

``` csharp
using System.Linq;

namespace TestingDemo
{
    class TestBlogDbSet : TestDbSet<Blog>
    {
        public override Blog Find(params object[] keyValues)
        {
            var id = (int)keyValues.Single();
            return this.SingleOrDefault(b => b.BlogId == id);
        }
    }
}
```  

## <a name="writing-some-tests"></a><span data-ttu-id="23504-157">Pisanie niektórych testów</span><span class="sxs-lookup"><span data-stu-id="23504-157">Writing some tests</span></span>  

<span data-ttu-id="23504-158">To wszystko, co należy zrobić, aby uruchomić testy.</span><span class="sxs-lookup"><span data-stu-id="23504-158">That’s all we need to do to start testing.</span></span> <span data-ttu-id="23504-159">Następujący test tworzy TestContext, a następnie usługi na podstawie tego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="23504-159">The following test creates a TestContext and then a service based on this context.</span></span> <span data-ttu-id="23504-160">Usługa jest następnie używany do utworzenia nowego bloga — przy użyciu metody AddBlog.</span><span class="sxs-lookup"><span data-stu-id="23504-160">The service is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="23504-161">Ponadto ten test sprawdza, czy usługa dodano nowego bloga kontekstu blogi właściwości i wywołuje SaveChanges w kontekście.</span><span class="sxs-lookup"><span data-stu-id="23504-161">Finally, the test verifies that the service added a new Blog to the context's Blogs property and called SaveChanges on the context.</span></span>  

<span data-ttu-id="23504-162">To przykładowe typy rzeczy, które można przetestować za pomocą podwójnego testu w pamięci i można dostosować logiki wartości podwójnej precyzji testów i weryfikacji zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="23504-162">This is just an example of the types of things you can test with an in-memory test double and you can adjust the logic of the test doubles and the verification to meet your requirements.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var context = new TestContext();

            var service = new BlogService(context);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            Assert.AreEqual(1, context.Blogs.Count());
            Assert.AreEqual("ADO.NET Blog", context.Blogs.Single().Name);
            Assert.AreEqual("http://blogs.msdn.com/adonet", context.Blogs.Single().Url);
            Assert.AreEqual(1, context.SaveChangesCount);
        }
    }
}
```  

<span data-ttu-id="23504-163">Oto inny przykład testu — tym razem taki, który wykonuje kwerendę.</span><span class="sxs-lookup"><span data-stu-id="23504-163">Here is another example of a test - this time one that performs a query.</span></span> <span data-ttu-id="23504-164">Uruchamia test, tworząc kontekstu testu z danymi w jego blogu właściwość — należy pamiętać, że dane nie są w kolejności alfabetycznej.</span><span class="sxs-lookup"><span data-stu-id="23504-164">The test starts by creating a test context with some data in its Blog property - note that the data is not in alphabetical order.</span></span> <span data-ttu-id="23504-165">Firma Microsoft można utworzyć BlogService, na podstawie naszych kontekstu testu i upewnij się, że dane, które firma Microsoft wrócić z GetAllBlogs są uporządkowane według nazwy.</span><span class="sxs-lookup"><span data-stu-id="23504-165">We can then create a BlogService based on our test context and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

<span data-ttu-id="23504-166">Ponadto będziemy pisać jeden więcej test, który korzysta z naszego metody asynchronicznej, upewnij się, że infrastruktura async jest dostępna w [TestDbSet](#creating-the-in-memory-test-doubles) działa.</span><span class="sxs-lookup"><span data-stu-id="23504-166">Finally, we'll write one more test that uses our async method to ensure that the async infrastructure we included in [TestDbSet](#creating-the-in-memory-test-doubles) is working.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    [TestClass]
    public class AsyncQueryTests
    {
        [TestMethod]
        public async Task GetAllBlogsAsync_orders_by_name()
        {
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
