---
title: Relacje, właściwości nawigacji i kluczy obcych — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: a1653afd609280ab572ef88a9fcf8a6275b79fd6
ms.sourcegitcommit: a81aed575372637997b18a0f9466d8fefb33350a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2018
ms.locfileid: "43821403"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a><span data-ttu-id="b9d75-102">Relacje, właściwości nawigacji i kluczy obcych</span><span class="sxs-lookup"><span data-stu-id="b9d75-102">Relationships, navigation properties and foreign keys</span></span>
<span data-ttu-id="b9d75-103">Ten temat zawiera omówienie sposobu zarządzania relacje między jednostkami w Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b9d75-103">This topic gives an overview of how Entity Framework manages relationships between entities.</span></span> <span data-ttu-id="b9d75-104">Daje ona również pewne wskazówki na temat sposobu mapowania i manipulowania nimi relacji.</span><span class="sxs-lookup"><span data-stu-id="b9d75-104">It also gives some guidance on how to map and manipulate relationships.</span></span>

## <a name="relationships-in-ef"></a><span data-ttu-id="b9d75-105">Relacje w programie EF</span><span class="sxs-lookup"><span data-stu-id="b9d75-105">Relationships in EF</span></span>

<span data-ttu-id="b9d75-106">Relacyjne bazy danych (zwane również skojarzeń) relacje między tabelami są definiowane za pomocą kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="b9d75-106">In relational databases, relationships (also called associations) between tables are defined through foreign keys.</span></span> <span data-ttu-id="b9d75-107">Klucz obcy (klucz OBCY) to kolumna lub połączenie kolumn, który służy do ustanawiania i wymuszają połączenie między danymi w dwóch tabelach.</span><span class="sxs-lookup"><span data-stu-id="b9d75-107">A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables.</span></span> <span data-ttu-id="b9d75-108">Zazwyczaj są trzy typy relacji: jeden do jednego, jeden do wielu i wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="b9d75-108">There are generally three types of relationships: one-to-one, one-to-many, and many-to-many.</span></span> <span data-ttu-id="b9d75-109">W relacji jeden do wielu klucz obcy jest zdefiniowany w tabeli, która reprezentuje liczbę końca relacji.</span><span class="sxs-lookup"><span data-stu-id="b9d75-109">In a one-to-many relationship, the foreign key is defined on the table that represents the many end of the relationship.</span></span> <span data-ttu-id="b9d75-110">Relacja wiele do wielu obejmuje zdefiniowanie trzeciej tabeli (nazywany połączeń lub łączony albo oczekuje tabeli), którego klucza podstawowego składa się z kluczy obcych z obu tabel powiązanych.</span><span class="sxs-lookup"><span data-stu-id="b9d75-110">The many-to-many relationship involves defining a third table (called a junction or join table), whose primary key is composed of the foreign keys from both related tables.</span></span> <span data-ttu-id="b9d75-111">W relacji jeden do jednego klucz podstawowy działa również jako klucz obcy, i nie ma żadnych oddzielne kolumny klucza obcego dla każdej tabeli.</span><span class="sxs-lookup"><span data-stu-id="b9d75-111">In a one-to-one relationship, the primary key acts additionally as a foreign key and there is no separate foreign key column for either table.</span></span>

<span data-ttu-id="b9d75-112">Na poniższej ilustracji przedstawiono dwie tabele, które uczestniczą w relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="b9d75-112">The following image shows two tables that participate in one-to-many relationship.</span></span> <span data-ttu-id="b9d75-113">**Kurs** tabela jest tabelą zależne, ponieważ zawiera on **DepartmentID** kolumny, która łączy się **działu** tabeli.</span><span class="sxs-lookup"><span data-stu-id="b9d75-113">The **Course** table is the dependent table because it contains the **DepartmentID** column that links it to the **Department** table.</span></span>

![Bazy danych 2](~/ef6/media/database2.png)

<span data-ttu-id="b9d75-115">Platformy Entity Framework jednostki może być powiązane z innymi obiektami za pośrednictwem stowarzyszenia lub relacji.</span><span class="sxs-lookup"><span data-stu-id="b9d75-115">In Entity Framework, an entity can be related to other entities through an association or relationship.</span></span> <span data-ttu-id="b9d75-116">Każda relacja zawiera dwa końce, które opisują typ jednostki i liczebności typu (jednego, zero lub jeden lub wiele) dwie jednostki w tej relacji.</span><span class="sxs-lookup"><span data-stu-id="b9d75-116">Each relationship contains two ends that describe the entity type and the multiplicity of the type (one, zero-or-one, or many) for the two entities in that relationship.</span></span> <span data-ttu-id="b9d75-117">Relacje mogą podlegać ograniczenia referencyjnego, w tym artykule opisano, które zakończenia w relacji jest główną rolę i który jest zależny roli.</span><span class="sxs-lookup"><span data-stu-id="b9d75-117">The relationship may be governed by a referential constraint, which describes which end in the relationship is a principal role and which is a dependent role.</span></span>

<span data-ttu-id="b9d75-118">Właściwości nawigacji Podaj sposób przechodzenia skojarzenie między dwoma typami encji.</span><span class="sxs-lookup"><span data-stu-id="b9d75-118">Navigation properties provide a way to navigate an association between two entity types.</span></span> <span data-ttu-id="b9d75-119">Każdy obiekt może mieć właściwości nawigacji dla każdej relacji, w których uczestniczy.</span><span class="sxs-lookup"><span data-stu-id="b9d75-119">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="b9d75-120">Właściwości nawigacji pozwala na przechodzenie relacji i zarządzanie nimi w obu kierunkach, zwracając obiekt odwołania (Jeśli liczebność jest jedną lub zero lub jeden) lub kolekcji (Jeśli liczebność to wiele).</span><span class="sxs-lookup"><span data-stu-id="b9d75-120">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="b9d75-121">Może również wybierzesz jednokierunkowe nawigacji, w którym to przypadku zdefiniować właściwości nawigacji na tylko jeden z typów, które uczestniczy w relacji, a nie w obu.</span><span class="sxs-lookup"><span data-stu-id="b9d75-121">You may also choose to have one-way navigation, in which case you define the navigation property on only one of the types that participates in the relationship and not on both.</span></span>

<span data-ttu-id="b9d75-122">Zalecane jest, aby uwzględnić właściwości w modelu, które są mapowane na klucze obce w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b9d75-122">It is recommended to include properties in the model that map to foreign keys in the database.</span></span> <span data-ttu-id="b9d75-123">Dołączone właściwości klucza obcego można utworzyć lub zmienić relacji, zmieniając wartość klucza obcego w obiekcie zależne.</span><span class="sxs-lookup"><span data-stu-id="b9d75-123">With foreign key properties included, you can create or change a relationship by modifying the foreign key value on a dependent object.</span></span> <span data-ttu-id="b9d75-124">Tego rodzaju skojarzenia nazywa się to skojarzenie klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="b9d75-124">This kind of association is called a foreign key association.</span></span> <span data-ttu-id="b9d75-125">Przy użyciu kluczy obcych jest nawet bardziej istotne, podczas pracy z odłączone jednostki.</span><span class="sxs-lookup"><span data-stu-id="b9d75-125">Using foreign keys is even more essential when working with disconnected entities.</span></span> <span data-ttu-id="b9d75-126">Należy pamiętać, że w przypadku pracy z 1-do-1 lub 1-0.. Relacje 1 jest nie oddzielne kolumny klucza obcego, właściwość klucza podstawowego działa jako klucza obcego i zawsze znajduje się w modelu.</span><span class="sxs-lookup"><span data-stu-id="b9d75-126">Note, that when working with 1-to-1 or 1-to-0..1 relationships, there is no separate foreign key column, the primary key property acts as the foreign key and is always included in the model.</span></span>

<span data-ttu-id="b9d75-127">Kolumny klucza obcego nie są uwzględnione w modelu informacji o skojarzeniu odbywa się jako niezależny obiekt.</span><span class="sxs-lookup"><span data-stu-id="b9d75-127">When foreign key columns are not included in the model, the association information is managed as an independent object.</span></span> <span data-ttu-id="b9d75-128">Relacje są śledzone za pomocą odwołania do obiektów zamiast właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="b9d75-128">Relationships are tracked through object references instead of foreign key properties.</span></span> <span data-ttu-id="b9d75-129">Skojarzenie tego typu jest nazywana *niezależnych skojarzenie*.</span><span class="sxs-lookup"><span data-stu-id="b9d75-129">This type of association is called an *independent association*.</span></span> <span data-ttu-id="b9d75-130">Najczęstszym sposobem modyfikowania *niezależnych skojarzenie* jest zmodyfikowanie właściwości nawigacji, które są generowane dla każdego obiektu, który uczestniczy w skojarzeniu.</span><span class="sxs-lookup"><span data-stu-id="b9d75-130">The most common way to modify an *independent association* is to modify the navigation properties that are generated for each entity that participates in the association.</span></span>

<span data-ttu-id="b9d75-131">Można użyć jednego lub obu typów skojarzeń w modelu.</span><span class="sxs-lookup"><span data-stu-id="b9d75-131">You can choose to use one or both types of associations in your model.</span></span> <span data-ttu-id="b9d75-132">Jednak w przypadku czystego relację wiele do wielu, która jest połączona za pomocą tabeli sprzężenia, która zawiera tylko klucze obce EF użyje niezależnych skojarzenia do zarządzania takich relacji wiele do wielu.</span><span class="sxs-lookup"><span data-stu-id="b9d75-132">However, if you have a pure many-to-many relationship that is connected by a join table that contains only foreign keys, the EF will use an independent association to manage such many-to-many relationship.</span></span>   

<span data-ttu-id="b9d75-133">Na poniższej ilustracji przedstawiono model koncepcyjny, który został utworzony za pomocą programu Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="b9d75-133">The following image shows a conceptual model that was created with the Entity Framework Designer.</span></span> <span data-ttu-id="b9d75-134">Model zawiera dwie jednostki, które uczestniczą w relacji jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="b9d75-134">The model contains two entities that participate in one-to-many relationship.</span></span> <span data-ttu-id="b9d75-135">Zarówno jednostki mają właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="b9d75-135">Both entities have navigation properties.</span></span> <span data-ttu-id="b9d75-136">**Kurs** jednostki depend i ma **DepartmentID** zdefiniowana właściwość klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="b9d75-136">**Course** is the depend entity and has the **DepartmentID** foreign key property defined.</span></span>

![RelationshipEFDesigner](~/ef6/media/relationshipefdesigner.png)

<span data-ttu-id="b9d75-138">Poniższy fragment kodu przedstawia ten sam model, który został utworzony za pomocą Code First.</span><span class="sxs-lookup"><span data-stu-id="b9d75-138">The following code snippet shows the same model that was created with Code First.</span></span>

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class Department
{
   public Department()
   {
     this.Course = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a><span data-ttu-id="b9d75-139">Konfigurowanie lub Mapowanie relacji</span><span class="sxs-lookup"><span data-stu-id="b9d75-139">Configuring or mapping relationships</span></span>

<span data-ttu-id="b9d75-140">Pozostałej części tej strony opisano, jak uzyskać dostęp i manipulowanie danymi za pomocą relacji.</span><span class="sxs-lookup"><span data-stu-id="b9d75-140">The rest of this page covers how to access and manipulate data using relationships.</span></span> <span data-ttu-id="b9d75-141">Aby uzyskać informacje na temat konfigurowania relacji w modelu zobacz następujące strony.</span><span class="sxs-lookup"><span data-stu-id="b9d75-141">For information on setting up relationships in your model, see the following pages.</span></span>

-   <span data-ttu-id="b9d75-142">Aby skonfigurować relacje w Code First, zobacz [adnotacje danych](~/ef6/modeling/code-first/data-annotations.md) i [Fluent API — relacje](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="b9d75-142">To configure relationships in Code First, see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>
-   <span data-ttu-id="b9d75-143">Aby skonfigurować relacji za pomocą programu Entity Framework Designer, zobacz [relacji z projektancie platformy EF](~/ef6/modeling/designer/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="b9d75-143">To configure relationships using the Entity Framework Designer, see [Relationships with the EF Designer](~/ef6/modeling/designer/relationships.md).</span></span>

## <a name="creating-and-modifying-relationships"></a><span data-ttu-id="b9d75-144">Tworzenie i modyfikowanie relacji</span><span class="sxs-lookup"><span data-stu-id="b9d75-144">Creating and modifying relationships</span></span>

<span data-ttu-id="b9d75-145">W *skojarzenie klucza obcego*, gdy zmienisz tę relację, stan obiektu zależnego za pomocą `EntityState.Unchanged` stan zmieni się na `EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="b9d75-145">In a *foreign key association*, when you change the relationship, the state of a dependent object with an `EntityState.Unchanged` state changes to `EntityState.Modified`.</span></span> <span data-ttu-id="b9d75-146">W relacji niezależne zmiana relacji nie aktualizuje stan obiektu zależnego.</span><span class="sxs-lookup"><span data-stu-id="b9d75-146">In an independent relationship, changing the relationship does not update the state of the dependent object.</span></span>

<span data-ttu-id="b9d75-147">Poniższe przykłady pokazują, jak korzystać z właściwości klucza obcego i właściwości nawigacji do skojarzenia powiązanych obiektów.</span><span class="sxs-lookup"><span data-stu-id="b9d75-147">The following examples show how to use the foreign key properties and navigation properties to associate the related objects.</span></span> <span data-ttu-id="b9d75-148">Za pomocą powiązań kluczy obcych można użyć jednej z metod można zmienić, tworzyć lub modyfikować relacje.</span><span class="sxs-lookup"><span data-stu-id="b9d75-148">With foreign key associations, you can use either method to change, create, or modify relationships.</span></span> <span data-ttu-id="b9d75-149">Za pomocą skojarzeń niezależne nie można użyć właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="b9d75-149">With independent associations, you cannot use the foreign key property.</span></span>

- <span data-ttu-id="b9d75-150">Przypisując nową wartość do właściwości klucza obcego, jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="b9d75-150">By assigning a new value to a foreign key property, as in the following example.</span></span>  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- <span data-ttu-id="b9d75-151">Poniższy kod usuwa relację, ustawiając klucz obcy **null**.</span><span class="sxs-lookup"><span data-stu-id="b9d75-151">The following code removes a relationship by setting the foreign key to **null**.</span></span> <span data-ttu-id="b9d75-152">Należy zauważyć, że właściwość klucza obcego, musi być typ zerowalny.</span><span class="sxs-lookup"><span data-stu-id="b9d75-152">Note, that the foreign key property must be nullable.</span></span>  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > <span data-ttu-id="b9d75-153">W przypadku odwołania w stanie dodany (w tym przykładzie obiekt kurs) referencyjna właściwość nawigacji nie zostaną zsynchronizowane przy użyciu wartości kluczy nowy obiekt do momentu SaveChanges jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="b9d75-153">If the reference is in the added state (in this example, the course object), the reference navigation property will not be synchronized with the key values of a new object until SaveChanges is called.</span></span> <span data-ttu-id="b9d75-154">Synchronizacja nie występuje, ponieważ kontekst nie zawiera stałe klucze dla dodanych obiektów, dopóki nie zostaną one zapisane.</span><span class="sxs-lookup"><span data-stu-id="b9d75-154">Synchronization does not occur because the object context does not contain permanent keys for added objects until they are saved.</span></span> <span data-ttu-id="b9d75-155">Jeśli konieczne jest posiadanie nowych obiektów w pełni zsynchronizowane tak szybko, jak ustawić relację, użyj jednej z następujących methods.\*</span><span class="sxs-lookup"><span data-stu-id="b9d75-155">If you must have new objects fully synchronized as soon as you set the relationship, use one of the following methods.\*</span></span>

- <span data-ttu-id="b9d75-156">Przypisując nowy obiekt z właściwością nawigacji.</span><span class="sxs-lookup"><span data-stu-id="b9d75-156">By assigning a new object to a navigation property.</span></span> <span data-ttu-id="b9d75-157">Poniższy kod tworzy relację między kurs i `department`.</span><span class="sxs-lookup"><span data-stu-id="b9d75-157">The following code creates a relationship between a course and a `department`.</span></span> <span data-ttu-id="b9d75-158">Jeśli obiekty są dołączone do kontekstu, `course` jest także dodawane do `department.Courses` kolekcji i obce odpowiedniej właściwości klucza na `course` obiektu ma ustawioną wartość właściwości klucza działu.</span><span class="sxs-lookup"><span data-stu-id="b9d75-158">If the objects are attached to the context, the `course` is also added to the `department.Courses` collection, and the corresponding foreign key property on the `course` object is set to the key property value of the department.</span></span>  
  ``` csharp
  course.Department = department;
  ```

- <span data-ttu-id="b9d75-159">Aby usunąć relację, ustaw właściwość nawigacji na `null`.</span><span class="sxs-lookup"><span data-stu-id="b9d75-159">To delete the relationship, set the navigation property to `null`.</span></span> <span data-ttu-id="b9d75-160">Pracy z platformą Entity Framework, który jest oparty na .NET 4.0 powiązanego zakończenia musi być załadowany, zanim zostanie ustawiona na wartość null.</span><span class="sxs-lookup"><span data-stu-id="b9d75-160">If you are working with Entity Framework that is based on .NET 4.0, then the related end needs to be loaded before you set it to null.</span></span> <span data-ttu-id="b9d75-161">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b9d75-161">For example:</span></span>   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  <span data-ttu-id="b9d75-162">Począwszy od Entity Framework 5.0, który jest oparty na .NET 4.5, można ustawić relację null bez powiązanym zakończeniem ładowania.</span><span class="sxs-lookup"><span data-stu-id="b9d75-162">Starting with Entity Framework 5.0, that is based on .NET 4.5, you can set the relationship to null without loading the related end.</span></span> <span data-ttu-id="b9d75-163">Można również ustawić bieżącą wartość null, przy użyciu następującej metody.</span><span class="sxs-lookup"><span data-stu-id="b9d75-163">You can also set the current value to null using the following method.</span></span>   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- <span data-ttu-id="b9d75-164">Przez usunięcie lub dodanie obiektu w kolekcji jednostek.</span><span class="sxs-lookup"><span data-stu-id="b9d75-164">By deleting or adding an object in an entity collection.</span></span> <span data-ttu-id="b9d75-165">Na przykład można dodać obiektu typu `Course` do `department.Courses` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="b9d75-165">For example, you can add an object of type `Course` to the `department.Courses` collection.</span></span> <span data-ttu-id="b9d75-166">Ta operacja tworzy relację między określonego **kurs** i określonego `department`.</span><span class="sxs-lookup"><span data-stu-id="b9d75-166">This operation creates a relationship between a particular **course** and a particular `department`.</span></span> <span data-ttu-id="b9d75-167">Jeśli obiekty są przyłączone do kontekstu, odwołanie działu i właściwość klucza obcego **kurs** obiektu zostanie ustawiony na odpowiednie `department`.</span><span class="sxs-lookup"><span data-stu-id="b9d75-167">If the objects are attached to the context, the department reference and the foreign key property on the **course** object will be set to the appropriate `department`.</span></span>  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- <span data-ttu-id="b9d75-168">Za pomocą `ChangeRelationshipState` metodę, aby zmienić stan określonej relacji między dwa obiekty jednostki.</span><span class="sxs-lookup"><span data-stu-id="b9d75-168">By using the `ChangeRelationshipState` method to change the state of the specified relationship between two entity objects.</span></span> <span data-ttu-id="b9d75-169">Ta metoda jest najczęściej używana podczas pracy z usługą aplikacji N-warstwowych i *niezależnych skojarzenia* (nie można używać z skojarzenie klucza obcego).</span><span class="sxs-lookup"><span data-stu-id="b9d75-169">This method is most commonly used when working with N-Tier applications and an *independent association* (it cannot be used with a foreign key association).</span></span> <span data-ttu-id="b9d75-170">Ponadto do używania tej metody należy usunąć w dół do `ObjectContext`, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="b9d75-170">Also, to use this method you must drop down to `ObjectContext`, as shown in the example below.</span></span>  
<span data-ttu-id="b9d75-171">W poniższym przykładzie istnieje relacja wiele do wielu między szkolenia i kursy.</span><span class="sxs-lookup"><span data-stu-id="b9d75-171">In the following example, there is a many-to-many relationship between Instructors and Courses.</span></span> <span data-ttu-id="b9d75-172">Wywoływanie `ChangeRelationshipState` metody i przekazywanie `EntityState.Added` parametr umożliwia `SchoolContext` wiedzieć, że dodano relację między dwoma obiektami:</span><span class="sxs-lookup"><span data-stu-id="b9d75-172">Calling the `ChangeRelationshipState` method and passing the `EntityState.Added` parameter, lets the `SchoolContext` know that a relationship has been added between the two objects:</span></span>
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  <span data-ttu-id="b9d75-173">Należy pamiętać, że Jeśli aktualizujesz (nie tylko dodanie) relację, należy usunąć stare relację po dodaniu nowego:</span><span class="sxs-lookup"><span data-stu-id="b9d75-173">Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:</span></span>

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a><span data-ttu-id="b9d75-174">Synchronizowanie zmian między kluczy obcych i właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="b9d75-174">Synchronizing the changes between the foreign keys and navigation properties</span></span>

<span data-ttu-id="b9d75-175">Po zmianie relacji obiektów dołączyć do kontekstu przy użyciu jednej z metod opisanych wyżej Entity Framework musi Synchronizuj klucze obce, odwołania i kolekcji. Entity Framework automatycznie zarządza tej synchronizacji (znany także jako relacja poprawki) dla obiektów POCO z serwerami proxy.</span><span class="sxs-lookup"><span data-stu-id="b9d75-175">When you change the relationship of the objects attached to the context by using one of the methods described above, Entity Framework needs to keep foreign keys, references, and collections in sync. Entity Framework automatically manages this synchronization (also known as relationship fix-up) for the POCO entities with proxies.</span></span> <span data-ttu-id="b9d75-176">Aby uzyskać więcej informacji, zobacz [pracy z serwerami proxy](~/ef6/fundamentals/proxies.md).</span><span class="sxs-lookup"><span data-stu-id="b9d75-176">For more information, see [Working with Proxies](~/ef6/fundamentals/proxies.md).</span></span>

<span data-ttu-id="b9d75-177">Jeśli używasz jednostki POCO bez serwera proxy należy musi upewnij się, że **metody DetectChanges** metoda jest wywoływana, aby zsynchronizować obiekty powiązane w kontekście.</span><span class="sxs-lookup"><span data-stu-id="b9d75-177">If you are using POCO entities without proxies, you must make sure that the **DetectChanges** method is called to synchronize the related objects in the context.</span></span> <span data-ttu-id="b9d75-178">Dokonany wybór, następujące interfejsy API automatycznie wyzwoli **metody DetectChanges** wywołania.</span><span class="sxs-lookup"><span data-stu-id="b9d75-178">Note, that the following APIs automatically trigger a **DetectChanges** call.</span></span>

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   <span data-ttu-id="b9d75-179">LINQ do wykonywania zapytań względem `DbSet`</span><span class="sxs-lookup"><span data-stu-id="b9d75-179">Executing a LINQ query against a `DbSet`</span></span>

## <a name="loading-related-objects"></a><span data-ttu-id="b9d75-180">Trwa ładowanie powiązanych obiektów</span><span class="sxs-lookup"><span data-stu-id="b9d75-180">Loading related objects</span></span>

<span data-ttu-id="b9d75-181">Platformy Entity Framework, którego używasz najczęściej ładowanie jednostek that are related to jednostki zwracanego przez skojarzenie zdefiniowane przy użyciu właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="b9d75-181">In Entity Framework you use most commonly use the navigation properties to load entities that are related to the returned entity by the defined association.</span></span> <span data-ttu-id="b9d75-182">Aby uzyskać więcej informacji, zobacz [ładowanie powiązanych obiektów](~/ef6/querying/related-data.md).</span><span class="sxs-lookup"><span data-stu-id="b9d75-182">For more information, see [Loading Related Objects](~/ef6/querying/related-data.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b9d75-183">W to skojarzenie klucza obcego w przypadku ładowania powiązanym zakończeniem obiektu zależnego powiązany obiekt zostaną załadowane zależnie od wartości klucza obcego zależnych od, która jest aktualnie w pamięci:</span><span class="sxs-lookup"><span data-stu-id="b9d75-183">In a foreign key association, when you load a related end of a dependent object, the related object will be loaded based on the foreign key value of the dependent that is currently in memory:</span></span>

``` csharp
    // Get the course where currently DepartmentID = 1.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

<span data-ttu-id="b9d75-184">Skojarzenie niezależnych powiązane koniec obiektu zależnego zostaje przesłane zapytanie, oparte na wartości klucza obcego, który jest obecnie dostępna w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b9d75-184">In an independent association, the related end of a dependent object is queried based on the foreign key value that is currently in the database.</span></span> <span data-ttu-id="b9d75-185">Jednak jeśli zmodyfikowano relację i spróbuje utworzyć relację jako referencyjna właściwość w punktach obiektu zależnego do innego obiektu podmiotu zabezpieczeń, który jest ładowany w kontekście obiektu programu Entity Framework jest zdefiniowana na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="b9d75-185">However, if the relationship was modified, and the reference property on the dependent object points to a different principal object that is loaded in the object context, Entity Framework will try to create a relationship as it is defined on the client.</span></span>

## <a name="managing-concurrency"></a><span data-ttu-id="b9d75-186">Zarządzanie współbieżnością</span><span class="sxs-lookup"><span data-stu-id="b9d75-186">Managing concurrency</span></span>

<span data-ttu-id="b9d75-187">W kluczu obcym i skojarzenia niezależnych kontrolach współbieżności są oparte na klucze jednostek i inne właściwości jednostki, które są zdefiniowane w modelu.</span><span class="sxs-lookup"><span data-stu-id="b9d75-187">In both foreign key and independent associations, concurrency checks are based on the entity keys and other entity properties that are defined in the model.</span></span> <span data-ttu-id="b9d75-188">Tworzenie modelu za pomocą projektancie platformy EF, ustaw `ConcurrencyMode` atrybutu **stałej** do określenia, czy właściwość powinna być sprawdzana współbieżność.</span><span class="sxs-lookup"><span data-stu-id="b9d75-188">When using the EF Designer to create a model, set the `ConcurrencyMode` attribute to **fixed** to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="b9d75-189">Podczas korzystania Code First do definiowania modelu należy używać `ConcurrencyCheck` adnotacja dla właściwości, które mają być sprawdzone współbieżności.</span><span class="sxs-lookup"><span data-stu-id="b9d75-189">When using Code First to define a model, use the `ConcurrencyCheck` annotation on properties that you want to be checked for concurrency.</span></span> <span data-ttu-id="b9d75-190">Podczas pracy z usługą Code First umożliwia także `TimeStamp` adnotacji, aby określić, czy właściwość powinna być sprawdzana współbieżność.</span><span class="sxs-lookup"><span data-stu-id="b9d75-190">When working with Code First you can also use the `TimeStamp` annotation to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="b9d75-191">Może mieć tylko jedną właściwość sygnatury czasowej w danej klasy.</span><span class="sxs-lookup"><span data-stu-id="b9d75-191">You can have only one timestamp property in a given class.</span></span> <span data-ttu-id="b9d75-192">Mapy kodu najpierw tę właściwość z polem wartości null w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b9d75-192">Code First maps this property to a non-nullable field in the database.</span></span>

<span data-ttu-id="b9d75-193">Firma Microsoft zaleca, zawsze używaj skojarzenie klucza obcego podczas pracy z obiektami, które uczestniczą w sprawdzania współbieżności i rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="b9d75-193">We recommend that you always use the foreign key association when working with entities that participate in concurrency checking and resolution.</span></span>

<span data-ttu-id="b9d75-194">Aby uzyskać więcej informacji, zobacz [Obsługa konfliktów współbieżności](~/ef6/saving/concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="b9d75-194">For more information, see [Handling Concurrency Conflicts](~/ef6/saving/concurrency.md).</span></span>

## <a name="working-with-overlapping-keys"></a><span data-ttu-id="b9d75-195">Praca z nakładającymi się kluczami</span><span class="sxs-lookup"><span data-stu-id="b9d75-195">Working with overlapping Keys</span></span>

<span data-ttu-id="b9d75-196">Nakładające się klucze są klucze złożone, gdzie niektóre właściwości klucza są również częścią innego klucza w jednostce.</span><span class="sxs-lookup"><span data-stu-id="b9d75-196">Overlapping keys are composite keys where some properties in the key are also part of another key in the entity.</span></span> <span data-ttu-id="b9d75-197">Skojarzenie niezależne, nie może mieć nakładających się klucza.</span><span class="sxs-lookup"><span data-stu-id="b9d75-197">You cannot have an overlapping key in an independent association.</span></span> <span data-ttu-id="b9d75-198">Aby zmienić to skojarzenie klucza obcego obejmującą nakładającymi się kluczami, zaleca się zmodyfikowanie wartości klucza obcego zamiast odwołania do obiektu.</span><span class="sxs-lookup"><span data-stu-id="b9d75-198">To change a foreign key association that includes overlapping keys, we recommend that you modify the foreign key values instead of using the object references.</span></span>
