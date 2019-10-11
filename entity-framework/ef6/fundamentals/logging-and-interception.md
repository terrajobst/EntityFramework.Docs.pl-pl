---
title: Rejestrowanie i przechwytywanie operacji bazy danych — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
ms.openlocfilehash: 35b0284a5ad8b2b732f074589bd458d243312575
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181657"
---
# <a name="logging-and-intercepting-database-operations"></a>Rejestrowanie i przechwytywanie operacji bazy danych
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.  

Począwszy od Entity Framework 6, w dowolnym momencie Entity Framework wysyła polecenie do bazy danych to polecenie może zostać przechwycone przez kod aplikacji. Jest to najczęściej używane do rejestrowania SQL, ale może być również używane do modyfikowania lub przerywania polecenia.  

W każdym przypadku EF obejmuje:  

- Właściwość dziennika dla kontekstu podobnego do DataContext. log w LINQ to SQL  
- Mechanizm pozwalający dostosować zawartość i formatowanie danych wyjściowych wysyłanych do dziennika  
- Bloki konstrukcyjne niskiego poziomu dla przechwycenia zapewniające większą kontrolę/elastyczność  

## <a name="context-log-property"></a>Właściwość dziennika kontekstu  

Właściwość DbContext. Database. log może być ustawiona na delegat dla dowolnej metody, która przyjmuje ciąg. Najczęściej jest używany z dowolnym elementem TextWriter przez ustawienie go na metodę "Write" tego elementu TextWriter. Wszystkie SQL wygenerowane przez bieżący kontekst zostaną zarejestrowane w tym składniku zapisywania. Na przykład następujący kod będzie rejestrował SQL w konsoli programu:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

Zwróć uwagę na kontekst. Database. log jest ustawiony na Console. Write. To wszystko, co jest konieczne do zarejestrowania bazy danych SQL w konsoli programu.  

Dodajmy kod prostego zapytania/wstawiania/aktualizacji, aby można było zobaczyć dane wyjściowe:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    var blog = context.Blogs.First(b => b.Title == "One Unicorn");

    blog.Posts.First().Title = "Green Eggs and Ham";

    blog.Posts.Add(new Post { Title = "I do not like them!" });

    context.SaveChangesAsync().Wait();
}
```  

Spowoduje to wygenerowanie następujących danych wyjściowych:  

``` SQL
SELECT TOP (1)
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title]
    FROM [dbo].[Blogs] AS [Extent1]
    WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 4 ms with result: SqlDataReader

SELECT
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title],
    [Extent1].[BlogId] AS [BlogId]
    FROM [dbo].[Posts] AS [Extent1]
    WHERE [Extent1].[BlogId] = @EntityKeyValue1
-- EntityKeyValue1: '1' (Type = Int32)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader

UPDATE [dbo].[Posts]
SET [Title] = @0
WHERE ([Id] = @1)
-- @0: 'Green Eggs and Ham' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 12 ms with result: 1

INSERT [dbo].[Posts]([Title], [BlogId])
VALUES (@0, @1)
SELECT [Id]
FROM [dbo].[Posts]
WHERE @@ROWCOUNT > 0 AND [Id] = scope_identity()
-- @0: 'I do not like them!' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader
```  

(Należy zauważyć, że jest to dane wyjściowe, przy założeniu, że wszystkie inicjalizacje bazy danych już się nie były Jeśli inicjalizacja bazy danych jeszcze się nie zakończyła, może być dużo więcej danych wyjściowych pokazujących, że wszystkie migracje pracują w obszarze okładek do sprawdzenia lub tworzenia nowej bazy danych.  

## <a name="what-gets-logged"></a>Co jest rejestrowane?  

Po ustawieniu właściwości log zostaną zarejestrowane wszystkie następujące elementy:  

- SQL dla wszystkich różnych rodzajów poleceń. Na przykład:  
    - Zapytania, w tym normalne zapytania LINQ, zapytania eSQL i nieprzetworzone zapytania z metod takich jak sqlQuery  
    - Wstawia, aktualizuje i usuwa wygenerowane w ramach metody SaveChanges  
    - Relacja ładowania zapytań, takich jak te wygenerowane przez ładowanie z opóźnieniem  
- Parametry  
- Określa, czy polecenie jest wykonywane asynchronicznie  
- Sygnatura czasowa wskazująca, kiedy uruchomiono polecenie  
- Bez względu na to, czy polecenie zostało wykonane pomyślnie, nie powiodło się, zwracając wyjątek, lub, w przypadku Async, zostało anulowane  
- Wskazanie wartości wyniku  
- Przybliżony czas wykonywania polecenia. Należy zauważyć, że jest to czas od wysłania polecenia, aby ponownie uzyskać obiekt wyniku. Nie zawiera czasu, aby odczytać wyniki.  

Analizując przykładowe dane wyjściowe, każdy z czterech zarejestrowanych poleceń jest:  

- Zapytanie pochodzące z wywołania kontekstu. Blogi. pierwszy  
    - Zwróć uwagę, że Metoda ToString pobierająca SQL nie działała dla tego zapytania, ponieważ element "First" nie udostępnia wartości IQueryable, w której można wywołać metodę ToString  
- Zapytanie wynikłe z załadowania blogu. Księgowa  
    - Zwróć uwagę na szczegóły parametrów dla wartości klucza, dla którego jest wykonywane ładowanie z opóźnieniem  
    - Rejestrowane są tylko właściwości parametru, które są ustawione na wartości inne niż domyślne. Na przykład Właściwość Size jest wyświetlana tylko wtedy, gdy jest różna od zera.  
- Dwa polecenia pochodzące z SaveChangesAsync; jeden dla aktualizacji, aby zmienić tytuł wpisu, drugi dla Wstaw, aby dodać nowy wpis  
    - Zwróć uwagę na szczegóły parametru właściwości FK i tytułu  
    - Zwróć uwagę, że te polecenia są wykonywane asynchronicznie  

## <a name="logging-to-different-places"></a>Rejestrowanie w różnych miejscach  

Jak pokazano powyżej, logowanie do konsoli jest bardzo proste. Można również łatwo rejestrować w pamięci, pliku itp. przy użyciu różnych rodzajów [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).  

Jeśli znasz LINQ to SQL można zauważyć, że w LINQ to SQL Właściwość log jest ustawiona na rzeczywisty obiekt TextWriter (na przykład Console. out), natomiast w EF Właściwość log ma ustawioną metodę, która akceptuje ciąg (na przykład , Console. Write lub Console. out. Write). Przyczyną tego jest oddzielenie EF od TextWriter przez zaakceptowanie dowolnego delegata, który może pełnić rolę ujścia dla ciągów. Załóżmy na przykład, że masz już pewną strukturę rejestrowania i definiujemy metodę rejestrowania, taką jak:  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

Może to być podłączane do właściwości log EF podobnej do:  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

## <a name="result-logging"></a>Rejestrowanie wyników  

Domyślny dziennik rejestratora (SQL), parametry i wiersz "Executing" z sygnaturą czasową przed wysłaniem polecenia do bazy danych. Wiersz "ukończony" zawierający czas, który upłynął, jest rejestrowany po wykonaniu polecenia.  

Należy pamiętać, że w przypadku poleceń asynchronicznych wiersz "ukończony" nie jest rejestrowany, dopóki zadanie asynchroniczne nie zostanie faktycznie ukończone, nie powiedzie się lub zostanie anulowane.  

Wiersz "ukończony" zawiera różne informacje w zależności od typu polecenia oraz tego, czy wykonywanie zakończyło się pomyślnie.  

### <a name="successful-execution"></a>Pomyślne wykonanie  

W przypadku poleceń, które zakończą się pomyślnie, dane wyjściowe to "ukończone w x MS z wynikiem:" po którym następuje kilka wskazań wyniku. Dla poleceń, które zwracają czytnik danych, wskazanie wyniku jest typem zwracanym przez [DbDataReader](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) . Dla poleceń, które zwracają liczbę całkowitą, taką jak polecenie Update wyświetlane powyżej wynik, jest to liczba całkowita.  

### <a name="failed-execution"></a>Nieudane wykonanie  

W przypadku poleceń, które kończą się niepowodzeniem, zwracając wyjątek, dane wyjściowe zawierają komunikat z wyjątku. Na przykład użycie sqlQuery do zapytania względem tabeli, która istnieje, spowoduje, że dane wyjściowe dziennika są podobne do następujących:  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

### <a name="canceled-execution"></a>Anulowano wykonywanie  

W przypadku poleceń asynchronicznych, gdy zadanie zostało anulowane, wynik może być spowodowany błędem, ponieważ jest to podstawowy dostawca ADO.NET często, gdy podejmowana jest próba anulowania. Jeśli to się nie stanie, a zadanie zostanie anulowane, dane wyjściowe będą wyglądać następująco:  

```console
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>Zmiana zawartości i formatowania dziennika  

W obszarze okładek Właściwość Database. log wykorzystuje obiekt DatabaseLogFormatter. Ten obiekt efektywnie wiąże implementację IDbCommandInterceptor (patrz poniżej) z delegatem akceptującym ciągi i DbContext. Oznacza to, że metody na DatabaseLogFormatter są wywoływane przed i po wykonaniu poleceń przez EF. Te metody DatabaseLogFormatter zbierają i formatują dane wyjściowe dziennika i wysyłają je do delegata.  

### <a name="customizing-databaselogformatter"></a>Dostosowywanie DatabaseLogFormatter  

Aby zmienić sposób rejestrowania i sposobu jego formatowania, można uzyskać nową klasę pochodzącą od DatabaseLogFormatter i zastępować metody zgodnie z potrzebami. Najczęstszymi metodami przesłonięcia są:  

- LogCommand — zastąpienie tego elementu, aby zmienić sposób rejestrowania poleceń przed ich wykonaniem. Domyślnie LogCommand wywołuje LogParameter dla każdego parametru; w zamian można wykonać te same czynności w przesłonięciu lub w parametrach obsługi w inny sposób.  
- LogResult — zastąpienie tej metody w celu zmiany sposobu rejestrowania wyniku wykonywania polecenia.  
- LogParameter — zastąpienie tego elementu, aby zmienić formatowanie i zawartość rejestrowania parametrów.  

Załóżmy na przykład, że chcemy zarejestrować tylko jeden wiersz przed wysłaniem każdego polecenia do bazy danych. Można to zrobić przy użyciu dwóch zastąpień:  

- Zastąp LogCommand formatowaniem i zapisaniem pojedynczej linii SQL  
- Przesłoń LogResult, aby nic nie robić.  

Kod będzie wyglądać następująco:

``` csharp
public class OneLineFormatter : DatabaseLogFormatter
{
    public OneLineFormatter(DbContext context, Action<string> writeAction)
        : base(context, writeAction)
    {
    }

    public override void LogCommand<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        Write(string.Format(
            "Context '{0}' is executing command '{1}'{2}",
            Context.GetType().Name,
            command.CommandText.Replace(Environment.NewLine, ""),
            Environment.NewLine));
    }

    public override void LogResult<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
    }
}
```  

Aby rejestrować dane wyjściowe, wystarczy wywołać metodę Write, która wyśle dane wyjściowe do skonfigurowanego delegata zapisu.  

(Należy zauważyć, że ten kod uproszczony usuwanie podziałów wierszy jako przykładu. Prawdopodobnie nie będzie dobrze działała w przypadku wyświetlania złożonego języka SQL.  

### <a name="setting-the-databaselogformatter"></a>Ustawianie DatabaseLogFormatter  

Po utworzeniu nowej klasy DatabaseLogFormatter należy ją zarejestrować w EF. Odbywa się to przy użyciu konfiguracji opartej na kodzie. W Nutshell oznacza to utworzenie nowej klasy, która pochodzi od dbconfiguration w tym samym zestawie, co Klasa DbContext, a następnie wywołanie SetDatabaseLogFormatter w konstruktorze tej nowej klasy. Na przykład:  

``` csharp
public class MyDbConfiguration : DbConfiguration
{
    public MyDbConfiguration()
    {
        SetDatabaseLogFormatter(
            (context, writeAction) => new OneLineFormatter(context, writeAction));
    }
}
```  

### <a name="using-the-new-databaselogformatter"></a>Korzystanie z nowego DatabaseLogFormatter  

Ta nowa DatabaseLogFormatter zostanie teraz użyta w dowolnym momencie. log jest ustawiony. Dlatego uruchomienie kodu z części 1 spowoduje teraz następujące wyniki:  

```console
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>Bloki konstrukcyjne przechwycenia  

Do tej pory sprawdziłmy, jak używać DbContext. Database. log w celu rejestrowania kodu SQL wygenerowanego przez EF. Jednak ten kod jest w rzeczywistości stosunkowo wąskim elewacją nad niektórymi blokami konstrukcyjnymi niskiego poziomu dla bardziej ogólnego przechwycenia.  

### <a name="interception-interfaces"></a>Interfejsy przechwycenia  

Kod przechwycenia jest zbudowany wokół koncepcji interfejsów przechwycenia. Te interfejsy dziedziczą z IDbInterceptor i definiują metody, które są wywoływane, gdy EF wykonuje pewne działania. Celem jest posiadanie jednego interfejsu dla każdego typu przechwytywanego obiektu. Na przykład interfejs IDbCommandInterceptor definiuje metody, które są wywoływane przed EF, wywołuje ExecuteNonQuery, ExecuteScalar, ExecuteReader i powiązane metody. Podobnie interfejs definiuje metody, które są wywoływane po zakończeniu każdej z tych operacji. Powyższa Klasa DatabaseLogFormatter implementuje ten interfejs do rejestrowania poleceń.  

### <a name="the-interception-context"></a>Kontekst przechwycenia  

Przeglądając metody zdefiniowane w dowolnym interfejsie interceptorów, jest oczywiste, że każde wywołanie otrzymuje obiekt typu DbInterceptionContext lub niektórych typów pochodnych takich jak DbCommandInterceptionContext @ no__t-0 @ no__t-1. Ten obiekt zawiera informacje kontekstowe dotyczące akcji, którą pobiera Dr. Na przykład, jeśli akcja jest wykonywana w imieniu DbContext, element DbContext zostanie uwzględniony w DbInterceptionContext. Podobnie w przypadku poleceń wykonywanych asynchronicznie flaga IsAsync jest ustawiana na DbCommandInterceptionContext.  

### <a name="result-handling"></a>Obsługa wyników  

Klasa DbCommandInterceptionContext @ no__t-0 @ no__t-1 zawiera właściwości o nazwie Result, OriginalResult, Exception i OriginalException. Te właściwości są ustawione na wartość null/zero dla wywołań metod przechwycenia, które są wywoływane przed wykonaniem operacji — to znaczy, dla... Wykonywanie metod. Jeśli operacja zostanie wykonana i powiedzie się, a następnie wynik i OriginalResult są ustawione na wynik operacji. Te wartości można następnie zaobserwować w metodach przechwycenia, które są wywoływane po wykonaniu operacji — to znaczy, w... Wykonane metody. Podobnie, jeśli operacja zgłasza, zostanie ustawiona właściwość wyjątku i Oryginalnaexception.  

#### <a name="suppressing-execution"></a>Pomijanie wykonywania  

Jeśli Interceptor ustawia właściwość wynik przed wykonaniem polecenia (w jednym z... Wykonywanie metod), a następnie EF nie podejmie próby rzeczywistego wykonania polecenia, ale zamiast tego będzie używać zestawu wyników. Innymi słowy Interceptor może pominąć wykonywanie polecenia, ale polecenie Dr kontynuuje działanie tak, jakby zostało wykonane.  

Przykładem użycia tej metody jest tworzenie wsadowe polecenia, które tradycyjnie zostało wykonane z dostawcą otoki. Interceptor będzie przechowywał polecenie do późniejszego wykonania jako Partia zadań, ale zostałaby "poudawać" do EF, że polecenie zostało wykonane jako normalne. Należy zauważyć, że wymaga to więcej niż to, aby zaimplementować przetwarzanie wsadowe, ale jest to przykład, w jaki sposób można użyć zmiany wyniku przechwycenia.  

Wykonywanie można także pominąć, ustawiając właściwość wyjątku w jednej z... Wykonywanie metod. Powoduje to, że EF kontynuuje działanie, tak jakby wykonywanie operacji kończyło się niepowodzeniem przez wygenerowanie danego wyjątku. Może to oczywiście spowodować awarię aplikacji, ale może to być również wyjątek przejściowy lub inny wyjątek, który jest obsługiwany przez EF. Na przykład może być używany w środowiskach testowych do testowania zachowania aplikacji, gdy wykonanie polecenia nie powiedzie się.  

#### <a name="changing-the-result-after-execution"></a>Zmiana wyniku po wykonaniu  

Jeśli Interceptor ustawia właściwość wynik po wykonaniu polecenia (w jednym z... Wykonane metody), a następnie EF użyje zmienionego wyniku zamiast wyniku, który faktycznie został zwrócony z operacji. Podobnie, jeśli Interceptor ustawia właściwość wyjątku po wykonaniu polecenia, EF zgłosi wyjątek, tak jakby operacja zgłosiła wyjątek.  

Interceptor może również ustawić właściwość wyjątku na wartość null, aby wskazać, że żaden wyjątek nie powinien zostać wygenerowany. Może to być przydatne, jeśli wykonanie operacji nie powiodło się, ale Interceptor pożyczy sobie kontynuacji, tak jakby operacja zakończyła się pomyślnie. Zwykle wymaga to również ustawienia wyniku, tak aby Dr miał pewną wartość wynikową do pracy z oczekiwaniami.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult i Oryginalnaexception  

Po wykonaniu operacji przez EF zostanie ustawiona wartość wynik i właściwości OriginalResult, jeśli wykonanie nie powiodło się lub wystąpił błąd podczas wykonywania.  

Właściwości OriginalResult i Oryginalnaexception są tylko do odczytu i są ustawiane tylko przez EF po faktycznym wykonaniu operacji. Właściwości te nie mogą być ustawiane przez Interceptory. Oznacza to, że każdy Interceptor może rozróżnić wyjątek lub wynik, który został ustawiony przez inny obiekt przechwytujący, w przeciwieństwie do rzeczywistego wyjątku lub wyniku, który wystąpił podczas wykonywania operacji.  

### <a name="registering-interceptors"></a>Rejestrowanie przechwyceń  

Po utworzeniu klasy implementującej jeden lub więcej interfejsów przechwycenia można ją zarejestrować w EF przy użyciu klasy dbprzechwytującej. Na przykład:  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

Interceptory mogą być również zarejestrowane na poziomie domeny aplikacji przy użyciu mechanizmu konfiguracji opartego na kodzie dbconfiguration.  

### <a name="example-logging-to-nlog"></a>Przykład: Rejestrowanie w usłudze NLog  

Przekażmy wszystko do przykładu korzystającego z IDbCommandInterceptor i [nLOG](https://nlog-project.org/) do:  

- Rejestruj Ostrzeżenie dla każdego polecenia, które jest wykonywane nieasynchronicznie  
- Rejestruj błąd dla każdego polecenia, które zgłasza po wykonaniu  

Oto Klasa, która wykonuje rejestrowanie, które należy zarejestrować, jak pokazano powyżej:  

``` csharp
public class NLogCommandInterceptor : IDbCommandInterceptor
{
    private static readonly Logger Logger = LogManager.GetCurrentClassLogger();

    public void NonQueryExecuting(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void NonQueryExecuted(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ReaderExecuting(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ReaderExecuted(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ScalarExecuting(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ScalarExecuted(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    private void LogIfNonAsync<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (!interceptionContext.IsAsync)
        {
            Logger.Warn("Non-async command used: {0}", command.CommandText);
        }
    }

    private void LogIfError<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (interceptionContext.Exception != null)
        {
            Logger.Error("Command {0} failed with exception {1}",
                command.CommandText, interceptionContext.Exception);
        }
    }
}
```  

Zwróć uwagę, w jaki sposób ten kod używa kontekstu przechwycenia, aby wykryć, kiedy polecenie jest wykonywane nieasynchronicznie i wykryć, kiedy wystąpił błąd podczas wykonywania polecenia.  
