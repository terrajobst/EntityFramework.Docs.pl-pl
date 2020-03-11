---
title: Wybieranie wersji środowiska uruchomieniowego Entity Framework dla modeli programu EF Designer — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418148"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>Wybieranie wersji środowiska uruchomieniowego Entity Framework dla modeli programu EF Designer
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.

Począwszy od EF6, dodano następujący ekran do projektanta EF, aby umożliwić wybranie wersji środowiska uruchomieniowego, która ma być docelowa podczas tworzenia modelu. Ekran pojawi się, gdy Najnowsza wersja Entity Framework nie jest jeszcze zainstalowana w projekcie. Jeśli Najnowsza wersja jest już zainstalowana, zostanie użyta domyślnie.

![Wyświetla](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>Element docelowy EF6. x

Aby dodać środowisko uruchomieniowe EF6 do projektu, możesz wybrać EF6 z ekranu "Wybierz swoją wersję". Po dodaniu EF6u ten ekran zostanie zatrzymany w bieżącym projekcie.

EF6 zostanie wyłączona, jeśli masz już zainstalowaną starszą wersję EF (ponieważ nie można określić wielu wersji środowiska uruchomieniowego z tego samego projektu). Jeśli opcja EF6 nie jest włączona w tym miejscu, wykonaj następujące kroki, aby uaktualnić projekt do EF6:

1.  Kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań i wybierz pozycję **Zarządzaj pakietami NuGet...**
2.  Wybierz **aktualizacje**
3.  Wybierz pozycję **EntityFramework** (Upewnij się, że zamierzasz ją zaktualizować do wybranej wersji)
4.  Kliknij pozycję **Update** (Aktualizuj)

 

## <a name="targeting-ef5x"></a>Element docelowy EF5. x

Aby dodać środowisko uruchomieniowe EF5 do projektu, możesz wybrać EF5 z ekranu "Wybierz swoją wersję". Po dodaniu EF5 nadal zobaczysz ekran z opcją EF6 wyłączona.

Jeśli masz już zainstalowaną wersję EF4. x środowiska uruchomieniowego, zobaczysz tę wersję EF podaną na ekranie, a nie EF5. W tej sytuacji można przeprowadzić uaktualnienie do EF5, wykonując następujące czynności:

1.  Wybieranie **narzędzi-&gt; Library Package Manager —&gt; konsoli Menedżera pakietów**
2.  Uruchom **install-package EntityFramework — wersja 5.0.0**

 

## <a name="targeting-ef4x"></a>Element docelowy EF4. x

Środowisko uruchomieniowe EF4. x można zainstalować w projekcie, wykonując następujące czynności:

1.  Wybieranie **narzędzi-&gt; Library Package Manager —&gt; konsoli Menedżera pakietów**
2.  Uruchom **install-package EntityFramework — wersja 4.3.0**
