---
title: Słownik Entity Framework — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: df0da4a68b3d2c882d9673417ee5fe335eccae2b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656159"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="2d816-102">Entity Framework słownik</span><span class="sxs-lookup"><span data-stu-id="2d816-102">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="2d816-103">Code First</span><span class="sxs-lookup"><span data-stu-id="2d816-103">Code First</span></span>
<span data-ttu-id="2d816-104">Tworzenie modelu Entity Framework przy użyciu kodu.</span><span class="sxs-lookup"><span data-stu-id="2d816-104">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="2d816-105">Model może kierować do istniejącej bazy danych lub nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d816-105">The model can target an existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="2d816-106">Context</span><span class="sxs-lookup"><span data-stu-id="2d816-106">Context</span></span>
<span data-ttu-id="2d816-107">Klasa, która reprezentuje sesję z bazą danych, umożliwiając wykonywanie zapytań i zapisywanie danych.</span><span class="sxs-lookup"><span data-stu-id="2d816-107">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="2d816-108">Kontekst pochodzi z klasy DbContext lub ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="2d816-108">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="2d816-109">Konwencja (Code First)</span><span class="sxs-lookup"><span data-stu-id="2d816-109">Convention (Code First)</span></span>
<span data-ttu-id="2d816-110">Reguła, która Entity Framework używa do wnioskowania kształtu modelu z klas.</span><span class="sxs-lookup"><span data-stu-id="2d816-110">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="2d816-111">Database First</span><span class="sxs-lookup"><span data-stu-id="2d816-111">Database First</span></span>
<span data-ttu-id="2d816-112">Tworzenie modelu Entity Framework przy użyciu projektanta EF, który jest przeznaczony dla istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d816-112">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="2d816-113">Ładowanie eager</span><span class="sxs-lookup"><span data-stu-id="2d816-113">Eager loading</span></span>
<span data-ttu-id="2d816-114">Wzorzec ładowania powiązanych danych, gdzie zapytanie dla jednego typu jednostki również ładuje powiązane jednostki jako część zapytania.</span><span class="sxs-lookup"><span data-stu-id="2d816-114">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="2d816-115">Projektant EF</span><span class="sxs-lookup"><span data-stu-id="2d816-115">EF Designer</span></span>
<span data-ttu-id="2d816-116">Projektant wizualny w programie Visual Studio, który umożliwia tworzenie modelu Entity Framework przy użyciu pól i wierszy.</span><span class="sxs-lookup"><span data-stu-id="2d816-116">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="2d816-117">Jednostka</span><span class="sxs-lookup"><span data-stu-id="2d816-117">Entity</span></span>
<span data-ttu-id="2d816-118">Klasa lub obiekt reprezentujący dane aplikacji, takie jak klienci, produkty i zamówienia.</span><span class="sxs-lookup"><span data-stu-id="2d816-118">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="2d816-119">Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="2d816-119">Entity Data Model</span></span>
<span data-ttu-id="2d816-120">Model, który opisuje jednostki i relacje między nimi.</span><span class="sxs-lookup"><span data-stu-id="2d816-120">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="2d816-121">Dr używa modelu EDM do opisywania model koncepcyjny, dla którego programy deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="2d816-121">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="2d816-122">Program EDM kompiluje w modelu relacji jednostki wprowadzonym przez Dr. Peterowi Chen.</span><span class="sxs-lookup"><span data-stu-id="2d816-122">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="2d816-123">Model EDM został pierwotnie opracowany z myślą o podstawowym celu przetworzenia wspólnego modelu danych w ramach zestawu technologii deweloperskich i serwerowych firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2d816-123">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="2d816-124">EDM jest również używany jako część protokołu OData.</span><span class="sxs-lookup"><span data-stu-id="2d816-124">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="2d816-125">Jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="2d816-125">Explicit loading</span></span>
<span data-ttu-id="2d816-126">Wzorzec ładowania powiązanych danych w przypadku ładowania powiązanych obiektów przez wywołanie interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="2d816-126">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2d816-127">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="2d816-127">Fluent API</span></span>
<span data-ttu-id="2d816-128">Interfejs API, który może służyć do konfigurowania modelu Code First.</span><span class="sxs-lookup"><span data-stu-id="2d816-128">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="2d816-129">Skojarzenie klucza obcego</span><span class="sxs-lookup"><span data-stu-id="2d816-129">Foreign key association</span></span>
<span data-ttu-id="2d816-130">Skojarzenie między jednostkami, w których właściwość reprezentująca klucz obcy jest uwzględniona w klasie jednostki zależnej.</span><span class="sxs-lookup"><span data-stu-id="2d816-130">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="2d816-131">Na przykład produkt zawiera właściwość IDKategorii.</span><span class="sxs-lookup"><span data-stu-id="2d816-131">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="2d816-132">Identyfikowanie relacji</span><span class="sxs-lookup"><span data-stu-id="2d816-132">Identifying relationship</span></span>
<span data-ttu-id="2d816-133">Relacja, w której klucz podstawowy jednostki głównej jest częścią klucza podstawowego jednostki zależnej.</span><span class="sxs-lookup"><span data-stu-id="2d816-133">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="2d816-134">W tym rodzaju relacji jednostka zależna nie może istnieć bez jednostki podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="2d816-134">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="2d816-135">Niezależne skojarzenie</span><span class="sxs-lookup"><span data-stu-id="2d816-135">Independent association</span></span>
<span data-ttu-id="2d816-136">Skojarzenie między jednostkami, w których nie ma właściwości reprezentującej klucz obcy w klasie jednostki zależnej.</span><span class="sxs-lookup"><span data-stu-id="2d816-136">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="2d816-137">Na przykład Klasa produktu zawiera relację do kategorii, ale nie ma właściwości IDKategorii.</span><span class="sxs-lookup"><span data-stu-id="2d816-137">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="2d816-138">Entity Framework śledzi stan skojarzenia niezależnie od stanu jednostek na dwa punkty końcowe skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="2d816-138">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="2d816-139">Ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="2d816-139">Lazy loading</span></span>
<span data-ttu-id="2d816-140">Wzorzec ładowania powiązanych danych, w przypadku których obiekty powiązane są automatycznie ładowane podczas uzyskiwania dostępu do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="2d816-140">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="2d816-141">Model First</span><span class="sxs-lookup"><span data-stu-id="2d816-141">Model First</span></span>
<span data-ttu-id="2d816-142">Tworzenie modelu Entity Framework przy użyciu programu EF Designer, który jest następnie używany do tworzenia nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d816-142">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="2d816-143">Właściwość nawigacji</span><span class="sxs-lookup"><span data-stu-id="2d816-143">Navigation property</span></span>
<span data-ttu-id="2d816-144">Właściwość jednostki, która odwołuje się do innej jednostki.</span><span class="sxs-lookup"><span data-stu-id="2d816-144">A property of an entity that references another entity.</span></span> <span data-ttu-id="2d816-145">Na przykład produkt zawiera właściwość nawigacji kategorii, a kategoria zawiera produkty właściwość nawigacji.</span><span class="sxs-lookup"><span data-stu-id="2d816-145">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="2d816-146">POCO</span><span class="sxs-lookup"><span data-stu-id="2d816-146">POCO</span></span>
<span data-ttu-id="2d816-147">Akronim dla zwykłego starego obiektu CLR.</span><span class="sxs-lookup"><span data-stu-id="2d816-147">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="2d816-148">Prosta Klasa użytkownika, która nie ma żadnych zależności z żadną strukturą.</span><span class="sxs-lookup"><span data-stu-id="2d816-148">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="2d816-149">W kontekście EF Klasa jednostki, która nie pochodzi z obiektu EntityObject, implementuje interfejsy lub ma atrybuty zdefiniowane w EF.</span><span class="sxs-lookup"><span data-stu-id="2d816-149">In the context of EF, an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="2d816-150">Takie klasy jednostek, które są oddzielone od struktury trwałości, są również określane jako "trwałość ignorujących".</span><span class="sxs-lookup"><span data-stu-id="2d816-150">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="2d816-151">Odwracanie relacji</span><span class="sxs-lookup"><span data-stu-id="2d816-151">Relationship inverse</span></span>
<span data-ttu-id="2d816-152">Odwrotny koniec relacji, na przykład produkt. Kategoria i Kategoria. Iloczyn.</span><span class="sxs-lookup"><span data-stu-id="2d816-152">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="2d816-153">Jednostka samośledzenia</span><span class="sxs-lookup"><span data-stu-id="2d816-153">Self-tracking entity</span></span>
<span data-ttu-id="2d816-154">Jednostka utworzona na podstawie szablonu generowania kodu, która ułatwia tworzenie aplikacji N-warstwowych.</span><span class="sxs-lookup"><span data-stu-id="2d816-154">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="2d816-155">Typ z tabeli na konkretny (TPC)</span><span class="sxs-lookup"><span data-stu-id="2d816-155">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="2d816-156">Metoda mapowania dziedziczenia, gdzie każdy nieabstrakcyjny typ w hierarchii jest mapowany do oddzielnej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2d816-156">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="2d816-157">Tabela na hierarchię (TPH)</span><span class="sxs-lookup"><span data-stu-id="2d816-157">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="2d816-158">Metoda mapowania dziedziczenia, gdzie wszystkie typy w hierarchii są mapowane na tę samą tabelę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2d816-158">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="2d816-159">Kolumna rozróżniacza służy do identyfikowania typu, z którym jest skojarzony każdy wiersz.</span><span class="sxs-lookup"><span data-stu-id="2d816-159">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="2d816-160">Tabela na typ (TPT)</span><span class="sxs-lookup"><span data-stu-id="2d816-160">Table-per-type (TPT)</span></span>
<span data-ttu-id="2d816-161">Metoda mapowania dziedziczenia, gdzie wspólne właściwości wszystkich typów w hierarchii są mapowane na tę samą tabelę w bazie danych, ale właściwości unikatowe dla każdego typu są mapowane na osobną tabelę.</span><span class="sxs-lookup"><span data-stu-id="2d816-161">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="2d816-162">Odnajdowanie typów</span><span class="sxs-lookup"><span data-stu-id="2d816-162">Type discovery</span></span>
<span data-ttu-id="2d816-163">Proces identyfikowania typów, które powinny być częścią modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2d816-163">The process of identifying the types that should be part of an Entity Framework model.</span></span>
