---
title: Analizy przypadków dla Entity Framework-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cd5d3ae3-717d-4095-a2ef-0e8fd72b1a2f
ms.openlocfilehash: d7982a3f89ac1e57b48039d828f287adf6dc5068
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417082"
---
# <a name="microsoft-case-studies-for-entity-framework"></a>Analizy przypadków firmy Microsoft dla Entity Framework
Analizy przypadków na tej stronie podkreślają kilka rzeczywistych projektów produkcyjnych, które zostały zatrudnione Entity Framework.
> [!NOTE]
> Szczegółowe wersje tych analiz przypadków nie są już dostępne w witrynie sieci Web firmy Microsoft. W związku z tym linki zostały usunięte.

## <a name="epicor"></a>Epicor
Epicor to duża globalna firma firmy (z ponad 400 deweloperów), która opracowuje rozwiązania do planowania zasobów przedsiębiorstwa (ERP) dla firm w więcej niż 150 krajach.
Ich produkt sztandarowe, Epicor 9, jest oparty na architekturze zorientowanej na usługi (SOA) przy użyciu .NET Framework.
Na przykład w przypadku wielu żądań klientów w celu zapewnienia pomocy technicznej dotyczącej języka Integrated Query (LINQ), a także chcesz zmniejszyć obciążenie serwerów programu SQL zaplecza, zespół zdecydował się na uaktualnienie do programu Visual Studio 2010 i .NET Framework 4,0.
Korzystając z Entity Framework 4,0, były w stanie osiągnąć te cele, a także znacznie uprościć programowanie i konserwację.
W szczególności zaawansowana obsługa T4 Entity Framework umożliwiała im pełną kontrolę nad wygenerowanym kodem i automatyczne Kompilowanie w funkcje zapisywania wydajności, takie jak wstępnie skompilowane zapytania i buforowanie.

> "Niedawno przeprowadzono pewne testy wydajnościowe z istniejącym kodem i można zmniejszyć liczbę żądań do SQL Server przez 90 procent.
Wynika to z ADO.NET Entity Framework 4 ". — Erik Johnsonem, wiceprezes ds. produktów  

## <a name="veracity-solutions"></a>Rozwiązania prawdziwie
Po pozyskaniu systemu oprogramowania do planowania zdarzeń, który miał być trudny do utrzymania i przedłużony przez długoterminowe rozwiązania o wysokiej dostępności używane w programie Visual Studio 2010 do jego ponownego zapisania jako zaawansowanej i łatwej w użyciu bogatej aplikacji internetowej opartej na technologii Silverlight 4.
Korzystając z usług .NET RIA, mogli szybko utworzyć warstwę usług na podstawie Entity Framework, które nie pozwalają na duplikowanie kodu i są dozwolone dla wspólnej logiki walidacji i uwierzytelniania w warstwach.  

> "Zostały sprzedane na Entity Framework podczas jego pierwszego wprowadzenia, a Entity Framework 4 okazało się jeszcze lepsze.
Ulepszone narzędzia i ułatwiają manipulowanie plikami. edmx, które definiują model koncepcyjny, model magazynu i mapowanie między tymi modelami... Za pomocą Entity Framework mogę uzyskać dostęp do tej warstwy dostępu do danych w ciągu dnia — i skompilować ją jak I.
Entity Framework to nasza Warstwa dostępu do danych. Nie wiem, dlaczego nikt nie używa. " – Jan McBride, Starszy programista

## <a name="nec-display-solutions-of-america"></a>Rozwiązania NEC Display of America
NEC chciała wprowadzić rynek na potrzeby reklamy na miejscu cyfrowym, dzięki któremu można korzystać z rozwiązań i właścicieli sieci oraz zwiększać własne przychody.
W tym celu uruchomiono parę aplikacji sieci Web, które automatyzują ręczne procesy wymagane w tradycyjnej kampanii usługi AD.
Lokacje zostały skompilowane przy użyciu ASP.NET, Silverlight 3, AJAX i WCF oraz Entity Framework w warstwie dostępu do danych, aby komunikować się z SQL Server 2008.

> "Dzięki SQL Server mogliśmy uzyskać przepływność, której potrzebujemy, aby zapewnić osobom i sieciom informacje w czasie rzeczywistym oraz niezawodność, która zapewnia, że informacje w naszych aplikacjach o znaczeniu krytycznym będą zawsze dostępne"-Jan Corcoran, Dyrektor ds. IT

## <a name="darwin-dimensions"></a>Wymiary Darwin
Korzystając z szerokiego zakresu technologii firmy Microsoft, zespół w usłudze Darwin jest skonfigurowany do tworzenia aplikacji, która umożliwia użytkownikom tworzenie atrakcyjnych, lifelikeych awatarów do użycia w grach, animacjach i stronach sieci społecznościowych.
Dzięki korzyściom z produktywności Entity Framework i ściąganiu składników, takich jak Windows Workflow Foundation (WF) i Windows Server AppFabric (pamięć podręczna aplikacji w pamięci podręcznej), zespół mógł dostarczyć niezwykły produkt w 35% mniej czas projektowania.
Pomimo tego, że członkowie zespołu dzielą się w wielu krajach, zespół postępuje zgodnie z procesem deweloperskim Agile z wydaniami tygodniowymi.

 > "Próbujemy utworzyć technologię technologii dla nas. Jako że jest to bardzo ważne, aby wykorzystać technologię, która oszczędza czas i pieniądze.
 Platforma .NET była wyborem szybkiego i ekonomicznego rozwoju. — Zachary Olsen, architekt  

## <a name="silverware"></a>Silver
Mając więcej niż 15 lat doświadczenia w opracowywaniu rozwiązań typu punkt sprzedaży (POS) dla małych i średnich grup restauracji, zespół programistyczny w ramach oprogramowania Silver jest skonfigurowany w celu ulepszania swoich produktów dzięki większej liczbie funkcji na poziomie przedsiębiorstwa, aby przyciągnąć większy łańcuchy restauracji.
Korzystając z najnowszej wersji narzędzi programistycznych firmy Microsoft, udało Ci się kompilować nowe rozwiązanie cztery razy szybciej niż wcześniej.
Najważniejsze nowe funkcje, takie jak LINQ i Entity Framework, ułatwiają przejście z programu Crystal Reports do SQL Server 2008 i SQL Server Reporting Services (SSRS) w celu przechowywania i raportowania danych.

> "Efektywne zarządzanie danymi jest kluczem do sukcesu programu Silver — i dlatego zdecydowano o przyjęciu SQL Reporting". — Nicholas Romanidis, dyrektor ds. inżynierów IT/oprogramowania
