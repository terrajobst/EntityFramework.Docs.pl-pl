---
title: Korzystanie z programu zmigrować. exe-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 989ea862-e936-4c85-926a-8cfbef5df5b8
ms.openlocfilehash: 34866ddbbf81f090a064af148a612dd354ae9401
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824815"
---
# <a name="using-migrateexe"></a>Korzystanie z programu zmigrować. exe
Migracje Code First można użyć do zaktualizowania bazy danych z poziomu programu Visual Studio, ale można ją również wykonać za pomocą narzędzia wiersza polecenia migruje. exe. Ta strona zawiera krótkie omówienie sposobu używania programu Migration. exe do wykonywania migracji względem bazy danych programu.

> [!NOTE]
> W tym artykule założono, że wiesz, jak używać Migracje Code First w podstawowych scenariuszach. Jeśli tego nie zrobisz, musisz przeczytać [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) przed kontynuowaniem.

## <a name="copy-migrateexe"></a>Kopiuj plik zmigrować. exe

W przypadku instalowania Entity Framework przy użyciu programu NuGet. exe zostanie umieszczony w folderze Tools pobranego pakietu. W folderze projektu &lt;&gt;pakiety \\\\EntityFramework.&lt;wersja&gt;narzędzia \\

Po przeprowadzeniu migracji pliku exe należy skopiować go do lokalizacji zestawu, który zawiera migracje.

Jeśli aplikacja jest przeznaczona dla platformy .NET 4, a nie 4,5, należy skopiować **plik redirect. config** do lokalizacji i zmienić jego nazwę na **zmigrować. exe. config**. Jest tak dlatego, że program migruje. exe pobiera poprawne przekierowania powiązań, aby można było zlokalizować zestaw Entity Framework.

| .NET 4.5                                      | .NET 4.0                                      |
|:----------------------------------------------|:----------------------------------------------|
| ![Pliki programu .NET 4,5](~/ef6/media/net45files.png) | ![Pliki programu .NET 4,0](~/ef6/media/net40files.png) |

> [!NOTE]
> Migracja. exe nie obsługuje zestawów x64.

Po przeniesieniu pliku Migration. exe do prawidłowego folderu należy go używać do wykonywania migracji do bazy danych programu. Wszystkie narzędzia są przeznaczone do wykonywania migracji. Nie można wygenerować migracji ani utworzyć skryptu SQL.

## <a name="see-options"></a>Zobacz Opcje

``` console
Migrate.exe /?
```

W powyższym oknie zostanie wyświetlona strona pomocy skojarzona z tym narzędziem. należy pamiętać, że w tej samej lokalizacji, w której uruchomiono plik migruje. exe, trzeba mieć EntityFramework. dll.

## <a name="migrate-to-the-latest-migration"></a>Migrowanie do najnowszej migracji

``` console
Migrate.exe MyMvcApplication.dll /startupConfigurationFile="..\\web.config"
```

Podczas uruchamiania programu Migration. exe jedynym obowiązkowym parametrem jest zestaw, który jest zestawem zawierającym migracje, które próbujesz uruchomić, ale będzie używać wszystkich ustawień opartych na Konwencji, jeśli nie określisz pliku konfiguracji.

## <a name="migrate-to-a-specific-migration"></a>Migrowanie do określonej migracji

``` console
Migrate.exe MyApp.exe /startupConfigurationFile="MyApp.exe.config" /targetMigration="AddTitle"
```

Jeśli chcesz przeprowadzić migrację do określonej migracji, możesz określić nazwę migracji. Spowoduje to uruchomienie wszystkich wcześniejszych migracji zgodnie z wymaganiami do momentu podanej migracji.

## <a name="specify-working-directory"></a>Określ katalog roboczy

``` console
Migrate.exe MyApp.exe /startupConfigurationFile="MyApp.exe.config" /startupDirectory="c:\\MyApp"
```

Jeśli zestaw ma zależności lub odczytuje pliki względem katalogu roboczego, należy ustawić startupDirectory.

## <a name="specify-migration-configuration-to-use"></a>Określ konfigurację migracji do użycia

``` console
Migrate.exe MyAssembly CustomConfig /startupConfigurationFile="..\\web.config"
```

Jeśli masz wiele klas konfiguracji migracji, klasy dziedziczenia z DbMigrationConfiguration, należy określić, który ma być używany dla tego wykonania. Jest to określone przez podanie opcjonalnego drugiego parametru bez przełącznika jak powyżej.

## <a name="provide-connection-string"></a>Podaj parametry połączenia

``` console
Migrate.exe BlogDemo.dll /connectionString="Data Source=localhost;Initial Catalog=BlogDemo;Integrated Security=SSPI" /connectionProviderName="System.Data.SqlClient"
```

Jeśli chcesz określić parametry połączenia w wierszu polecenia, należy również podać nazwę dostawcy. Nieokreślanie nazwy dostawcy spowoduje wystąpienie wyjątku.

## <a name="common-problems"></a>Typowe problemy

| Komunikat o błędzie                                                                                                                                                                                                                                                                                                                      | Rozwiązanie                                                                                                                                                                                                                                                                                             |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nieobsługiwany wyjątek: System. IO. FileLoadException: nie można załadować pliku lub zestawu "EntityFramework, Version = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089" lub jednej z jego zależności. Zlokalizowana definicja manifestu zestawu nie odpowiada odwołaniu do zestawu. (Wyjątek od HRESULT: 0x80131040)         | Zazwyczaj oznacza to, że uruchamiasz aplikację .NET 4 bez pliku redirect. config. Należy skopiować plik redirect. config do tej samej lokalizacji, w której znajduje się plik zmigrować. exe i zmienić jego nazwę na Migruj. exe. config.                                                                                       |
| Nieobsługiwany wyjątek: System. IO. FileLoadException: nie można załadować pliku lub zestawu "EntityFramework, Version = 4.4.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089" lub jednej z jego zależności. Zlokalizowana definicja manifestu zestawu nie odpowiada odwołaniu do zestawu. (Wyjątek od HRESULT: 0x80131040)          | Ten wyjątek oznacza, że uruchomiono aplikację .NET 4,5 z przekierowanym. config skopiowanym do lokalizacji migracji. exe. Jeśli aplikacja jest platformą .NET 4,5, nie trzeba mieć pliku konfiguracji z przekierowaniami wewnątrz. Usuń plik zmigrować. exe. config.                                    |
| Błąd: nie można zaktualizować bazy danych tak, aby była zgodna z bieżącym modelem, ponieważ istnieją oczekujące zmiany i automatyczna migracja jest wyłączona. Napisz oczekujące zmiany modelu do migracji opartej na kodzie lub Włącz automatyczną migrację. Ustaw DbMigrationsConfiguration. AutomaticMigrationsEnabled na wartość true, aby umożliwić automatyczną migrację. | Ten błąd występuje, jeśli uruchomiono migrację, gdy nie utworzono migracji w celu zaspokojenia zmian wprowadzonych w modelu, a baza danych nie jest zgodna z modelem. Przykładem może być dodanie właściwości do klasy modelu, a następnie uruchomienie pliku Migration. exe bez tworzenia migracji w celu uaktualnienia bazy danych. |
| Błąd: typ nie został rozpoznany dla elementu członkowskiego "System. Data. Entity. migrations. Design. ToolingFacade + UpdateRunner, EntityFramework, Version = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089".                                                                                                                                       | Ten błąd może być spowodowany przez określenie nieprawidłowego katalogu startowego. Musi to być lokalizacja pliku migracji. exe                                                                                                                                                                                      |
| Nieobsługiwany wyjątek: System. NullReferenceException: odwołanie do obiektu nie jest ustawione na wystąpienie obiektu. <br/>   w System. Data. Entity. migrations. Console. program. Main (ciąg [] args)                                                                                                                                             | Może to być spowodowane nieokreśleniem wymaganego parametru dla scenariusza, którego używasz. Na przykład określenie parametrów połączenia bez określenia nazwy dostawcy.                                                                                                                        |
| Błąd: w zestawie "ClassLibrary1" znaleziono więcej niż jeden typ konfiguracji migracji. Określ nazwę, która ma zostać użyta.                                                                                                                                                                                                  | Ponieważ stanem błędu, istnieje więcej niż jedna Klasa konfiguracji w danym zestawie. Aby określić, który z nich ma być używany, należy użyć przełącznika/configurationType.                                                                                                                                           |
| Błąd: nie można załadować pliku lub zestawu "&lt;assemblyName&gt;" lub jednej z jego zależności. Dana nazwa zestawu lub baza kodu jest nieprawidłowa. (Wyjątek od HRESULT: 0x80131047)                                                                                                                                                    | Może to być spowodowane nieprawidłową nazwą zestawu lub nieposiadaniem                                                                                                                                                                                                                          |
| Błąd: nie można załadować pliku lub zestawu "&lt;assemblyName&gt;" lub jednej z jego zależności. Podjęto próbę załadowania programu z nieprawidłowym formatem.                                                                                                                                                                          | Dzieje się tak, jeśli próbujesz uruchomić plik zmigrować. exe w przypadku aplikacji x64. EF 5,0 i niższych wersji będą działały tylko w architekturze x86.                                                                                                                                                                                |
