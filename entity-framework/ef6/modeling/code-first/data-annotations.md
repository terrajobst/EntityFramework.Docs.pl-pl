---
title: Code First adnotacje danych — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 9fac2a90c46d78ff5fd632800cc0050276467773
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419186"
---
# <a name="code-first-data-annotations"></a>Code First adnotacje danych
> [!NOTE]
> **Dr 4.1 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 4,1. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie te informacje nie są stosowane.

Zawartość na tej stronie jest dostosowana z artykułu utworzonego pierwotnie przez Julie Lerman (\<http://thedatafarm.com>).

Entity Framework Code First umożliwia korzystanie z własnych klas domeny do reprezentowania modelu, w którym opiera się program Dr, aby wykonywać zapytania, śledzenie zmian i aktualizowanie funkcji. Code First wykorzystuje wzorzec programistyczny nazywany "Konwencją od konfiguracji". W Code First założono, że klasy są zgodne z konwencjami Entity Framework i w takim przypadku automatycznie postanowili, jak wykonać swoje zadanie. Jeśli jednak klasy nie są zgodne z tymi konwencjami, można dodać konfiguracje do klas, aby dostarczyć EF z wymaganymi informacjami.

Code First zapewnia dwa sposoby dodawania tych konfiguracji do klas. Jeden z nich korzysta z prostych atrybutów o nazwie DataAnnotations, a drugi korzysta z interfejsu API Fluent Code First, który zapewnia sposób bezpodstawnego opisywania konfiguracji w kodzie.

Ten artykuł koncentruje się na używaniu adnotacji danych (w przestrzeni nazw System. ComponentModel. DataAnnotations) w celu skonfigurowania klas — wyróżnianie najczęściej potrzebnych konfiguracji. Adnotacje DataAnnotations są również zrozumiałe dla wielu aplikacji platformy .NET, takich jak ASP.NET MVC, dzięki czemu te aplikacje mogą korzystać z tych samych adnotacji w przypadku walidacji po stronie klienta.


## <a name="the-model"></a>Model

Pokażę Code First DataAnnotations z prostą parą klas: blog i post.

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }

    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public DateTime DateCreated { get; set; }
        public string Content { get; set; }
        public int BlogId { get; set; }
        public ICollection<Comment> Comments { get; set; }
    }
```

Podobnie jak w przypadku, w blogu i na wpisach klasy te wygodnie przestrzegać pierwszej Konwencji kodu i nie wymagają żadnych dostosowań, aby zapewnić kompatybilność EF. Można jednak również użyć adnotacji, aby uzyskać więcej informacji na temat klas i bazy danych, do której są mapowane.

 

## <a name="key"></a>Klucz

Entity Framework opiera się na każdej jednostce mającej wartość klucza, która jest używana do śledzenia jednostek. Jedną z Konwencji Code First są niejawne właściwości klucza; Code First będzie szukać właściwości o nazwie "ID" lub kombinacji nazwy klasy i "ID", takiej jak "BlogId". Ta właściwość zostanie zamapowana na kolumnę klucza podstawowego w bazie danych.

Zarówno w blogu, jak i po tej Konwencji są stosowane następujące klasy. Co zrobić, jeśli nie? Co zrobić, jeśli blog użył nazwy *PrimaryTrackingKey* zamiast tego, a nawet *foo*? Jeśli kod nie znajduje właściwości zgodnej z tą konwencją, zgłosi wyjątek z powodu Entity Framework wymaga, aby właściwość klucza była wymagana. Możesz użyć adnotacji klucza, aby określić, która właściwość ma być używana jako obiekt EntityKey.

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

Jeśli używasz funkcji generowania bazy danych w pierwszej kolejności, tabela blogu będzie miała kolumnę klucza podstawowego o nazwie PrimaryTrackingKey, która również jest definiowana jako tożsamość domyślnie.

![Tabela blogów z kluczem podstawowym](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a>Klucze złożone

Entity Framework obsługuje klucze złożone — klucze podstawowe, które składają się z więcej niż jednej właściwości. Na przykład może istnieć Klasa paszportu, której klucz podstawowy jest kombinacją PassportNumber i IssuingCountry.

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

Próba użycia powyższej klasy w modelu EF spowoduje, że `InvalidOperationException`:

*Nie można określić porządkowania złożonego klucza podstawowego dla typu "Passport". Użyj metody ColumnAttribute lub Haskey —, aby określić zamówienie dla złożonych kluczy podstawowych.*

Aby można było używać kluczy złożonych, Entity Framework wymaga zdefiniowania zamówienia dla właściwości klucza. Można to zrobić za pomocą adnotacji kolumny, aby określić kolejność.

>[!NOTE]
> Wartość Order jest względna (a nie oparta na indeksach), więc można użyć dowolnych wartości. Na przykład 100 i 200 byłyby dopuszczalne zamiast 1 i 2.

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

Jeśli masz jednostki z złożonymi kluczami obcymi, musisz określić tę samą kolejność kolumn, która została użyta dla odpowiednich właściwości klucza podstawowego.

Tylko kolejność względna we właściwościach klucza obcego musi być taka sama, dokładne wartości przypisane do **kolejności** nie muszą być zgodne. Na przykład w przypadku następujących klas można użyć 3 i 4 zamiast 1 i 2.

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a>Wymagany

Wymagana adnotacja informuje Dr, że określona właściwość jest wymagana.

Dodanie wymagane do właściwości title spowoduje wymuszenie EF (i MVC) w celu upewnienia się, że właściwość zawiera dane.

``` csharp
    [Required]
    public string Title { get; set; }
```

W przypadku braku dodatkowych zmian kodu lub znaczników w aplikacji aplikacja MVC przeprowadzi weryfikację po stronie klienta, nawet dynamicznie kompilując komunikat przy użyciu nazw właściwości i adnotacji.

![Błąd tworzenia strony z tytułem](~/ef6/media/jj591583-figure02.png)

Wymagany atrybut będzie również miał wpływ na wytworzoną bazę danych, ponieważ nie dopuszcza ona wartości null dla mapowanej właściwości. Zwróć uwagę, że pole title zostało zmienione na wartość "not null".

>[!NOTE]
> W niektórych przypadkach może nie być możliwe, aby kolumna w bazie danych nie mogła dopuszczać wartości null, mimo że właściwość jest wymagana. Na przykład w przypadku korzystania z danych strategii dziedziczenia TPH dla wielu typów jest przechowywany w jednej tabeli. Jeśli typ pochodny zawiera wymaganą właściwość, nie można wprowadzić wartości null w kolumnie, ponieważ nie wszystkie typy w hierarchii będą mieć tę właściwość.

 

![Tabela blogów](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a>MaxLength i MinLength

Atrybuty MaxLength i MinLength umożliwiają określenie dodatkowych walidacji właściwości, tak jak w przypadku, gdy jest to wymagane.

Oto wymagania dotyczące długości. W przykładzie pokazano również, jak łączyć atrybuty.

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

Adnotacja MaxLength będzie miała wpływ na bazę danych, ustawiając długość właściwości na 10.

![Tabela blogów pokazująca maksymalną długość w kolumnie Bloggername](~/ef6/media/jj591583-figure04.png)

Adnotacja po stronie klienta MVC i adnotacja po stronie serwera EF 4,1 będą zarówno uwzględniane w tej weryfikacji, jak i ponownie dynamicznie kompilują komunikat o błędzie: "pole Bloggername musi być ciągiem lub typem tablicy o maksymalnej długości" 10 "." Ten komunikat jest nieco długi. Wiele adnotacji pozwala określić komunikat o błędzie z atrybutem ErrorMessage.

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

W wymaganej adnotacji można także określić ErrorMessage.

![Utwórz stronę z niestandardowym komunikatem o błędzie](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a>NotMapped

Pierwsza Konwencja kodu wskazuje, że każda właściwość o obsługiwanym typie danych jest reprezentowana w bazie danych. Ale nie zawsze jest to przypadek w aplikacjach. Na przykład może istnieć właściwość w klasie blogu, która tworzy kod na podstawie pól tytuł i Bloggername. Tę właściwość można utworzyć dynamicznie i nie musi być przechowywana. Można oznaczyć wszystkie właściwości, które nie są mapowane do bazy danych za pomocą adnotacji NotMapped, takiej jak ta właściwość BlogCode.

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a>ComplexType

Nie jest bardzo rzadko opisywanie jednostek domeny w zestawie klas, a następnie warstwowe klasy do opisywania kompletnej jednostki. Na przykład możesz dodać klasę o nazwie BlogDetails do modelu.

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Zwróć uwagę, że BlogDetails nie ma żadnego typu właściwości klucza. W projekcie opartym na domenie BlogDetails jest określany jako obiekt wartości. Entity Framework odnosi się do obiektów wartości jako typów złożonych.  Typy złożone nie mogą być śledzone we własnym zakresie.

Jednak jako właściwość w klasie blogu BlogDetails ją będzie śledzony jako część obiektu blogu. Aby najpierw rozpoznać kod, należy oznaczyć klasę BlogDetails jako ComplexType.

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Teraz możesz dodać właściwość w klasie blogu, aby reprezentować BlogDetails dla tego bloga.

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

W bazie danych tabela blogów zawiera wszystkie właściwości blogu, w tym właściwości zawarte we właściwości BlogDetail. Domyślnie każda z nich jest poprzedzona nazwą typu złożonego, BlogDetail.

![Tabela blogów z typem złożonym](~/ef6/media/jj591583-figure06.png)


## <a name="concurrencycheck"></a>ConcurrencyCheck

Adnotacja ConcurrencyCheck umożliwia Oflagowanie jednej lub wielu właściwości, które mają być używane na potrzeby sprawdzania współbieżności w bazie danych, gdy użytkownik edytuje lub usunie jednostkę. Jeśli pracujesz z projektantem EF, jest to zgodne z ustawieniem ConcurrencyMode właściwości, który został naprawiony.

Zobaczmy, jak działa ConcurrencyCheck przez dodanie go do właściwości Bloggername.

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Gdy metody SaveChanges jest wywoływana, ze względu na adnotację ConcurrencyCheck w polu Bloggername, oryginalna wartość tej właściwości zostanie użyta w ramach aktualizacji. Polecenie spróbuje zlokalizować prawidłowy wiersz przez filtrowanie nie tylko dla wartości klucza, ale również od oryginalnej wartości Bloggername.  Poniżej przedstawiono krytyczne części polecenia aktualizacji wysyłane do bazy danych, w którym można zobaczyć, że polecenie zaktualizuje wiersz, który ma PrimaryTrackingKey ma wartość 1, a w polu Bloggername elementu "Julie", który był oryginalną wartością, gdy ten blog został pobrany z bazy danych.

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

Jeśli ktoś zmienił nazwę usługi Blogger dla tego bloga w międzyczasie, ta aktualizacja zakończy się niepowodzeniem i uzyskasz DbUpdateConcurrencyException, które należy obsłużyć.

 

## <a name="timestamp"></a>TimeStamp

Jest to bardziej powszechne, aby użyć pól rowversion lub timestamp do sprawdzania współbieżności. Zamiast używać adnotacji ConcurrencyCheck, można użyć bardziej specyficznej adnotacji sygnatury czasowej, tak długo, jak typ właściwości jest tablicą bajtów. Kod najpierw traktuje właściwości sygnatur czasowych tak samo jak właściwości ConcurrencyCheck, ale również upewnij się, że pole bazy danych, które generuje najpierw kod, nie dopuszcza wartości null. W danej klasie można mieć tylko jedną właściwość timestamp.

Dodanie następującej właściwości do klasy bloga:

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

wyniki w kodzie najpierw tworzą kolumnę znacznika czasu niedopuszczające wartości null w tabeli bazy danych.

![Tabela blogów z kolumną sygnatury czasowej](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a>Tabela i kolumna

Jeśli chcesz Code First utworzyć bazę danych, możesz zmienić nazwę tabel i kolumn, które tworzy. Możesz również użyć Code First z istniejącą bazą danych. Ale nie zawsze jest to przypadek, że nazwy klas i właściwości w domenie są zgodne z nazwami tabel i kolumn w bazie danych.

Moja Klasa jest nazywana blogiem i Konwencją, kod najpierw przyjmuje, że zostanie on zmapowany do tabeli o nazwie blogi. Jeśli tak nie jest, możesz określić nazwę tabeli z atrybutem tabeli. Na przykład adnotacja określa, że nazwa tabeli to InternalBlogs.

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

Adnotacja kolumny to więcej doświadczony w określaniu atrybutów mapowanej kolumny. Można określić nazwę, typ danych, a nawet kolejność, w jakiej w tabeli pojawia się kolumna. Oto przykład atrybutu Column.

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

Nie należy mylić atrybutu TypeName kolumny z adnotacją datadatatype. DataType jest adnotacją użytą dla interfejsu użytkownika i jest ignorowana przez Code First.

Poniżej znajduje się tabela po jej ponownej generacji. Nazwa tabeli została zmieniona na InternalBlogs, a kolumna opisu typu złożonego jest teraz BlogDescription. Ponieważ nazwa została określona w adnotacji, kod pierwszy nie będzie używać konwencji rozpoczynającej nazwę kolumny o nazwie typu złożonego.

![Zmieniono nazwę tabeli i kolumny blogów](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a>DatabaseGenerated

Ważne funkcje bazy danych to możliwość posiadania obliczanych właściwości. Jeśli mapujesz klasy Code First do tabel zawierających kolumny obliczane, nie chcesz, aby Entity Framework do aktualizacji tych kolumn. Ale chcesz, aby program Dr zwracał te wartości z bazy danych po wstawieniu lub zaktualizowaniu danych. Możesz użyć adnotacji DatabaseGenerated, aby oflagować te właściwości w klasie wraz z obliczanym wyliczeniem. Inne wyliczenia to brak i tożsamość.

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

Możesz użyć bazy danych wygenerowanej w kolumnach bajtowych lub sygnatur czasowych, gdy pierwszy generuje bazę danych, w przeciwnym razie należy używać jej tylko podczas wskazywania istniejących baz danych, ponieważ kod nie będzie mógł określić formuły dla kolumny obliczanej.

Przeczytasz powyżej, ponieważ domyślnie właściwość klucza, która jest liczbą całkowitą, staje się kluczem tożsamości w bazie danych. Jest to takie samo, jak ustawienie DatabaseGenerated na DatabaseGeneratedOption. Identity. Jeśli nie chcesz, aby był on kluczem tożsamości, możesz ustawić wartość na DatabaseGeneratedOption. None.

 

## <a name="index"></a>Indeks

> [!NOTE]
> **Dr 6.1 tylko** -atrybut indeksu został wprowadzony w Entity Framework 6,1. Jeśli używasz wcześniejszej wersji, informacje przedstawione w tej sekcji nie są stosowane.

Możesz utworzyć indeks dla jednej lub wielu kolumn przy użyciu **indeksuattribute**. Dodanie atrybutu do co najmniej jednej właściwości spowoduje, że program Dr utworzy odpowiedni indeks w bazie danych podczas tworzenia bazy danych lub tworzy szkielet odpowiednich wywołań funkcji **indeksu** w przypadku korzystania z migracje Code First.

Na przykład poniższy kod spowoduje utworzenie indeksu w kolumnie **Klasyfikacja** tabeli **wpisy** w bazie danych.

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

Domyślnie indeks będzie miał nazwę **\_, &lt;nazwa właściwości&gt;** (IX\_Klasyfikacja w powyższym przykładzie). Możesz również określić nazwę dla indeksu. Poniższy przykład określa, że indeks powinien mieć nazwę **PostRatingIndex**.

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

Domyślnie indeksy nie są unikatowe, ale można użyć **Isuniqued** nazwanego parametru, aby określić, że indeks powinien być unikatowy. W poniższym przykładzie wprowadzono unikatowy indeks nazwy logowania **użytkownika**.

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a>Indeksy z wieloma kolumnami

Indeksy obejmujące wiele kolumn są określane przy użyciu tej samej nazwy w wielu adnotacjach indeksu dla danej tabeli. Podczas tworzenia indeksów wielokolumnowych należy określić kolejność kolumn w indeksie. Na przykład poniższy kod tworzy indeks z wielokolumnową **klasyfikacją** i **BlogId** o nazwie **IX\_BlogIdAndRating**. **BlogId** jest pierwszą kolumną w indeksie i **klasyfikacją** jest drugi.

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a>Atrybuty relacji: InverseProperty i ForeignKey

> [!NOTE]
> Ta strona zawiera informacje na temat konfigurowania relacji w modelu Code First przy użyciu adnotacji danych. Aby uzyskać ogólne informacje na temat relacji w EF oraz jak uzyskać dostęp do danych i manipulować nimi przy użyciu relacji, zobacz [relacje & właściwości nawigacji](~/ef6/fundamentals/relationships.md). *

Pierwsza Konwencja kodowa będzie uwzględniać najpopularniejsze relacje w modelu, ale zdarzają się sytuacje, w których potrzebna jest pomoc.

Zmiana nazwy właściwości klucza w klasie blogu stworzyła problem z relacją do opublikowania. 

Podczas generowania bazy danych, kod najpierw widzi Właściwość BlogId w klasie post i rozpoznaje ją, zgodnie z Konwencją zgodną z nazwą klasy i "identyfikatorem" jako kluczem obcym klasy blogu. Nie istnieje jednak żadna właściwość BlogId w klasie blogu. Rozwiązanie to służy do tworzenia właściwości nawigacji w wpisie i używania zastosowanej adnotacji DataAnnotation, aby pomóc w kodzie najpierw zrozumieć, jak utworzyć relację między dwoma klasami — za pomocą właściwości post. BlogId — a także określić ograniczenia w Database.

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

W ograniczeniu w bazie danych zostanie wyświetlona relacja między InternalBlogs. PrimaryTrackingKey i Posts. BlogId. 

![Relacja między InternalBlogs. PrimaryTrackingKey i Posts. BlogId](~/ef6/media/jj591583-figure09.png)

InverseProperty jest używany, gdy istnieje wiele relacji między klasami.

W klasie post możesz chcieć śledzić, kto napisał wpis w blogu, a także kto go edytował. Poniżej przedstawiono dwie nowe właściwości nawigacji dla klasy post.

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

Należy również dodać do klasy Person, do której odwołują się te właściwości. Klasa Person ma właściwości nawigacji z powrotem do wpisu, jeden dla wszystkich wpisów zapisanych przez osobę i jeden dla wszystkich wpisów zaktualizowanych przez tę osobę.

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

Kod nie jest w stanie dopasować właściwości do obu klas samodzielnie. Tabela bazy danych dla wpisów powinna mieć jeden klucz obcy dla osoby CreatedBy i jeden dla osoby UpdatedBy, ale najpierw kod utworzy cztery właściwości klucza obcego: Person\_ID, Person\_Id1, CreatedBy\_ID i UpdatedBy\_ID.

![Tabela ogłoszeń z dodatkowymi kluczami obcym](~/ef6/media/jj591583-figure10.png)

Aby rozwiązać te problemy, można użyć adnotacji InverseProperty do określenia wyrównania właściwości.

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

Ponieważ właściwość PostsWritten w osobach wie, że odnosi się to do typu wpisu, spowoduje skompilowanie relacji do post. CreatedBy. Podobnie PostsUpdated zostanie połączony z wpisem post. UpdatedBy. I najpierw kod nie utworzy dodatkowych kluczy obcych.

![Tabela ogłoszeń bez dodatkowych kluczy obcych](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a>Podsumowanie

Adnotacje DataAnnotations nie tylko opisują weryfikację po stronie klienta i serwera w pierwszej klasie kodowej, ale umożliwiają również zwiększenie i nawet poprawienie założeń, które w pierwszej kolejności składają się na klasy na podstawie Konwencji. Za pomocą adnotacji DataAnnotation można nie tylko dyskowo generować schemat bazy danych, ale można również mapować najpierw klasy kodu do istniejącej bazy danych.

Chociaż są bardzo elastyczne, należy pamiętać, że adnotacje danych udostępniają tylko najczęściej potrzebne zmiany konfiguracji, które można wprowadzić w przypadku pierwszej klasy kodu. Aby skonfigurować klasy dla niektórych przypadków brzegowych, należy odszukać alternatywny mechanizm konfiguracji, Code First interfejs API Fluent.
