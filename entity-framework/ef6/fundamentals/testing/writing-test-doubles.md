---
title: Testowanie przy użyciu własnych testów podwaja się — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 3d8933fb5e17f8c01f3971495a1fcdb5b8cfab57
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416330"
---
# <a name="testing-with-your-own-test-doubles"></a>Testowanie przy użyciu własnych testów zostanie podwojone
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.  

Podczas pisania testów dla aplikacji często pożądane jest uniknięcie przeprowadzenia tej bazy danych.  Entity Framework pozwala to osiągnąć przez utworzenie kontekstu — z zachowaniem zdefiniowanym przez testy, które wykorzystuje dane znajdujące się w pamięci.  

## <a name="options-for-creating-test-doubles"></a>Opcje tworzenia podwojonych testów  

Istnieją dwa różne podejścia, których można użyć do utworzenia wersji w pamięci kontekstu.  

- **Utwórz własne podwójne testy** — to podejście polega na zapisywaniu własnej implementacji w pamięci dla kontekstu i dbsets. Zapewnia to dużą kontrolę nad sposobem zachowania klas, ale może polegać na pisaniu i tworzeniu rozsądnej ilości kodu.  
- **Użyj struktury imitacji do tworzenia podwójnego zatestowania** — przy użyciu struktury imitacji (takiej jak MOQ) możesz mieć implementacje w pamięci kontekstowe i zestawy tworzone dynamicznie w czasie wykonywania.  

W tym artykule opisano Tworzenie własnego testu podwójnie. Aby uzyskać informacje na temat korzystania z struktury imitacji, zobacz [testowanie przy użyciu struktury imitacji](mocking.md).  

## <a name="testing-with-pre-ef6-versions"></a>Testowanie za pomocą wersji pre-EF6  

Kod przedstawiony w tym artykule jest zgodny z EF6. Testowanie z EF5 i wcześniejszą wersją można znaleźć [w sekcji Testowanie przy użyciu fałszywego kontekstu](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Ograniczenia dotyczące niepodwojonych testów w pamięci  

Przetestowanie w pamięci może być dobrym sposobem zapewnienia pokryciu poziomu testów jednostkowych dla bitów aplikacji, które korzystają z EF. Jednak w tym przypadku używasz LINQ to Objects do wykonywania zapytań dotyczących danych w pamięci. Może to skutkować innym zachowaniem niż użycie dostawcy LINQ (LINQ to Entities) EF do translacji zapytań do bazy danych SQL, która jest uruchamiana w oparciu o bazę.  

Przykładem takiej różnicy jest ładowanie powiązanych danych. Jeśli utworzysz serię blogów, z których każdy ma powiązane wpisy, wówczas w przypadku korzystania z danych w pamięci wszystkie powiązane wpisy będą zawsze ładowane dla każdego bloga. Jednak w przypadku uruchamiania względem bazy danych dane będą ładowane tylko w przypadku użycia metody include.  

Z tego powodu zaleca się zawsze uwzględnienie pewnego poziomu testowania kompleksowego (oprócz testów jednostkowych), aby upewnić się, że aplikacja działa prawidłowo w odniesieniu do bazy danych.  

## <a name="following-along-with-this-article"></a>W tym artykule opisano następujące kwestie:  

W tym artykule przedstawiono kompletne listy kodu, które można skopiować do programu Visual Studio, aby postępować zgodnie z instrukcjami. Najłatwiej jest utworzyć **projekt testu jednostkowego** i należy wskazać **.NET Framework 4,5** , aby zakończyć sekcje, które używają Async.  

## <a name="creating-a-context-interface"></a>Tworzenie interfejsu kontekstu  

Przejdziemy do testowania usługi, która korzysta z modelu EF. Aby można było zamienić nasz kontekst EF z wersją znajdującą się w pamięci do testowania, zdefiniujemy interfejs, który zostanie wdrożony przez nasz kontekst EF (w pamięci podręcznej).

Usługa, którą zamierzamy przetestować, będzie wysyłać zapytania i modyfikować dane przy użyciu właściwości Nieogólnymi naszego kontekstu, a także wywołać metody SaveChanges, aby wypchnąć zmiany do bazy danych. Dlatego obejmujemy te elementy członkowskie w interfejsie.  

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

## <a name="the-ef-model"></a>Model EF  

Usługa, którą zamierzamy przetestować, wykorzystuje model EF składający się z BloggingContext oraz blogu i wpisów. Ten kod mógł zostać wygenerowany przez projektanta EF lub być modelem Code First.  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a>Implementowanie interfejsu kontekstu za pomocą narzędzia Dr Designer  

Należy pamiętać, że nasz kontekst implementuje interfejs IBloggingContext.  

Jeśli używasz Code First, możesz edytować swój kontekst bezpośrednio, aby zaimplementować interfejs. W przypadku korzystania z projektanta EF należy edytować szablon T4, który generuje kontekst. Otwórz model_name \<\>. Plik Context.tt, który jest zagnieżdżony w pliku edmx, Znajdź Poniższy fragment kodu i dodaj go do interfejsu, jak pokazano.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a>Usługa do przetestowania  

Aby zademonstrować testowanie przy użyciu testu w pamięci, należy napisać kilka testów dla BlogService. Usługa może tworzyć nowe blogi (addblog) i zwracać wszystkie blogi uporządkowane według nazwy (GetAllBlogs). Oprócz GetAllBlogs podano również metodę, która asynchronicznie pobiera wszystkie blogi uporządkowane według nazwy (GetAllBlogsAsync).  

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

## <a name="creating-the-in-memory-test-doubles"></a>Tworzenie testu w pamięci podwaja się  

Teraz, gdy mamy już rzeczywisty model EF i usługę, która może z niego korzystać, należy utworzyć test w pamięci, który można wykorzystać do testowania. Dla naszego kontekstu utworzyliśmy test TestContext. W teście podwajamy się, aby wybrać zachowanie, które chcemy w celu obsługi testów, które zamierzamy uruchomić. W tym przykładzie właśnie przechwytuje liczbę metody SaveChanges jest wywoływana, ale można dołączyć dowolną logikę, aby zweryfikować testowany scenariusz.  

Utworzyliśmy również TestDbSet, który zapewnia implementację Nieogólnymi. Podano kompletną implementację dla wszystkich metod w Nieogólnymi (z wyjątkiem znajdowania), ale musisz tylko zaimplementować elementy członkowskie, które będą używane przez scenariusz testowy.  

TestDbSet korzysta z innych klas infrastruktury, które zostały dołączone, aby zapewnić możliwość przetwarzania zapytań asynchronicznych.  

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

### <a name="implementing-find"></a>Implementowanie Znajdź  

Metoda Find jest trudna do zaimplementowania w ogólny sposób. Jeśli konieczne jest przetestowanie kodu, który używa metody Find, najłatwiej utworzyć test Nieogólnymi dla każdego typu jednostki, który musi obsługiwać Znajdowanie. Następnie można napisać logikę, aby znaleźć konkretny typ jednostki, jak pokazano poniżej.  

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

To wszystko, czego potrzebujemy do rozpoczęcia testowania. Poniższy test tworzy TestContext, a następnie usługę opartą na tym kontekście. Usługa jest następnie używana do tworzenia nowego bloga — za pomocą metody addblog. Na koniec test weryfikuje, czy usługa dodała nowy blog do właściwości bloga kontekstu i o nazwie metody SaveChanges w kontekście.  

Jest to tylko przykład typów rzeczy, które można testować za pomocą testu w pamięci, aby można było dostosować logikę podwajania testów i weryfikację w celu spełnienia wymagań.  

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

Oto inny przykład testu — ten czas wykonuje zapytanie. Test rozpoczyna się od utworzenia kontekstu testu z danymi w swojej właściwości blogu — należy zauważyć, że dane nie są w kolejności alfabetycznej. Możemy następnie utworzyć BlogService na podstawie naszego kontekstu testu i upewnić się, że dane, które powrócimy z GetAllBlogs, są uporządkowane według nazwy.  

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

Wreszcie napiszemy jeszcze jeden test, który używa naszej metody asynchronicznej, aby upewnić się, że infrastruktura asynchroniczna zawarta w [TestDbSet](#creating-the-in-memory-test-doubles) działa.  

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
