---
title: Za pomocą migrate.exe - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 989ea862-e936-4c85-926a-8cfbef5df5b8
ms.openlocfilehash: cf6c3a0a256730b24addf1012d6ff53b17035cd4
ms.sourcegitcommit: c568d33214fc25c76e02c8529a29da7a356b37b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/30/2018
ms.locfileid: "47459542"
---
# <a name="using-migrateexe"></a>Za pomocą migrate.exe
Migracje Code First pozwala zaktualizować bazę danych z wewnątrz programu visual studio, ale mogą być również wykonywane za pośrednictwem migrate.exe narzędzia wiersza polecenia. Ta strona będzie zapewniają szybki przegląd dotyczące sposobu używania migrate.exe do wykonania migracji w bazie danych.

> [!NOTE]
> W tym artykule przyjęto założenie, że wiesz, jak użyć migracje Code First w podstawowych scenariuszy. Jeśli tego nie zrobisz, a następnie należy odczytać [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) przed kontynuowaniem.

## <a name="copy-migrateexe"></a>Skopiuj migrate.exe

Po zainstalowaniu programu Entity Framework za pomocą narzędzia NuGet migrate.exe będzie znajdować się w folderze narzędzia pobranego pakietu. W &lt;folderu projektu&gt;\\pakietów\\platformy EntityFramework.&lt; Wersja&gt;\\narzędzia

Po utworzeniu migrate.exe musisz skopiować go do lokalizacji zestawu, który zawiera migracji.

Jeśli aplikacja jest przeznaczony dla .NET 4, a nie 4.5, należy skopiować **Redirect.config** w lokalizacji jako dobrze i zmień jego nazwę **migrate.exe.config**. Jest to więc migrate.exe pobiera przekierowania powiązań poprawna, aby można było zlokalizować zestawu platformy Entity Framework.

| .NET 4.5                                      | .NET 4.0                                      |
|:----------------------------------------------|:----------------------------------------------|
| ![Pliki .NET 4.5](~/ef6/media/net45files.png) | ![Pliki .NET 4.0](~/ef6/media/net40files.png) |

> [!NOTE]
> migrate.exe nie obsługuje x64 zestawów.

Po migrate.exe zostały przeniesione do poprawnego folderu powinno być można używać go do wykonania migracji w bazie danych. Jest wszystko, czego narzędzie zaprojektowano w celu wykonania migracji. Nie można wygenerować migracje ani utworzyć skrypt SQL.

## <a name="see-options"></a>Zobacz Opcje

``` console
Migrate.exe /?
```

Powyższe spowoduje wyświetlenie strony pomocy skojarzony z tym narzędziu, należy pamiętać, że musisz mieć EntityFramework.dll w tej samej lokalizacji, migrate.exe uruchomionych w kolejności, aby to działało.

## <a name="migrate-to-the-latest-migration"></a>Migracja do najnowszych migracji

``` console
Migrate.exe MyMvcApplication.dll /startupConfigurationFile=”..\\web.config”
```

Podczas uruchamiania migrate.exe tylko obowiązkowy parametr jest zestawu, który jest zestaw, który zawiera migracji, które próbujesz uruchomić, lecz będzie używać konwencji wszystkie na podstawie ustawienia, jeśli nie określisz pliku konfiguracji.

## <a name="migrate-to-a-specific-migration"></a>Migrowanie do dotyczące migracji

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /targetMigration=”AddTitle”
```

Jeśli chcesz uruchomić migracje maksymalnie dotyczące migracji, można określić nazwę migracji. Spowoduje to uruchomienie wszystkich poprzednich migracji zgodnie z wymaganiami aż do migracji, określić.

## <a name="specify-working-directory"></a>Określ katalog roboczy

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /startupDirectory=”c:\\MyApp”
```

Jeśli użytkownik zestawu ma zależności lub odczytuje pliki względem katalogu roboczego należy ustawić startupDirectory.

## <a name="specify-migration-configuration-to-use"></a>Określ konfigurację migracji w celu użycia

``` console
Migrate.exe MyAssembly CustomConfig /startupConfigurationFile=”..\\web.config”
```

Jeśli masz wiele klas konfiguracji migracji, klas dziedziczących DbMigrationConfiguration, należy określić, który ma być używany dla wykonania. To jest określona, zapewniając opcjonalny drugi parametr bez przełącznika jako powyżej.

## <a name="provide-connection-string"></a>Podaj parametry połączenia

``` console
Migrate.exe BlogDemo.dll /connectionString=”Data Source=localhost;Initial Catalog=BlogDemo;Integrated Security=SSPI” /connectionProviderName=”System.Data.SqlClient”
```

Jeśli chcesz określić parametry połączenia, w wierszu polecenia, należy również podać nazwę dostawcy. Nazwa dostawcy nie został podany spowoduje, że wyjątek.

## <a name="common-problems"></a>Typowe problemy

| Komunikat o błędzie                                                                                                                                                                                                                                                                                                                      | Rozwiązanie                                                                                                                                                                                                                                                                                             |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nieobsługiwany wyjątek: System.IO.FileLoadException: nie można załadować pliku lub zestawu "platformy EntityFramework, wersja = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089" lub jednej z jego zależności. Definicja manifestu zestawu znajduje się niezgodna odwołanie do zestawu. (Wyjątek od HRESULT: 0x80131040)         | Zwykle oznacza to, czy używasz aplikacji .NET 4, bez pliku Redirect.config. Należy skopiować Redirect.config do tej samej lokalizacji co migrate.exe i zmień jego nazwę na migrate.exe.config.                                                                                       |
| Nieobsługiwany wyjątek: System.IO.FileLoadException: nie można załadować pliku lub zestawu "platformy EntityFramework, wersja = 4.4.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089" lub jednej z jego zależności. Definicja manifestu zestawu znajduje się niezgodna odwołanie do zestawu. (Wyjątek od HRESULT: 0x80131040)          | Ten wyjątek oznacza, że działają .NET 4.5 aplikacji przy użyciu Redirect.config skopiowane do lokalizacji migrate.exe. Jeśli Twoja aplikacja jest .NET 4.5 nie trzeba mieć plik konfiguracji z przekierowywaniem wewnątrz. Usuń plik migrate.exe.config.                                    |
| Błąd: Nie można zaktualizować bazy danych do dopasowania w bieżącym modelu, ponieważ istnieją oczekujące zmiany, a następnie automatycznej migracji jest wyłączona. Zapisać zmiany oczekujące modelu migracji za pomocą kodu lub umożliwić automatyczną migrację. Ustaw DbMigrationsConfiguration.AutomaticMigrationsEnabled na wartość PRAWDA, aby włączyć automatyczną migrację. | Ten błąd występuje podczas uruchamiania migracji nie utworzono migracji, aby poradzić sobie ze zmianami wprowadzonymi w modelu, gdy baza danych nie jest zgodny z modelem. Dodawanie właściwości do klasy modelu, ponownie uruchomić migrate.exe bez tworzenia migracji, aby uaktualnić bazę danych jest przykładem. |
| Błąd: Typ nie zostanie rozwiązany dla elementu członkowskiego "System.Data.Entity.Migrations.Design.ToolingFacade+UpdateRunner,EntityFramework, Version = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089".                                                                                                                                       | Ten błąd może być spowodowany przez określenie katalogu startup niepoprawne. Musi to być lokalizacja migrate.exe                                                                                                                                                                                      |
| Nieobsługiwany wyjątek: System.NullReferenceException: odwołanie nie ustawione na wystąpienie obiektu do obiektu. <br/>   w System.Data.Entity.Migrations.Console.Program.Main (String [] args)                                                                                                                                             | Może to być spowodowane nie został podany wymagany parametr dla scenariusza, którego używasz. Na przykład określenie parametrów połączenia bez określenia nazwy dostawcy.                                                                                                                        |
| Błąd: znaleziono więcej niż jeden typ konfiguracji migracji w zestawie "ClassLibrary1". Określ nazwę, aby korzystać.                                                                                                                                                                                                  | Jak opisu błędu wynika, ma więcej niż jedną klasę konfiguracji w danym zestawie. Należy użyć przełącznika /configurationType, aby określić, której mają zostać użyte.                                                                                                                                           |
| Błąd: Nie można załadować pliku lub zestawu '&lt;assemblyName&gt;"lub jednej z jego zależności. Dany zestaw nazwy lub codebase był nieprawidłowy. (Wyjątek od HRESULT: 0x80131047)                                                                                                                                                    | Może to być spowodowane przez niepoprawnie określający nazwę zestawu, lub nie ma                                                                                                                                                                                                                          |
| Błąd: Nie można załadować pliku lub zestawu '&lt;assemblyName&gt;"lub jednej z jego zależności. Próbowano załadować program w niepoprawnym formacie.                                                                                                                                                                          | Dzieje się tak, jeśli chcesz uruchamiać migrate.exe x64 aplikacji. EF 5.0 i poniżej spowoduje działają tylko na x86.                                                                                                                                                                                |
