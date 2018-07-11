---
title: Pierwsze adnotacje danych - EF6 kodu
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
caps.latest.revision: 3
ms.openlocfilehash: 91d1d8c2608df8f7b38e70b565a4225cf10ae21f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949310"
---
# <a name="code-first-data-annotations"></a>Adnotacje danych na pierwszym kodu
> [!NOTE]
> **EF4.1 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 4.1. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.

Zawartość na tej stronie są zaczerpnięte z, a artykuł pierwotnie napisane przez Julie Lerman (\<http://thedatafarm.com>).

Entity Framework Code First pozwala na używanie własnych klas domeny do reprezentowania modelu, który EF opiera się na do wykonywania zapytań, śledzenia i zmienić aktualizacji funkcji. Kod najpierw wykorzystuje wzorzec programowania, nazywane Konwencji za pośrednictwem konfiguracji. Oznacza to, że kod najpierw zakłada, że Twoich zajęciach zgodne z konwencjami, używanych przez EF. W takim przypadku EF będzie można sprawdzić szczegóły go potrzebuje do wykonywania swojej pracy. Jednak jeśli Twoich zajęciach nie wykonuj tych konwencji, masz możliwość dodawania konfiguracje do swoich klas, aby zapewnić EF informacje, których potrzebuje.

Kod daje najpierw dodaj te konfiguracje do swoich klas na dwa sposoby. Jeden używa proste atrybuty o nazwie DataAnnotations, a drugi to najpierw przy użyciu kodu jest interfejs Fluent API, który zapewnia sposób, aby opisać konfiguracje obowiązkowo, w kodzie.

Ten artykuł koncentruje się na konfigurowanie Twoich zajęciach — wyróżnianie najczęściej wymagane konfiguracje, za pomocą DataAnnotations (w przestrzeni nazw System.ComponentModel.DataAnnotations). DataAnnotations również są zrozumiałe przez kilka aplikacji .NET, takich jak ASP.NET MVC, która umożliwia tych aplikacji korzystać z tej samej adnotacji dla walidacji po stronie klienta.


## <a name="the-model"></a>Model

Zademonstruję kodu pierwszy DataAnnotations przy użyciu prostego pary klas: Blog i Post.

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

Jak są one klasy blogu i wpis wygodnie Konwencją pierwszy kodu i wymagane nie ulepszeń ułatwiające EF pracować z nimi. Ale można również użyć adnotacje na zapewnienie EF więcej informacji na temat klas i mapować je do bazy danych.

 

## <a name="key"></a>Key

Entity Framework opiera się na każdej jednostki o wartości klucza, która jest używana do śledzenia jednostek. Jedną z Konwencji, od których zależy najpierw kod jest sposób jej działanie polega na właściwość, która jest kluczem w każdą z klas pierwszy kodu. Konwencja jest odszukaj właściwość o nazwie "Id" lub taką, która łączy nazwę klasy i "Id", takich jak "BlogId". Właściwość będą mapowane na kolumny klucza podstawowego w bazie danych.

Klasy blogu i wpis stosują taką Konwencję. Ale co zrobić, jeśli nie? Co zrobić, jeśli użyto nazwy w blogu *PrimaryTrackingKey* w zamian lub nawet *foo*? Jeśli kod najpierw nie może znaleźć właściwość, która pasuje do niniejszej Konwencji spowoduje zgłoszenie wyjątku, ze względu na wymagania programu Entity Framework, musi mieć właściwość klucza. Można użyć klucza adnotacji, aby określić, które właściwości, które ma być używany jako EntityKey.

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

Jeśli najpierw przy użyciu kodu jest funkcją generowanie bazy danych, tabela blogu będzie miała kolumny klucza podstawowego o nazwie PrimaryTrackingKey, który również jest zdefiniowany jako tożsamość domyślnie.

![jj591583_figure01](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a>Klucze złożone

Entity Framework obsługuje kluczy złożonych — klucze podstawowe, które składają się z więcej niż jednej właściwości. Na przykład usługi może mieć klasy usługi Passport, którego klucz podstawowy jest kombinacją PassportNumber i IssuingCountry.

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

Gdyby i spróbuj użyć klasy powyżej w modelu platformy EF otrzymamy informacją InvalidOperationExceptions;

*Nie można określić złożonego podstawowego klucza porządkowanie dla typu "Paszport". Użyj metody HasKey lub ColumnAttribute, aby określić zamówienia złożone kluczy podstawowych.*

W przypadku kluczy złożonych Entity Framework wymaga do definiowania porządku właściwości klucza. Można to zrobić za pomocą adnotacji kolumny w celu określania kolejności.

>[!NOTE]
> Wartość kolejności jest względna (a nie na podstawie indeksu), dzięki czemu można używać dowolnej wartości. Na przykład 100 do 200 byłoby dopuszczalne zamiast 1 i 2.

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

W przypadku jednostek za pomocą kluczy złożonych obcego należy określić w tej samej kolumnie, porządkowanie, który był używany dla odpowiednich właściwości klucza podstawowego.

Tylko względną kolejność w ramach właściwości klucza obcego muszą być takie same, dokładne wartości, które są przypisane do **kolejność** nie muszą być zgodne. Na przykład w następującej klasy 3 i 4 może służyć zamiast 1 i 2.

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

## <a name="required"></a>Wymagane

Wymagane adnotacji informuje EF, czy określona właściwość jest wymagana.

Dodawanie wymaganych do właściwości Title wymusi EF (i MVC), aby upewnić się, że właściwość zawiera dane.

``` csharp
    [Required]
    public string Title { get; set; }
```

Nie dodatkowych bez zmiany kodu lub języka znaczników w aplikacji, aplikacji MVC przeprowadzi weryfikację po stronie klienta, nawet dynamiczne tworzenie komunikat przy użyciu nazwy właściwości i adnotacji.

![jj591583_figure02](~/ef6/media/jj591583-figure02.png)

Wymagany atrybut wpłynie również na wygenerowanej bazy danych, wprowadzając mapowanej właściwości niedopuszczającej. Należy zauważyć, że pole tytułu została zmieniona na "nie ma wartości null".

>[!NOTE]
> W niektórych przypadkach może nie być możliwe dla kolumny w bazie danych, może nie dopuszczać wartości null, nawet jeśli właściwość jest wymagana. Na przykład, gdy przy użyciu danych strategii TPH dziedziczenia dla wielu typów są przechowywane w jednej tabeli. Jeśli typ pochodny obejmuje wymagana właściwość kolumny nie można dokonać innych niż null, ponieważ nie wszystkie typy w hierarchii mają ta właściwość.

 

![jj591583_figure03](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a>Element MaxLength i MinLength

Atrybuty MaxLength i MinLength umożliwiają określenie dodatkowych właściwości sprawdzania poprawności, tak jak w przypadku wymagany.

Oto BloggerName o wymagania dotyczące długości. W przykładzie pokazano również sposób łączenia atrybutów.

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

Adnotacja MaxLength wpłynie na bazie danych przez ustawienie właściwości długości do 10.

![jj591583_figure04](~/ef6/media/jj591583-figure04.png)

Adnotacja po stronie klienta w MVC i EF 4.1 po stronie serwera adnotacji zarówno podlegają weryfikacji ponownie dynamicznie tworzenie komunikat o błędzie: "pole BloggerName musi być typu string lub tablicy o maksymalnej długości"10"." Ten komunikat jest nieco długi. Wiele adnotacje umożliwiają określenie komunikat o błędzie z atrybutem komunikat o błędzie.

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Można również określić komunikat o błędzie w wymaganych adnotacji.

![jj591583_figure05](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a>NotMapped

Kod Konwencji pierwsze decyduje o tym, że dla każdej właściwości, który jest obsługiwany typ danych jest reprezentowana w bazie danych. Ale to nie jest zawsze w przypadku aplikacji. Na przykład może mieć właściwości w klasie blogów, która tworzy kod, w oparciu o pola tytułu i BloggerName. Tej właściwości można tworzyć dynamicznie i nie musi być przechowywany. Możesz oznaczyć wszystkie właściwości, które nie są mapowane do bazy danych z adnotacją NotMapped, np. Ta właściwość BlogCode.

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

Nie jest niczym niezwykłym opisują jednostek domeny przez zestaw klas, a następnie warstwy tych klas do opisania całą jednostkę. Może na przykład dodać klasę o nazwie BlogDetails do modelu.

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Należy zauważyć, że BlogDetails nie ma żadnych typów właściwości klucza. W przypadku projektowania opartego na domenach BlogDetails nazywa się obiektu wartości. Entity Framework odnosi się do obiektów wartości jako typy złożone.  Typy złożone nie mogą być śledzone własnych.

Jednak jako właściwość w klasie blogu BlogDetails będzie śledzona jako część obiektu blogu. Aby najpierw umożliwia to rozpoznawanie kodu należy oznaczyć klasę BlogDetails jako ComplexType.

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Teraz można dodać właściwość w klasie blogu do reprezentowania BlogDetails na blogu.

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

W bazie danych w tabeli blogu będzie zawierać wszystkie właściwości blogu, w tym właściwości zawarte w jego właściwość BlogDetail. Domyślnie każdej z nich jest poprzedzone nazwą typu złożonego BlogDetail.

![jj591583_figure06](~/ef6/media/jj591583-figure06.png)

Inny interesujący Uwaga jest mimo, że właściwość DateCreated została zdefiniowana jako nieprzyjmujące wartości daty/godziny w klasie, pole odpowiedniej bazy danych dopuszcza wartości null. Należy użyć wymaganych adnotacji, jeśli chcesz mieć wpływ na schemat bazy danych.

 

## <a name="concurrencycheck"></a>ConcurrencyCheck

Adnotacja ConcurrencyCheck pozwala Flaga jedną lub więcej właściwości, które ma być używany dla współbieżności sprawdzanie w bazie danych, gdy użytkownik edytuje lub usunięcie jednostki. Jeśli masz doświadczenie w pracy z projektantem EF, jest to zgodne z ustawienie właściwości przed na stałe.

Zobaczmy, jak działa ConcurrencyCheck, dodając ją do właściwości BloggerName.

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Po wywołaniu funkcji SaveChanges ze względu na adnotacji ConcurrencyCheck pola BloggerName oryginalna wartość tej właściwości będzie używany w aktualizacji. Polecenie będzie podejmować próby zlokalizowania prawidłowego wiersza, filtrując nie tylko na wartości klucza, ale także w oryginalnej wartości BloggerName.  Poniżej przedstawiono krytycznych części polecenia UPDATE wysyłane do bazy danych, gdzie można zobaczyć, polecenie spowoduje zaktualizowanie wiersza, który ma PrimaryTrackingKey jest 1 i BloggerName "Julie", który podczas oryginalnej wartości w tym blogu został pobrany z bazy danych.

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

Jeśli ktoś zmienił nazwę blogger blogu w tym samym czasie, ta aktualizacja zakończy się niepowodzeniem, a otrzymasz DbUpdateConcurrencyException, który należy do obsługi.

 

## <a name="timestamp"></a>Znacznik czasu:

Jest to bardziej powszechne, aby używać pól rowversion lub sygnatura czasowa sprawdzania współbieżności. Jednak zamiast przy użyciu adnotacji ConcurrencyCheck, można użyć bardziej szczegółowe adnotacji sygnatury czasowej, tak długo, jak typ właściwości to tablica bajtów. Kod najpierw traktują właściwości sygnatury czasowej niż jako właściwości ConcurrencyCheck, ale jej będzie również upewnij się, że pole bazy danych, która najpierw generuje kod innych niż null. Może mieć tylko jedną właściwość sygnatury czasowej w danej klasy.

Dodawanie następującej właściwości do klasy Blog:

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

wyniki w kodzie, najpierw tworząc kolumnę sygnatur czasowych nie dopuszcza wartości null w tabeli bazy danych.

![jj591583_figure07](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a>Tabel i kolumn

Pozwalasz Code First utworzyć bazę danych, można zmienić nazwy tabel i kolumn, które, która zostanie utworzona. Umożliwia także Code First przy użyciu istniejącej bazy danych. Ale nie zawsze jest przypadek, że nazwy klas i właściwości w domenie pasują do nazw tabel i kolumn w bazie danych.

Moje klasy ma nazwę blogu i zgodnie z Konwencją kod najpierw przyjęto założenie, że to będzie zmapowana do tabeli o nazwie blogów. Jeśli tak nie jest atrybutem tabeli można określić nazwę tabeli. Tutaj na przykład adnotacja jest określającą, czy nazwa tabeli jest InternalBlogs.

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

Adnotacja kolumny jest więcej doświadczenie podczas określania atrybutów zamapowanych kolumn. Można zastrzec nazwę, typ danych lub nawet kolejność, w której kolumna pojawia się w tabeli. Oto przykład atrybut kolumny.

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

Nie należy mylić w kolumnie Nazwa typu atrybutu o DataAnnotation typu danych. Typ danych jest adnotacja używane dla interfejsu użytkownika i jest ignorowana przez rozwiązanie Code First.

Oto tabeli po jest zostały ponownie wygenerowane. Nazwa tabeli został zmieniony na InternalBlogs i opis kolumny z typu złożonego jest teraz BlogDescription. Ponieważ nazwa została określona w adnotacji, kod najpierw nie będzie używać konwencji początkowych nazwa kolumny o nazwie typu złożonego.

![jj591583_figure08](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a>DatabaseGenerated

Funkcje bazy danych ważna jest możliwość mają być obliczane właściwości. Jeśli masz mapowania klas Code First tabel zawierających kolumny obliczane, nie ma programu Entity Framework, aby spróbować zaktualizować te kolumny. Jednak EF w celu uzyskania tych wartości z bazy danych, po zostały wstawione lub zaktualizowane dane. Do tych właściwości w klasie wraz z wyliczenia obliczane, można użyć adnotacji DatabaseGenerated. Inne typy wyliczeniowe to: Brak i tożsamości.

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

Można użyć bazy danych, generowany dla kolumn bajtów lub sygnatura czasowa, gdy kod najpierw generuje bazy danych, w przeciwnym razie należy używać tylko to po wskazaniu istniejących baz danych, ponieważ kod najpierw nie będzie można określić formułę dla kolumny obliczanej.

Już wspomniano powyżej, domyślnie, właściwość klucza, która jest liczbą całkowitą staną się klucza tożsamości w bazie danych. Który będzie taka sama jak ustawienie DatabaseGenerated DatabaseGenerationOption.Identity. Jeśli nie chcesz go jako klucza tożsamości, można ustawić wartości do DatabaseGenerationOption.None.

 

## <a name="index"></a>Indeks

> [!NOTE]
> **EF6.1 począwszy tylko** -atrybutu indeksu została wprowadzona w programie Entity Framework 6.1. Informacje przedstawione w tej sekcji nie ma zastosowania, jeśli używasz starszej wersji.

Można utworzyć indeksu w co najmniej jedną kolumnę, za pomocą **IndexAttribute**. Dodawanie atrybutu do jednej lub więcej właściwości będzie powodować EF utworzyć odpowiedni indeks w bazie danych, podczas tworzenia bazy danych lub tworzenia szkieletu odpowiednich **CreateIndex** wywołuje się, jeśli używasz migracje Code First.

Na przykład, poniższy kod będzie skutkować indeksu tworzona **ocena** kolumny **wpisy** tabeli w bazie danych.

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

Domyślnie, będą miały nazwę nadaną indeks **IX\_&lt;nazwa właściwości&gt;**  (IX\_klasyfikacji w powyższym przykładzie). Mimo że można określić również nazwę indeksu. W poniższym przykładzie określono indeks powinien zostać nazwany **PostRatingIndex**.

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

Indeksy są domyślnie nie jest unikatowa, ale można użyć **IsUnique** o nazwie parametru, aby określić, że indeks powinien być unikatowy. Poniższy przykład przedstawia unikatowego indeksu na **użytkownika**przez nazwę logowania.

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

### <a name="multiple-column-indexes"></a>Indeksy wielu kolumn

Indeksy, obejmujące wiele kolumn są określane przy użyciu tej samej nazwie w wielu adnotacjach indeksu dla danej tabeli. Podczas tworzenia indeksów wielokolumnowych, należy określić kolejność kolumn w indeksie. Na przykład, poniższy kod tworzy indeks wielokolumnowych na **ocena** i **BlogId** o nazwie **IX\_BlogAndRating**. **BlogId** jest w pierwszej kolumnie indeksu i **ocena** drugi.

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a>Relacji atrybutów: InverseProperty i klucza obcego

> [!NOTE]
> Ta strona zawiera informacje o konfigurowaniu relacje w modelu Code First przy użyciu adnotacji danych. Aby uzyskać ogólne informacje dotyczące relacji w EF i uzyskiwania dostępu i manipulowanie danymi za pomocą relacji, zobacz [relacje & właściwości nawigacji](~/ef6/fundamentals/relationships.md). *

Kod Konwencji pierwsze zajmie się najbardziej typowe relacje w modelu, ale istnieją przypadki, gdzie potrzebuje pomocy.

Zmiana nazwy właściwości klucza klasy blogu utworzone problem z jego relacji do wpisu. 

Podczas generowania bazy danych, kod najpierw widzi właściwość BlogId w klasie Post i rozpoznaje je, zgodnie z Konwencją, czy jest on zgodny Nazwa klasy oraz "Id" jako klucza obcego do klasy blogu. Ale nie ma właściwości BlogId w klasie blogu. Rozwiązania dla tego jest utworzenie właściwości nawigacji we wpisie i użyj DataAnnotation obcego, aby najpierw zrozumieć sposób tworzenia relacji między dwoma klasami kodu — przy użyciu właściwości Post.BlogId — oraz jak określić ograniczeń, Baza danych.

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

Ograniczenia w bazie danych przedstawiono relację między InternalBlogs.PrimaryTrackingKey i Posts.BlogId. 

![jj591583_figure09](~/ef6/media/jj591583-figure09.png)

InverseProperty jest używany, jeśli masz wiele relacji między klasami.

W klasie wpis może być do śledzenia określonego autora wpisu w blogu oraz która ją edytowała. Poniżej przedstawiono dwie nowe właściwości nawigacji dla klasy wpisu.

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

Należy również dodać w klasie osoby odwołuje się tych właściwości. Klasa osoba ma właściwości nawigacji Wstecz do wpisu, jeden dla wszystkich wpisów, napisane przez osoby i jeden dla wszystkich wpisów, aktualizowane przez tę osobę.

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

Kod najpierw jest możliwość dopasowania właściwości do dwóch klas samodzielnie. W tabeli bazy danych dla wpisów powinien mieć jeden klucz obcy dla osoby CreatedBy i jeden dla UpdatedBy osoby, ale kod najpierw utworzy cztery będą właściwości klucza obcego: osoba\_identyfikator, osoba\_Id1, CreatedBy\_identyfikator i UpdatedBy\_identyfikatora.

![jj591583_figure10](~/ef6/media/jj591583-figure10.png)

Aby rozwiązać te problemy, można użyć adnotacji InverseProperty, aby określić wyrównanie właściwości.

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

Ponieważ właściwość PostsWritten osobiście wie, to odnosi się do typu wpisu, utworzy relacji z elementem Post.CreatedBy. Podobnie Post.UpdatedBy połączyć PostsUpdated. I kod najpierw nie utworzy ich klucze obce dodatkowych.

![jj591583_figure11](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a>Podsumowanie

DataAnnotations nie tylko umożliwiają opisują weryfikacji po stronie klienta i serwera, w Twoich zajęciach pierwszy kodu, ale również umożliwiają rozszerzanie i nawet rozwiązać założeń, które kod najpierw spowoduje, że Twoje klasy oparte na konwencjach jej informacje. Za pomocą DataAnnotations można nie tylko zachęcić generowanie schematu bazy danych, ale można również mapować Twoich zajęciach pierwszy kodu do wcześniej istniejącej bazy danych.

Gdy są bardzo elastyczne, należy pamiętać, zapewniające DataAnnotations tylko najbardziej najczęściej potrzebne zmiany konfiguracji wprowadzone w Twoich zajęciach pierwszy kod. Aby skonfigurować Twoje klasy dla niektórych przypadków brzegowych, powinien wyglądać mechanizmu alternatywną konfigurację, Code First API Fluent.
