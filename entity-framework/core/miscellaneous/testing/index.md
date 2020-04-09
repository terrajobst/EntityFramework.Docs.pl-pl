---
title: Komponenty testowe przy użyciu EF Core - EF Core
description: Różne podejścia do testowania aplikacji korzystających z EF Core
author: ajcvickers
ms.date: 03/23/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: b1ab37ebb0a3aae09d5d5b225f746cf83dfba170
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634251"
---
# <a name="testing-code-that-uses-ef-core"></a><span data-ttu-id="0b7bf-103">Testowanie kodu, który używa EF Core</span><span class="sxs-lookup"><span data-stu-id="0b7bf-103">Testing code that uses EF Core</span></span>

<span data-ttu-id="0b7bf-104">Testowanie kodu, który uzyskuje dostęp do bazy danych wymaga albo:</span><span class="sxs-lookup"><span data-stu-id="0b7bf-104">Testing code that accesses a database requires either:</span></span>
* <span data-ttu-id="0b7bf-105">Uruchamianie zapytań i aktualizacji w tym samym systemie bazy danych, który jest używany w produkcji.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-105">Running queries and updates against the same database system used in production.</span></span>
* <span data-ttu-id="0b7bf-106">Uruchamianie zapytań i aktualizacji względem innych łatwiejszych w zarządzaniu systemem baz danych.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-106">Running queries and updates against some other easier to manage database system.</span></span>
* <span data-ttu-id="0b7bf-107">Za pomocą testu podwaja lub inny mechanizm, aby uniknąć korzystania z bazy danych w ogóle.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-107">Using test doubles or some other mechanism to avoid using a database at all.</span></span>

<span data-ttu-id="0b7bf-108">W tym dokumencie przedstawiono kompromisy związane z każdym z tych wyborów i pokazuje, jak EF Core może być używany z każdym podejściem.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-108">This document outlines the trade offs involved in each of these choices and shows how EF Core can be used with each approach.</span></span>  

## <a name="all-database-providers-are-not-equal"></a><span data-ttu-id="0b7bf-109">Wszyscy dostawcy baz danych nie są</span><span class="sxs-lookup"><span data-stu-id="0b7bf-109">All database providers are not equal</span></span>

<span data-ttu-id="0b7bf-110">Jest bardzo ważne, aby zrozumieć, że EF Core nie jest przeznaczony do abstrakcji każdy aspekt systemu baz danych.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-110">It is very important to understand that EF Core is not designed to abstract every aspect of the underlying database system.</span></span>
<span data-ttu-id="0b7bf-111">Zamiast tego EF Core jest wspólny zestaw wzorców i pojęć, które mogą być używane z dowolnego systemu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-111">Instead, EF Core is a common set of patterns and concepts that can be used with any database system.</span></span>
<span data-ttu-id="0b7bf-112">Ef Core dostawców bazy danych następnie warstwy zachowania specyficzne dla bazy danych i funkcji w tej wspólnej struktury.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-112">EF Core database providers then layer database-specific behavior and functionality over this common framework.</span></span>
<span data-ttu-id="0b7bf-113">Dzięki temu każdy system bazy danych, aby zrobić to, co robi najlepiej przy jednoczesnym zachowaniu podobieństwa, w stosownych przypadkach, z innymi systemami bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-113">This allows each database system to do what it does best while still maintaining commonality, where appropriate, with other database systems.</span></span> 

<span data-ttu-id="0b7bf-114">Zasadniczo oznacza to, że wyłączenie dostawcy bazy danych spowoduje zmianę zachowania EF Core i nie można oczekiwać, że aplikacja będzie działać poprawnie, chyba że jawnie uwzględnia wszystkie różnice w zachowaniu.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-114">Fundamentally, this means that switching out the database provider will change EF Core behavior and the application can't be expected to function correctly unless it explicitly accounts for all differences in behavior.</span></span>
<span data-ttu-id="0b7bf-115">Mając na uwadze to, w wielu przypadkach będzie to działać, ponieważ istnieje wysoki stopień podobieństwa między relacyjnymi bazami danych.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-115">That being said, in many cases doing this will work because there is a high degree of commonality amongst relational databases.</span></span>
<span data-ttu-id="0b7bf-116">To jest dobre i złe.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-116">This is good and bad.</span></span>
<span data-ttu-id="0b7bf-117">Dobrze, ponieważ przemieszczanie się między bazami danych może być stosunkowo łatwe.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-117">Good because moving between databases can be relatively easy.</span></span>
<span data-ttu-id="0b7bf-118">Źle, ponieważ może dać fałszywe poczucie bezpieczeństwa, jeśli aplikacja nie jest w pełni przetestowany przeciwko nowemu systemowi bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-118">Bad because it can give a false sense of security if the application is not fully tested against the new database system.</span></span>  

## <a name="approach-1-production-database-system"></a><span data-ttu-id="0b7bf-119">Podejście 1: System produkcyjnej bazy danych</span><span class="sxs-lookup"><span data-stu-id="0b7bf-119">Approach 1: Production database system</span></span>

<span data-ttu-id="0b7bf-120">Jak opisano w poprzedniej sekcji, jedynym sposobem, aby upewnić się, że testujesz to, co działa w produkcji jest użycie tego samego systemu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-120">As described in the previous section, the only way to be sure you are testing what runs in production is to use the same database system.</span></span>
<span data-ttu-id="0b7bf-121">Na przykład jeśli wdrożona aplikacja używa sql azure, następnie testowanie należy również wykonać przeciwko SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-121">For example, if the deployed application uses SQL Azure, then testing should also be done against SQL Azure.</span></span>

<span data-ttu-id="0b7bf-122">Jednak posiadanie wszystkich testów uruchamiania deweloperów przeciwko sql azure podczas aktywnej pracy nad kodem będzie zarówno powolne, jak i kosztowne.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-122">However, having every developer run tests against SQL Azure while actively working on the code would be both slow and expensive.</span></span>
<span data-ttu-id="0b7bf-123">Ilustruje to główny kompromis związany z tymi podejściami: kiedy należy odejść od systemu produkcyjnej bazy danych, aby poprawić wydajność testów?</span><span class="sxs-lookup"><span data-stu-id="0b7bf-123">This illustrates the main trade off involved throughout these approaches: when is it appropriate to deviate from the production database system so as to improve test efficiency?</span></span>

<span data-ttu-id="0b7bf-124">Na szczęście w tym przypadku odpowiedź jest dość prosta: użyj lokalnego lub lokalnego programu SQL Server do testowania deweloperów.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-124">Luckily, in this case the answer is quite easy: use local or on-premises SQL Server for developer testing.</span></span>
<span data-ttu-id="0b7bf-125">SQL Azure i SQL Server są bardzo podobne, więc testowanie sql server jest zwykle uzasadnione kompromisu.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-125">SQL Azure and SQL Server are extremely similar, so testing against SQL Server is usually a reasonable trade off.</span></span>
<span data-ttu-id="0b7bf-126">Mając na to na powiedziane, nadal jest mądry, aby uruchomić testy przeciwko samej platformy SQL Azure przed przejściem do produkcji.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-126">That being said, it is still wise to run tests against SQL Azure itself before going into production.</span></span>
 
### <a name="localdb"></a><span data-ttu-id="0b7bf-127">Localdb</span><span class="sxs-lookup"><span data-stu-id="0b7bf-127">LocalDb</span></span> 

<span data-ttu-id="0b7bf-128">Wszystkie główne systemy baz danych mają jakąś formę "Developer Edition" do testowania lokalnego.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-128">All the major database systems have some form of "Developer Edition" for local testing.</span></span>
<span data-ttu-id="0b7bf-129">SQL Server posiada również funkcję o nazwie [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span><span class="sxs-lookup"><span data-stu-id="0b7bf-129">SQL Server also also has a feature called [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span></span>
<span data-ttu-id="0b7bf-130">Główną zaletą LocalDb jest to, że obraca się wystąpienie bazy danych na żądanie.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-130">The primary advantage of LocalDb is that it spins up the database instance on demand.</span></span>
<span data-ttu-id="0b7bf-131">Pozwala to uniknąć usługi bazy danych uruchomionej na komputerze, nawet jeśli nie są uruchomione testy.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-131">This avoids having a database service running on your machine even when you're not running tests.</span></span>

<span data-ttu-id="0b7bf-132">LocalDb nie jest bez problemów:</span><span class="sxs-lookup"><span data-stu-id="0b7bf-132">LocalDb is not without it's issues:</span></span>
* <span data-ttu-id="0b7bf-133">Nie obsługuje wszystkiego, co robi [program SQL Server Developer Edition.](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15)</span><span class="sxs-lookup"><span data-stu-id="0b7bf-133">It doesn't support everything that [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) does.</span></span>
* <span data-ttu-id="0b7bf-134">Nie jest dostępny w systemie Linux.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-134">It isn't available on Linux.</span></span>
* <span data-ttu-id="0b7bf-135">Może to spowodować opóźnienie przy pierwszym uruchomieniu testowym, ponieważ usługa jest spun up.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-135">It can cause lag on first test run as the service is spun up.</span></span>

<span data-ttu-id="0b7bf-136">Osobiście nigdy nie znalazłem problemu z usługą bazy danych działającą na moim komputerze deweloperskim i ogólnie polecam użycie Developer Edition.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-136">Personally, I've never found it a problem having a database service running on my dev machine and I would generally recommend using Developer Edition instead.</span></span>
<span data-ttu-id="0b7bf-137">Jednak może to być odpowiednie dla niektórych osób, zwłaszcza na mniej wydajnych komputerach deweloperskich.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-137">However, it may be appropriate for some people, especially on less powerful dev machines.</span></span>  

## <a name="approach-2-sqlite"></a><span data-ttu-id="0b7bf-138">Podejście 2: SQLite</span><span class="sxs-lookup"><span data-stu-id="0b7bf-138">Approach 2: SQLite</span></span>

<span data-ttu-id="0b7bf-139">EF Core testuje dostawcę programu SQL Server przede wszystkim przez uruchomienie go w przypadku lokalnego wystąpienia programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-139">EF Core tests the SQL Server provider primarily by running it against a local SQL Server instance.</span></span>
<span data-ttu-id="0b7bf-140">Te testy uruchamiają dziesiątki tysięcy zapytań w ciągu kilku minut na szybkim komputerze.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-140">These tests run tens of thousands of queries in a couple of minutes on a fast machine.</span></span>
<span data-ttu-id="0b7bf-141">To pokazuje, że przy użyciu systemu prawdziwej bazy danych może być rozwiązaniem wydajne.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-141">This illustrates that using the real database system can be a performant solution.</span></span>
<span data-ttu-id="0b7bf-142">Jest to mit, że przy użyciu niektórych lżejszych bazy danych jest jedynym sposobem, aby szybko uruchomić testy.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-142">It is a myth that using some lighter-weight database is the only way to run tests quickly.</span></span>

<span data-ttu-id="0b7bf-143">Mając na to na myśli, co zrobić, jeśli z jakiegokolwiek powodu nie można uruchomić testy przeciwko coś blisko systemu bazy danych produkcji?</span><span class="sxs-lookup"><span data-stu-id="0b7bf-143">That being said, what if for whatever reason you can't run tests against something close to your production database system?</span></span>
<span data-ttu-id="0b7bf-144">Następnym najlepszym wyborem jest użycie czegoś o podobnej funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-144">The next best choice is to use something with similar functionality.</span></span>
<span data-ttu-id="0b7bf-145">Zwykle oznacza to inną relacyjną bazę danych, dla której [SQLite](https://sqlite.org/index.html) jest oczywistym wyborem.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-145">This usually means another relational database, for which [SQLite](https://sqlite.org/index.html) is the obvious choice.</span></span>

<span data-ttu-id="0b7bf-146">SQLite to dobry wybór, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="0b7bf-146">SQLite is a good choice because:</span></span>
* <span data-ttu-id="0b7bf-147">Działa w trakcie z aplikacją i tak ma niskie obciążenie.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-147">It runs in-process with your application and so has low overhead.</span></span>
* <span data-ttu-id="0b7bf-148">Używa prostych, automatycznie utworzonych plików dla baz danych, a więc nie wymaga zarządzania bazami danych.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-148">It uses simple, automatically created files for databases, and so doesn't require database management.</span></span>
* <span data-ttu-id="0b7bf-149">Ma tryb w pamięci, który pozwala uniknąć nawet tworzenia plików.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-149">It has an in-memory mode that avoids even the file creation.</span></span>

<span data-ttu-id="0b7bf-150">Należy jednak pamiętać, że:</span><span class="sxs-lookup"><span data-stu-id="0b7bf-150">However, remember that:</span></span>
* <span data-ttu-id="0b7bf-151">SqLite inevitability nie obsługuje wszystko, co system produkcyjnej bazy danych nie.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-151">SQLite inevitability doesn't support everything that your production database system does.</span></span>
* <span data-ttu-id="0b7bf-152">SQLite będzie zachowywać się inaczej niż system produkcyjnej bazy danych dla niektórych zapytań.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-152">SQLite will behave differently than your production database system for some queries.</span></span>

<span data-ttu-id="0b7bf-153">Więc jeśli używasz SQLite do niektórych testów, upewnij się również, aby przetestować przeciwko rzeczywistemu systemowi bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-153">So if you do use SQLite for some testing, make sure to also test against your real database system.</span></span>

<span data-ttu-id="0b7bf-154">Zobacz [Testowanie z SQLite](xref:core/miscellaneous/testing/sqlite) dla EF Core wskazówki dotyczące.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-154">See [Testing with SQLite](xref:core/miscellaneous/testing/sqlite) for EF Core specific guidance.</span></span> 

## <a name="approach-3-the-ef-core-in-memory-database"></a><span data-ttu-id="0b7bf-155">Podejście 3: Baza danych EF Core w pamięci</span><span class="sxs-lookup"><span data-stu-id="0b7bf-155">Approach 3: The EF Core in-memory database</span></span>

<span data-ttu-id="0b7bf-156">EF Core jest wyposażony w bazę danych w pamięci, której używamy do wewnętrznych testów samego ef core.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-156">EF Core comes with an in-memory database that we use for internal testing of EF Core itself.</span></span>
<span data-ttu-id="0b7bf-157">Ta baza danych na ogół **nie nadaje się jako substytut dla aplikacji testujących, które używają EF Core**.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-157">This database is in general **not suitable as a substitute for testing applications that use EF Core**.</span></span> <span data-ttu-id="0b7bf-158">Są to:</span><span class="sxs-lookup"><span data-stu-id="0b7bf-158">Specifically:</span></span>
* <span data-ttu-id="0b7bf-159">Nie jest to relacyjna baza danych</span><span class="sxs-lookup"><span data-stu-id="0b7bf-159">It is not a relational database</span></span>
* <span data-ttu-id="0b7bf-160">Nie obsługuje transakcji</span><span class="sxs-lookup"><span data-stu-id="0b7bf-160">It doesn't support transactions</span></span>
* <span data-ttu-id="0b7bf-161">Nie jest zoptymalizowany pod kątem wydajności</span><span class="sxs-lookup"><span data-stu-id="0b7bf-161">It is not optimized for performance</span></span>

<span data-ttu-id="0b7bf-162">Nic z tego nie jest bardzo ważne podczas testowania ef core wewnętrznych, ponieważ używamy go w szczególności, gdzie baza danych jest bez znaczenia dla testu.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-162">None of this is very important when testing EF Core internals because we use it specifically where the database is irrelevant to the test.</span></span>
<span data-ttu-id="0b7bf-163">Z drugiej strony te rzeczy wydają się być bardzo ważne podczas testowania aplikacji, która używa EF Core.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-163">On the other hand, these things tend to be very important when testing an application that uses EF Core.</span></span>

## <a name="unit-testing"></a><span data-ttu-id="0b7bf-164">Testowanie jednostek</span><span class="sxs-lookup"><span data-stu-id="0b7bf-164">Unit testing</span></span>

<span data-ttu-id="0b7bf-165">Należy wziąć pod uwagę testowanie fragmentu logiki biznesowej, który może być konieczne użycie niektórych danych z bazy danych, ale nie jest z natury testowania interakcji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-165">Consider testing a piece of business logic that might need to use some data from a database, but is not inherently testing the database interactions.</span></span>
<span data-ttu-id="0b7bf-166">Jedną z opcji jest użycie [podwójnego testu,](https://en.wikipedia.org/wiki/Test_double) takiego jak makieta lub podróbka.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-166">One option is to use a [test double](https://en.wikipedia.org/wiki/Test_double) such as a mock or fake.</span></span>

<span data-ttu-id="0b7bf-167">Używamy podwaja testu do wewnętrznych testów EF Core.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-167">We use test doubles for internal testing of EF Core.</span></span>
<span data-ttu-id="0b7bf-168">Jednak nigdy nie staramy się wyśmiewać DbContext lub IQueryable.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-168">However, we never try to mock DbContext or IQueryable.</span></span>
<span data-ttu-id="0b7bf-169">Jest to trudne, uciążliwe i kruche.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-169">Doing so is difficult, cumbersome, and fragile.</span></span>
<span data-ttu-id="0b7bf-170">**Nie rób tego.**</span><span class="sxs-lookup"><span data-stu-id="0b7bf-170">**Don't do it.**</span></span>

<span data-ttu-id="0b7bf-171">Zamiast tego używamy bazy danych w pamięci podczas testowania jednostki coś, co używa DbContext.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-171">Instead we use the in-memory database when unit testing something that uses DbContext.</span></span>
<span data-ttu-id="0b7bf-172">W takim przypadku przy użyciu bazy danych w pamięci jest właściwe, ponieważ test nie jest zależny od zachowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-172">In this case using the in-memory database is appropriate because the test is not dependent on database behavior.</span></span>
<span data-ttu-id="0b7bf-173">Po prostu nie rób tego, aby przetestować rzeczywiste zapytania bazy danych lub aktualizacje.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-173">Just don't do this to test actual database queries or updates.</span></span>   

<span data-ttu-id="0b7bf-174">Zobacz [Testowanie z dostawcą w pamięci](xref:core/miscellaneous/testing/in-memory) ef core wskazówki dotyczące korzystania z bazy danych w pamięci do testowania jednostek.</span><span class="sxs-lookup"><span data-stu-id="0b7bf-174">See [Testing with the in-memory provider](xref:core/miscellaneous/testing/in-memory) for EF Core specific guidance on using the in-memory database for unit testing.</span></span>
