---
title: Analizy przypadków dla platformy Entity Framework - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cd5d3ae3-717d-4095-a2ef-0e8fd72b1a2f
ms.openlocfilehash: d7982a3f89ac1e57b48039d828f287adf6dc5068
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490886"
---
# <a name="microsoft-case-studies-for-entity-framework"></a>Analizy przypadków firmy Microsoft dla programu Entity Framework
Analizy przypadków na tej stronie zaznacz kilka projektów rzeczywistych produkcji, które zostały użyte Entity Framework.
> [!NOTE]
> Szczegółowe wersje tych przypadków nie są już dostępne w witrynie internetowej firmy Microsoft. W związku z tym linki zostały usunięte.

## <a name="epicor"></a>Epicor
Epicor jest firmy zajmującej się oprogramowaniem globalnego dużych (z ponad 400 deweloperzy), który zajmuje się opracowywaniem rozwiązań planowania zasobów przedsiębiorstwa (ERP) dla przedsiębiorstw w więcej niż 150 krajach.
Ich produktem, Epicor 9 opiera się na Service-Oriented architektury (SOA) przy użyciu programu .NET Framework.
W obliczu wiele żądań klientów, aby zapewnić obsługę Language Integrated Query (LINQ) i chce również zmniejszyć obciążenie na serwerach SQL zaplecza, zespół postanowił uaktualnienie do programu Visual Studio 2010 oraz programu .NET Framework 4.0.
Za pomocą programu Entity Framework 4.0, mieli osiąganiu tych celów, a także znacznie upraszczają projektowania i utrzymania.
W szczególności pomocy technicznej programu Entity Framework sformatowanego T4 pozwoliły im przejęciu pełnej kontroli nad ich wygenerowanego kodu oraz automatyczne kompilowanie w funkcji zapisywania wydajności, takich jak wstępnie skompilowanym zapytania i buforowanie.

> "Firma Microsoft prowadzone niektóre testy wydajności ostatnio z istniejącym kodem i byliśmy w stanie zredukować żądania do programu SQL Server o 90%.
Jest ono spowodowane ADO.NET Entity Framework 4." — Badania nad produktami Erik Johnson, wiceprezes ds.  

## <a name="veracity-solutions"></a>Wiarygodności rozwiązania
Nabycia planowania wydarzeń system oprogramowania, który będzie utrudniać ich utrzymywanie i rozszerzać w długim okresie, wiarygodności rozwiązania używane programu Visual Studio 2010 do ponownego napisania zaawansowane i łatwe w użyciu Rich Internet Application wbudowane w program Silverlight 4.
Przy użyciu platformy .NET RIA Services mieli szybkie tworzenie warstwy usług, na podstawie programu Entity Framework, która uniknąć duplikowania kodu i mogą uzyskać typowe sprawdzania poprawności i logika uwierzytelniania w warstwach.  

> "Firma Microsoft zostały sprzedane na platformie Entity Framework została wprowadzona, gdy programu Entity Framework 4 okazał się jeszcze lepsze.
Zwiększona narzędzi i łatwiej jest je modyfikować pliki edmx, które definiują modelu koncepcyjnego, model magazynu i mapowanie między tymi modelami... Za pomocą programu Entity Framework, można uzyskać tej warstwy dostępu do danych, pracy w ciągu dnia — i skompiluj go się, jak można przejść.
Entity Framework jest naszym warstwy dostępu do danych de facto; Nie wiem, dlaczego każda osoba w takich sytuacjach przydałaby z niej korzystać." — Jan McBride, starszy Deweloper

## <a name="nec-display-solutions-of-america"></a>Wyświetlanie NEC rozwiązania Ameryki
Firma NEC chciała wejścia na rynek cyfrowego reklamy oparte na miejscu za pomocą rozwiązania do korzyści reklamodawców i właściciele sieci i zwiększyć swoje własne przychodów.
Aby to zrobić, on uruchamiany parę automatyzowanie procesów ręcznych wymagane dla kampanii ad tradycyjnych aplikacji sieci web.
Witryny zostały zbudowane przy użyciu platformy ASP.NET, program Silverlight 3, wywołań AJAX i WCF, wraz z programu Entity Framework w warstwie dostępu do danych na komunikowanie się z programu SQL Server 2008.

> "Z programem SQL Server, firma Microsoft uznało, uzyskujemy przepływności potrzebowaliśmy, aby obsługiwać reklamodawców i sieci z informacji w czasie rzeczywistym i niezawodności, aby zapewnić informacje zawarte w naszej aplikacji o krytycznym znaczeniu będzie zawsze dostępny" — Mike Corcoran Dyrektor ds. IT

## <a name="darwin-dimensions"></a>Wymiary Darwin
Przy użyciu technologii szeroki zakres firmy Microsoft, zespół Darwin określone do utworzenia Evolver — portal online awatara, który klientów można użyć do tworzenia niesamowitych, w rzeczywistości awatary do użytku w gry, animacji i stron sieci społecznościowych.
Za pomocą korzyści wydajności platformy Entity Framework i ściąganie w składnikach, takich jak Windows Workflow Foundation (WF) i Windows Server AppFabric (pamięć podręczna wysoce skalowalnych aplikacji w pamięci) zespół był w stanie dostarczać wspaniałe produktu w 35% niższy czas opracowywania.
Pomimo mających członków zespołu Podziel w wielu krajach zespołu, zgodnie z procesem zwinne Wytwarzanie oprogramowania przy użyciu cotygodniowych aktualizacjach.

 > "Firma Microsoft nie należy utworzyć technologii sake tej technologii. Autostart jest niezwykle istotne, że możemy korzystać z technologii, który oszczędza czas i pieniądze.
 .NET były wyborem dla rozwoju szybki i tani". — Zachary Olsen, architekt  

## <a name="silverware"></a>Artykuły srebrne
Za pomocą więcej niż 15 lat doświadczenia w tworzeniu sprzedaży (POS) rozwiązania grup restauracji małych i średnich firm zespół projektowy w artykuły srebrne określone w celu ulepszenia swoich produktów z większą liczbą funkcji na poziomie przedsiębiorstwa w celu przyciągnięcia większej łańcuchy restauracji.
Korzystając z najnowszej wersji narzędzi programistycznych firmy Microsoft, byli w stanie tworzyć nowe rozwiązanie cztery razy szybciej niż wcześniej.
Najważniejsze nowe funkcje, takie jak LINQ i Entity Framework łatwiejsze przenoszenie z programu Crystal Reports do programu SQL Server 2008 i SQL Server Reporting Services (SSRS) do przechowywania ich danych i raportowaniem.

> "Zarządzanie danymi efektywne ma kluczowe znaczenie dla powodzenia artykuły srebrne — i dlatego podjęliśmy decyzję o przyjęcie, SQL Reporting". -Nicholas Romanidis, Dyrektor ds. IT / inżynierii oprogramowania
