---
title: Automatyczne wykrywanie zmian — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: 9af85fd7ca48a14432a1f33c59079fc438ef8810
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490992"
---
# <a name="automatic-detect-changes"></a>Automatyczne wykrywanie zmian
Korzystając z większości obiektów POCO ustalenia, jak jednostki został zmieniony (i w związku z tym aktualizacje, które muszą być wysyłane do bazy danych) odbywa się przez algorytm wykrywa zmiany. Wykryj zmiany działa został określony poprzez wykrycie różnic między bieżącej wartości właściwości jednostki i oryginalne wartości właściwości, które są przechowywane w migawce jednostki zapytania lub został dołączony. Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.  

Domyślnie program Entity Framework wykonuje wykrywać zmiany automatycznie wywołanego następujących metod:  

- DbSet.Find  
- DbSet.Local  
- DbSet.Add  
- DbSet.AddRange
- DbSet.Remove  
- DbSet.RemoveRange
- DbSet.Attach  
- DbContext.SaveChanges  
- DbContext.GetValidationErrors  
- DbContext.Entry  
- DbChangeTracker.Entries  

## <a name="disabling-automatic-detection-of-changes"></a>Wyłączenie automatycznego wykrywania zmian  

Jeśli śledzisz partii jednostek w kontekst można wywołać jedną z następujących metod wiele razy w pętli, może zostać znaczne ulepszenia wydajności przez wyłączenie wykrywania zmian na czas trwania pętli. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

Nie należy zapominać ponownie włączyć funkcję wykrywania zmian po pętli — używaliśmy try/finally aby upewnić się, nawet wtedy, gdy kod w pętli zgłasza wyjątek zawsze jest ponownie włączone.  

Alternatywa wyłączenie i ponowne włączenie pozostaw automatyczne wykrywanie zmian wyłączony na cały czas i albo Kontekst wywołania. ChangeTracker.DetectChanges jawnie lub użyj zmienić przerwami proxy śledzenia. Obie te opcje są zaawansowane i łatwo wprowadzić drobne błędy do aplikacji więc używane ostrożnie.  

Jeśli potrzebujesz dodać lub usunąć wiele obiektów z kontekstu, należy wziąć pod uwagę przy użyciu DbSet.AddRange i DbSet.RemoveRange. Ta metoda automatycznie Wykryj zmiany tylko jeden raz po ukończeniu operacji dodawania lub usuwania. 
