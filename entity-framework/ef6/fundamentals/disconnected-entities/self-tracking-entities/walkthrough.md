---
title: Samodzielnie śledzenia jednostek przewodnik — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: d89c452410d34bea71e8220aae141c3bfca3e1ce
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490277"
---
# <a name="self-tracking-entities-walkthrough"></a>Śledzenie własnym wskazówki jednostek
> [!IMPORTANT]
> Nie zaleca się przy użyciu szablonu samoobsługowego tracking jednostek. Tylko będą dostępne do obsługi istniejących aplikacji. Jeśli aplikacja wymaga pracy z wykresami odłączonych jednostek, należy wziąć pod uwagę inne alternatywy dla takich jak [słupkowych jednostek](http://trackableentities.github.io/), która jest podobna do samoobsługowego-Tracking-jednostek, które jest bardziej aktywnie rozwijany przez technologię Społeczność lub pisanie kodu niestandardowego za pomocą śledzenia interfejsów API zmian niskiego poziomu.

W tym instruktażu przedstawiono scenariusz, w którym usługi Windows Communication Foundation (WCF) udostępnia operację, która zwraca grafu jednostki. Następnie aplikacja kliencka manipuluje tego wykresu i przesyła zmiany do operacji usługi, która weryfikuje i zapisuje aktualizacji do bazy danych przy użyciu platformy Entity Framework.

Przed wykonaniem tego przewodnika, upewnij się, możesz przeczytać [jednostek Self-Tracking](index.md) strony.

W tym przewodniku wykona następujące czynności:

-   Tworzy bazę danych w celu uzyskania dostępu do.
-   Tworzy bibliotekę klas, który zawiera model.
-   Zamiany w szablonie Self-Tracking jednostki Generator.
-   Przenosi klas jednostek do oddzielnego projektu.
-   Tworzy usługę WCF, która udostępnia operacje do wykonywania zapytań i zapisać jednostki.
-   Tworzy klienta aplikacji (konsoli i WPF), które mogą korzystać z usługi.

Użyjemy pierwszej bazy danych w ramach tego przewodnika, ale te same techniki stosuje się jednakowo do pierwszego modelu.

## <a name="pre-requisites"></a>Wymagania wstępne

Do przeprowadzenia tego instruktażu potrzebujesz najnowszej wersji programu Visual Studio.

## <a name="create-a-database"></a>Tworzenie bazy danych

Serwer bazy danych, który został zainstalowany przy użyciu programu Visual Studio różni się zależnie od wersji programu Visual Studio zostały zainstalowane:

-   Jeśli używasz programu Visual Studio 2012, a następnie będzie utworzenie bazy danych LocalDB.
-   Jeśli używasz programu Visual Studio 2010 zostanie utworzona baza danych programu SQL Express.

Rozpocznijmy i wygenerować bazę danych.

-   Otwórz program Visual Studio
-   **Widok —&gt; Eksploratora serwera**
-   Kliknij prawym przyciskiem myszy **połączeń danych -&gt; Dodaj połączenie...**
-   Jeśli nie jest połączona z bazą danych za pomocą Eksploratora serwera, zanim będzie konieczne wybranie **programu Microsoft SQL Server** jako źródło danych
-   Łączenie się z LocalDB lub SQL Express, w zależności od tego, który z nich został zainstalowany
-   Wprowadź **STESample** jako nazwa bazy danych
-   Wybierz **OK** i uzyskasz, jeśli chcesz utworzyć nową bazę danych, wybierz opcję **tak**
-   Nowa baza danych będzie teraz wyświetlany w Eksploratorze serwera
-   Jeśli używasz programu Visual Studio 2012
    -   Kliknij prawym przyciskiem myszy w bazie danych w Eksploratorze serwera i wybierz **nowe zapytanie**
    -   Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonania**
-   Jeśli używasz programu Visual Studio 2010
    -   Wybierz **dane —&gt; języka Transact SQL edytor —&gt; nowego połączenia zapytania...**
    -   Wprowadź **.\\ SQLEXPRESS** jako nazwę serwera i kliknij przycisk **OK**
    -   Wybierz **STESample** bazy danych z listy rozwijanej w górnej części edytora zapytań
    -   Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonaj instrukcję SQL**

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

Up, potrzebujemy projektu, aby przełączyć modelu.

-   **Plik —&gt; New -&gt; projektu...**
-   Wybierz **Visual C\#**  z okienka po lewej stronie i następnie **biblioteki klas**
-   Wprowadź **STESample** jako nazwę i kliknij przycisk **OK**

Teraz utworzymy prosty model w Projektancie platformy EF dostępu do naszej bazy danych:

-   **Projekt —&gt; Dodaj nowy element...**
-   Wybierz **danych** z okienka po lewej stronie i następnie **ADO.NET Entity Data Model**
-   Wprowadź **BloggingModel** jako nazwę i kliknij przycisk **OK**
-   Wybierz **Generuj z bazy danych** i kliknij przycisk **dalej**
-   Wprowadź informacje o połączeniu dla bazy danych, który został utworzony w poprzedniej sekcji
-   Wprowadź **BloggingContext** jako nazwy parametrów połączenia, a następnie kliknij przycisk **dalej**
-   Zaznacz pole wyboru obok pozycji **tabel** i kliknij przycisk **Zakończ**

## <a name="swap-to-ste-code-generation"></a>Przechodź do generowania kodu WKLEJ

Teraz należy wyłączyć domyślne generowanie kodu i wymiany Self-Tracking jednostek.

### <a name="if-you-are-using-visual-studio-2012"></a>Jeśli używasz programu Visual Studio 2012

-   Rozwiń **BloggingModel.edmx** w **Eksploratora rozwiązań** i Usuń **BloggingModel.tt** i **BloggingModel.Context.tt** 
     *Spowoduje to wyłączenie generowania kodu domyślnego*
-   Kliknij prawym przyciskiem myszy pusty obszar w Projektancie platformy EF powierzchni i wybierz **Dodaj element generowanie kodu...**
-   Wybierz **Online** w okienku po lewej stronie i wyszukaj **Generator WKLEJ**
-   Wybierz **Generator Wklej kod c\#**  szablonu, wprowadź **STETemplate** jako nazwę i kliknij przycisk **Dodaj**
-   **STETemplate.tt** i **STETemplate.Context.tt** pliki zostaną dodane zagnieżdżony w ramach pliku BloggingModel.edmx

### <a name="if-you-are-using-visual-studio-2010"></a>Jeśli używasz programu Visual Studio 2010

-   Kliknij prawym przyciskiem myszy pusty obszar w Projektancie platformy EF powierzchni i wybierz **Dodaj element generowanie kodu...**
-   Wybierz **kodu** z okienka po lewej stronie i następnie **Generator jednostki Self-Tracking ADO.NET**
-   Wprowadź **STETemplate** jako nazwę i kliknij przycisk **Dodaj**
-   **STETemplate.tt** i **STETemplate.Context.tt** pliki zostaną dodane bezpośrednio do projektu

## <a name="move-entity-types-into-separate-project"></a>Przenieś typów jednostek do oddzielnego projektu

Aby korzystanie z jednostek Self-Tracking naszej aplikacji klienta musi mieć dostęp do klas jednostek wygenerowana przez nasz model. Ponieważ nie chcemy ujawniać cały model do aplikacji klienckiej będziemy przenosić klas jednostek do oddzielnego projektu.

Pierwszym krokiem jest zatrzymać generowanie klas jednostek w istniejący projekt:

-   Kliknij prawym przyciskiem myszy **STETemplate.tt** w **Eksploratora rozwiązań** i wybierz **właściwości**
-   W **właściwości** Wyczyść okno **TextTemplatingFileGenerator** z **CustomTool** właściwości
-   Rozwiń **STETemplate.tt** w **Eksploratora rozwiązań** i usunąć wszystkie pliki zagnieżdżony w nim

Następnie użyjemy Dodaj nowy projekt i wygenerowania klas obiektów w nim

-   **Plik —&gt; Add -&gt; projektu...**
-   Wybierz **Visual C\#**  z okienka po lewej stronie i następnie **biblioteki klas**
-   Wprowadź **STESample.Entities** jako nazwę i kliknij przycisk **OK**
-   **Projekt —&gt; Dodaj istniejący element...**
-   Przejdź do **STESample** folder projektu
-   Zaznacz, aby wyświetlić **wszystkie pliki (\*.\*)**
-   Wybierz **STETemplate.tt** pliku
-   Kliknij strzałkę listy rozwijanej obok **Dodaj** i wybrać **Dodaj jako łącze**

    ![Dodaj połączony szablon](~/ef6/media/addlinkedtemplate.png)

Przedstawimy również upewnij się, że wygenerowanie klas obiektów w tej samej przestrzeni nazw jako kontekst. Zmniejsza to po prostu liczbę za pomocą instrukcji, które musimy dodać w naszej aplikacji.

-   Kliknij prawym przyciskiem myszy na połączonym **STETemplate.tt** w **Eksploratora rozwiązań** i wybierz **właściwości**
-   W **właściwości** zestaw okna **niestandardowe narzędzie Namespace** do **STESample**

Kod wygenerowany przez szablon WKLEJ należy odwołanie do **System.Runtime.Serialization** w celu kompilacji. Ta biblioteka jest wymagany w przypadku WCF **DataContract** i **DataMember** atrybuty, które są używane dla typów możliwych do serializacji jednostek.

-   Kliknij prawym przyciskiem myszy **STESample.Entities** projektu w **Eksploratora rozwiązań** i wybierz **Dodaj odwołanie...**
    -   W programie Visual Studio 2012 — zaznacz pole wyboru obok pozycji **System.Runtime.Serialization** i kliknij przycisk **OK**
    -   W programie Visual Studio 2010 — wybierz **System.Runtime.Serialization** i kliknij przycisk **OK**

Na koniec projektu z nasz kontekst w nim należy odwołania do typów jednostek.

-   Kliknij prawym przyciskiem myszy **STESample** projektu w **Eksploratora rozwiązań** i wybierz **Dodaj odwołanie...**
    -   W programie Visual Studio 2012 — wybierz **rozwiązania** z okienka po lewej stronie, zaznacz pole wyboru obok pozycji **STESample.Entities** i kliknij przycisk **OK**
    -   W programie Visual Studio 2010 — wybierz **projektów** zaznacz **STESample.Entities** i kliknij przycisk **OK**

>[!NOTE]
> Inną opcją przechodzenia typów jednostek do oddzielnego projektu jest można przenieść pliku szablonu, a nie połączenie go z jego domyślnej lokalizacji. Jeśli to zrobisz, musisz zaktualizować **wejściowy** zmiennej szablonu, aby podać ścieżkę względną do pliku edmx (w tym przykładzie, która byłaby **... \\BloggingModel.edmx**).

## <a name="create-a-wcf-service"></a>Tworzenie usługi WCF

Teraz nadszedł czas, aby dodać usługi WCF w celu udostępnienia danych, Zaczniemy od utworzenia projektu.

-   **Plik —&gt; Add -&gt; projektu...**
-   Wybierz **Visual C\#**  z okienka po lewej stronie i następnie **aplikacja usługi WCF**
-   Wprowadź **STESample.Service** jako nazwę i kliknij przycisk **OK**
-   Dodaj odwołanie do **System.Data.Entity** zestawu
-   Dodaj odwołanie do **STESample** i **STESample.Entities** projektów

Potrzebujemy skopiować parametry połączenia platformy EF do tego projektu, dzięki czemu znajduje się w czasie wykonywania.

-   Otwórz **App.Config** plik ** STESample ** projektu i skopiuj **connectionStrings** — element
-   Wklej **connectionStrings** element jako element podrzędny **konfiguracji** elementu **Web.Config** w pliku **STESample.Service** projektu

Teraz nadszedł czas, aby zaimplementować rzeczywistej usługi.

-   Otwórz **IService1.cs** i zastąp jego zawartość następującym kodem

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

-   Otwórz **plik Service1.svc** i zastąp jego zawartość następującym kodem

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

## <a name="consume-the-service-from-a-console-application"></a>Korzystanie z usługi z aplikacji konsoli

Utwórz aplikację konsolową, która korzysta z naszych usług.

-   **Plik —&gt; New -&gt; projektu...**
-   Wybierz **Visual C\#**  z okienka po lewej stronie i następnie **aplikacji konsoli**
-   Wprowadź **STESample.ConsoleTest** jako nazwę i kliknij przycisk **OK**
-   Dodaj odwołanie do **STESample.Entities** projektu

Potrzebujemy odwołanie do usługi dla usługi WCF

-   Kliknij prawym przyciskiem myszy **STESample.ConsoleTest** projektu w **Eksploratora rozwiązań** i wybierz **Dodaj odwołanie do usługi...**
-   Kliknij przycisk **odnajdywania**
-   Wprowadź **BloggingService** jako przestrzeń nazw i kliknij przycisk **OK**

Obecnie firma Microsoft może pisanie kodu w celu korzystania z usługi.

-   Otwórz **Program.cs** i zastąp jego zawartość następującym kodem.

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

Teraz można uruchomić aplikacji, aby zobaczyć go w działaniu.

-   Kliknij prawym przyciskiem myszy **STESample.ConsoleTest** projektu w **Eksploratora rozwiązań** i wybierz **debugowanie -&gt; Uruchom nowe wystąpienie**

Zostaną wyświetlone następujące dane wyjściowe, gdy aplikacja wykonuje.

```
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

Utworzymy aplikację WPF, która korzysta z naszych usług.

-   **Plik —&gt; New -&gt; projektu...**
-   Wybierz **Visual C\#**  z okienka po lewej stronie i następnie **aplikacji WPF**
-   Wprowadź **STESample.WPFTest** jako nazwę i kliknij przycisk **OK**
-   Dodaj odwołanie do **STESample.Entities** projektu

Potrzebujemy odwołanie do usługi dla usługi WCF

-   Kliknij prawym przyciskiem myszy **STESample.WPFTest** projektu w **Eksploratora rozwiązań** i wybierz **Dodaj odwołanie do usługi...**
-   Kliknij przycisk **odnajdywania**
-   Wprowadź **BloggingService** jako przestrzeń nazw i kliknij przycisk **OK**

Obecnie firma Microsoft może pisanie kodu w celu korzystania z usługi.

-   Otwórz **MainWindow.xaml** i zastąp jego zawartość następującym kodem.

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

-   Otwórz kod związany z dla głównego (**MainWindow.xaml.cs**) i zastąp jego zawartość następującym kodem

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

Teraz można uruchomić aplikacji, aby zobaczyć go w działaniu.

-   Kliknij prawym przyciskiem myszy **STESample.WPFTest** projektu w **Eksploratora rozwiązań** i wybierz **debugowanie -&gt; Uruchom nowe wystąpienie**
-   Można manipulować danymi na ekranie i zapisać go za pośrednictwem usługi przy użyciu **Zapisz** przycisku

![Okno główne programu WPF](~/ef6/media/wpf.png)
