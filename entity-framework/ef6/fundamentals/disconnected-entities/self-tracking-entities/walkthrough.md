---
title: Wskazówki dotyczące samoobsługowego śledzenia jednostek — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 9bd644461f50a7eff1006cb8866ca9a3b08b6b8d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181715"
---
# <a name="self-tracking-entities-walkthrough"></a>Wskazówki dotyczące samośledzenia jednostek
> [!IMPORTANT]
> Nie zalecamy już korzystania z szablonu samośledzenia jednostek. Będzie ona nadal dostępna tylko do obsługi istniejących aplikacji. Jeśli aplikacja wymaga pracy z odłączonymi wykresami jednostek, należy wziąć pod uwagę inne alternatywy, takie jak [śledzone jednostki](https://trackableentities.github.io/), które są technologią podobną do samodzielnego śledzenia jednostek, które są opracowywane przez społeczność lub piszą kod niestandardowy przy użyciu interfejsów API śledzenia zmian niskiego poziomu.

W tym instruktażu przedstawiono scenariusz, w którym usługa Windows Communication Foundation (WCF) uwidacznia operację zwracającą wykres jednostki. Następnie aplikacja kliencka operuje na tym grafie i przesyła modyfikacje do operacji usługi, która weryfikuje i zapisuje aktualizacje bazy danych przy użyciu Entity Framework.

Przed ukończeniem tego instruktażu upewnij się, że odczytywano stronę [jednostki samośledzenia](index.md) .

W tym instruktażu są wykonywane następujące akcje:

-   Tworzy bazę danych do uzyskania dostępu.
-   Tworzy bibliotekę klas, która zawiera model.
-   Zamienia na szablon generatora jednostek samośledzenia.
-   Przenosi klasy jednostek do oddzielnego projektu.
-   Tworzy usługę WCF, która uwidacznia operacje do wykonywania zapytań i zapisywania jednostek.
-   Tworzy aplikacje klienckie (konsole i WPF) korzystające z usługi.

Będziemy używać Database First w tym instruktażu, ale te same techniki mają jednakową zastosowanie do Model First.

## <a name="pre-requisites"></a>Wymagania wstępne

Aby ukończyć ten Instruktaż, potrzebna jest Najnowsza wersja programu Visual Studio.

## <a name="create-a-database"></a>Tworzenie bazy danych

Serwer bazy danych zainstalowany przy użyciu programu Visual Studio różni się w zależności od zainstalowanej wersji programu Visual Studio:

-   Jeśli używasz programu Visual Studio 2012, zostanie utworzona baza danych LocalDB.
-   Jeśli używasz programu Visual Studio 2010, tworzysz bazę danych SQL Express.

Przyjrzyjmy się i wygenerujemy bazę danych.

-   Otwórz program Visual Studio
-   **Widok-&gt; Eksplorator serwera**
-   Kliknij prawym przyciskiem myszy pozycję **połączenia danych —&gt; Dodaj połączenie...**
-   Jeśli nie masz połączenia z bazą danych Eksplorator serwera przed wybraniem **Microsoft SQL Server** jako źródła danych
-   Nawiąż połączenie z usługą LocalDB lub SQL Express, w zależności od tego, który z nich jest zainstalowany
-   Wprowadź **STESample** jako nazwę bazy danych
-   Wybierz pozycję **OK** . pojawi się monit z prośbą o utworzenie nowej bazy danych, a następnie wybierz pozycję **tak** .
-   Nowa baza danych zostanie wyświetlona w Eksplorator serwera
-   Jeśli używasz programu Visual Studio 2012
    -   Kliknij prawym przyciskiem myszy bazę danych w Eksplorator serwera a następnie wybierz pozycję **nowe zapytanie** .
    -   Skopiuj następujące SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Execute (wykonaj** ).
-   Jeśli używasz programu Visual Studio 2010
    -   Wybierz pozycję **dane —&gt; edytor języka Transact SQL —&gt; nowe połączenie zapytania...**
    -   Wprowadź **.\\SQLExpress** jako nazwę serwera, a następnie kliknij przycisk **OK** .
    -   Wybierz bazę danych **STESample** z listy rozwijanej w górnej części edytora zapytań.
    -   Skopiuj poniższy kod SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Wykonaj SQL**

``` SQL
    CREATE TABLE [dbo].[Blogs] (
        [BlogId] INT IDENTITY (1, 1) NOT NULL,
        [Name] NVARCHAR (200) NULL,
        [Url]  NVARCHAR (200) NULL,
        CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
    );

    CREATE TABLE [dbo].[Posts] (
        [PostId] INT IDENTITY (1, 1) NOT NULL,
        [Title] NVARCHAR (200) NULL,
        [Content] NTEXT NULL,
        [BlogId] INT NOT NULL,
        CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
        CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
    );

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a>Tworzenie modelu

Najpierw potrzebny jest projekt, w którym należy umieścić model.

-   **Plik —&gt; nowy&gt; projekt...**
-   Wybierz pozycję **Visual C\#** z okienka po lewej stronie, a następnie **bibliotekę klas**
-   Wprowadź **STESample** jako nazwę, a następnie kliknij przycisk **OK** .

Teraz utworzysz prosty model w programie Dr Designer, aby uzyskać dostęp do bazy danych:

-   **Projekt —&gt; Dodaj nowy element...**
-   Wybierz pozycję **dane** w okienku po lewej stronie, a następnie **ADO.NET Entity Data Model**
-   Wprowadź **BloggingModel** jako nazwę, a następnie kliknij przycisk **OK** .
-   Wybierz pozycję **Generuj z bazy danych** i kliknij przycisk **dalej** .
-   Wprowadź informacje o połączeniu dla bazy danych utworzonej w poprzedniej sekcji
-   Wprowadź **BloggingContext** jako nazwę parametrów połączenia i kliknij przycisk **dalej** .
-   Zaznacz pole wyboru obok pozycji **tabele** i kliknij przycisk **Zakończ** .

## <a name="swap-to-ste-code-generation"></a>Zamień na generowanie kodu wklej

Teraz musimy wyłączyć domyślne generowanie kodu i zamianę na jednostki samośledzenia.

### <a name="if-you-are-using-visual-studio-2012"></a>Jeśli używasz programu Visual Studio 2012

-   Rozwiń węzeł **BloggingModel. edmx** w **Eksplorator rozwiązań** i Usuń **BloggingModel.tt** i **BloggingModel.Context.tt** ,
    *spowoduje to wyłączenie domyślnego generowania kodu* .
-   Kliknij prawym przyciskiem myszy pusty obszar na powierzchni projektanta EF i wybierz polecenie **Dodaj element generowania kodu...**
-   Wybierz pozycję **online** w okienku po lewej stronie i Wyszukaj **Generator wklej**
-   Wybierz **Generator wklej dla szablonu C\#** , wprowadź **STETemplate** jako nazwę, a następnie kliknij przycisk **Dodaj** .
-   Pliki **STETemplate.tt** i **STETemplate.Context.tt** są dodawane zagnieżdżone w pliku BloggingModel. edmx

### <a name="if-you-are-using-visual-studio-2010"></a>Jeśli używasz programu Visual Studio 2010

-   Kliknij prawym przyciskiem myszy pusty obszar na powierzchni projektanta EF i wybierz polecenie **Dodaj element generowania kodu...**
-   Zaznacz **kod** w lewym okienku, a następnie **ADO.NET samośledzenia jednostek**
-   Wprowadź **STETemplate** jako nazwę, a następnie kliknij przycisk **Dodaj** .
-   Pliki **STETemplate.tt** i **STETemplate.Context.tt** są dodawane bezpośrednio do projektu

## <a name="move-entity-types-into-separate-project"></a>Przenoszenie typów jednostek do oddzielnego projektu

Aby korzystać z jednostek samośledzenia, nasza aplikacja kliencka musi mieć dostęp do klas jednostek generowanych przez nasz model. Ponieważ nie chcemy ujawniać całego modelu aplikacji klienckiej, zamierzamy przenieść klasy jednostek do osobnego projektu.

Pierwszym krokiem jest zatrzymanie generowania klas jednostki w istniejącym projekcie:

-   Kliknij prawym przyciskiem myszy pozycję **STETemplate.tt** w **Eksplorator rozwiązań** i wybierz pozycję **Właściwości**
-   W oknie **Właściwości** Wyczyść **TextTemplatingFileGenerator** z właściwości **CustomTool**
-   Rozwiń **STETemplate.tt** w **Eksplorator rozwiązań** i Usuń wszystkie pliki zagnieżdżone w tym obszarze

Następnie będziemy dodawać nowy projekt i generować klasy jednostek

-   **&gt; dodawania&gt; projektu...**
-   Wybierz pozycję **Visual C\#** z okienka po lewej stronie, a następnie **bibliotekę klas**
-   Wprowadź **STESample. Entities** jako nazwę, a następnie kliknij przycisk **OK** .
-   **Projekt —&gt; dodać istniejącego elementu...**
-   Przejdź do folderu projektu **STESample**
-   Wybierz, aby wyświetlić **wszystkie pliki (\*.\*)**
-   Wybierz plik **STETemplate.tt**
-   Kliknij strzałkę listy rozwijanej obok przycisku **Dodaj** i wybierz pozycję **Dodaj jako łącze**

    ![Dodaj połączony szablon](~/ef6/media/addlinkedtemplate.png)

Chcemy również upewnić się, że klasy jednostek są generowane w tej samej przestrzeni nazw co kontekst. Pozwala to zmniejszyć liczbę instrukcji using, które należy dodać w całej aplikacji.

-   Kliknij prawym przyciskiem myszy **STETemplate.tt** połączone w **Eksplorator rozwiązań** i wybierz polecenie **Właściwości**
-   W oknie **Właściwości** Ustaw **przestrzeń nazw niestandardowego narzędzia** na **STESample**

Kod wygenerowany przez szablon wklej musi być odwołaniem do elementu **System. Runtime. Serialization** , aby można było go skompilować. Ta biblioteka jest wymagana dla atrybutów **DataContract** i **DataMember** programu WCF, które są używane dla serializowanych typów jednostek.

-   Kliknij prawym przyciskiem myszy projekt **STESample. Entities** w **Eksplorator rozwiązań** i wybierz polecenie **Dodaj odwołanie...**
    -   W programie Visual Studio 2012 — zaznacz pole wyboru obok pozycji **System. Runtime. Serialization** i kliknij przycisk **OK** .
    -   W programie Visual Studio 2010 — wybierz pozycję **System. Runtime. Serialization** i kliknij przycisk **OK** .

Na koniec projekt z naszym kontekstem musi być odwołaniem do typów jednostek.

-   Kliknij prawym przyciskiem myszy projekt **STESample** w **Eksplorator rozwiązań** i wybierz polecenie **Dodaj odwołanie...**
    -   W programie Visual Studio 2012 — wybierz **rozwiązanie** z okienka po lewej stronie, zaznacz pole wyboru obok pozycji **STESample. Entities** i kliknij przycisk **OK** .
    -   W programie Visual Studio 2010 — wybierz kartę **projekty** , wybierz pozycję **STESample. Entities** , a następnie kliknij przycisk **OK** .

>[!NOTE]
> Kolejną opcją przenoszenia typów jednostek do oddzielnego projektu jest przeniesienie pliku szablonu zamiast łączenia go z lokalizacji domyślnej. W takim przypadku należy zaktualizować zmienną **plik_wejściowy** w szablonie, aby zapewnić ścieżkę względną do pliku edmx (w tym przykładzie będzie to **..\\BloggingModel. edmx**).

## <a name="create-a-wcf-service"></a>Tworzenie usługi WCF

Teraz czas na dodanie usługi WCF w celu udostępnienia naszych danych zostanie rozpoczęty przez utworzenie projektu.

-   **&gt; dodawania&gt; projektu...**
-   Wybierz pozycję **Visual C\#** w lewym okienku, a następnie **aplikację usługi WCF**
-   Wprowadź **STESample. Service** jako nazwę, a następnie kliknij przycisk **OK** .
-   Dodawanie odwołania do zestawu **System. Data. Entity**
-   Dodawanie odwołania do projektów **STESample** i **STESample. Entities**

Musimy skopiować parametry połączenia EF do tego projektu, aby znalazł się on w czasie wykonywania.

-   Otwórz plik **App. config** projektu **STESample **i skopiuj element **connectionStrings**
-   Wklej element **connectionStrings** jako element podrzędny elementu **konfiguracji** w pliku **Web. config** w projekcie **STESample. Service**

Teraz czas na wdrożenie rzeczywistej usługi.

-   Otwórz **IService1.cs** i Zastąp zawartość następującym kodem

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   Otwórz **Service1. svc** i Zastąp zawartość następującym kodem

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a>Korzystanie z usługi z poziomu aplikacji konsolowej

Utwórzmy aplikację konsolową, która używa naszej usługi.

-   **Plik —&gt; nowy&gt; projekt...**
-   Wybierz pozycję **Visual C\#** w okienku po lewej stronie, a następnie **aplikację konsolową**
-   Wprowadź **STESample. ConsoleTest** jako nazwę, a następnie kliknij przycisk **OK** .
-   Dodaj odwołanie do projektu **STESample. Entities**

Potrzebujemy odwołania do usługi dla naszej usługi WCF

-   Kliknij prawym przyciskiem myszy projekt **STESample. ConsoleTest** w **Eksplorator rozwiązań** i wybierz pozycję **Dodaj odwołanie do usługi...**
-   Kliknij pozycję **odkryj**
-   Wprowadź **BloggingService** jako przestrzeń nazw, a następnie kliknij przycisk **OK** .

Teraz możemy napisać kod, aby korzystać z usługi.

-   Otwórz **program.cs** i Zastąp zawartość następującym kodem.

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

Teraz możesz uruchomić aplikację, aby zobaczyć ją w działaniu.

-   Kliknij prawym przyciskiem myszy projekt **STESample. ConsoleTest** w **Eksplorator rozwiązań** i wybierz polecenie **Debuguj-&gt; Uruchom nowe wystąpienie**

Po uruchomieniu aplikacji zobaczysz następujące dane wyjściowe.

```console
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a>Korzystanie z usługi z aplikacji WPF

Utwórzmy aplikację WPF, która używa naszej usługi.

-   **Plik —&gt; nowy&gt; projekt...**
-   Wybierz pozycję **Visual C\#** z okienka po lewej stronie, a następnie **aplikację WPF**
-   Wprowadź **STESample. WPFTest** jako nazwę, a następnie kliknij przycisk **OK** .
-   Dodaj odwołanie do projektu **STESample. Entities**

Potrzebujemy odwołania do usługi dla naszej usługi WCF

-   Kliknij prawym przyciskiem myszy projekt **STESample. WPFTest** w **Eksplorator rozwiązań** i wybierz pozycję **Dodaj odwołanie do usługi...**
-   Kliknij pozycję **odkryj**
-   Wprowadź **BloggingService** jako przestrzeń nazw, a następnie kliknij przycisk **OK** .

Teraz możemy napisać kod, aby korzystać z usługi.

-   Otwórz **MainWindow. XAML** i Zastąp zawartość następującym kodem.

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   Otwórz kod związany z MainWindow (**MainWindow.XAML.cs**) i Zastąp zawartość następującym kodem

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

Teraz możesz uruchomić aplikację, aby zobaczyć ją w działaniu.

-   Kliknij prawym przyciskiem myszy projekt **STESample. WPFTest** w **Eksplorator rozwiązań** i wybierz polecenie **Debuguj-&gt; Uruchom nowe wystąpienie**
-   Można manipulować danymi przy użyciu ekranu i zapisywać je za pośrednictwem usługi przy użyciu przycisku **Zapisz**

![Główne okno WPF](~/ef6/media/wpf.png)
