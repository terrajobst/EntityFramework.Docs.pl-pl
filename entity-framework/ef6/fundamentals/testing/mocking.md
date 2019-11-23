---
title: Testowanie przy użyciu struktury imitacji — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: 790e077c5b30c4a68a96b3c1a99b40893b2bbe55
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181572"
---
# <a name="testing-with-a-mocking-framework"></a>Testowanie przy użyciu struktury imitacji
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.  

Podczas pisania testów dla aplikacji często pożądane jest uniknięcie przeprowadzenia tej bazy danych.  Entity Framework pozwala to osiągnąć przez utworzenie kontekstu — z zachowaniem zdefiniowanym przez testy, które wykorzystuje dane znajdujące się w pamięci.  

## <a name="options-for-creating-test-doubles"></a>Opcje tworzenia podwojonych testów  

Istnieją dwa różne podejścia, których można użyć do utworzenia wersji w pamięci kontekstu.  

- **Utwórz własne podwójne testy** — to podejście polega na zapisywaniu własnej implementacji w pamięci dla kontekstu i dbsets. Zapewnia to dużą kontrolę nad sposobem zachowania klas, ale może polegać na pisaniu i tworzeniu rozsądnej ilości kodu.  
- **Użyj struktury imitacji do tworzenia podwójnego zatestowania** — przy użyciu struktury imitacji (takiej jak MOQ) możesz mieć implementacje w pamięci kontekstu i zestawy tworzone dynamicznie w czasie wykonywania.  

W tym artykule opisano sposób korzystania z struktury imitacji. W celu utworzenia własnego przykładu testowego Zobacz test ze swoimi niektórymi [próbami](writing-test-doubles.md).  

Aby zademonstrować używanie EF z platformą do imitacji, zamierzamy używać MOQ. Najprostszym sposobem uzyskania MOQ jest zainstalowanie [pakietu MOQ z narzędzia NuGet](https://nuget.org/packages/Moq/).  

## <a name="testing-with-pre-ef6-versions"></a>Testowanie za pomocą wersji pre-EF6  

Scenariusz przedstawiony w tym artykule zależy od niektórych zmian wprowadzonych w celu Nieogólnymi w EF6. Testowanie z EF5 i wcześniejszą wersją można znaleźć [w sekcji Testowanie przy użyciu fałszywego kontekstu](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Ograniczenia dotyczące niepodwojonych testów w pamięci  

Przetestowanie w pamięci może być dobrym sposobem zapewnienia pokryciu poziomu testów jednostkowych dla bitów aplikacji, które korzystają z EF. Jednak w tym przypadku używasz LINQ to Objects do wykonywania zapytań dotyczących danych w pamięci. Może to skutkować innym zachowaniem niż użycie dostawcy LINQ (LINQ to Entities) EF do translacji zapytań do bazy danych SQL, która jest uruchamiana w oparciu o bazę.  

Przykładem takiej różnicy jest ładowanie powiązanych danych. Jeśli utworzysz serię blogów, z których każdy ma powiązane wpisy, wówczas w przypadku korzystania z danych w pamięci wszystkie powiązane wpisy będą zawsze ładowane dla każdego bloga. Jednak w przypadku uruchamiania względem bazy danych dane będą ładowane tylko w przypadku użycia metody include.  

Z tego powodu zaleca się zawsze uwzględnienie pewnego poziomu testowania kompleksowego (oprócz testów jednostkowych), aby upewnić się, że aplikacja działa prawidłowo w odniesieniu do bazy danych.  

## <a name="following-along-with-this-article"></a>W tym artykule opisano następujące kwestie:  

W tym artykule przedstawiono kompletne listy kodu, które można skopiować do programu Visual Studio, aby postępować zgodnie z instrukcjami. Najłatwiej jest utworzyć **projekt testu jednostkowego** i należy wskazać **.NET Framework 4,5** , aby zakończyć sekcje, które używają Async.  

## <a name="the-ef-model"></a>Model EF  

Usługa, którą zamierzamy przetestować, wykorzystuje model EF składający się z BloggingContext oraz blogu i wpisów. Ten kod mógł zostać wygenerowany przez projektanta EF lub być modelem Code First.  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext
    {
        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }
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

### <a name="virtual-dbset-properties-with-ef-designer"></a>Wirtualne właściwości Nieogólnymi za pomocą narzędzia Dr Designer  

Należy zauważyć, że właściwości Nieogólnymi w kontekście są oznaczone jako wirtualne. Dzięki temu platforma służąca do imitacji będzie mogła pochodzić od naszego kontekstu i przesłaniać te właściwości za pomocą zaimplementowanej implementacji.  

Jeśli używasz Code First, możesz edytować klasy bezpośrednio. W przypadku korzystania z projektanta EF należy edytować szablon T4, który generuje kontekst. Otwórz model_name \<\>. Plik Context.tt, który jest zagnieżdżony w pliku edmx, Znajdź Poniższy fragment kodu i dodaj go do słowa kluczowego Virtual, jak pokazano.  

``` csharp
public string DbSet(EntitySet entitySet)
{
    return string.Format(
        CultureInfo.InvariantCulture,
        "{0} virtual DbSet\<{1}> {2} {{ get; set; }}",
        Accessibility.ForReadOnlyProperty(entitySet),
        _typeMapper.GetTypeName(entitySet.ElementType),
        _code.Escape(entitySet));
}
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
        private BloggingContext _context;

        public BlogService(BloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = _context.Blogs.Add(new Blog { Name = name, Url = url });
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

## <a name="testing-non-query-scenarios"></a>Testowanie scenariuszy innych niż zapytania  

To wszystko, czego potrzebujemy do rozpoczęcia testowania metod niezwiązanych z badaniem. Poniższy test używa MOQ do utworzenia kontekstu. Następnie tworzy blog Nieogólnymi\<\> i nadaje mu przewody do zwrócenia z właściwości bloga kontekstu. Następnie kontekst służy do tworzenia nowego BlogService, który jest następnie używany do tworzenia nowego bloga — za pomocą metody addblog. Na koniec test weryfikuje, czy usługa dodała nowy blog o nazwie metody SaveChanges w kontekście.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Data.Entity;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var mockSet = new Mock<DbSet<Blog>>();

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(m => m.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            mockSet.Verify(m => m.Add(It.IsAny<Blog>()), Times.Once());
            mockContext.Verify(m => m.SaveChanges(), Times.Once());
        }
    }
}
```  

## <a name="testing-query-scenarios"></a>Testowanie scenariuszy zapytań  

Aby móc wykonywać zapytania na naszym teście Nieogólnymi, musimy skonfigurować implementację IQueryable. Pierwszym krokiem jest utworzenie niektórych danych znajdujących się w pamięci — używamy listy\<\>blogu. Następnie utworzymy blog kontekstu i Nieogólnymi\<\> następnie przeniesiemy implementację w interfejsie IQueryable dla Nieogólnymi — po prostu delegowanie do dostawcy LINQ to Objects, który współpracuje z\<listą T\>.  

Możemy następnie utworzyć BlogService na podstawie naszych testów, aby upewnić się, że dane, które powrócimy z GetAllBlogs, są uporządkowane według nazwy.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Provider).Returns(data.Provider);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

### <a name="testing-with-async-queries"></a>Testowanie przy użyciu zapytań asynchronicznych

Entity Framework 6 wprowadził zestaw metod rozszerzających, które mogą być używane do asynchronicznego wykonywania zapytania. Przykłady tych metod to: ToListAsync, FirstAsync, ForEachAsync itd.  

Ponieważ zapytania Entity Framework używają LINQ, metody rozszerzające są zdefiniowane w interfejsie IQueryable i IEnumerable. Jednak ponieważ są one przeznaczone wyłącznie dla Entity Framework, w przypadku próby użycia ich w zapytaniu LINQ, które nie jest kwerendą Entity Framework, może zostać wyświetlony następujący błąd:

> Źródło IQueryable nie implementuje IDbAsyncEnumerable{0}. Do Entity Framework operacji asynchronicznych można używać tylko źródeł, które implementują IDbAsyncEnumerable. Aby uzyskać więcej informacji, zobacz [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).  

Chociaż metody asynchroniczne są obsługiwane tylko w przypadku uruchamiania zapytania EF, można użyć ich w teście jednostkowym, gdy działa w teście w pamięci o podwójnej części Nieogólnymi.  

Aby można było użyć metod asynchronicznych, należy utworzyć DbAsyncQueryProvider w pamięci, aby przetworzyć kwerendę asynchroniczną. Chociaż można skonfigurować dostawcę zapytań za pomocą MOQ, znacznie łatwiej jest utworzyć test podwójnej implementacji w kodzie. Kod dla tej implementacji jest następujący:  

``` csharp
using System.Collections.Generic;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
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

Teraz, gdy mamy dostawcę zapytań asynchronicznych, możemy napisać test jednostkowy dla naszej nowej metody GetAllBlogsAsync.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
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

            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IDbAsyncEnumerable<Blog>>()
                .Setup(m => m.GetAsyncEnumerator())
                .Returns(new TestDbAsyncEnumerator<Blog>(data.GetEnumerator()));

            mockSet.As<IQueryable<Blog>>()
                .Setup(m => m.Provider)
                .Returns(new TestDbAsyncQueryProvider<Blog>(data.Provider));

            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
