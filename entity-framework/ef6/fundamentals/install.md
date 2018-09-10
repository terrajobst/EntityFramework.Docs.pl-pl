---
title: Pobieranie programu Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 601f8d123d5494be6a658da1c4ad3743ed50385c
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250884"
---
# <a name="get-entity-framework"></a>Pobieranie programu Entity Framework
Entity Framework składa się z narzędziami EF dla programu Visual Studio i środowiska uruchomieniowego EF.

## <a name="ef-tools-for-visual-studio"></a>EF Tools for Visual Studio

Narzędzi Entity Framework Tools for Visual Studio obejmują w kreatorze modelu platformy EF i projektancie platformy EF są wymagane dla bazy danych po raz pierwszy i modelowanie pierwszy przepływów pracy. Narzędzia platformy EF znajdują się w najnowszych wersjach programu Visual Studio. Jeśli wykonać niestandardową instalację programu Visual Studio, musisz upewnić się, że element "Entity Framework 6 Tools" jest zaznaczone, wybierając obciążenia zawierający go lub wybierając go jako poszczególnych składników.

W przypadku niektórych poprzednich wersji programu Visual Studio zaktualizowanych narzędzi EF są dostępne do pobrania. Zobacz [wersje serwera Visual Studio](~/ef6/what-is-new/visual-studio.md) wskazówki dotyczące sposobu uzyskania najnowszej wersji programu EF narzędzi dostępnych dla używanej wersji programu Visual Studio.

## <a name="ef-runtime"></a>Środowiska uruchomieniowego EF

Najnowszą wersję programu Entity Framework jest dostępna jako [pakiet NuGet platformy EntityFramework](http://nuget.org/packages/EntityFramework/). Jeśli użytkownik nie jest zaznajomiony z Menedżera pakietów NuGet, zachęcamy do odczytania [Przegląd NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).

### <a name="installing-the-ef-nuget-package"></a>Instalowanie pakietu NuGet programu EF

Można zainstalować pakiet platformy EntityFramework, klikając prawym przyciskiem myszy **odwołania** folderu projektu i wybierając polecenie **Zarządzaj pakietami NuGet...**

![Zarządzaj pakietami NuGet](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Instalowanie za pomocą konsoli Menedżera pakietów

Alternatywnie, można zainstalować platformy EntityFramework, uruchamiając następujące polecenie w [Konsola Menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>Instalowanie określonej wersji programu EF

Od wersji 4.1 EF wydano nowe wersje środowiska uruchomieniowego EF jako [pakiet NuGet platformy EntityFramework](https://www.nuget.org/packages/EntityFramework/). Dowolne z tych wersji można dodać do projektu na podstawie .NET Framework, uruchamiając następujące polecenie w programie Visual Studio [Konsola Menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):

``` powershell
Install-Package EntityFramework -Version <number>
```

Należy pamiętać, że `<number>` reprezentuje określoną wersję platformy EF do zainstalowania. Na przykład 6.2.0 stanowi wersję numer EF 6.2.   

EF środowisk wykonawczych, zanim 4.1 były częścią środowiska .NET Framework i nie można zainstalować oddzielnie.

### <a name="installing-the-latest-preview"></a>Instalowanie najnowszej wersji zapoznawczej

Powyższych metod prześle Ci najnowsze wydania Entity Framework w pełni obsługiwane. Często są wersje wstępne programu Entity Framework jest dostępna, że chętnie można wypróbować, a następnie Prześlij nam opinię na temat.

Do zainstalowania najnowszej wersji zapoznawczej platformy EntityFramework, możesz wybrać **Uwzględnij wydania wstępne** w oknie Zarządzanie pakietami NuGet. Jeśli nie wstępnej wersji są dostępne najnowsze zostanie automatycznie pobrana w pełni obsługiwana wersja programu Entity Framework.

![Uwzględnij wersję wstępną](~/ef6/media/includeprerelease.png)

Alternatywnie, możesz uruchomić następujące polecenie w [Konsola Menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework -Pre
```
