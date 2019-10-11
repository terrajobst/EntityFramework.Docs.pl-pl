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
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="aaa75-102">Testowanie przy użyciu struktury imitacji</span><span class="sxs-lookup"><span data-stu-id="aaa75-102">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="aaa75-103">**Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="aaa75-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="aaa75-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="aaa75-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="aaa75-105">Podczas pisania testów dla aplikacji często pożądane jest uniknięcie przeprowadzenia tej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="aaa75-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="aaa75-106">Entity Framework pozwala to osiągnąć przez utworzenie kontekstu — z zachowaniem zdefiniowanym przez testy, które wykorzystuje dane znajdujące się w pamięci.</span><span class="sxs-lookup"><span data-stu-id="aaa75-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="aaa75-107">Opcje tworzenia podwojonych testów</span><span class="sxs-lookup"><span data-stu-id="aaa75-107">Options for creating test doubles</span></span>  

<span data-ttu-id="aaa75-108">Istnieją dwa różne podejścia, których można użyć do utworzenia wersji w pamięci kontekstu.</span><span class="sxs-lookup"><span data-stu-id="aaa75-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="aaa75-109">**Utwórz własne podwójne testy** — to podejście polega na zapisywaniu własnej implementacji w pamięci dla kontekstu i dbsets.</span><span class="sxs-lookup"><span data-stu-id="aaa75-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="aaa75-110">Zapewnia to dużą kontrolę nad sposobem zachowania klas, ale może polegać na pisaniu i tworzeniu rozsądnej ilości kodu.</span><span class="sxs-lookup"><span data-stu-id="aaa75-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="aaa75-111">**Użyj struktury imitacji do tworzenia podwójnego zatestowania** — przy użyciu struktury imitacji (takiej jak MOQ) możesz mieć implementacje w pamięci kontekstu i zestawy tworzone dynamicznie w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="aaa75-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of your context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="aaa75-112">W tym artykule opisano sposób korzystania z struktury imitacji.</span><span class="sxs-lookup"><span data-stu-id="aaa75-112">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="aaa75-113">W celu utworzenia własnego przykładu testowego Zobacz test ze swoimi niektórymi [próbami](writing-test-doubles.md).</span><span class="sxs-lookup"><span data-stu-id="aaa75-113">For creating your own test doubles see [Testing with Your Own Test Doubles](writing-test-doubles.md).</span></span>  

<span data-ttu-id="aaa75-114">Aby zademonstrować używanie EF z platformą do imitacji, zamierzamy używać MOQ.</span><span class="sxs-lookup"><span data-stu-id="aaa75-114">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="aaa75-115">Najprostszym sposobem uzyskania MOQ jest zainstalowanie [pakietu MOQ z narzędzia NuGet](https://nuget.org/packages/Moq/).</span><span class="sxs-lookup"><span data-stu-id="aaa75-115">The easiest way to get Moq is to install the [Moq package from NuGet](https://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="aaa75-116">Testowanie za pomocą wersji pre-EF6</span><span class="sxs-lookup"><span data-stu-id="aaa75-116">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="aaa75-117">Scenariusz przedstawiony w tym artykule zależy od niektórych zmian wprowadzonych w celu Nieogólnymi w EF6.</span><span class="sxs-lookup"><span data-stu-id="aaa75-117">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="aaa75-118">Testowanie z EF5 i wcześniejszą wersją można znaleźć [w sekcji Testowanie przy użyciu fałszywego kontekstu](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="aaa75-118">For testing with EF5 and earlier version see [Testing with a Fake Context](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="aaa75-119">Ograniczenia dotyczące niepodwojonych testów w pamięci</span><span class="sxs-lookup"><span data-stu-id="aaa75-119">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="aaa75-120">Przetestowanie w pamięci może być dobrym sposobem zapewnienia pokryciu poziomu testów jednostkowych dla bitów aplikacji, które korzystają z EF.</span><span class="sxs-lookup"><span data-stu-id="aaa75-120">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="aaa75-121">Jednak w tym przypadku używasz LINQ to Objects do wykonywania zapytań dotyczących danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="aaa75-121">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="aaa75-122">Może to skutkować innym zachowaniem niż użycie dostawcy LINQ (LINQ to Entities) EF do translacji zapytań do bazy danych SQL, która jest uruchamiana w oparciu o bazę.</span><span class="sxs-lookup"><span data-stu-id="aaa75-122">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="aaa75-123">Przykładem takiej różnicy jest ładowanie powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="aaa75-123">One example of such a difference is loading related data.</span></span> <span data-ttu-id="aaa75-124">Jeśli utworzysz serię blogów, z których każdy ma powiązane wpisy, wówczas w przypadku korzystania z danych w pamięci wszystkie powiązane wpisy będą zawsze ładowane dla każdego bloga.</span><span class="sxs-lookup"><span data-stu-id="aaa75-124">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="aaa75-125">Jednak w przypadku uruchamiania względem bazy danych dane będą ładowane tylko w przypadku użycia metody include.</span><span class="sxs-lookup"><span data-stu-id="aaa75-125">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="aaa75-126">Z tego powodu zaleca się zawsze uwzględnienie pewnego poziomu testowania kompleksowego (oprócz testów jednostkowych), aby upewnić się, że aplikacja działa prawidłowo w odniesieniu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="aaa75-126">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="aaa75-127">W tym artykule opisano następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="aaa75-127">Following along with this article</span></span>  

<span data-ttu-id="aaa75-128">W tym artykule przedstawiono kompletne listy kodu, które można skopiować do programu Visual Studio, aby postępować zgodnie z instrukcjami.</span><span class="sxs-lookup"><span data-stu-id="aaa75-128">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="aaa75-129">Najłatwiej jest utworzyć **projekt testu jednostkowego** i należy wskazać **.NET Framework 4,5** , aby zakończyć sekcje, które używają Async.</span><span class="sxs-lookup"><span data-stu-id="aaa75-129">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="aaa75-130">Model EF</span><span class="sxs-lookup"><span data-stu-id="aaa75-130">The EF model</span></span>  

<span data-ttu-id="aaa75-131">Usługa, którą zamierzamy przetestować, wykorzystuje model EF składający się z BloggingContext oraz blogu i wpisów.</span><span class="sxs-lookup"><span data-stu-id="aaa75-131">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="aaa75-132">Ten kod mógł zostać wygenerowany przez projektanta EF lub być modelem Code First.</span><span class="sxs-lookup"><span data-stu-id="aaa75-132">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="aaa75-133">Wirtualne właściwości Nieogólnymi za pomocą narzędzia Dr Designer</span><span class="sxs-lookup"><span data-stu-id="aaa75-133">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="aaa75-134">Należy zauważyć, że właściwości Nieogólnymi w kontekście są oznaczone jako wirtualne.</span><span class="sxs-lookup"><span data-stu-id="aaa75-134">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="aaa75-135">Dzięki temu platforma służąca do imitacji będzie mogła pochodzić od naszego kontekstu i przesłaniać te właściwości za pomocą zaimplementowanej implementacji.</span><span class="sxs-lookup"><span data-stu-id="aaa75-135">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="aaa75-136">Jeśli używasz Code First, możesz edytować klasy bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="aaa75-136">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="aaa75-137">W przypadku korzystania z projektanta EF należy edytować szablon T4, który generuje kontekst.</span><span class="sxs-lookup"><span data-stu-id="aaa75-137">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="aaa75-138">Otwórz \<model_name @ no__t-1. Plik Context.tt, który jest zagnieżdżony w pliku edmx, Znajdź Poniższy fragment kodu i dodaj go do słowa kluczowego Virtual, jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="aaa75-138">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

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

## <a name="service-to-be-tested"></a><span data-ttu-id="aaa75-139">Usługa do przetestowania</span><span class="sxs-lookup"><span data-stu-id="aaa75-139">Service to be tested</span></span>  

<span data-ttu-id="aaa75-140">Aby zademonstrować testowanie przy użyciu testu w pamięci, należy napisać kilka testów dla BlogService.</span><span class="sxs-lookup"><span data-stu-id="aaa75-140">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="aaa75-141">Usługa może tworzyć nowe blogi (addblog) i zwracać wszystkie blogi uporządkowane według nazwy (GetAllBlogs).</span><span class="sxs-lookup"><span data-stu-id="aaa75-141">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="aaa75-142">Oprócz GetAllBlogs podano również metodę, która asynchronicznie pobiera wszystkie blogi uporządkowane według nazwy (GetAllBlogsAsync).</span><span class="sxs-lookup"><span data-stu-id="aaa75-142">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

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

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="aaa75-143">Testowanie scenariuszy innych niż zapytania</span><span class="sxs-lookup"><span data-stu-id="aaa75-143">Testing non-query scenarios</span></span>  

<span data-ttu-id="aaa75-144">To wszystko, czego potrzebujemy do rozpoczęcia testowania metod niezwiązanych z badaniem.</span><span class="sxs-lookup"><span data-stu-id="aaa75-144">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="aaa75-145">Poniższy test używa MOQ do utworzenia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="aaa75-145">The following test uses Moq to create a context.</span></span> <span data-ttu-id="aaa75-146">Następnie tworzy Nieogólnymi @ no__t-0Blog @ no__t-1 i drutuje, aby można było zwrócić z właściwości bloga kontekstu.</span><span class="sxs-lookup"><span data-stu-id="aaa75-146">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="aaa75-147">Następnie kontekst służy do tworzenia nowego BlogService, który jest następnie używany do tworzenia nowego bloga — za pomocą metody addblog.</span><span class="sxs-lookup"><span data-stu-id="aaa75-147">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="aaa75-148">Na koniec test weryfikuje, czy usługa dodała nowy blog o nazwie metody SaveChanges w kontekście.</span><span class="sxs-lookup"><span data-stu-id="aaa75-148">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

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

## <a name="testing-query-scenarios"></a><span data-ttu-id="aaa75-149">Testowanie scenariuszy zapytań</span><span class="sxs-lookup"><span data-stu-id="aaa75-149">Testing query scenarios</span></span>  

<span data-ttu-id="aaa75-150">Aby móc wykonywać zapytania na naszym teście Nieogólnymi, musimy skonfigurować implementację IQueryable.</span><span class="sxs-lookup"><span data-stu-id="aaa75-150">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="aaa75-151">Pierwszym krokiem jest utworzenie niektórych danych znajdujących się w pamięci — używamy listy @ no__t-0Blog @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="aaa75-151">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="aaa75-152">Następnie utworzymy kontekst i Nieogólnymi @ no__t-0Blog @ no__t-1, a następnie nastąpi przełączenie implementacji IQueryable dla Nieogólnymi — są one tylko delegowane do dostawcy LINQ to Objects, który współpracuje z listą @ no__t-2T @ no__t-3.</span><span class="sxs-lookup"><span data-stu-id="aaa75-152">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="aaa75-153">Możemy następnie utworzyć BlogService na podstawie naszych testów, aby upewnić się, że dane, które powrócimy z GetAllBlogs, są uporządkowane według nazwy.</span><span class="sxs-lookup"><span data-stu-id="aaa75-153">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

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

### <a name="testing-with-async-queries"></a><span data-ttu-id="aaa75-154">Testowanie przy użyciu zapytań asynchronicznych</span><span class="sxs-lookup"><span data-stu-id="aaa75-154">Testing with async queries</span></span>

<span data-ttu-id="aaa75-155">Entity Framework 6 wprowadził zestaw metod rozszerzających, które mogą być używane do asynchronicznego wykonywania zapytania.</span><span class="sxs-lookup"><span data-stu-id="aaa75-155">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="aaa75-156">Przykłady tych metod to: ToListAsync, FirstAsync, ForEachAsync itd.</span><span class="sxs-lookup"><span data-stu-id="aaa75-156">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="aaa75-157">Ponieważ zapytania Entity Framework używają LINQ, metody rozszerzające są zdefiniowane w interfejsie IQueryable i IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="aaa75-157">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="aaa75-158">Jednak ponieważ są one przeznaczone wyłącznie dla Entity Framework, w przypadku próby użycia ich w zapytaniu LINQ, które nie jest kwerendą Entity Framework, może zostać wyświetlony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="aaa75-158">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="aaa75-159">Źródło IQueryable nie implementuje IDbAsyncEnumerable @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="aaa75-159">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="aaa75-160">Do Entity Framework operacji asynchronicznych można używać tylko źródeł, które implementują IDbAsyncEnumerable.</span><span class="sxs-lookup"><span data-stu-id="aaa75-160">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="aaa75-161">Aby uzyskać więcej informacji, zobacz [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).</span><span class="sxs-lookup"><span data-stu-id="aaa75-161">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="aaa75-162">Chociaż metody asynchroniczne są obsługiwane tylko w przypadku uruchamiania zapytania EF, można użyć ich w teście jednostkowym, gdy działa w teście w pamięci o podwójnej części Nieogólnymi.</span><span class="sxs-lookup"><span data-stu-id="aaa75-162">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="aaa75-163">Aby można było użyć metod asynchronicznych, należy utworzyć DbAsyncQueryProvider w pamięci, aby przetworzyć kwerendę asynchroniczną.</span><span class="sxs-lookup"><span data-stu-id="aaa75-163">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="aaa75-164">Chociaż można skonfigurować dostawcę zapytań za pomocą MOQ, znacznie łatwiej jest utworzyć test podwójnej implementacji w kodzie.</span><span class="sxs-lookup"><span data-stu-id="aaa75-164">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="aaa75-165">Kod dla tej implementacji jest następujący:</span><span class="sxs-lookup"><span data-stu-id="aaa75-165">The code for this implementation is as follows:</span></span>  

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

<span data-ttu-id="aaa75-166">Teraz, gdy mamy dostawcę zapytań asynchronicznych, możemy napisać test jednostkowy dla naszej nowej metody GetAllBlogsAsync.</span><span class="sxs-lookup"><span data-stu-id="aaa75-166">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

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
