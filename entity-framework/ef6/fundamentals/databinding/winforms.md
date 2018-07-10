---
title: Wiązanie danych z WinForms - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
caps.latest.revision: 3
ms.openlocfilehash: b17bc91fd7d665f6d75bf5f1e5798ddd16aa345d
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913438"
---
# <a name="databinding-with-winforms"></a>Wiązanie danych z WinForms
Ten przewodnik krok po kroku pokazano, jak powiązać POCO typy formantów w formularzach systemu Windows (WinForms) w postaci "elementy główne szczegóły". Aplikacja używa platformy Entity Framework do wypełniania obiekty z danymi z bazy danych, śledzenie zmian i utrwalanie danych w bazie danych.

Model definiuje dwa typy, które uczestniczą w relacji jeden do wielu: kategorii (jednostki\\głównego) i produktów (zależne\\szczegółów). Następnie narzędzia programu Visual Studio są używane do powiązania typów zdefiniowanych w modelu do kontrolki WinForms. Struktura powiązanie danych formularzy WinForm umożliwia nawigacji między powiązane obiekty: Wybieranie wierszy w widoku głównego powoduje, że widok szczegółów aktualizacji za pomocą odpowiednich danych podrzędnych.

Zrzuty ekranu i zamieszczone w tym przewodniku są pobierane z programu Visual Studio 2013, ale możesz ukończyć ten przewodnik przy użyciu programu Visual Studio 2012 lub Visual Studio 2010.

## <a name="pre-requisites"></a>Wymagania wstępne

Musisz mieć program Visual Studio 2013, Visual Studio 2012 lub Visual Studio 2010 należy zainstalować w celu przeprowadzenia tego instruktażu.

Jeśli używasz programu Visual Studio 2010 należy zainstalować NuGet. Aby uzyskać więcej informacji, zobacz [Instalowanie systemu NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="create-the-application"></a>Tworzenie aplikacji

-   Otwórz program Visual Studio
-   **Plik —&gt; New -&gt; projektu...**
-   Wybierz **Windows** w okienku po lewej stronie i **Windows FormsApplication** w okienku po prawej stronie
-   Wprowadź **WinFormswithEFSample** jako nazwę
-   Wybierz **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Instalowanie pakietu NuGet programu Entity Framework

-   W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **WinFormswithEFSample** projektu
-   Wybierz **Zarządzaj pakietami NuGet...**
-   W oknie dialogowym pakiety zarządzania NuGet wybierz **Online** kartę i wybierz polecenie **EntityFramework** pakietu
-   Kliknij przycisk **instalacji**  
    > [!NOTE]
    > Oprócz zestawu EntityFramework dodawany jest również odwołania do System.ComponentModel.DataAnnotations. Jeśli projekt zawiera odwołanie do System.Data.Entity, następnie zostanie on usunięty po zainstalowaniu pakietu platformy EntityFramework. Zestaw System.Data.Entity nie jest już używany w przypadku aplikacji platformy Entity Framework 6.

## <a name="implementing-ilistsource-for-collections"></a>Implementowanie IListSource dla kolekcji

Właściwości kolekcji musi implementować interfejsu IListSource umożliwiające powiązanie dwukierunkowe danych za pomocą sortowania, korzystając z Windows Forms. W tym celu użyjemy rozszerzenie observablecollection — Dodawanie funkcji IListSource.

-   Dodaj **ObservableListSource** klasy do projektu:
    -   Kliknij prawym przyciskiem myszy nazwę projektu
    -   Wybierz **Add -&gt; nowy element**
    -   Wybierz **klasy** i wprowadź **ObservableListSource** nazwy klasy
-   Zastąp kod wygenerowany przez domyślną, używając następującego kodu:

*Ta klasa umożliwia dwukierunkowe danych powiązania, a także sortowanie. Klasa pochodzi z observablecollection —&lt;T&gt; i dodaje Jawna implementacja interfejsu IListSource. Metoda GetList() IListSource zaimplementowano by zwrócić implementacje IBindingList, która pozostaje zsynchronizowany z observablecollection —. Implementacja IBindingList generowane przez ToBindingList obsługuje sortowanie. Metoda rozszerzenie ToBindingList jest zdefiniowany w zestawie platformy EntityFramework.*

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
                return _bindingList  (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a>Zdefiniuj Model

W tym przewodniku, możesz wybrać do zaimplementowania model przy użyciu Code First i projektancie platformy EF. Wykonaj jedną z dwóch poniższych sekcjach.

### <a name="option-1-define-a-model-using-code-first"></a>Opcja 1: Definiowanie modelu za pomocą funkcji Code First

W tej sekcji przedstawiono sposób tworzenia modelu i jego skojarzonej bazy danych za pomocą funkcji Code First. Przejdź do następnej sekcji (**opcja 2: Definiowanie model przy użyciu pierwszej bazy danych)** Jeśli zostanie wykorzystany raczej Database First odwrócić inżynier modelu z bazy danych za pomocą projektanta EF

Korzystając z rozwiązania deweloperskiego Code First zwykle Rozpocznij od pisania klas .NET Framework, które definiują model koncepcyjny (domena).

-   Dodaj nową **produktu** klasy do projektu
-   Zastąp kod wygenerowany przez domyślną, używając następującego kodu:

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

-   Dodaj **kategorii** klasy do projektu.
-   Zastąp kod wygenerowany przez domyślną, używając następującego kodu:

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

Oprócz Definiowanie jednostek, musisz zdefiniować klasę, która pochodzi od klasy **DbContext** i udostępnia **DbSet&lt;TEntity&gt;**  właściwości. **DbSet** właściwości umożliwiają kontekstu wiedzieć, jakie typy, które mają zostać uwzględnione w modelu. **DbContext** i **DbSet** typy są definiowane w zestawie platformy EntityFramework.

Wystąpienie typu DbContext pochodzące zarządza obiektami jednostki w czasie wykonywania, zawierającą wypełnianie obiektów przy użyciu danych z bazy danych, zmień śledzenie i przechowywanie danych w bazie danych.

-   Dodaj nową **ProductContext** klasy do projektu.
-   Zastąp kod wygenerowany przez domyślną, używając następującego kodu:

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

EF generuje kod z modelu przy użyciu szablonów T4. Szablonów dostarczanych z programem Visual Studio lub pobrać z galerii programu Visual Studio są przeznaczone do użytku ogólnego przeznaczenia. To oznacza, że jednostki generowane w te szablony mają prosty interfejs ICollection&lt;T&gt; właściwości. Jednak podczas wiązania z danymi jest wskazane właściwości kolekcji, które implementują IListSource. Dlatego utworzyliśmy klasy ObservableListSource powyżej i teraz zamierzamy zmodyfikować szablony Aby użyć tej klasy.

-   Otwórz **Eksploratora rozwiązań** i Znajdź **ProductModel.edmx** pliku
-   Znajdź **ProductModel.tt** pliku, który będzie można zagnieździć w pliku ProductModel.edmx

    ![ProductModelTemplate](~/ef6/media/productmodeltemplate.png)

-   Kliknij dwukrotnie plik ProductModel.tt, aby otworzyć go w edytorze programu Visual Studio
-   Znajdź i Zamień dwa wystąpienia "**ICollection**"z"**ObservableListSource**". Te znajdują się w przybliżeniu wiersze 296 i 484.
-   Znajdź i Zamień pierwsze wystąpienie ciągu "**hashset —**"z"**ObservableListSource**". To wystąpienie znajduje się w wierszu około 50. **Nie** zastąpić drugie wystąpienie hashset — znaleziono w dalszej części kodu.
-   Zapisz plik ProductModel.tt. Powinno to spowodować kodu dla jednostek do ponownego wygenerowania. Jeśli kod nie jest generowany ponownie automatycznie, kliknij prawym przyciskiem myszy ProductModel.tt i wybierz polecenie "Uruchom narzędzie niestandardowe".

Jeśli teraz otworzysz plik Category.cs (która jest zagnieżdżony w ProductModel.tt), wówczas powinien zostać wyświetlony, że kolekcja produktów ma typ **ObservableListSource&lt;produktu&gt;**.

Skompiluj projekt.

## <a name="lazy-loading"></a>Ładowanie z opóźnieniem

**Produktów** właściwość **kategorii** klasy i **kategorii** właściwość **produktu** klasy są właściwości nawigacji. Platformy Entity Framework właściwości nawigacji Podaj sposób przechodzenia relację między dwoma typami encji.

EF daje możliwość ładowanie powiązanych jednostek z bazy danych automatycznie po raz pierwszy możesz uzyskać dostęp do właściwości nawigacji. W przypadku tego typu (nazywane powolne ładowanie) podczas ładowania należy pamiętać, że po raz pierwszy możesz uzyskać dostęp do każdej właściwości nawigacji oddzielnego zapytania będą wykonywane względem bazy danych, jeśli zawartość nie jest już w kontekście.

Korzystając z typów jednostki POCO, EF realizuje powolne ładowanie przez tworzenie wystąpień typów pochodnych serwera proxy podczas wykonywania, a następnie zastępowanie właściwości wirtualnych w Twoich zajęciach, które można dodać punktów zaczepienia ładowania. Aby uzyskać powolne ładowanie powiązanych obiektów, należy zadeklarować nawigacji metody pobierające właściwości jako **publicznych** i **wirtualnego** (**Overridable** w języku Visual Basic), i możesz klasy nie może być **zapieczętowanego** (**NotOverridable** w języku Visual Basic). Przy użyciu bazy danych właściwości nawigacji pierwszy zostaną zastosowane automatycznie wirtualnego, aby umożliwić ładowanie z opóźnieniem. W sekcji Code First Wybraliśmy Tworzenie właściwości nawigacji wirtualnego z tego samego powodu

## <a name="bind-object-to-controls"></a>Wiązanie obiektu z kontrolkami

Dodawanie klas, które są zdefiniowane w modelu jako źródła danych dla tej aplikacji formularzy WinForms.

-   W menu głównym wybierz **projekt —&gt; Dodaj nowe źródło danych...**
    (w programie Visual Studio 2010, musisz wybrać **dane —&gt; Dodaj nowe źródło danych...** )
-   Wybierz typ źródła danych okna wybierz **obiektu** i kliknij przycisk **dalej**
-   Wybierz obiekty danych okna dialogowego rozwijania może **WinFormswithEFSample** dwa razy i wybierz pozycję **kategorii** nie ma potrzeby wybierz źródło danych produktu, ponieważ można je uzyskać do niego w produkcie Właściwość w źródle danych kategorii.

    ![DataSource](~/ef6/media/datasource.png)

-   Kliknij przycisk **Zakończ.** 
     *Okna źródła danych nie są wyświetlane, wybierz opcję *** Widok —&gt; inne Windows -&gt; źródeł danych**
-   Naciśnij ikonę pinezki, więc okna źródeł danych, automatycznie ukrywać. Może być konieczne przycisk Odśwież okno zostało już widoczne.

    ![DataSource2](~/ef6/media/datasource2.png)

-   W Eksploratorze rozwiązań kliknij dwukrotnie **Form1.cs** plik, aby otworzyć formularz główny w projektancie.
-   Wybierz **kategorii** źródła danych i przeciągnij je na formularzu. Domyślnie nowe DataGridView (**categoryDataGridView**) i formanty nawigacji na pasku narzędzi są dodawane do projektanta. Te kontrolki są powiązane z BindingSource (**categoryBindingSource**) i powiązanie Navigator (**categoryBindingNavigator**) składników, które są również tworzone.
-   Edytuj kolumny w **categoryDataGridView**. Chcemy ustawić **CategoryId** kolumny tylko do odczytu. Wartość **CategoryId** właściwości został wygenerowany przez bazę danych, po oszczędzamy danych.
    -   Kliknij prawym przyciskiem myszy formant DataGridView i wybierz pozycję Edytuj kolumny...
    -   Wybierz kolumnę CategoryId i wartość True tylko do odczytu
    -   Naciśnij przycisk OK.
-   Wybierz produkty z kategorii źródła danych i przeciągnij go na formularzu. ProductDataGridView i productBindingSource są dodawane do formularza.
-   Edytuj kolumny na productDataGridView. Chcemy ukryć kolumny CategoryId i kategorii i ustawić ProductId tylko do odczytu. Wartość właściwości ProductId jest generowany przez bazę danych, po oszczędzamy danych.
    -   Kliknij prawym przyciskiem myszy formant DataGridView i wybierz **Edytowanie kolumn...** .
    -   Wybierz **ProductId** kolumny oraz zestawu **tylko do odczytu** do **True**.
    -   Wybierz **CategoryId** kolumny i naciśnij klawisz **Usuń** przycisku. Wykonaj te czynności z **kategorii** kolumny.
    -   Naciśnij klawisz **OK**.

    Do tej pory firma Microsoft skojarzony naszych formantów DataGridView BindingSource składniki w projektancie. W następnej sekcji dodamy kod kodu można ustawić categoryBindingSource.DataSource do kolekcji jednostek, które obecnie są śledzone przez DbContext. Gdy przeciągnąć i porzucony produktów z kategorii, WinForms trwało Dbamy konfigurowania właściwości productsBindingSource.DataSource categoryBindingSource i productsBindingSource.DataMember właściwości produktów. Ze względu na to powiązanie tylko produkty należące do aktualnie wybranej kategorii będą wyświetlane w productDataGridView.
-   Włącz **Zapisz** przycisk na pasku nawigacji, klikając prawym przyciskiem myszy i wybierając **włączone**.

    ![Formularz Form1 projektanta](~/ef6/media/form1-designer.png)

-   Dodaj program obsługi zdarzeń zapisywania przycisku przez dwukrotne kliknięcie przycisku. Spowoduje to dodanie obsługi zdarzeń i wyświetlić kod związany z formularza. Kod **categoryBindingNavigatorSaveItem\_kliknij** programu obsługi zdarzeń zostanie dodana w następnej sekcji.

## <a name="add-the-code-that-handles-data-interaction"></a>Dodaj kod, który obsługuje interakcji z danych

Teraz dodamy kod, aby użyć ProductContext przeprowadzić dostępu do danych. Aktualizowanie kodu dla okna głównego formularza, jak pokazano poniżej.

Ten kod deklaruje długotrwałych wystąpienie ProductContext. Obiekt ProductContext jest używany do wykonywania zapytań i zapisać dane w bazie danych. Metoda Dispose() w wystąpieniu ProductContext jest następnie wywoływana z przeciążonej OnClosing. Komentarze w kodzie zawierają szczegółowe informacje o dany kod realizuje.

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

-   Kompiluj i uruchom aplikację i przetestowania funkcji.

    ![Form1BeforeSave](~/ef6/media/form1beforesave.png)

-   Po zapisaniu wygenerowane klucze są wyświetlane na ekranie.

    ![Form1AfterSave](~/ef6/media/form1aftersave.png)

-   Jeśli została użyta Code First, to będzie również informacja, że **WinFormswithEFSample.ProductContext** baza danych została utworzona dla Ciebie.

    ![ServerObjExplorer](~/ef6/media/serverobjexplorer.png)
