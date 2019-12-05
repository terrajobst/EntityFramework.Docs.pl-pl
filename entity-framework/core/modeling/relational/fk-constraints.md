---
title: Ograniczenia klucza obcego — EF Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 2855137adf2ba3c9edaabd15a05f7a209f00f685
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824590"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="9ce3f-102">Ograniczenia klucza obcego</span><span class="sxs-lookup"><span data-stu-id="9ce3f-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="9ce3f-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="9ce3f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="9ce3f-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="9ce3f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="9ce3f-105">Ograniczenie klucza obcego jest wprowadzane dla każdej relacji w modelu.</span><span class="sxs-lookup"><span data-stu-id="9ce3f-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="9ce3f-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="9ce3f-106">Conventions</span></span>

<span data-ttu-id="9ce3f-107">Zgodnie z Konwencją ograniczenia klucza obcego mają nazwę `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="9ce3f-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="9ce3f-108">W przypadku złożonych kluczy obcych `<foreign key property name>` stać się rozdzielaną podkreśleniem listę nazw właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="9ce3f-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="9ce3f-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="9ce3f-109">Data Annotations</span></span>

<span data-ttu-id="9ce3f-110">Nazw ograniczeń klucza obcego nie można skonfigurować przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="9ce3f-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="9ce3f-111">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="9ce3f-111">Fluent API</span></span>

<span data-ttu-id="9ce3f-112">Za pomocą interfejsu API Fluent można skonfigurować nazwę ograniczenia klucza obcego dla relacji.</span><span class="sxs-lookup"><span data-stu-id="9ce3f-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
