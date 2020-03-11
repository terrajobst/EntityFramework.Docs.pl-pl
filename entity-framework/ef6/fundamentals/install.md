---
title: Pobierz Entity Framework-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 2bdec6a9be228fbe934d0f46aa1bfafdfb2c971c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419469"
---
# <a name="get-entity-framework"></a>Pobieranie platformy Entity Framework
Entity Framework składa się z narzędzi EF dla programu Visual Studio i środowiska uruchomieniowego EF.

## <a name="ef-tools-for-visual-studio"></a>Narzędzia EF Tools for Visual Studio

Entity Framework Tools dla programu Visual Studio zawiera kreatora Dr Designer i dr Model Wizard i są wymagane dla pierwszej bazy danych i modeluje pierwsze przepływy pracy. Narzędzia Dr są dołączone do wszystkich najnowszych wersji programu Visual Studio. Jeśli wykonujesz niestandardową instalację programu Visual Studio, musisz upewnić się, że element "Entity Framework 6 Tools" jest wybierany przez wybranie obciążenia, które je zawiera, lub wybranie go jako pojedynczego składnika.

W przypadku niektórych wcześniejszych wersji programu Visual Studio zaktualizowane narzędzia EF są dostępne jako pobrane. Zobacz [wersje programu Visual Studio](~/ef6/what-is-new/visual-studio.md) , aby uzyskać wskazówki dotyczące uzyskiwania najnowszej wersji narzędzi EF dostępnych dla używanej wersji programu Visual Studio.

## <a name="ef-runtime"></a>Środowisko uruchomieniowe EF

Najnowsza wersja Entity Framework jest dostępna jako [pakiet NuGet EntityFramework](https://nuget.org/packages/EntityFramework/). Jeśli nie znasz Menedżera pakietów NuGet, zachęcamy do przeczytania [omówienia narzędzia NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).

### <a name="installing-the-ef-nuget-package"></a>Instalowanie pakietu NuGet EF

Aby zainstalować pakiet EntityFramework, kliknij prawym przyciskiem myszy folder **odwołania** projektu i wybierz polecenie **Zarządzaj pakietami NuGet...**

![Zarządzanie pakietami NuGet](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Instalowanie z konsoli Menedżera pakietów

Alternatywnie możesz zainstalować EntityFramework, uruchamiając następujące polecenie w [konsoli Menedżera pakietów](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>Instalowanie określonej wersji programu EF

Z EF 4,1, nowe wersje środowiska uruchomieniowego EF zostały wydane jako [pakiet NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/). Dowolną z tych wersji można dodać do projektu opartego na .NET Framework, uruchamiając następujące polecenie w [konsoli Menedżera pakietów](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)programu Visual Studio:

``` powershell
Install-Package EntityFramework -Version <number>
```

Należy pamiętać, że `<number>` reprezentuje określoną wersję EF do zainstalowania. Na przykład 6.2.0 jest wersją numeru dla EF 6,2.   

Środowiska uruchomieniowe EF przed 4,1 były częścią .NET Framework i nie można ich instalować oddzielnie.

### <a name="installing-the-latest-preview"></a>Instalowanie najnowszej wersji zapoznawczej

Powyższe metody zapewniają najnowszą w pełni obsługiwaną wersję Entity Framework. Dostępne są często wstępne wersje Entity Framework dostępnych, które można wypróbować i przekazać nam swoją opinię.

Aby zainstalować najnowszą wersję zapoznawczą EntityFramework, możesz wybrać opcję **Uwzględnij wersję wstępną** w oknie Zarządzaj pakietami NuGet. Jeśli wersje wstępne nie są dostępne, automatycznie otrzymasz najnowszą w pełni obsługiwaną wersję programu Entity Framework.

![Uwzględnij wersję wstępną](~/ef6/media/includeprerelease.png)

Alternatywnie możesz uruchomić następujące polecenie w [konsoli Menedżera pakietów](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework -Pre
```
