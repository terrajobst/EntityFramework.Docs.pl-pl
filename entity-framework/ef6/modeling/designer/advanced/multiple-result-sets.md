---
title: Procedury składowane z wieloma zestawami wyników — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: 098ed88ba52e211965baf3660f0e51bd74c71efd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418705"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="3cb63-102">Procedury składowane z wieloma zestawami wyników</span><span class="sxs-lookup"><span data-stu-id="3cb63-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="3cb63-103">Czasami w przypadku korzystania z procedur składowanych należy zwrócić więcej niż jeden zestaw wyników.</span><span class="sxs-lookup"><span data-stu-id="3cb63-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="3cb63-104">Ten scenariusz jest często używany do zmniejszenia liczby rejsów rundy bazy danych wymaganych do utworzenia jednego ekranu.</span><span class="sxs-lookup"><span data-stu-id="3cb63-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span><span data-ttu-id="3cb63-105"> Przed EF5, Entity Framework zezwoli na wywoływanie procedury składowanej, ale zwróci tylko pierwszy zestaw wyników do kodu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="3cb63-105"> Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="3cb63-106">W tym artykule przedstawiono dwa sposoby korzystania z programu w celu uzyskania dostępu do więcej niż jednego zestawu wyników z procedury składowanej w Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3cb63-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="3cb63-107">Taki, który używa tylko kodu i współpracuje z pierwszym kodem i programem Dr Designer, który działa tylko z programem Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="3cb63-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="3cb63-108">Obsługa narzędzi i interfejsów API dla tego działania powinna zostać zwiększona w przyszłych wersjach Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3cb63-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="3cb63-109">Modelowanie</span><span class="sxs-lookup"><span data-stu-id="3cb63-109">Model</span></span>

<span data-ttu-id="3cb63-110">W przykładach w tym artykule jest używany podstawowy model blogu i ogłoszeń, w których blog zawiera wiele ogłoszeń, a wpis należy do jednego bloga.</span><span class="sxs-lookup"><span data-stu-id="3cb63-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="3cb63-111">Będziemy używać procedury składowanej w bazie danych, która zwraca wszystkie blogi i wpisy, podobnie jak w przypadku:</span><span class="sxs-lookup"><span data-stu-id="3cb63-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="3cb63-112">Uzyskiwanie dostępu do wielu zestawów wyników przy użyciu kodu</span><span class="sxs-lookup"><span data-stu-id="3cb63-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="3cb63-113">Możemy użyć kodu, aby wydać pierwotne polecenie SQL w celu wykonania naszej procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="3cb63-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="3cb63-114">Zaletą tego podejścia jest to, że działa ona zarówno z kodem, jak i programem Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="3cb63-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="3cb63-115">Aby można było korzystać z wielu zestawów wyników, musimy porzucić interfejs API ObjectContext przy użyciu interfejsu IObjectContextAdapter.</span><span class="sxs-lookup"><span data-stu-id="3cb63-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="3cb63-116">Po utworzeniu obiektu ObjectContext możemy użyć metody tłumaczenia, aby przetłumaczyć wyniki naszej procedury składowanej na jednostki, które mogą być śledzone i używane w EF jako normalne.</span><span class="sxs-lookup"><span data-stu-id="3cb63-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="3cb63-117">Poniższy przykład kodu demonstruje to w działaniu.</span><span class="sxs-lookup"><span data-stu-id="3cb63-117">The following code sample demonstrates this in action.</span></span>

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

<span data-ttu-id="3cb63-118">Metoda tłumaczenia akceptuje czytnik otrzymany po wykonaniu procedury, nazwy obiektu EntitySet i MergeOption.</span><span class="sxs-lookup"><span data-stu-id="3cb63-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="3cb63-119">Nazwa obiektu EntitySet będzie taka sama jak Właściwość Nieogólnymi w kontekście pochodnym.</span><span class="sxs-lookup"><span data-stu-id="3cb63-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="3cb63-120">Wyliczenie MergeOption określa, jak są obsługiwane wyniki, jeśli ta sama jednostka już istnieje w pamięci.</span><span class="sxs-lookup"><span data-stu-id="3cb63-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="3cb63-121">W tym miejscu wykonujemy iterację kolekcji blogów przed wywołaniem NextResult. jest to ważne w przypadku powyższego kodu, ponieważ pierwszy zestaw wyników musi być użyty przed przejściem do następnego zestawu wyników.</span><span class="sxs-lookup"><span data-stu-id="3cb63-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="3cb63-122">Po wywołaniu dwóch metod translacji blog i wpisy są śledzone przez EF w taki sam sposób jak inne jednostki i dlatego mogą być modyfikowane lub usuwane i zapisywane jako normalne.</span><span class="sxs-lookup"><span data-stu-id="3cb63-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="3cb63-123">EF nie przyjmuje mapowania do konta podczas tworzenia jednostek przy użyciu metody tłumaczenia.</span><span class="sxs-lookup"><span data-stu-id="3cb63-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="3cb63-124">Będzie po prostu odpowiadać nazwom kolumn w zestawie wyników z nazwami właściwości w klasach.</span><span class="sxs-lookup"><span data-stu-id="3cb63-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="3cb63-125">Jeśli jest włączone ładowanie z opóźnieniem, uzyskanie dostępu do właściwości Posts w jednej z jednostek blogu spowoduje, że program EF nawiąże połączenie z bazą danych w celu opóźnieniem załadowania wszystkich wpisów, nawet jeśli zostały już załadowane.</span><span class="sxs-lookup"><span data-stu-id="3cb63-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="3cb63-126">Wynika to z faktu, że EF nie wie, czy załadowano wszystkie wpisy, czy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3cb63-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="3cb63-127">Aby tego uniknąć, należy wyłączyć ładowanie z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="3cb63-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="3cb63-128">Wiele zestawów wyników z konfiguracją w EDMX</span><span class="sxs-lookup"><span data-stu-id="3cb63-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="3cb63-129">Aby można było skonfigurować wiele zestawów wyników w EDMX, należy wskazać element docelowy .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="3cb63-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="3cb63-130">Jeśli celem jest program .NET 4,0, można użyć metody opartej na kodzie pokazanej w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3cb63-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="3cb63-131">Jeśli używasz programu Dr Designer, możesz również zmodyfikować model, aby wie o różnych zestawach wyników, które zostaną zwrócone.</span><span class="sxs-lookup"><span data-stu-id="3cb63-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="3cb63-132">Przede wszystkim należy wiedzieć, że narzędzia nie obsługują wielu zestawów wyników, więc trzeba ręcznie edytować plik EDMX.</span><span class="sxs-lookup"><span data-stu-id="3cb63-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="3cb63-133">Edytowanie pliku edmx, tak jak to będzie działało, ale spowoduje również przerwanie weryfikacji modelu w programie VS.</span><span class="sxs-lookup"><span data-stu-id="3cb63-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="3cb63-134">Dlatego jeśli sprawdzasz model, zawsze pojawią się błędy.</span><span class="sxs-lookup"><span data-stu-id="3cb63-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="3cb63-135">Aby to zrobić, należy dodać procedurę składowaną do modelu, tak jak w przypadku pojedynczego zapytania zestawu wyników.</span><span class="sxs-lookup"><span data-stu-id="3cb63-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="3cb63-136">Po wybraniu tej opcji należy kliknąć prawym przyciskiem myszy Model i wybrać polecenie **Otwórz za pomocą.**</span><span class="sxs-lookup"><span data-stu-id="3cb63-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="3cb63-137">następnie **XML**</span><span class="sxs-lookup"><span data-stu-id="3cb63-137">then **Xml**</span></span>

    ![Otwórz jako](~/ef6/media/openas.png)

<span data-ttu-id="3cb63-139">Po otwarciu modelu jako XML należy wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="3cb63-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="3cb63-140">Znajdź typ złożony i import funkcji w modelu:</span><span class="sxs-lookup"><span data-stu-id="3cb63-140">Find the complex type and function import in your model:</span></span>

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

 

-   <span data-ttu-id="3cb63-141">Usuń typ złożony</span><span class="sxs-lookup"><span data-stu-id="3cb63-141">Remove the complex type</span></span>
-   <span data-ttu-id="3cb63-142">Zaktualizuj funkcję import, tak aby była mapowana na jednostki, w naszym przypadku będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="3cb63-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="3cb63-143">Oznacza to, że model, który procedura składowana zwróci dwie kolekcje, jeden z wpisów w blogu i jeden z wpisów post.</span><span class="sxs-lookup"><span data-stu-id="3cb63-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="3cb63-144">Znajdź element mapowania funkcji:</span><span class="sxs-lookup"><span data-stu-id="3cb63-144">Find the function mapping element:</span></span>

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

-   <span data-ttu-id="3cb63-145">Zastąp mapowanie wyników jednym dla każdej zwracanej jednostki, na przykład:</span><span class="sxs-lookup"><span data-stu-id="3cb63-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

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

<span data-ttu-id="3cb63-146">Istnieje również możliwość mapowania zestawów wyników do typów złożonych, takich jak te utworzone domyślnie.</span><span class="sxs-lookup"><span data-stu-id="3cb63-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="3cb63-147">W tym celu należy utworzyć nowy typ złożony, zamiast usuwać go i użyć typów złożonych wszędzie tam, gdzie użyto nazw jednostek w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="3cb63-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="3cb63-148">Po zmianie tych mapowań można zapisać model i wykonać następujący kod w celu użycia procedury składowanej:</span><span class="sxs-lookup"><span data-stu-id="3cb63-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

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
> <span data-ttu-id="3cb63-149">Jeśli ręcznie edytujesz plik EDMX dla modelu, zostanie on nadpisany, jeśli kiedykolwiek ponownie wygenerujesz model z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3cb63-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="3cb63-150">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="3cb63-150">Summary</span></span>

<span data-ttu-id="3cb63-151">W tym miejscu przedstawiono dwie różne metody uzyskiwania dostępu do wielu zestawów wyników przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3cb63-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="3cb63-152">Oba te elementy są równie ważne, w zależności od sytuacji i preferencji, a następnie należy wybrać ten, który jest najlepszy do Twoich potrzeb.</span><span class="sxs-lookup"><span data-stu-id="3cb63-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="3cb63-153">Planowane jest, aby obsługa wielu zestawów wyników została ulepszona w przyszłych wersjach Entity Framework i wykonywanie kroków opisanych w tym dokumencie nie będzie już konieczne.</span><span class="sxs-lookup"><span data-stu-id="3cb63-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
