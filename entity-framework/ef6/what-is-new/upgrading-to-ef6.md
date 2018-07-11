---
title: Uaktualnianie do programu Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
caps.latest.revision: 3
ms.openlocfilehash: d22f0d686e6b8e3922d37245386af7723bf08ba1
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949267"
---
# <a name="upgrading-to-entity-framework-6"></a>Uaktualnianie do programu Entity Framework 6

W poprzednich wersjach programu EF kod został podzielony między podstawowe biblioteki (przede wszystkim System.Data.Entity.dll) dostarczana jako część programu .NET Framework i poza pasmem (OOB) biblioteki (przede wszystkim EntityFramework.dll) dostarczana z pakietu NuGet. Programy EF6 przyjmuje kod z bibliotekami podstawowych i dołącza go do biblioteki OOB. Jest to niezbędne w celu umożliwienia EF ma zostać wykonane "open source" i aby można było podlegać ewolucji w różnym tempie z .NET Framework. Skutkiem tego jest aplikacji będzie konieczne można odbudować przeniesionych typów.

Powinna to być proste dla aplikacji, które korzystają z typu DbContext jako wysłane w programie EF 4.1 i nowszych. Trochę więcej pracy jest wymagane dla aplikacji, które korzystają z obiektu ObjectContext, ale nadal nie jest ciężko.

Poniżej przedstawiono listę kontrolną czynności, które należy wykonać, aby uaktualnić istniejącą aplikację platformy EF6.

## <a name="1-install-the-ef6-nuget-package"></a>1. Zainstaluj pakiet NuGet platformy EF6

Należy uaktualnić do nowego środowiska uruchomieniowego platformy Entity Framework 6.

1. Kliknij prawym przyciskiem myszy na projekt i wybierz **Zarządzaj pakietami NuGet...**  
2. W obszarze **Online** wybierz opcję **EntityFramework** i kliknij przycisk **instalacji**  
   > [!NOTE]
   > Jeśli poprzednią wersję został zainstalowany pakiet NuGet platformy EntityFramework to spowoduje uaktualnienie go do platformy EF6.

Alternatywnie można uruchom następujące polecenie z poziomu konsoli Menedżera pakietów:

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. Upewnij się, czy odwołania do zestawów do System.Data.Entity.dll zostały usunięte

Instalowanie pakietu EF6 NuGet należy automatycznie usunąć wszystkie odwołania do System.Data.Entity z projektu.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. Zamień wszystkie modele EF projektanta (EDMX) umożliwia generowanie kodu w wersji 6.x EF

Jeśli masz wszystkie modele utworzone za pomocą projektanta EF, należy zaktualizować szablonów generowania kodu, aby wygenerować kod zgodny EF6.

> [!NOTE]
> Brak dostępnych obecnie tylko EF 6.x DbContext Generator szablonów dla programu Visual Studio 2012 i 2013.

1. Usuwanie istniejących szablonów generowania kodu. Te pliki będą zwykle miały nazwy nadane  **\<edmx_file_name\>.tt** i  **\<edmx_file_name\>. Context.TT** i można zagnieździć w pliku edmx w Eksploratorze rozwiązań. Szablony można wybrać w Eksploratorze rozwiązań, a następnie naciśnij klawisz **Del** klawisz, aby je usunąć.  
   > [!NOTE]
   > W projektów witryny sieci Web szablonów będzie nie być zagnieżdżony w pliku edmx, ale wymienione obok niej w Eksploratorze rozwiązań.  

   > [!NOTE]
   > W projektach VB.NET, musisz włączyć "Pokaż wszystkie pliki" można było wyświetlić pliki zagnieżdżonych szablonów.
2. Dodaj odpowiedni szablon generowanie kodu w wersji 6.x EF. Otwieranie modelu w Projektancie platformy EF, kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Dodaj element generowanie kodu...**
    - Jeśli używasz interfejsu API typu DbContext (zalecane) następnie **EF 6.x DbContext Generator** będą dostępne w ramach **danych** kartę.  
      > [!NOTE]
      > Jeśli używasz programu Visual Studio 2012 należy zainstalować narzędzia EF 6, aby ten szablon. Zobacz [uzyskać Entity Framework](~/ef6/fundamentals/install.md) Aby uzyskać szczegółowe informacje.  

    - Jeśli korzystasz z interfejsu API obiektu ObjectContext to będzie konieczne wybranie **Online** kartę i wyszukaj **EF 6.x EntityObject Generator**.  
3. Jeśli jakiekolwiek dostosowania są stosowane do szablonów generowania kodu, należy ponownie zastosować je do zaktualizowanych szablonów.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. Aktualizowanie przestrzeni nazw dla wszystkich typów EF core używane

Przestrzenie nazw dla typu DbContext i Code First nie uległy zmianie. To oznacza dla wielu aplikacji, które używają programu EF 4.1 lub nowszym będzie trzeba wprowadzić zmiany.

Typów, takich jak ObjectContext, które były wcześniej System.Data.Entity.dll zostały przeniesione do nowej przestrzeni nazw. Oznacza to, że może być konieczne zaktualizowanie swoje *przy użyciu* lub *importu* dyrektywy algorytmie EF6.

Ogólną zasadą zmian przestrzeni nazw jest, że dowolnego typu w System.Data.* jest przenoszony do System.Data.Entity.Core.*. Innymi słowy, wystarczy wstawić **Entity.Core.** Po dane systemowe. Na przykład:

- System.Data.EntityException = > dane systemowe. **Entity.Core.** EntityException  
- System.Data.Objects.ObjectContext = > dane systemowe. **Entity.Core.** Objects.ObjectContext  
- System.Data.Objects.DataClasses.RelationshipManager = > dane systemowe. **Entity.Core.** Objects.DataClasses.RelationshipManager  

Te typy są w *Core* przestrzenie nazw, ponieważ nie są używane bezpośrednio w przypadku większości aplikacji na podstawie typu DbContext. Niektóre typy, które były częścią System.Data.Entity.dll są nadal używane najczęściej i bezpośrednio do aplikacji opartych na DbContext i dlatego nie zostały przeniesione do *Core* przestrzeni nazw. Są to:

- System.Data.EntityState = > dane systemowe. **Jednostki.** EntityState  
- System.Data.Objects.DataClasses.EdmFunctionAttribute = > dane systemowe. **Entity.DbFunctionAttribute**  
  > [!NOTE]
  > Ta klasa została zmieniona; Klasa o starej nazwie nadal istnieje i działa, ale jej oznaczone jako przestarzałe.  
- System.Data.Objects.EntityFunctions = > dane systemowe. **Entity.DbFunctions**  
  > [!NOTE]
  > Ta klasa została zmieniona; Klasa o starej nazwie nadal istnieje i działa, ale teraz oznaczone jako przestarzałe).  
- Przestrzenne klasy (na przykład DbGeography, DbGeometry) zostały przeniesione z System.Data.Spatial = > dane systemowe. **Jednostki.** Przestrzenne

> [!NOTE]
> Niektóre typy w przestrzeni nazw System.Data znajdują się w System.Data.dll, który nie jest zestawem EF. Te typy nie zostały przeniesione, i dlatego ich przestrzenie nazw pozostają niezmienione.
