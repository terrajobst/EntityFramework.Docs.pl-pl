---
title: Ograniczenia klucza obcego — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655996"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="3f7c2-102">Ograniczenia klucza obcego</span><span class="sxs-lookup"><span data-stu-id="3f7c2-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="3f7c2-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="3f7c2-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3f7c2-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="3f7c2-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3f7c2-105">Ograniczenie klucza obcego jest wprowadzane dla każdej relacji w modelu.</span><span class="sxs-lookup"><span data-stu-id="3f7c2-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="3f7c2-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="3f7c2-106">Conventions</span></span>

<span data-ttu-id="3f7c2-107">Zgodnie z Konwencją ograniczenia klucza obcego mają nazwę `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="3f7c2-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="3f7c2-108">W przypadku złożonych kluczy obcych `<foreign key property name>` stać się rozdzielaną podkreśleniem listę nazw właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="3f7c2-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3f7c2-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="3f7c2-109">Data Annotations</span></span>

<span data-ttu-id="3f7c2-110">Nazw ograniczeń klucza obcego nie można skonfigurować przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="3f7c2-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3f7c2-111">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="3f7c2-111">Fluent API</span></span>

<span data-ttu-id="3f7c2-112">Za pomocą interfejsu API Fluent można skonfigurować nazwę ograniczenia klucza obcego dla relacji.</span><span class="sxs-lookup"><span data-stu-id="3f7c2-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
