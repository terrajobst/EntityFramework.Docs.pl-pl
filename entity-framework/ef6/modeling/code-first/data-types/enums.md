---
title: Pomoc techniczna dla wyliczenia — najpierw - Code EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 986991c2dd9470867aaf5424ecb176d7db172426
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489508"
---
# <a name="enum-support---code-first"></a>Pomoc techniczna dla wyliczenia — kod najpierw
> [!NOTE]
> **EF5 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 5. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.

W tym przewodniku krok po kroku i wideo pokazuje, jak za pomocą typach wyliczeniowych Entity Framework Code First. Ilustruje też sposób używania typów wyliczeniowych w zapytaniu programu LINQ.

W tym przewodniku będzie używać Code First, aby utworzyć nową bazę danych, ale można również użyć [Code First, aby zmapować istniejącą bazę danych](~/ef6/modeling/code-first/workflows/existing-database.md).

Obsługa typu wyliczeniowego została wprowadzona w Entity Framework 5. Aby korzystać z nowych funkcji, takich jak typy wyliczeniowe, typy danych przestrzennych i funkcji z wartościami przechowywanymi w tabeli, należy wskazać .NET Framework 4.5. Program Visual Studio 2012 jest przeznaczony dla platformy .NET 4.5 domyślnie.

W programie Entity Framework, wyliczenie może mieć następujące typy bazowe: **bajtów**, **Int16**, **Int32**, **Int64** , lub **SByte**.

## <a name="watch-the-video"></a>Obejrzyj wideo
To wideo pokazuje, jak za pomocą typach wyliczeniowych Entity Framework Code First. Ilustruje też sposób używania typów wyliczeniowych w zapytaniu programu LINQ.

**Osoba prezentująca**: Julia Kornich

**Film wideo**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Musisz być w wersji programu Visual Studio 2012 Ultimate, Premium, Professional i Web Express zainstalowany w celu przeprowadzenia tego instruktażu.

 

## <a name="set-up-the-project"></a>Konfigurowanie projektu

1.  Otwórz program Visual Studio 2012
2.  Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**
3.  W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu
4.  Wprowadź **EnumCodeFirst** jako nazwę projektu i kliknij przycisk **OK**

## <a name="define-a-new-model-using-code-first"></a>Definiowanie nowego modelu za pomocą funkcji Code First

Korzystając z rozwiązania deweloperskiego Code First zwykle Rozpocznij od pisania klas .NET Framework, które definiują model koncepcyjny (domena). Poniższy kod definiuje klasę działu.

Ten kod definiuje również wyliczenie DepartmentNames. Domyślnie jest wyliczenie **int** typu. Właściwość Nazwa klasy działu jest typu DepartmentNames.

Otwórz plik Program.cs i wklej poniższe definicje klas.

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a>Definiowanie typu pochodnego typu DbContext

Oprócz definiowania jednostek, musisz zdefiniować klasę, która pochodzi od typu DbContext i udostępnia DbSet&lt;TEntity&gt; właściwości. DbSet&lt;TEntity&gt; właściwości umożliwiają kontekstu wiedzieć, jakie typy, które mają zostać uwzględnione w modelu.

Wystąpienie typu DbContext pochodzące zarządza obiektami jednostki w czasie wykonywania, zawierającą wypełnianie obiektów przy użyciu danych z bazy danych, zmień śledzenie i przechowywanie danych w bazie danych.

Typy DbContext i DbSet są definiowane w zestawie platformy EntityFramework. Firma Microsoft doda odwołanie do tej biblioteki DLL przy użyciu pakietu EntityFramework NuGet.

1.  W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu.
2.  Wybierz **Zarządzaj pakietami NuGet...**
3.  W oknie dialogowym pakiety zarządzania NuGet wybierz **Online** kartę i wybierz polecenie **EntityFramework** pakietu.
4.  Kliknij przycisk **instalacji**

Należy pamiętać, że oprócz zestawu platformy EntityFramework, odwołania do zestawów System.ComponentModel.DataAnnotations i System.Data.Entity są dodawane także.

W górnej części pliku Program.cs Dodaj następującą instrukcję using:

``` csharp
using System.Data.Entity;
```

W pliku Program.cs Dodaj definicję kontekstu. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>Utrwalanie i pobieranie danych

Otwórz plik Program.cs, w którym jest zdefiniowana metoda Main. Dodaj następujący kod do funkcji Main. Ten kod dodaje nowy obiekt działu do kontekstu. Następnie zapisuje dane. Kod wykonuje też zapytanie LINQ, które zwraca działu, gdzie nazwa jest DepartmentNames.English.

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Skompilować i uruchomić aplikację. Program generuje następujące wyniki:

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a>Wyświetlanie wygenerowanych bazy danych

Po uruchomieniu aplikacji po raz pierwszy, platformy Entity Framework tworzy bazę danych. Dysponujemy programu Visual Studio 2012, baza danych zostanie utworzony w wystąpieniu programu LocalDB. Domyślnie platforma Entity Framework nazwy bazy danych po w pełni kwalifikowanej nazwie pochodnej kontekstu (w tym przykładzie, który jest **EnumCodeFirst.EnumTestContext**). Kolejne razy, zostanie użyta istniejącej bazy danych.  

Należy pamiętać, że jeśli wprowadzisz zmiany do modelu, po utworzeniu bazy danych, należy użyć migracje Code First do zaktualizowania schematu bazy danych. Zobacz [Code First dla nowej bazy danych](~/ef6/modeling/code-first/workflows/new-database.md) na przykład za pomocą migracji.

Aby wyświetlić i bazy danych, wykonaj następujące czynności:

1.  W menu głównym programu Visual Studio 2012, wybierz **widoku**  - &gt; **Eksplorator obiektów SQL Server**.
2.  Jeśli LocalDB, nie ma na liście serwerów, kliknij prawym przyciskiem myszy **programu SQL Server** i wybierz **dodawania serwera SQL** Użyj domyślnej **uwierzytelniania Windows** połączyć się z Wystąpienia LocalDB
3.  Rozwiń węzeł LocalDB
4.  Unfold **baz danych** folder, aby zobaczyć nową bazę danych, a następnie przejdź do **działu** tabeli należy pamiętać, że Code First nie tworzy tabelę, która mapuje dane na typ wyliczeniowy
5.  Aby wyświetlić dane, kliknij prawym przyciskiem myszy w tabeli i wybrać **widoku danych**

## <a name="summary"></a>Podsumowanie

W tym przewodniku zobaczyliśmy, jak za pomocą typach wyliczeniowych Entity Framework Code First. 
