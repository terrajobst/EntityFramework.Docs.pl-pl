---
title: Testowanie za pomocą framework pozorowania — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: abb915f2df5645b3838a659dafc59a2b499b4ee2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998216"
---
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="7699a-102">Testowanie za pomocą pozorowania framework</span><span class="sxs-lookup"><span data-stu-id="7699a-102">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="7699a-103">**EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="7699a-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="7699a-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="7699a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="7699a-105">Podczas pisania testów dla aplikacji często jest pożądane, aby uniknąć osiągnięcia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7699a-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="7699a-106">Entity Framework można to osiągnąć, tworząc kontekst — w przypadku zachowanie zdefiniowane przez testy — korzystającej z danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="7699a-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="7699a-107">Opcje tworzenia testu wartości podwójnej precyzji</span><span class="sxs-lookup"><span data-stu-id="7699a-107">Options for creating test doubles</span></span>  

<span data-ttu-id="7699a-108">Istnieją dwa różne podejścia, które mogą służyć do tworzenia w pamięci wersję kontekstu.</span><span class="sxs-lookup"><span data-stu-id="7699a-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="7699a-109">**Tworzenie własnych testów, wartości podwójnej precyzji** — ta strategia polega na pisanie własnego kontekstu i DbSets implementacji w pamięci.</span><span class="sxs-lookup"><span data-stu-id="7699a-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="7699a-110">Zapewnia wysoki poziom kontroli nad jak zachowują się klasy, ale może obejmować pisanie i będącej właścicielem rozsądnym kodu.</span><span class="sxs-lookup"><span data-stu-id="7699a-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="7699a-111">**Umożliwia tworzenie testów wartości podwójnej precyzji pozorowania framework** — za pomocą pozorowania framework (na przykład Moq) może mieć implementacji w pamięci, kontekstu i zestawy tworzone dynamicznie w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="7699a-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="7699a-112">W tym artykule zajmuje się za pomocą pozorowania framework.</span><span class="sxs-lookup"><span data-stu-id="7699a-112">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="7699a-113">W celu tworzenia własnych podwaja testu zobacz [testowanie za pomocą Your Own testowanie wartości podwójnej precyzji](writing-test-doubles.md).</span><span class="sxs-lookup"><span data-stu-id="7699a-113">For creating your own test doubles see [Testing with Your Own Test Doubles](writing-test-doubles.md).</span></span>  

<span data-ttu-id="7699a-114">Aby zademonstrować przy użyciu programu EF pozorowania Framework są użyjemy Moq.</span><span class="sxs-lookup"><span data-stu-id="7699a-114">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="7699a-115">Najprostszym sposobem uzyskania Moq jest zainstalować [Moq pakietu NuGet](http://nuget.org/packages/Moq/).</span><span class="sxs-lookup"><span data-stu-id="7699a-115">The easiest way to get Moq is to install the [Moq package from NuGet](http://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="7699a-116">Testowanie za pomocą wersji pre-EF6</span><span class="sxs-lookup"><span data-stu-id="7699a-116">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="7699a-117">Scenariusz przedstawione w tym artykule jest zależna od pewne zmiany, które wprowadziliśmy DbSet w EF6.</span><span class="sxs-lookup"><span data-stu-id="7699a-117">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="7699a-118">Testowanie za pomocą EF5 i starszych wersji dla [testowanie za pomocą kontekstu fałszywe](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="7699a-118">For testing with EF5 and earlier version see [Testing with a Fake Context](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="7699a-119">Ograniczenia dotyczące wartości podwójnej precyzji EF testu w pamięci</span><span class="sxs-lookup"><span data-stu-id="7699a-119">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="7699a-120">Test w pamięci wartości podwójnej precyzji, może być dobrym sposobem zapewnienia poziomu zasięg bitów aplikacji korzystających z programu EF testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="7699a-120">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="7699a-121">Jednak w ten sposób używasz LINQ to Objects do wykonywania zapytań dotyczących danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="7699a-121">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="7699a-122">Może to spowodować, że inaczej niż tłumaczenie zapytań SQL, która jest uruchamiana względem bazy danych przy użyciu programu EF firmy dostawcy LINQ (LINQ to Entities).</span><span class="sxs-lookup"><span data-stu-id="7699a-122">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="7699a-123">Przykładem takiej różnicy Trwa ładowanie powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="7699a-123">One example of such a difference is loading related data.</span></span> <span data-ttu-id="7699a-124">Jeśli tworzysz szereg blogi każdy z powiązanymi wpisy, a następnie korzystając z danych w pamięci dla każdego bloga zawsze zostaną załadowane pokrewnych wpisów.</span><span class="sxs-lookup"><span data-stu-id="7699a-124">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="7699a-125">Jednak podczas uruchamiania w bazie danych dane tylko zostanie załadowany Jeśli używana jest metoda Include.</span><span class="sxs-lookup"><span data-stu-id="7699a-125">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="7699a-126">Z tego powodu zaleca się zawsze zawierać pewien stopień end-to-end testy (oprócz testy jednostkowe) w celu zapewnienia działania usługi aplikacji poprawnie względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7699a-126">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="7699a-127">Zgodnie z tego artykułu</span><span class="sxs-lookup"><span data-stu-id="7699a-127">Following along with this article</span></span>  

<span data-ttu-id="7699a-128">Ten artykuł zawiera kompletny kod ofert, kopiowane do programu Visual Studio, aby z niego skorzystać, jeśli chcesz.</span><span class="sxs-lookup"><span data-stu-id="7699a-128">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="7699a-129">Najłatwiej utworzyć **projektu testu jednostkowego** i należy do obiektu docelowego **.NET Framework 4.5** do wykonania w sekcjach, korzystających z async.</span><span class="sxs-lookup"><span data-stu-id="7699a-129">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="7699a-130">Modelu platformy EF</span><span class="sxs-lookup"><span data-stu-id="7699a-130">The EF model</span></span>  

<span data-ttu-id="7699a-131">Usługa użyjemy do przetestowania, korzysta z programu EF modelu składa się z BloggingContext i klas blogu i Post.</span><span class="sxs-lookup"><span data-stu-id="7699a-131">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="7699a-132">Ten kod został wygenerowany przez projektanta programu EF lub model Code First.</span><span class="sxs-lookup"><span data-stu-id="7699a-132">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="7699a-133">Właściwości DbSet wirtualnych za pomocą projektanta EF</span><span class="sxs-lookup"><span data-stu-id="7699a-133">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="7699a-134">Należy pamiętać o tym, czy właściwości DbSet w kontekście są oznaczone jako wirtualny.</span><span class="sxs-lookup"><span data-stu-id="7699a-134">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="7699a-135">Umożliwi to pozorowania platformę, by dziedziczyć nasz kontekst i zastępowanie te właściwości z implementacją pozorowane.</span><span class="sxs-lookup"><span data-stu-id="7699a-135">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="7699a-136">Jeśli używasz Code First, a następnie w Twoich zajęciach można edytować bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="7699a-136">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="7699a-137">Jeśli używasz projektancie platformy EF następnie należy edytować szablon T4, który generuje kontekstu.</span><span class="sxs-lookup"><span data-stu-id="7699a-137">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="7699a-138">Otwórz \<nazwa_modelu\>. Plik context.TT, który jest zagnieżdżony w przypadku pliku edmx, znajdź następujący fragment kodu i Dodaj virtual — słowo kluczowe, jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="7699a-138">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

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

## <a name="service-to-be-tested"></a><span data-ttu-id="7699a-139">Usługi w celu zbadania</span><span class="sxs-lookup"><span data-stu-id="7699a-139">Service to be tested</span></span>  

<span data-ttu-id="7699a-140">Aby zademonstrować, testowanie za pomocą testu w pamięci wartości podwójnej precyzji zamierzamy zapisywać dla BlogService kilka testów.</span><span class="sxs-lookup"><span data-stu-id="7699a-140">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="7699a-141">Usługa jest w stanie tworzenie nowych blogów (AddBlog) i zwraca wszystkie blogi uporządkowane według nazwy (GetAllBlogs).</span><span class="sxs-lookup"><span data-stu-id="7699a-141">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="7699a-142">Oprócz GetAllBlogs udostępniliśmy również metodę, która asynchronicznie pobierze wszystkie blogi uporządkowane według nazwy (GetAllBlogsAsync).</span><span class="sxs-lookup"><span data-stu-id="7699a-142">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

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

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="7699a-143">-Query scenariuszy testowania</span><span class="sxs-lookup"><span data-stu-id="7699a-143">Testing non-query scenarios</span></span>  

<span data-ttu-id="7699a-144">To wszystko, co należy zrobić, aby rozpocząć testowanie metody niebędącą zapytaniem.</span><span class="sxs-lookup"><span data-stu-id="7699a-144">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="7699a-145">Następującego testu używa Moq, aby utworzyć kontekst.</span><span class="sxs-lookup"><span data-stu-id="7699a-145">The following test uses Moq to create a context.</span></span> <span data-ttu-id="7699a-146">Następnie tworzy DbSet\<blogu\> i tworzącej mają być zwracane przez właściwości blogi kontekstu.</span><span class="sxs-lookup"><span data-stu-id="7699a-146">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="7699a-147">Dalej aby utworzyć nowy BlogService, który jest następnie używany do tworzenia nowego bloga — przy użyciu metody AddBlog używany jest kontekst.</span><span class="sxs-lookup"><span data-stu-id="7699a-147">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="7699a-148">Ponadto ten test sprawdza, czy usługa dodano nowego bloga i o nazwie SaveChanges w kontekście.</span><span class="sxs-lookup"><span data-stu-id="7699a-148">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

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

## <a name="testing-query-scenarios"></a><span data-ttu-id="7699a-149">Scenariusze testowania zapytań</span><span class="sxs-lookup"><span data-stu-id="7699a-149">Testing query scenarios</span></span>  

<span data-ttu-id="7699a-150">Aby można było wykonać zapytania względem naszym teście DbSet double musimy skonfigurować implementację elementu IQueryable.</span><span class="sxs-lookup"><span data-stu-id="7699a-150">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="7699a-151">Pierwszym krokiem jest utworzenie niektóre dane w pamięci — używamy listy\<blogu\>.</span><span class="sxs-lookup"><span data-stu-id="7699a-151">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="7699a-152">Następnie utworzymy kontekst i DBSet\<Blog\> następnie Podłączanie implementację IQueryable DbSet — są one po prostu delegowanie do LINQ do dostawcy obiektów, która współdziała z listy\<T\>.</span><span class="sxs-lookup"><span data-stu-id="7699a-152">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="7699a-153">Firma Microsoft można utworzyć BlogService, oparte na typach Double nasze badania i upewnij się, że dane, które firma Microsoft wrócić z GetAllBlogs są uporządkowane według nazwy.</span><span class="sxs-lookup"><span data-stu-id="7699a-153">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

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
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(0 => data.GetEnumerator());

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

### <a name="testing-with-async-queries"></a><span data-ttu-id="7699a-154">Testowanie za pomocą zapytania asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="7699a-154">Testing with async queries</span></span>

<span data-ttu-id="7699a-155">Entity Framework 6 wprowadziliśmy zestaw metod rozszerzenia, które może służyć do asynchronicznego wykonywanie zapytania.</span><span class="sxs-lookup"><span data-stu-id="7699a-155">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="7699a-156">Przykładami tych metod ToListAsync FirstAsync, ForEachAsync itp.</span><span class="sxs-lookup"><span data-stu-id="7699a-156">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="7699a-157">Ponieważ Entity Framework zapytań korzystaj z LINQ, metody rozszerzające są definiowane na IQueryable i IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="7699a-157">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="7699a-158">Ponieważ są one przeznaczone tylko do użycia z programem Entity Framework możesz otrzymać następujący błąd przy próbie z nich korzystać na zapytanie LINQ, która nie jest zapytaniem Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="7699a-158">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="7699a-159">Źródło IQueryable nie implementuje IDbAsyncEnumerable{0}.</span><span class="sxs-lookup"><span data-stu-id="7699a-159">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="7699a-160">Tylko źródła, które implementują IDbAsyncEnumerable może służyć do operacji asynchronicznych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7699a-160">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="7699a-161">Aby uzyskać więcej informacji, zobacz [ http://go.microsoft.com/fwlink/?LinkId=287068 ](http://go.microsoft.com/fwlink/?LinkId=287068).</span><span class="sxs-lookup"><span data-stu-id="7699a-161">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](http://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="7699a-162">Podczas gdy metody asynchroniczne są obsługiwane tylko przy uruchamianiu kwerendę EF, można ich używać w testu jednostkowego, gdy uruchomiona przed w pamięci testowanie double DbSet.</span><span class="sxs-lookup"><span data-stu-id="7699a-162">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="7699a-163">Aby można było używać metod asynchronicznych, musimy utworzyć DbAsyncQueryProvider w pamięci do przetworzenia zapytania asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="7699a-163">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="7699a-164">O ile można skonfigurować dostawcę zapytania za pomocą Moq, jest znacznie łatwiejsze utworzyć implementacja podwójnego testowego w kodzie.</span><span class="sxs-lookup"><span data-stu-id="7699a-164">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="7699a-165">Kod dla tej implementacji jest następująca:</span><span class="sxs-lookup"><span data-stu-id="7699a-165">The code for this implementation is as follows:</span></span>  

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

<span data-ttu-id="7699a-166">Teraz, gdy dostawca zapytania asynchroniczne możemy napisać test jednostkowy dla naszej nowej metody GetAllBlogsAsync.</span><span class="sxs-lookup"><span data-stu-id="7699a-166">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

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
