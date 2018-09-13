---
title: Relacje, właściwości nawigacji i kluczy obcych — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: a98c1bf798a8a6d2c748408d7363d5f884e7e6e9
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490548"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a>Relacje, właściwości nawigacji i kluczy obcych
Ten temat zawiera omówienie sposobu zarządzania relacje między jednostkami w Entity Framework. Daje ona również pewne wskazówki na temat sposobu mapowania i manipulowania nimi relacji.

## <a name="relationships-in-ef"></a>Relacje w programie EF

Relacyjne bazy danych (zwane również skojarzeń) relacje między tabelami są definiowane za pomocą kluczy obcych. Klucz obcy (klucz OBCY) to kolumna lub połączenie kolumn, który służy do ustanawiania i wymuszają połączenie między danymi w dwóch tabelach. Zazwyczaj są trzy typy relacji: jeden do jednego, jeden do wielu i wiele do wielu. W relacji jeden do wielu klucz obcy jest zdefiniowany w tabeli, która reprezentuje liczbę końca relacji. Relacja wiele do wielu obejmuje zdefiniowanie trzeciej tabeli (nazywany połączeń lub łączony albo oczekuje tabeli), którego klucza podstawowego składa się z kluczy obcych z obu tabel powiązanych. W relacji jeden do jednego klucz podstawowy działa również jako klucz obcy, i nie ma żadnych oddzielne kolumny klucza obcego dla każdej tabeli.

Na poniższej ilustracji przedstawiono dwie tabele, które uczestniczą w relacji jeden do wielu. **Kurs** tabela jest tabelą zależne, ponieważ zawiera on **DepartmentID** kolumny, która łączy się **działu** tabeli.

![Dział i kurs tabel](~/ef6/media/database2.png)

Platformy Entity Framework jednostki może być powiązane z innymi obiektami za pośrednictwem stowarzyszenia lub relacji. Każda relacja zawiera dwa końce, które opisują typ jednostki i liczebności typu (jednego, zero lub jeden lub wiele) dwie jednostki w tej relacji. Relacje mogą podlegać ograniczenia referencyjnego, w tym artykule opisano, które zakończenia w relacji jest główną rolę i który jest zależny roli.

Właściwości nawigacji Podaj sposób przechodzenia skojarzenie między dwoma typami encji. Każdy obiekt może mieć właściwości nawigacji dla każdej relacji, w których uczestniczy. Właściwości nawigacji pozwala na przechodzenie relacji i zarządzanie nimi w obu kierunkach, zwracając obiekt odwołania (Jeśli liczebność jest jedną lub zero lub jeden) lub kolekcji (Jeśli liczebność to wiele). Może również wybierzesz jednokierunkowe nawigacji, w którym to przypadku zdefiniować właściwości nawigacji na tylko jeden z typów, które uczestniczy w relacji, a nie w obu.

Zalecane jest, aby uwzględnić właściwości w modelu, które są mapowane na klucze obce w bazie danych. Dołączone właściwości klucza obcego można utworzyć lub zmienić relacji, zmieniając wartość klucza obcego w obiekcie zależne. Tego rodzaju skojarzenia nazywa się to skojarzenie klucza obcego. Przy użyciu kluczy obcych jest nawet bardziej istotne, podczas pracy z odłączone jednostki. Należy pamiętać, że w przypadku pracy z 1-do-1 lub 1-0.. Relacje 1 jest nie oddzielne kolumny klucza obcego, właściwość klucza podstawowego działa jako klucza obcego i zawsze znajduje się w modelu.

Kolumny klucza obcego nie są uwzględnione w modelu informacji o skojarzeniu odbywa się jako niezależny obiekt. Relacje są śledzone za pomocą odwołania do obiektów zamiast właściwości klucza obcego. Skojarzenie tego typu jest nazywana *niezależnych skojarzenie*. Najczęstszym sposobem modyfikowania *niezależnych skojarzenie* jest zmodyfikowanie właściwości nawigacji, które są generowane dla każdego obiektu, który uczestniczy w skojarzeniu.

Można użyć jednego lub obu typów skojarzeń w modelu. Jednak w przypadku czystego relację wiele do wielu, która jest połączona za pomocą tabeli sprzężenia, która zawiera tylko klucze obce EF użyje niezależnych skojarzenia do zarządzania takich relacji wiele do wielu.   

Na poniższej ilustracji przedstawiono model koncepcyjny, który został utworzony za pomocą programu Entity Framework Designer. Model zawiera dwie jednostki, które uczestniczą w relacji jeden do wielu. Zarówno jednostki mają właściwości nawigacji. **Kurs** jednostki depend i ma **DepartmentID** zdefiniowana właściwość klucza obcego.

![Dział i kurs tabel z właściwości nawigacji](~/ef6/media/relationshipefdesigner.png)

Poniższy fragment kodu przedstawia ten sam model, który został utworzony za pomocą Code First.

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class Department
{
   public Department()
   {
     this.Course = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a>Konfigurowanie lub Mapowanie relacji

Pozostałej części tej strony opisano, jak uzyskać dostęp i manipulowanie danymi za pomocą relacji. Aby uzyskać informacje na temat konfigurowania relacji w modelu zobacz następujące strony.

-   Aby skonfigurować relacje w Code First, zobacz [adnotacje danych](~/ef6/modeling/code-first/data-annotations.md) i [Fluent API — relacje](~/ef6/modeling/code-first/fluent/relationships.md).
-   Aby skonfigurować relacji za pomocą programu Entity Framework Designer, zobacz [relacji z projektancie platformy EF](~/ef6/modeling/designer/relationships.md).

## <a name="creating-and-modifying-relationships"></a>Tworzenie i modyfikowanie relacji

W *skojarzenie klucza obcego*, gdy zmienisz tę relację, stan obiektu zależnego za pomocą `EntityState.Unchanged` stan zmieni się na `EntityState.Modified`. W relacji niezależne zmiana relacji nie aktualizuje stan obiektu zależnego.

Poniższe przykłady pokazują, jak korzystać z właściwości klucza obcego i właściwości nawigacji do skojarzenia powiązanych obiektów. Za pomocą powiązań kluczy obcych można użyć jednej z metod można zmienić, tworzyć lub modyfikować relacje. Za pomocą skojarzeń niezależne nie można użyć właściwości klucza obcego.

- Przypisując nową wartość do właściwości klucza obcego, jak w poniższym przykładzie.  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- Poniższy kod usuwa relację, ustawiając klucz obcy **null**. Należy zauważyć, że właściwość klucza obcego, musi być typ zerowalny.  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > W przypadku odwołania w stanie dodany (w tym przykładzie obiekt kurs) referencyjna właściwość nawigacji nie zostaną zsynchronizowane przy użyciu wartości kluczy nowy obiekt do momentu SaveChanges jest wywoływana. Synchronizacja nie występuje, ponieważ kontekst nie zawiera stałe klucze dla dodanych obiektów, dopóki nie zostaną one zapisane. Jeśli konieczne jest posiadanie nowych obiektów w pełni zsynchronizowane tak szybko, jak ustawić relację, użyj jednej z następujących methods.*

- Przypisując nowy obiekt z właściwością nawigacji. Poniższy kod tworzy relację między kurs i `department`. Jeśli obiekty są dołączone do kontekstu, `course` jest także dodawane do `department.Courses` kolekcji i obce odpowiedniej właściwości klucza na `course` obiektu ma ustawioną wartość właściwości klucza działu.  
  ``` csharp
  course.Department = department;
  ```

- Aby usunąć relację, ustaw właściwość nawigacji na `null`. Pracy z platformą Entity Framework, który jest oparty na .NET 4.0 powiązanego zakończenia musi być załadowany, zanim zostanie ustawiona na wartość null. Na przykład:   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  Począwszy od Entity Framework 5.0, który jest oparty na .NET 4.5, można ustawić relację null bez powiązanym zakończeniem ładowania. Można również ustawić bieżącą wartość null, przy użyciu następującej metody.   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- Przez usunięcie lub dodanie obiektu w kolekcji jednostek. Na przykład można dodać obiektu typu `Course` do `department.Courses` kolekcji. Ta operacja tworzy relację między określonego **kurs** i określonego `department`. Jeśli obiekty są przyłączone do kontekstu, odwołanie działu i właściwość klucza obcego **kurs** obiektu zostanie ustawiony na odpowiednie `department`.  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- Za pomocą `ChangeRelationshipState` metodę, aby zmienić stan określonej relacji między dwa obiekty jednostki. Ta metoda jest najczęściej używana podczas pracy z usługą aplikacji N-warstwowych i *niezależnych skojarzenia* (nie można używać z skojarzenie klucza obcego). Ponadto do używania tej metody należy usunąć w dół do `ObjectContext`, jak pokazano w poniższym przykładzie.  
W poniższym przykładzie istnieje relacja wiele do wielu między szkolenia i kursy. Wywoływanie `ChangeRelationshipState` metody i przekazywanie `EntityState.Added` parametr umożliwia `SchoolContext` wiedzieć, że dodano relację między dwoma obiektami:
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  Należy pamiętać, że Jeśli aktualizujesz (nie tylko dodanie) relację, należy usunąć stare relację po dodaniu nowego:

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a>Synchronizowanie zmian między kluczy obcych i właściwości nawigacji

Po zmianie relacji obiektów dołączyć do kontekstu przy użyciu jednej z metod opisanych wyżej Entity Framework musi Synchronizuj klucze obce, odwołania i kolekcji. Entity Framework automatycznie zarządza tej synchronizacji (znany także jako relacja poprawki) dla obiektów POCO z serwerami proxy. Aby uzyskać więcej informacji, zobacz [pracy z serwerami proxy](~/ef6/fundamentals/proxies.md).

Jeśli używasz jednostki POCO bez serwera proxy należy musi upewnij się, że **metody DetectChanges** metoda jest wywoływana, aby zsynchronizować obiekty powiązane w kontekście. Dokonany wybór, następujące interfejsy API automatycznie wyzwoli **metody DetectChanges** wywołania.

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   LINQ do wykonywania zapytań względem `DbSet`

## <a name="loading-related-objects"></a>Trwa ładowanie powiązanych obiektów

Platformy Entity Framework, którego używasz najczęściej ładowanie jednostek that are related to jednostki zwracanego przez skojarzenie zdefiniowane przy użyciu właściwości nawigacji. Aby uzyskać więcej informacji, zobacz [ładowanie powiązanych obiektów](~/ef6/querying/related-data.md).

> [!NOTE]
> W to skojarzenie klucza obcego w przypadku ładowania powiązanym zakończeniem obiektu zależnego powiązany obiekt zostaną załadowane zależnie od wartości klucza obcego zależnych od, która jest aktualnie w pamięci:

``` csharp
    // Get the course where currently DepartmentID = 1.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

Skojarzenie niezależnych powiązane koniec obiektu zależnego zostaje przesłane zapytanie, oparte na wartości klucza obcego, który jest obecnie dostępna w bazie danych. Jednak jeśli zmodyfikowano relację i spróbuje utworzyć relację jako referencyjna właściwość w punktach obiektu zależnego do innego obiektu podmiotu zabezpieczeń, który jest ładowany w kontekście obiektu programu Entity Framework jest zdefiniowana na komputerze klienckim.

## <a name="managing-concurrency"></a>Zarządzanie współbieżnością

W kluczu obcym i skojarzenia niezależnych kontrolach współbieżności są oparte na klucze jednostek i inne właściwości jednostki, które są zdefiniowane w modelu. Tworzenie modelu za pomocą projektancie platformy EF, ustaw `ConcurrencyMode` atrybutu **stałej** do określenia, czy właściwość powinna być sprawdzana współbieżność. Podczas korzystania Code First do definiowania modelu należy używać `ConcurrencyCheck` adnotacja dla właściwości, które mają być sprawdzone współbieżności. Podczas pracy z usługą Code First umożliwia także `TimeStamp` adnotacji, aby określić, czy właściwość powinna być sprawdzana współbieżność. Może mieć tylko jedną właściwość sygnatury czasowej w danej klasy. Mapy kodu najpierw tę właściwość z polem wartości null w bazie danych.

Firma Microsoft zaleca, zawsze używaj skojarzenie klucza obcego podczas pracy z obiektami, które uczestniczą w sprawdzania współbieżności i rozwiązanie.

Aby uzyskać więcej informacji, zobacz [Obsługa konfliktów współbieżności](~/ef6/saving/concurrency.md).

## <a name="working-with-overlapping-keys"></a>Praca z nakładającymi się kluczami

Nakładające się klucze są klucze złożone, gdzie niektóre właściwości klucza są również częścią innego klucza w jednostce. Skojarzenie niezależne, nie może mieć nakładających się klucza. Aby zmienić to skojarzenie klucza obcego obejmującą nakładającymi się kluczami, zaleca się zmodyfikowanie wartości klucza obcego zamiast odwołania do obiektu.
