---
title: Rejestrowanie i przechwytuje operacji bazy danych — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
caps.latest.revision: 3
ms.openlocfilehash: 9c95b7490c11a28524a1468518261aa4bc317262
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914270"
---
# <a name="logging-and-intercepting-database-operations"></a>Rejestrowanie i przechwytuje operacji bazy danych
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.  

Począwszy od platformy Entity Framework 6, w dowolnym momencie Entity Framework wysyła polecenie do bazy danych to polecenie może zostać przechwycona przez kod aplikacji. Najczęściej jest używana do logowania SQL, ale może również służyć do modyfikowania lub Przerwij polecenie.  

W szczególności EF obejmuje:  

- Właściwości rejestrowania w kontekście podobnie jak DataContext.Log w składniku LINQ to SQL  
- Mechanizm, aby dostosować zawartość i formatowania danych wyjściowych wysyłane do dziennika  
- Niskiego poziomu bloków konstrukcyjnych dla przejmowanie, dzięki czemu większa kontrola/elastycznie  

## <a name="context-log-property"></a>Właściwości rejestrowania w kontekście  

Można ustawić właściwości DbContext.Database.Log delegata dla dowolnej metody, która przyjmuje ciąg. Najczęściej jest używany z dowolnym TextWriter, ustawiając jej do metody "Zapisywanie" tego TextWriter. Moduł zapisujący zostaną zarejestrowane SQL wszystkich generowanych przez bieżącego kontekstu. Na przykład poniższy kod zostanie dziennika SQL do konsoli:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

Należy zauważyć, że kontekst. Console.Write — ustawiono Database.Log. To wszystko, co jest potrzebne do logowania SQL do konsoli.  

### <a name="example-output"></a>Przykładowe dane wyjściowe  

Dodajmy kilka prostych kodów zapytania/insert/update, dzięki czemu możemy zobaczyć pewne dane wyjściowe:  

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

Spowoduje to wygenerowanie następujące dane wyjściowe:  

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

(Należy pamiętać, że to dane wyjściowe przy założeniu, że już stało się inicjowanie bazy danych. Jeśli inicjowanie bazy danych nie ma już wystąpił, a następnie będzie można o wiele więcej danych wyjściowych, przedstawiający wszystkie prace migracje nie dzieje się w tle, aby sprawdzić lub Utwórz nową bazę danych.)  

### <a name="what-gets-logged"></a>Co pobiera zarejestrowane?  

Gdy właściwość dziennika ma wartość wszystkie poniższe będą rejestrowane:  

- SQL dla różnych rodzajów poleceń. Na przykład:  
    - Zapytania, łącznie z normalnym zapytań LINQ, eSQL kwerend i pierwotne zapytania z metody takie jak SqlQuery  
    - Operacje wstawiania, aktualizacji i usuwania, wygenerowane jako część SaveChanges  
    - Podczas ładowania zapytania, np. tych generowanych przez powolne ładowanie relacji  
- Parametry  
- Określa, czy polecenie jest wykonywane asynchronicznie  
- Sygnatura czasowa wskazująca, podczas uruchamiania polecenia wykonywania  
- Czy polecenie zostało wykonane pomyślnie, nie powiodło się, zgłaszając wyjątek lub asynchroniczne, zostało anulowane  
- Niektóre wskazania wartość wyniku  
- Kwota przybliżony czas potrzebny do wykonania polecenia. Należy pamiętać, że jest to czas wysyłanie polecenia do pobierania obiektu wyników, ponownie. Nie ma czasu można odczytać wyników.  

Patrząc powyższych danych wyjściowych przykładu, każdy z czterech poleceń rejestrowane są:  

- Zapytanie wynikające z wywołania do kontekstu. Blogs.First  
    - Należy zauważyć, że metoda ToString pobierania SQL nie będzie działać dla tego zapytania, ponieważ "First" nie zawiera element IQueryable, na którym może być wywoływana ToString  
- Zapytanie wynikające z powolne ładowanie blogu. Wpisy  
    - Zwróć uwagę, dzieje się szczegóły parametrów dla wartości klucza, dla którego opóźnieniem ładowania  
    - Rejestrowane są tylko te właściwości parametru, które są ustawione na wartości innych niż domyślne. Na przykład właściwość rozmiaru jest wyświetlana tylko jeśli jest różna od zera.  
- Dwa polecenia wynikające z SaveChangesAsync; jeden dla aktualizacji zmienić tytuł wpisu, drugie dla instrukcji insert dodać nowy wpis  
    - Zwróć uwagę, szczegóły parametrów właściwości klucza Obcego i tytuł  
    - Należy zauważyć, że te polecenia są wykonywane asynchronicznie  

### <a name="logging-to-different-places"></a>Rejestrowanie w różnych miejscach  

Jak wspomniano powyżej logowanie do konsoli jest bardzo proste. Jest również łatwe logowanie do pamięci, plików, itp. przy użyciu różnych rodzajów z [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).  

Jeśli znasz za pomocą LINQ to SQL może się okazać, że w składniku LINQ to SQL dziennika właściwość jest ustawiona do rzeczywistego TextWriter obiektu (np. Console.Out) podczas w programie EF ustawiono właściwość dziennika do metody, która akceptuje parametry (np. Console.Write — lub Console.Out.Write). Przyczyną jest oddzielenie EF z TextWriter, akceptując dowolnym delegatem, który może działać jako obiekt sink dla ciągów. Załóżmy, że masz już pewne struktury rejestrowania i definiuje metodę rejestrowania w następujący sposób:  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

To może podłączany do właściwości rejestrowania EF następująco:  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

### <a name="result-logging"></a>Rejestrowanie wyników  

Rejestrator domyślny tekst polecenia (SQL), parametry i dzienników wiersza "Executing" z sygnaturą czasową przed wysłaniem polecenia do bazy danych. "Ukończone" wiersz zawierający czas, który upłynął są rejestrowane następujące wykonanie polecenia.  

Należy pamiętać, że asynchroniczne "ukończony" wiersz polecenia nie jest rejestrowane, dopóki zadanie asynchroniczne faktycznie kończy, kończy się niepowodzeniem lub zostanie anulowane.  

"Ukończone" wiersz zawiera różne informacje w zależności od typu polecenia i określa, czy wykonywanie zakończyło się pomyślnie.  

#### <a name="successful-execution"></a>Pomyślne wykonanie  

W przypadku poleceń, które się to odbyć danych wyjściowych jest "wykonane w x ms z wynikiem:" następuje niektóre wskazania wynik był. Dla poleceń, które zwracają wynik czytnik danych oznaczenie jest typ [obiekt DbDataReader](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) zwracane. Dla poleceń, które zwracają wartość całkowitą, takich jak aktualizacja przedstawionego powyżej wyniku, przedstawione polecenia jest tym liczbą całkowitą.  

#### <a name="failed-execution"></a>Wykonywanie nie powiodło się  

Pomijanie wykonania Jeśli ta opcja interceptor ustawia właściwości wyniku, zanim to polecenie zostało wykonane (w jednym z... Wykonywanie metod) następnie EF nie podejmie próby rzeczywistego wykonania polecenia, ale zamiast tego użyje po prostu zestaw wyników.  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

#### <a name="canceled-execution"></a>Innymi słowy interceptor można pominąć wykonywanie polecenia, ale ma EF kontynuowane tak, jakby było zostały wykonane polecenie.  

Przykład sposobu użycia tego może być jest polecenia przetwarzania wsadowego, która była tradycyjnie wykonywana przy użyciu dostawcy zawijania. Interceptor będzie przechowywać polecenia do późniejszego wykonania jako zadania wsadowego, ale będzie "poudawać" do programu EF, że polecenie było wykonywane w zwykły.  

```  
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>Należy pamiętać, że wymaga więcej niż to do zaimplementowania przetwarzania wsadowego, ale jest to przykład jak zmiana wyniku przejmowanie mogą być używane.  

### <a name="databaselogformatter"></a>Wykonanie również można pominąć, ustawiając właściwość wyjątku w jednej z... Wykonywanie metod.  

Powoduje to EF kontynuować, tak, jakby wykonanie operacji miał nie powiodło się, zwracając dany wyjątek. Oczywiście, może to spowodować aplikacji awarię, ale może to być także wyjątek przejściowy lub innych wyjątków, który jest obsługiwany przez EF. Na przykład to może służyć w środowiskach testowych do testowania zachowanie aplikacji, gdy wykonywanie polecenia nie powiodło się. Zmiana wyniku Po wykonaniu  

### <a name="customizing-databaselogformatter"></a>Jeśli ta opcja ustawia interceptor właściwości wyniku Po wykonaniu polecenia (w jednym z... Wykonania metody), a następnie platforma EF użyje wynik zmieniono zamiast wynik, który faktycznie został zwrócony przez operację.  

Podobnie jeśli interceptor ustawia właściwość wyjątku po wykonaniu polecenia, następnie EF spowoduje zgłoszenie wyjątku zestaw tak, jakby operacji miał wyjątek. Interceptor można również ustawić właściwość wyjątku na wartość null, aby wskazać, że nie należy zgłosić wyjątek.  

- Może to być przydatne, jeśli nie można wykonać operacji, ale interceptor chce EF, aby kontynuować, tak, jakby zakończyło się operacji. Zwykle obejmuje to również ustawienie wynik tak, aby EF jakąś wartość wynik, aby pracować, jak długo.  
- OriginalResult i oryginalny wyjątek  
- Po wykonaniu operacji EF zostanie ustawiony wynik i OriginalResult właściwości, jeśli wykonanie nie zakończyła się niepowodzeniem lub właściwości wyjątku i oryginalny wyjątek, jeśli wykonanie nie powiodło się z powodu wyjątku.  

Właściwości OriginalResult i oryginalny wyjątek są przeznaczone tylko do odczytu i ustawionych tylko przez EF po rzeczywistego wykonania operacji. Te właściwości nie może ustawić interceptory.  

- Oznacza to, że wszelkie interceptor może rozróżnić wystąpi wyjątek lub wynik, która została ustawiona przez niektóre interceptor, w przeciwieństwie do rzeczywistego wyjątek lub wynik, który wystąpił podczas operacji został wykonany.  
- Rejestrowanie interceptory  

Po utworzeniu klasy, która implementuje co najmniej jeden z interfejsów przejmowanie mogą być rejestrowane struktury jednostek przy użyciu klasy DbInterception.

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

Interceptory można zarejestrować w taki sposób, na poziomie domeny aplikacji przy użyciu mechanizmu DbConfiguration konfiguracja na podstawie kodu.  

Przykład: Rejestrowanie NLog Teraz umieść to wszystko ze sobą przykładowi, przy użyciu IDbCommandInterceptor i NLog do:  

### <a name="setting-the-databaselogformatter"></a>Ostrzeżenie dla dowolnego polecenia, który jest wykonywany bez asynchronicznie dziennika  

Błąd dla dowolnego polecenia, który zgłasza wyjątek podczas wykonywania Poniżej przedstawiono klasy, która wykonuje rejestrowanie, które powinny być rejestrowane, jak pokazano powyżej: Zwróć uwagę, jak ten kod używa kontekstu przejmowanie, aby dowiedzieć się, gdy polecenie jest wykonywane innych niż asynchronicznie, a także do wykrywania, gdy wystąpił błąd podczas wykonywania polecenia. Na przykład:  

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

### <a name="using-the-new-databaselogformatter"></a>Za pomocą nowego DatabaseLogFormatter  

Ten nowy DatabaseLogFormatter będzie używany w dowolnym momencie Database.Log jest ustawiona. Tak uruchamiając kod z części 1 teraz spowoduje następujące dane wyjściowe:  

```  
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>Bloki konstrukcyjne przejmowanie  

Do tej pory mają zobaczyliśmy, jak używać DbContext.Database.Log logowania SQL generowane przez EF. Ale ten kod jest faktycznie fasada stosunkowo alokowania elastycznego za pośrednictwem niektórych niskiego poziomu bloków konstrukcyjnych dla bardziej ogólnych przejmowanie.  

### <a name="interception-interfaces"></a>Przejmowanie interfejsów  

Kod zatrzymania opiera się na koncepcji przejmowanie interfejsów. Te interfejsy dziedziczyć IDbInterceptor i definiowania metod, które są wywoływane, gdy EF wykonuje jakąś akcję. Celem jest zapewnienie jednego interfejsu na typ obiektu, które są przechwytywane. Na przykład interfejs IDbCommandInterceptor definiuje metody, które są wywoływane przed EF wywołuje ExecuteNonQuery ExecuteScalar, ExecuteReader oraz powiązanych metod. Podobnie interfejs definiuje metody, które są wywoływane po zakończeniu każdej z tych operacji. Klasa DatabaseLogFormatter, która przyjrzeliśmy się powyżej implementuje ten interfejs logowania poleceń.  

### <a name="the-interception-context"></a>Kontekst przejmowanie  

Patrząc metod w interfejsach interceptor wobec jego okaże się, że każde wywołanie podanego obiektu typu DbInterceptionContext lub typu pochodzi od klasy to przykład DbCommandInterceptionContext\<\>. Ten obiekt zawiera informacje kontekstowe o akcji, która zajmuje EF. Na przykład jeśli akcja jest wykonywana w imieniu typu DbContext, kontekstu DbContext jest uwzględnione w DbInterceptionContext. Podobnie do poleceń, które są wykonywane asynchronicznie, IsAsync flaga jest ustawiona na DbCommandInterceptionContext.  

### <a name="result-handling"></a>Obsługa wyników  

DbCommandInterceptionContext\< \> klasa zawiera właściwości o nazwie wynik, OriginalResult, wyjątków i oryginalny wyjątek. Te właściwości są ustawione na wartość null/zero dla wywołania metod przejmowanie, które są wywoływane przed operacji executed—i.e.... Wykonywanie metod. Jeśli operacja jest wykonywana, a zakończy się pomyślnie, następnie wynik i OriginalResult są ustawione na wynik operacji. Następnie można zaobserwować te wartości w metodach przejmowanie, które są wywoływane po operacji executed—i.e.... Wykonane metody. Podobnie jeśli operacja zgłosi, następnie właściwości wyjątku i oryginalny wyjątek zostaną ustawione.  

#### <a name="suppressing-execution"></a>Pomijanie wykonania  

Jeśli ta opcja interceptor ustawia właściwości wyniku, zanim to polecenie zostało wykonane (w jednym z... Wykonywanie metod) następnie EF nie podejmie próby rzeczywistego wykonania polecenia, ale zamiast tego użyje po prostu zestaw wyników. Innymi słowy interceptor można pominąć wykonywanie polecenia, ale ma EF kontynuowane tak, jakby było zostały wykonane polecenie.  

Przykład sposobu użycia tego może być jest polecenia przetwarzania wsadowego, która była tradycyjnie wykonywana przy użyciu dostawcy zawijania. Interceptor będzie przechowywać polecenia do późniejszego wykonania jako zadania wsadowego, ale będzie "poudawać" do programu EF, że polecenie było wykonywane w zwykły. Należy pamiętać, że wymaga więcej niż to do zaimplementowania przetwarzania wsadowego, ale jest to przykład jak zmiana wyniku przejmowanie mogą być używane.  

Wykonanie również można pominąć, ustawiając właściwość wyjątku w jednej z... Wykonywanie metod. Powoduje to EF kontynuować, tak, jakby wykonanie operacji miał nie powiodło się, zwracając dany wyjątek. Oczywiście, może to spowodować aplikacji awarię, ale może to być także wyjątek przejściowy lub innych wyjątków, który jest obsługiwany przez EF. Na przykład to może służyć w środowiskach testowych do testowania zachowanie aplikacji, gdy wykonywanie polecenia nie powiodło się.  

#### <a name="changing-the-result-after-execution"></a>Zmiana wyniku Po wykonaniu  

Jeśli ta opcja ustawia interceptor właściwości wyniku Po wykonaniu polecenia (w jednym z... Wykonania metody), a następnie platforma EF użyje wynik zmieniono zamiast wynik, który faktycznie został zwrócony przez operację. Podobnie jeśli interceptor ustawia właściwość wyjątku po wykonaniu polecenia, następnie EF spowoduje zgłoszenie wyjątku zestaw tak, jakby operacji miał wyjątek.  

Interceptor można również ustawić właściwość wyjątku na wartość null, aby wskazać, że nie należy zgłosić wyjątek. Może to być przydatne, jeśli nie można wykonać operacji, ale interceptor chce EF, aby kontynuować, tak, jakby zakończyło się operacji. Zwykle obejmuje to również ustawienie wynik tak, aby EF jakąś wartość wynik, aby pracować, jak długo.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult i oryginalny wyjątek  

Po wykonaniu operacji EF zostanie ustawiony wynik i OriginalResult właściwości, jeśli wykonanie nie zakończyła się niepowodzeniem lub właściwości wyjątku i oryginalny wyjątek, jeśli wykonanie nie powiodło się z powodu wyjątku.  

Właściwości OriginalResult i oryginalny wyjątek są przeznaczone tylko do odczytu i ustawionych tylko przez EF po rzeczywistego wykonania operacji. Te właściwości nie może ustawić interceptory. Oznacza to, że wszelkie interceptor może rozróżnić wystąpi wyjątek lub wynik, która została ustawiona przez niektóre interceptor, w przeciwieństwie do rzeczywistego wyjątek lub wynik, który wystąpił podczas operacji został wykonany.  

### <a name="registering-interceptors"></a>Rejestrowanie interceptory  

Po utworzeniu klasy, która implementuje co najmniej jeden z interfejsów przejmowanie mogą być rejestrowane struktury jednostek przy użyciu klasy DbInterception. Na przykład:  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

Interceptory można zarejestrować w taki sposób, na poziomie domeny aplikacji przy użyciu mechanizmu DbConfiguration konfiguracja na podstawie kodu.  

### <a name="example-logging-to-nlog"></a>Przykład: Rejestrowanie NLog  

Teraz umieść to wszystko ze sobą przykładowi, przy użyciu IDbCommandInterceptor i [NLog](http://nlog-project.org/) do:  

- Ostrzeżenie dla dowolnego polecenia, który jest wykonywany bez asynchronicznie dziennika  
- Błąd dla dowolnego polecenia, który zgłasza wyjątek podczas wykonywania  

Poniżej przedstawiono klasy, która wykonuje rejestrowanie, które powinny być rejestrowane, jak pokazano powyżej:  

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

Zwróć uwagę, jak ten kod używa kontekstu przejmowanie, aby dowiedzieć się, gdy polecenie jest wykonywane innych niż asynchronicznie, a także do wykrywania, gdy wystąpił błąd podczas wykonywania polecenia.  