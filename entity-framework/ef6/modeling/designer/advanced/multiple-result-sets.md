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
# <a name="stored-procedures-with-multiple-result-sets"></a>Procedury składowane z wieloma zestawami wyników
Czasami w przypadku korzystania z procedur składowanych należy zwrócić więcej niż jeden zestaw wyników. Ten scenariusz jest często używany do zmniejszenia liczby rejsów rundy bazy danych wymaganych do utworzenia jednego ekranu. Przed EF5, Entity Framework zezwoli na wywoływanie procedury składowanej, ale zwróci tylko pierwszy zestaw wyników do kodu wywołującego.

W tym artykule przedstawiono dwa sposoby korzystania z programu w celu uzyskania dostępu do więcej niż jednego zestawu wyników z procedury składowanej w Entity Framework. Taki, który używa tylko kodu i współpracuje z pierwszym kodem i programem Dr Designer, który działa tylko z programem Dr Designer. Obsługa narzędzi i interfejsów API dla tego działania powinna zostać zwiększona w przyszłych wersjach Entity Framework.

## <a name="model"></a>Modelowanie

W przykładach w tym artykule jest używany podstawowy model blogu i ogłoszeń, w których blog zawiera wiele ogłoszeń, a wpis należy do jednego bloga. Będziemy używać procedury składowanej w bazie danych, która zwraca wszystkie blogi i wpisy, podobnie jak w przypadku:

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>Uzyskiwanie dostępu do wielu zestawów wyników przy użyciu kodu

Możemy użyć kodu, aby wydać pierwotne polecenie SQL w celu wykonania naszej procedury składowanej. Zaletą tego podejścia jest to, że działa ona zarówno z kodem, jak i programem Dr Designer.

Aby można było korzystać z wielu zestawów wyników, musimy porzucić interfejs API ObjectContext przy użyciu interfejsu IObjectContextAdapter.

Po utworzeniu obiektu ObjectContext możemy użyć metody tłumaczenia, aby przetłumaczyć wyniki naszej procedury składowanej na jednostki, które mogą być śledzone i używane w EF jako normalne. Poniższy przykład kodu demonstruje to w działaniu.

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

Metoda tłumaczenia akceptuje czytnik otrzymany po wykonaniu procedury, nazwy obiektu EntitySet i MergeOption. Nazwa obiektu EntitySet będzie taka sama jak Właściwość Nieogólnymi w kontekście pochodnym. Wyliczenie MergeOption określa, jak są obsługiwane wyniki, jeśli ta sama jednostka już istnieje w pamięci.

W tym miejscu wykonujemy iterację kolekcji blogów przed wywołaniem NextResult. jest to ważne w przypadku powyższego kodu, ponieważ pierwszy zestaw wyników musi być użyty przed przejściem do następnego zestawu wyników.

Po wywołaniu dwóch metod translacji blog i wpisy są śledzone przez EF w taki sam sposób jak inne jednostki i dlatego mogą być modyfikowane lub usuwane i zapisywane jako normalne.

>[!NOTE]
> EF nie przyjmuje mapowania do konta podczas tworzenia jednostek przy użyciu metody tłumaczenia. Będzie po prostu odpowiadać nazwom kolumn w zestawie wyników z nazwami właściwości w klasach.

>[!NOTE]
> Jeśli jest włączone ładowanie z opóźnieniem, uzyskanie dostępu do właściwości Posts w jednej z jednostek blogu spowoduje, że program EF nawiąże połączenie z bazą danych w celu opóźnieniem załadowania wszystkich wpisów, nawet jeśli zostały już załadowane. Wynika to z faktu, że EF nie wie, czy załadowano wszystkie wpisy, czy w bazie danych. Aby tego uniknąć, należy wyłączyć ładowanie z opóźnieniem.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>Wiele zestawów wyników z konfiguracją w EDMX

>[!NOTE]
> Aby można było skonfigurować wiele zestawów wyników w EDMX, należy wskazać element docelowy .NET Framework 4,5. Jeśli celem jest program .NET 4,0, można użyć metody opartej na kodzie pokazanej w poprzedniej sekcji.

Jeśli używasz programu Dr Designer, możesz również zmodyfikować model, aby wie o różnych zestawach wyników, które zostaną zwrócone. Przede wszystkim należy wiedzieć, że narzędzia nie obsługują wielu zestawów wyników, więc trzeba ręcznie edytować plik EDMX. Edytowanie pliku edmx, tak jak to będzie działało, ale spowoduje również przerwanie weryfikacji modelu w programie VS. Dlatego jeśli sprawdzasz model, zawsze pojawią się błędy.

-   Aby to zrobić, należy dodać procedurę składowaną do modelu, tak jak w przypadku pojedynczego zapytania zestawu wyników.
-   Po wybraniu tej opcji należy kliknąć prawym przyciskiem myszy Model i wybrać polecenie **Otwórz za pomocą.** następnie **XML**

    ![Otwórz jako](~/ef6/media/openas.png)

Po otwarciu modelu jako XML należy wykonać następujące czynności:

-   Znajdź typ złożony i import funkcji w modelu:

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

 

-   Usuń typ złożony
-   Zaktualizuj funkcję import, tak aby była mapowana na jednostki, w naszym przypadku będzie wyglądać następująco:

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

Oznacza to, że model, który procedura składowana zwróci dwie kolekcje, jeden z wpisów w blogu i jeden z wpisów post.

-   Znajdź element mapowania funkcji:

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

-   Zastąp mapowanie wyników jednym dla każdej zwracanej jednostki, na przykład:

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

Istnieje również możliwość mapowania zestawów wyników do typów złożonych, takich jak te utworzone domyślnie. W tym celu należy utworzyć nowy typ złożony, zamiast usuwać go i użyć typów złożonych wszędzie tam, gdzie użyto nazw jednostek w powyższym przykładzie.

Po zmianie tych mapowań można zapisać model i wykonać następujący kod w celu użycia procedury składowanej:

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
> Jeśli ręcznie edytujesz plik EDMX dla modelu, zostanie on nadpisany, jeśli kiedykolwiek ponownie wygenerujesz model z bazy danych.

## <a name="summary"></a>Podsumowanie

W tym miejscu przedstawiono dwie różne metody uzyskiwania dostępu do wielu zestawów wyników przy użyciu Entity Framework. Oba te elementy są równie ważne, w zależności od sytuacji i preferencji, a następnie należy wybrać ten, który jest najlepszy do Twoich potrzeb. Planowane jest, aby obsługa wielu zestawów wyników została ulepszona w przyszłych wersjach Entity Framework i wykonywanie kroków opisanych w tym dokumencie nie będzie już konieczne.
