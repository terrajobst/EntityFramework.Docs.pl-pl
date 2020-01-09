---
title: Typy jednostek — EF Core
description: Jak skonfigurować i zmapować typy jednostek przy użyciu Entity Framework Core
author: roji
ms.date: 12/03/2019
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/entity-types
ms.openlocfilehash: b3d9ad753637d021d9aa52965da38091ae690f77
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502454"
---
# <a name="entity-types"></a><span data-ttu-id="3a0ac-103">Typy jednostek</span><span class="sxs-lookup"><span data-stu-id="3a0ac-103">Entity Types</span></span>

<span data-ttu-id="3a0ac-104">Uwzględnienie Nieogólnymi typu w Twoim kontekście oznacza, że jest ona uwzględniona w modelu EF Core; zwykle odwołujemy się do takiego typu jako *jednostki*.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-104">Including a DbSet of a type on your context means that it is included in EF Core's model; we usually refer to such a type as an *entity*.</span></span> <span data-ttu-id="3a0ac-105">EF Core może odczytywać i zapisywać wystąpienia jednostek z/do bazy danych, a jeśli używana jest relacyjna baza danych, EF Core może tworzyć tabele dla jednostek za pośrednictwem migracji.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-105">EF Core can read and write entity instances from/to the database, and if you're using a relational database, EF Core can create tables for your entities via migrations.</span></span>

## <a name="including-types-in-the-model"></a><span data-ttu-id="3a0ac-106">Uwzględnianie typów w modelu</span><span class="sxs-lookup"><span data-stu-id="3a0ac-106">Including types in the model</span></span>

<span data-ttu-id="3a0ac-107">Według Konwencji typy, które są ujawniane we właściwościach Nieogólnymi w kontekście, są uwzględniane w modelu jako jednostki.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-107">By convention, types that are exposed in DbSet properties on your context are included in the model as entities.</span></span> <span data-ttu-id="3a0ac-108">Typy jednostek, które są określone w metodzie `OnModelCreating` są również uwzględniane, podobnie jak wszystkie typy, które są znalezione przez rekursywnie Eksplorowanie właściwości nawigacji innych wykrytych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-108">Entity types that are specified in the `OnModelCreating` method are also included, as are any types that are found by recursively exploring the navigation properties of other discovered entity types.</span></span>

<span data-ttu-id="3a0ac-109">W poniższym przykładzie kodu są uwzględniane wszystkie typy:</span><span class="sxs-lookup"><span data-stu-id="3a0ac-109">In the code sample below, all types are included:</span></span>

* <span data-ttu-id="3a0ac-110">`Blog` jest uwzględniona, ponieważ jest uwidoczniona we właściwości Nieogólnymi kontekstu.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-110">`Blog` is included because it's exposed in a DbSet property on the context.</span></span>
* <span data-ttu-id="3a0ac-111">`Post` jest dołączana, ponieważ została odnaleziona za pośrednictwem `Blog.Posts` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-111">`Post` is included because it's discovered via the `Blog.Posts` navigation property.</span></span>
* <span data-ttu-id="3a0ac-112">`AuditEntry`, ponieważ jest określony w `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-112">`AuditEntry` because it is specified in `OnModelCreating`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a><span data-ttu-id="3a0ac-113">Wykluczanie typów z modelu</span><span class="sxs-lookup"><span data-stu-id="3a0ac-113">Excluding types from the model</span></span>

<span data-ttu-id="3a0ac-114">Jeśli nie chcesz dołączać typu do modelu, możesz go wykluczyć:</span><span class="sxs-lookup"><span data-stu-id="3a0ac-114">If you don't want a type to be included in the model, you can exclude it:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="3a0ac-115">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="3a0ac-115">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="3a0ac-116">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="3a0ac-116">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

## <a name="table-name"></a><span data-ttu-id="3a0ac-117">Nazwa tabeli</span><span class="sxs-lookup"><span data-stu-id="3a0ac-117">Table name</span></span>

<span data-ttu-id="3a0ac-118">Według Konwencji każdy typ jednostki zostanie skonfigurowany do mapowania do tabeli bazy danych o tej samej nazwie, co Właściwość Nieogólnymi, która uwidacznia jednostkę.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-118">By convention, each entity type will be set up to map to a database table with the same name as the DbSet property that exposes the entity.</span></span> <span data-ttu-id="3a0ac-119">Jeśli dla danej jednostki nie istnieje Nieogólnymi, używana jest nazwa klasy.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-119">If no DbSet exists for the given entity, the class name is used.</span></span>

<span data-ttu-id="3a0ac-120">Możesz ręcznie skonfigurować nazwę tabeli:</span><span class="sxs-lookup"><span data-stu-id="3a0ac-120">You can manually configure the table name:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="3a0ac-121">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="3a0ac-121">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="3a0ac-122">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="3a0ac-122">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

***

## <a name="table-schema"></a><span data-ttu-id="3a0ac-123">Schemat tabeli</span><span class="sxs-lookup"><span data-stu-id="3a0ac-123">Table schema</span></span>

<span data-ttu-id="3a0ac-124">W przypadku korzystania z relacyjnej bazy danych tabele są według Konwencji utworzonych w domyślnym schemacie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-124">When using a relational database, tables are by convention created in your database's default schema.</span></span> <span data-ttu-id="3a0ac-125">Na przykład Microsoft SQL Server będzie używać schematu `dbo` (SQLite nie obsługuje schematów).</span><span class="sxs-lookup"><span data-stu-id="3a0ac-125">For example, Microsoft SQL Server will use the `dbo` schema (SQLite does not support schemas).</span></span>

<span data-ttu-id="3a0ac-126">Można skonfigurować tabele do utworzenia w określonym schemacie w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3a0ac-126">You can configure tables to be created in a specific schema as follows:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="3a0ac-127">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="3a0ac-127">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="3a0ac-128">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="3a0ac-128">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

***

<span data-ttu-id="3a0ac-129">Zamiast określać schemat dla każdej tabeli, można również zdefiniować domyślny schemat na poziomie modelu przy użyciu interfejsu API Fluent:</span><span class="sxs-lookup"><span data-stu-id="3a0ac-129">Rather than specifying the schema for each table, you can also define the default schema at the model level with the fluent API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

<span data-ttu-id="3a0ac-130">Należy zauważyć, że ustawienie domyślny schemat wpłynie również na inne obiekty bazy danych, takie jak sekwencje.</span><span class="sxs-lookup"><span data-stu-id="3a0ac-130">Note that setting the default schema will also affect other database objects, such as sequences.</span></span>
