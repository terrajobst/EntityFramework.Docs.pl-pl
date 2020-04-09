---
title: Powiązanie danych za pomocą WPF - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639144"
---
> [!IMPORTANT]
> **Ten dokument jest prawidłowy tylko dla programu WPF w programie .NET Framework**
>
> W tym dokumencie opisano powiązanie danych dla WPF WPF w programie .NET Framework. W przypadku nowych projektów .NET Core zaleca się użycie [ef core](/ef/core) zamiast entity framework 6. Dokumentacja wiązania danych w EF Core jest śledzona w [#778 problem](https://github.com/dotnet/EntityFramework.Docs/issues/778).

# <a name="databinding-with-wpf"></a>Powiązanie danych za pomocą WPF
W tym przewodniku krok po kroku pokazano, jak powiązać typy POCO do formantów WPF w formularzu "master-detail". Aplikacja używa interfejsów API frameworka do wypełniania obiektów danymi z bazy danych, śledzenia zmian i utrwalania danych w bazie danych.

Model definiuje dwa typy, które uczestniczą w **Category** relacji jeden\\do wielu: Kategoria (główny główny) i **Produkt** (szczegóły zależne).\\ Następnie narzędzia programu Visual Studio są używane do powiązania typów zdefiniowanych w modelu do formantów WPF. Struktura powiązania danych WPF umożliwia nawigację między powiązanymi obiektami: wybranie wierszy w widoku głównym powoduje, że widok szczegółów do aktualizacji z odpowiednimi danymi podrzędnymi.

Zrzuty ekranu i listy kodów w tym instruktażu są pobierane z programu Visual Studio 2013, ale można ukończyć ten przewodnik za pomocą programu Visual Studio 2012 lub Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Użyj opcji "Obiekt" do tworzenia źródeł danych WPF

W poprzedniej wersji entity framework uzyliśmy przy użyciu **opcji bazy danych** podczas tworzenia nowego źródła danych na podstawie modelu utworzonego za pomocą projektanta EF. Stało się tak, ponieważ projektant będzie generować kontekst, który pochodzi z ObjectContext i klasy jednostek, które pochodzą z EntityObject. Za pomocą opcji bazy danych pomoże napisać najlepszy kod do interakcji z tej powierzchni interfejsu API.

Projektanci EF dla programu Visual Studio 2012 i Visual Studio 2013 generują kontekst, który wywodzi się z DbContext wraz z prostymi klasami jednostek POCO. W programie Visual Studio 2010 zaleca się zamianę na szablon generowania kodu, który używa DbContext, jak opisano w dalszej części tego przewodnika.

Podczas korzystania z powierzchni interfejsu API DbContext należy użyć **opcji Obiekt** podczas tworzenia nowego źródła danych, jak pokazano w tym instruktażu.

W razie potrzeby można [przywrócić do generowania kodu opartego na ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) dla modeli utworzonych za pomocą projektanta EF.

## <a name="pre-requisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik, musisz mieć zainstalowany program Visual Studio 2013, Visual Studio 2012 lub Visual Studio 2010.

Jeśli używasz programu Visual Studio 2010, należy również zainstalować NuGet. Aby uzyskać więcej informacji, zobacz [Instalowanie nuget](https://docs.microsoft.com/nuget/install-nuget-client-tools).  

## <a name="create-the-application"></a>Tworzenie aplikacji

-   Otwórz program Visual Studio.
-   **Plik&gt; —&gt; nowy — projekt....**
-   Wybieranie **systemu Windows** w lewym okienku i **WPFApplication** w prawym okienku
-   Wprowadź **WPFwithEFSample** jako nazwę
-   Wybierz **przycisk OK**

## <a name="install-the-entity-framework-nuget-package"></a>Instalowanie pakietu NuGet framework encji

-   W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **WinFormswithEFSample**
-   Wybierz **pozycję Zarządzaj pakietami NuGet...**
-   W oknie dialogowym Zarządzanie pakietami NuGet wybierz kartę **Online** i wybierz pakiet **EntityFramework**
-   Kliknij **pozycję Zainstaluj**  
    >[!NOTE]
    > Oprócz zestawu EntityFramework dodano również odwołanie do System.ComponentModel.DataAnnotations. Jeśli projekt ma odwołanie do System.Data.Entity, zostanie on usunięty po zainstalowaniu pakietu EntityFramework. Zestaw System.Data.Entity nie jest już używany dla aplikacji Entity Framework 6.

## <a name="define-a-model"></a>Definiowanie modelu

W tym instruktażu można wybrać do zaimplementowania modelu przy użyciu code first lub projektanta EF. Ukończ jedną z dwóch poniższych sekcji.

### <a name="option-1-define-a-model-using-code-first"></a>Opcja 1: Najpierw zdefiniuj model przy użyciu kodu

W tej sekcji pokazano, jak utworzyć model i skojarzoną z nim bazę danych przy użyciu kodu najpierw. Przejdź do następnej sekcji **(Opcja 2: Zdefiniuj model przy użyciu bazy danych najpierw),** jeśli wolisz użyć bazy danych najpierw do odtworzenia modelu z bazy danych przy użyciu projektanta EF

Podczas korzystania z code first rozwoju zwykle rozpocząć od pisania .NET Framework klas, które definiują model koncepcyjny (domena).

-   Dodaj nową klasę do **WPFwithEFSample:**
    -   Kliknij prawym przyciskiem myszy nazwę projektu
    -   Wybierz pozycję **Dodaj**, a następnie **pozycję Nowy element**
    -   Wybierz **klasę** i wprowadź **produkt** dla nazwy klasy
-   Zastąp definicję klasy **produktu** następującym kodem:

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

**Właściwość Produkty** w **Category** klasy i **Category** właściwości na **Product** klasy są właściwości nawigacji. W entity framework właściwości nawigacji umożliwiają poruszanie się po relacji między dwoma typami jednostek.

Oprócz definiowania jednostek, należy zdefiniować klasę, która pochodzi z DbContext&lt;i udostępnia&gt; właściwości DbSet TEntity. Właściwości DbSet&lt;TEntity&gt; niech kontekst wiedzieć, jakie typy, które mają zostać uwzględnione w modelu.

Wystąpienie typu pochodnego DbContext zarządza obiektami jednostki w czasie wykonywania, który obejmuje wypełnianie obiektów danymi z bazy danych, śledzenie zmian i utrwalanie danych do bazy danych.

-   Dodaj nową klasę **ProductContext** do projektu z następującą definicją:

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

Skompiluj projekt.

### <a name="option-2-define-a-model-using-database-first"></a>Opcja 2: Najpierw zdefiniuj model przy użyciu bazy danych

W tej sekcji pokazano, jak używać najpierw bazy danych do inżynierii wstecznej modelu z bazy danych przy użyciu projektanta EF. Jeśli poprzednia sekcja została ukończona (**Opcja 1: Zdefiniuj model przy użyciu najpierw kodu),** pomiń tę sekcję i przejdź bezpośrednio do sekcji **Ładowanie z opóźnieniem.**

#### <a name="create-an-existing-database"></a>Tworzenie istniejącej bazy danych

Zazwyczaj podczas kierowania istniejącej bazy danych zostanie już utworzona, ale w tym instruktażu musimy utworzyć bazę danych, aby uzyskać dostęp.

Serwer bazy danych zainstalowany w programie Visual Studio różni się w zależności od zainstalowanej wersji programu Visual Studio:

-   Jeśli używasz programu Visual Studio 2010, będziesz tworzyć bazę danych PROGRAMU SQL Express.
-   Jeśli używasz programu Visual Studio 2012, a następnie będziesz tworzyć bazy danych [LocalDB.](https://msdn.microsoft.com/library/hh510202.aspx)

Przejdźmy dalej i wygenerujmy bazę danych.

-   **Widok&gt; — Eksplorator serwera**
-   Kliknij prawym przyciskiem myszy na **Połączenia danych -&gt; Dodaj połączenie...**
-   Jeśli nie masz połączenia z bazą danych z Eksploratora serwera, zanim będziesz musiał wybrać program Microsoft SQL Server jako źródło danych

    ![Zmienianie źródła danych](~/ef6/media/changedatasource.png)

-   Połącz się z localdb lub SQL Express, w zależności od tego, który z nich został zainstalowany, i wprowadź **produkty** jako nazwę bazy danych

    ![Dodawanie lokalnej bazy danych połączeń](~/ef6/media/addconnectionlocaldb.png)

    ![Dodaj ekspres połączenia](~/ef6/media/addconnectionexpress.png)

-   Wybierz **przycisk OK,** a pojawi się pytanie, czy chcesz utworzyć nową bazę danych, wybierz **tak**

    ![Create Database](~/ef6/media/createdatabase.png)

-   Nowa baza danych pojawi się teraz w Eksploratorze serwera, kliknij ją prawym przyciskiem myszy i wybierz **pozycję Nowa kwerenda**
-   Skopiuj następujący SQL do nowej kwerendy, a następnie kliknij prawym przyciskiem myszy kwerendę i wybierz polecenie **Wykonaj**

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a>Model inżyniera odwrotnego

Mamy zamiar korzystać z Entity Framework Designer, który jest dołączony do programu Visual Studio, aby utworzyć nasz model.

-   **Projekt&gt; — dodaj nowy element...**
-   Wybierz **pozycję Dane** z lewego menu, a następnie ADO.NET modelu danych **encji**
-   Wprowadź **ProductModel** jako nazwę i kliknij **przycisk OK**
-   Spowoduje to uruchomienie **Kreatora modeli danych encji**
-   Wybierz **pozycję Generuj z bazy danych** i kliknij przycisk **Dalej**

    ![Wybierz zawartość modelu](~/ef6/media/choosemodelcontents.png)

-   Wybierz połączenie z bazą danych utworzoną w pierwszej sekcji, wprowadź **ProductContext** jako nazwę ciągu połączenia i kliknij przycisk **Dalej**

    ![Wybierz połączenie](~/ef6/media/chooseyourconnection.png)

-   Kliknij pole wyboru obok pozycji "Tabele", aby zaimportować wszystkie tabele i kliknij przycisk "Zakończ"

    ![Wybierz swoje obiekty](~/ef6/media/chooseyourobjects.png)

Po zakończeniu procesu inżynierii odwrotnej nowy model jest dodawany do projektu i otwiera się do wyświetlania w Entity Framework Designer. Plik App.config został również dodany do projektu ze szczegółami połączenia dla bazy danych.

#### <a name="additional-steps-in-visual-studio-2010"></a>Dodatkowe kroki w programie Visual Studio 2010

Jeśli pracujesz w programie Visual Studio 2010, należy zaktualizować projektanta EF do generowania kodu EF6.

-   Kliknij prawym przyciskiem myszy puste miejsce modelu w projektancie EF i wybierz pozycję **Dodaj element generowania kodu...**
-   Wybierz **szablony online** z lewego menu i wyszukaj **DbContext**
-   Wybierz **EF 6.x DbContext\#Generator dla C ,** wprowadź **ProductsModel** jako nazwę i kliknij przycisk Dodaj

#### <a name="updating-code-generation-for-data-binding"></a>Aktualizowanie generowania kodu dla powiązania danych

EF generuje kod z modelu przy użyciu szablonów T4. Szablony dostarczane z programem Visual Studio lub pobrane z galerii programu Visual Studio są przeznaczone do użytku ogólnego. Oznacza to, że jednostki generowane z tych szablonów mają proste właściwości ICollection&lt;T.&gt; Jednak podczas wykonywania powiązania danych przy użyciu WPF WPF jest pożądane, aby użyć **ObservableCollection** dla właściwości kolekcji, dzięki czemu WPF można śledzić zmiany wprowadzone do kolekcji. W tym celu będziemy modyfikować szablony do korzystania z ObservableCollection.

-   Otwórz **Eksploratora rozwiązań** i znajdź plik **ProductModel.edmx**
-   Znajdź plik **ProductModel.tt,** który będzie zagnieżdżony pod plikiem ProductModel.edmx

    ![Szablon modelu produktu WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   Kliknij dwukrotnie plik ProductModel.tt, aby otworzyć go w edytorze Visual Studio
-   Znajdź i wymień dwa wystąpienia "**ICollection**" na "**ObservableCollection**". Znajdują się one w przybliżeniu na liniach 296 i 484.
-   Znajdź i zastąp pierwsze wystąpienie**hashsetu**na "**ObservableCollection**". To wystąpienie znajduje się w przybliżeniu na linii 50. **Nie** należy zastępować drugiego wystąpienia zestawu hashset znalezionego w dalszej części kodu.
-   Znajdź i zastąp jedyne wystąpienie "**System.Collections.Generic**" na "**System.Collections.ObjectModel**". Znajduje się on w przybliżeniu na linii 424.
-   Zapisz plik ProductModel.tt. Powinno to spowodować, że kod dla jednostek do ponownego wygenerowanie. Jeśli kod nie zostanie automatycznie wygenerowany, kliknij prawym przyciskiem myszy ProductModel.tt i wybierz "Uruchom narzędzie niestandardowe".

Jeśli teraz otworzysz plik Category.cs (który jest zagnieżdżony w obszarze ProductModel.tt), powinieneś zobaczyć, że kolekcja Products ma typ **&lt;ObservableCollection Product&gt;**.

Skompiluj projekt.

## <a name="lazy-loading"></a>Z opóźnieniem

**Właściwość Produkty** w **Category** klasy i **Category** właściwości na **Product** klasy są właściwości nawigacji. W entity framework właściwości nawigacji umożliwiają poruszanie się po relacji między dwoma typami jednostek.

EF umożliwia automatyczne ładowanie powiązanych jednostek z bazy danych przy pierwszym wejściu do właściwości nawigacji. W przypadku tego typu ładowania (nazywanego ładowaniem z opóźnieniem) należy pamiętać, że przy pierwszym dostępie do każdej właściwości nawigacji zostanie wykonana oddzielna kwerenda względem bazy danych, jeśli zawartość nie znajduje się jeszcze w kontekście.

Podczas korzystania z typów jednostek POCO EF osiąga ładowanie z opóźnieniem, tworząc wystąpienia pochodnych typów proxy w czasie wykonywania, a następnie zastępując właściwości wirtualne w klasach, aby dodać hak ładowania. Aby uzyskać opóźnienie ładowania powiązanych obiektów, należy zadeklarować metodyciwości nawigacji jako **publiczne** i **wirtualne** **(możliwe do zastąpienia** w języku Visual Basic), a klasa nie może być **zapieczętowana** **(NotOverridable** w języku Visual Basic). Podczas korzystania z bazy danych pierwsze właściwości nawigacji są automatycznie wirtualne, aby włączyć ładowanie z opóźnieniem. W sekcji Code First zdecydowaliśmy się uczynić właściwości nawigacji wirtualnymi z tego samego powodu

## <a name="bind-object-to-controls"></a>Obiekt powiązania z formanty

Dodaj klasy, które są zdefiniowane w modelu jako źródła danych dla tej aplikacji WPF.

-   Kliknij dwukrotnie plik **MainWindow.xaml** w Eksploratorze rozwiązań, aby otworzyć formularz główny
-   Z menu głównego wybierz **Project -&gt; Dodaj nowe źródło danych ...**
    (w programie Visual Studio 2010 należy wybrać **opcję Dane —&gt; Dodaj nowe źródło danych...**)
-   W obszarze Wybieranie okna typem źródła danych wybierz pozycję **Obiekt** i kliknij przycisk **Dalej**
-   W oknie dialogowym Wybieranie obiektów danych rozwiń **wpfwithefsample** dwa razy i wybierz **kategorię**  
    *Nie ma potrzeby wybierania źródła danych **produktu,** ponieważ do niego przejdziemy za pośrednictwem właściwości **Product**'s w źródle danych **kategoria***  

    ![Wybieranie obiektów danych](~/ef6/media/selectdataobjects.png)

-   Kliknij **pozycję Zakończ.**
-   Okno Źródła danych jest otwierane obok okna MainWindow.xaml *Jeśli okno Źródła danych nie jest wyświetlane, wybierz pozycję Widok — **&gt; Inne źródła&gt; danych systemu Windows** *
-   Naciśnij ikonę pinezki, aby okno Źródła danych nie było automatycznie ukrywane. Może być konieczne naciśnięcie przycisku odświeżania, jeśli okno było już widoczne.

    ![Źródła danych](~/ef6/media/datasources.png)

-   Zaznacz źródło danych **Kategoria** i przeciągnij je w formularzu.

Następujące zdarzenia miały miejsce, gdy przeciągliśmy to źródło:

-   Do XAML dodano zasób **CategoryViewSource** i formant **categoryDataGrid** 
-   Właściwość DataContext w nadrzędnym elemencie Siatki została ustawiona na "{StaticResource **categoryViewSource** }".Zasób **CategoryViewSource** służy jako źródło\\powiązania dla zewnętrznego elementu siatki nadrzędnej. Wewnętrzne elementy siatki dziedziczą następnie wartość DataContext z nadrzędnej siatki (właściwość CategoryDataGrid's ItemsSource jest ustawiona na "{Binding}")

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a>Dodawanie siatki szczegółów

Teraz, gdy mamy siatkę do wyświetlania kategorii dodajmy siatkę szczegółów, aby wyświetlić skojarzone produkty.

-   Wybierz **Products** właściwość spod źródła danych **kategoria** i przeciągnij go w formularzu.
    -   Do xaml są dodawane zasoby **CategoryProductsViewSource** i **siatka productDataGrid**
    -   Ścieżka powiązania dla tego zasobu jest ustawiona na Produkty
    -   Struktura wiązania danych WPF zapewnia, że tylko produkty związane z wybraną kategorią są wyświetlane w **productDataGrid**
-   W przyborniku przeciągnij **przycisk** do formularza. Ustaw **właściwość Nazwa** na **buttonSave** i **Content,** właściwość **Zapisz**.

Formularz powinien wyglądać podobnie do tego:

![Projektant](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Dodaj kod, który obsługuje interakcję z danymi

Nadszedł czas, aby dodać niektóre programy obsługi zdarzeń do okna głównego.

-   W oknie XAML kliknij element ** &lt;Okno,** w tym oknie wybiera okno główne
-   W oknie **Właściwości** wybierz pozycję **Zdarzenia** w prawym górnym rogu, a następnie kliknij dwukrotnie pole tekstowe po prawej stronie etykiety **Loaded**

    ![Właściwości okna głównego](~/ef6/media/mainwindowproperties.png)

-   Dodaj również zdarzenie **Kliknij** dla przycisku **Zapisz,** klikając dwukrotnie przycisk Zapisz w projektancie. 

Spowoduje to, że do kodu za formularzem, będziemy teraz edytować kod, aby użyć ProductContext do wykonywania dostępu do danych. Zaktualizuj kod mainwindow, jak pokazano poniżej.

Kod deklaruje długotrwałe wystąpienie **ProduktuContext**. **Obiekt ProductContext** służy do wykonywania zapytań i zapisywania danych w bazie danych. **Dispose()** w **ProductContext wystąpienie** jest następnie wywoływana z overridden **OnClosing** metody.Komentarze do kodu zawierają szczegółowe informacje o tym, co robi kod.

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a>Testowanie aplikacji WPF

-   Skompiluj i uruchom aplikację. Jeśli użyto najpierw kodu, wtedy zobaczysz, że **wpfwithEFSample.ProductContext** bazy danych jest tworzony dla Ciebie.
-   Wprowadź nazwę kategorii w górnej siatce i nazwy produktów w dolnej siatce *Nie wprowadzaj niczego w kolumnach identyfikatorów, ponieważ klucz podstawowy jest generowany przez bazę danych*

    ![Okno główne z nowymi kategoriami i produktami](~/ef6/media/screen1.png)

-   Naciśnij przycisk **Zapisz,** aby zapisać dane w bazie danych

Po wywołaniu **SaveChanges()** programu DbContext identyfikatory są wypełniane wartościami generowanymi przez bazę danych. Ponieważ po **savechanges()** nazwaliśmy **refresh()** formanty **DataGrid** są również aktualizowane o nowe wartości.

![Okno główne z wypełnionymi identyfikatorami](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby dowiedzieć się więcej na temat powiązania danych z kolekcjami przy użyciu WPF, zobacz [ten temat](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) w dokumentacji WPF.  
