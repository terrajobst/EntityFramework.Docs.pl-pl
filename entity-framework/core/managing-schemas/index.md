---
title: Zarządzanie schematami bazy danych — EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416306"
---
# <a name="managing-database-schemas"></a>Zarządzanie schematami bazy danych

EF Core oferuje dwa podstawowe sposoby utrzymywania synchronizacji modelu EF Core i schematu bazy danych. Aby wybrać jeden z tych dwóch, zdecyduj, czy model EF Core lub schemat bazy danych jest źródłem prawdy.

Jeśli chcesz, aby model EF Core był źródłem prawdy, użyj [migracji][1]. W miarę wprowadzania zmian w modelu EF Core ta metoda stopniowo stosuje odpowiednie zmiany schematu do bazy danych, tak aby była zgodna z modelem EF Core.

Jeśli chcesz, aby schemat bazy danych był źródłem prawdy, [Użyj odtwarzania.][2] Takie podejście umożliwia tworzenie szkieletu DbContext i klas typu jednostki przez odtwarzanie schematu bazy danych do modelu EF Core.

> [!NOTE]
> [Interfejsy API tworzenia i upuszczania][3] mogą również tworzyć schemat bazy danych na podstawie modelu EF Core. Jednak są one głównie do testowania, tworzenia prototypów i innych scenariuszy, w których porzucanie bazy danych jest dopuszczalne.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
