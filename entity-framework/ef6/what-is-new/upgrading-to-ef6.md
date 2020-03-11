---
title: Uaktualnianie do Entity Framework 6 EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 4395a9c117a6cf38e7fc08f11ee689d6fffa6fed
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419655"
---
# <a name="upgrading-to-entity-framework-6"></a>Uaktualnianie do Entity Framework 6

W poprzednich wersjach EF kod został podzielony między biblioteki podstawowe (przede wszystkim system. Data. Entity. dll), które są dostarczane jako część bibliotek .NET Framework i out-of-Band (EntityFramework. dll) dostarczonych w pakiecie NuGet. EF6 Pobiera kod z podstawowych bibliotek i dołącza go do bibliotek OOB. Było to konieczne, aby zapewnić, że EF ma być typu open source i aby można go było rozwijać w innym tempie niż .NET Framework. W związku z tym aplikacje muszą być ponownie skompilowane względem przenoszonych typów.

Powinno to być proste w przypadku aplikacji korzystających z DbContext jako dostarczonych w EF 4,1 i nowszych. W przypadku aplikacji korzystających z obiektu ObjectContext wymagana jest nieco większa ilość pracy, ale nadal nie jest to trudne do wykonania.

Poniżej znajduje się lista kontrolna czynności, które należy wykonać, aby uaktualnić istniejącą aplikację do programu EF6.

## <a name="1-install-the-ef6-nuget-package"></a>1. Zainstaluj pakiet NuGet EF6

Należy uaktualnić do nowego środowiska uruchomieniowego Entity Framework 6.

1. Kliknij prawym przyciskiem myszy projekt i wybierz pozycję **Zarządzaj pakietami NuGet...**  
2. Na karcie **online** wybierz pozycję **EntityFramework** , a następnie kliknij pozycję **Instaluj** .  
   > [!NOTE]
   > Jeśli zainstalowano poprzednią wersję pakietu NuGet EntityFramework, spowoduje to uaktualnienie do EF6.

Alternatywnie można uruchomić następujące polecenie z poziomu konsoli Menedżera pakietów:

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. Upewnij się, że odwołania do zestawu System. Data. Entity. dll zostały usunięte.

Zainstalowanie pakietu NuGet EF6 powinno automatycznie usunąć wszystkie odwołania do elementu System. Data. Entity z projektu.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. Zamień wszystkie modele EF Designer (EDMX), aby użyć programu EF 6. x generowanie kodu

Jeśli masz jakieś modele utworzone za pomocą narzędzia Dr Designer, musisz zaktualizować szablony generowania kodu, aby wygenerować kod zgodny z EF6.

> [!NOTE]
> Obecnie dostępne są szablony generatora DbContext w programie Dr 6. x dla programu Visual Studio 2012 i 2013.

1. Usuń istniejące szablony generowania kodu. Te pliki mają zwykle nazwę **\<edmx_file_name\>. tt** i **\<edmx_file_name\>. Context.tt** i być zagnieżdżone w pliku edmx w Eksplorator rozwiązań. Możesz wybrać Szablony w Eksplorator rozwiązań i nacisnąć klawisz **del** , aby je usunąć.  
   > [!NOTE]
   > W projektach witryn sieci Web szablony nie będą zagnieżdżone w pliku edmx, ale wymienione obok niego w Eksplorator rozwiązań.  

   > [!NOTE]
   > W projektach VB.NET należy włączyć opcję "Pokaż wszystkie pliki", aby można było zobaczyć pliki szablonów zagnieżdżonych.
2. Dodaj odpowiedni szablon generowania kodu dla programu EF 6. x. Otwórz model w projektancie EF, kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Dodaj element generowania kodu...**
    - Jeśli używasz interfejsu API DbContext (zalecane), a następnie na karcie **dane** będzie dostępna usługa **Dr 6. x DbContext Generator** .  
      > [!NOTE]
      > W przypadku korzystania z programu Visual Studio 2012 należy zainstalować narzędzia Dr 6, aby mieć ten szablon. Aby uzyskać szczegółowe informacje, zobacz [pobieranie Entity Framework](~/ef6/fundamentals/install.md) .  

    - Jeśli używasz interfejsu API ObjectContext, musisz wybrać kartę **online** i wyszukać **generatora EntityObject programu EF 6. x**.  
3. W przypadku zastosowania dowolnych dostosowań do szablonów generowania kodu należy je ponownie zastosować do zaktualizowanych szablonów.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. Zaktualizuj obszary nazw dla wszystkich używanych podstawowych typów EF

Przestrzenie nazw dla typów DbContext i Code First nie uległy zmianie. Oznacza to, że dla wielu aplikacji używających EF 4,1 lub nowszych nie trzeba zmieniać żadnych elementów.

Typy takie jak ObjectContext, które wcześniej znajdowały się w System. Data. Entity. dll, zostały przeniesione do nowych przestrzeni nazw. Oznacza to, że może być konieczne zaktualizowanie dyrektywy *using* lub *Import* , aby kompilować z Ef6.

Ogólna reguła dotycząca zmian przestrzeni nazw polega na tym, że dowolny typ w System. Data. * jest przenoszony do System. Data. Entity. Core. *. Innymi słowy, po prostu Wstaw **jednostkę Entity. Core.** po elemencie System. Data. Na przykład:

- System. Data. EntityException = > System. Data. **Entity. Core**. EntityException  
- System. Data. Objects. ObjectContext = > System. Data. **Entity. Core**. Obiekty. ObjectContext  
- System. Data. Objects. DataClasses. RelationshipManager = > System. Data. **Entity. Core**. Obiekty. DataClasses. RelationshipManager  

Te typy znajdują się w *podstawowych* obszarach nazw, ponieważ nie są używane bezpośrednio dla większości aplikacji opartych na kontekście DbContext. Niektóre typy, które były częścią elementu System. Data. Entity. dll, są nadal używane często i bezpośrednio dla aplikacji opartych na kontekście DbContext, dlatego nie zostały przeniesione do *podstawowych* przestrzeni nazw. Są to:

- System. Data. EntityState = > System. Data. **Jednostka**. EntityState  
- System. Data. Objects. DataClasses. EdmFunctionAttribute = > System. Data. **Entity. Dbfunctionattribute**  
  > [!NOTE]
  > Nazwa tej klasy została zmieniona; Klasa ze starą nazwą nadal istnieje i działa, ale teraz jest oznaczona jako przestarzała.  
- System. Data. Objects. EntityFunctions = > System. Data. **Entity.** DB— funkcje  
  > [!NOTE]
  > Nazwa tej klasy została zmieniona; Klasa ze starą nazwą nadal istnieje i działa, ale teraz jest oznaczona jako przestarzała.  
- Klasy przestrzenne (na przykład DbGeography, DbGeometry) zostały przeniesione z elementu System. Data. przestrzenny = > System. Data. **Jednostka**. Przestrzennych

> [!NOTE]
> Niektóre typy w przestrzeni nazw System. Data są w pliku System. Data. dll, który nie jest zestawem EF. Te typy nie zostały przeniesione i dlatego ich przestrzenie nazw pozostaną niezmienione.
