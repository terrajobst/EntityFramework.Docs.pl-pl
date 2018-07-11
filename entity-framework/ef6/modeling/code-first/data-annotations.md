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
# <a name="code-first-data-annotations"></a><span data-ttu-id="a2757-102">Adnotacje danych na pierwszym kodu</span><span class="sxs-lookup"><span data-stu-id="a2757-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="a2757-103">**EF4.1 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="a2757-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="a2757-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="a2757-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="a2757-105">Zawartość na tej stronie są zaczerpnięte z, a artykuł pierwotnie napisane przez Julie Lerman (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="a2757-105">The content on this page is adapted from and article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="a2757-106">Entity Framework Code First pozwala na używanie własnych klas domeny do reprezentowania modelu, który EF opiera się na do wykonywania zapytań, śledzenia i zmienić aktualizacji funkcji.</span><span class="sxs-lookup"><span data-stu-id="a2757-106">Entity Framework Code First allows you to use your own domain classes to represent the model which EF relies on to perform querying, change tracking and updating functions.</span></span> <span data-ttu-id="a2757-107">Kod najpierw wykorzystuje wzorzec programowania, nazywane Konwencji za pośrednictwem konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="a2757-107">Code first leverages a programming pattern referred to as convention over configuration.</span></span> <span data-ttu-id="a2757-108">Oznacza to, że kod najpierw zakłada, że Twoich zajęciach zgodne z konwencjami, używanych przez EF.</span><span class="sxs-lookup"><span data-stu-id="a2757-108">What this means is that code first will assume that your classes follow the conventions that EF uses.</span></span> <span data-ttu-id="a2757-109">W takim przypadku EF będzie można sprawdzić szczegóły go potrzebuje do wykonywania swojej pracy.</span><span class="sxs-lookup"><span data-stu-id="a2757-109">In that case, EF will be able to work out the details it needs to do its job.</span></span> <span data-ttu-id="a2757-110">Jednak jeśli Twoich zajęciach nie wykonuj tych konwencji, masz możliwość dodawania konfiguracje do swoich klas, aby zapewnić EF informacje, których potrzebuje.</span><span class="sxs-lookup"><span data-stu-id="a2757-110">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the information it needs.</span></span>

<span data-ttu-id="a2757-111">Kod daje najpierw dodaj te konfiguracje do swoich klas na dwa sposoby.</span><span class="sxs-lookup"><span data-stu-id="a2757-111">Code first gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="a2757-112">Jeden używa proste atrybuty o nazwie DataAnnotations, a drugi to najpierw przy użyciu kodu jest interfejs Fluent API, który zapewnia sposób, aby opisać konfiguracje obowiązkowo, w kodzie.</span><span class="sxs-lookup"><span data-stu-id="a2757-112">One is using simple attributes called DataAnnotations and the other is using code first’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="a2757-113">Ten artykuł koncentruje się na konfigurowanie Twoich zajęciach — wyróżnianie najczęściej wymagane konfiguracje, za pomocą DataAnnotations (w przestrzeni nazw System.ComponentModel.DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="a2757-113">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="a2757-114">DataAnnotations również są zrozumiałe przez kilka aplikacji .NET, takich jak ASP.NET MVC, która umożliwia tych aplikacji korzystać z tej samej adnotacji dla walidacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="a2757-114">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="a2757-115">Model</span><span class="sxs-lookup"><span data-stu-id="a2757-115">The model</span></span>

<span data-ttu-id="a2757-116">Zademonstruję kodu pierwszy DataAnnotations przy użyciu prostego pary klas: Blog i Post.</span><span class="sxs-lookup"><span data-stu-id="a2757-116">I’ll demonstrate code first DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

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

<span data-ttu-id="a2757-117">Jak są one klasy blogu i wpis wygodnie Konwencją pierwszy kodu i wymagane nie ulepszeń ułatwiające EF pracować z nimi.</span><span class="sxs-lookup"><span data-stu-id="a2757-117">As they are, the Blog and Post classes conveniently follow code first convention and required no tweaks to help EF work with them.</span></span> <span data-ttu-id="a2757-118">Ale można również użyć adnotacje na zapewnienie EF więcej informacji na temat klas i mapować je do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a2757-118">But you can also use the annotations to provide more information to EF about the classes and the database that they map to.</span></span>

 

## <a name="key"></a><span data-ttu-id="a2757-119">Key</span><span class="sxs-lookup"><span data-stu-id="a2757-119">Key</span></span>

<span data-ttu-id="a2757-120">Entity Framework opiera się na każdej jednostki o wartości klucza, która jest używana do śledzenia jednostek.</span><span class="sxs-lookup"><span data-stu-id="a2757-120">Entity Framework relies on every entity having a key value that it uses for tracking entities.</span></span> <span data-ttu-id="a2757-121">Jedną z Konwencji, od których zależy najpierw kod jest sposób jej działanie polega na właściwość, która jest kluczem w każdą z klas pierwszy kodu.</span><span class="sxs-lookup"><span data-stu-id="a2757-121">One of the conventions that code first depends on is how it implies which property is the key in each of the code first classes.</span></span> <span data-ttu-id="a2757-122">Konwencja jest odszukaj właściwość o nazwie "Id" lub taką, która łączy nazwę klasy i "Id", takich jak "BlogId".</span><span class="sxs-lookup"><span data-stu-id="a2757-122">That convention is to look for a property named “Id” or one that combines the class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="a2757-123">Właściwość będą mapowane na kolumny klucza podstawowego w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a2757-123">The property will map to a primary key column in the database.</span></span>

<span data-ttu-id="a2757-124">Klasy blogu i wpis stosują taką Konwencję.</span><span class="sxs-lookup"><span data-stu-id="a2757-124">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="a2757-125">Ale co zrobić, jeśli nie?</span><span class="sxs-lookup"><span data-stu-id="a2757-125">But what if they didn’t?</span></span> <span data-ttu-id="a2757-126">Co zrobić, jeśli użyto nazwy w blogu *PrimaryTrackingKey* w zamian lub nawet *foo*?</span><span class="sxs-lookup"><span data-stu-id="a2757-126">What if Blog used the name *PrimaryTrackingKey* instead or even *foo*?</span></span> <span data-ttu-id="a2757-127">Jeśli kod najpierw nie może znaleźć właściwość, która pasuje do niniejszej Konwencji spowoduje zgłoszenie wyjątku, ze względu na wymagania programu Entity Framework, musi mieć właściwość klucza.</span><span class="sxs-lookup"><span data-stu-id="a2757-127">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="a2757-128">Można użyć klucza adnotacji, aby określić, które właściwości, które ma być używany jako EntityKey.</span><span class="sxs-lookup"><span data-stu-id="a2757-128">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

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

<span data-ttu-id="a2757-129">Jeśli najpierw przy użyciu kodu jest funkcją generowanie bazy danych, tabela blogu będzie miała kolumny klucza podstawowego o nazwie PrimaryTrackingKey, który również jest zdefiniowany jako tożsamość domyślnie.</span><span class="sxs-lookup"><span data-stu-id="a2757-129">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey which is also defined as Identity by default.</span></span>

![jj591583_figure01](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="a2757-131">Klucze złożone</span><span class="sxs-lookup"><span data-stu-id="a2757-131">Composite keys</span></span>

<span data-ttu-id="a2757-132">Entity Framework obsługuje kluczy złożonych — klucze podstawowe, które składają się z więcej niż jednej właściwości.</span><span class="sxs-lookup"><span data-stu-id="a2757-132">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="a2757-133">Na przykład usługi może mieć klasy usługi Passport, którego klucz podstawowy jest kombinacją PassportNumber i IssuingCountry.</span><span class="sxs-lookup"><span data-stu-id="a2757-133">For example, your could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

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

<span data-ttu-id="a2757-134">Gdyby i spróbuj użyć klasy powyżej w modelu platformy EF otrzymamy informacją InvalidOperationExceptions;</span><span class="sxs-lookup"><span data-stu-id="a2757-134">If you were to try and use the above class in your EF model you wuld get an InvalidOperationExceptions stating;</span></span>

<span data-ttu-id="a2757-135">*Nie można określić złożonego podstawowego klucza porządkowanie dla typu "Paszport". Użyj metody HasKey lub ColumnAttribute, aby określić zamówienia złożone kluczy podstawowych.*</span><span class="sxs-lookup"><span data-stu-id="a2757-135">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="a2757-136">W przypadku kluczy złożonych Entity Framework wymaga do definiowania porządku właściwości klucza.</span><span class="sxs-lookup"><span data-stu-id="a2757-136">When you have composite keys, Entity Framework requires you to define an order of the key properties.</span></span> <span data-ttu-id="a2757-137">Można to zrobić za pomocą adnotacji kolumny w celu określania kolejności.</span><span class="sxs-lookup"><span data-stu-id="a2757-137">You can do this using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="a2757-138">Wartość kolejności jest względna (a nie na podstawie indeksu), dzięki czemu można używać dowolnej wartości.</span><span class="sxs-lookup"><span data-stu-id="a2757-138">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="a2757-139">Na przykład 100 do 200 byłoby dopuszczalne zamiast 1 i 2.</span><span class="sxs-lookup"><span data-stu-id="a2757-139">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

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

<span data-ttu-id="a2757-140">W przypadku jednostek za pomocą kluczy złożonych obcego należy określić w tej samej kolumnie, porządkowanie, który był używany dla odpowiednich właściwości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="a2757-140">If you have entities with composite foreign keys then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="a2757-141">Tylko względną kolejność w ramach właściwości klucza obcego muszą być takie same, dokładne wartości, które są przypisane do **kolejność** nie muszą być zgodne.</span><span class="sxs-lookup"><span data-stu-id="a2757-141">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="a2757-142">Na przykład w następującej klasy 3 i 4 może służyć zamiast 1 i 2.</span><span class="sxs-lookup"><span data-stu-id="a2757-142">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

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

## <a name="required"></a><span data-ttu-id="a2757-143">Wymagane</span><span class="sxs-lookup"><span data-stu-id="a2757-143">Required</span></span>

<span data-ttu-id="a2757-144">Wymagane adnotacji informuje EF, czy określona właściwość jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="a2757-144">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="a2757-145">Dodawanie wymaganych do właściwości Title wymusi EF (i MVC), aby upewnić się, że właściwość zawiera dane.</span><span class="sxs-lookup"><span data-stu-id="a2757-145">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="a2757-146">Nie dodatkowych bez zmiany kodu lub języka znaczników w aplikacji, aplikacji MVC przeprowadzi weryfikację po stronie klienta, nawet dynamiczne tworzenie komunikat przy użyciu nazwy właściwości i adnotacji.</span><span class="sxs-lookup"><span data-stu-id="a2757-146">With no additional no code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![jj591583_figure02](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="a2757-148">Wymagany atrybut wpłynie również na wygenerowanej bazy danych, wprowadzając mapowanej właściwości niedopuszczającej.</span><span class="sxs-lookup"><span data-stu-id="a2757-148">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="a2757-149">Należy zauważyć, że pole tytułu została zmieniona na "nie ma wartości null".</span><span class="sxs-lookup"><span data-stu-id="a2757-149">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="a2757-150">W niektórych przypadkach może nie być możliwe dla kolumny w bazie danych, może nie dopuszczać wartości null, nawet jeśli właściwość jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="a2757-150">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="a2757-151">Na przykład, gdy przy użyciu danych strategii TPH dziedziczenia dla wielu typów są przechowywane w jednej tabeli.</span><span class="sxs-lookup"><span data-stu-id="a2757-151">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="a2757-152">Jeśli typ pochodny obejmuje wymagana właściwość kolumny nie można dokonać innych niż null, ponieważ nie wszystkie typy w hierarchii mają ta właściwość.</span><span class="sxs-lookup"><span data-stu-id="a2757-152">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![jj591583_figure03](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="a2757-154">Element MaxLength i MinLength</span><span class="sxs-lookup"><span data-stu-id="a2757-154">MaxLength and MinLength</span></span>

<span data-ttu-id="a2757-155">Atrybuty MaxLength i MinLength umożliwiają określenie dodatkowych właściwości sprawdzania poprawności, tak jak w przypadku wymagany.</span><span class="sxs-lookup"><span data-stu-id="a2757-155">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="a2757-156">Oto BloggerName o wymagania dotyczące długości.</span><span class="sxs-lookup"><span data-stu-id="a2757-156">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="a2757-157">W przykładzie pokazano również sposób łączenia atrybutów.</span><span class="sxs-lookup"><span data-stu-id="a2757-157">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="a2757-158">Adnotacja MaxLength wpłynie na bazie danych przez ustawienie właściwości długości do 10.</span><span class="sxs-lookup"><span data-stu-id="a2757-158">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![jj591583_figure04](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="a2757-160">Adnotacja po stronie klienta w MVC i EF 4.1 po stronie serwera adnotacji zarówno podlegają weryfikacji ponownie dynamicznie tworzenie komunikat o błędzie: "pole BloggerName musi być typu string lub tablicy o maksymalnej długości"10"." Ten komunikat jest nieco długi.</span><span class="sxs-lookup"><span data-stu-id="a2757-160">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="a2757-161">Wiele adnotacje umożliwiają określenie komunikat o błędzie z atrybutem komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="a2757-161">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="a2757-162">Można również określić komunikat o błędzie w wymaganych adnotacji.</span><span class="sxs-lookup"><span data-stu-id="a2757-162">You can also specify ErrorMessage in the Required annotation.</span></span>

![jj591583_figure05](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="a2757-164">NotMapped</span><span class="sxs-lookup"><span data-stu-id="a2757-164">NotMapped</span></span>

<span data-ttu-id="a2757-165">Kod Konwencji pierwsze decyduje o tym, że dla każdej właściwości, który jest obsługiwany typ danych jest reprezentowana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a2757-165">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="a2757-166">Ale to nie jest zawsze w przypadku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a2757-166">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="a2757-167">Na przykład może mieć właściwości w klasie blogów, która tworzy kod, w oparciu o pola tytułu i BloggerName.</span><span class="sxs-lookup"><span data-stu-id="a2757-167">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="a2757-168">Tej właściwości można tworzyć dynamicznie i nie musi być przechowywany.</span><span class="sxs-lookup"><span data-stu-id="a2757-168">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="a2757-169">Możesz oznaczyć wszystkie właściwości, które nie są mapowane do bazy danych z adnotacją NotMapped, np. Ta właściwość BlogCode.</span><span class="sxs-lookup"><span data-stu-id="a2757-169">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

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

 

## <a name="complextype"></a><span data-ttu-id="a2757-170">ComplexType</span><span class="sxs-lookup"><span data-stu-id="a2757-170">ComplexType</span></span>

<span data-ttu-id="a2757-171">Nie jest niczym niezwykłym opisują jednostek domeny przez zestaw klas, a następnie warstwy tych klas do opisania całą jednostkę.</span><span class="sxs-lookup"><span data-stu-id="a2757-171">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="a2757-172">Może na przykład dodać klasę o nazwie BlogDetails do modelu.</span><span class="sxs-lookup"><span data-stu-id="a2757-172">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="a2757-173">Należy zauważyć, że BlogDetails nie ma żadnych typów właściwości klucza.</span><span class="sxs-lookup"><span data-stu-id="a2757-173">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="a2757-174">W przypadku projektowania opartego na domenach BlogDetails nazywa się obiektu wartości.</span><span class="sxs-lookup"><span data-stu-id="a2757-174">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="a2757-175">Entity Framework odnosi się do obiektów wartości jako typy złożone.</span><span class="sxs-lookup"><span data-stu-id="a2757-175">Entity Framework refers to value objects as complex types.</span></span>  <span data-ttu-id="a2757-176">Typy złożone nie mogą być śledzone własnych.</span><span class="sxs-lookup"><span data-stu-id="a2757-176">Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="a2757-177">Jednak jako właściwość w klasie blogu BlogDetails będzie śledzona jako część obiektu blogu.</span><span class="sxs-lookup"><span data-stu-id="a2757-177">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="a2757-178">Aby najpierw umożliwia to rozpoznawanie kodu należy oznaczyć klasę BlogDetails jako ComplexType.</span><span class="sxs-lookup"><span data-stu-id="a2757-178">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="a2757-179">Teraz można dodać właściwość w klasie blogu do reprezentowania BlogDetails na blogu.</span><span class="sxs-lookup"><span data-stu-id="a2757-179">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="a2757-180">W bazie danych w tabeli blogu będzie zawierać wszystkie właściwości blogu, w tym właściwości zawarte w jego właściwość BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="a2757-180">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="a2757-181">Domyślnie każdej z nich jest poprzedzone nazwą typu złożonego BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="a2757-181">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![jj591583_figure06](~/ef6/media/jj591583-figure06.png)

<span data-ttu-id="a2757-183">Inny interesujący Uwaga jest mimo, że właściwość DateCreated została zdefiniowana jako nieprzyjmujące wartości daty/godziny w klasie, pole odpowiedniej bazy danych dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="a2757-183">Another interesting note is that although the DateCreated property was defined as a non-nullable DateTime in the class, the relevant database field is nullable.</span></span> <span data-ttu-id="a2757-184">Należy użyć wymaganych adnotacji, jeśli chcesz mieć wpływ na schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a2757-184">You must use the Required annotation if you wish to affect the database schema.</span></span>

 

## <a name="concurrencycheck"></a><span data-ttu-id="a2757-185">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="a2757-185">ConcurrencyCheck</span></span>

<span data-ttu-id="a2757-186">Adnotacja ConcurrencyCheck pozwala Flaga jedną lub więcej właściwości, które ma być używany dla współbieżności sprawdzanie w bazie danych, gdy użytkownik edytuje lub usunięcie jednostki.</span><span class="sxs-lookup"><span data-stu-id="a2757-186">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="a2757-187">Jeśli masz doświadczenie w pracy z projektantem EF, jest to zgodne z ustawienie właściwości przed na stałe.</span><span class="sxs-lookup"><span data-stu-id="a2757-187">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="a2757-188">Zobaczmy, jak działa ConcurrencyCheck, dodając ją do właściwości BloggerName.</span><span class="sxs-lookup"><span data-stu-id="a2757-188">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="a2757-189">Po wywołaniu funkcji SaveChanges ze względu na adnotacji ConcurrencyCheck pola BloggerName oryginalna wartość tej właściwości będzie używany w aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="a2757-189">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="a2757-190">Polecenie będzie podejmować próby zlokalizowania prawidłowego wiersza, filtrując nie tylko na wartości klucza, ale także w oryginalnej wartości BloggerName.</span><span class="sxs-lookup"><span data-stu-id="a2757-190">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span>  <span data-ttu-id="a2757-191">Poniżej przedstawiono krytycznych części polecenia UPDATE wysyłane do bazy danych, gdzie można zobaczyć, polecenie spowoduje zaktualizowanie wiersza, który ma PrimaryTrackingKey jest 1 i BloggerName "Julie", który podczas oryginalnej wartości w tym blogu został pobrany z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a2757-191">Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="a2757-192">Jeśli ktoś zmienił nazwę blogger blogu w tym samym czasie, ta aktualizacja zakończy się niepowodzeniem, a otrzymasz DbUpdateConcurrencyException, który należy do obsługi.</span><span class="sxs-lookup"><span data-stu-id="a2757-192">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="a2757-193">Znacznik czasu:</span><span class="sxs-lookup"><span data-stu-id="a2757-193">TimeStamp</span></span>

<span data-ttu-id="a2757-194">Jest to bardziej powszechne, aby używać pól rowversion lub sygnatura czasowa sprawdzania współbieżności.</span><span class="sxs-lookup"><span data-stu-id="a2757-194">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="a2757-195">Jednak zamiast przy użyciu adnotacji ConcurrencyCheck, można użyć bardziej szczegółowe adnotacji sygnatury czasowej, tak długo, jak typ właściwości to tablica bajtów.</span><span class="sxs-lookup"><span data-stu-id="a2757-195">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="a2757-196">Kod najpierw traktują właściwości sygnatury czasowej niż jako właściwości ConcurrencyCheck, ale jej będzie również upewnij się, że pole bazy danych, która najpierw generuje kod innych niż null.</span><span class="sxs-lookup"><span data-stu-id="a2757-196">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="a2757-197">Może mieć tylko jedną właściwość sygnatury czasowej w danej klasy.</span><span class="sxs-lookup"><span data-stu-id="a2757-197">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="a2757-198">Dodawanie następującej właściwości do klasy Blog:</span><span class="sxs-lookup"><span data-stu-id="a2757-198">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="a2757-199">wyniki w kodzie, najpierw tworząc kolumnę sygnatur czasowych nie dopuszcza wartości null w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a2757-199">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![jj591583_figure07](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="a2757-201">Tabel i kolumn</span><span class="sxs-lookup"><span data-stu-id="a2757-201">Table and Column</span></span>

<span data-ttu-id="a2757-202">Pozwalasz Code First utworzyć bazę danych, można zmienić nazwy tabel i kolumn, które, która zostanie utworzona.</span><span class="sxs-lookup"><span data-stu-id="a2757-202">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="a2757-203">Umożliwia także Code First przy użyciu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a2757-203">You can also use Code First with an existing database.</span></span> <span data-ttu-id="a2757-204">Ale nie zawsze jest przypadek, że nazwy klas i właściwości w domenie pasują do nazw tabel i kolumn w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a2757-204">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="a2757-205">Moje klasy ma nazwę blogu i zgodnie z Konwencją kod najpierw przyjęto założenie, że to będzie zmapowana do tabeli o nazwie blogów.</span><span class="sxs-lookup"><span data-stu-id="a2757-205">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="a2757-206">Jeśli tak nie jest atrybutem tabeli można określić nazwę tabeli.</span><span class="sxs-lookup"><span data-stu-id="a2757-206">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="a2757-207">Tutaj na przykład adnotacja jest określającą, czy nazwa tabeli jest InternalBlogs.</span><span class="sxs-lookup"><span data-stu-id="a2757-207">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="a2757-208">Adnotacja kolumny jest więcej doświadczenie podczas określania atrybutów zamapowanych kolumn.</span><span class="sxs-lookup"><span data-stu-id="a2757-208">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="a2757-209">Można zastrzec nazwę, typ danych lub nawet kolejność, w której kolumna pojawia się w tabeli.</span><span class="sxs-lookup"><span data-stu-id="a2757-209">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="a2757-210">Oto przykład atrybut kolumny.</span><span class="sxs-lookup"><span data-stu-id="a2757-210">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="a2757-211">Nie należy mylić w kolumnie Nazwa typu atrybutu o DataAnnotation typu danych.</span><span class="sxs-lookup"><span data-stu-id="a2757-211">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="a2757-212">Typ danych jest adnotacja używane dla interfejsu użytkownika i jest ignorowana przez rozwiązanie Code First.</span><span class="sxs-lookup"><span data-stu-id="a2757-212">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="a2757-213">Oto tabeli po jest zostały ponownie wygenerowane.</span><span class="sxs-lookup"><span data-stu-id="a2757-213">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="a2757-214">Nazwa tabeli został zmieniony na InternalBlogs i opis kolumny z typu złożonego jest teraz BlogDescription.</span><span class="sxs-lookup"><span data-stu-id="a2757-214">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="a2757-215">Ponieważ nazwa została określona w adnotacji, kod najpierw nie będzie używać konwencji początkowych nazwa kolumny o nazwie typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="a2757-215">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![jj591583_figure08](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="a2757-217">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="a2757-217">DatabaseGenerated</span></span>

<span data-ttu-id="a2757-218">Funkcje bazy danych ważna jest możliwość mają być obliczane właściwości.</span><span class="sxs-lookup"><span data-stu-id="a2757-218">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="a2757-219">Jeśli masz mapowania klas Code First tabel zawierających kolumny obliczane, nie ma programu Entity Framework, aby spróbować zaktualizować te kolumny.</span><span class="sxs-lookup"><span data-stu-id="a2757-219">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="a2757-220">Jednak EF w celu uzyskania tych wartości z bazy danych, po zostały wstawione lub zaktualizowane dane.</span><span class="sxs-lookup"><span data-stu-id="a2757-220">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="a2757-221">Do tych właściwości w klasie wraz z wyliczenia obliczane, można użyć adnotacji DatabaseGenerated.</span><span class="sxs-lookup"><span data-stu-id="a2757-221">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="a2757-222">Inne typy wyliczeniowe to: Brak i tożsamości.</span><span class="sxs-lookup"><span data-stu-id="a2757-222">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="a2757-223">Można użyć bazy danych, generowany dla kolumn bajtów lub sygnatura czasowa, gdy kod najpierw generuje bazy danych, w przeciwnym razie należy używać tylko to po wskazaniu istniejących baz danych, ponieważ kod najpierw nie będzie można określić formułę dla kolumny obliczanej.</span><span class="sxs-lookup"><span data-stu-id="a2757-223">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="a2757-224">Już wspomniano powyżej, domyślnie, właściwość klucza, która jest liczbą całkowitą staną się klucza tożsamości w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a2757-224">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="a2757-225">Który będzie taka sama jak ustawienie DatabaseGenerated DatabaseGenerationOption.Identity.</span><span class="sxs-lookup"><span data-stu-id="a2757-225">That would be the same as setting DatabaseGenerated to DatabaseGenerationOption.Identity.</span></span> <span data-ttu-id="a2757-226">Jeśli nie chcesz go jako klucza tożsamości, można ustawić wartości do DatabaseGenerationOption.None.</span><span class="sxs-lookup"><span data-stu-id="a2757-226">If you do not want it to be an identity key, you can set the value to DatabaseGenerationOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="a2757-227">Indeks</span><span class="sxs-lookup"><span data-stu-id="a2757-227">Index</span></span>

> [!NOTE]
> <span data-ttu-id="a2757-228">**EF6.1 począwszy tylko** -atrybutu indeksu została wprowadzona w programie Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="a2757-228">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="a2757-229">Informacje przedstawione w tej sekcji nie ma zastosowania, jeśli używasz starszej wersji.</span><span class="sxs-lookup"><span data-stu-id="a2757-229">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="a2757-230">Można utworzyć indeksu w co najmniej jedną kolumnę, za pomocą **IndexAttribute**.</span><span class="sxs-lookup"><span data-stu-id="a2757-230">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="a2757-231">Dodawanie atrybutu do jednej lub więcej właściwości będzie powodować EF utworzyć odpowiedni indeks w bazie danych, podczas tworzenia bazy danych lub tworzenia szkieletu odpowiednich **CreateIndex** wywołuje się, jeśli używasz migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="a2757-231">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="a2757-232">Na przykład, poniższy kod będzie skutkować indeksu tworzona **ocena** kolumny **wpisy** tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a2757-232">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

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

<span data-ttu-id="a2757-233">Domyślnie, będą miały nazwę nadaną indeks **IX\_&lt;nazwa właściwości&gt;**  (IX\_klasyfikacji w powyższym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="a2757-233">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="a2757-234">Mimo że można określić również nazwę indeksu.</span><span class="sxs-lookup"><span data-stu-id="a2757-234">You can also specify a name for the index though.</span></span> <span data-ttu-id="a2757-235">W poniższym przykładzie określono indeks powinien zostać nazwany **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="a2757-235">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="a2757-236">Indeksy są domyślnie nie jest unikatowa, ale można użyć **IsUnique** o nazwie parametru, aby określić, że indeks powinien być unikatowy.</span><span class="sxs-lookup"><span data-stu-id="a2757-236">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="a2757-237">Poniższy przykład przedstawia unikatowego indeksu na **użytkownika**przez nazwę logowania.</span><span class="sxs-lookup"><span data-stu-id="a2757-237">The following example introduces a unique index on a **User**'s login name.</span></span>

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

### <a name="multiple-column-indexes"></a><span data-ttu-id="a2757-238">Indeksy wielu kolumn</span><span class="sxs-lookup"><span data-stu-id="a2757-238">Multiple-Column Indexes</span></span>

<span data-ttu-id="a2757-239">Indeksy, obejmujące wiele kolumn są określane przy użyciu tej samej nazwie w wielu adnotacjach indeksu dla danej tabeli.</span><span class="sxs-lookup"><span data-stu-id="a2757-239">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="a2757-240">Podczas tworzenia indeksów wielokolumnowych, należy określić kolejność kolumn w indeksie.</span><span class="sxs-lookup"><span data-stu-id="a2757-240">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="a2757-241">Na przykład, poniższy kod tworzy indeks wielokolumnowych na **ocena** i **BlogId** o nazwie **IX\_BlogAndRating**.</span><span class="sxs-lookup"><span data-stu-id="a2757-241">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogAndRating**.</span></span> <span data-ttu-id="a2757-242">**BlogId** jest w pierwszej kolumnie indeksu i **ocena** drugi.</span><span class="sxs-lookup"><span data-stu-id="a2757-242">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="a2757-243">Relacji atrybutów: InverseProperty i klucza obcego</span><span class="sxs-lookup"><span data-stu-id="a2757-243">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="a2757-244">Ta strona zawiera informacje o konfigurowaniu relacje w modelu Code First przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="a2757-244">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="a2757-245">Aby uzyskać ogólne informacje dotyczące relacji w EF i uzyskiwania dostępu i manipulowanie danymi za pomocą relacji, zobacz [relacje & właściwości nawigacji](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="a2757-245">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="a2757-246">Kod Konwencji pierwsze zajmie się najbardziej typowe relacje w modelu, ale istnieją przypadki, gdzie potrzebuje pomocy.</span><span class="sxs-lookup"><span data-stu-id="a2757-246">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="a2757-247">Zmiana nazwy właściwości klucza klasy blogu utworzone problem z jego relacji do wpisu.</span><span class="sxs-lookup"><span data-stu-id="a2757-247">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="a2757-248">Podczas generowania bazy danych, kod najpierw widzi właściwość BlogId w klasie Post i rozpoznaje je, zgodnie z Konwencją, czy jest on zgodny Nazwa klasy oraz "Id" jako klucza obcego do klasy blogu.</span><span class="sxs-lookup"><span data-stu-id="a2757-248">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="a2757-249">Ale nie ma właściwości BlogId w klasie blogu.</span><span class="sxs-lookup"><span data-stu-id="a2757-249">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="a2757-250">Rozwiązania dla tego jest utworzenie właściwości nawigacji we wpisie i użyj DataAnnotation obcego, aby najpierw zrozumieć sposób tworzenia relacji między dwoma klasami kodu — przy użyciu właściwości Post.BlogId — oraz jak określić ograniczeń, Baza danych.</span><span class="sxs-lookup"><span data-stu-id="a2757-250">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

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

<span data-ttu-id="a2757-251">Ograniczenia w bazie danych przedstawiono relację między InternalBlogs.PrimaryTrackingKey i Posts.BlogId.</span><span class="sxs-lookup"><span data-stu-id="a2757-251">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![jj591583_figure09](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="a2757-253">InverseProperty jest używany, jeśli masz wiele relacji między klasami.</span><span class="sxs-lookup"><span data-stu-id="a2757-253">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="a2757-254">W klasie wpis może być do śledzenia określonego autora wpisu w blogu oraz która ją edytowała.</span><span class="sxs-lookup"><span data-stu-id="a2757-254">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="a2757-255">Poniżej przedstawiono dwie nowe właściwości nawigacji dla klasy wpisu.</span><span class="sxs-lookup"><span data-stu-id="a2757-255">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="a2757-256">Należy również dodać w klasie osoby odwołuje się tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="a2757-256">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="a2757-257">Klasa osoba ma właściwości nawigacji Wstecz do wpisu, jeden dla wszystkich wpisów, napisane przez osoby i jeden dla wszystkich wpisów, aktualizowane przez tę osobę.</span><span class="sxs-lookup"><span data-stu-id="a2757-257">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="a2757-258">Kod najpierw jest możliwość dopasowania właściwości do dwóch klas samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="a2757-258">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="a2757-259">W tabeli bazy danych dla wpisów powinien mieć jeden klucz obcy dla osoby CreatedBy i jeden dla UpdatedBy osoby, ale kod najpierw utworzy cztery będą właściwości klucza obcego: osoba\_identyfikator, osoba\_Id1, CreatedBy\_identyfikator i UpdatedBy\_identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="a2757-259">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four will foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![jj591583_figure10](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="a2757-261">Aby rozwiązać te problemy, można użyć adnotacji InverseProperty, aby określić wyrównanie właściwości.</span><span class="sxs-lookup"><span data-stu-id="a2757-261">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="a2757-262">Ponieważ właściwość PostsWritten osobiście wie, to odnosi się do typu wpisu, utworzy relacji z elementem Post.CreatedBy.</span><span class="sxs-lookup"><span data-stu-id="a2757-262">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="a2757-263">Podobnie Post.UpdatedBy połączyć PostsUpdated.</span><span class="sxs-lookup"><span data-stu-id="a2757-263">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="a2757-264">I kod najpierw nie utworzy ich klucze obce dodatkowych.</span><span class="sxs-lookup"><span data-stu-id="a2757-264">And code first will not create the extra foreign keys.</span></span>

![jj591583_figure11](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="a2757-266">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a2757-266">Summary</span></span>

<span data-ttu-id="a2757-267">DataAnnotations nie tylko umożliwiają opisują weryfikacji po stronie klienta i serwera, w Twoich zajęciach pierwszy kodu, ale również umożliwiają rozszerzanie i nawet rozwiązać założeń, które kod najpierw spowoduje, że Twoje klasy oparte na konwencjach jej informacje.</span><span class="sxs-lookup"><span data-stu-id="a2757-267">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="a2757-268">Za pomocą DataAnnotations można nie tylko zachęcić generowanie schematu bazy danych, ale można również mapować Twoich zajęciach pierwszy kodu do wcześniej istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a2757-268">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="a2757-269">Gdy są bardzo elastyczne, należy pamiętać, zapewniające DataAnnotations tylko najbardziej najczęściej potrzebne zmiany konfiguracji wprowadzone w Twoich zajęciach pierwszy kod.</span><span class="sxs-lookup"><span data-stu-id="a2757-269">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="a2757-270">Aby skonfigurować Twoje klasy dla niektórych przypadków brzegowych, powinien wyglądać mechanizmu alternatywną konfigurację, Code First API Fluent.</span><span class="sxs-lookup"><span data-stu-id="a2757-270">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
