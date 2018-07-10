---
title: Pierwotne zapytania SQL - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
caps.latest.revision: 3
ms.openlocfilehash: 1d968604cfa500784c4699b0923512572a06d773
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912737"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="d2f95-102">Pierwotne zapytania SQL</span><span class="sxs-lookup"><span data-stu-id="d2f95-102">Raw SQL Queries</span></span>
<span data-ttu-id="d2f95-103">Platformy Entity Framework umożliwia tworzenie zapytań za pomocą LINQ z Twoich zajęciach jednostki.</span><span class="sxs-lookup"><span data-stu-id="d2f95-103">Entity Framework allows you to query using LINQ with your entity classes.</span></span> <span data-ttu-id="d2f95-104">Jednak może być razy, które chcesz uruchamiać zapytania przy użyciu surowego SQL bezpośrednio w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d2f95-104">However, there may be times that you want to run queries using raw SQL directly against the database.</span></span> <span data-ttu-id="d2f95-105">W tym wywołaniem procedury składowane, które mogą być przydatne w przypadku Code First modeli, które aktualnie nie obsługują mapowanie procedur składowanych.</span><span class="sxs-lookup"><span data-stu-id="d2f95-105">This includes calling stored procedures, which can be helpful for Code First models that currently do not support mapping to stored procedures.</span></span> <span data-ttu-id="d2f95-106">Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="d2f95-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="writing-sql-queries-for-entities"></a><span data-ttu-id="d2f95-107">Pisanie zapytań SQL dla jednostek</span><span class="sxs-lookup"><span data-stu-id="d2f95-107">Writing SQL queries for entities</span></span>  

<span data-ttu-id="d2f95-108">Metoda SqlQuery na DbSet umożliwia pierwotne zapytania SQL, który ma zostać zapisany, który zwróci wystąpień jednostek.</span><span class="sxs-lookup"><span data-stu-id="d2f95-108">The SqlQuery method on DbSet allows a raw SQL query to be written that will return entity instances.</span></span> <span data-ttu-id="d2f95-109">Zwracanych obiektów będą śledzone przez kontekst, tak jak powinny, jeśli zostały zwrócone przez zapytanie LINQ.</span><span class="sxs-lookup"><span data-stu-id="d2f95-109">The returned objects will be tracked by the context just as they would be if they were returned by a LINQ query.</span></span> <span data-ttu-id="d2f95-110">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d2f95-110">For example:</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="d2f95-111">Należy pamiętać, że podobnie jak w przypadku zapytań LINQ, zapytanie nie jest wykonywany do momentu wyliczane są wyniki — w powyższym przykładzie jest to zrobić za pomocą wywołania tolist —.</span><span class="sxs-lookup"><span data-stu-id="d2f95-111">Note that, just as for LINQ queries, the query is not executed until the results are enumerated—in the example above this is done with the call to ToList.</span></span>  

<span data-ttu-id="d2f95-112">Należy zachować ostrożność przy każdym pierwotne zapytania SQL są zapisywane w dwóch powodów.</span><span class="sxs-lookup"><span data-stu-id="d2f95-112">Care should be taken whenever raw SQL queries are written for two reasons.</span></span> <span data-ttu-id="d2f95-113">Po pierwsze zapytanie mają być zapisywane upewnij się, że tylko zwraca jednostek, które są naprawdę żądanego typu.</span><span class="sxs-lookup"><span data-stu-id="d2f95-113">First, the query should be written to ensure that it only returns entities that are really of the requested type.</span></span> <span data-ttu-id="d2f95-114">Na przykład podczas korzystania z funkcji, takich jak dziedziczenie jest łatwo można napisać zapytanie, które spowoduje utworzenie jednostek, które są nieprawidłowego typu CLR.</span><span class="sxs-lookup"><span data-stu-id="d2f95-114">For example, when using features such as inheritance it is easy to write a query that will create entities that are of the wrong CLR type.</span></span>  

<span data-ttu-id="d2f95-115">Po drugie niektóre typy pierwotne zapytania SQL ujawnia potencjalne zagrożenia bezpieczeństwa, zwłaszcza w części dotyczącej ataki przez wstrzyknięcie kodu SQL.</span><span class="sxs-lookup"><span data-stu-id="d2f95-115">Second, some types of raw SQL query expose potential security risks, especially around SQL injection attacks.</span></span> <span data-ttu-id="d2f95-116">Upewnij się, że użyto parametrów w zapytaniu w prawidłowy sposób, aby zabezpieczyć się przed takimi atakami.</span><span class="sxs-lookup"><span data-stu-id="d2f95-116">Make sure that you use parameters in your query in the correct way to guard against such attacks.</span></span>  

### <a name="loading-entities-from-stored-procedures"></a><span data-ttu-id="d2f95-117">Trwa ładowanie jednostek z procedur składowanych</span><span class="sxs-lookup"><span data-stu-id="d2f95-117">Loading entities from stored procedures</span></span>  

<span data-ttu-id="d2f95-118">DbSet.SqlQuery służy do ładowania jednostki w wynikach procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="d2f95-118">You can use DbSet.SqlQuery to load entities from the results of a stored procedure.</span></span> <span data-ttu-id="d2f95-119">Na przykład poniższy kod wywołuje dbo. Procedura GetBlogs w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="d2f95-119">For example, the following code calls the dbo.GetBlogs procedure in the database:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

<span data-ttu-id="d2f95-120">Parametry można też przekazać do procedury składowanej przy użyciu następującej składni:</span><span class="sxs-lookup"><span data-stu-id="d2f95-120">You can also pass parameters to a stored procedure using the following syntax:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a><span data-ttu-id="d2f95-121">Pisanie zapytań SQL dla typów innych niż jednostek</span><span class="sxs-lookup"><span data-stu-id="d2f95-121">Writing SQL queries for non-entity types</span></span>  

<span data-ttu-id="d2f95-122">Zapytanie SQL zwraca wystąpień dowolnego typu, w tym typów pierwotnych, mogą być tworzone przy użyciu metody SqlQuery klasy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d2f95-122">A SQL query returning instances of any type, including primitive types, can be created using the SqlQuery method on the Database class.</span></span> <span data-ttu-id="d2f95-123">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d2f95-123">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="d2f95-124">Kontekst nigdy nie będą śledzone wyniki zwrócone z SqlQuery w bazie danych, nawet jeśli obiekty są wystąpieniami typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="d2f95-124">The results returned from SqlQuery on Database will never be tracked by the context even if the objects are instances of an entity type.</span></span>  

## <a name="sending-raw-commands-to-the-database"></a><span data-ttu-id="d2f95-125">Wysyłania poleceń pierwotnych do bazy danych</span><span class="sxs-lookup"><span data-stu-id="d2f95-125">Sending raw commands to the database</span></span>  

<span data-ttu-id="d2f95-126">W poleceniach zapytań nie mogą być wysyłane do bazy danych przy użyciu metody ExecuteSqlCommand w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d2f95-126">Non-query commands can be sent to the database using the ExecuteSqlCommand method on Database.</span></span> <span data-ttu-id="d2f95-127">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d2f95-127">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

<span data-ttu-id="d2f95-128">Należy zauważyć, że wszelkie zmiany wprowadzone do danych w bazie danych przy użyciu ExecuteSqlCommand nieprzezroczysta dla kontekstu aż do jednostek są załadowane lub ponownie załadować z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d2f95-128">Note that any changes made to data in the database using ExecuteSqlCommand are opaque to the context until entities are loaded or reloaded from the database.</span></span>  

### <a name="output-parameters"></a><span data-ttu-id="d2f95-129">Parametry wyjściowe</span><span class="sxs-lookup"><span data-stu-id="d2f95-129">Output Parameters</span></span>  

<span data-ttu-id="d2f95-130">Jeśli używane są parametry wyjściowe, ich wartości nie będzie dostępne, dopóki wyniki zostały całkowicie odczytane.</span><span class="sxs-lookup"><span data-stu-id="d2f95-130">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="d2f95-131">Jest to spowodowane bazowego zachowanie obiekt DbDataReader, zobacz [podczas pobierania danych przy użyciu elementu DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="d2f95-131">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>  
