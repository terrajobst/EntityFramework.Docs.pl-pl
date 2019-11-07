---
title: Wartości domyślne — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655904"
---
# <a name="default-values"></a><span data-ttu-id="308c5-102">Wartości domyślne</span><span class="sxs-lookup"><span data-stu-id="308c5-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="308c5-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="308c5-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="308c5-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="308c5-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="308c5-105">Wartość domyślna kolumny to wartość, która zostanie wstawiona, jeśli zostanie wstawiony nowy wiersz, ale nie określono wartości dla tej kolumny.</span><span class="sxs-lookup"><span data-stu-id="308c5-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="308c5-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="308c5-106">Conventions</span></span>

<span data-ttu-id="308c5-107">Zgodnie z Konwencją wartość domyślna nie jest skonfigurowana.</span><span class="sxs-lookup"><span data-stu-id="308c5-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="308c5-108">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="308c5-108">Data Annotations</span></span>

<span data-ttu-id="308c5-109">Nie można ustawić wartości domyślnej przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="308c5-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="308c5-110">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="308c5-110">Fluent API</span></span>

<span data-ttu-id="308c5-111">Możesz użyć interfejsu API Fluent, aby określić wartość domyślną dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="308c5-111">You can use the Fluent API to specify the default value for a property.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

<span data-ttu-id="308c5-112">Można również określić fragment SQL, który jest używany do obliczania wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="308c5-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
