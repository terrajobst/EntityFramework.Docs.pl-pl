---
title: Praca z Stanami jednostek — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: ef0e8d5a5a9d66adab7046088c49d8cd472edc8a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419700"
---
# <a name="working-with-entity-states"></a><span data-ttu-id="95379-102">Praca z Stanami jednostek</span><span class="sxs-lookup"><span data-stu-id="95379-102">Working with entity states</span></span>
<span data-ttu-id="95379-103">W tym temacie opisano sposób dodawania i dołączania jednostek do kontekstu oraz sposobu, w jaki Entity Framework przetwarzają je podczas metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="95379-103">This topic will cover how to add and attach entities to a context and how Entity Framework processes these during SaveChanges.</span></span>
<span data-ttu-id="95379-104">Entity Framework ma obsłużyć śledzenie stanu jednostek, gdy są one połączone z kontekstem, ale w przypadku scenariuszy rozłączonych lub N-warstwowych można pozwolić, aby EF wiedział, jaki jest stan jednostek.</span><span class="sxs-lookup"><span data-stu-id="95379-104">Entity Framework takes care of tracking the state of entities while they are connected to a context, but in disconnected or N-Tier scenarios you can let EF know what state your entities should be in.</span></span>
<span data-ttu-id="95379-105">Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="95379-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="entity-states-and-savechanges"></a><span data-ttu-id="95379-106">Stany jednostek i metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="95379-106">Entity states and SaveChanges</span></span>

<span data-ttu-id="95379-107">Jednostka może znajdować się w jednym z pięciu Stanów, zgodnie z definicją EntityState.</span><span class="sxs-lookup"><span data-stu-id="95379-107">An entity can be in one of five states as defined by the EntityState enumeration.</span></span> <span data-ttu-id="95379-108">Te Stany to:</span><span class="sxs-lookup"><span data-stu-id="95379-108">These states are:</span></span>  

- <span data-ttu-id="95379-109">Dodano: jednostka jest śledzona przez kontekst, ale jeszcze nie istnieje w bazie danych</span><span class="sxs-lookup"><span data-stu-id="95379-109">Added: the entity is being tracked by the context but does not yet exist in the database</span></span>  
- <span data-ttu-id="95379-110">Niezmieniony: jednostka jest śledzona przez kontekst i istnieje w bazie danych, a jej wartości właściwości nie uległy zmianie z wartości w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="95379-110">Unchanged: the entity is being tracked by the context and exists in the database, and its property values have not changed from the values in the database</span></span>  
- <span data-ttu-id="95379-111">Zmodyfikowano: jednostka jest śledzona przez kontekst i istnieje w bazie danych, a niektóre lub wszystkie wartości właściwości zostały zmodyfikowane.</span><span class="sxs-lookup"><span data-stu-id="95379-111">Modified: the entity is being tracked by the context and exists in the database, and some or all of its property values have been modified</span></span>  
- <span data-ttu-id="95379-112">Usunięto: jednostka jest śledzona przez kontekst i istnieje w bazie danych, ale została oznaczona do usunięcia z bazy danych przy następnym wywołaniu metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="95379-112">Deleted: the entity is being tracked by the context and exists in the database, but has been marked for deletion from the database the next time SaveChanges is called</span></span>  
- <span data-ttu-id="95379-113">Odłączono: jednostka nie jest śledzona przez kontekst</span><span class="sxs-lookup"><span data-stu-id="95379-113">Detached: the entity is not being tracked by the context</span></span>  

<span data-ttu-id="95379-114">Metody SaveChanges wykonuje różne rzeczy dla jednostek w różnych stanach:</span><span class="sxs-lookup"><span data-stu-id="95379-114">SaveChanges does different things for entities in different states:</span></span>  

- <span data-ttu-id="95379-115">Niezmienione jednostki nie są rozróżniane przez metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="95379-115">Unchanged entities are not touched by SaveChanges.</span></span> <span data-ttu-id="95379-116">Aktualizacje nie są wysyłane do bazy danych dla jednostek w stanie niezmienionym.</span><span class="sxs-lookup"><span data-stu-id="95379-116">Updates are not sent to the database for entities in the Unchanged state.</span></span>  
- <span data-ttu-id="95379-117">Dodane jednostki są wstawiane do bazy danych, a następnie stają się niezmienione, gdy metody SaveChanges zwraca.</span><span class="sxs-lookup"><span data-stu-id="95379-117">Added entities are inserted into the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="95379-118">Zmodyfikowane jednostki są aktualizowane w bazie danych, a następnie stają się niezmienione, gdy metody SaveChanges zwraca.</span><span class="sxs-lookup"><span data-stu-id="95379-118">Modified entities are updated in the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="95379-119">Usunięte jednostki są usuwane z bazy danych, a następnie odłączane od kontekstu.</span><span class="sxs-lookup"><span data-stu-id="95379-119">Deleted entities are deleted from the database and are then detached from the context.</span></span>  

<span data-ttu-id="95379-120">W poniższych przykładach pokazano, jak można zmienić stan jednostki lub grafu jednostki.</span><span class="sxs-lookup"><span data-stu-id="95379-120">The following examples show ways in which the state of an entity or an entity graph can be changed.</span></span>  

## <a name="adding-a-new-entity-to-the-context"></a><span data-ttu-id="95379-121">Dodawanie nowej jednostki do kontekstu</span><span class="sxs-lookup"><span data-stu-id="95379-121">Adding a new entity to the context</span></span>  

<span data-ttu-id="95379-122">Nową jednostkę można dodać do kontekstu, wywołując metodę Add w Nieogólnymi.</span><span class="sxs-lookup"><span data-stu-id="95379-122">A new entity can be added to the context by calling the Add method on DbSet.</span></span>
<span data-ttu-id="95379-123">Powoduje to umieszczenie jednostki w stanie dodany, co oznacza, że zostanie ona wstawiona do bazy danych przy następnym wywołaniu metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="95379-123">This puts the entity into the Added state, meaning that it will be inserted into the database the next time that SaveChanges is called.</span></span>
<span data-ttu-id="95379-124">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="95379-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

<span data-ttu-id="95379-125">Innym sposobem dodania nowej jednostki do kontekstu jest zmiana jej stanu na dodany.</span><span class="sxs-lookup"><span data-stu-id="95379-125">Another way to add a new entity to the context is to change its state to Added.</span></span> <span data-ttu-id="95379-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="95379-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

<span data-ttu-id="95379-127">Na koniec można dodać nową jednostkę do kontekstu, łącząc ją z inną jednostką, która jest już śledzona.</span><span class="sxs-lookup"><span data-stu-id="95379-127">Finally, you can add a new entity to the context by hooking it up to another entity that is already being tracked.</span></span>
<span data-ttu-id="95379-128">Może to być spowodowane dodaniem nowej jednostki do właściwości nawigacji kolekcji innej jednostki lub przez ustawienie właściwości nawigacji referencyjnej innej jednostki, aby wskazywała nową jednostkę.</span><span class="sxs-lookup"><span data-stu-id="95379-128">This could be by adding the new entity to the collection navigation property of another entity or by setting a reference navigation property of another entity to point to the new entity.</span></span> <span data-ttu-id="95379-129">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="95379-129">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

<span data-ttu-id="95379-130">Należy zauważyć, że dla wszystkich tych przykładów, jeśli dodawana jednostka zawiera odwołania do innych jednostek, które nie są jeszcze śledzone, te nowe jednostki zostaną również dodane do kontekstu i zostaną wstawione do bazy danych przy następnym wywołaniu metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="95379-130">Note that for all of these examples if the entity being added has references to other entities that are not yet tracked then these new entities will also be added to the context and will be inserted into the database the next time that SaveChanges is called.</span></span>  

## <a name="attaching-an-existing-entity-to-the-context"></a><span data-ttu-id="95379-131">Dołączanie istniejącej jednostki do kontekstu</span><span class="sxs-lookup"><span data-stu-id="95379-131">Attaching an existing entity to the context</span></span>  

<span data-ttu-id="95379-132">Jeśli masz już istniejącą jednostkę w bazie danych, która nie jest obecnie śledzona przez kontekst, możesz powiedzieć kontekstowi, aby śledził jednostkę przy użyciu metody Attach w Nieogólnymi.</span><span class="sxs-lookup"><span data-stu-id="95379-132">If you have an entity that you know already exists in the database but which is not currently being tracked by the context then you can tell the context to track the entity using the Attach method on DbSet.</span></span> <span data-ttu-id="95379-133">Jednostka będzie w stanie niezmienionym w kontekście.</span><span class="sxs-lookup"><span data-stu-id="95379-133">The entity will be in the Unchanged state in the context.</span></span> <span data-ttu-id="95379-134">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="95379-134">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="95379-135">Należy zauważyć, że w bazie danych nie zostaną wprowadzone żadne zmiany, jeśli metody SaveChanges jest wywoływana bez wykonywania jakichkolwiek innych operacji manipulowania załączoną jednostką.</span><span class="sxs-lookup"><span data-stu-id="95379-135">Note that no changes will be made to the database if SaveChanges is called without doing any other manipulation of the attached entity.</span></span> <span data-ttu-id="95379-136">Wynika to z faktu, że jednostka jest w stanie niezmienionym.</span><span class="sxs-lookup"><span data-stu-id="95379-136">This is because the entity is in the Unchanged state.</span></span>  

<span data-ttu-id="95379-137">Innym sposobem dołączenia istniejącej jednostki do kontekstu jest zmiana stanu na niezmieniony.</span><span class="sxs-lookup"><span data-stu-id="95379-137">Another way to attach an existing entity to the context is to change its state to Unchanged.</span></span> <span data-ttu-id="95379-138">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="95379-138">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="95379-139">Należy zauważyć, że w obu tych przykładach, jeśli dołączona jednostka ma odwołania do innych jednostek, które nie są jeszcze śledzone, te nowe jednostki zostaną również dołączone do kontekstu w stanie niezmienionym.</span><span class="sxs-lookup"><span data-stu-id="95379-139">Note that for both of these examples if the entity being attached has references to other entities that are not yet tracked then these new entities will also attached to the context in the Unchanged state.</span></span>  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a><span data-ttu-id="95379-140">Dołączanie istniejącej, ale zmodyfikowanej jednostki do kontekstu</span><span class="sxs-lookup"><span data-stu-id="95379-140">Attaching an existing but modified entity to the context</span></span>  

<span data-ttu-id="95379-141">Jeśli masz już istniejącą jednostkę w bazie danych, ale w której można było wprowadzić zmiany, możesz poinstruować kontekst, aby dołączyć jednostkę i ustawić jej stan na zmodyfikowano.</span><span class="sxs-lookup"><span data-stu-id="95379-141">If you have an entity that you know already exists in the database but to which changes may have been made then you can tell the context to attach the entity and set its state to Modified.</span></span>
<span data-ttu-id="95379-142">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="95379-142">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="95379-143">Zmiana stanu na zmodyfikowano wszystkie właściwości jednostki zostanie oznaczona jako zmodyfikowana, a wszystkie wartości właściwości będą wysyłane do bazy danych po wywołaniu metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="95379-143">When you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  

<span data-ttu-id="95379-144">Należy pamiętać, że jeśli dołączona jednostka ma odwołania do innych jednostek, które nie są jeszcze śledzone, wówczas te nowe jednostki zostaną dołączone do kontekstu w stanie niezmienionym — nie zostaną automatycznie zmodyfikowane.</span><span class="sxs-lookup"><span data-stu-id="95379-144">Note that if the entity being attached has references to other entities that are not yet tracked, then these new entities will attached to the context in the Unchanged state—they will not automatically be made Modified.</span></span>
<span data-ttu-id="95379-145">Jeśli masz wiele jednostek, które muszą zostać oznaczone jako zmodyfikowane, należy ustawić dla każdej z nich stan osobno.</span><span class="sxs-lookup"><span data-stu-id="95379-145">If you have multiple entities that need to be marked Modified you should set the state for each of these entities individually.</span></span>  

## <a name="changing-the-state-of-a-tracked-entity"></a><span data-ttu-id="95379-146">Zmiana stanu śledzonej jednostki</span><span class="sxs-lookup"><span data-stu-id="95379-146">Changing the state of a tracked entity</span></span>  

<span data-ttu-id="95379-147">Stan jednostki, która jest już śledzona, można zmienić, ustawiając właściwość State na jej wpis.</span><span class="sxs-lookup"><span data-stu-id="95379-147">You can change the state of an entity that is already being tracked by setting the State property on its entry.</span></span> <span data-ttu-id="95379-148">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="95379-148">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="95379-149">Należy zauważyć, że wywołanie metody Add lub Attach dla jednostki, która jest już śledzona, może być również używane do zmiany stanu jednostki.</span><span class="sxs-lookup"><span data-stu-id="95379-149">Note that calling Add or Attach for an entity that is already tracked can also be used to change the entity state.</span></span> <span data-ttu-id="95379-150">Na przykład wywołanie metody Attach dla jednostki, która jest obecnie w stanie dodany, zmieni swój stan na niezmieniony.</span><span class="sxs-lookup"><span data-stu-id="95379-150">For example, calling Attach for an entity that is currently in the Added state will change its state to Unchanged.</span></span>  

## <a name="insert-or-update-pattern"></a><span data-ttu-id="95379-151">Wstaw lub Aktualizuj wzorzec</span><span class="sxs-lookup"><span data-stu-id="95379-151">Insert or update pattern</span></span>  

<span data-ttu-id="95379-152">Typowym wzorcem dla niektórych aplikacji jest dodanie jednostki jako nowej (w wyniku wstawienia bazy danych) lub dołączenie jednostki jako istniejącej i oznaczenie jej jako zmodyfikowanej (w wyniku aktualizacji bazy danych) w zależności od wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="95379-152">A common pattern for some applications is to either Add an entity as new (resulting in a database insert) or Attach an entity as existing and mark it as modified (resulting in a database update) depending on the value of the primary key.</span></span>
<span data-ttu-id="95379-153">Na przykład podczas korzystania z wygenerowanej przez bazę danych liczb całkowitych klucze podstawowe są wspólne do traktowania jednostki z kluczem zerowym jako nowy i jednostki z niezerowym kluczem jako istniejący.</span><span class="sxs-lookup"><span data-stu-id="95379-153">For example, when using database generated integer primary keys it is common to treat an entity with a zero key as new and an entity with a non-zero key as existing.</span></span>
<span data-ttu-id="95379-154">Ten wzorzec można osiągnąć przez ustawienie stanu jednostki na podstawie sprawdzenia wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="95379-154">This pattern can be achieved by setting the entity state based on a check of the primary key value.</span></span> <span data-ttu-id="95379-155">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="95379-155">For example:</span></span>  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

<span data-ttu-id="95379-156">Należy zauważyć, że po zmianie stanu na zmodyfikowane wszystkie właściwości jednostki zostaną oznaczone jako zmodyfikowane, a wszystkie wartości właściwości będą wysyłane do bazy danych po wywołaniu metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="95379-156">Note that when you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  
