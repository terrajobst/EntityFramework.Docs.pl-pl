---
title: Entity Framework słownik - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
ms.openlocfilehash: 4ad56c4d655e004d97c3537707fa6b13c7acf88e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997722"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="1f3c2-102">Entity Framework słownik</span><span class="sxs-lookup"><span data-stu-id="1f3c2-102">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="1f3c2-103">Najpierw kod</span><span class="sxs-lookup"><span data-stu-id="1f3c2-103">Code First</span></span>
<span data-ttu-id="1f3c2-104">Tworzenie modelu Entity Framework przy użyciu kodu.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-104">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="1f3c2-105">Można wskazać modelu i istniejącej lub nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-105">The model can target and existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="1f3c2-106">Kontekst</span><span class="sxs-lookup"><span data-stu-id="1f3c2-106">Context</span></span>
<span data-ttu-id="1f3c2-107">Klasa, która reprezentuje sesję z bazą danych, dzięki czemu możesz do zapytania i Zapisz dane.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-107">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="1f3c2-108">Kontekst jest pochodną klasy DbContext lub obiektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-108">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="1f3c2-109">Konwencja (kod: pierwszej)</span><span class="sxs-lookup"><span data-stu-id="1f3c2-109">Convention (Code First)</span></span>
<span data-ttu-id="1f3c2-110">Reguła, która korzysta z programu Entity Framework wywnioskowania kształt modelu z klas.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-110">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="1f3c2-111">Najpierw bazy danych</span><span class="sxs-lookup"><span data-stu-id="1f3c2-111">Database First</span></span>
<span data-ttu-id="1f3c2-112">Tworzenie modelu Entity Framework, za pomocą projektanta EF, który wspiera istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-112">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="1f3c2-113">Wczesne ładowanie</span><span class="sxs-lookup"><span data-stu-id="1f3c2-113">Eager loading</span></span>
<span data-ttu-id="1f3c2-114">Wzorzec załadunku, powiązanych danych, gdzie zapytanie dla jednego typu obiektu również ładuje powiązanych jednostek jako część zapytania.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-114">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="1f3c2-115">Projektancie platformy EF</span><span class="sxs-lookup"><span data-stu-id="1f3c2-115">EF Designer</span></span>
<span data-ttu-id="1f3c2-116">Projektant wizualny w programie Visual Studio umożliwia utworzenie modelu Entity Framework, używając okna i wierszy.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-116">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="1f3c2-117">Jednostka</span><span class="sxs-lookup"><span data-stu-id="1f3c2-117">Entity</span></span>
<span data-ttu-id="1f3c2-118">Klasa lub obiekt, który reprezentuje dane aplikacji, takich jak klientów, produkty i zamówienia.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-118">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="1f3c2-119">Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="1f3c2-119">Entity Data Model</span></span>
<span data-ttu-id="1f3c2-120">Model, który opisuje jednostek i relacji między nimi.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-120">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="1f3c2-121">EF używa EDM do opisu modelu koncepcyjnego, względem którego programy dla deweloperów.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-121">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="1f3c2-122">EDM opiera się na modelu Relacja jednostki, wynikające z odzyskiwania po awarii. Peter Chen.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-122">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="1f3c2-123">Podstawowym celem staje się wspólnego modelu danych na zestaw technologii firmy Microsoft dla deweloperów i serwer został pierwotnie opracowana EDM.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-123">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="1f3c2-124">EDM jest również używane jako część protokołu OData.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-124">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="1f3c2-125">jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="1f3c2-125">Explicit loading</span></span>
<span data-ttu-id="1f3c2-126">Wzorzec załadunku, powiązanych danych, gdzie obiekty powiązane są ładowane przez wywołanie interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-126">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1f3c2-127">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="1f3c2-127">Fluent API</span></span>
<span data-ttu-id="1f3c2-128">Interfejs API, który może służyć do konfigurowania model Code First.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-128">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="1f3c2-129">Skojarzenie klucza obcego</span><span class="sxs-lookup"><span data-stu-id="1f3c2-129">Foreign key association</span></span>
<span data-ttu-id="1f3c2-130">Skojarzenia między jednostkami, której właściwość, która reprezentuje klucz obcy znajduje się w klasie jednostki zależne.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-130">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="1f3c2-131">Na przykład produkt zawiera właściwość CategoryId.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-131">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="1f3c2-132">Identyfikowanie relacji</span><span class="sxs-lookup"><span data-stu-id="1f3c2-132">Identifying relationship</span></span>
<span data-ttu-id="1f3c2-133">Relacja, w którym klucz podstawowy jednostki głównej jest częścią klucza podstawowego jednostki zależne.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-133">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="1f3c2-134">W tego rodzaju relacji jednostki zależne nie może istnieć bez jednostki głównej.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-134">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="1f3c2-135">Niezależnie od skojarzenia</span><span class="sxs-lookup"><span data-stu-id="1f3c2-135">Independent association</span></span>
<span data-ttu-id="1f3c2-136">Skojarzenia między jednostkami w przypadku, gdy nie ma właściwości reprezentujący klucz obcy w klasie jednostki zależne.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-136">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="1f3c2-137">Na przykład klasa produktu zawiera relację z kategorii, ale nie ma właściwości CategoryId.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-137">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="1f3c2-138">Entity Framework śledzi stan skojarzenia, niezależnie od stanu jednostek końców dwóch skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-138">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="1f3c2-139">Ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="1f3c2-139">Lazy loading</span></span>
<span data-ttu-id="1f3c2-140">Wzorzec załadunku, powiązanych danych, gdzie obiekty powiązane są ładowane automatycznie podczas uzyskiwania dostępu do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-140">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="1f3c2-141">Najpierw modelu</span><span class="sxs-lookup"><span data-stu-id="1f3c2-141">Model First</span></span>
<span data-ttu-id="1f3c2-142">Tworzenie modelu Entity Framework, za pomocą projektanta EF następnie używany do tworzenia nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-142">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="1f3c2-143">Właściwość nawigacji</span><span class="sxs-lookup"><span data-stu-id="1f3c2-143">Navigation property</span></span>
<span data-ttu-id="1f3c2-144">Właściwość obiektu, który odwołuje się do innej jednostki.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-144">A property of an entity that references another entity.</span></span> <span data-ttu-id="1f3c2-145">Na przykład produkt zawiera właściwość nawigacji kategorii i kategoria zawiera właściwość nawigacji produktów.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-145">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="1f3c2-146">OBIEKTÓW POCO</span><span class="sxs-lookup"><span data-stu-id="1f3c2-146">POCO</span></span>
<span data-ttu-id="1f3c2-147">Akronim obiektu CLR zwykły stary.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-147">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="1f3c2-148">Klasa prostych użytkownika, która nie ma zależności za pomocą dowolnej platformy.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-148">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="1f3c2-149">W kontekście EF Klasa jednostki, która nie pochodzi od EntityObject, implementuje żadnych interfejsów lub niesie ze sobą wszelkie atrybuty zdefiniowane w programie EF.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-149">In the context of EF, a an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="1f3c2-150">Takie klasy jednostek, które są całkowicie niezależni od framework trwałości są również określane jako "trwałość zakresu".</span><span class="sxs-lookup"><span data-stu-id="1f3c2-150">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="1f3c2-151">Odwrotne relacji</span><span class="sxs-lookup"><span data-stu-id="1f3c2-151">Relationship inverse</span></span>
<span data-ttu-id="1f3c2-152">Przeciwległym końcu relacji, na przykład produktu. Kategoria i kategorii. Produkt.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-152">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="1f3c2-153">Samodzielnie śledzenia jednostki</span><span class="sxs-lookup"><span data-stu-id="1f3c2-153">Self-tracking entity</span></span>
<span data-ttu-id="1f3c2-154">Jednostka utworzona na podstawie szablonu generowania kodu, który pomaga w rozwoju N-warstwowej.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-154">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="1f3c2-155">Typ tabeli na konkretny (TPC)</span><span class="sxs-lookup"><span data-stu-id="1f3c2-155">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="1f3c2-156">Metoda mapowania dziedziczenia, gdzie każdy typ nieabstrakcyjnej w hierarchii jest mapowany do oddzielnych tabel w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-156">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="1f3c2-157">Tabela wg hierarchii (TPH)</span><span class="sxs-lookup"><span data-stu-id="1f3c2-157">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="1f3c2-158">Metoda mapowania dziedziczenia, w którym wszystkie typy w hierarchii są mapowane na tej samej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-158">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="1f3c2-159">Dyskryminatora kolumny jest używana do identyfikowania, jaki typ każdy wiersz jest skojarzony.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-159">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="1f3c2-160">Tabela wg typu (TPT)</span><span class="sxs-lookup"><span data-stu-id="1f3c2-160">Table-per-type (TPT)</span></span>
<span data-ttu-id="1f3c2-161">Metoda mapowania dziedziczenia, gdzie wspólne właściwości wszystkich typów w hierarchii są mapowane na tej samej tabeli w bazie danych, ale unikatowe dla każdego typu właściwości są mapowane na osobnej tabeli.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-161">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="1f3c2-162">Typ odnajdywania</span><span class="sxs-lookup"><span data-stu-id="1f3c2-162">Type discovery</span></span>
<span data-ttu-id="1f3c2-163">Proces identyfikacji typów, które powinny być częścią modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1f3c2-163">The process of identifying the types that should be part of an Entity Framework model.</span></span>
