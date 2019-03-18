---
title: Entity Framework słownik - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
ms.openlocfilehash: 4e42e5870879524b814cecdc361e688d36f0180f
ms.sourcegitcommit: 6c4e06bc62d98442530e93a44725e38e59483d42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58131385"
---
# <a name="entity-framework-glossary"></a>Entity Framework słownik
## <a name="code-first"></a>Najpierw kod
Tworzenie modelu Entity Framework przy użyciu kodu. Model można wskazać istniejącej lub nowej bazy danych.

## <a name="context"></a>Kontekst
Klasa, która reprezentuje sesję z bazą danych, dzięki czemu możesz do zapytania i Zapisz dane. Kontekst jest pochodną klasy DbContext lub obiektu ObjectContext.

## <a name="convention-code-first"></a>Konwencja (kod: pierwszej)
Reguła, która korzysta z programu Entity Framework wywnioskowania kształt modelu z klas.

## <a name="database-first"></a>Najpierw bazy danych
Tworzenie modelu Entity Framework, za pomocą projektanta EF, który wspiera istniejącej bazy danych.

## <a name="eager-loading"></a>Wczesne ładowanie
Wzorzec załadunku, powiązanych danych, gdzie zapytanie dla jednego typu obiektu również ładuje powiązanych jednostek jako część zapytania.

## <a name="ef-designer"></a>Projektancie platformy EF
Projektant wizualny w programie Visual Studio umożliwia utworzenie modelu Entity Framework, używając okna i wierszy.

## <a name="entity"></a>Jednostka
Klasa lub obiekt, który reprezentuje dane aplikacji, takich jak klientów, produkty i zamówienia.

## <a name="entity-data-model"></a>Entity Data Model
Model, który opisuje jednostek i relacji między nimi. EF używa EDM do opisu modelu koncepcyjnego, względem którego programy dla deweloperów. EDM opiera się na modelu Relacja jednostki, wynikające z odzyskiwania po awarii. Peter Chen. Podstawowym celem staje się wspólnego modelu danych na zestaw technologii firmy Microsoft dla deweloperów i serwer został pierwotnie opracowana EDM. EDM jest również używane jako część protokołu OData.

## <a name="explicit-loading"></a>jawne ładowanie
Wzorzec załadunku, powiązanych danych, gdzie obiekty powiązane są ładowane przez wywołanie interfejsu API.

## <a name="fluent-api"></a>Interfejs Fluent API
Interfejs API, który może służyć do konfigurowania model Code First.

## <a name="foreign-key-association"></a>Skojarzenie klucza obcego
Skojarzenia między jednostkami, której właściwość, która reprezentuje klucz obcy znajduje się w klasie jednostki zależne. Na przykład produkt zawiera właściwość CategoryId.

## <a name="identifying-relationship"></a>Identyfikowanie relacji
Relacja, w którym klucz podstawowy jednostki głównej jest częścią klucza podstawowego jednostki zależne. W tego rodzaju relacji jednostki zależne nie może istnieć bez jednostki głównej.

## <a name="independent-association"></a>Niezależnie od skojarzenia
Skojarzenia między jednostkami w przypadku, gdy nie ma właściwości reprezentujący klucz obcy w klasie jednostki zależne. Na przykład klasa produktu zawiera relację z kategorii, ale nie ma właściwości CategoryId. Entity Framework śledzi stan skojarzenia, niezależnie od stanu jednostek końców dwóch skojarzenia.

## <a name="lazy-loading"></a>Ładowanie z opóźnieniem
Wzorzec załadunku, powiązanych danych, gdzie obiekty powiązane są ładowane automatycznie podczas uzyskiwania dostępu do właściwości nawigacji.

## <a name="model-first"></a>Najpierw modelu
Tworzenie modelu Entity Framework, za pomocą projektanta EF następnie używany do tworzenia nowej bazy danych.

## <a name="navigation-property"></a>Właściwość nawigacji
Właściwość obiektu, który odwołuje się do innej jednostki. Na przykład produkt zawiera właściwość nawigacji kategorii i kategoria zawiera właściwość nawigacji produktów.

## <a name="poco"></a>POCO
Akronim obiektu CLR zwykły stary. Klasa prostych użytkownika, która nie ma zależności za pomocą dowolnej platformy. W kontekście EF, klasa jednostki, która nie pochodzi od EntityObject, implementuje żadnych interfejsów lub niesie ze sobą wszelkie atrybuty zdefiniowane w programie EF. Takie klasy jednostek, które są całkowicie niezależni od framework trwałości są również określane jako "trwałość zakresu".  

## <a name="relationship-inverse"></a>Odwrotne relacji
Przeciwległym końcu relacji, na przykład produktu. Kategoria i kategorii. Produkt.

## <a name="self-tracking-entity"></a>Samodzielnie śledzenia jednostki
Jednostka utworzona na podstawie szablonu generowania kodu, który pomaga w rozwoju N-warstwowej.

## <a name="table-per-concrete-type-tpc"></a>Typ tabeli na konkretny (TPC)
Metoda mapowania dziedziczenia, gdzie każdy typ nieabstrakcyjnej w hierarchii jest mapowany do oddzielnych tabel w bazie danych.

## <a name="table-per-hierarchy-tph"></a>Tabela wg hierarchii (TPH)
Metoda mapowania dziedziczenia, w którym wszystkie typy w hierarchii są mapowane na tej samej tabeli w bazie danych. Dyskryminatora kolumny jest używana do identyfikowania, jaki typ każdy wiersz jest skojarzony.

## <a name="table-per-type-tpt"></a>Tabela wg typu (TPT)
Metoda mapowania dziedziczenia, gdzie wspólne właściwości wszystkich typów w hierarchii są mapowane na tej samej tabeli w bazie danych, ale unikatowe dla każdego typu właściwości są mapowane na osobnej tabeli.

## <a name="type-discovery"></a>Typ odnajdywania
Proces identyfikacji typów, które powinny być częścią modelu Entity Framework.
