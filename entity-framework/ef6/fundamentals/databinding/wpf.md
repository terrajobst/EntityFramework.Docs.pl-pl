---
title: Powiązanie danych przy użyciu platformy WPF — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 0b1f4d5ea204cd80acf42caa499732610daa0e31
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994826"
---
# <a name="databinding-with-wpf"></a>Powiązanie danych przy użyciu platformy WPF
Ten przewodnik krok po kroku pokazano, jak powiązać POCO typy kontrolek WPF w postaci "elementy główne szczegóły". Aplikacja używa interfejsów API programu Entity Framework do wypełniania obiekty z danymi z bazy danych, śledzenie zmian i utrwalanie danych w bazie danych.

Model definiuje dwa typy, które uczestniczą w relacji jeden do wielu: **kategorii** (jednostki\\głównego) i **produktu** (zależne\\szczegółów). Następnie narzędzi programu Visual Studio są używane do powiązania typy zdefiniowane w modelu, który ma kontrolek WPF. Struktura powiązanie danych WPF umożliwia nawigacji między powiązane obiekty: Wybieranie wierszy w widoku głównego powoduje, że widok szczegółów aktualizacji za pomocą odpowiednich danych podrzędnych.

Zrzuty ekranu i zamieszczone w tym przewodniku są pobierane z programu Visual Studio 2013, ale możesz ukończyć ten przewodnik przy użyciu programu Visual Studio 2012 lub Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Użyj opcji "Object" do tworzenia źródeł danych w WPF

Za pomocą poprzedniej wersji programu Entity Framework użyliśmy zalecamy używanie **bazy danych** podczas tworzenia nowego źródła danych oparty na modelu utworzonych za pomocą projektancie platformy EF. Taka konfiguracja była ponieważ projektant wygeneruje kontekstu, który pochodzi od obiektu ObjectContext i klas jednostek, które pochodną EntityObject. Przy użyciu opcji bazy danych umożliwia pisanie kodu najlepsze do interakcji z tym powierzchni interfejsu API.

Projektant EF dla programu Visual Studio 2012 i Visual Studio 2013 generowanie kontekstu, który pochodzi od typu DbContext wraz z prostego klas obiektów POCO. Za pomocą programu Visual Studio 2010, zaleca się zamianę do szablonu generowania kodu, który używa typu DbContext, zgodnie z opisem w dalszej części tego przewodnika.

Korzystając z powierzchni interfejsu API typu DbContext należy użyć **obiektu** opcji podczas tworzenia nowego źródła danych, jak pokazano w tym przewodniku.

Jeśli to konieczne, możesz [powrócić do generowania kodu na podstawie obiektu ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) dla modeli utworzonych za pomocą projektanta EF.

## <a name="pre-requisites"></a>Wymagania wstępne

Musisz mieć program Visual Studio 2013, Visual Studio 2012 lub Visual Studio 2010 należy zainstalować w celu przeprowadzenia tego instruktażu.

Jeśli używasz programu Visual Studio 2010 należy zainstalować NuGet. Aby uzyskać więcej informacji, zobacz [Instalowanie systemu NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).  

## <a name="create-the-application"></a>Tworzenie aplikacji

-   Otwórz program Visual Studio
-   **Plik —&gt; New -&gt; projektu...**
-   Wybierz **Windows** w okienku po lewej stronie i **WPFApplication** w okienku po prawej stronie
-   Wprowadź **WPFwithEFSample** jako nazwę
-   Wybierz **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Instalowanie pakietu NuGet programu Entity Framework

-   W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **WinFormswithEFSample** projektu
-   Wybierz **Zarządzaj pakietami NuGet...**
-   W oknie dialogowym pakiety zarządzania NuGet wybierz **Online** kartę i wybierz polecenie **EntityFramework** pakietu
-   Kliknij przycisk **instalacji**  
    >[!NOTE]
    > Oprócz zestawu EntityFramework dodawany jest również odwołania do System.ComponentModel.DataAnnotations. Jeśli projekt zawiera odwołanie do System.Data.Entity, następnie zostanie on usunięty po zainstalowaniu pakietu platformy EntityFramework. Zestaw System.Data.Entity nie jest już używany w przypadku aplikacji platformy Entity Framework 6.

## <a name="define-a-model"></a>Zdefiniuj Model

W tym przewodniku, możesz wybrać do zaimplementowania model przy użyciu Code First i projektancie platformy EF. Wykonaj jedną z dwóch poniższych sekcjach.

### <a name="option-1-define-a-model-using-code-first"></a>Opcja 1: Definiowanie modelu za pomocą funkcji Code First

W tej sekcji przedstawiono sposób tworzenia modelu i jego skojarzonej bazy danych za pomocą funkcji Code First. Przejdź do następnej sekcji (**opcja 2: Definiowanie model przy użyciu pierwszej bazy danych)** Jeśli zostanie wykorzystany raczej Database First odwrócić inżynier modelu z bazy danych za pomocą projektanta EF

Korzystając z rozwiązania deweloperskiego Code First zwykle Rozpocznij od pisania klas .NET Framework, które definiują model koncepcyjny (domena).

-   Dodaj nową klasę do **WPFwithEFSample:**
    -   Kliknij prawym przyciskiem myszy nazwę projektu
    -   Wybierz **Dodaj**, następnie **nowy element**
    -   Wybierz **klasy** i wprowadź **produktu** nazwy klasy
-   Zastąp **produktu** klasy definicji z następującym kodem:

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

**Produktów** właściwość **kategorii** klasy i **kategorii** właściwość **produktu** klasy są właściwości nawigacji. Platformy Entity Framework właściwości nawigacji Podaj sposób przechodzenia relację między dwoma typami encji.

Oprócz definiowania jednostek, musisz zdefiniować klasę, która pochodzi od typu DbContext i udostępnia DbSet&lt;TEntity&gt; właściwości. DbSet&lt;TEntity&gt; właściwości umożliwiają kontekstu wiedzieć, jakie typy, które mają zostać uwzględnione w modelu.

Wystąpienie typu DbContext pochodzące zarządza obiektami jednostki w czasie wykonywania, zawierającą wypełnianie obiektów przy użyciu danych z bazy danych, zmień śledzenie i przechowywanie danych w bazie danych.

-   Dodaj nową **ProductContext** klasy do projektu przy użyciu następujących definicji:

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

### <a name="option-2-define-a-model-using-database-first"></a>Opcja 2: Definiowanie model przy użyciu pierwszej bazy danych

W tej sekcji pokazano, jak użyć pierwszej bazy danych do odtworzenia modelu z bazy danych za pomocą projektanta EF. Jeśli należy wykonać czynności opisane w poprzedniej sekcji (**opcja 1: Definiowanie modelu za pomocą funkcji Code First)**, Pomiń tę sekcję i przejdź bezpośrednio do **powolne ładowanie** sekcji.

#### <a name="create-an-existing-database"></a>Utwórz istniejącej bazy danych

Zazwyczaj przeznaczonych do istniejącej bazy danych, zostanie już utworzony, ale w tym przewodniku należy utworzyć bazy danych w celu uzyskania dostępu.

Serwer bazy danych, który został zainstalowany przy użyciu programu Visual Studio różni się zależnie od wersji programu Visual Studio zostały zainstalowane:

-   Jeśli używasz programu Visual Studio 2010 zostanie utworzona baza danych programu SQL Express.
-   Jeśli używasz programu Visual Studio 2012, a następnie zostanie utworzona [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) bazy danych.

Rozpocznijmy i wygenerować bazę danych.

-   **Widok —&gt; Eksploratora serwera**
-   Kliknij prawym przyciskiem myszy **połączeń danych -&gt; Dodaj połączenie...**
-   Jeśli nie jest połączona z bazą danych za pomocą Eksploratora serwera przed, musisz wybrać programu Microsoft SQL Server jako źródło danych

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   Łączenie się z LocalDB lub SQL Express, w zależności od tego, który z nich został zainstalowany, a następnie wprowadź **produktów** jako nazwa bazy danych

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   Wybierz **OK** i uzyskasz, jeśli chcesz utworzyć nową bazę danych, wybierz opcję **tak**

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   Nowe bazy danych będą teraz wyświetlane w Eksploratorze serwera, kliknij prawym przyciskiem myszy na nim i wybierz **nowe zapytanie**
-   Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonania**

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

#### <a name="reverse-engineer-model"></a>Model odtwarzania

Zamierzamy korzystania z programu Entity Framework Designer, który wchodzi w skład programu Visual Studio, aby utworzyć nasz model.

-   **Projekt —&gt; Dodaj nowy element...**
-   Wybierz **danych** menu po lewej stronie i następnie **ADO.NET Entity Data Model**
-   Wprowadź **ProductModel** jako nazwę i kliknij przycisk **OK**
-   Spowoduje to uruchomienie **Kreator modelu Entity Data Model**
-   Wybierz **Generuj z bazy danych** i kliknij przycisk **dalej**

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   Wybierz połączenie do bazy danych utworzonej w pierwszej sekcji, wprowadź **ProductContext** jako nazwa parametrów połączenia i kliknij przycisk **dalej**

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   Kliknij pole wyboru obok "Tabele", aby zaimportować wszystkie tabele i kliknij przycisk "Zakończ"

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

Po zakończeniu procesu odtwarzania nowy model jest dodawane do projektu i otworzona w celu wyświetlania w Projektancie Entity Framework. Dodano również pliku App.config do projektu przy użyciu szczegółów połączenia dla bazy danych.

#### <a name="additional-steps-in-visual-studio-2010"></a>Dodatkowe kroki w programie Visual Studio 2010

Jeśli pracujesz w programie Visual Studio 2010 należy zaktualizować projektancie platformy EF używać EF6 generowania kodu.

-   Kliknij prawym przyciskiem myszy na puste miejsce modelu w Projektancie platformy EF, a następnie wybierz pozycję **Dodaj element generowanie kodu...**
-   Wybierz **szablonów Online** z menu po lewej stronie i wyszukaj **DbContext**
-   Wybierz **EF Generator DbContext dla języka C w wersji 6.x\#,** wprowadź **ProductsModel** jako nazwę i kliknij przycisk Dodaj

#### <a name="updating-code-generation-for-data-binding"></a>Aktualizowanie generowania kodu dla powiązania danych

EF generuje kod z modelu przy użyciu szablonów T4. Szablonów dostarczanych z programem Visual Studio lub pobrać z galerii programu Visual Studio są przeznaczone do użytku ogólnego przeznaczenia. To oznacza, że jednostki generowane w te szablony mają prosty interfejs ICollection&lt;T&gt; właściwości. Jednak podczas ustalania danych powiązania, za pomocą WPF jest pożądane, aby użyć **observablecollection —** dla właściwości kolekcji, tak że WPF można śledzenie pracy do kolekcji. W tym celu firma Microsoft będzie modyfikować szablony do użycia z observablecollection —.

-   Otwórz **Eksploratora rozwiązań** i Znajdź **ProductModel.edmx** pliku
-   Znajdź **ProductModel.tt** pliku, który będzie można zagnieździć w pliku ProductModel.edmx

    ![WpfProductModelTemplate](~/ef6/media/wpfproductmodeltemplate.png)

-   Kliknij dwukrotnie plik ProductModel.tt, aby otworzyć go w edytorze programu Visual Studio
-   Znajdź i Zamień dwa wystąpienia "**ICollection**"z"**observablecollection —**". Są to znajduje się mniej więcej w wierszach 296 i 484.
-   Znajdź i Zamień pierwsze wystąpienie ciągu "**hashset —**"z"**observablecollection —**". To wystąpienie znajduje się mniej więcej w wierszu 50. **Nie** zastąpić drugie wystąpienie hashset — znaleziono w dalszej części kodu.
-   Znajdź i Zamień tylko wystąpienie "**System.Collections.Generic**"z"**System.Collections.ObjectModel**". To znajduje się mniej więcej w wierszu 424.
-   Zapisz plik ProductModel.tt. Powinno to spowodować kodu dla jednostek do ponownego wygenerowania. Jeśli kod nie jest generowany ponownie automatycznie, kliknij prawym przyciskiem myszy ProductModel.tt i wybierz polecenie "Uruchom narzędzie niestandardowe".

Jeśli teraz otworzysz plik Category.cs (która jest zagnieżdżony w ProductModel.tt), wówczas powinien zostać wyświetlony, że kolekcja produktów ma typ **observablecollection —&lt;produktu&gt;**.

Skompiluj projekt.

## <a name="lazy-loading"></a>Ładowanie z opóźnieniem

**Produktów** właściwość **kategorii** klasy i **kategorii** właściwość **produktu** klasy są właściwości nawigacji. Platformy Entity Framework właściwości nawigacji Podaj sposób przechodzenia relację między dwoma typami encji.

EF daje możliwość ładowanie powiązanych jednostek z bazy danych automatycznie po raz pierwszy możesz uzyskać dostęp do właściwości nawigacji. W przypadku tego typu (nazywane powolne ładowanie) podczas ładowania należy pamiętać, że po raz pierwszy możesz uzyskać dostęp do każdej właściwości nawigacji oddzielnego zapytania będą wykonywane względem bazy danych, jeśli zawartość nie jest już w kontekście.

Korzystając z typów jednostki POCO, EF realizuje powolne ładowanie przez tworzenie wystąpień typów pochodnych serwera proxy podczas wykonywania, a następnie zastępowanie właściwości wirtualnych w Twoich zajęciach, które można dodać punktów zaczepienia ładowania. Aby uzyskać powolne ładowanie powiązanych obiektów, należy zadeklarować nawigacji metody pobierające właściwości jako **publicznych** i **wirtualnego** (**Overridable** w języku Visual Basic), i możesz klasy nie może być **zapieczętowanego** (**NotOverridable** w języku Visual Basic). Przy użyciu bazy danych właściwości nawigacji pierwszy zostaną zastosowane automatycznie wirtualnego, aby umożliwić ładowanie z opóźnieniem. W sekcji Code First Wybraliśmy Tworzenie właściwości nawigacji wirtualnego z tego samego powodu

## <a name="bind-object-to-controls"></a>Wiązanie obiektu z kontrolkami

Dodawanie klas, które są zdefiniowane w modelu jako źródła danych dla tej aplikacji WPF.

-   Kliknij dwukrotnie **MainWindow.xaml** w Eksploratorze rozwiązań, aby otworzyć formularz główny
-   W menu głównym wybierz **projekt —&gt; Dodaj nowe źródło danych...**
    (w programie Visual Studio 2010, musisz wybrać **dane —&gt; Dodaj nowe źródło danych...** )
-   W polu Wybierz Typewindow źródła danych wybierz **obiektu** i kliknij przycisk **dalej**
-   Wybierz obiekty danych okna dialogowego rozwijania może **WPFwithEFSample** dwa razy i wybierz pozycję **kategorii**  
    *Nie ma potrzeby wybrać **produktu** źródła danych, ponieważ można je uzyskać do niego za pośrednictwem **produktu**przez właściwość **kategorii** źródła danych*  

    ![SelectDataObjects](~/ef6/media/selectdataobjects.png)

-   Kliknij przycisk **Zakończ.**
-   Okno źródeł danych zostanie otwarty obok okna MainWindow.xaml *okna źródła danych nie są wyświetlane, wybierz opcję **widok —&gt; inne Windows -&gt; źródeł danych***
-   Naciśnij ikonę pinezki, więc okna źródeł danych, automatycznie ukrywać. Może być konieczne przycisk Odśwież okno zostało już widoczne.

    ![Źródła danych](~/ef6/media/datasources.png)

-   Wybierz pozycję ** kategorii ** źródła danych i przeciągnij je na formularzu.

Następujące wystąpił podczas możemy przeciągnąć tego źródła:

-   **CategoryViewSource** zasobów i ** categoryDataGrid ** kontroli zostały dodane do XAML. Aby uzyskać więcej informacji na temat DataViewSources zobacz http://bea.stollnitz.com/blog/?p=387.
-   Właściwość DataContext nadrzędnego elementu siatki "{staticresource — **categoryViewSource** }".  **CategoryViewSource** zasobu służy jako źródło powiązania zewnętrzne\\nadrzędnego elementu siatki. Wewnętrzny elementów siatki, wartość elementu DataContext następnie jest Dziedzicz po elemencie nadrzędnym siatki (właściwości ItemsSource categoryDataGrid jest ustawiona na "{powiązania}"). 

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

## <a name="adding-a-details-grid"></a>Dodawanie szczegółów siatki

Skoro mamy już siatki, aby wyświetlić kategorie Przyjrzyjmy dodać siatkę szczegóły, aby wyświetlić skojarzone produkty.

-   Wybierz pozycję ** produktów ** właściwości w obszarze ** kategorii ** źródła danych i przeciągnij je na formularzu.
    -   **CategoryProductsViewSource** zasobów i **productDataGrid** siatki są dodawane do XAML
    -   Ścieżka powiązania dla tego zasobu jest równa produktów
    -   Framework powiązanie danych WPF zapewnia tylko produktów związanych z wybraną kategorię wyświetlane w **productDataGrid**
-   Z przybornika przeciągnij **przycisk** się do formularza. Ustaw **nazwa** właściwości **buttonSave** i **zawartości** właściwości **Zapisz**.

Formularz powinien wyglądać mniej więcej tak:

![Projektant](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Dodaj kod, który obsługuje interakcji z danych

Nadszedł czas, aby dodać niektóre procedury obsługi zdarzeń do głównego okna.

-   W oknie XAML kliknij  **&lt;okna** elementu, po wybraniu tej opcji okna głównego
-   W **właściwości** wybierz okno **zdarzenia** w prawym górnym rogu, następnie kliknij dwukrotnie przycisk po prawej stronie pola tekstowego **Loaded** etykiety

    ![MainWindowProperties](~/ef6/media/mainwindowproperties.png)

-   Również dodać **kliknij** zdarzenie **Zapisz** przycisku przez dwukrotne kliknięcie przycisku Zapisz w projektancie. 

Wybranie tej opcji powoduje możesz kodu formularza, firma Microsoft będzie teraz edytować kod, aby użyć ProductContext przeprowadzić dostępu do danych. Zaktualizuj kod dla okna głównego, jak pokazano poniżej.

Ten kod deklaruje wystąpienie długotrwałych **ProductContext**. **ProductContext** obiekt jest używany do wykonywania zapytań i zapisać dane w bazie danych. **Dispose**() na **ProductContext** wystąpienia jest następnie wywoływana z zastąpione **OnClosing** metody. Komentarze w kodzie zawierają szczegółowe informacje o dany kod realizuje.

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

-   Skompilować i uruchomić aplikację. Jeśli użyto Code First, a następnie zostanie wyświetlony **WPFwithEFSample.ProductContext** baza danych została utworzona dla Ciebie.
-   Wprowadź nazwę kategorii w górnym siatki produktu nazwy i w dół siatki *wypełniać kolumny Identyfikatora, ponieważ klucz podstawowy jest generowany przez bazę danych*

    ![Screen1](~/ef6/media/screen1.png)

-   Naciśnij klawisz **Zapisz** przycisk, aby zapisać dane w bazie danych

Po wywołaniu przez DbContext **SaveChanges**(), identyfikatory są wypełniane przy użyciu wartości z bazy danych, wygenerowane. Ponieważ firma Microsoft o nazwie **Odśwież**() po **SaveChanges**() **DataGrid** formanty są aktualizowane na nowe wartości.

![Screen2](~/ef6/media/screen2.png)
