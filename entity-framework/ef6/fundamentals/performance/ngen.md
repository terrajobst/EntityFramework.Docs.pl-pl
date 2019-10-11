---
title: Poprawianie wydajności uruchamiania za pomocą narzędzia NGen-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
ms.openlocfilehash: c9b5f8a06add9133d30955e3cc97a92e9b189bdf
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182682"
---
# <a name="improving-startup-performance-with-ngen"></a>Poprawianie wydajności uruchamiania za pomocą narzędzia NGen
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.  

.NET Framework obsługuje generowanie obrazów natywnych dla zarządzanych aplikacji i bibliotek w celu ułatwienia uruchamiania aplikacji, a także w niektórych przypadkach użycie mniejszej ilości pamięci. Obrazy natywne są tworzone przez przetłumaczenie zestawów kodu zarządzanego do plików zawierających natywne instrukcje maszyn przed wykonaniem aplikacji, co uwalnia kompilator .NET JIT (just-in-Time) przed wygenerowaniem natywnych instrukcji w środowisko uruchomieniowe aplikacji.  

Przed wersjami 6 podstawowe biblioteki środowiska uruchomieniowego EF były częścią .NET Framework i obrazy natywne zostały automatycznie wygenerowane dla nich. Począwszy od wersji 6, całe środowisko uruchomieniowe EF zostało połączone z pakietem NuGet EntityFramework. Obrazy natywne muszą być teraz generowane przy użyciu narzędzia wiersza polecenia NGen. exe, aby uzyskać podobne wyniki.  

Obserwacje eksperymentalne pokazują, że obrazy natywne zestawów środowiska uruchomieniowego EF mogą być obcinane od 1 do 3 sekund czasu uruchamiania aplikacji.  

## <a name="how-to-use-ngenexe"></a>Jak używać programu NGen. exe  

Najbardziej podstawową funkcją narzędzia NGen. exe jest "Install" (czyli do tworzenia i utrwalania na dysku) obrazów natywnych dla zestawu i wszystkich jego bezpośrednich zależności. Poniżej przedstawiono sposób, w jaki można to osiągnąć:  

1. Otwórz okno wiersza polecenia jako administrator  
2. Zmień bieżący katalog roboczy na lokalizację zestawów, dla których mają zostać wygenerowane obrazy natywne:  

  ``` console
    cd <*Assemblies location*>  
  ```
3. W zależności od używanego systemu operacyjnego i konfiguracji aplikacji może być konieczne wygenerowanie obrazów natywnych dla architektury 32-bitowej o wartości 64 lub obu.  

    W przypadku uruchomienia 32 bitowego:  
  ``` console
    %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
  ```
    W przypadku uruchomienia 64 bitowego:
  ``` console
    %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
  ```

> [!TIP]
> Generowanie obrazów natywnych dla niewłaściwej architektury jest bardzo powszechnym błędem. W razie wątpliwości można po prostu wygenerować obrazy natywne dla wszystkich architektur, które mają zastosowanie do systemu operacyjnego zainstalowanego na komputerze.  

Program NGen. exe obsługuje również inne funkcje, takie jak Odinstalowywanie i wyświetlanie zainstalowanych obrazów natywnych, kolejkowanie generacji wielu obrazów itd. Aby uzyskać szczegółowe informacje na temat użycia, zapoznaj się z [dokumentacją Ngen. exe](https://msdn.microsoft.com/library/6t9t5wcf.aspx).  

## <a name="when-to-use-ngenexe"></a>Kiedy używać NGen. exe  

Gdy chodzi o wybór zestawów do generowania obrazów natywnych dla aplikacji w oparciu o Dr w wersji 6 lub nowszej, należy wziąć pod uwagę następujące opcje:  

- **Główny zestaw środowiska uruchomieniowego EF, EntityFramework. dll**: Typowa aplikacja oparta na EF wykonuje znaczną ilość kodu z tego zestawu podczas uruchamiania lub pierwszego dostępu do bazy danych. W związku z tym tworzenie obrazów natywnych tego zestawu da największe zyski w zakresie wydajności uruchamiania.  
- **Dowolny zestaw dostawcy EF używany przez aplikację**: Czas uruchamiania może również nieco wzczerpać korzyści z generowania obrazów natywnych. Na przykład jeśli aplikacja używa dostawcy EF do SQL Server należy wygenerować obraz natywny dla EntityFramework. SqlServer. dll.  
- **Zestawy aplikacji i inne zależności**: [Dokumentacja programu Ngen. exe](https://msdn.microsoft.com/library/6t9t5wcf.aspx) obejmuje ogólne kryteria wyboru zestawów do generowania obrazów natywnych oraz wpływ obrazów natywnych na zabezpieczenia, zaawansowane opcje, takie jak "twarde wiązanie", takie jak używanie obrazów natywnych w debugowaniu i scenariusze profilowania itp.  

> [!TIP]
> Upewnij się, że dokładnie mierzy wpływ używania obrazów natywnych na wydajność uruchamiania i ogólną wydajność aplikacji oraz porównanie ich z rzeczywistymi wymaganiami. Chociaż obrazy natywne zwykle pomogą ulepszyć wydajność, a w niektórych przypadkach zmniejszają użycie pamięci, a nie wszystkie scenariusze byłyby równie korzystne. Na przykład w przypadku stałego wykonywania stanu (czyli, gdy wszystkie metody używane przez aplikację zostały wywołane co najmniej raz) kod wygenerowany przez kompilator JIT może w rzeczywistości spowodować nieco lepszą wydajność niż obrazy natywne.  

## <a name="using-ngenexe-in-a-development-machine"></a>Korzystanie z programu NGen. exe na komputerze deweloperskim  

Podczas opracowywania kompilator JIT platformy .NET oferuje najlepsze, ogólne kompromisy dla często zmieniających się kodów. Generowanie obrazów natywnych dla skompilowanych zależności, takich jak zestawy środowiska uruchomieniowego EF, mogą pomóc przyspieszyć programowanie i testowanie poprzez wycinanie kilku sekund na początku każdego wykonania.  

Dobrym miejscem, aby znaleźć zestawy środowiska uruchomieniowego EF, jest lokalizacja pakietu NuGet dla rozwiązania. Na przykład w przypadku aplikacji używającej EF 6.0.2 z SQL Server i przeznaczonym dla platformy .NET 4,5 lub nowszej można wpisać następujące polecenie w oknie wiersza polecenia (Pamiętaj, aby otworzyć go jako administrator):  

```console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> Pozwala to wykorzystać fakt, że zainstalowanie obrazów natywnych dla dostawcy EF dla SQL Server również domyślnie zainstaluje obrazy natywne dla głównego zestawu środowiska uruchomieniowego EF. Dzieje się tak, ponieważ program NGen. exe może wykryć, że EntityFramework. dll jest bezpośrednią zależnością zestawu EntityFramework. SqlServer. dll znajdującego się w tym samym katalogu.  

## <a name="creating-native-images-during-setup"></a>Tworzenie obrazów natywnych podczas instalacji  

Zestaw narzędzi WiX obsługuje Kolejkowanie obrazów natywnych dla zestawów zarządzanych podczas instalacji, zgodnie z opisem w tym [przewodniku](https://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html). Inną alternatywą jest utworzenie niestandardowego zadania konfiguracji, które wykonuje polecenie NGen. exe.  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a>Weryfikowanie, czy obrazy natywne są używane na potrzeby EF  

Możesz sprawdzić, czy określona aplikacja korzysta z zestawu natywnego, szukając załadowanych zestawów z rozszerzeniem ". ni. dll" lub ". ni. exe". Na przykład obraz macierzysty dla głównego zestawu środowiska uruchomieniowego EF będzie miał nazwę EntityFramework. ni. dll. Łatwym sposobem sprawdzenia załadowanych zestawów .NET procesu jest użycie [Eksploratora procesów](https://technet.microsoft.com/sysinternals/bb896653).  

## <a name="other-things-to-be-aware-of"></a>Inne zagadnienia, z którymi należy się zapoznać  

**Tworzenie obrazu natywnego zestawu nie należy mylić z rejestrowaniem zestawu w [globalnej pamięci podręcznej zestawów](https://msdn.microsoft.com/library/yf1d93sz.aspx)** . Program NGen. exe umożliwia tworzenie obrazów zestawów, które nie znajdują się w pamięci podręcznej, a w rzeczywistości wiele aplikacji, które używają określonej wersji EF, mogą współużytkować ten sam obraz macierzysty. System Windows 8 może automatycznie tworzyć obrazy natywne dla zestawów umieszczonych w pamięci podręcznej GAC, ale środowisko uruchomieniowe EF jest zoptymalizowane pod kątem wdrożenia wraz z aplikacją, a firma Microsoft nie zaleca rejestrowania go w pamięci podręcznej, ponieważ ma negatywny wpływ na rozdzielczość zestawu i Obsługa aplikacji między innymi aspektami.  
