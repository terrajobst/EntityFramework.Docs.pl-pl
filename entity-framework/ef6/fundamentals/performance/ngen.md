---
title: Zwiększanie wydajności uruchamiania za pomocą narzędzia NGen — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
caps.latest.revision: 4
ms.openlocfilehash: cffd2deea3148a16ed704d1e5e7b365eda06f72b
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949049"
---
# <a name="improving-startup-performance-with-ngen"></a>Zwiększanie wydajności uruchamiania za pomocą narzędzia NGen
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.  

Program .NET Framework obsługuje generowanie obrazów natywnych dla zarządzanych aplikacji i bibliotek, aby ułatwić szybsze uruchamianie aplikacji, a także w niektórych przypadkach Użyj mniejszej ilości pamięci. Obrazy natywne są tworzone przez tłumaczenie zestawów kodu zarządzanego w plikach zawierających instrukcji maszyny macierzystej, zanim aplikacja zostanie wykonany, zwalniania kompilatora .NET JIT (Just-In-Time), trzeba wygenerować macierzysty zgodnie z instrukcjami na środowisko uruchomieniowe aplikacji.  

Przed wersją 6 środowiska uruchomieniowego EF core biblioteki były częścią programu .NET Framework i obrazy natywne są generowane automatycznie dla nich. Począwszy od wersji 6 całego środowiska uruchomieniowego EF została połączona w pakiet NuGet platformy EntityFramework. Obrazy natywne mają do chwili obecnej można wygenerować za pomocą narzędzia wiersza polecenia NGen.exe, aby uzyskać wyniki podobne.  

Empiryczne obserwacje pokazują, że obrazy natywne zestawów środowiska uruchomieniowego EF można wyciąć od 1 do 3 sekund czas uruchamiania aplikacji.  

## <a name="how-to-use-ngenexe"></a>Jak używać NGen.exe  

Najbardziej podstawowa funkcja narzędzia NGen.exe jest "Zainstaluj" (oznacza to, aby utworzyć i zachować na dysku) obrazów natywnych dla zestawu i wszystkich jego zależności bezpośrednich. Poniżej przedstawiono, jak można uzyskać, który:  

1. Otwórz okno wiersza polecenia jako administrator  
2. Zmień bieżący katalog roboczy do lokalizacji zestawów, który chcesz wygenerować obrazów natywnych dla:  

  ``` console
    cd <*Assemblies location*>  
  ```
3. W zależności od używanego systemu operacyjnego i konfiguracji aplikacji może być konieczne generuje obrazy natywne dla architektury 32-bitowej i 64-bitowa architektura lub obu.  

    Dla 32-bitowych, uruchom:  
  ``` console
    %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
  ```
    Dla 64-bitowy, uruchom:
  ``` console
    %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
  ```

> [!TIP]
> Generowanie obrazów macierzystych dla niewłaściwego architektury jest bardzo Częsty błąd. W razie wątpliwości można po prostu generowanie obrazów natywnych dla wszystkich architektur, które dotyczą system operacyjny zainstalowany na maszynie.  

NGen.exe obsługuje także inne funkcje, takie jak odinstalowywanie i wyświetlanie zainstalowanych obrazów natywnych, kolejkowania generowania wielu obrazów itd. Aby uzyskać więcej szczegółów dotyczących użycia przeczytaj [dokumentacji NGen.exe](https://msdn.microsoft.com/library/6t9t5wcf.aspx).  

## <a name="when-to-use-ngenexe"></a>Kiedy należy używać NGen.exe  

Jeśli chodzi o określić zestawy, które generuje obrazy natywne dla w aplikacji opartej na EF w wersji 6 lub nowszego, należy rozważyć następujące opcje:  

- **Głównym zestawie środowiska uruchomieniowego EF, EntityFramework.dll**: Typowa aplikacja na podstawie EF jest wykonywany znacząca ilość kodu z tego zestawu podczas uruchamiania lub w jego pierwszego dostępu do bazy danych. W związku z tym tworzenie obrazów natywnych tego zestawu powoduje wygenerowanie największy wzrost wydajności uruchamiania.  
- **Każdy zespół dostawcy EF używanych przez aplikację**: czas uruchamiania mogą również zyskać nieco dzięki generowanie obrazów macierzystych, które z nich. Na przykład jeśli aplikacja używa dostawcy EF dla programu SQL Server należy wygenerować obraz natywny dla EntityFramework.SqlServer.dll.  
- **Zestawy aplikacji i inne zależności**: [dokumentacji NGen.exe](https://msdn.microsoft.com/library/6t9t5wcf.aspx) opisano ogólne kryteria wyboru zestawy, które można wygenerować obrazów natywnych dla i wpływu na obrazy natywne dotyczący zabezpieczeń, Zaawansowane opcje, takie jak "trwałego powiązania" scenariuszy, takich jak przy użyciu obrazów natywnych w debugowania i profilowania w scenariuszach itp.  

> [!TIP]
> Upewnij się, starannie mierzenie wpływu na wydajność uruchamiania i ogólną wydajność aplikacji przy użyciu obrazów macierzystych i porównać ich rzeczywiste wymagania. Natomiast obrazy natywne pomoże ogólnie poprawy uruchomienia wydajność i w niektórych przypadkach zmniejszenie zużycia pamięci, nie wszystkie scenariusze skorzystają równomiernie. Na przykład w stanie stabilności wykonywania (gdy co najmniej raz wywołania wszystkich metod, które są używane przez aplikację) kod wygenerowany przez kompilator JIT może w rzeczywistości przynieść wydajność nieco lepiej niż obrazy natywne.  

## <a name="using-ngenexe-in-a-development-machine"></a>Za pomocą NGen.exe w komputerze deweloperskim  

Podczas tworzenia .NET JIT kompilatora oferuje najlepsze rozwiązanie ogólne dla kodu, które zmieniają się często. Generowanie obrazów macierzystych skompilowanych zależności, takich jak zestawy środowiska uruchomieniowego EF może pomóc przyspieszyć programowanie i testowanie przez wycinanie kilka sekund na początku każdego wykonania.  

Dobrym miejscem do śledzenia Znajdź zestawy środowiska uruchomieniowego EF jest lokalizacja pakietu NuGet dla rozwiązania. Na przykład w aplikacji przy użyciu programu EF 6.0.2 z programem SQL Server i przeznaczonych dla platformy .NET 4.5 lub nowszej w oknie wiersza polecenia można wpisać następujące (Pamiętaj otworzyć go jako administrator):  

``` console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> To wykorzystuje fakt, że instalowanie obrazów natywnych dla platformy EF dostawcy dla programu SQL Server zostanie również domyślnie zainstalowany obrazy natywne dla zestawu głównego środowiska uruchomieniowego EF. To działa, ponieważ NGen.exe może wykryć, czy EntityFramework.dll jest bezpośrednia zależność zestawu EntityFramework.SqlServer.dll, znajdującego się w tym samym katalogu.  

## <a name="creating-native-images-during-setup"></a>Tworzenie obrazów natywnych podczas instalacji  

Zestaw narzędzi WiX obsługuje kolejkowania generowanie obrazów natywnych dla zestawów zarządzanych podczas instalacji, jak wyjaśniono w tym [poradnik](http://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html). Kolejny alternatywny sposób polega na utworzeniu zadania instalacji niestandardowej, wykonaj polecenie NGen.exe.  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a>Weryfikowanie, czy obrazy natywne są używane na platformie EF  

Można sprawdzić, czy określona aplikacja używa natywny zestaw, wyszukując załadowanych zestawów, które mają rozszerzenie ". ni.dll"or". ni.exe". Na przykład obraz natywny dla zestawu głównego środowiska uruchomieniowego EF zostanie wywołana EntityFramework.ni.dll. Prosty sposób sprawdzić załadowanych zestawów .NET procesu jest użycie [Eksplorator procesów](https://technet.microsoft.com/sysinternals/bb896653).  

## <a name="other-things-to-be-aware-of"></a>Inne zagadnienia, które należy pamiętać o  

**Tworzenie obrazu natywnego zestawu, nie należy mylić z rejestrowaniem zestawu w [globalnej pamięci podręcznej zestawów (Global Assembly Cache)](https://msdn.microsoft.com/library/yf1d93sz.aspx)**. NGen.exe umożliwia tworzenie obrazów zestawów, które nie znajdują się w pamięci podręcznej GAC, a w rzeczywistości wielu zastosowań, które korzystały z określonej wersji EF mogą udostępniać tego samego obrazu natywnego. Windows 8 może automatycznie tworzyć obrazy natywne dla zestawów umieszczane w pamięci podręcznej GAC, środowiska uruchomieniowego EF jest zoptymalizowany do wdrożenia wraz z aplikacji i nie jest zalecane rejestrowanie w pamięci podręcznej GAC, ponieważ ma negatywny wpływ na rozpoznawania zestawu i Obsługa aplikacji wśród innych aspektów.  
