---
title: Specyfikacja CSDL - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 88669cf80f9a792fda7d191d9f6be2b1734691df
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994732"
---
# <a name="csdl-specification"></a>Specyfikacja CSDL
Język definicji schematu koncepcyjnego (CSDL) to oparty na standardzie XML język, który opisuje jednostki, relacje i funkcje, które tworzą model koncepcyjny aplikacji opartych na danych. Ten model koncepcyjny może służyć przez Entity Framework lub usługi danych WCF. Metadane opisujące z CSDL jest używany przez program Entity Framework do mapowania jednostek i relacji, które są zdefiniowane w modelu koncepcyjnym ze źródłem danych. Aby uzyskać więcej informacji, zobacz [Specyfikacja SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) i [Specyfikacja MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).

CSDL to implementacja programu Entity Framework modelu Entity Data Model.

W aplikacji Entity Framework metadanych modelu koncepcyjnego jest ładowany z pliku .csdl (napisanego w CSDL) do wystąpienia System.Data.Metadata.Edm.EdmItemCollection i są dostępne za pomocą metody Klasa System.Data.Metadata.Edm.MetadataWorkspace. Entity Framework używa modelu koncepcyjnego metadanych do translacji zapytania względem modelu koncepcyjnego polecenia specyficznymi dla źródła danych.

W Projektancie platformy EF przechowuje informacje o modelu koncepcyjnego w pliku edmx w czasie projektowania. W czasie kompilacji projektancie platformy EF używa informacji w pliku edmx można utworzyć pliku .csdl, które są wymagane przez Entity Framework w czasie wykonywania.

Wersje CSDL są zróżnicowane według przestrzeni nazw XML.

| Wersja CSDL | Namespace XML                                |
|:-------------|:---------------------------------------------|
| CSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm |
| CSDL v2      | http://schemas.microsoft.com/ado/2008/09/edm |
| CSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Elementu Association (CSDL)

**Skojarzenia** element definiuje relację między dwoma typami encji. Skojarzenia należy określić typy jednostek, które są zaangażowane w relacji i możliwa liczba typów jednostek na każdym końcu relacji, który jest znany jako liczebności. Liczebność elementu end skojarzenia mogą mieć wartość równą jeden (1), zero lub jeden (od 0 do 1) lub wielu (\*). Informacja ta jest określona w dwa elementy End podrzędnych.

Wystąpienia typu jednostki na jednym końcu asocjacji jest możliwy za pośrednictwem właściwości nawigacji lub kluczy obcych, jeśli są one widoczne na typ jednostki.

W aplikacji wystąpienia skojarzenia reprezentuje określone skojarzenia między wystąpieniami typów jednostek. Skojarzenie wystąpienia są logicznie pogrupowane w zestawie skojarzenia.

**Skojarzenia** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Końcowy (elementy dokładnie 2)
-   ReferentialConstraint (zero lub jeden element)
-   Elementów adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **skojarzenia** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                        |
|:---------------|:------------|:-----------------------------|
| **Nazwa**       | Tak         | Nazwa skojarzenia. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **skojarzenia** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **skojarzenia** element, który definiuje **CustomerOrders** skojarzenie po klucze obce nie być narażone na **klienta** i  **Kolejność** typów jednostek. **Liczebność** wartości dla każdej **zakończenia** skojarzenia wskazują tę liczbę **zamówienia** może być skojarzony z **klienta**, ale tylko jeden **klienta** może być skojarzony z **kolejności**. Ponadto **OnDelete** elementu wskazuje, że wszystkie **zamówienia** powiązanych z określonego **klienta** i zostały załadowane do obiektu ObjectContext zostaną usunięte. Jeśli **klienta** zostanie usunięty.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

W poniższym przykładzie przedstawiono **skojarzenia** element, który definiuje **CustomerOrders** skojarzenie po klucze obce być narażone na **klienta** i  **Kolejność** typów jednostek. Za pomocą kluczy obcych widoczne, relacji między jednostkami odbywa się za pomocą **ReferentialConstraint** elementu. Odpowiedni element AssociationSetMapping nie jest konieczne do mapowania tego skojarzenia ze źródłem danych.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
   <ReferentialConstraint>
        <Principal Role="Customer">
            <PropertyRef Name="Id" />
        </Principal>
        <Dependent Role="Order">
             <PropertyRef Name="CustomerId" />
         </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="associationset-element-csdl"></a>Obiekt AssociationSet — Element (CSDL)

**AssociationSet** element język definicji schematu koncepcyjnego (CSDL) to kontener logiczny dla wystąpień skojarzeń tego samego typu. Zestaw skojarzeń zawiera definicję dla grupowania wystąpień skojarzeń, dzięki czemu mogą być mapowane do źródła danych.  

**AssociationSet** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden elementy dozwolone)
-   Końcowy (dokładnie dwa elementy wymagane)
-   Elementów adnotacji (zero lub więcej elementów dozwolone)

**Skojarzenia** atrybut określa typ powiązania, który zawiera zestaw skojarzeń. Zestawy jednostek, składających się kończy się zestawu skojarzeń są określane za pomocą podrzędnej dokładnie dwóch **zakończenia** elementów.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **AssociationSet** elementu.

| Nazwa atrybutu  | Jest wymagany | Wartość                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**        | Tak         | Nazwa zestawu jednostek. Wartość **nazwa** atrybut nie może być taka sama jak wartość **skojarzenia** atrybutu.                                 |
| **Skojarzenie** | Tak         | W pełni kwalifikowana nazwa skojarzenia, które zestawu skojarzeń zawiera wystąpienia. Skojarzenie musi być w tej samej przestrzeni nazw jako zestaw skojarzeń. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **AssociationSet** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EntityContainer** elementu przy użyciu dwóch **AssociationSet** elementy:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="collectiontype-element-csdl"></a>Element CollectionType (CSDL)

**CollectionType** element język definicji schematu koncepcyjnego (CSDL) określa, że parametr funkcji lub funkcji zwracany typ jest kolekcją. **CollectionType** element może być elementem podrzędnym elementu parametru lub element ReturnType (funkcja). Typ kolekcji można określić za pomocą **typu** atrybutu lub jedną z następujących elementów podrzędnych:

-   **Typ CollectionType**
-   Element referenceType
-   RowType
-   TypeRef

> [!NOTE]
> Model nie zostanie przeprowadzona Weryfikacja Jeśli typ kolekcji jest określony zarówno **typu** atrybut i nie zawiera elementu podrzędnego.

 

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **CollectionType** elementu. Należy pamiętać, że **DefaultValue**, **MaxLength**, **FixedLength**, **dokładności**, **skalowania**,  **Unicode**, i **sortowania** atrybuty dotyczą tylko kolekcje **EDMSimpleTypes**.

| Nazwa atrybutu                                                          | Jest wymagany | Wartość                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**                                                                | Nie          | Typ kolekcji.                                                                                                                                                                                                      |
| **Dopuszcza wartości null**                                                            | Nie          | **Wartość true,** (wartość domyślna) lub **False** w zależności od tego, czy właściwość może mieć wartości null. <br/> [!NOTE]                                                                                                                 |
| > W CSDL v1 musi mieć właściwość typu złożonego `Nullable="False"`. |             |                                                                                                                                                                                                                                  |
| **defaultValue**                                                        | Nie          | Wartość domyślna właściwości.                                                                                                                                                                                               |
| **Element maxLength**                                                           | Nie          | Maksymalna długość wartości właściwości.                                                                                                                                                                                        |
| **Wartości**                                                         | Nie          | **Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg znaków o stałej długości.                                                                                                                           |
| **Precyzja**                                                           | Nie          | Dokładność wartości właściwości.                                                                                                                                                                                             |
| **Skala**                                                               | Nie          | Skala wartości właściwości.                                                                                                                                                                                                 |
| **SRID**                                                                | Nie          | Identyfikator odwołania przestrzennego systemu. Prawidłowy tylko w przypadku właściwości typów przestrzennych.   Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) |
| **Unicode**                                                             | Nie          | **Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg Unicode.                                                                                                                                |
| **Sortowanie**                                                           | Nie          | Ciąg, który określa kolejność sortowania, które ma być używany w źródle danych.                                                                                                                                                    |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **CollectionType** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano funkcję definiowanych przez model, który używa **CollectionType** elementu, aby określić, że funkcja zwraca kolekcję **osoby** typów jednostek (jak określono za pomocą **ElementType** atrybutu).

``` xml
 <Function Name="LastNamesAfter">
        <Parameter Name="someString" Type="Edm.String"/>
        <ReturnType>
             <CollectionType  ElementType="SchoolModel.Person"/>
        </ReturnType>
        <DefiningExpression>
             SELECT VALUE p
             FROM SchoolEntities.People AS p
             WHERE p.LastName >= someString
        </DefiningExpression>
 </Function>
```
 

W poniższym przykładzie pokazano funkcji definiowanych przez model, który używa **CollectionType** elementu, aby określić, że funkcja zwraca Kolekcja wierszy (jak określono w **RowType** elementu).

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

W poniższym przykładzie pokazano funkcji definiowanych przez model, który używa **CollectionType** elementu, aby określić, że funkcja przyjmuje jako parametr zbiór **działu** typów jednostek.

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="complextype-element-csdl"></a>Element ComplexType (CSDL)

A **ComplexType** element definiuje strukturę danych składające się z **EdmSimpleType** właściwości lub inne typy złożone.  Typ złożony mogą być właściwość typu jednostki lub innego typu złożonego. Typ złożony jest podobny dla typu jednostki, w tym, że typ złożony definiuje dane. Jednakże istnieją niektóre podstawowe różnice między typy złożone i typy jednostek:

-   Typy złożone nie mają tożsamości (lub kluczy) i dlatego nie mogą istnieć niezależnie. Typy złożone może istnieć tylko jako właściwości typów jednostek lub inne typy złożone.
-   Typy złożone nie mogą uczestniczyć w skojarzenia. Ani end elementu association może być to typ złożony, i w związku z tym nie można zdefiniować właściwości nawigacji dla typów złożonych.
-   Właściwość typu złożonego nie może mieć wartość null, jeśli właściwości skalarne typu złożonego każdego można ustawić na wartość null.

A **ComplexType** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Właściwości (zero lub więcej elementów)
-   Elementów adnotacji (zero lub więcej elementów)

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **ComplexType** elementu.

| Nazwa atrybutu                                                                                                 | Jest wymagany | Wartość                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nazwa                                                                                                           | Tak         | Nazwa typu złożonego. Nazwa typu złożonego nie może być taka sama jak nazwa innego typu złożonego, typ jednostki lub skojarzenia, który znajduje się w zakresie modelu. |
| BaseType                                                                                                       | Nie          | Nazwa innego typu złożonego, który jest typ bazowy typ złożony, który jest definiowany. <br/> [!NOTE]                                                                     |
| > Ten atrybut nie ma zastosowania w wersji 1 CSDL. Dziedziczenia dla typów złożonych nie jest obsługiwane w tej wersji. |             |                                                                                                                                                                                     |
| Abstrakcyjny                                                                                                       | Nie          | **Wartość true,** lub **False** (wartość domyślna) w zależności od tego, czy typ złożony jest typem abstrakcyjnym. <br/> [!NOTE]                                                                  |
| > Ten atrybut nie ma zastosowania w wersji 1 CSDL. Typy złożone w tej wersji nie może być typów abstrakcyjnych.         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **ComplexType** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

Poniższy przykład przedstawia typ złożony, **adres**, za pomocą **EdmSimpleType** właściwości **adres**, **Miasto**,  **StanLubProwincja**, **kraju**, i **KodPocztowy**.

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

Aby zdefiniować typ złożony **adres** (p. wyżej) jako właściwość typu jednostki, należy zadeklarować typ właściwości w definicji typu jednostki. W poniższym przykładzie przedstawiono **adres** właściwość typu złożonego typu encji (**wydawcy**):

``` xml
 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BooksModel.Address" Name="Address" Nullable="false" />
       <NavigationProperty Name="Books" Relationship="BooksModel.PublishedBy"
                           FromRole="Publisher" ToRole="Book" />
     </EntityType>
```
 

 

## <a name="definingexpression-element-csdl"></a>Element DefiningExpression (CSDL)

**DefiningExpression** element język definicji schematu koncepcyjnego (CSDL) zawiera wyrażenie SQL jednostki, które definiuje funkcję w modelu koncepcyjnym.  

> [!NOTE]
> Do celów sprawdzania poprawności **DefiningExpression** element może zawierać dowolną zawartość. Jednak Entity Framework spowoduje zgłoszenie wyjątku w czasie wykonywania przypadku **DefiningExpression** element nie zawiera nieprawidłową SQL jednostki.

 

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **DefiningExpression** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie użyto **DefiningExpression** elementu, aby zdefiniować funkcję, która zwraca liczbę lat, ponieważ została opublikowana książki. Zawartość **DefiningExpression** elementu są zapisywane w języku SQL jednostki.

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Element zależne (CSDL)

**Zależne** element język definicji schematu koncepcyjnego (CSDL) jest elementem podrzędnym elementu ReferentialConstraint i definiuje zależne koniec ograniczenia referencyjnego. A **ReferentialConstraint** element definiuje funkcjonalność, która jest podobna do ograniczenia integralności referencyjnej w relacyjnej bazie danych. W ten sam sposób, w kolumnie (lub kolumny) z tabeli bazy danych odwołać się do klucza podstawowego z innej tabeli Właściwość (lub właściwości), typu jednostki odwoływać się do klucza jednostki innego typu jednostki. Typ jednostki, do którego istnieje odwołanie jest wywoływana *jednostki zakończenia* ograniczenia. Typ jednostki, który odwołuje się do zakończenia podmiotu zabezpieczeń jest wywoływana *zależne zakończenia* ograniczenia. **PropertyRef** elementy są używane do określenia, których kluczy odwoływać się do zakończenia podmiotu zabezpieczeń.

**Zależne** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   PropertyRef (jeden lub więcej elementów)
-   Elementów adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **zależne** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Rola**       | Tak         | Nazwa typu jednostki, na końcu zależne skojarzenia. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **zależne** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **ReferentialConstraint** używany jako część definicji elementu **PublishedBy** skojarzenia. **PublisherId** właściwość **książki** typu jednostki tworzy zależne koniec ograniczenia referencyjnego.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="documentation-element-csdl"></a>Element documentation (CSDL)

**Dokumentacji** element język definicji schematu koncepcyjnego (CSDL) może służyć do Podaj informacje dotyczące obiektu, który jest zdefiniowany w elemencie rodzica. W pliku edmx gdy **dokumentacji** element jest elementem podrzędnym elementu, który pojawia się jako obiekt na powierzchni projektowej w Projektancie platformy EF (np. jednostek, skojarzeń lub właściwości), zawartość **dokumentacji**  element będzie wyświetlany jako w programie Visual Studio **właściwości** okna dla obiektu.

**Dokumentacji** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   **Podsumowanie**: Krótki opis elementu nadrzędnego. (element zero lub jeden)
-   **LongDescription**: rozbudowany opis elementu nadrzędnego. (element zero lub jeden)
-   Elementy adnotacji. (elementy zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **dokumentacji** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **dokumentacji** element jako element podrzędny elementu EntityType. Gdyby poniższy fragment kodu w CSDL edmx zawartości pliku, zawartość **Podsumowanie** i **LongDescription** elementy pojawią się w programie Visual Studio **właściwości** okna po kliknięciu `Customer` typu jednostki.

``` xml
 <EntityType Name="Customer">
    <Documentation>
      <Summary>Summary here.</Summary>
      <LongDescription>Long description here.</LongDescription>
    </Documentation>
    <Key>
      <PropertyRef Name="CustomerId" />
    </Key>
    <Property Type="Int32" Name="CustomerId" Nullable="false" />
    <Property Type="String" Name="Name" Nullable="false" />
 </EntityType>
```
 

 

## <a name="end-element-csdl"></a>Element end (CSDL)

**Zakończenia** element język definicji schematu koncepcyjnego (CSDL) może być elementem podrzędnym elementu Association lub elementu AssociationSet. W każdym przypadku rolę **zakończenia** różni się elementu i atrybuty stosowane są różne.

### <a name="end-element-as-a-child-of-the-association-element"></a>Końcowy Element jako element podrzędny elementu Association

**Zakończenia** — element (jako element podrzędny elementu **skojarzenia** elementu) identyfikuje typ jednostki na jednym końcu asocjacji i liczbę wystąpień typu jednostki, które może znajdować się na końcu tego skojarzenia. Skojarzenia są zdefiniowane jako część skojarzenia; Skojarzenie musi mieć dokładnie dwa punkty końcowe skojarzenia. Wystąpienia typu jednostki na jednym końcu asocjacji jest możliwy za pośrednictwem właściwości nawigacji lub kluczy obcych, jeśli są one widoczne na typ jednostki.  

**Zakończenia** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   OnDelete (zero lub jeden element)
-   Elementów adnotacji (zero lub więcej elementów)

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **zakończenia** elementu, gdy jest elementem podrzędnym elementu **skojarzenia** elementu.

| Nazwa atrybutu   | Jest wymagany | Wartość                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**         | Tak         | Nazwa typu jednostki, na jednym końcu skojarzenia.                                                                                                                                                                                                                                                                                                                                                         |
| **Rola**         | Nie          | Nazwa dla elementu end skojarzenia. Jeśli nie podano żadnej nazwy, nazwa typu jednostki na punkt końcowy będzie używany.                                                                                                                                                                                                                                                                                           |
| **Liczebność** | Tak         | **1**, **od 0 do 1**, lub **\*** w zależności od liczby wystąpień typów jednostek, które mogą być na końcu skojarzenia. <br/> **1** oznacza tego wystąpienia typu dokładnie jedną jednostkę istnieje na końcu skojarzenia. <br/> **od 0 do 1** oznacza, że na końcu skojarzenia zera lub jednego wystąpienia typu jednostki. <br/> **\*** oznacza, że wartość zero, jeden lub więcej wystąpień typu jednostki na końcu skojarzenia. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **zakończenia** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

#### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **skojarzenia** element, który definiuje **CustomerOrders** skojarzenia. **Liczebność** wartości dla każdej **zakończenia** skojarzenia wskazują tę liczbę **zamówienia** może być skojarzony z **klienta**, ale tylko jeden **klienta** może być skojarzony z **kolejności**. Ponadto **OnDelete** elementu wskazuje, że wszystkie **zamówienia** powiązanych z określonego **klienta** i które zostały załadowane do obiektu ObjectContext będzie Jeśli usunięto **klienta** zostanie usunięty.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Końcowy Element jako element podrzędny elementu AssociationSet

**Zakończenia** element określa jeden z punktów końcowych zestaw skojarzeń. **AssociationSet** musi zawierać dwa **zakończenia** elementów. Informacje zawarte w **zakończenia** element jest używany w mapowaniu skojarzenia Ustaw ze źródłem danych.

**Zakończenia** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Elementów adnotacji (zero lub więcej elementów)

> [!NOTE]
> Adnotacja elementów musi występować po wszystkich innych elementów podrzędnych. Elementów adnotacji są dozwolone tylko w wersji 2 CSDL i nowszych wersjach.

 

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **zakończenia** elementu, gdy jest elementem podrzędnym elementu **AssociationSet** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Obiekt EntitySet**  | Tak         | Nazwa **EntitySet** element, który definiuje jeden z punktów końcowych nadrzędnego **AssociationSet** elementu. **EntitySet** element musi być zdefiniowany w tym samym kontenerze jednostki jako element nadrzędny **AssociationSet** elementu. |
| **Rola**       | Nie          | Nazwa skojarzenia zestawu zakończenia. Jeśli **roli** atrybut nie jest używany, nazwę punktu końcowego zestawu skojarzenia będzie nazwa zestawu jednostek.                                                                   |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **zakończenia** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

#### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EntityContainer** elementu przy użyciu dwóch **AssociationSet** przy użyciu dwóch elementów **zakończenia** elementy:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitycontainer-element-csdl"></a>Element EntityContainer (CSDL)

**EntityContainer** element język definicji schematu koncepcyjnego (CSDL) jest kontenerem logicznym, zestawów encji, zestawów skojarzeń i Importy — funkcja. Kontener jednostek modelu koncepcyjnego mapuje do kontenera magazynu modelu jednostki za pośrednictwem elementu w elemencie EntityContainerMapping. Kontener jednostek modelu magazynu w tym artykule opisano struktury bazy danych: zestawy jednostek opisują tabel, zestawów skojarzeń opisano ograniczenia klucza obcego i Importy funkcji opisano procedury składowane w bazie danych.

**EntityContainer** element może mieć zero lub jeden elementów dokumentacji. Jeśli **dokumentacji** element jest obecny, musi poprzedzać wszystkie **EntitySet**, **AssociationSet**, i **FunctionImport** elementów.

**EntityContainer** element może mieć zero lub więcej z następujących elementów podrzędnych (w podanej kolejności):

-   Obiekt EntitySet
-   Obiekt AssociationSet
-   Element FunctionImport
-   Elementów adnotacji

Możesz rozszerzyć **EntityContainer** element, aby uwzględnić zawartość innego **EntityContainer** który mieści się w tej samej przestrzeni nazw. Aby uwzględnić zawartość innego **EntityContainer**, w odwołujących się **obiektu EntityContainer** elementu, ustaw wartość **rozszerza** atrybutu Nazwa  **Obiekt EntityContainer** element, który chcesz dołączyć. Wszystkie elementy podrzędne elementu dołączonej **EntityContainer** elementu będą traktowane jako elementy podrzędne odwołujących się **EntityContainer** elementu.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **Using** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa kontenera jednostek.                               |
| **Rozszerza**    | Nie          | Nazwa innego kontenera jednostek w ramach tej samej przestrzeni nazw. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **EntityContainer** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EntityContainer** element, który definiuje trzy zestawów encji i zestawów skojarzeń dwa.

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entityset-element-csdl"></a>Element EntitySet (CSDL)

**EntitySet** element język definicji schematu koncepcyjnego to kontener logiczny dla wystąpień tego typu jednostki i wystąpień dowolnego typu, który jest tworzony na podstawie tego typu jednostki. Relacja między typem encji i zestaw jednostek jest analogiczne do relację wiersz tabeli w relacyjnej bazie danych. Np. wiersz typ jednostki definiuje zestaw powiązanych danych i jak tabela, zestaw jednostek zawiera wystąpienia tej definicji. Zestaw jednostek zapewnia konstrukcję grupowanie wystąpień typów jednostek, dzięki czemu mogą być mapowane do pokrewne struktury danych w źródle danych.  

Może być określona więcej niż jeden zestaw jednostek dla typu określonego obiektu.

> [!NOTE]
> W Projektancie platformy EF nie obsługuje modelami koncepcyjnymi, które zawierają wiele zestawów jednostek według typu.

 

**EntitySet** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Element documentation (zero lub jeden elementy dozwolone)
-   Elementów adnotacji (zero lub więcej elementów dozwolone)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **EntitySet** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa zestawu jednostek.                                                              |
| **Typ entityType** | Tak         | W pełni kwalifikowana nazwa typu jednostki, dla której zestaw jednostek zawiera wystąpienia. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **EntitySet** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EntityContainer** element z trzech **EntitySet** elementy:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

Istnieje możliwość definiowania wielu zestawów jednostek według typu (MEST). W poniższym przykładzie zdefiniowano kontener jednostek z dwoma zestawami jednostki dla **książki** typ jednostki:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="FictionBooks" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="BookAuthor" Association="BooksModel.BookAuthor">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitytype-element-csdl"></a>Element EntityType (CSDL)

**EntityType** element reprezentuje strukturę koncepcji najwyższego poziomu, takich jak klient lub kolejność, w modelu koncepcyjnym. Typ jednostki jest szablonem dla wystąpień typów jednostek w aplikacji. Każdy szablon zawiera następujące informacje:

-   Unikatowa nazwa. (Wymagane).
-   Klucz jednostki, który jest definiowany przez jedną lub więcej właściwości. (Wymagane).
-   Właściwości zawierający dane. (Opcjonalnie).
-   Właściwości nawigacji, które umożliwiają nawigację z jednym końcu asocjacji na drugiej stronie. (Opcjonalnie).

W aplikacji wystąpienia typu jednostki reprezentuje określonego obiektu (na przykład konkretnego klienta lub zamówienia). Każde wystąpienie typu jednostki musi mieć unikatowy klucz w ramach zestawu jednostek.

Dwa wystąpienia typu jednostki są traktowane jako równe tylko wtedy, gdy są one tego samego typu i wartości kluczy podmiotu są takie same.

**EntityType** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Klucz (zero lub jeden element)
-   Właściwości (zero lub więcej elementów)
-   Element NavigationProperty (zero lub więcej elementów)
-   Elementów adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **EntityType** elementu.

| Nazwa atrybutu                                                                                                                                  | Jest wymagany | Wartość                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Nazwa**                                                                                                                                        | Tak         | Nazwa typu jednostki.                                                                     |
| **BaseType**                                                                                                                                    | Nie          | Nazwa innego typu jednostki, który jest typem podstawowym typu jednostki, który jest definiowany.  |
| **Abstrakcyjny**                                                                                                                                    | Nie          | **Wartość true,** lub **False**, w zależności od tego, czy typ obiektu jest typem abstrakcyjnym.                 |
| **OpenType**                                                                                                                                    | Nie          | **Wartość true,** lub **False** w zależności od tego, czy typ jednostki jest typem jednostki open. <br/> [!NOTE] |
| > **OpenType** atrybut ma zastosowanie tylko do typów jednostek, które są zdefiniowane w modelach koncepcyjnych, które są używane z architektury ADO.NET Data Services. |             |                                                                                                  |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **EntityType** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EntityType** element z trzech **właściwość** elementów i dwóch **element NavigationProperty** elementy:

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="enumtype-element-csdl"></a>Element EnumType (CSDL)

**EnumType** element reprezentuje Typ wyliczany.

**EnumType** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Element członkowski (zero lub więcej elementów)
-   Elementów adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **EnumType** elementu.

| Nazwa atrybutu     | Jest wymagany | Wartość                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**           | Tak         | Nazwa typu jednostki.                                                                                                                                                                  |
| **IsFlags**        | Nie          | **Wartość true,** lub **False**, w zależności od tego, czy typ wyliczeniowy może służyć jako zestaw flag. Wartość domyślna to **wartość False.**.                                                                     |
| **UnderlyingType** | Nie          | **Edm.Byte**, **Edm.Int16**, **typem Edm.Int32**, **Edm.Int64** lub **Edm.SByte** ogranicza zakres wartości tego typu.   Domyślny typ podstawowy wyliczenia elementów to **typem Edm.Int32.**. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **EnumType** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EnumType** element z trzech **elementu członkowskiego** elementy:

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Function — Element (CSDL)

**Funkcja** element język definicji schematu koncepcyjnego (CSDL) jest używany do definiowania lub deklarowania funkcji w modelu koncepcyjnym. Funkcja jest zdefiniowana za pomocą elementu DefiningExpression.  

A **funkcja** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Parametr (zero lub więcej elementów)
-   DefiningExpression (zero lub jeden element)
-   ReturnType (funkcja) (zero lub jeden element)
-   Elementów adnotacji (zero lub więcej elementów)

Zwracana typ dla funkcji, należy określić albo **ReturnType** — element (funkcja) lub **ReturnType** atrybutu (patrz poniżej), ale nie oba. Możliwe zwracane typy są wszelkie EdmSimpleType, jednostki typu, typu złożonego, typ wiersza lub typu referencyjnego (lub zbiór jednego z następujących typów).

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **funkcja** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                              |
|:---------------|:------------|:-----------------------------------|
| **Nazwa**       | Tak         | Nazwa funkcji.          |
| **ReturnType** | Nie          | Typ zwracany przez funkcję. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **funkcja** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie użyto **funkcja** elementu, aby zdefiniować funkcję, która zwraca liczbę lat, ponieważ został zatrudniony pod kierunkiem instruktora.

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>Element FunctionImport (CSDL)

**FunctionImport** element język definicji schematu koncepcyjnego (CSDL) reprezentuje funkcję, która jest zdefiniowana w źródle danych, ale dostępne obiekty za pośrednictwem modelu koncepcyjnego. Na przykład elementem funkcji w modelu magazynu może służyć do reprezentowania procedurę składowaną w bazie danych. A **FunctionImport** elementu w modelu koncepcyjnym reprezentuje odpowiednich funkcji w aplikacji Entity Framework i jest mapowany do funkcji modelu magazynu przy użyciu elementu FunctionImportMapping. Gdy funkcja jest wywoływana w aplikacji, odpowiednie procedury składowanej jest wykonywany w bazie danych.

**FunctionImport** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden elementy dozwolone)
-   Parametr (zero lub więcej elementów dozwolone)
-   Elementów adnotacji (zero lub więcej elementów dozwolone)
-   ReturnType (FunctionImport) (zero lub więcej elementów dozwolone)

Jeden **parametru** element powinien być zdefiniowany dla każdego parametru, który akceptuje funkcji.

Zwracana typ dla funkcji, należy określić albo **ReturnType** elementu (element FunctionImport) lub **ReturnType** atrybutu (patrz poniżej), ale nie oba. Wartość zwracany typ musi być kolekcją EdmSimpleType, EntityType lub ComplexType.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **FunctionImport** elementu.

| Nazwa atrybutu   | Jest wymagany | Wartość                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**         | Tak         | Nazwa funkcji zaimportowane.                                                                                                                                                                    |
| **ReturnType**   | Nie          | Typ, który zwraca funkcja. Nie należy używać tego atrybutu, jeśli funkcja nie zwraca wartości. W przeciwnym razie wartość musi być kolekcją ComplexType, EntityType lub EDMSimpleType.        |
| **Obiekt EntitySet**    | Nie          | Jeśli funkcja zwraca kolekcję jednostek typy wartości **EntitySet** musi być zestaw jednostek do której należy kolekcji. W przeciwnym razie **EntitySet** atrybutu nie może być używany. |
| **IsComposable** | Nie          | Jeśli wartość jest równa true, funkcja jest konfigurowalna (funkcji zwracającej tabelę) i mogą być używane w zapytaniu LINQ.  Wartość domyślna to **false**.                                                           |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **FunctionImport** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **FunctionImport** element, który przyjmuje jeden parametr i zwraca kolekcję typów jednostek:

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Kluczowym elementem (CSDL)

**Klucz** element jest elementem podrzędnym elementu EntityType i definiuje *klucz jednostki* (właściwość lub ustaw właściwości typu jednostki, które określają tożsamość). Właściwości, które tworzą klucz jednostki są wybierane w czasie projektowania. Wartości właściwości klucza jednostki musi jednoznacznie identyfikują wystąpienie jednostki typu w obrębie jednostki ustawienie w czasie wykonywania. Należy wybrać właściwości, które tworzą klucz jednostki w celu zagwarantowania unikatowości wystąpień w zestawie jednostek. **Klucz** element definiuje klucz jednostki, odwołując się do przynajmniej jednej z właściwości typu jednostki.

**Klucz** element może mieć następujące elementy podrzędne:

-   PropertyRef (jeden lub więcej elementów)
-   Elementów adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **klucz** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

Poniższy przykład definiuje typ jednostki o nazwie **książki**. Klucz jednostki jest zdefiniowany, odwołując się do **ISBN** właściwość typu jednostki.

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

**ISBN** właściwość jest dobrym wyborem dla klucza jednostki, ponieważ International Standard książki numer (ISBN) unikatowo identyfikuje książki.

Poniższy przykład przedstawia typ jednostki (**Autor**) zawierający klucz jednostki, która składa się z dwóch właściwości **nazwa** i **adres**.

``` xml
 <EntityType Name="Author">
   <Key>
     <PropertyRef Name="Name" />
     <PropertyRef Name="Address" />
   </Key>
   <Property Type="String" Name="Name" Nullable="false" />
   <Property Type="String" Name="Address" Nullable="false" />
   <NavigationProperty Name="Books" Relationship="BooksModel.WrittenBy"
                       FromRole="Author" ToRole="Book" />
 </EntityType>
```
 

Za pomocą **nazwa** i **adres** jednostki klucz jest wyboru rozsądny, ponieważ dwóch autorów o takiej samej nazwie jest mało prawdopodobne na żywo na ten sam adres. Jednak ten wybór dla klucza jednostki absolutnie nie gwarantuje klucze unikatowe jednostki w zestawie jednostek. Dodawanie właściwości, takie jak **wartość IDAutora**, który może być używany do jednoznacznego identyfikowania Autor może w tym przypadku zalecane.

 

## <a name="member-element-csdl"></a>Element członkowski (CSDL)

**Elementu członkowskiego** element jest elementem podrzędnym elementu EnumType i definiuje składową typu wyliczeniowego.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **FunctionImport** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa elementu członkowskiego.                                                                                                                                                                  |
| **Wartość**      | Nie          | Wartość elementu członkowskiego. Domyślnie pierwszy element członkowski ma wartość 0, a wartość każdego kolejnych moduł wyliczający jest zwiększana o 1. Może istnieć wiele składowych o tej samej wartości. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **FunctionImport** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EnumType** element z trzech **elementu członkowskiego** elementy:

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>Element NavigationProperty — Element (CSDL)

A **element NavigationProperty** element definiuje właściwość nawigacji, który zawiera dokumentację na drugim końcu skojarzenia. W odróżnieniu od właściwości zdefiniowane przy użyciu elementu właściwości właściwości nawigacji definiuje kształt i właściwości danych. Zapewniają one sposób przechodzenia skojarzenie między dwoma typami encji.

Należy pamiętać, że właściwości nawigacji są opcjonalne na obu typów jednostek na końcach asocjacji. Jeśli zdefiniujesz właściwości nawigacji na jednym typie jednostek na końcu asocjacji, ma Zdefiniuj właściwość nawigacji typu jednostki na drugim końcu skojarzenia.

Typ danych zwracanych przez właściwość nawigacji jest ustalana liczebność jej punkt końcowy skojarzenia zdalnego. Na przykład, załóżmy, że właściwość nawigacji, **OrdersNavProp**, istnieje na **klienta** typu jednostki i przechodzi skojarzenia typu jeden do wielu między **klienta** i  **Kolejność**. Ponieważ punkt końcowy skojarzenia zdalnego dla właściwości nawigacji ma liczebność wiele (\*), jego typ danych to kolekcja (z **kolejności**). Podobnie, jeśli właściwość nawigacji, **CustomerNavProp**, istnieje na **kolejności** typu jednostki, będzie jego typu danych **klienta** ponieważ liczebności zakończenia zdalnego jeden (1).

A **element NavigationProperty** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Elementów adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **element NavigationProperty** elementu.

| Nazwa atrybutu   | Jest wymagany | Wartość                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**         | Tak         | Nazwa właściwości nawigacji.                                                                                                                                                                                                             |
| **Relacja** | Tak         | Nazwa skojarzenia, które znajduje się w zakresie modelu.                                                                                                                                                                                |
| **ToRole**       | Tak         | Koniec asocjacji, na której kończy się nawigacji. Wartość **ToRole** atrybut musi być taka sama jak wartość jednego z **roli** atrybuty zdefiniowane w jednym z punktów końcowych skojarzenia (zdefiniowanej w elemencie punkt końcowy skojarzenia).       |
| **Wartości elementów FromRole**     | Tak         | Koniec skojarzenia, w którym rozpoczyna się nawigacji. Wartość **FromRole** atrybut musi być taka sama jak wartość jednego z **roli** atrybuty zdefiniowane w jednym z punktów końcowych skojarzenia (zdefiniowanej w elemencie punkt końcowy skojarzenia). |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **element NavigationProperty** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie zdefiniowano typ jednostki (**książki**) z dwóch właściwości nawigacji (**PublishedBy** i **WrittenBy**):

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="ondelete-element-csdl"></a>Element OnDelete (CSDL)

**OnDelete** element język definicji schematu koncepcyjnego (CSDL) definiuje zachowanie, która jest połączona z skojarzenia. Jeśli **akcji** ma ustawioną wartość atrybutu **Cascade** na jednym końcu asocjacji powiązanych typów jednostek na drugim końcu skojarzenia zostaną usunięte po usunięciu typu jednostki, które na pierwszy punkt końcowy. Jeśli skojarzenie między dwoma typami encji jest relacji klucza podstawowego klucza do podstawowej, a następnie załadować obiektu zależnego zostanie usunięty po usunięciu obiekt podmiotu zabezpieczeń na drugim końcu skojarzenia niezależnie od wartości **OnDelete** Specyfikacja.  

> [!NOTE]
> **OnDelete** element ma wpływ tylko na zachowanie środowiska uruchomieniowego aplikacji; nie ma wpływu na zachowanie w źródle danych. Zachowanie zdefiniowane w źródle danych powinna być taka sama jak zachowanie zdefiniowane w aplikacji.

 

**OnDelete** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Elementów adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **OnDelete** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Akcja**     | Tak         | **Kaskadowe** lub **Brak**. Jeśli **Cascade**, typy jednostek zależnych zostaną usunięte po usunięciu typ podmiotu zabezpieczeń jednostki. Jeśli **Brak**, typy jednostek zależnych nie zostaną usunięte po usunięciu typ podmiotu zabezpieczeń jednostki. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **skojarzenia** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **skojarzenia** element, który definiuje **CustomerOrders** skojarzenia. **OnDelete** elementu wskazuje, że wszystkie **zamówienia** powiązanych z określonego **klienta** i zostały załadowane do obiektu ObjectContext zostaną usunięte po  **Klient** zostanie usunięty.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Parameter — Element (CSDL)

**Parametru** element język definicji schematu koncepcyjnego (CSDL) może być elementem podrzędnym elementu FunctionImport lub elemencie Function.

### <a name="functionimport-element-application"></a>FunctionImport Element aplikacji

A **parametru** — element (jako element podrzędny elementu **FunctionImport** elementu) służy do definiowania parametry wejściowe i wyjściowe w przypadku importowania funkcji, które są zadeklarowane w CSDL.

**Parametru** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden elementy dozwolone)
-   Elementów adnotacji (zero lub więcej elementów dozwolone)

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **parametru** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa parametru.                                                                                                                                                                                                      |
| **Typ**       | Tak         | Typ parametru. Wartość musi być **EDMSimpleType** lub typ złożony, który znajduje się w zakresie modelu.                                                                                                             |
| **Tryb**       | Nie          | **W**, **się**, lub **InOut** w zależności od tego, czy parametr jest danych wejściowych, danych wyjściowych lub parametr input/output.                                                                                                                |
| **Element maxLength**  | Nie          | Maksymalna dozwolona liczba znaków parametru.                                                                                                                                                                                    |
| **Precyzja**  | Nie          | Dokładność parametru.                                                                                                                                                                                                 |
| **Skala**      | Nie          | Skala parametru.                                                                                                                                                                                                     |
| **SRID**       | Nie          | Identyfikator odwołania przestrzennego systemu. Prawidłowy tylko dla parametrów typów przestrzennych. Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **parametru** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

#### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **FunctionImport** elementu za pomocą jednego **parametru** elementu podrzędnego. Funkcja przyjmuje jeden parametr wejściowy i zwraca kolekcję typów jednostek.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>Funkcja elementu aplikacji

A **parametru** — element (jako element podrzędny elementu **funkcja** elementu) definiuje parametry dla funkcji, które zdefiniowana lub zadeklarowana w modelu koncepcyjnym.

**Parametru** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden elementy)
-   Typ CollectionType (zero lub jeden elementy)
-   Element ReferenceType (zero lub jeden elementy)
-   RowType (zero lub jeden elementy)

> [!NOTE]
> Tylko jeden z **CollectionType**, **ReferenceType**, lub **RowType** elementy mogą być elementem podrzędnym **właściwość** elementu.

 

-   Elementów adnotacji (zero lub więcej elementów dozwolone)

> [!NOTE]
> Adnotacja elementów musi występować po wszystkich innych elementów podrzędnych. Elementów adnotacji są dozwolone tylko w wersji 2 CSDL i nowszych wersjach.

 

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **parametru** elementu.

| Nazwa atrybutu   | Jest wymagany | Wartość                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**         | Tak         | Nazwa parametru.                                                                                                                                                                                                      |
| **Typ**         | Nie          | Typ parametru. Parametr może być dowolny z następujących typów (lub kolekcje z tych typów): <br/> **EdmSimpleType** <br/> Typ jednostki <br/> Typ złożony <br/> Typ wiersza <br/> typ odwołania                             |
| **Dopuszcza wartości null**     | Nie          | **Wartość true,** (wartość domyślna) lub **False** w zależności od tego, czy właściwość może mieć **null** wartość.                                                                                                                          |
| **defaultValue** | Nie          | Wartość domyślna właściwości.                                                                                                                                                                                              |
| **Element maxLength**    | Nie          | Maksymalna długość wartości właściwości.                                                                                                                                                                                       |
| **Wartości**  | Nie          | **Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg znaków o stałej długości.                                                                                                                          |
| **Precyzja**    | Nie          | Dokładność wartości właściwości.                                                                                                                                                                                            |
| **Skala**        | Nie          | Skala wartości właściwości.                                                                                                                                                                                                |
| **SRID**         | Nie          | Identyfikator odwołania przestrzennego systemu. Prawidłowy tylko w przypadku właściwości typów przestrzennych. Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**      | Nie          | **Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg Unicode.                                                                                                                               |
| **Sortowanie**    | Nie          | Ciąg, który określa kolejność sortowania, które ma być używany w źródle danych.                                                                                                                                                   |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **parametru** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

#### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **funkcja** element, który korzysta z jednego **parametru** elementu podrzędnego, aby zdefiniować parametr funkcji.

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
``` 

 

## <a name="principal-element-csdl"></a>Element jednostki (CSDL)

**Jednostki** element język definicji schematu koncepcyjnego (CSDL) jest element podrzędny do elementu ReferentialConstraint, który definiuje główny koniec ograniczenia referencyjnego. A **ReferentialConstraint** element definiuje funkcjonalność, która jest podobna do ograniczenia integralności referencyjnej w relacyjnej bazie danych. W ten sam sposób, w kolumnie (lub kolumny) z tabeli bazy danych odwołać się do klucza podstawowego z innej tabeli Właściwość (lub właściwości), typu jednostki odwoływać się do klucza jednostki innego typu jednostki. Typ jednostki, do którego istnieje odwołanie jest wywoływana *jednostki zakończenia* ograniczenia. Typ jednostki, który odwołuje się do zakończenia podmiotu zabezpieczeń jest wywoływana *zależne zakończenia* ograniczenia. **PropertyRef** elementy są używane do określania, klucze, które są przywoływane przez zależne zakończenia.

**Jednostki** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   PropertyRef (jeden lub więcej elementów)
-   Elementów adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **jednostki** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Rola**       | Tak         | Nazwa typu jednostki, na końcu jednostki skojarzenia. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **jednostki** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **ReferentialConstraint** element, który jest częścią definicji **PublishedBy** skojarzenia. **Identyfikator** właściwość **wydawcy** typu jednostki stanowi główny koniec ograniczenia referencyjnego.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="property-element-csdl"></a>Property — Element (CSDL)

**Właściwość** element język definicji schematu koncepcyjnego (CSDL) może być elementem podrzędnym elementu EntityType, ComplexType element lub RowType element.

### <a name="entitytype-and-complextype-element-applications"></a>Obiekt EntityType i ComplexType elementu aplikacji

**Właściwość** elementów (jako elementy podrzędne **EntityType** lub **ComplexType** elementy) zdefiniuj kształt i charakterystyki danych, która będzie zawierać wystąpienia typu jednostki lub typ złożony . Właściwości w modelu koncepcyjnym są analogiczne do właściwości, które są zdefiniowane w klasie. W ten sam sposób, że właściwości w klasie określenia kształtu elementu klasy i zawierają informacje o obiektach właściwości w modelu koncepcyjnym określenia kształtu typu jednostki i zawierają informacje na temat wystąpień typu jednostki.

**Właściwość** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Element documentation (zero lub jeden elementy dozwolone)
-   Elementów adnotacji (zero lub więcej elementów dozwolone)

Następujące aspekty mogą być stosowane do **właściwość** element: **Nullable**, **DefaultValue**, **MaxLength**,  **Wartości**, **dokładności**, **skalowania**, **Unicode**, **sortowania**,  **Właściwość ConcurrencyMode**. Zestawy reguł są atrybuty XML, które zawierają informacje dotyczące sposobu wartości właściwości są przechowywane w magazynie danych.

> [!NOTE]
> Aspektami może być stosowany tylko do właściwości typu **EDMSimpleType**.

 

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **właściwość** elementu.

| Nazwa atrybutu                                                         | Jest wymagany | Wartość                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**                                                               | Tak         | Nazwa właściwości.                                                                                                                                                                                                       |
| **Typ**                                                               | Tak         | Typ wartości właściwości. Typ wartości właściwości musi być **EDMSimpleType** lub typ złożony (wskazywanym przez w pełni kwalifikowaną nazwą), który znajduje się w zakresie modelu.                                                 |
| **Dopuszcza wartości null**                                                           | Nie          | **Wartość true,** (wartość domyślna) lub <strong>False</strong> w zależności od tego, czy właściwość może mieć wartości null. <br/> [!NOTE]                                                                                                   |
| > W wersji 1 CSDL musi mieć właściwość typu złożonego `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **defaultValue**                                                       | Nie          | Wartość domyślna właściwości.                                                                                                                                                                                              |
| **Element maxLength**                                                          | Nie          | Maksymalna długość wartości właściwości.                                                                                                                                                                                       |
| **Wartości**                                                        | Nie          | **Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg znaków o stałej długości.                                                                                                                          |
| **Precyzja**                                                          | Nie          | Dokładność wartości właściwości.                                                                                                                                                                                            |
| **Skala**                                                              | Nie          | Skala wartości właściwości.                                                                                                                                                                                                |
| **SRID**                                                               | Nie          | Identyfikator odwołania przestrzennego systemu. Prawidłowy tylko w przypadku właściwości typów przestrzennych. Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                            | Nie          | **Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg Unicode.                                                                                                                               |
| **Sortowanie**                                                          | Nie          | Ciąg, który określa kolejność sortowania, które ma być używany w źródle danych.                                                                                                                                                   |
| **Właściwość ConcurrencyMode**                                                    | Nie          | **Brak** (wartość domyślna) lub **stałe**. Jeśli wartość jest równa **stałe**, zostanie użyta wartość właściwości w kontroli optymistycznej współbieżności.                                                                                  |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **właściwość** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

#### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EntityType** element z trzech **właściwość** elementy:

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

W poniższym przykładzie przedstawiono **ComplexType** element z pięciu **właściwość** elementy:

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a>RowType Element aplikacji

**Właściwość** elementów (jako element podrzędny elementu **RowType** elementu) zdefiniuj kształt i charakterystyki dane, które mogą być przekazywane do lub zwracany z funkcji definiowanych przez model.  

**Właściwość** element może mieć dokładnie jeden z następujących elementów podrzędnych:

-   Typ CollectionType
-   Element referenceType
-   RowType

**Właściwość** element może mieć żadnych liczba elementów podrzędnych w adnotacji.

> [!NOTE]
> Elementów adnotacji są dozwolone tylko w wersji 2 CSDL i nowszych wersjach.

 

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **właściwość** elementu.

| Nazwa atrybutu                                                     | Jest wymagany | Wartość                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**                                                           | Tak         | Nazwa właściwości.                                                                                                                                                                                                       |
| **Typ**                                                           | Tak         | Typ wartości właściwości.                                                                                                                                                                                                 |
| **Dopuszcza wartości null**                                                       | Nie          | **Wartość true,** (wartość domyślna) lub **False** w zależności od tego, czy właściwość może mieć wartości null. <br/> [!NOTE]                                                                                                                |
| > W wersji 1 CSDL musi mieć właściwość typu złożonego `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **defaultValue**                                                   | Nie          | Wartość domyślna właściwości.                                                                                                                                                                                              |
| **Element maxLength**                                                      | Nie          | Maksymalna długość wartości właściwości.                                                                                                                                                                                       |
| **Wartości**                                                    | Nie          | **Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg znaków o stałej długości.                                                                                                                          |
| **Precyzja**                                                      | Nie          | Dokładność wartości właściwości.                                                                                                                                                                                            |
| **Skala**                                                          | Nie          | Skala wartości właściwości.                                                                                                                                                                                                |
| **SRID**                                                           | Nie          | Identyfikator odwołania przestrzennego systemu. Prawidłowy tylko w przypadku właściwości typów przestrzennych. Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Nie          | **Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg Unicode.                                                                                                                               |
| **Sortowanie**                                                      | Nie          | Ciąg, który określa kolejność sortowania, które ma być używany w źródle danych.                                                                                                                                                   |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **właściwość** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

#### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **właściwość** elementy służące do określenia kształtu elementu zwracany typ funkcji definiowanych przez model.

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

 

## <a name="propertyref-element-csdl"></a>Element PropertyRef (CSDL)

**PropertyRef** element język definicji schematu koncepcyjnego (CSDL) odwołuje się do właściwości typu jednostki, aby wskazać, że właściwość wykona jedną z następujących ról:

-   Część klucza jednostki (właściwość lub ustaw właściwości typu jednostki, które określają tożsamość). Co najmniej jeden **PropertyRef** elementy mogą być używane do definiowania klucza jednostki.
-   Koniec zależnych lub jednostki ograniczenia referencyjnego.

**PropertyRef** element może mieć tylko elementów adnotacji (zero lub więcej) jako elementy podrzędne.

> [!NOTE]
> Elementów adnotacji są dozwolone tylko w wersji 2 CSDL i nowszych wersjach.

 

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **PropertyRef** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                |
|:---------------|:------------|:-------------------------------------|
| **Nazwa**       | Tak         | Nazwa właściwości, której dotyczy odwołanie. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **PropertyRef** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

Poniższy przykład definiuje typ jednostki (**książki**). Klucz jednostki jest zdefiniowany, odwołując się do **ISBN** właściwość typu jednostki.

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

W następnym przykładzie dwóch **PropertyRef** elementy są używane w celu wskazania, że dwie właściwości (**identyfikator** i **PublisherId**) są głównym i zależnym końców typu odwołanie ograniczenie.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="referencetype-element-csdl"></a>Element ReferenceType (CSDL)

**ReferenceType** element język definicji schematu koncepcyjnego (CSDL) Określa odwołanie do typu jednostki. **ReferenceType** element może być elementem podrzędnym następujące elementy:

-   ReturnType (funkcja)
-   Parametr
-   Typ CollectionType

**ReferenceType** element jest używany podczas definiowania parametr lub zwracany typ funkcji.

A **ReferenceType** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Elementów adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **ReferenceType** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                         |
|:---------------|:------------|:----------------------------------------------|
| **Typ**       | Tak         | Nazwa typu jednostki, do którego nastąpiło odwołanie. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **ReferenceType** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **ReferenceType** element używany jako element podrzędny elementu **parametru** elementu w funkcji definiowanych przez model, który akceptuje odwołanie do **osoby** jednostki Typ:

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
   <Parameter Name="instructor">
     <ReferenceType Type="SchoolModel.Person" />
   </Parameter>
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

W poniższym przykładzie przedstawiono **ReferenceType** element używany jako element podrzędny elementu **ReturnType** — element (funkcja) w funkcji definiowanych przez model, który zwraca odwołanie do **osoby**typ jednostki:

``` xml
 <Function Name="GetPersonReference">
     <Parameter Name="p" Type="SchoolModel.Person" />
     <ReturnType>
         <ReferenceType Type="SchoolModel.Person" />
     </ReturnType>
     <DefiningExpression>
           REF(p)
     </DefiningExpression>
 </Function>
```
 

 

## <a name="referentialconstraint-element-csdl"></a>Element ReferentialConstraint (CSDL)

A **ReferentialConstraint** element w język definicji schematu koncepcyjnego (CSDL) definiuje funkcjonalność, która jest podobna do ograniczenia integralności referencyjnej w relacyjnej bazie danych. W ten sam sposób, w kolumnie (lub kolumny) z tabeli bazy danych odwołać się do klucza podstawowego z innej tabeli Właściwość (lub właściwości), typu jednostki odwoływać się do klucza jednostki innego typu jednostki. Typ jednostki, do którego istnieje odwołanie jest wywoływana *jednostki zakończenia* ograniczenia. Typ jednostki, który odwołuje się do zakończenia podmiotu zabezpieczeń jest wywoływana *zależne zakończenia* ograniczenia.

Jeśli klucz obcy, która jest widoczna w jednej jednostce typu odwołuje się do właściwości na inny typ jednostki, **ReferentialConstraint** element definiuje skojarzenie między tymi typami dwie jednostki. Ponieważ **ReferentialConstraint** element udostępnia dowiedzieć się, jak dwa typy jednostek są powiązane z nie odpowiadającego elementu AssociationSetMapping niezbędne jest w języku specyfikacji mapowanie (MSL). Skojarzenie między dwoma typami jednostek, które nie mają klucze obce udostępniane musi mieć odpowiedni **AssociationSetMapping** element, aby zamapować informacji o skojarzeniu ze źródłem danych.

Jeśli klucz obcy nie jest uwidaczniana typu encji **ReferentialConstraint** elementu można zdefiniować tylko klucz do podstawowego ograniczenia klucza podstawowego między typem jednostki i innego typu jednostki.

A **ReferentialConstraint** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Podmiot zabezpieczeń (dokładnie jeden element)
-   Zależnych od ustawień lokalnych (dokładnie jeden element)
-   Elementów adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

**ReferentialConstraint** element może mieć dowolną liczbę adnotacji atrybutów (niestandardowe atrybuty XML). Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **ReferentialConstraint** używany jako część definicji elementu **PublishedBy** skojarzenia.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="returntype-function-element-csdl"></a>Element ReturnType (funkcja) (CSDL)

**ReturnType** (funkcja) element w język definicji schematu koncepcyjnego (CSDL) określa typ zwracany dla funkcji, która jest zdefiniowana w elemencie Function. Można również określić typ zwracany z funkcji **ReturnType** atrybutu.

Zwraca typy mogą być **EdmSimpleType**, typ jednostki, typu złożonego, typ wiersza, typu referencyjnego lub zbiór jednego z tych typów.

Zwracany typ funkcji można określić za pomocą albo **typu** atrybutu **ReturnType** elementu (funkcja), czy jeden z następujących elementów podrzędnych:

-   Typ CollectionType
-   Element referenceType
-   RowType

> [!NOTE]
> Model nie zostanie zweryfikowany, jeśli określisz, że funkcja zwracany typ zarówno **typu** atrybutu **ReturnType** elementu (funkcja) i jeden z elementów podrzędnych.

 

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **ReturnType** — element (funkcja).

| Nazwa atrybutu | Jest wymagany | Wartość                              |
|:---------------|:------------|:-----------------------------------|
| **ReturnType** | Nie          | Typ zwracany przez funkcję. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **ReturnType** — element (funkcja). Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie użyto **funkcja** elementu, aby zdefiniować funkcji, która zwraca liczbę lat książki znajdował się w drukowania. Należy zauważyć, że typ zwracany jest określony przez **typu** atrybutu **ReturnType** — element (funkcja).

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>Element ReturnType (FunctionImport) (CSDL)

**ReturnType** elementu (element FunctionImport) w język definicji schematu koncepcyjnego (CSDL) określa typ zwracany dla funkcji, która jest zdefiniowana w elemencie FunctionImport. Można również określić typ zwracany z funkcji **ReturnType** atrybutu.

Zwraca typy mogą być dowolnej kolekcji typu jednostki, typu złożonego lub **EdmSimpleType**,

Zwracany typ funkcji jest określony za pomocą **typu** atrybutu **ReturnType** elementu (element FunctionImport).

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **ReturnType** elementu (element FunctionImport).

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**       | Nie          | Typ, który zwraca funkcja. Wartość musi być kolekcją ComplexType, EntityType lub EDMSimpleType.                                                                                      |
| **Obiekt EntitySet**  | Nie          | Jeśli funkcja zwraca kolekcję jednostek typy wartości **EntitySet** musi być zestaw jednostek do której należy kolekcji. W przeciwnym razie **EntitySet** atrybutu nie może być używany. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **ReturnType** elementu (element FunctionImport). Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie użyto **FunctionImport** zwracającego książki i wydawców. Należy pamiętać, że funkcja zwraca dwa zestawy wyników i w związku z tym dwa **ReturnType** są określone elementy (FunctionImport).

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>Element RowType (CSDL)

A **RowType** element w język definicji schematu koncepcyjnego (CSDL) definiuje strukturę nienazwane jako parametr lub zwracany typ funkcji zdefiniowanych w modelu koncepcyjnym.

A **RowType** element może być elementem podrzędnym elementu następujące elementy:

-   Typ CollectionType
-   Parametr
-   ReturnType (funkcja)

A **RowType** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Właściwości (jeden lub więcej)
-   Elementów adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **RowType** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano funkcji definiowanych przez model, który używa **CollectionType** elementu, aby określić, że funkcja zwraca Kolekcja wierszy (jak określono w **RowType** elementu).

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```

## <a name="schema-element-csdl"></a>Element schematu (CSDL)

**Schematu** element jest elementem głównym modelu koncepcyjnego definicji. Zawiera definicje dla obiektów, funkcji i kontenerów, które tworzą modelu koncepcyjnego.

**Schematu** element może zawierać zero lub więcej z następujących elementów podrzędnych:

-   za pomocą
-   Obiekt EntityContainer
-   Typ entityType
-   EnumType
-   Skojarzenie
-   ComplexType
-   Funkcja

A **schematu** element może zawierać zero lub jeden elementów adnotacji.

> [!NOTE]
> **Funkcja** element i adnotacji elementy są dozwolone tylko w wersji 2 CSDL i nowszych wersjach.

 

**Schematu** element używa **Namespace** atrybut do definiowania przestrzeni nazw dla typu jednostki, typu złożonego i skojarzenia obiekty w modelu koncepcyjnym. W przestrzeni nazw nie dwa obiekty mogą mieć takiej samej nazwie. Przestrzenie nazw może obejmować wiele **schematu** elementów i wiele plików .csdl.

Model koncepcyjny przestrzeni nazw różni się od przestrzeni nazw XML **schematu** elementu. Model koncepcyjny przestrzeni nazw (zgodnie z definicją **Namespace** atrybutu) to logiczny kontener przeznaczony dla typów jednostek, typy złożone i typy stowarzyszenie. Przestrzeń nazw XML (wskazywanym przez **xmlns** atrybutu) z **schematu** element jest domyślny obszar nazw dla elementów podrzędnych i atrybutów **schematu** elementu. Obszary nazw XML w postaci http://schemas.microsoft.com/ado/YYYY/MM/edm (gdzie RRRR i MM stanowi rok i miesiąc odpowiednio) są zarezerwowane dla CSDL. Niestandardowe elementy i atrybuty nie może być w przestrzeni nazw, które mają postać.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty mogą być stosowane do **schematu** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Tak         | Przestrzeń nazw modelu koncepcyjnego. Wartość **Namespace** atrybut jest używany w celu utworzenia w pełni kwalifikowana nazwa typu. Na przykład jeśli **EntityType** o nazwie *klienta* znajduje się w przestrzeni nazw Simple.Example.Model, a następnie w pełni kwalifikowana nazwa **EntityType** jest SimpleExampleModel.Customer. <br/> Nie można użyć następujących ciągów jako wartość pozycji **Namespace** atrybut: **systemu**, **przejściowy**, lub **Edm**. Wartość **Namespace** atrybut nie może być taka sama jak wartość **Namespace** atrybutu w elemencie schematu SSDL. |
| **Alias**      | Nie          | Identyfikator używany zamiast nazwy przestrzeni nazw. Na przykład jeśli **EntityType** o nazwie *klienta* znajduje się w przestrzeni nazw Simple.Example.Model i wartość **Alias** atrybut jest *modelu*, wówczas można użyć Model.Customer jako w pełni kwalifikowana nazwa **typu EntityType.**                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **schematu** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **schematu** element, który zawiera **EntityContainer** element, dwa **EntityType** elementów, a drugi **skojarzenia** elementu.

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
       Namespace="ExampleModel" Alias="Self">
         <EntityContainer Name="ExampleModelContainer">
           <EntitySet Name="Customers"
                      EntityType="ExampleModel.Customer" />
           <EntitySet Name="Orders" EntityType="ExampleModel.Order" />
           <AssociationSet
                       Name="CustomerOrder"
                       Association="ExampleModel.CustomerOrders">
             <End Role="Customer" EntitySet="Customers" />
             <End Role="Order" EntitySet="Orders" />
           </AssociationSet>
         </EntityContainer>
         <EntityType Name="Customer">
           <Key>
             <PropertyRef Name="CustomerId" />
           </Key>
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
           <Property Type="String" Name="Name" Nullable="false" />
           <NavigationProperty
                    Name="Orders"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Customer" ToRole="Order" />
         </EntityType>
         <EntityType Name="Order">
           <Key>
             <PropertyRef Name="OrderId" />
           </Key>
           <Property Type="Int32" Name="OrderId" Nullable="false" />
           <Property Type="Int32" Name="ProductId" Nullable="false" />
           <Property Type="Int32" Name="Quantity" Nullable="false" />
           <NavigationProperty
                    Name="Customer"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Order" ToRole="Customer" />
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
         </EntityType>
         <Association Name="CustomerOrders">
           <End Type="ExampleModel.Customer"
                Role="Customer" Multiplicity="1" />
           <End Type="ExampleModel.Order"
                Role="Order" Multiplicity="*" />
           <ReferentialConstraint>
             <Principal Role="Customer">
               <PropertyRef Name="CustomerId" />
             </Principal>
             <Dependent Role="Order">
               <PropertyRef Name="CustomerId" />
             </Dependent>
           </ReferentialConstraint>
         </Association>
       </Schema>
```
 

 

## <a name="typeref-element-csdl"></a>Element TypeRef (CSDL)

**TypeRef** element język definicji schematu koncepcyjnego (CSDL) zawiera odwołanie do istniejącego typu nazwanego. **TypeRef** element może być elementem podrzędnym elementu CollectionType, który jest używany do określenia, czy funkcja jest kolekcją jako parametr lub zwracany typ.

A **TypeRef** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Elementów adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **TypeRef** elementu. Należy pamiętać, że **DefaultValue**, **MaxLength**, **FixedLength**, **dokładności**, **skalowania**,  **Unicode**, i **sortowania** atrybuty dotyczą tylko **EDMSimpleTypes**.

| Nazwa atrybutu                                                     | Jest wymagany | Wartość                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**                                                           | Nie          | Nazwa typu, do którego nastąpiło odwołanie.                                                                                                                                                                                          |
| **Dopuszcza wartości null**                                                       | Nie          | **Wartość true,** (wartość domyślna) lub **False** w zależności od tego, czy właściwość może mieć wartości null. <br/> [!NOTE]                                                                                                                |
| > W wersji 1 CSDL musi mieć właściwość typu złożonego `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **defaultValue**                                                   | Nie          | Wartość domyślna właściwości.                                                                                                                                                                                              |
| **Element maxLength**                                                      | Nie          | Maksymalna długość wartości właściwości.                                                                                                                                                                                       |
| **Wartości**                                                    | Nie          | **Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg znaków o stałej długości.                                                                                                                          |
| **Precyzja**                                                      | Nie          | Dokładność wartości właściwości.                                                                                                                                                                                            |
| **Skala**                                                          | Nie          | Skala wartości właściwości.                                                                                                                                                                                                |
| **SRID**                                                           | Nie          | Identyfikator odwołania przestrzennego systemu. Prawidłowy tylko w przypadku właściwości typów przestrzennych. Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Nie          | **Wartość true,** lub **False** w zależności od tego, czy przechowywana wartość właściwości jako ciąg Unicode.                                                                                                                               |
| **Sortowanie**                                                      | Nie          | Ciąg, który określa kolejność sortowania, które ma być używany w źródle danych.                                                                                                                                                   |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **CollectionType** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano funkcji definiowanych przez model, który używa **TypeRef** — element (jako element podrzędny elementu **CollectionType** elementu) do określenia, że funkcja ta akceptuje zbiór  **Dział** typów jednostek.

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="using-element-csdl"></a>Za pomocą elementu (CSDL)

**Using** element język definicji schematu koncepcyjnego (CSDL) importuje zawartość modelu koncepcyjnego, który znajduje się w innej przestrzeni nazw. Ustawiając wartość **Namespace** atrybut, można się odwoływać do typów jednostek, typy złożone i typy skojarzenia, które są zdefiniowane w modelu koncepcyjnym innego. Więcej niż jeden **Using** element może być elementem podrzędnym **schematu** elementu.

> [!NOTE]
> **Using** element CSDL nie działa dokładnie tak jak **przy użyciu** instrukcji w języku programowania. Importując przestrzeń nazw z **przy użyciu** instrukcji w języku programowania, możesz nie wpływają na obiekty w oryginalnym przestrzeni nazw. W CSDL importowanych przestrzeni nazw może zawierać typ jednostki, który pochodzi od typu jednostki w oryginalnym przestrzeni nazw. Może to wpłynąć na zestawy jednostek, zadeklarowany w oryginalnym przestrzeni nazw.

 

**Using** element może mieć następujące elementy podrzędne:

-   Dokumentacja (zero lub jeden elementy dozwolone)
-   Elementów adnotacji (zero lub więcej elementów dozwolone)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty mogą być stosowane do **Using** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Tak         | Nazwa importowanych przestrzeni nazw.                                                                                                                                                |
| **Alias**      | Tak         | Identyfikator używany zamiast nazwy przestrzeni nazw. Mimo że ten atrybut jest wymagany, nie jest wymagane, aby z niego korzystać zamiast nazwy przestrzeni nazw kwalifikowania nazwy obiektów. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **Using** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

 

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano **Using** elementu używane do importowania przestrzeni nazw, która jest definiowane w innym miejscu. Należy pamiętać, że przestrzeń nazw dla **schematu** elementu wyświetlana jest `BooksModel`. `Address` Właściwość `Publisher` **EntityType** to typ złożony, który jest zdefiniowany w `ExtendedBooksModel` przestrzeni nazw (zaimportowane wraz z **Using** elementu).

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
           Namespace="BooksModel" Alias="Self">

     <Using Namespace="BooksModel.Extended" Alias="BMExt" />

 <EntityContainer Name="BooksContainer" >
       <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
     </EntityContainer>

 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BMExt.Address" Name="Address" Nullable="false" />
     </EntityType>

 </Schema>
```
 

 

## <a name="annotation-attributes-csdl"></a>Atrybuty adnotacji (CSDL)

Atrybuty adnotacji w język definicji schematu koncepcyjnego (CSDL) są niestandardowe atrybuty XML w modelu koncepcyjnym. Oprócz prawidłowe struktury XML, wymaga spełnienia następujących warunków adnotacji atrybutów:

-   Atrybuty adnotacji nie może być w przestrzeni nazw XML, który jest zarezerwowany dla CSDL.
-   Więcej niż jeden atrybut adnotacji można stosować do danego elementu CSDL.
-   W pełni kwalifikowanej nazwy wszelkie atrybuty dwóch adnotacji nie może być taka sama.

Atrybuty adnotacji może służyć do zapewnienia dodatkowe metadane na temat elementów w modelu koncepcyjnym. W czasie wykonywania przy użyciu klas w przestrzeni nazw System.Data.Metadata.Edm możliwy jest metadanych elementów adnotacji.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EntityType** element z atrybutem adnotacji (**CustomAttribute —**). W przykładzie pokazano również element adnotacji stosowane do elementu typu jednostki.

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

Poniższy kod służy do pobierania metadanych w atrybucie adnotacji i zapisuje go do konsoli:

``` xml
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomAttribute"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomAttribute"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

Powyższy kod zakłada, że `School.csdl` plik znajduje się w katalogu wyjściowego projektu i dodano następujące `Imports` i `Using` instrukcje do projektu:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a>Elementów adnotacji (CSDL)

Elementy adnotacji w język definicji schematu koncepcyjnego (CSDL) są niestandardowe elementy XML w modelu koncepcyjnym. Oprócz prawidłowe struktury XML, wymaga spełnienia następujących warunków elementów adnotacji:

-   Elementów adnotacji nie może być w przestrzeni nazw XML, który jest zarezerwowany dla CSDL.
-   Więcej niż jeden element adnotacji może być elementem podrzędnym danego elementu CSDL.
-   W pełni kwalifikowanej nazwy dowolne elementy dwóch adnotacji nie może być taka sama.
-   Adnotacja elementów musi występować po wszystkich innych elementów podrzędnych danego elementu CSDL.

Elementów adnotacji może służyć do zapewnienia dodatkowe metadane na temat elementów w modelu koncepcyjnym. Począwszy od programu .NET Framework w wersji 4, metadanych elementów adnotacji są dostępne w czasie wykonywania przy użyciu klas w przestrzeni nazw System.Data.Metadata.Edm.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EntityType** element z element adnotacji (**CustomElement**). Przykład pokazują również adnotacji zastosowany do elementu typu jednostki.

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

Poniższy kod służy do pobierania metadanych w elemencie adnotacji i zapisuje go do konsoli:

``` csharp
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomElement"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomElement"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

Powyższy kod założono, że plik School.csdl znajduje się w katalogu wyjściowego projektu, i dodano następujące `Imports` i `Using` instrukcje do projektu:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a>Model koncepcyjny typów (CSDL)

Język definicji schematu koncepcyjnego (CSDL) obsługuje zestaw abstrakcyjne pierwotne typy danych, nazywane **EDMSimpleTypes**, który definiują właściwości w modelu koncepcyjnym. **EDMSimpleTypes** serwerów proxy dla typów danych pierwotnych, które są obsługiwane w środowisku hostingu lub magazynu.

Poniższa tabela zawiera listę typów danych pierwotnych, które są obsługiwane przez CSDL. W tabeli przedstawiono listę zestawów reguł, które mogą być stosowane do każdego **EDMSimpleType**.

| EDMSimpleType                    | Opis                                                | Zastosowanie zestawów reguł                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **Edm.Binary**                   | Zawiera dane binarne.                                      | Element MaxLength, wartości null, domyślne                                |
| **Typem Edm.Boolean**                  | Zawiera wartość **true** lub **false**.                  | Wartość null, domyślne                                                        |
| **Edm.Byte**                     | Zawiera wartość Liczba całkowita bez znaku 8-bitowych.                  | Precyzja dopuszczającego wartość null, domyślny                                             |
| **Edm.DateTime**                 | Reprezentuje datę i godzinę.                                | Precyzja dopuszczającego wartość null, domyślny                                             |
| **Edm.DateTimeOffset**           | Zawiera datę i godzinę w ciągu kilku minut od GMT przesunięcia. | Precyzja dopuszczającego wartość null, domyślny                                             |
| **Edm.Decimal**                  | Zawiera wartość liczbową ze stałym dokładności i skali.   | Precyzja dopuszczającego wartość null, domyślny                                             |
| **Edm.Double**                   | Zawiera zmiennoprzecinkowa numer z dokładnością do 15 cyfr   | Precyzja dopuszczającego wartość null, domyślny                                             |
| **Edm.Float**                    | Zawiera zmiennoprzecinkowej liczba z 7-cyfrowy dokładnością.   | Precyzja dopuszczającego wartość null, domyślny                                             |
| **Edm.Guid**                     | Zawiera unikatowy identyfikator 16-bajtowy.                      | Precyzja dopuszczającego wartość null, domyślny                                             |
| **Edm.Int16**                    | Zawiera wartość liczby całkowitej ze znakiem 16-bitowych.                    | Precyzja dopuszczającego wartość null, domyślny                                             |
| **Typem Edm.Int32**                    | Zawiera wartość całkowita 32-bitowa.                    | Precyzja dopuszczającego wartość null, domyślny                                             |
| **Edm.Int64**                    | Zawiera wartość całkowita 64-bitowa.                    | Precyzja dopuszczającego wartość null, domyślny                                             |
| **Edm.SByte**                    | Zawiera wartość całkowita 8-bitowa.                     | Precyzja dopuszczającego wartość null, domyślny                                             |
| **Edm.String**                   | Zawiera dane znaków.                                   | Unicode dla wpisu, MaxLength, sortowanie, dokładności, dopuszczającego wartość null, domyślne |
| **Edm.Time**                     | Zawiera porze dnia.                                    | Precyzja dopuszczającego wartość null, domyślny                                             |
| **Edm.Geography**                |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.GeographyPoint**           |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.GeographyLineString**      |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.GeographyPolygon**         |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.GeographyMultiPoint**      |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.GeographyMultiLineString** |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.GeographyMultiPolygon**    |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.GeographyCollection**      |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.Geometry**                 |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.GeometryPoint**            |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.GeometryLineString**       |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.GeometryPolygon**          |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.GeometryMultiPoint**       |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.GeometryMultiLineString**  |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.GeometryMultiPolygon**     |                                                            | Wartość null, domyślnie, SRID                                                  |
| **Edm.GeometryCollection**       |                                                            | Wartość null, domyślnie, SRID                                                  |

## <a name="facets-csdl"></a>Zestawy reguł (CSDL)

Aspekty w język definicji schematu koncepcyjnego (CSDL) reprezentują ograniczenia dotyczące właściwości typów jednostek i typów złożonych. Zestawy reguł są wyświetlane jako atrybuty XML w następujących elementów CSDL:

-   Właściwość
-   TypeRef
-   Parametr

W poniższej tabeli opisano aspekty, które są obsługiwane przez CSDL. Wszystkie zestawy reguł są opcjonalne. Niektóre aspekty wymienione poniżej są używane przez program Entity Framework, podczas generowania bazy danych na podstawie modelu koncepcyjnego.

> [!NOTE]
> Informacje o typach danych w modelu koncepcyjnym na ten temat można znaleźć w koncepcyjny modelu typy (CSDL).

| zestaw reguł               | Opis                                                                                                                                                                                                                                                   | Informacje zawarte w tym artykule dotyczą                                                                                                                                                                                                                                                                                                                                                                           | Użyto do generowania bazy danych | Używane przez środowisko uruchomieniowe |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| **Sortowanie**       | Określa kolejność sortowania (lub sekwencji sortowania) do użycia podczas przeprowadzania porównania i kolejność operacji na wartościach właściwości.                                                                                                               | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | Tak                              | Nie                  |
| **Właściwość ConcurrencyMode** | Wskazuje, że wartość właściwości powinien być używany w celu pomyślnych kontroli współbieżności.                                                                                                                                                                    | Wszystkie **EDMSimpleType** właściwości                                                                                                                                                                                                                                                                                                                                                     | Nie                               | Tak                 |
| **Default**         | Określa domyślną wartość właściwości, jeśli nie dostarczono żadnej wartości dla wystąpienia.                                                                                                                                                                       | Wszystkie **EDMSimpleType** właściwości                                                                                                                                                                                                                                                                                                                                                     | Tak                              | Tak                 |
| **Wartości**     | Określa, czy długość wartości właściwości mogą się różnić.                                                                                                                                                                                                  | **Edm.Binary**, **Edm.String**                                                                                                                                                                                                                                                                                                                                                       | Tak                              | Nie                  |
| **Element maxLength**       | Określa maksymalną długość wartości właściwości.                                                                                                                                                                                                           | **Edm.Binary**, **Edm.String**                                                                                                                                                                                                                                                                                                                                                       | Tak                              | Nie                  |
| **Dopuszcza wartości null**        | Określa, czy właściwość może mieć **null** wartość.                                                                                                                                                                                                     | Wszystkie **EDMSimpleType** właściwości                                                                                                                                                                                                                                                                                                                                                     | Tak                              | Tak                 |
| **Precyzja**       | Dla właściwości typu **dziesiętna**, określa liczbę cyfr, może mieć wartości właściwości. Dla właściwości typu **czasu**, **daty/godziny**, i **DateTimeOffset**, określa liczbę cyfr ułamkowych części sekundy w wartości właściwości. | **Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**                                                                                                                                                                                                                                                                                                              | Tak                              | Nie                  |
| **Skala**           | Określa liczbę cyfr po prawej stronie przecinka dziesiętnego dla wartości właściwości.                                                                                                                                                                      | **Edm.Decimal**                                                                                                                                                                                                                                                                                                                                                                      | Tak                              | Nie                  |
| **SRID**            | Określa identyfikator System przestrzenne odwołanie do systemu. Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).                                                              | **Edm.Geography Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection** | Nie                               | Tak                 |
| **Unicode**         | Wskazuje, czy wartość właściwości jest przechowywana jako Unicode.                                                                                                                                                                                                    | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | Tak                              | Tak                 |

>[!NOTE]
> Podczas generowania bazę danych z modelu koncepcyjnego, Kreator bazy danych Generowanie rozpoznaje wartość **parametru StoreGeneratedPattern** atrybutu na **właściwość** elementu, jeśli znajduje się w następujących przestrzeń nazw: http://schemas.microsoft.com/ado/2009/02/edm/annotation. Obsługiwane wartości dla atrybutu to **tożsamości** i **obliczane**. Wartość **tożsamości** dadzą kolumny bazy danych przy użyciu wartości tożsamości, który jest generowany w bazie danych. Wartość **obliczane** dadzą kolumna z wartością, która jest kolumną obliczaną w bazie danych.

### <a name="example"></a>Przykład

Poniższy przykład przedstawia aspektami stosowany do właściwości typu jednostki:

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="http://schemas.microsoft.com/ado/2009/02/edm/annotation" />
   <Property Type="String"
             Name="ProductName"
             Nullable="false"
             MaxLength="50" />
   <Property Type="String"
             Name="Location"
             Nullable="true"
             MaxLength="25" />
 </EntityType>
```
