---
title: Testowanie za pomocą framework pozorowania — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: 3d39b41018beb70b72105dfb2fe4d61afc0b0525
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319208"
---
# <a name="testing-with-a-mocking-framework"></a>Testowanie za pomocą pozorowania framework
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.  

Podczas pisania testów dla aplikacji często jest pożądane, aby uniknąć osiągnięcia bazy danych.  Entity Framework można to osiągnąć, tworząc kontekst — w przypadku zachowanie zdefiniowane przez testy — korzystającej z danych w pamięci.  

## <a name="options-for-creating-test-doubles"></a>Opcje tworzenia testu wartości podwójnej precyzji  

Istnieją dwa różne podejścia, które mogą służyć do tworzenia w pamięci wersję kontekstu.  

- **Tworzenie własnych testów, wartości podwójnej precyzji** — ta strategia polega na pisanie własnego kontekstu i DbSets implementacji w pamięci. Zapewnia wysoki poziom kontroli nad jak zachowują się klasy, ale może obejmować pisanie i będącej właścicielem rozsądnym kodu.  
- **Umożliwia tworzenie testów wartości podwójnej precyzji pozorowania framework** — za pomocą pozorowania framework (na przykład Moq) może mieć implementacji w pamięci kontekstu i zestawy tworzone dynamicznie w czasie wykonywania.  

W tym artykule zajmuje się za pomocą pozorowania framework. W celu tworzenia własnych podwaja testu zobacz [testowanie za pomocą Your Own testowanie wartości podwójnej precyzji](writing-test-doubles.md).  

Aby zademonstrować przy użyciu programu EF pozorowania Framework są użyjemy Moq. Najprostszym sposobem uzyskania Moq jest zainstalować [Moq pakietu NuGet](http://nuget.org/packages/Moq/).  

## <a name="testing-with-pre-ef6-versions"></a>Testowanie za pomocą wersji pre-EF6  

Scenariusz przedstawione w tym artykule jest zależna od pewne zmiany, które wprowadziliśmy DbSet w EF6. Testowanie za pomocą EF5 i starszych wersji dla [testowanie za pomocą kontekstu fałszywe](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Ograniczenia dotyczące wartości podwójnej precyzji EF testu w pamięci  

Test w pamięci wartości podwójnej precyzji, może być dobrym sposobem zapewnienia poziomu zasięg bitów aplikacji korzystających z programu EF testów jednostkowych. Jednak w ten sposób używasz LINQ to Objects do wykonywania zapytań dotyczących danych w pamięci. Może to spowodować, że inaczej niż tłumaczenie zapytań SQL, która jest uruchamiana względem bazy danych przy użyciu programu EF firmy dostawcy LINQ (LINQ to Entities).  

Przykładem takiej różnicy Trwa ładowanie powiązanych danych. Jeśli tworzysz szereg blogi każdy z powiązanymi wpisy, a następnie korzystając z danych w pamięci dla każdego bloga zawsze zostaną załadowane pokrewnych wpisów. Jednak podczas uruchamiania w bazie danych dane tylko zostanie załadowany Jeśli używana jest metoda Include.  

Z tego powodu zaleca się zawsze zawierać pewien stopień end-to-end testy (oprócz testy jednostkowe) w celu zapewnienia działania usługi aplikacji poprawnie względem bazy danych.  

## <a name="following-along-with-this-article"></a>Zgodnie z tego artykułu  

Ten artykuł zawiera kompletny kod ofert, kopiowane do programu Visual Studio, aby z niego skorzystać, jeśli chcesz. Najłatwiej utworzyć **projektu testu jednostkowego** i należy do obiektu docelowego **.NET Framework 4.5** do wykonania w sekcjach, korzystających z async.  

## <a name="the-ef-model"></a>Modelu platformy EF  

Usługa użyjemy do przetestowania, korzysta z programu EF modelu składa się z BloggingContext i klas blogu i Post. Ten kod został wygenerowany przez projektanta programu EF lub model Code First.  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a>Właściwości DbSet wirtualnych za pomocą projektanta EF  

Należy pamiętać o tym, czy właściwości DbSet w kontekście są oznaczone jako wirtualny. Umożliwi to pozorowania platformę, by dziedziczyć nasz kontekst i zastępowanie te właściwości z implementacją pozorowane.  

Jeśli używasz Code First, a następnie w Twoich zajęciach można edytować bezpośrednio. Jeśli używasz projektancie platformy EF następnie należy edytować szablon T4, który generuje kontekstu. Otwórz \<nazwa_modelu\>. Plik context.TT, który jest zagnieżdżony w przypadku pliku edmx, znajdź następujący fragment kodu i Dodaj virtual — słowo kluczowe, jak pokazano.  

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

## <a name="testing-non-query-scenarios"></a>-Query scenariuszy testowania  

To wszystko, co należy zrobić, aby rozpocząć testowanie metody niebędącą zapytaniem. Następującego testu używa Moq, aby utworzyć kontekst. Następnie tworzy DbSet\<blogu\> i tworzącej mają być zwracane przez właściwości blogi kontekstu. Dalej aby utworzyć nowy BlogService, który jest następnie używany do tworzenia nowego bloga — przy użyciu metody AddBlog używany jest kontekst. Ponadto ten test sprawdza, czy usługa dodano nowego bloga i o nazwie SaveChanges w kontekście.  

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

## <a name="testing-query-scenarios"></a>Scenariusze testowania zapytań  

Aby można było wykonać zapytania względem naszym teście DbSet double musimy skonfigurować implementację elementu IQueryable. Pierwszym krokiem jest utworzenie niektóre dane w pamięci — używamy listy\<blogu\>. Następnie utworzymy kontekst i DBSet\<Blog\> następnie Podłączanie implementację IQueryable DbSet — są one po prostu delegowanie do LINQ do dostawcy obiektów, która współdziała z listy\<T\>.  

Firma Microsoft można utworzyć BlogService, oparte na typach Double nasze badania i upewnij się, że dane, które firma Microsoft wrócić z GetAllBlogs są uporządkowane według nazwy.  

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

### <a name="testing-with-async-queries"></a>Testowanie za pomocą zapytania asynchroniczne

Entity Framework 6 wprowadziliśmy zestaw metod rozszerzenia, które może służyć do asynchronicznego wykonywanie zapytania. Przykładami tych metod ToListAsync FirstAsync, ForEachAsync itp.  

Ponieważ Entity Framework zapytań korzystaj z LINQ, metody rozszerzające są definiowane na IQueryable i IEnumerable. Ponieważ są one przeznaczone tylko do użycia z programem Entity Framework możesz otrzymać następujący błąd przy próbie z nich korzystać na zapytanie LINQ, która nie jest zapytaniem Entity Framework:

> Źródło IQueryable nie implementuje IDbAsyncEnumerable{0}. Tylko źródła, które implementują IDbAsyncEnumerable może służyć do operacji asynchronicznych Entity Framework. Aby uzyskać więcej informacji, zobacz [ http://go.microsoft.com/fwlink/?LinkId=287068 ](https://go.microsoft.com/fwlink/?LinkId=287068).  

Podczas gdy metody asynchroniczne są obsługiwane tylko przy uruchamianiu kwerendę EF, można ich używać w testu jednostkowego, gdy uruchomiona przed w pamięci testowanie double DbSet.  

Aby można było używać metod asynchronicznych, musimy utworzyć DbAsyncQueryProvider w pamięci do przetworzenia zapytania asynchroniczne. O ile można skonfigurować dostawcę zapytania za pomocą Moq, jest znacznie łatwiejsze utworzyć implementacja podwójnego testowego w kodzie. Kod dla tej implementacji jest następująca:  

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

Teraz, gdy dostawca zapytania asynchroniczne możemy napisać test jednostkowy dla naszej nowej metody GetAllBlogsAsync.  

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
