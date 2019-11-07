---
title: Wprowadzenie do Entity Framework 6 EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 66ce9113-81d2-480f-8c16-d00ec405b2f7
uid: ef6/get-started
ms.openlocfilehash: 74ae347af3c386639631f28ccb2ddbe9f444953a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655846"
---
# <a name="get-started-with-entity-framework-6"></a>Wprowadzenie do Entity Framework 6

Ten przewodnik zawiera kolekcję linków do wybranych artykułów dokumentacji, przewodników i filmów wideo, które mogą pomóc szybko rozpocząć pracę.

## <a name="fundamentals"></a>Podstawy

* [Pobieranie platformy Entity Framework](~/ef6/fundamentals/install.md)

  W tym artykule dowiesz się, jak dodać Entity Framework do aplikacji i, jeśli chcesz używać projektanta EF, upewnij się, że zainstalowano go w programie Visual Studio.

* [Tworzenie modelu: Code First, Projektant EF i przepływy pracy EF](~/ef6/modeling/index.md)

  Czy wolisz określić model EF piszący kod lub pola i linie rysowania?
Czy zamierzasz użyć EF do mapowania obiektów do istniejącej bazy danych, czy chcesz, aby program Dr utworzył bazę danych dostosowaną do Twoich obiektów?
Tutaj dowiesz się więcej na temat dwóch różnych metod korzystania z EF6: Dr Designer i Code First.
Upewnij się, że obserwujemy dyskusję i Obejrzyj film wideo dotyczący różnic.

* [Praca z klasą DbContext](~/ef6/fundamentals/working-with-dbcontext.md)

  DbContext to pierwszy i najważniejszy typ EF, które należy poznać, jak korzystać z programu. Służy jako Program Launchpad dla zapytań bazy danych i śledzi zmiany wprowadzone w obiektach, dzięki czemu można je utrwalić z powrotem do bazy danych.

* [Pytanie](~/ef6/resources/get-help.md)

  Dowiedz się, jak uzyskać pomoc od ekspertów i podzielić się swoimi odpowiedziami na społeczność.

* [Współtworzenie](https://github.com/aspnet/EntityFramework6/)

  Entity Framework 6 używa otwartego modelu programowania. Dowiedz się, jak możesz pomóc ulepszyć program EF, odwiedzając nasz repozytorium GitHub.

## <a name="code-first-resources"></a>Zasoby Code First

  - [Code First istniejący przepływ pracy bazy danych](~/ef6/modeling/code-first/workflows/existing-database.md)
  - [Code First nowego przepływu pracy bazy danych](~/ef6/modeling/code-first/workflows/new-database.md)
  - [Mapowanie tekstów stałych przy użyciu Code First](~/ef6/modeling/code-first/data-types/enums.md)
  - [Mapowanie typów przestrzennych przy użyciu Code First](~/ef6/modeling/code-first/data-types/spatial.md)
  - [Pisanie niestandardowych Konwencji Code First](~/ef6/modeling/code-first/conventions/custom.md)
  - [Korzystanie z konfiguracji Code First Fluent z Visual Basic](~/ef6/modeling/code-first/fluent/vb.md)
  - [Migracje Code First](~/ef6/modeling/code-first/migrations/index.md)
  - [Migracje Code First w środowiskach zespołu](~/ef6/modeling/code-first/migrations/teams.md)
  - [Automatyczne migracje Code First](~/ef6/modeling/code-first/migrations/automatic.md) (nie jest to już zalecane)

## <a name="ef-designer-resources"></a>Zasoby narzędzia Dr Designer
  - [Przepływ pracy Database First](~/ef6/modeling/designer/workflows/database-first.md)
  - [Przepływ pracy Model First](~/ef6/modeling/designer/workflows/model-first.md)
  - [Mapowanie typów wyliczeniowych](~/ef6/modeling/designer/data-types/enums.md)
  - [Mapowanie typów przestrzennych](~/ef6/modeling/designer/data-types/spatial.md)
  - [Mapowanie dziedziczenia w hierarchii na hierarchię](~/ef6/modeling/designer/inheritance/tph.md)
  - [Mapowanie dziedziczenia według typu tabeli](~/ef6/modeling/designer/inheritance/tpt.md)
  - [Mapowanie procedury składowanej dla aktualizacji](~/ef6/modeling/designer/stored-procedures/cud.md)
  - [Mapowanie procedury składowanej dla zapytania](~/ef6/modeling/designer/stored-procedures/query.md)
  - [Dzielenie jednostki](~/ef6/modeling/designer/entity-splitting.md)
  - [Dzielenie tabeli](~/ef6/modeling/designer/table-splitting.md)
  - [Definiowanie zapytania](~/ef6/modeling/designer/advanced/defining-query.md) (Zaawansowane)
  - [Funkcje z wartościami przechowywanymi w tabeli](~/ef6/modeling/designer/advanced/tvfs.md) (Zaawansowane)

## <a name="other-resources"></a>Inne zasoby
  - [Zapytanie asynchroniczne i zapisywanie](~/ef6/fundamentals/async.md)
  - [Wiązanie danych z WinForms](~/ef6/fundamentals/databinding/winforms.md)
  - [Wiązanie danych z WPF](~/ef6/fundamentals/databinding/wpf.md)
  - [Rozłączone scenariusze z jednostkami samośledzenia](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (nie jest to już zalecane)
