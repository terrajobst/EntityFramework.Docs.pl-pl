---
title: Klucze alternatywne (ograniczenia Unique) — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054203"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="db971-102">Klucze alternatywne (ograniczenia Unique)</span><span class="sxs-lookup"><span data-stu-id="db971-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="db971-103">Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie.</span><span class="sxs-lookup"><span data-stu-id="db971-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="db971-104">Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="db971-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="db971-105">Wprowadzono unikatowego ograniczenia dla każdego alternatywnego klucza w modelu.</span><span class="sxs-lookup"><span data-stu-id="db971-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="db971-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="db971-106">Conventions</span></span>

<span data-ttu-id="db971-107">Według Konwencji, będzie miała nazwę indeksu i ograniczenia, które zostały wprowadzone dla klucza alternatywnego `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="db971-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="db971-108">Złożonych kluczy alternatywnych `<property name>` staje się podkreślenia oddzielone listę nazw właściwości.</span><span class="sxs-lookup"><span data-stu-id="db971-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="db971-109">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="db971-109">Data Annotations</span></span>

<span data-ttu-id="db971-110">Unikalne ograniczenia nie można skonfigurować za pomocą adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="db971-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="db971-111">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="db971-111">Fluent API</span></span>

<span data-ttu-id="db971-112">Aby skonfigurować nazwę indeksu i ograniczenie klucza alternatywnego, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="db971-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
