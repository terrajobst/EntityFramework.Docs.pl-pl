---
title: Interfejs API Fluent — Konfigurowanie i mapowanie właściwości i typów — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 7371cc99142ccf8fc6bea237d7d58d1e67fcecec
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419067"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="9f66c-102">Interfejs API Fluent — Konfigurowanie i mapowanie właściwości i typów</span><span class="sxs-lookup"><span data-stu-id="9f66c-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="9f66c-103">Podczas pracy z Entity Framework Code First domyślnym zachowaniem jest mapowanie klas POCO na tabele przy użyciu zestawu Konwencji rozszerzania do EF.</span><span class="sxs-lookup"><span data-stu-id="9f66c-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="9f66c-104">Czasami jednak nie ma potrzeby stosowania tych konwencji lub nie należy ich stosować do zamapowania jednostek na coś innego niż te, które są podyktowane przez konwencje.</span><span class="sxs-lookup"><span data-stu-id="9f66c-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="9f66c-105">Istnieją dwa podstawowe sposoby konfigurowania EF do używania czegoś innego niż konwencji, czyli [adnotacji](~/ef6/modeling/code-first/data-annotations.md) lub interfejsu API Fluent systemu szyfrowania plików.</span><span class="sxs-lookup"><span data-stu-id="9f66c-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="9f66c-106">Adnotacje obejmują tylko podzestaw funkcji interfejsu API Fluent, więc istnieją scenariusze mapowania, których nie można osiągnąć przy użyciu adnotacji.</span><span class="sxs-lookup"><span data-stu-id="9f66c-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="9f66c-107">Ten artykuł został zaprojektowany z założeniami, aby zademonstrować, jak używać interfejsu API Fluent do konfigurowania właściwości.</span><span class="sxs-lookup"><span data-stu-id="9f66c-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="9f66c-108">Najczęściej dostępnym interfejsem API Fluent kodu jest zastąpienie metody [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) w pochodnym [kontekście DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="9f66c-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="9f66c-109">Poniższe przykłady zaprojektowano w celu pokazania, jak wykonywać różne zadania za pomocą interfejsu API Fluent oraz jak można skopiować kod i dostosować go do własnych potrzeb modelu, jeśli chcesz zobaczyć model, którego można używać z usługą AS, na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="9f66c-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="9f66c-110">Ustawienia całego modelu</span><span class="sxs-lookup"><span data-stu-id="9f66c-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="9f66c-111">Domyślny schemat (EF6 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="9f66c-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="9f66c-112">Począwszy od EF6 można użyć metody HasDefaultSchema na DbModelBuilder, aby określić schemat bazy danych, który ma być używany dla wszystkich tabel, procedury składowane itd. To ustawienie domyślne zostanie przesłonięte dla wszystkich obiektów jawnie skonfigurowanych dla innego schematu.</span><span class="sxs-lookup"><span data-stu-id="9f66c-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="9f66c-113">Konwencje niestandardowe (EF6 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="9f66c-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="9f66c-114">Począwszy od EF6, można utworzyć własne konwencje, aby uzupełnić te zawarte w Code First.</span><span class="sxs-lookup"><span data-stu-id="9f66c-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="9f66c-115">Aby uzyskać więcej informacji, zobacz [konwencje Code First niestandardowych](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="9f66c-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="9f66c-116">Odwzorowanie właściwości</span><span class="sxs-lookup"><span data-stu-id="9f66c-116">Property Mapping</span></span>  

<span data-ttu-id="9f66c-117">Metoda [Właściwości](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) służy do konfigurowania atrybutów dla każdej właściwości należącej do jednostki lub typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="9f66c-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="9f66c-118">Metoda Property służy do uzyskania obiektu konfiguracji dla danej właściwości.</span><span class="sxs-lookup"><span data-stu-id="9f66c-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="9f66c-119">Opcje obiektu konfiguracji są specyficzne dla konfigurowanego typu; Isunicode jest dostępny tylko dla właściwości ciągu na przykład.</span><span class="sxs-lookup"><span data-stu-id="9f66c-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="9f66c-120">Konfigurowanie klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="9f66c-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="9f66c-121">Entity Framework Konwencji dla kluczy podstawowych jest:</span><span class="sxs-lookup"><span data-stu-id="9f66c-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="9f66c-122">Klasa definiuje właściwość, której nazwa to "ID" lub "ID"</span><span class="sxs-lookup"><span data-stu-id="9f66c-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="9f66c-123">lub nazwa klasy, po której następuje wartość "ID" lub "ID"</span><span class="sxs-lookup"><span data-stu-id="9f66c-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="9f66c-124">Aby jawnie ustawić właściwość jako klucz podstawowy, można użyć metody Haskey —.</span><span class="sxs-lookup"><span data-stu-id="9f66c-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="9f66c-125">W poniższym przykładzie metoda Haskey — jest używana do konfigurowania klucza podstawowego InstructorID dla typu OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="9f66c-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="9f66c-126">Konfigurowanie złożonego klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="9f66c-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="9f66c-127">Poniższy przykład konfiguruje właściwości DepartmentID i Name w postaci złożonego klucza podstawowego typu działu.</span><span class="sxs-lookup"><span data-stu-id="9f66c-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="9f66c-128">Wyłączanie tożsamości dla numerycznych kluczy podstawowych</span><span class="sxs-lookup"><span data-stu-id="9f66c-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="9f66c-129">Poniższy przykład ustawia właściwość DepartmentID na system. ComponentModel. DataAnnotations. DatabaseGeneratedOption. None, aby wskazać, że wartość nie zostanie wygenerowana przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="9f66c-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="9f66c-130">Określanie maksymalnej długości właściwości</span><span class="sxs-lookup"><span data-stu-id="9f66c-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="9f66c-131">W poniższym przykładzie właściwość Name nie może być dłuższa niż 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="9f66c-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="9f66c-132">Jeśli wartość jest dłuższa niż 50 znaków, zostanie wyświetlony wyjątek [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) .</span><span class="sxs-lookup"><span data-stu-id="9f66c-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="9f66c-133">Jeśli Code First tworzy bazę danych z tego modelu, ustawi również maksymalną długość kolumny Nazwa na 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="9f66c-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="9f66c-134">Konfigurowanie właściwości, która ma być wymagana</span><span class="sxs-lookup"><span data-stu-id="9f66c-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="9f66c-135">W poniższym przykładzie właściwość Name jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="9f66c-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="9f66c-136">Jeśli nazwa nie zostanie określona, zostanie wyświetlony wyjątek DbEntityValidationException.</span><span class="sxs-lookup"><span data-stu-id="9f66c-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="9f66c-137">Jeśli Code First tworzy bazę danych z tego modelu, kolumna użyta do przechowania tej właściwości zazwyczaj nie dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="9f66c-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="9f66c-138">W niektórych przypadkach może nie być możliwe, aby kolumna w bazie danych nie mogła dopuszczać wartości null, mimo że właściwość jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="9f66c-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="9f66c-139">Na przykład w przypadku korzystania z danych strategii dziedziczenia TPH dla wielu typów jest przechowywany w jednej tabeli.</span><span class="sxs-lookup"><span data-stu-id="9f66c-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="9f66c-140">Jeśli typ pochodny zawiera wymaganą właściwość, nie można wprowadzić wartości null w kolumnie, ponieważ nie wszystkie typy w hierarchii będą mieć tę właściwość.</span><span class="sxs-lookup"><span data-stu-id="9f66c-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="9f66c-141">Konfigurowanie indeksu dla jednej lub wielu właściwości</span><span class="sxs-lookup"><span data-stu-id="9f66c-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="9f66c-142">**Dr 6.1 tylko** -atrybut indeksu został wprowadzony w Entity Framework 6,1.</span><span class="sxs-lookup"><span data-stu-id="9f66c-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="9f66c-143">Jeśli używasz wcześniejszej wersji, informacje przedstawione w tej sekcji nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="9f66c-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="9f66c-144">Tworzenie indeksów nie jest natywnie obsługiwane przez interfejs API Fluent, ale można korzystać z pomocy technicznej dla elementu **indexattribute** za pośrednictwem interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="9f66c-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="9f66c-145">Atrybuty indeksu są przetwarzane przez dołączenie do modelu adnotacji modelu, która następnie jest później przekształcana w indeks w bazie danych w potoku.</span><span class="sxs-lookup"><span data-stu-id="9f66c-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="9f66c-146">Można ręcznie dodać te same adnotacje przy użyciu interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="9f66c-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="9f66c-147">Najprostszym sposobem jest utworzenie wystąpienia **indexattribute** , które zawiera wszystkie ustawienia dla nowego indeksu.</span><span class="sxs-lookup"><span data-stu-id="9f66c-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="9f66c-148">Następnie można utworzyć wystąpienie klasy **IndexAnnotation** , która jest typem określonym przez EF, który przekonwertuje ustawienia **indexattribute** na adnotację modelu, która może być przechowywana w modelu EF.</span><span class="sxs-lookup"><span data-stu-id="9f66c-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="9f66c-149">Można je następnie przesłać do metody **HasColumnAnnotation** w interfejsie API Fluent, określając **indeks** nazwy adnotacji.</span><span class="sxs-lookup"><span data-stu-id="9f66c-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="9f66c-150">Aby zapoznać się z pełną listą ustawień dostępnych w **indeksie indexattribute**, zapoznaj się z sekcją *indeks* w obszarze [Code First adnotacje danych](~/ef6/modeling/code-first/data-annotations.md).</span><span class="sxs-lookup"><span data-stu-id="9f66c-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="9f66c-151">Dotyczy to również dostosowywania nazwy indeksu, tworzenia unikatowych indeksów i tworzenia indeksów obejmujących wiele kolumn.</span><span class="sxs-lookup"><span data-stu-id="9f66c-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="9f66c-152">Można określić wiele adnotacji indeksów dla pojedynczej właściwości, przekazując tablicę **indexattribute** do konstruktora **IndexAnnotation**.</span><span class="sxs-lookup"><span data-stu-id="9f66c-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation(
        "Index",  
        new IndexAnnotation(new[]
            {
                new IndexAttribute("Index1"),
                new IndexAttribute("Index2") { IsUnique = true }
            })));
```  

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="9f66c-153">Określanie, aby nie mapować właściwości CLR do kolumny w bazie danych</span><span class="sxs-lookup"><span data-stu-id="9f66c-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="9f66c-154">Poniższy przykład pokazuje, jak określić, że właściwość typu CLR nie jest zamapowana na kolumnę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9f66c-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="9f66c-155">Mapowanie właściwości CLR do określonej kolumny w bazie danych</span><span class="sxs-lookup"><span data-stu-id="9f66c-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="9f66c-156">Poniższy przykład mapuje nazwę właściwości CLR do kolumny bazy danych Departmentname.</span><span class="sxs-lookup"><span data-stu-id="9f66c-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="9f66c-157">Zmiana nazwy klucza obcego, który nie jest zdefiniowany w modelu</span><span class="sxs-lookup"><span data-stu-id="9f66c-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="9f66c-158">Jeśli zdecydujesz się nie definiować klucza obcego w typie CLR, ale chcesz określić nazwę, którą powinna zawierać w bazie danych, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="9f66c-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="9f66c-159">Konfigurowanie, czy właściwość String obsługuje zawartość Unicode</span><span class="sxs-lookup"><span data-stu-id="9f66c-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="9f66c-160">Domyślnie ciągi są w formacie Unicode (nvarchar w SQL Server).</span><span class="sxs-lookup"><span data-stu-id="9f66c-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="9f66c-161">Można użyć metody isunicode, aby określić, że ciąg powinien być typu varchar.</span><span class="sxs-lookup"><span data-stu-id="9f66c-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="9f66c-162">Konfigurowanie typu danych kolumny bazy danych</span><span class="sxs-lookup"><span data-stu-id="9f66c-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="9f66c-163">Metoda [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) umożliwia mapowanie do różnych reprezentacji tego samego typu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="9f66c-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="9f66c-164">Użycie tej metody nie pozwala na wykonywanie konwersji danych w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="9f66c-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="9f66c-165">Należy pamiętać, że funkcja isunicode jest preferowanym sposobem ustawiania kolumn na varchar, ponieważ jest to baza danych niezależny od.</span><span class="sxs-lookup"><span data-stu-id="9f66c-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="9f66c-166">Konfigurowanie właściwości typu złożonego</span><span class="sxs-lookup"><span data-stu-id="9f66c-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="9f66c-167">Istnieją dwa sposoby konfigurowania właściwości skalarnych w typie złożonym.</span><span class="sxs-lookup"><span data-stu-id="9f66c-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="9f66c-168">Możesz wywołać właściwość w ComplexTypeConfiguration.</span><span class="sxs-lookup"><span data-stu-id="9f66c-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="9f66c-169">Można również użyć notacji kropka, aby uzyskać dostęp do właściwości typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="9f66c-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="9f66c-170">Konfigurowanie właściwości, która ma być używana jako optymistyczny Token współbieżności</span><span class="sxs-lookup"><span data-stu-id="9f66c-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="9f66c-171">Aby określić, że właściwość w jednostce reprezentuje Token współbieżności, można użyć atrybutu ConcurrencyCheck lub metody IsConcurrencyToken.</span><span class="sxs-lookup"><span data-stu-id="9f66c-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="9f66c-172">Można również użyć metody IsRowVersion, aby skonfigurować właściwość jako wersję wiersza w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9f66c-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="9f66c-173">Ustawienie właściwości jako wersji wiersza automatycznie konfiguruje ją jako optymistyczny Token współbieżności.</span><span class="sxs-lookup"><span data-stu-id="9f66c-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="9f66c-174">Mapowanie typu</span><span class="sxs-lookup"><span data-stu-id="9f66c-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="9f66c-175">Określanie, że Klasa jest typu złożonego</span><span class="sxs-lookup"><span data-stu-id="9f66c-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="9f66c-176">Zgodnie z Konwencją typ, który nie ma określonego klucza podstawowego, jest traktowany jako typ złożony.</span><span class="sxs-lookup"><span data-stu-id="9f66c-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="9f66c-177">Istnieją scenariusze, w których Code First nie wykrywa typu złożonego (na przykład jeśli masz właściwość o nazwie ID, ale nie ma znaczenia, że jest to klucz podstawowy).</span><span class="sxs-lookup"><span data-stu-id="9f66c-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="9f66c-178">W takich przypadkach można użyć interfejsu API Fluent, aby jawnie określić, że typ jest typem złożonym.</span><span class="sxs-lookup"><span data-stu-id="9f66c-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="9f66c-179">Określanie, aby nie mapować typu jednostki CLR na tabelę w bazie danych</span><span class="sxs-lookup"><span data-stu-id="9f66c-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="9f66c-180">Poniższy przykład pokazuje, jak wykluczyć typ CLR z mapowania do tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9f66c-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="9f66c-181">Mapowanie typu jednostki na określoną tabelę w bazie danych</span><span class="sxs-lookup"><span data-stu-id="9f66c-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="9f66c-182">Wszystkie właściwości działu zostaną zamapowane na kolumny w tabeli o nazwie dział t_.</span><span class="sxs-lookup"><span data-stu-id="9f66c-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="9f66c-183">Można również określić nazwę schematu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="9f66c-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="9f66c-184">Mapowanie dziedziczenia tabeli na hierarchię (TPH)</span><span class="sxs-lookup"><span data-stu-id="9f66c-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="9f66c-185">W scenariuszu mapowania TPH wszystkie typy w hierarchii dziedziczenia są mapowane na pojedynczą tabelę.</span><span class="sxs-lookup"><span data-stu-id="9f66c-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="9f66c-186">Kolumna rozróżniacza służy do identyfikowania typu każdego wiersza.</span><span class="sxs-lookup"><span data-stu-id="9f66c-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="9f66c-187">Podczas tworzenia modelu przy użyciu Code First TPH jest domyślną strategią dla typów, które uczestniczą w hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="9f66c-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="9f66c-188">Domyślnie kolumna rozróżniacz jest dodawana do tabeli o nazwie "rozróżniacz", a nazwa typu CLR każdego typu w hierarchii jest używana dla wartości rozróżniacza.</span><span class="sxs-lookup"><span data-stu-id="9f66c-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="9f66c-189">Domyślne zachowanie można zmienić za pomocą interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="9f66c-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="9f66c-190">Mapowanie dziedziczenia według typu tabeli (TPT)</span><span class="sxs-lookup"><span data-stu-id="9f66c-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="9f66c-191">W scenariuszu mapowania TPT wszystkie typy są mapowane na poszczególne tabele.</span><span class="sxs-lookup"><span data-stu-id="9f66c-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="9f66c-192">Właściwości, które należą wyłącznie do typu podstawowego lub typu pochodnego, są przechowywane w tabeli, która jest mapowana na ten typ.</span><span class="sxs-lookup"><span data-stu-id="9f66c-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="9f66c-193">Tabele mapowane na typy pochodne również przechowują klucz obcy, który łączy tabelę pochodną z tabelą bazową.</span><span class="sxs-lookup"><span data-stu-id="9f66c-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="9f66c-194">Mapowanie dziedziczenia klasy opartej na tabeli (TPC)</span><span class="sxs-lookup"><span data-stu-id="9f66c-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="9f66c-195">W scenariuszu mapowania TPC wszystkie typy nieabstrakcyjne w hierarchii są mapowane do poszczególnych tabel.</span><span class="sxs-lookup"><span data-stu-id="9f66c-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="9f66c-196">Tabele mapowane na klasy pochodne nie mają relacji z tabelą, która jest mapowana na klasę bazową w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9f66c-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="9f66c-197">Wszystkie właściwości klasy, łącznie z dziedziczonymi właściwościami, są mapowane na kolumny odpowiadającej tabeli.</span><span class="sxs-lookup"><span data-stu-id="9f66c-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="9f66c-198">Wywołaj metodę MapInheritedProperties, aby skonfigurować każdy typ pochodny.</span><span class="sxs-lookup"><span data-stu-id="9f66c-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="9f66c-199">MapInheritedProperties ponownie mapuje wszystkie właściwości, które były dziedziczone z klasy bazowej na nowe kolumny w tabeli dla klasy pochodnej.</span><span class="sxs-lookup"><span data-stu-id="9f66c-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="9f66c-200">Należy zauważyć, że ponieważ tabele uczestniczące w hierarchii dziedziczenia TPC nie współdzielą klucza podstawowego, podczas wstawiania w tabelach, które są mapowane na podklasy, nie są dostępne klucze jednostkowe, jeśli masz wartości wygenerowane przez bazę danych z tym samym inicjatorem tożsamości.</span><span class="sxs-lookup"><span data-stu-id="9f66c-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="9f66c-201">Aby rozwiązać ten problem, możesz określić inną początkową wartość inicjatora dla każdej tabeli lub wyłączyć tożsamość we właściwości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="9f66c-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="9f66c-202">Tożsamość jest wartością domyślną dla właściwości klucza Integer podczas pracy z Code First.</span><span class="sxs-lookup"><span data-stu-id="9f66c-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .Property(c => c.CourseID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);

modelBuilder.Entity<OnsiteCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnsiteCourse");
});

modelBuilder.Entity<OnlineCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnlineCourse");
});
```  

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="9f66c-203">Mapowanie właściwości typu jednostki na wiele tabel w bazie danych (podział jednostki)</span><span class="sxs-lookup"><span data-stu-id="9f66c-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="9f66c-204">Dzielenie jednostek umożliwia rozproszenie właściwości typu jednostki między wieloma tabelami.</span><span class="sxs-lookup"><span data-stu-id="9f66c-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="9f66c-205">W poniższym przykładzie jednostka działu jest dzielona na dwie tabele: Department i DepartmentDetails.</span><span class="sxs-lookup"><span data-stu-id="9f66c-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="9f66c-206">Dzielenie jednostek używa wielu wywołań metody map do mapowania podzestawu właściwości do konkretnej tabeli.</span><span class="sxs-lookup"><span data-stu-id="9f66c-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Name });
        m.ToTable("Department");
    })
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Administrator, t.StartDate, t.Budget });
        m.ToTable("DepartmentDetails");
    });
```  

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="9f66c-207">Mapowanie wielu typów jednostek na jedną tabelę w bazie danych (podział tabeli)</span><span class="sxs-lookup"><span data-stu-id="9f66c-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="9f66c-208">Poniższy przykład mapuje dwa typy jednostek, które współużytkują klucz podstawowy z jedną tabelą.</span><span class="sxs-lookup"><span data-stu-id="9f66c-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="9f66c-209">Mapowanie typu jednostki, aby wstawiać/aktualizować/usuwać procedury składowane (EF6 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="9f66c-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="9f66c-210">Począwszy od EF6 można zmapować jednostki, aby używać procedur składowanych do wstawiania aktualizacji i usuwania.</span><span class="sxs-lookup"><span data-stu-id="9f66c-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="9f66c-211">Aby uzyskać więcej informacji, zobacz [Code First wstawiania/aktualizowania/usuwania procedur składowanych](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span><span class="sxs-lookup"><span data-stu-id="9f66c-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="9f66c-212">Model używany w przykładach</span><span class="sxs-lookup"><span data-stu-id="9f66c-212">Model Used in Samples</span></span>  

<span data-ttu-id="9f66c-213">Następujący model Code First jest używany dla przykładów na tej stronie.</span><span class="sxs-lookup"><span data-stu-id="9f66c-213">The following Code First model is used for the samples on this page.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.ModelConfiguration.Conventions;
// add a reference to System.ComponentModel.DataAnnotations DLL
using System.ComponentModel.DataAnnotations;
using System.Collections.Generic;
using System;

public class SchoolEntities : DbContext
{
    public DbSet<Course> Courses { get; set; }
    public DbSet<Department> Departments { get; set; }
    public DbSet<Instructor> Instructors { get; set; }
    public DbSet<OfficeAssignment> OfficeAssignments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention then the generated tables will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}

public class Department
{
    public Department()
    {
        this.Courses = new HashSet<Course>();
    }
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }
    public decimal Budget { get; set; }
    public System.DateTime StartDate { get; set; }
    public int? Administrator { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; private set; }
}

public class Course
{
    public Course()
    {
        this.Instructors = new HashSet<Instructor>();
    }
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
    public virtual ICollection<Instructor> Instructors { get; private set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}

public class Instructor
{
    public Instructor()
    {
        this.Courses = new List<Course>();
    }

    // Primary key
    public int InstructorID { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
    public System.DateTime HireDate { get; set; }

    // Navigation properties
    public virtual ICollection<Course> Courses { get; private set; }
}

public class OfficeAssignment
{
    // Specifying InstructorID as a primary
    [Key()]
    public Int32 InstructorID { get; set; }

    public string Location { get; set; }

    // When Entity Framework sees Timestamp attribute
    // it configures ConcurrencyCheck and DatabaseGeneratedPattern=Computed.
    [Timestamp]
    public Byte[] Timestamp { get; set; }

    // Navigation property
    public virtual Instructor Instructor { get; set; }
}
```  
