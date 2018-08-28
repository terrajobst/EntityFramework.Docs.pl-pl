---
title: Testowanie za pomocą własnych testów wartości podwójnej precyzji - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: fa63c483e0a55b6cbd43382f68ab9592f3c1768b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997220"
---
# <a name="testing-with-your-own-test-doubles"></a>Testowanie za pomocą własnych testów, wartości podwójnej precyzji
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.  

Podczas pisania testów dla aplikacji często jest pożądane, aby uniknąć osiągnięcia bazy danych.  Entity Framework można to osiągnąć, tworząc kontekst — w przypadku zachowanie zdefiniowane przez testy — korzystającej z danych w pamięci.  

## <a name="options-for-creating-test-doubles"></a>Opcje tworzenia testu wartości podwójnej precyzji  

Istnieją dwa różne podejścia, które mogą służyć do tworzenia w pamięci wersję kontekstu.  

- **Tworzenie własnych testów, wartości podwójnej precyzji** — ta strategia polega na pisanie własnego kontekstu i DbSets implementacji w pamięci. Zapewnia wysoki poziom kontroli nad jak zachowują się klasy, ale może obejmować pisanie i będącej właścicielem rozsądnym kodu.  
- **Umożliwia tworzenie testów wartości podwójnej precyzji pozorowania framework** — za pomocą pozorowania framework (na przykład Moq) może mieć implementacji w pamięci, kontekstu i zestawy tworzone dynamicznie w czasie wykonywania.  

W tym artykule poradzi sobie z tworzeniem własnych testów double. Uzyskać informacje o używaniu pozorowania framework zobacz [testowanie za pomocą struktury pozorowanie](mocking.md).  

## <a name="testing-with-pre-ef6-versions"></a>Testowanie za pomocą wersji pre-EF6  

Kod przedstawiony w tym artykule jest zgodny z platformy EF6. Testowanie za pomocą EF5 i starszych wersji dla [testowanie za pomocą kontekstu fałszywe](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Ograniczenia dotyczące wartości podwójnej precyzji EF testu w pamięci  

Test w pamięci wartości podwójnej precyzji, może być dobrym sposobem zapewnienia poziomu zasięg bitów aplikacji korzystających z programu EF testów jednostkowych. Jednak w ten sposób używasz LINQ to Objects do wykonywania zapytań dotyczących danych w pamięci. Może to spowodować, że inaczej niż tłumaczenie zapytań SQL, która jest uruchamiana względem bazy danych przy użyciu programu EF firmy dostawcy LINQ (LINQ to Entities).  

Przykładem takiej różnicy Trwa ładowanie powiązanych danych. Jeśli tworzysz szereg blogi każdy z powiązanymi wpisy, a następnie korzystając z danych w pamięci dla każdego bloga zawsze zostaną załadowane pokrewnych wpisów. Jednak podczas uruchamiania w bazie danych dane tylko zostanie załadowany Jeśli używana jest metoda Include.  

Z tego powodu zaleca się zawsze zawierać pewien stopień end-to-end testy (oprócz testy jednostkowe) w celu zapewnienia działania usługi aplikacji poprawnie względem bazy danych.  

## <a name="following-along-with-this-article"></a>Zgodnie z tego artykułu  

Ten artykuł zawiera kompletny kod ofert, kopiowane do programu Visual Studio, aby z niego skorzystać, jeśli chcesz. Najłatwiej utworzyć **projektu testu jednostkowego** i należy do obiektu docelowego **.NET Framework 4.5** do wykonania w sekcjach, korzystających z async.  

## <a name="creating-a-context-interface"></a>Tworzenie interfejsu kontekstu  

Zamierzamy Przyjrzyj się testowanie to usługa, która korzysta z programu EF modelu. Aby można było zastąpić nasz kontekst EF wersją w pamięci do testowania, zdefiniujemy interfejs zostaną imeplement nasz kontekst EF (oraz jej w pamięci podwójnej precyzji).  

Usługi, którą użyjemy do przetestowania spowoduje zapytania i modyfikowanie danych za pomocą właściwości DbSet nasz kontekst i również wywołać funkcję SaveChanges wypychania zmian do bazy danych. Możemy więc dołączania te elementy członkowskie w interfejsie.  

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

## <a name="the-ef-model"></a>Modelu platformy EF  

Usługa użyjemy do przetestowania, korzysta z programu EF modelu składa się z BloggingContext i klas blogu i Post. Ten kod został wygenerowany przez projektanta programu EF lub model Code First.  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a>Implementowanie interfejsu kontekstu za pomocą projektanta EF  

Należy pamiętać, że nasz kontekst implementuje interfejs IBloggingContext.  

Jeśli używasz Code First można edytować kontekst bezpośrednio do implementacji interfejsu. Jeśli używasz projektancie platformy EF następnie należy edytować szablon T4, który generuje kontekstu. Otwórz \<nazwa_modelu\>. Plik context.TT, który jest zagnieżdżony w przypadku pliku edmx, znajdź następujący fragment kodu i Dodaj w interfejsie, jak pokazano.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a>Usługi w celu zbadania  

Aby zademonstrować, testowanie za pomocą testu w pamięci wartości podwójnej precyzji zamierzamy zapisywać dla BlogService kilka testów. Usługa jest w stanie tworzenie nowych blogów (AddBlog) i zwraca wszystkie blogi uporządkowane według nazwy (GetAllBlogs). Oprócz GetAllBlogs udostępniliśmy również metodę, która asynchronicznie pobierze wszystkie blogi uporządkowane według nazwy (GetAllBlogsAsync).  

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

<a name="creating-the-in-memory-test-doubles"/> ## Podwaja się tworzenie testu w pamięci  

Teraz gdy mamy rzeczywiste modelu platformy EF i usługa, która służy nadszedł czas na tworzenie testów w pamięci double, firma Microsoft można używać do testowania. Utworzyliśmy testu TestContext double na nasz kontekst. W wartości podwójnej precyzji testu, otrzymujemy wybierz zachowanie, chcemy, aby można było obsługiwać testy użyjemy do uruchomienia. W tym przykładzie po prostu wychwycimy liczba przypadków, gdy jest wywoływana SaveChanges, ale może zawierać dowolną logikę wymaganą wymaganego do weryfikacji scenariuszy, które testujesz.  

Utworzyliśmy również TestDbSet, zapewniająca DbSet implementację w pamięci. Udostępniamy pełny implementacja dla wszystkich metod na DbSet (z wyjątkiem znaleźć), ale musisz wdrożyć elementów członkowskich, które będą używać Twojego scenariusza testu.  

TestDbSet sprawia, że użycie niektórych innych klas infrastruktury, które wprowadziliśmy, aby upewnić się, że zapytania asynchroniczne mogą być przetwarzane.  

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

### <a name="implementing-find"></a>Implementowanie wyszukiwania  

Metoda wyszukiwania jest trudne do zaimplementowania w ogólny sposób. Jeśli musisz przetestować kod, który sprawia, że użyj metody Find, który najłatwiej utworzyć test znaleźć DbSet dla każdego z typów jednostek, które muszą być obsługiwane. Następnie można napisać logikę, aby znaleźć konkretny typ jednostki, jak pokazano poniżej.  

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

## <a name="writing-some-tests"></a>Pisanie niektórych testów  

To wszystko, co należy zrobić, aby uruchomić testy. Następujący test tworzy TestContext, a następnie usługi na podstawie tego kontekstu. Usługa jest następnie używany do utworzenia nowego bloga — przy użyciu metody AddBlog. Ponadto ten test sprawdza, czy usługa dodano nowego bloga kontekstu blogi właściwości i wywołuje SaveChanges w kontekście.  

To przykładowe typy rzeczy, które można przetestować za pomocą podwójnego testu w pamięci i można dostosować logiki wartości podwójnej precyzji testów i weryfikacji zgodnie z wymaganiami.  

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

Oto inny przykład testu — tym razem taki, który wykonuje kwerendę. Uruchamia test, tworząc kontekstu testu z danymi w jego blogu właściwość — należy pamiętać, że dane nie są w kolejności alfabetycznej. Firma Microsoft można utworzyć BlogService, na podstawie naszych kontekstu testu i upewnij się, że dane, które firma Microsoft wrócić z GetAllBlogs są uporządkowane według nazwy.  

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

Ponadto będziemy pisać jeden więcej test, który korzysta z naszego metody asynchronicznej, upewnij się, że infrastruktura async jest dostępna w [TestDbSet](#creating-the-in-memory-test-doubles) działa.  

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
