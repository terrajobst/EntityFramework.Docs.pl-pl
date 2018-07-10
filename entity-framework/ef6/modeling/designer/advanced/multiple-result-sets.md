---
title: Procedury składowane z wielu zestawów wyników — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
caps.latest.revision: 3
ms.openlocfilehash: 68d544b0c553868ad1ff36cd24db19cff08db073
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912857"
---
# <a name="stored-procedures-with-multiple-result-sets"></a>Procedury składowane z wielu zestawów wyników
Czasami w przypadku, gdy przy użyciu przechowywanych procedur należy zwrócić więcej niż jeden wynik jest ustawiona. Ten scenariusz jest najczęściej używany do zmniejszenia liczby bazy danych rund wymagane do redagowania na jednym ekranie. Przed EF5 platformy Entity Framework pozwoliłoby procedura składowana wywoływana, ale zwróci tylko pierwszy zestaw wyników do kodu wywołującego.

W tym artykule przedstawiono dwie metody, których można uzyskać dostęp do więcej niż jeden zestaw wyników z procedury składowanej platformy Entity Framework. Taki, który używa tylko kodu i współdziała z obu kod najpierw i projektancie platformy EF i taki, który działa tylko w Projektancie platformy EF. Narzędzia i obsługa interfejsu API dla tej powinna zwiększyć w przyszłych wersjach programu Entity Framework.

## <a name="model"></a>Model

Przykłady w niniejszym artykule użyć podstawowa blogu i modelu wpisów, gdzie blogu ma wiele wpisów i wpis należy do jednego blogu. Firma Microsoft użyje procedurę składowaną w bazie danych, które zwraca wszystkie blogów i wpisów, podobnie do następującej:

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>Uzyskiwanie dostępu do wielu wyników ustawia przy użyciu kodu

Firma Microsoft może wykonać kod użycia do wystawiania pierwotne polecenia SQL do wykonywania naszego procedury składowanej. Zaletą tego podejścia jest to, że działa zarówno kod najpierw i projektancie platformy EF.

Aby uzyskać wynik wielu ustawia pracy potrzebnych do spadku API obiektu ObjectContext za pomocą interfejsu IObjectContextAdapter.

Gdy będziemy już mieć obiektu ObjectContext, a następnie możemy użyć metody Translate do translacji wyników naszego procedury składowanej do jednostek, które mogą być śledzone i używane w programie EF, jak zwykle. Poniższy przykładowy kod przedstawia to w działaniu.

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

Metoda Translate przyjmuje czytnika, które odebraliśmy, gdy firma Microsoft wykonywane procedury, nazwa obiektu EntitySet i MergeOption. Nazwa obiektu EntitySet będzie taka sama jak właściwość DbSet pochodnej kontekstu. Wyliczenie MergeOption kontroluje sposób obsługi wyniki, jeśli istnieje już tej samej jednostki w pamięci.

W tym miejscu możemy iterowania po kolekcji blogów przed nazywamy NextResult, jest to ważne, dany kod powyżej, ponieważ pierwszy zestaw wyników, muszą być przetworzone przed przejściem do następnego zestawu wyników.

Po dwóch przetłumaczyć metody są wywoływane, a następnie jednostek blogu i Post są śledzone przez EF w taki sam sposób jak inne jednostki, a zatem być zmodyfikowany lub usunięty i zapisywane jako normalny.

>[!NOTE]
> EF nie przyjmuje żadnego mapowania pod uwagę podczas tworzenia jednostki przy użyciu metody translacji. Będą one po prostu zgodne nazwy kolumn w zestawie wyników z nazwami właściwości w Twoich zajęciach.

>[!NOTE]
> Czy w przypadku ładowania z opóźnieniem, włączone, uzyskiwania dostępu do właściwości wpisy na jednej z jednostek blogu następnie EF połączy się bazy danych do załadowania opóźnieniem wszystkie wpisy, mimo że firma Microsoft zostały już załadowane je wszystkie. Jest to spowodowane EF nie wiedzieć, czy zostały załadowane wszystkie wpisy lub jeśli istnieje więcej w bazie danych. Jeśli chcesz tego uniknąć, a następnie konieczne będzie wyłączenie ładowania z opóźnieniem.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>Wiele zestawów wyników za pomocą skonfigurowanej w EDMX

>[!NOTE]
> Należy wskazać .NET Framework 4.5, aby mieć możliwość skonfigurowania wielu zestawów wyników w EDMX. Jeśli masz na celu platformy .NET 4.0, można użyć metody oparte na kodzie pokazano w poprzedniej sekcji.

Jeśli używasz projektancie platformy EF, można również zmodyfikować model, aby poinformować go o zestawach różne wyniki, które zostaną zwrócone. Warto zapoznać się przed ręcznie jest, że narzędzi nie jest wynikiem wielu ustawić wiedzieć, więc musisz ręcznie edytować plik edmx. Edytowanie pliku edmx, tak jak to działa, ale spowoduje również przerwanie sprawdzania poprawności modelu w programie VS. Dlatego jeśli Weryfikacja modelu będzie zawsze występują błędy.

-   W tym celu należy dodać procedurę składowaną do modelu, podobnie jak w przypadku pojedynczego wyniku kwerendy zestawu.
-   Po tym, będzie konieczne modelu kliknij prawym przyciskiem myszy i wybierz **Otwórz za pomocą...** następnie **Xml**

    ![OpenAs](~/ef6/media/openas.png)

Masz jeden raz modelu otwarty jako XML, a następnie należy wykonać następujące czynności:

-   Znajdź złożonych importu typ i funkcję w modelu:

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
-   Importowanie funkcji należy zaktualizować tak, że jest on mapowany do jednostek, w tym przypadku, który będzie wyglądać następująco:

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

Informuje modelu, czy procedura składowana zwróci dwie kolekcje, jeden z wpisów w blogu i jeden z wpisów post.

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

-   Zastąp mapowania wynik jednym dla każdej jednostki, które są zwracane, takie jak następujące:

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

Istnieje również możliwość mapowania typów złożonych, takiego jak utworzone domyślnie zestawów wyników. W tym celu możesz utworzyć nowy typ złożony, zamiast je, usuwania i używać złożone typy wszędzie, gdyby użyto nazwy jednostek w powyższych przykładach.

Po mapowania te zostały zmienione, można zapisać model i wykonaj następujący kod, aby użyć procedury składowanej:

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
> Ręczna Edycja pliku edmx dla modelu zostaną zastąpione, jeśli kiedykolwiek ponowne wygenerowanie modelu z bazy danych.

## <a name="summary"></a>Podsumowanie

Zostały tutaj pokazano dwa różne sposoby uzyskiwania dostępu do wielu wyników ustawia używający narzędzia Entity Framework. Obie z nich są równoważne w zależności od potrzeb i preferencji i wybrać ten, który wydaje się najlepiej w przypadku Twojej sytuacji. Planuje obsługę wielu wyników, których zestawy będą ulepszone w przyszłych wersjach programu Entity Framework i, wykonując kroki opisane w tym dokumencie nie będzie już konieczne.
