---
title: Wybieranie jednostki wersję środowiska uruchomieniowego EF projektanta modeli - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 8864bb8166a7c16455d5c3bebe91e2ce8d142685
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998245"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>Wybieranie jednostki wersję środowiska uruchomieniowego EF projektanta modeli
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.

Począwszy od platformy EF6 dodano następujący ekran projektancie platformy EF, aby możliwe było wybrać wersję środowiska uruchomieniowego, który ma pod kątem podczas jego tworzenia. Ekran pojawi się na najnowszą wersję programu Entity Framework nie jest już zainstalowany w projekcie. Jeśli już jest zainstalowana najnowsza wersja będzie używany tylko domyślnie.

![Ekran](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>Określanie wartości docelowej EF6.x

Możesz wybrać platformy EF6 z ekranu wybierz wersję usługi, aby dodać środowiska uruchomieniowego platformy EF6 do projektu. Po dodaniu EF6 należy zatrzymać, widzisz ten ekran w bieżącym projekcie.

EF6 zostanie wyłączona, jeśli masz już starszą wersję programu EF zainstalowany (ponieważ nie może wskazać wiele wersji środowiska uruchomieniowego, w tym samym projekcie). Jeśli opcja EF6 nie jest włączona w tym miejscu, wykonaj następujące kroki, aby uaktualnić projekt do EF6:

1.  Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Zarządzaj pakietami NuGet...**
2.  Wybierz **aktualizacji**
3.  Wybierz **EntityFramework** (Upewnij się, będzie ona konieczność jej zaktualizowania do wersji mają)
4.  Kliknij przycisk **aktualizacji**

 

## <a name="targeting-ef5x"></a>Określanie wartości docelowej EF5.x

Możesz wybrać EF5 z ekranu wybierz wersję usługi, aby dodać środowiska uruchomieniowego EF5 do projektu. Po dodaniu EF5 nadal zobaczysz ekran z opcją EF6 wyłączone.

Jeśli masz EF4.x wersję środowiska uruchomieniowego już zainstalowanego zostanie wyświetlone tej wersji na liście ekranu, a nie EF5 EF. W takiej sytuacji można przeprowadzić uaktualnienie do EF5 wykonując następujące czynności:

1.  Wybierz **Tools -&gt; Menedżer pakietów biblioteki -&gt; Konsola Menedżera pakietów**
2.  Uruchom **EntityFramework instalacji pakietu-wersja 5.0.0**

 

## <a name="targeting-ef4x"></a>Określanie wartości docelowej EF4.x

Można zainstalować środowisko uruchomieniowe EF4.x do projektu wykonując następujące czynności:

1.  Wybierz **Tools -&gt; Menedżer pakietów biblioteki -&gt; Konsola Menedżera pakietów**
2.  Uruchom **EntityFramework instalacji pakietu — w wersji 4.3.0**
