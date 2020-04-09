---
title: Wprowadzenie do entity framework 6 — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 66ce9113-81d2-480f-8c16-d00ec405b2f7
uid: ef6/get-started
ms.openlocfilehash: 729dea2c474c35f638ccaf6673550f76e88e2667
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136264"
---
# <a name="get-started-with-entity-framework-6"></a>Wprowadzenie do entity framework 6

Ten przewodnik zawiera zbiór łączy do wybranych artykułów z dokumentacji, instruktajnów i filmów, które mogą ułatwić szybkie rozpoczęcie pracy.

## <a name="fundamentals"></a>Podstawy

* [Pobieranie platformy Entity Framework](~/ef6/fundamentals/install.md)

  W tym miejscu dowiesz się, jak dodać entity framework do aplikacji i, jeśli chcesz użyć projektanta EF, upewnij się, że masz go zainstalowanego w programie Visual Studio.

* [Tworzenie modelu: Najpierw kod, projektant EF i przepływy pracy EF](~/ef6/modeling/index.md)

  Czy wolisz określić kod do pisania modelu EF lub pola rysunku i linie?
Czy zamierzasz użyć EF do mapowania obiektów do istniejącej bazy danych lub chcesz EF utworzyć bazę danych dostosowaną do obiektów?
Tutaj dowiesz się o dwóch różnych podejść do korzystania z EF6: EF Designer i Code First.
Upewnij się, że śledzisz dyskusję i obejrzyj film o różnicy.

* [Praca z klasą DbContext](~/ef6/fundamentals/working-with-dbcontext.md)

  DbContext jest pierwszym i najważniejszym typem EF, który należy nauczyć się używać. Służy jako launchpad dla zapytań bazy danych i śledzi zmiany wprowadzone w obiektach, dzięki czemu mogą być utrwalane z powrotem do bazy danych.

* [Zadaj pytanie](~/ef6/resources/get-help.md)

  Dowiedz się, jak uzyskać pomoc od ekspertów i wnieść własne odpowiedzi do społeczności.

* [Przyczynić się](https://github.com/aspnet/EntityFramework6/)

  Entity Framework 6 używa otwartego modelu rozwoju. Dowiedz się, jak możesz ulekwować, odwiedzając nasze repozytorium GitHub.

## <a name="code-first-resources"></a>Zasoby Code First

  - [Najpierw kodować istniejący przepływ pracy bazy danych](~/ef6/modeling/code-first/workflows/existing-database.md)
  - [Najpierw kod do nowego przepływu pracy bazy danych](~/ef6/modeling/code-first/workflows/new-database.md)
  - [Mapowanie wyliczenia przy użyciu kodu](~/ef6/modeling/code-first/data-types/enums.md)
  - [Mapowanie typów przestrzennych przy użyciu kodu](~/ef6/modeling/code-first/data-types/spatial.md)
  - [Pisanie konwencji pierwszego kodu niestandardowego](~/ef6/modeling/code-first/conventions/custom.md)
  - [Korzystanie z pierwszej konfiguracji kod fluent z visual basic](~/ef6/modeling/code-first/fluent/vb.md)
  - [Migracje kodu](~/ef6/modeling/code-first/migrations/index.md)
  - [Migracje kodu po raz pierwszy w środowiskach zespołowych](~/ef6/modeling/code-first/migrations/teams.md)
  - [Automatyczne migracje pierwszego kodu](~/ef6/modeling/code-first/migrations/automatic.md) (nie jest to już zalecane)

## <a name="ef-designer-resources"></a>Zasoby EF Designer
  - [Pierwszy przepływ pracy bazy danych](~/ef6/modeling/designer/workflows/database-first.md)
  - [Pierwszy przepływ pracy modelu](~/ef6/modeling/designer/workflows/model-first.md)
  - [Mapowanie wyliczenia](~/ef6/modeling/designer/data-types/enums.md)
  - [Mapowanie typów przestrzennych](~/ef6/modeling/designer/data-types/spatial.md)
  - [Mapowanie dziedziczenia tabeli na hierarchię](~/ef6/modeling/designer/inheritance/tph.md)
  - [Mapowanie dziedziczenia tabeli na typ](~/ef6/modeling/designer/inheritance/tpt.md)
  - [Mapowanie procedur składowanych dla aktualizacji](~/ef6/modeling/designer/stored-procedures/cud.md)
  - [Mapowanie procedur składowanych dla kwerendy](~/ef6/modeling/designer/stored-procedures/query.md)
  - [Dzielenie jednostki](~/ef6/modeling/designer/entity-splitting.md)
  - [Dzielenie tabeli](~/ef6/modeling/designer/table-splitting.md)
  - [Definiowanie kwerendy](~/ef6/modeling/designer/advanced/defining-query.md) (zaawansowane)
  - [Funkcje wyceniane w tabeli](~/ef6/modeling/designer/advanced/tvfs.md) (zaawansowane)

## <a name="other-resources"></a>Inne zasoby
  - [Asynchronizowanie i zapisywanie](~/ef6/fundamentals/async.md)
  - [Powiązanie danych za pomocą winform](~/ef6/fundamentals/databinding/winforms.md)
  - [Powiązanie danych za pomocą WPF](~/ef6/fundamentals/databinding/wpf.md)
  - [Rozłączone scenariusze z jednostkami samoobjuwaczy (nie](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) jest to już zalecane)
