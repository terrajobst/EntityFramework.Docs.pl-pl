---
title: Obsługa wyliczenia-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419109"
---
# <a name="enum-support---code-first"></a>Obsługa wyliczenia — Code First
> [!NOTE]
> **EF5 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 5. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.

Ten film wideo i przewodnik krok po kroku pokazują, jak używać typów wyliczeniowych z Entity Framework Code First. Pokazano również, jak używać wyliczeń w zapytaniu LINQ.

Ten Instruktaż będzie używać Code First do tworzenia nowej bazy danych, ale można również użyć [Code First do mapowania istniejącej bazy danych](~/ef6/modeling/code-first/workflows/existing-database.md).

Obsługa wyliczenia została wprowadzona w Entity Framework 5. Aby korzystać z nowych funkcji, takich jak wyliczenia, typy danych przestrzennych i funkcje zwracające tabelę, należy określić wartość docelową .NET Framework 4,5. Program Visual Studio 2012 Domyślnie kieruje .NET 4,5.

W Entity Framework Wyliczenie może mieć następujące typy podstawowe: **Byte**, **Int16**, **Int32**, **Int64** **lub**bajty.

## <a name="watch-the-video"></a>Obejrzyj film
W tym filmie wideo pokazano, jak używać typów wyliczeniowych z Entity Framework Code First. Pokazano również, jak używać wyliczeń w zapytaniu LINQ.

**Przedstawione przez**: Julia Kornich

**Wideo**: [wmv](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik, musisz mieć zainstalowaną wersję Visual Studio 2012, Ultimate, Premium, Professional lub Web Express Edition.

 

## <a name="set-up-the-project"></a>Konfigurowanie projektu

1.  Otwórz program Visual Studio 2012
2.  W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt** .
3.  W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **konsoli**
4.  Wprowadź **EnumCodeFirst** jako nazwę projektu, a następnie kliknij przycisk **OK** .

## <a name="define-a-new-model-using-code-first"></a>Zdefiniuj nowy model przy użyciu Code First

W przypadku korzystania z Code First projektowania zwykle zaczynasz od pisania klas .NET Framework, które definiują model koncepcyjny (domeny). Poniższy kod definiuje klasę działu.

Kod definiuje również Wyliczenie DepartmentNames. Domyślnie Wyliczenie jest typu **int** . Właściwość Name klasy Department ma typ DepartmentNames.

Otwórz plik Program.cs i wklej następujące definicje klas.

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
 

## <a name="define-the-dbcontext-derived-type"></a>Zdefiniuj typ pochodny DbContext

Oprócz definiowania jednostek należy zdefiniować klasę, która dziedziczy z DbContext i uwidacznia Nieogólnymi&lt;&gt; właściwości. Nieogólnymi&lt;&gt; właściwości umożliwiają kontekstowi znać, które typy mają być uwzględnione w modelu.

Wystąpienie typu pochodnego DbContext zarządza obiektami obiektów w czasie wykonywania, co obejmuje wypełnianie obiektów danymi z bazy danych, śledzenie zmian i utrwalanie danych w bazie danych.

Typy DbContext i Nieogólnymi są zdefiniowane w zestawie EntityFramework. Dodamy odwołanie do tej biblioteki DLL przy użyciu pakietu NuGet EntityFramework.

1.  W Eksplorator rozwiązań kliknij prawym przyciskiem myszy nazwę projektu.
2.  Wybierz pozycję **Zarządzaj pakietami NuGet...**
3.  W oknie dialogowym Zarządzanie pakietami NuGet wybierz kartę **online** i wybierz pakiet **EntityFramework** .
4.  Kliknij przycisk **Instaluj**

Należy pamiętać, że oprócz zestawu EntityFramework odwołania do zestawów System. ComponentModel. DataAnnotations i system. Data. Entity są również dodawane.

W górnej części pliku Program.cs Dodaj następującą instrukcję using:

``` csharp
using System.Data.Entity;
```

W Program.cs Dodaj definicję kontekstu. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>Utrwalanie i pobieranie danych

Otwórz plik Program.cs, w którym jest zdefiniowana Metoda Main. Dodaj następujący kod do funkcji main. Kod dodaje nowy obiekt działu do kontekstu. Następnie zapisuje dane. Kod wykonuje również zapytanie LINQ, które zwraca dział, w którym nazwa jest DepartmentNames. English.

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

Skompiluj i uruchom aplikację. Program tworzy następujące dane wyjściowe:

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a>Wyświetlanie wygenerowanej bazy danych

Po pierwszym uruchomieniu aplikacji Entity Framework tworzy bazę danych. Ze względu na to, że zainstalowano program Visual Studio 2012, baza danych zostanie utworzona w wystąpieniu LocalDB. Domyślnie Entity Framework nazw bazy danych po w pełni kwalifikowana nazwa kontekstu pochodnego (w tym przykładzie to **EnumCodeFirst. EnumTestContext**). Kolejne użycie istniejącej bazy danych będzie możliwe.  

Należy pamiętać, że jeśli wprowadzisz zmiany w modelu po utworzeniu bazy danych, należy użyć Migracje Code First, aby zaktualizować schemat bazy danych. Zapoznaj się z tematem [Code First do nowej bazy danych](~/ef6/modeling/code-first/workflows/new-database.md) , aby zapoznać się z przykładem użycia migracji.

Aby wyświetlić bazę danych i dane, wykonaj następujące czynności:

1.  W menu głównym programu Visual Studio 2012 wybierz pozycję **wyświetl** -&gt; **Eksplorator obiektów SQL Server**.
2.  Jeśli LocalDB nie znajduje się na liście serwerów, kliknij prawym przyciskiem myszy na **SQL Server** i wybierz polecenie **Dodaj SQL Server** Użyj domyślnego **uwierzytelniania systemu Windows** , aby nawiązać połączenie z wystąpieniem LocalDB
3.  Rozwiń węzeł LocalDB
4.  Unfold folder **Databases** , aby wyświetlić nową bazę danych, i przejdź do uwagi do tabeli **działu** , że Code First nie utworzy tabeli, która jest mapowana na typ wyliczenia
5.  Aby wyświetlić dane, kliknij prawym przyciskiem myszy tabelę i wybierz polecenie **Wyświetl dane**

## <a name="summary"></a>Podsumowanie

W tym instruktażu przedstawiono sposób używania typów wyliczeniowych z Entity Framework Code First. 
