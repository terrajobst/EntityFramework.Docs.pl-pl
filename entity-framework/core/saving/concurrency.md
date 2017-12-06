---
title: "Obsługa współbieżności - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bce0539d-b0cd-457d-be71-f7ca16f3baea
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: bbd3e154c1b27b16c7d8f8fbf9ed51df0849795c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="handling-concurrency"></a>Obsługa współbieżności

Jeśli właściwość jest skonfigurowana jako tokenu współbieżności następnie EF sprawdzi żaden inny użytkownik zmodyfikował tę wartość w bazie danych podczas zapisywania zmian w tym rekordzie.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) w witrynie GitHub.

## <a name="how-concurrency-handling-works-in-ef-core"></a>Jak działa Obsługa współbieżności w EF Core

Aby uzyskać szczegółowy opis działania Obsługa współbieżności w Entity Framework Core, zobacz [tokeny współbieżności](../modeling/concurrency.md).

## <a name="resolving-concurrency-conflicts"></a>Rozwiązywanie konfliktów współbieżności

Rozwiązywanie konfliktów współbieżności obejmuje przy użyciu algorytmu, aby scalić zmiany oczekujące z bieżącego użytkownika z zmiany wprowadzone w bazie danych. Dokładne podejście różni się zależnie od aplikacji, ale typowym podejściem jest wyświetlany wartości dla użytkownika i je określić poprawnych wartości, które mają być przechowywane w bazie danych.

**Istnieją trzy zestawy wartości może pomóc rozwiązać konflikt współbieżności.**

* **Bieżące wartości** wartości próby zapisu w bazie danych aplikacji.

* **Oryginalne wartości** to wartości, które zostały pierwotnie pobrany z bazy danych, aby zmiany zostały wprowadzone.

* **Baza danych wartości** wartości przechowywane w bazie danych.

Aby obsługiwać konflikt współbieżności, catch `DbUpdateConcurrencyException` podczas `SaveChanges()`, użyj `DbUpdateConcurrencyException.Entries` przygotować nowy zestaw zmian do odpowiednich jednostek, a następnie spróbuj ponownie `SaveChanges()` operacji.

W poniższym przykładzie `Person.FirstName` i `Person.LastName` są skonfigurowane jako token współbieżności. Brak `// TODO:` — komentarz w lokalizacji, w którym można dołączyć określonego logikę aplikacji, aby wybrać wartość do zapisania w bazie danych.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Concurrency/Sample.cs?highlight=53,54)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.ComponentModel.DataAnnotations;
using System.Linq;

namespace EFSaving.Concurrency
{
    public class Sample
    {
        public static void Run()
        {
            // Ensure database is created and has a person in it
            using (var context = new PersonContext())
            {
                context.Database.EnsureDeleted();
                context.Database.EnsureCreated();

                context.People.Add(new Person { FirstName = "John", LastName = "Doe" });
                context.SaveChanges();
            }

            using (var context = new PersonContext())
            {
                // Fetch a person from database and change phone number
                var person = context.People.Single(p => p.PersonId == 1);
                person.PhoneNumber = "555-555-5555";

                // Change the persons name in the database (will cause a concurrency conflict)
                context.Database.ExecuteSqlCommand("UPDATE dbo.People SET FirstName = 'Jane' WHERE PersonId = 1");

                try
                {
                    // Attempt to save changes to the database
                    context.SaveChanges();
                }
                catch (DbUpdateConcurrencyException ex)
                {
                    foreach (var entry in ex.Entries)
                    {
                        if (entry.Entity is Person)
                        {
                            // Using a NoTracking query means we get the entity but it is not tracked by the context
                            // and will not be merged with existing entities in the context.
                            var databaseEntity = context.People.AsNoTracking().Single(p => p.PersonId == ((Person)entry.Entity).PersonId);
                            var databaseEntry = context.Entry(databaseEntity);

                            foreach (var property in entry.Metadata.GetProperties())
                            {
                                var proposedValue = entry.Property(property.Name).CurrentValue;
                                var originalValue = entry.Property(property.Name).OriginalValue;
                                var databaseValue = databaseEntry.Property(property.Name).CurrentValue;

                                // TODO: Logic to decide which value should be written to database
                                // entry.Property(property.Name).CurrentValue = <value to be saved>;

                                // Update original values to
                                entry.Property(property.Name).OriginalValue = databaseEntry.Property(property.Name).CurrentValue;
                            }
                        }
                        else
                        {
                            throw new NotSupportedException("Don't know how to handle concurrency conflicts for " + entry.Metadata.Name);
                        }
                    }

                    // Retry the save operation
                    context.SaveChanges();
                }
            }
        }

        public class PersonContext : DbContext
        {
            public DbSet<Person> People { get; set; }

            protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            {
                optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFSaving.Concurrency;Trusted_Connection=True;");
            }
        }

        public class Person
        {
            public int PersonId { get; set; }

            [ConcurrencyCheck]
            public string FirstName { get; set; }

            [ConcurrencyCheck]
            public string LastName { get; set; }

            public string PhoneNumber { get; set; }
        }

    }
}
```
