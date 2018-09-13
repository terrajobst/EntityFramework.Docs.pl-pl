---
title: Procedury składowane z wielu zestawów wyników — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: 098ed88ba52e211965baf3660f0e51bd74c71efd
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489313"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="d169c-102">Procedury składowane z wielu zestawów wyników</span><span class="sxs-lookup"><span data-stu-id="d169c-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="d169c-103">Czasami w przypadku, gdy przy użyciu przechowywanych procedur należy zwrócić więcej niż jeden wynik jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="d169c-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="d169c-104">Ten scenariusz jest najczęściej używany do zmniejszenia liczby bazy danych rund wymagane do redagowania na jednym ekranie.</span><span class="sxs-lookup"><span data-stu-id="d169c-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span> <span data-ttu-id="d169c-105">Przed EF5 platformy Entity Framework pozwoliłoby procedura składowana wywoływana, ale zwróci tylko pierwszy zestaw wyników do kodu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="d169c-105">Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="d169c-106">W tym artykule przedstawiono dwie metody, których można uzyskać dostęp do więcej niż jeden zestaw wyników z procedury składowanej platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d169c-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="d169c-107">Taki, który używa tylko kodu i współdziała z obu kod najpierw i projektancie platformy EF i taki, który działa tylko w Projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="d169c-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="d169c-108">Narzędzia i obsługa interfejsu API dla tej powinna zwiększyć w przyszłych wersjach programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d169c-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="d169c-109">Model</span><span class="sxs-lookup"><span data-stu-id="d169c-109">Model</span></span>

<span data-ttu-id="d169c-110">Przykłady w niniejszym artykule użyć podstawowa blogu i modelu wpisów, gdzie blogu ma wiele wpisów i wpis należy do jednego blogu.</span><span class="sxs-lookup"><span data-stu-id="d169c-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="d169c-111">Firma Microsoft użyje procedurę składowaną w bazie danych, które zwraca wszystkie blogów i wpisów, podobnie do następującej:</span><span class="sxs-lookup"><span data-stu-id="d169c-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="d169c-112">Uzyskiwanie dostępu do wielu wyników ustawia przy użyciu kodu</span><span class="sxs-lookup"><span data-stu-id="d169c-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="d169c-113">Firma Microsoft może wykonać kod użycia do wystawiania pierwotne polecenia SQL do wykonywania naszego procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="d169c-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="d169c-114">Zaletą tego podejścia jest to, że działa zarówno kod najpierw i projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="d169c-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="d169c-115">Aby uzyskać wynik wielu ustawia pracy potrzebnych do spadku API obiektu ObjectContext za pomocą interfejsu IObjectContextAdapter.</span><span class="sxs-lookup"><span data-stu-id="d169c-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="d169c-116">Gdy będziemy już mieć obiektu ObjectContext, a następnie możemy użyć metody Translate do translacji wyników naszego procedury składowanej do jednostek, które mogą być śledzone i używane w programie EF, jak zwykle.</span><span class="sxs-lookup"><span data-stu-id="d169c-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="d169c-117">Poniższy przykładowy kod przedstawia to w działaniu.</span><span class="sxs-lookup"><span data-stu-id="d169c-117">The following code sample demonstrates this in action.</span></span>

``` csharp
    using (var db = new BloggingContext())
    {
        // If using Code First we need to make sure the model is built before we open the connection
        // This isn't required for models created with the EF Designer
        db.Database.Initialize(force: false);

        // Create a SQL command to execute the sproc
        var cmd = db.Database.Connection.CreateCommand();
        cmd.CommandText = "[dbo].[GetAllBlogsAndPosts]";

        try
        {

            db.Database.Connection.Open();
            // Run the sproc
            var reader = cmd.ExecuteReader();

            // Read Blogs from the first result set
            var blogs = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Blog>(reader, "Blogs", MergeOption.AppendOnly);   


            foreach (var item in blogs)
            {
                Console.WriteLine(item.Name);
            }        

            // Move to second result set and read Posts
            reader.NextResult();
            var posts = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Post>(reader, "Posts", MergeOption.AppendOnly);


            foreach (var item in posts)
            {
                Console.WriteLine(item.Title);
            }
        }
        finally
        {
            db.Database.Connection.Close();
        }
    }
```

<span data-ttu-id="d169c-118">Metoda Translate przyjmuje czytnika, które odebraliśmy, gdy firma Microsoft wykonywane procedury, nazwa obiektu EntitySet i MergeOption.</span><span class="sxs-lookup"><span data-stu-id="d169c-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="d169c-119">Nazwa obiektu EntitySet będzie taka sama jak właściwość DbSet pochodnej kontekstu.</span><span class="sxs-lookup"><span data-stu-id="d169c-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="d169c-120">Wyliczenie MergeOption kontroluje sposób obsługi wyniki, jeśli istnieje już tej samej jednostki w pamięci.</span><span class="sxs-lookup"><span data-stu-id="d169c-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="d169c-121">W tym miejscu możemy iterowania po kolekcji blogów przed nazywamy NextResult, jest to ważne, dany kod powyżej, ponieważ pierwszy zestaw wyników, muszą być przetworzone przed przejściem do następnego zestawu wyników.</span><span class="sxs-lookup"><span data-stu-id="d169c-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="d169c-122">Po dwóch przetłumaczyć metody są wywoływane, a następnie jednostek blogu i Post są śledzone przez EF w taki sam sposób jak inne jednostki, a zatem być zmodyfikowany lub usunięty i zapisywane jako normalny.</span><span class="sxs-lookup"><span data-stu-id="d169c-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="d169c-123">EF nie przyjmuje żadnego mapowania pod uwagę podczas tworzenia jednostki przy użyciu metody translacji.</span><span class="sxs-lookup"><span data-stu-id="d169c-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="d169c-124">Będą one po prostu zgodne nazwy kolumn w zestawie wyników z nazwami właściwości w Twoich zajęciach.</span><span class="sxs-lookup"><span data-stu-id="d169c-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="d169c-125">Czy w przypadku ładowania z opóźnieniem, włączone, uzyskiwania dostępu do właściwości wpisy na jednej z jednostek blogu następnie EF połączy się bazy danych do załadowania opóźnieniem wszystkie wpisy, mimo że firma Microsoft zostały już załadowane je wszystkie.</span><span class="sxs-lookup"><span data-stu-id="d169c-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="d169c-126">Jest to spowodowane EF nie wiedzieć, czy zostały załadowane wszystkie wpisy lub jeśli istnieje więcej w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d169c-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="d169c-127">Jeśli chcesz tego uniknąć, a następnie konieczne będzie wyłączenie ładowania z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="d169c-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="d169c-128">Wiele zestawów wyników za pomocą skonfigurowanej w EDMX</span><span class="sxs-lookup"><span data-stu-id="d169c-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="d169c-129">Należy wskazać .NET Framework 4.5, aby mieć możliwość skonfigurowania wielu zestawów wyników w EDMX.</span><span class="sxs-lookup"><span data-stu-id="d169c-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="d169c-130">Jeśli masz na celu platformy .NET 4.0, można użyć metody oparte na kodzie pokazano w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="d169c-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="d169c-131">Jeśli używasz projektancie platformy EF, można również zmodyfikować model, aby poinformować go o zestawach różne wyniki, które zostaną zwrócone.</span><span class="sxs-lookup"><span data-stu-id="d169c-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="d169c-132">Warto zapoznać się przed ręcznie jest, że narzędzi nie jest wynikiem wielu ustawić wiedzieć, więc musisz ręcznie edytować plik edmx.</span><span class="sxs-lookup"><span data-stu-id="d169c-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="d169c-133">Edytowanie pliku edmx, tak jak to działa, ale spowoduje również przerwanie sprawdzania poprawności modelu w programie VS.</span><span class="sxs-lookup"><span data-stu-id="d169c-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="d169c-134">Dlatego jeśli Weryfikacja modelu będzie zawsze występują błędy.</span><span class="sxs-lookup"><span data-stu-id="d169c-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="d169c-135">W tym celu należy dodać procedurę składowaną do modelu, podobnie jak w przypadku pojedynczego wyniku kwerendy zestawu.</span><span class="sxs-lookup"><span data-stu-id="d169c-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="d169c-136">Po tym, będzie konieczne modelu kliknij prawym przyciskiem myszy i wybierz **Otwórz za pomocą...**</span><span class="sxs-lookup"><span data-stu-id="d169c-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="d169c-137">następnie **Xml**</span><span class="sxs-lookup"><span data-stu-id="d169c-137">then **Xml**</span></span>

    ![Otwórz jako](~/ef6/media/openas.png)

<span data-ttu-id="d169c-139">Masz jeden raz modelu otwarty jako XML, a następnie należy wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="d169c-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="d169c-140">Znajdź złożonych importu typ i funkcję w modelu:</span><span class="sxs-lookup"><span data-stu-id="d169c-140">Find the complex type and function import in your model:</span></span>

``` xml
    <!-- CSDL content -->
    <edmx:ConceptualModels>

    ...

      <FunctionImport Name="GetAllBlogsAndPosts" ReturnType="Collection(BlogModel.GetAllBlogsAndPosts_Result)" />

    ...

      <ComplexType Name="GetAllBlogsAndPosts_Result">
        <Property Type="Int32" Name="BlogId" Nullable="false" />
        <Property Type="String" Name="Name" Nullable="false" MaxLength="255" />
        <Property Type="String" Name="Description" Nullable="true" />
      </ComplexType>

    ...

    </edmx:ConceptualModels>
```

 

-   <span data-ttu-id="d169c-141">Usuń typ złożony</span><span class="sxs-lookup"><span data-stu-id="d169c-141">Remove the complex type</span></span>
-   <span data-ttu-id="d169c-142">Importowanie funkcji należy zaktualizować tak, że jest on mapowany do jednostek, w tym przypadku, który będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="d169c-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="d169c-143">Informuje modelu, czy procedura składowana zwróci dwie kolekcje, jeden z wpisów w blogu i jeden z wpisów post.</span><span class="sxs-lookup"><span data-stu-id="d169c-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="d169c-144">Znajdź element mapowania funkcji:</span><span class="sxs-lookup"><span data-stu-id="d169c-144">Find the function mapping element:</span></span>

``` xml
    <!-- C-S mapping content -->
    <edmx:Mappings>

    ...

      <FunctionImportMapping FunctionImportName="GetAllBlogsAndPosts" FunctionName="BlogModel.Store.GetAllBlogsAndPosts">
        <ResultMapping>
          <ComplexTypeMapping TypeName="BlogModel.GetAllBlogsAndPosts_Result">
            <ScalarProperty Name="BlogId" ColumnName="BlogId" />
            <ScalarProperty Name="Name" ColumnName="Name" />
            <ScalarProperty Name="Description" ColumnName="Description" />
          </ComplexTypeMapping>
        </ResultMapping>
      </FunctionImportMapping>

    ...

    </edmx:Mappings>
```

-   <span data-ttu-id="d169c-145">Zastąp mapowania wynik jednym dla każdej jednostki, które są zwracane, takie jak następujące:</span><span class="sxs-lookup"><span data-stu-id="d169c-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

``` xml
    <ResultMapping>
      <EntityTypeMapping TypeName ="BlogModel.Blog">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="Name" ColumnName="Name" />
        <ScalarProperty Name="Description" ColumnName="Description" />
      </EntityTypeMapping>
    </ResultMapping>
    <ResultMapping>
      <EntityTypeMapping TypeName="BlogModel.Post">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="PostId" ColumnName="PostId"/>
        <ScalarProperty Name="Title" ColumnName="Title" />
        <ScalarProperty Name="Text" ColumnName="Text" />
      </EntityTypeMapping>
    </ResultMapping>
```

<span data-ttu-id="d169c-146">Istnieje również możliwość mapowania typów złożonych, takiego jak utworzone domyślnie zestawów wyników.</span><span class="sxs-lookup"><span data-stu-id="d169c-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="d169c-147">W tym celu możesz utworzyć nowy typ złożony, zamiast je, usuwania i używać złożone typy wszędzie, gdyby użyto nazwy jednostek w powyższych przykładach.</span><span class="sxs-lookup"><span data-stu-id="d169c-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="d169c-148">Po mapowania te zostały zmienione, można zapisać model i wykonaj następujący kod, aby użyć procedury składowanej:</span><span class="sxs-lookup"><span data-stu-id="d169c-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

``` csharp
    using (var db = new BlogEntities())
    {
        var results = db.GetAllBlogsAndPosts();

        foreach (var result in results)
        {
            Console.WriteLine("Blog: " + result.Name);
        }

        var posts = results.GetNextResult<Post>();

        foreach (var result in posts)
        {
            Console.WriteLine("Post: " + result.Title);
        }

        Console.ReadLine();
    }
```

>[!NOTE]
> <span data-ttu-id="d169c-149">Ręczna Edycja pliku edmx dla modelu zostaną zastąpione, jeśli kiedykolwiek ponowne wygenerowanie modelu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d169c-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="d169c-150">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="d169c-150">Summary</span></span>

<span data-ttu-id="d169c-151">Zostały tutaj pokazano dwa różne sposoby uzyskiwania dostępu do wielu wyników ustawia używający narzędzia Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d169c-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="d169c-152">Obie z nich są równoważne w zależności od potrzeb i preferencji i wybrać ten, który wydaje się najlepiej w przypadku Twojej sytuacji.</span><span class="sxs-lookup"><span data-stu-id="d169c-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="d169c-153">Planuje obsługę wielu wyników, których zestawy będą ulepszone w przyszłych wersjach programu Entity Framework i, wykonując kroki opisane w tym dokumencie nie będzie już konieczne.</span><span class="sxs-lookup"><span data-stu-id="d169c-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
