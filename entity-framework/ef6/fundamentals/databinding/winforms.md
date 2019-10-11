---
title: Wiązanie danych z WinForms-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 4b3eee20ff238864b94ef4edfb97c1bae0713300
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181786"
---
# <a name="databinding-with-winforms"></a>Wiązanie danych z WinForms
W tym przewodniku krok po kroku pokazano, jak powiązać typy POCO z kontrolkami formularzy okien (WinForms) w formularzu "wzorzec-szczegóły". Aplikacja używa Entity Framework do wypełniania obiektów danymi z bazy danych, śledzenia zmian i utrwalania danych w bazie danych.

Model definiuje dwa typy, które uczestniczą w relacji jeden do wielu: Kategoria (główna @ no__t-0master) i produkt (zależne: @ no__t-1detail). Następnie narzędzia programu Visual Studio są używane do powiązania typów zdefiniowanych w modelu z kontrolkami WinForms. Środowisko WinForms — powiązania danych umożliwia nawigowanie między obiektami pokrewnymi: wybranie wierszy w widoku wzorca powoduje aktualizację widoku szczegółów z odpowiednimi danymi podrzędnymi.

Zrzuty ekranu i listy kodu w tym instruktażu są pobierane z Visual Studio 2013 ale można wykonać ten Instruktaż w programie Visual Studio 2012 lub Visual Studio 2010.

## <a name="pre-requisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik, musisz mieć zainstalowany Visual Studio 2013, Visual Studio 2012 lub Visual Studio 2010.

W przypadku korzystania z programu Visual Studio 2010 należy również zainstalować pakiet NuGet. Aby uzyskać więcej informacji, zobacz [Instalowanie programu NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="create-the-application"></a>Tworzenie aplikacji

-   Otwórz program Visual Studio
-   **Plik-&gt; nowy-&gt; projektu...**
-   Wybierz pozycję **Windows** w lewym okienku i **FormsApplication Windows** w okienku po prawej stronie
-   Wprowadź **WinFormswithEFSample** jako nazwę
-   Wybierz **przycisk OK**

## <a name="install-the-entity-framework-nuget-package"></a>Zainstaluj pakiet NuGet Entity Framework

-   W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **WinFormswithEFSample**
-   Wybierz pozycję **Zarządzaj pakietami NuGet...**
-   W oknie dialogowym Zarządzanie pakietami NuGet wybierz kartę **online** i wybierz pakiet **EntityFramework**
-   Kliknij przycisk **Instaluj**  
    > [!NOTE]
    > Oprócz zestawu EntityFramework dodawane jest również odwołanie do elementu System. ComponentModel. DataAnnotations. Jeśli projekt zawiera odwołanie do System. Data. Entity, zostanie usunięte po zainstalowaniu pakietu EntityFramework. Zestaw system. Data. Entity nie jest już używany w przypadku aplikacji Entity Framework 6.

## <a name="implementing-ilistsource-for-collections"></a>Implementowanie elementu IListSource dla kolekcji

Właściwości kolekcji muszą implementować interfejs IListSource, aby umożliwić dwukierunkowe wiązanie danych z sortowaniem podczas korzystania z Windows Forms. Aby to zrobić, możemy zwiększyć ObservableCollection, aby dodać funkcje IListSource.

-   Dodaj klasę **ObservableListSource** do projektu:
    -   Kliknij prawym przyciskiem myszy nazwę projektu
    -   Wybierz pozycję **Dodaj-&gt; nowy element**
    -   Wybierz **klasę** i wprowadź **ObservableListSource** dla nazwy klasy
-   Zastąp kod wygenerowany domyślnie następującym kodem:

@no__t — Klasa 0This umożliwia tworzenie dwukierunkowych powiązań danych oraz sortowanie. Klasa pochodzi z ObservableCollection @ no__t-0T @ no__t-1 i dodaje jawną implementację IListSource. Metoda GetList () IListSource jest zaimplementowana w celu zwrócenia implementacji IBindingList, która pozostaje w synchronizacji z ObservableCollection. Implementacja IBindingList wygenerowana przez ToBindingList obsługuje sortowanie. Metoda rozszerzenia ToBindingList jest zdefiniowana w zestawie EntityFramework. *

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a>Zdefiniuj model

W tym instruktażu możesz wybrać wdrożenie modelu przy użyciu Code First lub programu Dr Designer. Wykonaj jedną z dwóch poniższych sekcji.

### <a name="option-1-define-a-model-using-code-first"></a>Option 1: Zdefiniuj model przy użyciu Code First

W tej sekcji pokazano, jak utworzyć model i skojarzoną z nim bazę danych przy użyciu Code First. Przejdź do następnej sekcji (**Option 2: Zdefiniuj model przy użyciu Database First)**  Jeśli wolisz używać Database First do odtwarzania modelu z bazy danych przy użyciu narzędzia Dr Designer

W przypadku korzystania z Code First projektowania zwykle zaczynasz od pisania klas .NET Framework, które definiują model koncepcyjny (domeny).

-   Dodaj nową klasę **produktu** do projektu
-   Zastąp kod wygenerowany domyślnie następującym kodem:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   Dodaj klasę **kategorii** do projektu.
-   Zastąp kod wygenerowany domyślnie następującym kodem:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

Oprócz definiowania jednostek należy zdefiniować klasę, która dziedziczy z **DbContext** i uwidacznia **nieogólnymi @ no__t-2TEntity @ no__t-3** właściwości. Właściwości **nieogólnymi** umożliwiają kontekstowi znać typy, które mają zostać uwzględnione w modelu. Typy **DbContext** i **nieogólnymi** są zdefiniowane w zestawie EntityFramework.

Wystąpienie typu pochodnego DbContext zarządza obiektami obiektów w czasie wykonywania, co obejmuje wypełnianie obiektów danymi z bazy danych, śledzenie zmian i utrwalanie danych w bazie danych.

-   Dodaj nową klasę **ProductContext** do projektu.
-   Zastąp kod wygenerowany domyślnie następującym kodem:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

Kompiluj projekt.

### <a name="option-2-define-a-model-using-database-first"></a>Opcja 2: Zdefiniuj model przy użyciu Database First

W tej sekcji pokazano, jak używać programu Database First, aby odtworzyć model z bazy danych przy użyciu narzędzia Dr Designer. Jeśli poprzednia sekcja została ukończona (**Option 1: Zdefiniuj model przy użyciu Code First)** , a następnie Pomiń tę sekcję i przejdź bezpośrednio do sekcji **ładowania z opóźnieniem** .

#### <a name="create-an-existing-database"></a>Tworzenie istniejącej bazy danych

Zwykle w przypadku określania docelowej istniejącej bazy danych zostanie ona już utworzona, ale w tym instruktażu musimy utworzyć bazę danych do uzyskania dostępu.

Serwer bazy danych zainstalowany przy użyciu programu Visual Studio różni się w zależności od zainstalowanej wersji programu Visual Studio:

-   Jeśli używasz programu Visual Studio 2010, tworzysz bazę danych SQL Express.
-   Jeśli używasz programu Visual Studio 2012, zostanie utworzona baza danych [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .

Przyjrzyjmy się i wygenerujemy bazę danych.

-   **Widok-&gt; Eksplorator serwera**
-   Kliknij prawym przyciskiem myszy pozycję **połączenia danych-&gt; Dodaj połączenie...**
-   Jeśli nie masz połączenia z bazą danych Eksplorator serwera przed wybraniem Microsoft SQL Server jako źródła danych

    ![Zmień źródło danych](~/ef6/media/changedatasource.png)

-   Połącz się z usługą LocalDB lub SQL Express, w zależności od tego, który został zainstalowany, i wprowadź **produkty** jako nazwę bazy danych

    ![Dodaj LocalDB połączenia](~/ef6/media/addconnectionlocaldb.png)

    ![Dodaj połączenie ekspresowe](~/ef6/media/addconnectionexpress.png)

-   Wybierz pozycję **OK** . pojawi się monit z prośbą o utworzenie nowej bazy danych, a następnie wybierz pozycję **tak** .

    ![Utwórz bazę danych](~/ef6/media/createdatabase.png)

-   Nowa baza danych będzie teraz wyświetlana w Eksplorator serwera, kliknij ją prawym przyciskiem myszy, a następnie wybierz pozycję **nowe zapytanie** .
-   Skopiuj następujące SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Execute (wykonaj** ).

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

#### <a name="reverse-engineer-model"></a>Odtwórz Model

Będziemy używać Entity Framework Designer, które są dołączone jako część programu Visual Studio, aby utworzyć nasz model.

-   **Projekt-&gt; Dodaj nowy element...**
-   Wybierz pozycję **dane** z menu po lewej stronie, a następnie **ADO.NET Entity Data Model**
-   Wprowadź **ProductModel** jako nazwę, a następnie kliknij przycisk **OK** .
-   Spowoduje to uruchomienie **kreatora Entity Data Model**
-   Wybierz pozycję **Generuj z bazy danych** i kliknij przycisk **dalej** .

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   Wybierz połączenie z bazą danych utworzoną w pierwszej sekcji, wprowadź **ProductContext** jako nazwę parametrów połączenia i kliknij przycisk **dalej** .

    ![Wybierz połączenie](~/ef6/media/chooseyourconnection.png)

-   Kliknij pole wyboru obok pozycji "tabele", aby zaimportować wszystkie tabele, a następnie kliknij przycisk Zakończ.

    ![Wybieranie obiektów](~/ef6/media/chooseyourobjects.png)

Po zakończeniu procesu odtwarzania nowy model zostanie dodany do projektu i otwarty do wyświetlania w Entity Framework Designer. Plik App. config został również dodany do projektu i zawiera szczegółowe informacje o połączeniu dla bazy danych.

#### <a name="additional-steps-in-visual-studio-2010"></a>Dodatkowe kroki w programie Visual Studio 2010

Jeśli pracujesz w programie Visual Studio 2010, musisz zaktualizować projektanta EF, aby korzystał z generowania kodu EF6.

-   Kliknij prawym przyciskiem myszy pusty punkt w modelu w programie Dr Designer i wybierz polecenie **Dodaj element generowania kodu...**
-   Wybierz pozycję **Szablony online** z menu po lewej stronie i Wyszukaj w usłudze **DbContext**
-   Wybierz pozycję **Dr 6. x DbContext generator dla języka C @ no__t-1,** wprowadź **ProductsModel** jako nazwę i kliknij przycisk Dodaj.

#### <a name="updating-code-generation-for-data-binding"></a>Aktualizowanie generowania kodu dla powiązania danych

EF generuje kod z modelu przy użyciu szablonów T4. Szablony dostarczane z programem Visual Studio lub pobrane z galerii programu Visual Studio są przeznaczone do ogólnego użycia. Oznacza to, że jednostki wygenerowane na podstawie tych szablonów mają proste właściwości ICollection @ no__t-0T @ no__t-1. Jednak podczas tworzenia powiązania danych wskazane jest posiadanie właściwości kolekcji, które implementują IListSource. Dlatego utworzyliśmy ObservableListSource klasy powyżej i teraz zmodyfikujemy szablony, aby użyć tej klasy.

-   Otwórz **Eksplorator rozwiązań** i Znajdź plik **ProductModel. edmx**
-   Znajdź plik **ProductModel.tt** , który zostanie zagnieżdżony w pliku ProductModel. edmx

    ![Szablon modelu produktu](~/ef6/media/productmodeltemplate.png)

-   Kliknij dwukrotnie plik ProductModel.tt, aby otworzyć go w edytorze programu Visual Studio.
-   Znajdź i Zamień dwa wystąpienia elementu "**ICollection**" na "**ObservableListSource**". Znajdują się one w około wierszach 296 i 484.
-   Znajdź i Zamień pierwsze wystąpienie elementu "**HashSet —** " na "**ObservableListSource**". To wystąpienie znajduje się w około wiersz 50. **Nie** zamieniaj drugiego wystąpienia HashSet — znalezionego w dalszej części kodu.
-   Zapisz plik ProductModel.tt. Powinno to spowodować, że kod dla jednostek zostanie ponownie wygenerowany. Jeśli kod nie zostanie wygenerowany automatycznie, kliknij prawym przyciskiem myszy pozycję ProductModel.tt i wybierz polecenie "Uruchom narzędzie niestandardowe".

Jeśli teraz otworzysz plik Category.cs (który jest zagnieżdżony w obszarze ProductModel.tt), powinna zostać wyświetlona, że kolekcja Products ma typ **ObservableListSource @ no__t-1Product @ no__t-2**.

Kompiluj projekt.

## <a name="lazy-loading"></a>Ładowanie z opóźnieniem

Właściwości **Products** klasy **Category** i **Category** klasy **Product** są właściwościami nawigacji. W Entity Framework właściwości nawigacji umożliwiają nawigowanie po relacjach między dwoma typami jednostek.

EF oferuje opcję ładowania powiązanych jednostek z bazy danych automatycznie przy pierwszym dostępie do właściwości nawigacji. W przypadku tego typu ładowania (nazywanego ładowaniem opóźnionym) należy pamiętać, że podczas pierwszego uzyskiwania dostępu do każdej właściwości nawigacji oddzielne zapytanie zostanie wykonane względem bazy danych, jeśli zawartość nie jest jeszcze w kontekście.

W przypadku korzystania z typów jednostek POCO EF osiąga opóźnione ładowanie przez utworzenie wystąpień pochodnych typów proxy podczas wykonywania, a następnie Zastępowanie właściwości wirtualnych w klasach, aby dodać punkt zaczepienia ładowania. Aby uzyskać opóźnione ładowanie pokrewnych obiektów, należy zadeklarować metody do pobierania właściwości nawigacji jako **publiczne** i **wirtualne** (Zastąp w Visual Basic), a Klasa nie może być **zapieczętowana** **(** **NotOverridable** w Visual Basic). Przy użyciu Database First właściwości nawigacji są automatycznie wprowadzane do wirtualnego, aby umożliwić ładowanie z opóźnieniem. W sekcji Code First wybrano, aby właściwości nawigacji były wirtualne z tego samego powodu

## <a name="bind-object-to-controls"></a>Powiąż obiekt z kontrolkami

Dodaj klasy, które są zdefiniowane w modelu jako źródła danych dla tej aplikacji WinForms.

-   Z menu głównego wybierz pozycję **projekt-&gt; Dodaj nowe źródło danych...**
    (w programie Visual Studio 2010 musisz wybrać pozycję **dane-&gt; Dodaj nowe źródło danych...** )
-   W oknie Wybierz typ źródła danych wybierz pozycję **obiekt** i kliknij przycisk **dalej** .
-   W oknie dialogowym Wybieranie obiektów danych unfold **WinFormswithEFSample** dwa razy i wybierz **kategorię** nie ma potrzeby wybierania źródła danych produktu, ponieważ zostanie on przechodzący przez właściwość produktu w źródle danych kategorii.

    ![Źródło danych](~/ef6/media/datasource.png)

-   Kliknij przycisk **Zakończ.**
    Jeśli okno źródła danych nie jest wyświetlane, wybierz pozycję **wyświetl &gt; inne źródła danych Windows-&gt;**
-   Naciśnij ikonę pinezki, aby okno źródła danych nie było ukrywane. Może być konieczne kliknięcie przycisku Odśwież, jeśli okno było już widoczne.

    ![Źródło danych 2](~/ef6/media/datasource2.png)

-   W Eksplorator rozwiązań kliknij dwukrotnie plik **Form1.cs** , aby otworzyć formularz główny w projektancie.
-   Wybierz **kategorię** źródło danych i przeciągnij ją na formularz. Domyślnie do projektanta dodawane są nowe kontrolki paska narzędzi DataGridView (**categoryDataGridView**) i nawigacyjnego. Te kontrolki są powiązane z utworzonymi również składnikami BindingSource (**categoryBindingSource**) i nawigatora powiązań (**categoryBindingNavigator**).
-   Edytuj kolumny w **categoryDataGridView**. Chcemy ustawić kolumnę **IDkategorii** na tylko do odczytu. Wartość właściwości **IDkategorii** jest generowana przez bazę danych po zapisaniu danych.
    -   Kliknij prawym przyciskiem myszy formant DataGridView i wybierz polecenie Edytuj kolumny...
    -   Wybierz kolumnę IDKategorii i ustaw wartość tylko do true
    -   Naciśnij przycisk OK
-   Wybierz pozycję produkty z kategorii źródło danych i przeciągnij ją w formularzu. ProductDataGridView i productBindingSource są dodawane do formularza.
-   Edytuj kolumny w productDataGridView. Chcemy ukryć IDKategorii i kolumny kategorii i ustawić ProductId jako tylko do odczytu. Wartość właściwości ProductId jest generowana przez bazę danych po zapisaniu danych.
    -   Kliknij prawym przyciskiem myszy formant DataGridView i wybierz polecenie **Edytuj kolumny...** .
    -   Wybierz kolumnę **ProductID** i dla opcji **ReadOnly** Ustaw **wartość true**.
    -   Wybierz kolumnę **IDkategorii** i naciśnij przycisk **Usuń** . Wykonaj te same czynności z kolumną **Category** .
    -   Naciśnij klawisz **OK**.

    Do tej pory skojarzenie formantów DataGridView ze składnikami BindingSource w projektancie. W następnej sekcji dodamy kod do kodu w celu ustawienia categoryBindingSource. DataSource do kolekcji jednostek, które są aktualnie śledzone przez DbContext. Podczas przeciągania i upuszczania produktów z poziomu kategorii WinForms brały pod uwagę skonfigurowanie właściwości productsBindingSource. DataSource na wartość categoryBindingSource i productsBindingSource. DataMember dla produktów. Ze względu na to powiązanie tylko produkty należące do aktualnie wybranej kategorii będą wyświetlane w productDataGridView.
-   Włącz przycisk **Zapisz** na pasku narzędzi nawigacji, klikając prawym przyciskiem myszy i wybierając pozycję **włączone**.

    ![Projektant formularza 1](~/ef6/media/form1-designer.png)

-   Dodaj program obsługi zdarzeń dla przycisku Zapisz przez dwukrotne kliknięcie przycisku. Spowoduje to dodanie obsługi zdarzeń i przełączenie do kodu powiązanego z formularzem. Kod dla programu obsługi zdarzeń **categoryBindingNavigatorSaveItem @ no__t-1CLICK** zostanie dodany w następnej sekcji.

## <a name="add-the-code-that-handles-data-interaction"></a>Dodawanie kodu, który obsługuje interakcję z danymi

Teraz dodamy kod do korzystania z ProductContext w celu uzyskania dostępu do danych. Zaktualizuj kod dla głównego okna formularza, jak pokazano poniżej.

Kod deklaruje długotrwałe wystąpienie ProductContext. Obiekt ProductContext jest używany do wykonywania zapytań i zapisywania danych w bazie danych. Metoda Dispose () w wystąpieniu ProductContext jest następnie wywoływana z przesłoniętej metody onzamykającej. Komentarze do kodu zawierają szczegółowe informacje o tym, co robi kod.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a>Testowanie aplikacji Windows Forms

-   Skompiluj i uruchom aplikację, a następnie możesz przetestować jej działanie.

    ![Formularz 1 przed zapisaniem](~/ef6/media/form1beforesave.png)

-   Po zapisaniu kluczy generowanych przez magazyn są wyświetlane na ekranie.

    ![Formularz 1 po zapisaniu](~/ef6/media/form1aftersave.png)

-   Jeśli używasz Code First, zobaczysz również, że została utworzona baza danych **WinFormswithEFSample. ProductContext** .

    ![Eksplorator obiektów serwera](~/ef6/media/serverobjexplorer.png)
