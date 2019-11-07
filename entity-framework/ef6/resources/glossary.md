---
title: Słownik Entity Framework — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: df0da4a68b3d2c882d9673417ee5fe335eccae2b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656159"
---
# <a name="entity-framework-glossary"></a>Entity Framework słownik
## <a name="code-first"></a>Code First
Tworzenie modelu Entity Framework przy użyciu kodu. Model może kierować do istniejącej bazy danych lub nowej bazy danych.

## <a name="context"></a>Context
Klasa, która reprezentuje sesję z bazą danych, umożliwiając wykonywanie zapytań i zapisywanie danych. Kontekst pochodzi z klasy DbContext lub ObjectContext.

## <a name="convention-code-first"></a>Konwencja (Code First)
Reguła, która Entity Framework używa do wnioskowania kształtu modelu z klas.

## <a name="database-first"></a>Database First
Tworzenie modelu Entity Framework przy użyciu projektanta EF, który jest przeznaczony dla istniejącej bazy danych.

## <a name="eager-loading"></a>Ładowanie eager
Wzorzec ładowania powiązanych danych, gdzie zapytanie dla jednego typu jednostki również ładuje powiązane jednostki jako część zapytania.

## <a name="ef-designer"></a>Projektant EF
Projektant wizualny w programie Visual Studio, który umożliwia tworzenie modelu Entity Framework przy użyciu pól i wierszy.

## <a name="entity"></a>Jednostka
Klasa lub obiekt reprezentujący dane aplikacji, takie jak klienci, produkty i zamówienia.

## <a name="entity-data-model"></a>Entity Data Model
Model, który opisuje jednostki i relacje między nimi. Dr używa modelu EDM do opisywania model koncepcyjny, dla którego programy deweloperskie. Program EDM kompiluje w modelu relacji jednostki wprowadzonym przez Dr. Peterowi Chen. Model EDM został pierwotnie opracowany z myślą o podstawowym celu przetworzenia wspólnego modelu danych w ramach zestawu technologii deweloperskich i serwerowych firmy Microsoft. EDM jest również używany jako część protokołu OData.

## <a name="explicit-loading"></a>Jawne ładowanie
Wzorzec ładowania powiązanych danych w przypadku ładowania powiązanych obiektów przez wywołanie interfejsu API.

## <a name="fluent-api"></a>Interfejs API Fluent
Interfejs API, który może służyć do konfigurowania modelu Code First.

## <a name="foreign-key-association"></a>Skojarzenie klucza obcego
Skojarzenie między jednostkami, w których właściwość reprezentująca klucz obcy jest uwzględniona w klasie jednostki zależnej. Na przykład produkt zawiera właściwość IDKategorii.

## <a name="identifying-relationship"></a>Identyfikowanie relacji
Relacja, w której klucz podstawowy jednostki głównej jest częścią klucza podstawowego jednostki zależnej. W tym rodzaju relacji jednostka zależna nie może istnieć bez jednostki podmiotu zabezpieczeń.

## <a name="independent-association"></a>Niezależne skojarzenie
Skojarzenie między jednostkami, w których nie ma właściwości reprezentującej klucz obcy w klasie jednostki zależnej. Na przykład Klasa produktu zawiera relację do kategorii, ale nie ma właściwości IDKategorii. Entity Framework śledzi stan skojarzenia niezależnie od stanu jednostek na dwa punkty końcowe skojarzenia.

## <a name="lazy-loading"></a>Ładowanie z opóźnieniem
Wzorzec ładowania powiązanych danych, w przypadku których obiekty powiązane są automatycznie ładowane podczas uzyskiwania dostępu do właściwości nawigacji.

## <a name="model-first"></a>Model First
Tworzenie modelu Entity Framework przy użyciu programu EF Designer, który jest następnie używany do tworzenia nowej bazy danych.

## <a name="navigation-property"></a>Właściwość nawigacji
Właściwość jednostki, która odwołuje się do innej jednostki. Na przykład produkt zawiera właściwość nawigacji kategorii, a kategoria zawiera produkty właściwość nawigacji.

## <a name="poco"></a>POCO
Akronim dla zwykłego starego obiektu CLR. Prosta Klasa użytkownika, która nie ma żadnych zależności z żadną strukturą. W kontekście EF Klasa jednostki, która nie pochodzi z obiektu EntityObject, implementuje interfejsy lub ma atrybuty zdefiniowane w EF. Takie klasy jednostek, które są oddzielone od struktury trwałości, są również określane jako "trwałość ignorujących".  

## <a name="relationship-inverse"></a>Odwracanie relacji
Odwrotny koniec relacji, na przykład produkt. Kategoria i Kategoria. Iloczyn.

## <a name="self-tracking-entity"></a>Jednostka samośledzenia
Jednostka utworzona na podstawie szablonu generowania kodu, która ułatwia tworzenie aplikacji N-warstwowych.

## <a name="table-per-concrete-type-tpc"></a>Typ z tabeli na konkretny (TPC)
Metoda mapowania dziedziczenia, gdzie każdy nieabstrakcyjny typ w hierarchii jest mapowany do oddzielnej tabeli w bazie danych.

## <a name="table-per-hierarchy-tph"></a>Tabela na hierarchię (TPH)
Metoda mapowania dziedziczenia, gdzie wszystkie typy w hierarchii są mapowane na tę samą tabelę w bazie danych. Kolumna rozróżniacza służy do identyfikowania typu, z którym jest skojarzony każdy wiersz.

## <a name="table-per-type-tpt"></a>Tabela na typ (TPT)
Metoda mapowania dziedziczenia, gdzie wspólne właściwości wszystkich typów w hierarchii są mapowane na tę samą tabelę w bazie danych, ale właściwości unikatowe dla każdego typu są mapowane na osobną tabelę.

## <a name="type-discovery"></a>Odnajdowanie typów
Proces identyfikowania typów, które powinny być częścią modelu Entity Framework.
