---
title: Zarządzanie schematami bazy danych — EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416306"
---
# <a name="managing-database-schemas"></a>Zarządzanie schematami bazy danych

EF Core zapewnia dwa podstawowe sposoby przechowywania modelu EF Core i schematu bazy danych w synchronizacji. Aby wybrać między tymi dwoma, zdecydować, czy model EF Core lub schemat bazy danych jest źródłem prawdy.

Jeśli chcesz, aby twój model EF Core był źródłem prawdy, użyj [migrations][1]. W miarę wprowadzania zmian w modelu EF Core, to podejście stopniowo stosuje odpowiednie zmiany schematu do bazy danych, dzięki czemu pozostaje zgodna z modelem EF Core.

Użyj [inżynierii odwrotnej,][2] jeśli chcesz, aby schemat bazy danych był źródłem prawdy. Takie podejście umożliwia szkieletowanie DbContext i klasy typu jednostki przez inżynierii odwrotnej schematu bazy danych do modelu EF Core.

> [!NOTE]
> Interfejsy [API tworzenia i upuszczania][3] można również utworzyć schemat bazy danych z modelu EF Core. Jednak są one przede wszystkim do testowania, prototypowania i innych scenariuszy, w których upuszczanie bazy danych jest dopuszczalne.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
