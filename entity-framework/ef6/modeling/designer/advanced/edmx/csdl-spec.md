---
title: Specyfikacja CSDL — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182596"
---
# <a name="csdl-specification"></a>Specyfikacja CSDL
Język definicji schematu koncepcyjnego (CSDL) to język oparty na języku XML, który opisuje jednostki, relacje i funkcje, które tworzą model koncepcyjny aplikacji opartej na danych. Ten model koncepcyjny może być używany przez Entity Framework lub Usługi danych programu WCF. Metadane, które są opisane w CSDL, są używane przez Entity Framework do mapowania jednostek i relacji, które są zdefiniowane w modelu koncepcyjnym ze źródłem danych. Aby uzyskać więcej informacji, zobacz [Specyfikacja SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) i [Specyfikacja MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).

CSDL jest implementacją Entity Framework Entity Data Model.

W aplikacji Entity Framework metadane modelu koncepcyjnego są ładowane z pliku. csdl (zapisanym w CSDL) do wystąpienia elementu System. Data. Metadata. Edm. EdmItemCollection i są dostępne przy użyciu metod w Klasa System. Data. Metadata. Edm. MetadataWorkspace. Entity Framework używa metadanych modelu koncepcyjnego do translacji zapytań względem modelu koncepcyjnego na polecenia specyficzne dla źródła danych.

Projektant EF przechowuje informacje o modelu koncepcyjnym w pliku. edmx w czasie projektowania. W czasie kompilacji program Dr Designer używa informacji w pliku. edmx, aby utworzyć plik. csdl, który jest wymagany przez Entity Framework w czasie wykonywania.

Wersje CSDL są odróżniane według przestrzeni nazw XML.

| Wersja CSDL | Przestrzeń nazw XML                                |
|:-------------|:---------------------------------------------|
| CSDL, wersja 1      | https://schemas.microsoft.com/ado/2006/04/edm |
| CSDL v2      | https://schemas.microsoft.com/ado/2008/09/edm |
| CSDL v3      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Element Association (CSDL)

Element **Association** definiuje relację między dwoma typami jednostek. Skojarzenie musi określać typy jednostek, które są uwzględnione w relacji, oraz liczbę typów jednostek na każdym końcu relacji, która jest znana jako liczebność. Liczebność elementu end skojarzenia może mieć wartość jeden (1), zero lub jeden (0.. 1) lub wiele (\*). Te informacje są określone w dwóch podrzędnych elementach end.

Do wystąpień typu jednostki na jednym końcu skojarzenia można uzyskać dostęp za poorednictwem właściwości nawigacji lub kluczy obcych, jeśli są one uwidocznione w typie jednostki.

W aplikacji wystąpienie skojarzenia reprezentuje określone skojarzenie między wystąpieniami typów jednostek. Wystąpienia skojarzeń są logicznie pogrupowane w zestawie skojarzenia.

Element **Association** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   End (dokładnie dwa elementy)
-   ReferentialConstraint (zero lub jeden element)
-   Elementy adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Association** .

| Nazwa atrybutu | Jest wymagana | Value                        |
|:---------------|:------------|:-----------------------------|
| **Nazwa**       | Tak         | Nazwa skojarzenia. |

 

> [!NOTE]
> Do elementu **Association** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **skojarzenia** , który definiuje skojarzenie **CustomerOrders** , gdy klucze obce nie zostały ujawnione w typach jednostek **klienta** i **zamówienia** . Wartości **liczebności** dla każdego **końca** skojarzenia wskazują, że wiele **zamówień** może być skojarzonych z **klientem**, ale tylko jeden **Klient** może być skojarzony z **kolejnością**. Ponadto element **onDelete** wskazuje, że wszystkie **zamówienia** , które są powiązane z konkretnym **klientem** i zostały załadowane do obiektu ObjectContext, zostaną usunięte, jeśli **Klient** zostanie usunięty.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

Poniższy przykład pokazuje element **skojarzenia** , który definiuje skojarzenie **CustomerOrders** , gdy klucze obce zostały ujawnione w typach jednostek **klienta** i **zamówienia** . W przypadku wyeksponowanych kluczy obcych relacja między jednostkami jest zarządzana za pomocą elementu **ReferentialConstraint** . Odpowiedni element AssociationSetMapping nie jest konieczny do mapowania tego skojarzenia ze źródłem danych.

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
 

 

## <a name="associationset-element-csdl"></a>AssociationSet, element (CSDL)

Element **AssociationSet** w języku definicji schematu koncepcyjnego (CSDL) to logiczny kontener dla wystąpień skojarzeń tego samego typu. Zestaw skojarzeń zawiera definicję grupowania wystąpień skojarzeń, aby można je było mapować do źródła danych.  

Element **AssociationSet** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (dozwolone zero lub jeden element)
-   End (dokładnie wymagane dwa elementy)
-   Elementy adnotacji (dozwolone zero lub więcej elementów)

Atrybut **Association** określa typ skojarzenia, które zawiera zestaw skojarzeń. Zestawy jednostek, które tworzą końce zestawu skojarzeń, są określone z dokładnie dwoma podrzędnymi elementami **End** .

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **AssociationSet** .

| Nazwa atrybutu  | Jest wymagana | Value                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**        | Tak         | Nazwa zestawu jednostek. Wartość atrybutu **name** nie może być taka sama jak wartość atrybutu **skojarzenia** .                                 |
| **Skojarzenie** | Tak         | W pełni kwalifikowana nazwa skojarzenia, z którą zestaw skojarzeń zawiera wystąpienia. Skojarzenie musi znajdować się w tej samej przestrzeni nazw co zestaw skojarzenia. |

 

> [!NOTE]
> Do elementu **AssociationSet** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EntityContainer** z dwoma elementami **AssociationSet** :

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
 

 

## <a name="collectiontype-element-csdl"></a>CollectionType — element (CSDL)

Element **CollectionType** w języku definicji schematu koncepcyjnego (CSDL) określa, że parametr funkcji lub typ zwracany funkcji jest kolekcją. Element **CollectionType** może być elementem podrzędnym elementu Parameter lub ReturnType (Function). Typ kolekcji można określić przy użyciu atrybutu **typu** lub jednego z następujących elementów podrzędnych:

-   **CollectionType**
-   ReferenceType
-   RowType
-   TypeRef

> [!NOTE]
> Model nie zostanie zweryfikowany, jeśli typ kolekcji jest określony zarówno z atrybutem **typu** , jak i elementem podrzędnym.

 

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **CollectionType** . Należy zauważyć, że atrybuty **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**i **Collation** mają zastosowanie tylko do kolekcji **EDMSimpleTypes**.

| Nazwa atrybutu                                                          | Jest wymagana | Value                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**                                                                | Nie          | Typ kolekcji.                                                                                                                                                                                                      |
| **Wymaga**                                                            | Nie          | **Wartość true** (wartość domyślna) lub **Fałsz** w zależności od tego, czy właściwość może mieć wartość null. <br/> [!NOTE]                                                                                                                 |
| > W CSDL V1, właściwość typu złożonego musi mieć `Nullable="False"`. |             |                                                                                                                                                                                                                                  |
| **DefaultValue**                                                        | Nie          | Wartość domyślna właściwości.                                                                                                                                                                                               |
| **MaxLength**                                                           | Nie          | Maksymalna długość wartości właściwości.                                                                                                                                                                                        |
| **FixedLength**                                                         | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg o stałej długości.                                                                                                                           |
| **Dokładne**                                                           | Nie          | Precyzja wartości właściwości.                                                                                                                                                                                             |
| **Zasięgu**                                                               | Nie          | Skala wartości właściwości.                                                                                                                                                                                                 |
| **SRID**                                                                | Nie          | Identyfikator odwołania do systemu przestrzennego. Prawidłowe tylko dla właściwości typów przestrzennych.   Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) |
| **Unicode**                                                             | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg Unicode.                                                                                                                                |
| **Sortowanie**                                                           | Nie          | Ciąg określający sekwencję sortowania, która ma być używana w źródle danych.                                                                                                                                                    |

 

> [!NOTE]
> Do elementu **CollectionType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład pokazuje funkcję zdefiniowaną przez model, która używa elementu **CollectionType** , aby określić, że funkcja zwraca kolekcję typów jednostek **osób** (jak określono w atrybucie **ElementType** ).

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
 

Poniższy przykład pokazuje funkcję zdefiniowaną przez model, która używa elementu **CollectionType** , aby określić, że funkcja zwraca kolekcję wierszy (jak określono w elemencie **RowType** ).

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
 

Poniższy przykład pokazuje funkcję zdefiniowaną przez model, która używa elementu **CollectionType** , aby określić, że funkcja akceptuje jako parametr Kolekcja typów jednostek **działu** .

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
 

 

## <a name="complextype-element-csdl"></a>ComplexType — element (CSDL)

Element **complexType** definiuje strukturę danych składającą się z właściwości **EdmSimpleType** lub innych typów złożonych.  Typ złożony może być właściwością typu jednostki lub innego typu złożonego. Typ złożony jest podobny do typu jednostki, w którym typ złożony definiuje dane. Istnieją jednak pewne kluczowe różnice między typami złożonymi i typami jednostek:

-   Typy złożone nie mają tożsamości (lub kluczy) i dlatego nie mogą występować niezależnie. Typy złożone mogą istnieć tylko jako właściwości typów jednostek lub innych typów złożonych.
-   Typy złożone nie mogą uczestniczyć w skojarzeniach. Żadne zakończenie skojarzenia nie może być typu złożonego, dlatego właściwości nawigacji nie mogą być zdefiniowane dla typów złożonych.
-   Właściwość typu złożonego nie może mieć wartości null, ale właściwości skalarne typu złożonego mogą być ustawione na wartość null.

Element **complexType** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Właściwość (zero lub więcej elementów)
-   Elementy adnotacji (zero lub więcej elementów)

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **complexType** .

| Nazwa atrybutu                                                                                                 | Jest wymagana | Value                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name                                                                                                           | Tak         | Nazwa typu złożonego. Nazwa typu złożonego nie może być taka sama jak nazwa innego typu złożonego, typu jednostki lub skojarzenia znajdującego się w zakresie modelu. |
| BaseType                                                                                                       | Nie          | Nazwa innego typu złożonego, który jest typem podstawowym typu złożonego, który jest definiowany. <br/> [!NOTE]                                                                     |
| > Ten atrybut nie ma zastosowania w CSDL v1. Dziedziczenie dla typów złożonych nie jest obsługiwane w tej wersji. |             |                                                                                                                                                                                     |
| Abstrakcyjny                                                                                                       | Nie          | **Wartość true** lub **false** (wartość domyślna) w zależności od tego, czy typ złożony jest typem abstrakcyjnym. <br/> [!NOTE]                                                                  |
| > Ten atrybut nie ma zastosowania w CSDL v1. Typy złożone w tej wersji nie mogą być typami abstrakcyjnymi.         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> Do elementu **complexType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono typ złożony, **adres**, z właściwościami **EdmSimpleType** **StreetAddress**, **miasto**, **StateOrProvince**, **Country**i **KodPocztowy**.

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

Aby zdefiniować **adres** typu złożonego (powyżej) jako właściwość typu jednostki, należy zadeklarować typ właściwości w definicji typu jednostki. Poniższy przykład przedstawia Właściwość **Address** jako typ złożony w typie podmiotu (**Wydawca**):

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
 

 

## <a name="definingexpression-element-csdl"></a>DefiningExpression, element (CSDL)

Element **DefiningExpression** w języku definicji schematu koncepcyjnego (CSDL) zawiera wyrażenie Entity SQL, które definiuje funkcję w modelu koncepcyjnym.  

> [!NOTE]
> W celu sprawdzenia poprawności element **DefiningExpression** może zawierać dowolną zawartość. Jednak Entity Framework zgłosi wyjątek w czasie wykonywania, jeśli element **DefiningExpression** nie zawiera prawidłowych Entity SQL.

 

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Do elementu **DefiningExpression** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

W poniższym przykładzie użyto elementu **DefiningExpression** , aby zdefiniować funkcję, która zwraca liczbę lat od momentu opublikowania książki. Zawartość elementu **DefiningExpression** jest zapisywana w Entity SQL.

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Element zależny (CSDL)

Element **zależny** w języku definicji schematu koncepcyjnego (CSDL) jest elementem podrzędnym elementu ReferentialConstraint i definiuje zależne zakończenie ograniczenia referencyjnego. Element **ReferentialConstraint** definiuje funkcje podobne do ograniczenia integralności referencyjnej w relacyjnej bazie danych. W taki sam sposób, w jaki kolumna (lub kolumny) z tabeli bazy danych może odwoływać się do klucza podstawowego innej tabeli, właściwość (lub właściwości) typu jednostki może odwoływać się do klucza jednostki innego typu jednostki. Typ obiektu, do którego istnieje odwołanie, nazywa się *głównym końcem* ograniczenia. Typ jednostki, który odwołuje się do głównego końca, nazywa się *końcem zależnym* od ograniczenia. Elementy **PropertyRef** są używane do określania, które klucze odwołują się do głównego celu.

Element **zależny** może mieć następujące elementy podrzędne (w podanej kolejności):

-   PropertyRef (co najmniej jeden element)
-   Elementy adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **zależnego** .

| Nazwa atrybutu | Jest wymagana | Value                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Rola**       | Tak         | Nazwa typu jednostki na elemencie zależnym końca skojarzenia. |

 

> [!NOTE]
> Do elementu **zależnego** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład przedstawia element **ReferentialConstraint** , który jest używany jako część definicji skojarzenia **PublishedBy** . Właściwość **publisherID** typu jednostki **książki** tworzy zależne zakończenie ograniczenia referencyjnego.

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
 

 

## <a name="documentation-element-csdl"></a>Element dokumentacji (CSDL)

Element **dokumentacji** w języku definicji schematu koncepcyjnego (CSDL) może służyć do dostarczania informacji dotyczących obiektu, który jest zdefiniowany w elemencie nadrzędnym. W pliku. edmx, gdy element **dokumentacji** jest elementem podrzędnym elementu, który pojawia się jako obiekt na powierzchni projektowej programu EF Designer (na przykład jednostki, skojarzenia lub właściwości), zawartość elementu **dokumentacji** pojawi się w Okno **Właściwości** programu Visual Studio dla obiektu.

Element **dokumentacji** może zawierać następujące elementy podrzędne (w podanej kolejności):

-   **Podsumowanie**: Krótki opis elementu nadrzędnego. (zero lub jeden element)
-   **LongDescription**: Obszerny opis elementu nadrzędnego. (zero lub jeden element)
-   Elementy adnotacji. (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Do elementu **dokumentacji** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **dokumentacji** jako element podrzędny elementu EntityType. Jeśli Poniższy fragment kodu znajduje się w zawartości CSDL pliku. edmx, zawartość elementów **podsumowania** i **longDescription** pojawi się w oknie **Właściwości** programu Visual Studio po kliknięciu typu jednostki `Customer`.

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
 

 

## <a name="end-element-csdl"></a>End — element (CSDL)

Element **End** w języku definicji schematu koncepcyjnego (CSDL) może być elementem podrzędnym elementu Association lub AssociationSet elementu. W każdym przypadku rola elementu **końcowego** jest inna, a odpowiednie atrybuty są różne.

### <a name="end-element-as-a-child-of-the-association-element"></a>End — element jako element podrzędny elementu Association

Element **End** (jako element podrzędny elementu **Association** ) identyfikuje typ jednostki na jednym końcu skojarzenia i liczbę wystąpień typu jednostki, które mogą istnieć na tym końcu skojarzenia. Punkty końcowe skojarzenia są zdefiniowane jako część skojarzenia; skojarzenie musi mieć dokładnie dwa punkty końcowe skojarzenia. Do wystąpienia typu jednostki na jednym końcu skojarzenia można uzyskać dostęp za poorednictwem właściwości nawigacji lub kluczy obcych, jeśli są one uwidocznione w typie jednostki.  

Element **końcowy** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   OnDelete (zero lub jeden element)
-   Elementy adnotacji (zero lub więcej elementów)

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do elementu **końcowego** , gdy jest elementem podrzędnym elementu **skojarzenia** .

| Nazwa atrybutu   | Jest wymagana | Value                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**         | Tak         | Nazwa typu jednostki na jednym końcu skojarzenia.                                                                                                                                                                                                                                                                                                                                                         |
| **Rola**         | Nie          | Nazwa punktu końcowego skojarzenia. Jeśli nie podano nazwy, zostanie użyta nazwa typu jednostki na końcu skojarzenia.                                                                                                                                                                                                                                                                                           |
| **Liczebność** | Tak         | **1**, **0.. 1**lub **\*** w zależności od liczby wystąpień typu jednostki, które mogą znajdować się na końcu skojarzenia. <br/> **1** wskazuje, że dokładnie jedno wystąpienie typu jednostki istnieje na końcu skojarzenia. <br/> **0.. 1** oznacza, że na końcu skojarzenia istnieją wystąpienia typu jednostki, które są równe zero lub jeden. <br/> **\*** oznacza, że na końcu skojarzenia istnieje zero, jedno lub więcej wystąpień typu jednostki. |

 

> [!NOTE]
> Do elementu **End** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

#### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **skojarzenia** , który definiuje skojarzenie **CustomerOrders** . Wartości **liczebności** dla każdego **końca** skojarzenia wskazują, że wiele **zamówień** może być skojarzonych z **klientem**, ale tylko jeden **Klient** może być skojarzony z **kolejnością**. Ponadto element **onDelete** wskazuje, że wszystkie **zamówienia** , które są powiązane z konkretnym **klientem** i które zostały załadowane do obiektu ObjectContext, zostaną usunięte, jeśli **Klient** zostanie usunięty.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>End — element jako element podrzędny elementu AssociationSet

Element **końcowy** określa jeden koniec zestawu skojarzenia. Element **AssociationSet** musi zawierać dwa elementy **End** . Informacje zawarte w elemencie **End** są używane w mapowaniu zestawu skojarzenia ze źródłem danych.

Element **końcowy** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Elementy adnotacji (zero lub więcej elementów)

> [!NOTE]
> Elementy adnotacji muszą występować po wszystkich innych elementach podrzędnych. Elementy adnotacji są dozwolone tylko w obszarze CSDL v2 i nowszych.

 

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do elementu **końcowego** , gdy jest elementem podrzędnym elementu **AssociationSet** .

| Nazwa atrybutu | Jest wymagana | Value                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Elementy**  | Tak         | Nazwa elementu **EntitySet** , który definiuje jeden koniec elementu nadrzędnego obiektu **AssociationSet** . Element **EntitySet** musi być zdefiniowany w tym samym kontenerze jednostki co element nadrzędny **AssociationSet** . |
| **Rola**       | Nie          | Nazwa punktu końcowego zestawu skojarzenia. Jeśli atrybut **roli** nie jest używany, nazwa punktu końcowego zestawu skojarzenia będzie nazwą zestawu jednostek.                                                                   |

 

> [!NOTE]
> Do elementu **End** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

#### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EntityContainer** z dwoma elementami **AssociationSet** , z których każdy ma dwa elementy **End** :

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
 

 

## <a name="entitycontainer-element-csdl"></a>EntityContainer, element (CSDL)

Element **EntityContainer** w języku definicji schematu koncepcyjnego (CSDL) to logiczny kontener dla zestawów jednostek, zestawów skojarzeń i importów funkcji. Kontener jednostek modelu koncepcyjnego mapuje do kontenera jednostek modelu magazynu za pomocą elementu EntityContainerMapping. Kontener jednostek modelu magazynu opisuje strukturę bazy danych: zestawy jednostek opisują tabele, zestawy skojarzeń opisują ograniczenia FOREIGN KEY i importów funkcji opisują procedury składowane w bazie danych.

Element **EntityContainer** może mieć zero lub jeden element dokumentacji. Jeśli element **dokumentacji** jest obecny, musi poprzedzać wszystkie elementy **EntitySet**, **AssociationSet**i **FunctionImport** .

Element **EntityContainer** może mieć zero lub więcej następujących elementów podrzędnych (w podanej kolejności):

-   Elementy
-   AssociationSet
-   Element FunctionImport
-   Elementy adnotacji

Można rozwinąć element **EntityContainer** , aby dołączyć zawartość innego **obiektu EntityContainer** znajdującego się w tej samej przestrzeni nazw. Aby dołączyć zawartość innego **obiektu EntityContainer**, w elemencie odwołującego się do **obiektu EntityContainer** ustaw wartość atrybutu **extends** na nazwę elementu **EntityContainer** , który ma zostać uwzględniony. Wszystkie elementy podrzędne dołączonego elementu **EntityContainer** będą traktowane jako elementy podrzędne elementu **EntityContainer** , do którego się odwołuje.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **using** .

| Nazwa atrybutu | Jest wymagana | Value                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa kontenera jednostek.                               |
| **Poszerza**    | Nie          | Nazwa innego kontenera jednostek w tej samej przestrzeni nazw. |

 

> [!NOTE]
> Do elementu **EntityContainer** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EntityContainer** , który definiuje trzy zestawy jednostek i dwa zestawy kojarzenia.

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

Element **EntitySet** w języku definicji schematu koncepcyjnego jest kontenerem logicznym dla wystąpień typu jednostki i wystąpień dowolnego typu pochodzącego od tego typu jednostki. Relacja między typem jednostki a zestawem jednostek jest analogiczna do relacji między wierszem a tabelą w relacyjnej bazie danych. Podobnie jak w przypadku wiersza, typ jednostki definiuje zestaw powiązanych danych i, podobnie jak tabela, zestaw jednostek zawiera wystąpienia tej definicji. Zestaw jednostek zawiera konstrukcję do grupowania wystąpień typu jednostki, aby można je było zamapować na powiązane struktury danych w źródle danych.  

Można zdefiniować więcej niż jeden zestaw jednostek dla określonego typu jednostki.

> [!NOTE]
> Program Dr Designer nie obsługuje modeli koncepcyjnych, które zawierają wiele zestawów jednostek na typ.

 

Element **EntitySet** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Element dokumentacji (dozwolone zero lub jeden element)
-   Elementy adnotacji (dozwolone zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EntitySet** .

| Nazwa atrybutu | Jest wymagana | Value                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa zestawu jednostek.                                                              |
| **Elementy** | Tak         | W pełni kwalifikowana nazwa typu jednostki, dla którego zestaw jednostek zawiera wystąpienia. |

 

> [!NOTE]
> Do elementu **EntitySet** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EntityContainer** z trzema elementami **EntitySet** :

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
 

Istnieje możliwość zdefiniowania wielu zestawów jednostek na typ (MEST). W poniższym przykładzie zdefiniowano kontener jednostek z dwoma zestawami jednostek dla typu jednostki **książki** :

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
 

 

## <a name="entitytype-element-csdl"></a>EntityType — element (CSDL)

Element **EntityType** reprezentuje strukturę koncepcji najwyższego poziomu, taką jak klient lub zamówienie, w modelu koncepcyjnym. Typ jednostki to szablon dla wystąpień typów jednostek w aplikacji. Każdy szablon zawiera następujące informacje:

-   Unikatowa nazwa. (Wymagane).
-   Klucz jednostki, który jest zdefiniowany przez jedną lub więcej właściwości. (Wymagane).
-   Właściwości zawierające dane. (Opcjonalnie).
-   Właściwości nawigacji, które umożliwiają nawigację z jednego końca skojarzenia z drugim. (Opcjonalnie).

W aplikacji wystąpienie typu jednostki reprezentuje określony obiekt (na przykład konkretny klient lub zamówienie). Każde wystąpienie typu jednostki musi mieć unikatowy klucz jednostki w ramach zestawu jednostek.

Dwa wystąpienia typu jednostki są uważane za równe tylko wtedy, gdy są one tego samego typu, a wartości ich kluczy jednostek są takie same.

Element **EntityType** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Klucz (zero lub jeden element)
-   Właściwość (zero lub więcej elementów)
-   Element NavigationProperty (zero lub więcej)
-   Elementy adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EntityType** .

| Nazwa atrybutu                                                                                                                                  | Jest wymagana | Value                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Nazwa**                                                                                                                                        | Tak         | Nazwa typu jednostki.                                                                     |
| **BaseType**                                                                                                                                    | Nie          | Nazwa innego typu jednostki, który jest typem podstawowym typu jednostki, który jest definiowany.  |
| **Streszczeń**                                                                                                                                    | Nie          | **Wartość true** lub **false**, w zależności od tego, czy typ jednostki jest typem abstrakcyjnym.                 |
| **OpenType**                                                                                                                                    | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy typem jednostki jest otwarty typ jednostki. <br/> [!NOTE] |
| > Atrybut **OpenType** ma zastosowanie tylko do typów jednostek, które są zdefiniowane w modelach koncepcyjnych, które są używane z ADO.NET Data Services. |             |                                                                                                  |

 

> [!NOTE]
> Do elementu **EntityType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EntityType** z trzema elementami **Właściwości** i dwoma elementami **NavigationProperty** :

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
 

 

## <a name="enumtype-element-csdl"></a>EnumType, element (CSDL)

Element **EnumType** reprezentuje typ wyliczeniowy.

Element **EnumType** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Członek (zero lub więcej elementów)
-   Elementy adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EnumType** .

| Nazwa atrybutu     | Jest wymagana | Value                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**           | Tak         | Nazwa typu jednostki.                                                                                                                                                                  |
| **IsFlags**        | Nie          | **Wartość true** lub **false**, w zależności od tego, czy typ wyliczeniowy może być używany jako zestaw flag. Wartość domyślna to **false.** .                                                                     |
| **Podstawowytype** | Nie          | **EDM. Byte**, **EDM. Int16**, **EDM. Int32**, **EDM. Int64** lub **EDM.** bajty definiujące zakres wartości typu.   Domyślny typ podstawowy elementów wyliczenia to **EDM. Int32.** . |

 

> [!NOTE]
> Do elementu **EnumType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EnumType** z trzema elementami **składowymi** :

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Element Function (CSDL)

Element **Function** w języku definicji schematu koncepcyjnego (CSDL) służy do definiowania lub deklarowania funkcji w modelu koncepcyjnym. Funkcja jest definiowana przy użyciu elementu DefiningExpression.  

Element **Function** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Parametr (zero lub więcej elementów)
-   DefiningExpression (zero lub jeden element)
-   ReturnValue (funkcja) (zero lub jeden element)
-   Elementy adnotacji (zero lub więcej elementów)

Typ zwracany dla funkcji musi być określony za pomocą elementu **ReturnType** (Function) lub atrybutu **ReturnType** (patrz poniżej), ale nie obu. Możliwe typy zwracane to dowolne EdmSimpleType, typ jednostki, typ złożony, typ wiersza lub typ ref (lub kolekcja jednego z tych typów).

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Function** .

| Nazwa atrybutu | Jest wymagana | Value                              |
|:---------------|:------------|:-----------------------------------|
| **Nazwa**       | Tak         | Nazwa funkcji.          |
| **Atrybuty** | Nie          | Typ zwracany przez funkcję. |

 

> [!NOTE]
> Do elementu **Function** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

W poniższym przykładzie użyto elementu **Function** do zdefiniowania funkcji, która zwraca liczbę lat od czasu zatrudnienia instruktora.

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>FunctionImport, element (CSDL)

Element **FunctionImport** w języku definicji schematu koncepcyjnego (CSDL) reprezentuje funkcję, która jest zdefiniowana w źródle danych, ale jest dostępna dla obiektów w modelu koncepcyjnym. Na przykład element Function w modelu magazynu może służyć do reprezentowania procedury składowanej w bazie danych. Element **FunctionImport** w modelu koncepcyjnym reprezentuje odpowiednią funkcję w aplikacji Entity Framework i jest mapowany na funkcję modelu magazynu za pomocą elementu FunctionImportMapping. Gdy funkcja jest wywoływana w aplikacji, odpowiednia procedura składowana jest wykonywana w bazie danych.

Element **FunctionImport** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (dozwolone zero lub jeden element)
-   Parametr (co najmniej jeden element jest dozwolony)
-   Elementy adnotacji (dozwolone zero lub więcej elementów)
-   ReturnType (FunctionImport) (dozwolone zero lub więcej elementów)

Dla każdego parametru akceptowanego przez funkcję należy zdefiniować jeden element **parametru** .

Typ zwracany dla funkcji musi być określony za pomocą elementu **ReturnType** (FunctionImport) lub atrybutu **ReturnType** (patrz poniżej), ale nie obu. Wartość zwracanego typu musi być kolekcją EdmSimpleType, EntityType lub ComplexType.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **FunctionImport** .

| Nazwa atrybutu   | Jest wymagana | Value                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**         | Tak         | Nazwa zaimportowanej funkcji.                                                                                                                                                                    |
| **Atrybuty**   | Nie          | Typ zwracany przez funkcję. Nie używaj tego atrybutu, jeśli funkcja nie zwraca wartości. W przeciwnym razie wartość musi być kolekcją ComplexType, EntityType lub EDMSimpleType.        |
| **Elementy**    | Nie          | Jeśli funkcja zwraca kolekcję typów jednostek, wartość **obiektu EntitySet** musi być zestawem jednostek, do którego należy kolekcja. W przeciwnym razie atrybut **EntitySet** nie może być używany. |
| **IsComposable** | Nie          | Jeśli wartość jest równa true, funkcja jest możliwa do przetworzenia (funkcja zwracająca tabelę) i może być użyta w zapytaniu LINQ.  Wartość domyślna to **false**.                                                           |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowych atrybutów XML) może zostać zastosowana do elementu **FunctionImport** . Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **FunctionImport** , który akceptuje jeden parametr i zwraca kolekcję typów jednostek:

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Key — element (CSDL)

Element **Key** jest elementem podrzędnym elementu EntityType i definiuje *klucz jednostki* (właściwość lub zestaw właściwości typu jednostki, który określa tożsamość). Właściwości tworzące klucz jednostki są wybierane w czasie projektowania. Wartości właściwości klucza jednostki muszą jednoznacznie identyfikować wystąpienie typu jednostki w ramach zestawu jednostek w czasie wykonywania. Właściwości tworzące klucz jednostki powinny być wybierane w celu zagwarantowania unikatowej wartości wystąpień w zestawie jednostek. Element **Key** definiuje klucz jednostki przez odwołanie do co najmniej jednej właściwości typu jednostki.

Element **Key** może mieć następujące elementy podrzędne:

-   PropertyRef (co najmniej jeden element)
-   Elementy adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Do elementu **Key** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

W poniższym przykładzie zdefiniowano typ jednostki o nazwie **książka**. Klucz jednostki jest definiowany przez odwołanie się do właściwości **ISBN** typu jednostki.

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
 

Właściwość **ISBN** jest dobrym wyborem dla klucza jednostki, ponieważ międzynarodowy numer książki standardowej (ISBN) jednoznacznie identyfikuje książkę.

W poniższym przykładzie przedstawiono typ jednostki (**autor**), który ma klucz jednostki składający się z dwóch właściwości: **Nazwa** i **adres**.

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
 

Użycie **nazwy** i **adresu** dla klucza jednostki jest uzasadnionym wyborem, ponieważ dwa autorów o tej samej nazwie jest mało prawdopodobne w tym samym adresie. Jednak wybór dla klucza jednostki nie gwarantuje jednowzględnie unikatowych kluczy jednostki w zestawie jednostek. W takim przypadku zaleca się dodanie właściwości, takiej jak **AUTHORID**, która może służyć do unikatowej identyfikacji autora.

 

## <a name="member-element-csdl"></a>Element członkowski (CSDL)

Element **członkowski** jest elementem podrzędnym elementu EnumType i definiuje element członkowski typu wyliczeniowego.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **FunctionImport** .

| Nazwa atrybutu | Jest wymagana | Value                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa elementu członkowskiego.                                                                                                                                                                  |
| **Wartość**      | Nie          | Wartość elementu członkowskiego. Domyślnie pierwszy element członkowski ma wartość 0, a wartość każdego kolejnego modułu wyliczającego jest zwiększana o 1. Może istnieć wiele elementów członkowskich z tymi samymi wartościami. |

 

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowych atrybutów XML) może zostać zastosowana do elementu **FunctionImport** . Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EnumType** z trzema elementami **składowymi** :

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>Element NavigationProperty (CSDL)

Element **NavigationProperty** definiuje właściwość nawigacji, która zawiera odwołanie do drugiego końca skojarzenia. W przeciwieństwie do właściwości zdefiniowanych za pomocą elementu właściwości, właściwości nawigacji nie definiują kształtu i charakterystyki danych. Umożliwiają one nawigowanie po skojarzeniu między dwoma typami jednostek.

Należy zauważyć, że właściwości nawigacji są opcjonalne na obu typach jednostek na końcu skojarzenia. Jeśli zdefiniujesz właściwość nawigacji na jednym typie jednostki na końcu skojarzenia, nie musisz definiować właściwości nawigacji dla typu jednostki na drugim końcu skojarzenia.

Typ danych zwracanych przez właściwość nawigacji jest określany przez liczebność jego zdalnego skojarzenia. Załóżmy na przykład, że właściwość nawigacji, **OrdersNavProp**, istnieje w typie jednostki **klienta** i przechodzi do skojarzenia jeden-do-wielu między **klientem** i **kolejnością**. Ze względu na to, że zdalne skojarzenie dla właściwości nawigacji ma liczebność wiele (\*), jego typ danych jest kolekcją ( **zamówienia**). Podobnie, jeśli właściwość nawigacji, **CustomerNavProp**, istnieje w typie podmiotu **zamówienia** , jego typem danych byłby **Klient** , ponieważ liczebność zdalnego zakończenia to jeden (1).

Element **NavigationProperty** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Elementy adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **NavigationProperty** .

| Nazwa atrybutu   | Jest wymagana | Value                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**         | Tak         | Nazwa właściwości nawigacji.                                                                                                                                                                                                             |
| **Relacje** | Tak         | Nazwa skojarzenia znajdującego się w zakresie modelu.                                                                                                                                                                                |
| **ToRole**       | Tak         | Koniec skojarzenia, na którym kończy się nawigacja. Wartość atrybutu **ToRole** musi być taka sama jak wartość jednego z atrybutów **roli** zdefiniowanych na jednym z punktów końcowych skojarzenia (zdefiniowana w elemencie AssociationEnd).       |
| **FromRole**     | Tak         | Koniec skojarzenia, z którego rozpoczyna się nawigacja. Wartość atrybutu **FromRole** musi być taka sama jak wartość jednego z atrybutów **roli** zdefiniowanych na jednym z punktów końcowych skojarzenia (zdefiniowana w elemencie AssociationEnd). |

 

> [!NOTE]
> Do elementu **NavigationProperty** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

W poniższym przykładzie zdefiniowano typ jednostki (**książkę**) z dwiema właściwościami nawigacji (**PublishedBy** i **WrittenBy**):

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
 

 

## <a name="ondelete-element-csdl"></a>OnDelete — element (CSDL)

Element **onDelete** w języku definicji schematu koncepcyjnego (CSDL) definiuje zachowanie, które jest połączone ze skojarzeniem. Jeśli atrybut **Action** ma ustawioną wartość **kaskadową** na jednym końcu skojarzenia, powiązane typy jednostek na drugim końcu skojarzenia są usuwane, gdy typ jednostki na pierwszym końcu zostanie usunięty. Jeśli skojarzenie między dwoma typami jednostek jest relacją klucz podstawowy-do-podstawowego, wówczas załadowany obiekt zależny jest usuwany, gdy obiekt Principal na drugim końcu skojarzenia zostanie usunięty bez względu na specyfikację **onDelete** .  

> [!NOTE]
> Element **onDelete** ma wpływ tylko na zachowanie aplikacji w czasie wykonywania. nie ma to wpływu na zachowanie w źródle danych. Zachowanie zdefiniowane w źródle danych powinno być takie samo jak zachowanie zdefiniowane w aplikacji.

 

Element **onDelete** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Elementy adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **onDelete** .

| Nazwa atrybutu | Jest wymagana | Value                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Akcja**     | Tak         | **Kaskada** lub **none**. W przypadku usunięcia **kaskadowych**typów jednostek zależnych zostaną usunięte, gdy typ podmiotu zabezpieczeń zostanie usunięty. Jeśli **nie**, typy jednostek zależnych nie zostaną usunięte, gdy typ podmiotu zabezpieczeń zostanie usunięty. |

 

> [!NOTE]
> Do elementu **Association** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **skojarzenia** , który definiuje skojarzenie **CustomerOrders** . Element **onDelete** wskazuje, że wszystkie **zamówienia** , które są powiązane z konkretnym **klientem** i zostały załadowane do obiektu ObjectContext, zostaną usunięte po usunięciu **klienta** .

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Parameter — element (CSDL)

Element **Parameter** w języku definicji schematu koncepcyjnego (CSDL) może być elementem podrzędnym elementu FunctionImport lub funkcji.

### <a name="functionimport-element-application"></a>Aplikacja elementu FunctionImport

Element **Parameter** (jako element podrzędny elementu **FunctionImport** ) służy do definiowania parametrów wejściowych i wyjściowych dla importów funkcji, które są zadeklarowane w CSDL.

Element **Parameter** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (dozwolone zero lub jeden element)
-   Elementy adnotacji (dozwolone zero lub więcej elementów)

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **parametru** .

| Nazwa atrybutu | Jest wymagana | Value                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa parametru.                                                                                                                                                                                                      |
| **Typ**       | Tak         | Typ parametru. Wartość musi być typu **EDMSimpleType** lub złożonego, który znajduje się w zakresie modelu.                                                                                                             |
| **Wyst**       | Nie          | **W**, **out**lub **Inout** , w zależności od tego, czy parametr jest parametrem wejściowym, wyjściowym lub wejściowym.                                                                                                                |
| **MaxLength**  | Nie          | Maksymalna dozwolona długość parametru.                                                                                                                                                                                    |
| **Dokładne**  | Nie          | Precyzja parametru.                                                                                                                                                                                                 |
| **Zasięgu**      | Nie          | Skala parametru.                                                                                                                                                                                                     |
| **SRID**       | Nie          | Identyfikator odwołania do systemu przestrzennego. Prawidłowe tylko dla parametrów typów przestrzennych. Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

 

> [!NOTE]
> Do elementu **Parameter** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

#### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **FunctionImport** z jednym elementem podrzędnym **parametru** . Funkcja akceptuje jeden parametr wejściowy i zwraca kolekcję typów jednostek.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>Aplikacja elementu funkcji

Element **Parameter** (jako element podrzędny elementu **Function** ) definiuje parametry dla funkcji, które są zdefiniowane lub deklarowane w modelu koncepcyjnym.

Element **Parameter** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   CollectionType (zero lub jeden element)
-   ReferenceType (zero lub jeden element)
-   RowType (zero lub jeden element)

> [!NOTE]
> Tylko jeden z elementów **CollectionType**, **ReferenceType**lub **RowType** może być elementem podrzędnym elementu **Właściwości** .

 

-   Elementy adnotacji (dozwolone zero lub więcej elementów)

> [!NOTE]
> Elementy adnotacji muszą występować po wszystkich innych elementach podrzędnych. Elementy adnotacji są dozwolone tylko w obszarze CSDL v2 i nowszych.

 

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **parametru** .

| Nazwa atrybutu   | Jest wymagana | Value                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**         | Tak         | Nazwa parametru.                                                                                                                                                                                                      |
| **Typ**         | Nie          | Typ parametru. Parametr może być dowolnym z następujących typów (lub kolekcji tych typów): <br/> **EdmSimpleType** <br/> typ jednostki <br/> typ złożony <br/> Typ wiersza <br/> typ odwołania                             |
| **Wymaga**     | Nie          | **Wartość true** (wartość domyślna) lub **Fałsz** w zależności od tego, czy właściwość może mieć wartość **null** .                                                                                                                          |
| **DefaultValue** | Nie          | Wartość domyślna właściwości.                                                                                                                                                                                              |
| **MaxLength**    | Nie          | Maksymalna długość wartości właściwości.                                                                                                                                                                                       |
| **FixedLength**  | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg o stałej długości.                                                                                                                          |
| **Dokładne**    | Nie          | Precyzja wartości właściwości.                                                                                                                                                                                            |
| **Zasięgu**        | Nie          | Skala wartości właściwości.                                                                                                                                                                                                |
| **SRID**         | Nie          | Identyfikator odwołania do systemu przestrzennego. Prawidłowe tylko dla właściwości typów przestrzennych. Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**      | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg Unicode.                                                                                                                               |
| **Sortowanie**    | Nie          | Ciąg określający sekwencję sortowania, która ma być używana w źródle danych.                                                                                                                                                   |

 

> [!NOTE]
> Do elementu **Parameter** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

#### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **funkcji** , który używa jednego **parametru** elementu podrzędnego do definiowania parametru funkcji.

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a>Element Principal (CSDL)

Element **Principal** w języku definicji schematu koncepcyjnego (CSDL) jest elementem podrzędnym elementu ReferentialConstraint, który definiuje główne zakończenie ograniczenia referencyjnego. Element **ReferentialConstraint** definiuje funkcje podobne do ograniczenia integralności referencyjnej w relacyjnej bazie danych. W taki sam sposób, w jaki kolumna (lub kolumny) z tabeli bazy danych może odwoływać się do klucza podstawowego innej tabeli, właściwość (lub właściwości) typu jednostki może odwoływać się do klucza jednostki innego typu jednostki. Typ obiektu, do którego istnieje odwołanie, nazywa się *głównym końcem* ograniczenia. Typ jednostki, który odwołuje się do głównego końca, nazywa się *końcem zależnym* od ograniczenia. Elementy **PropertyRef** są używane do określania, które klucze są przywoływane przez zakończenie zależne.

Element **Principal** może mieć następujące elementy podrzędne (w podanej kolejności):

-   PropertyRef (co najmniej jeden element)
-   Elementy adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **głównego** .

| Nazwa atrybutu | Jest wymagana | Value                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Rola**       | Tak         | Nazwa typu jednostki na końcu elementu głównego skojarzenia. |

 

> [!NOTE]
> Do elementu **Principal** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład przedstawia element **ReferentialConstraint** , który jest częścią definicji skojarzenia **PublishedBy** . Właściwość **ID** typu jednostki **wydawcy** wykonuje główne zakończenie ograniczenia referencyjnego.

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
 

 

## <a name="property-element-csdl"></a>Element właściwości (CSDL)

Element **Property** w języku definicji schematu koncepcyjnego (CSDL) może być elementem podrzędnym elementu EntityType, elementu complexType lub elementu RowType.

### <a name="entitytype-and-complextype-element-applications"></a>Elementy EntityType i ComplexType — aplikacje

Elementy **Właściwości** (jako elementy podrzędne elementu **EntityType** lub **complexType** ) definiują kształt i charakterystykę danych, które będzie zawierać wystąpienie typu jednostki lub wystąpienie typu złożonego. Właściwości w modelu koncepcyjnym są analogiczne do właściwości, które są zdefiniowane w klasie. W taki sam sposób, w jaki właściwości klasy definiują kształt klasy i zawierają informacje o obiektach, właściwości w modelu koncepcyjnym definiują kształt typu jednostki i zawierają informacje o wystąpieniach typu jednostki.

Element **Property** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Element dokumentacji (dozwolone zero lub jeden element)
-   Elementy adnotacji (dozwolone zero lub więcej elementów)

Do elementu **Property** można zastosować następujące aspekty: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**. Aspektami są atrybuty XML, które dostarczają informacji o sposobie przechowywania wartości właściwości w magazynie danych.

> [!NOTE]
> Zestawy reguł mogą być stosowane tylko do właściwości typu **EDMSimpleType**.

 

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Właściwości** .

| Nazwa atrybutu                                                         | Jest wymagana | Value                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**                                                               | Tak         | Nazwa właściwości.                                                                                                                                                                                                       |
| **Typ**                                                               | Tak         | Typ wartości właściwości. Typ wartości właściwości musi być typem **EDMSimpleType** lub złożonym (wskazywanym przez w pełni kwalifikowaną nazwą), która znajduje się w zakresie modelu.                                                 |
| **Wymaga**                                                           | Nie          | **Wartość true** (wartość domyślna) lub <strong>Fałsz</strong> w zależności od tego, czy właściwość może mieć wartość null. <br/> [!NOTE]                                                                                                   |
| > W obszarze CSDL V1 właściwość typu złożonego musi mieć `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                       | Nie          | Wartość domyślna właściwości.                                                                                                                                                                                              |
| **MaxLength**                                                          | Nie          | Maksymalna długość wartości właściwości.                                                                                                                                                                                       |
| **FixedLength**                                                        | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg o stałej długości.                                                                                                                          |
| **Dokładne**                                                          | Nie          | Precyzja wartości właściwości.                                                                                                                                                                                            |
| **Zasięgu**                                                              | Nie          | Skala wartości właściwości.                                                                                                                                                                                                |
| **SRID**                                                               | Nie          | Identyfikator odwołania do systemu przestrzennego. Prawidłowe tylko dla właściwości typów przestrzennych. Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                            | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg Unicode.                                                                                                                               |
| **Sortowanie**                                                          | Nie          | Ciąg określający sekwencję sortowania, która ma być używana w źródle danych.                                                                                                                                                   |
| **Obsługują**                                                    | Nie          | **Brak** (wartość domyślna) lub **stała**. Jeśli wartość jest ustawiona na **stała**, wartość właściwości zostanie użyta podczas optymistycznej kontroli współbieżności.                                                                                  |

 

> [!NOTE]
> Do elementu **Property** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

#### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EntityType** z trzema elementami **Właściwości** :

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
 

Poniższy przykład pokazuje element **complexType** z pięcioma elementami **Właściwości** :

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a>Aplikacja elementu RowType

Elementy **Właściwości** (jako elementy podrzędne elementu **RowType** ) definiują kształt i charakterystykę danych, które mogą być przesyłane lub zwracane z funkcji zdefiniowanej przez model.  

Element **Property** może mieć dokładnie jeden z następujących elementów podrzędnych:

-   CollectionType
-   ReferenceType
-   RowType

Element **Property** może mieć dowolną liczbę elementów podrzędnych adnotacji.

> [!NOTE]
> Elementy adnotacji są dozwolone tylko w obszarze CSDL v2 i nowszych.

 

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Właściwości** .

| Nazwa atrybutu                                                     | Jest wymagana | Value                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**                                                           | Tak         | Nazwa właściwości.                                                                                                                                                                                                       |
| **Typ**                                                           | Tak         | Typ wartości właściwości.                                                                                                                                                                                                 |
| **Wymaga**                                                       | Nie          | **Wartość true** (wartość domyślna) lub **Fałsz** w zależności od tego, czy właściwość może mieć wartość null. <br/> [!NOTE]                                                                                                                |
| > W CSDL V1 właściwość typu złożonego musi mieć `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | Nie          | Wartość domyślna właściwości.                                                                                                                                                                                              |
| **MaxLength**                                                      | Nie          | Maksymalna długość wartości właściwości.                                                                                                                                                                                       |
| **FixedLength**                                                    | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg o stałej długości.                                                                                                                          |
| **Dokładne**                                                      | Nie          | Precyzja wartości właściwości.                                                                                                                                                                                            |
| **Zasięgu**                                                          | Nie          | Skala wartości właściwości.                                                                                                                                                                                                |
| **SRID**                                                           | Nie          | Identyfikator odwołania do systemu przestrzennego. Prawidłowe tylko dla właściwości typów przestrzennych. Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg Unicode.                                                                                                                               |
| **Sortowanie**                                                      | Nie          | Ciąg określający sekwencję sortowania, która ma być używana w źródle danych.                                                                                                                                                   |

 

> [!NOTE]
> Do elementu **Property** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

#### <a name="example"></a>Przykład

Poniższy przykład pokazuje elementy **Właściwości** używane do definiowania kształtu zwracanego typu funkcji zdefiniowanej przez model.

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
 

 

## <a name="propertyref-element-csdl"></a>PropertyRef, element (CSDL)

Element **PropertyRef** w języku definicji schematu koncepcyjnego (CSDL) odwołuje się do właściwości typu jednostki, aby wskazać, że właściwość wykona jedną z następujących ról:

-   Część klucza jednostki (właściwości lub zestawu właściwości typu jednostki, które określają tożsamość). Do zdefiniowania klucza jednostki można użyć co najmniej jednego elementu **PropertyRef** .
-   Zależne lub główne zakończenie ograniczenia referencyjnego.

Element **PropertyRef** może zawierać tylko elementy adnotacji (zero lub więcej) jako elementy podrzędne.

> [!NOTE]
> Elementy adnotacji są dozwolone tylko w obszarze CSDL v2 i nowszych.

 

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **PropertyRef** .

| Nazwa atrybutu | Jest wymagana | Value                                |
|:---------------|:------------|:-------------------------------------|
| **Nazwa**       | Tak         | Nazwa właściwości, której dotyczy odwołanie. |

 

> [!NOTE]
> Do elementu **PropertyRef** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

W poniższym przykładzie zdefiniowano typ jednostki (**książkę**). Klucz jednostki jest definiowany przez odwołanie się do właściwości **ISBN** typu jednostki.

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
 

W następnym przykładzie dwa elementy **PropertyRef** są używane do wskazywania, że dwie właściwości (**ID** i **publisherID**) są podmiotem zabezpieczeń i zależnymi końcami ograniczenia referencyjnego.

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
 

 

## <a name="referencetype-element-csdl"></a>ReferenceType — element (CSDL)

Element **ReferenceType** w języku definicji schematu koncepcyjnego (CSDL) określa odwołanie do typu jednostki. Element **ReferenceType** może być elementem podrzędnym następujących elementów:

-   ReturnType (funkcja)
-   Parametr
-   CollectionType

Element **ReferenceType** jest używany podczas definiowania parametru lub zwracanego typu dla funkcji.

Element **ReferenceType** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Elementy adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **ReferenceType** .

| Nazwa atrybutu | Jest wymagana | Value                                         |
|:---------------|:------------|:----------------------------------------------|
| **Typ**       | Tak         | Nazwa typu obiektu, do którego występuje odwołanie. |

 

> [!NOTE]
> Do elementu **ReferenceType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano element **ReferenceType** używany jako element podrzędny elementu **Parameter** w funkcji zdefiniowanej przez model, która akceptuje odwołanie do typu jednostki **osoby** :

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
 

W poniższym przykładzie pokazano element **ReferenceType** używany jako element podrzędny elementu **ReturnType** (Function) w funkcji zdefiniowanej przez model, która zwraca odwołanie do typu jednostki **osoby** :

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
 

 

## <a name="referentialconstraint-element-csdl"></a>ReferentialConstraint, element (CSDL)

Element **ReferentialConstraint** w języku definicji schematu koncepcyjnego (CSDL) definiuje funkcję podobną do ograniczenia integralności referencyjnej w relacyjnej bazie danych. W taki sam sposób, w jaki kolumna (lub kolumny) z tabeli bazy danych może odwoływać się do klucza podstawowego innej tabeli, właściwość (lub właściwości) typu jednostki może odwoływać się do klucza jednostki innego typu jednostki. Typ obiektu, do którego istnieje odwołanie, nazywa się *głównym końcem* ograniczenia. Typ jednostki, który odwołuje się do głównego końca, nazywa się *końcem zależnym* od ograniczenia.

Jeśli klucz obcy, który jest narażony na jeden typ jednostki odwołuje się do właściwości innego typu jednostki, element **ReferentialConstraint** definiuje skojarzenie między dwoma typami jednostek. Ponieważ element **ReferentialConstraint** zawiera informacje o tym, jak są powiązane dwa typy jednostek, w języku specyfikacji mapowania (MSL) nie jest wymagany odpowiedni element AssociationSetMapping. Skojarzenie między dwoma typami jednostek, które nie mają uwidocznionych kluczy obcych musi mieć odpowiedni element **AssociationSetMapping** , aby mapować informacje o skojarzeniach ze źródłem danych.

Jeśli klucz obcy nie jest uwidoczniony dla typu jednostki, element **ReferentialConstraint** może definiować tylko ograniczenie PRIMARY KEY-to-PRIMARY KEY dla typu jednostki i innego typu jednostki.

Element **ReferentialConstraint** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Podmiot zabezpieczeń (dokładnie jeden element)
-   Zależne (dokładnie jeden element)
-   Elementy adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Element **ReferentialConstraint** może mieć dowolną liczbę atrybutów adnotacji (niestandardowe atrybuty XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład przedstawia element **ReferentialConstraint** , który jest używany jako część definicji skojarzenia **PublishedBy** .

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
 

 

## <a name="returntype-function-element-csdl"></a>ReturnType (funkcja) — element (CSDL)

Element **ReturnType** (Function) w języku definicji schematu koncepcyjnego (CSDL) Określa zwracany typ dla funkcji, która jest zdefiniowana w elemencie Function. Typ zwracany funkcji można także określić przy użyciu atrybutu **ReturnValue** .

Zwracanymi typami mogą być dowolne **EdmSimpleType**, typ jednostki, typ złożony, typ wiersza, typ odwołania lub kolekcja jednego z tych typów.

Zwracany typ funkcji można określić przy użyciu atrybutu **typu** **ReturnValue** (funkcja) lub z jednym z następujących elementów podrzędnych:

-   CollectionType
-   ReferenceType
-   RowType

> [!NOTE]
> Model nie zostanie zweryfikowany w przypadku określenia typu zwracanego funkcji z atrybutem **typu** **ReturnValue** (Function) i jednym z elementów podrzędnych.

 

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **ReturnType** (Function).

| Nazwa atrybutu | Jest wymagana | Value                              |
|:---------------|:------------|:-----------------------------------|
| **Atrybuty** | Nie          | Typ zwracany przez funkcję. |

 

> [!NOTE]
> Do elementu **ReturnType** (Function) można stosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

W poniższym przykładzie użyto elementu **Function** do zdefiniowania funkcji, która zwraca liczbę lat, w których książka była drukowana. Zwróć uwagę, że typ zwracany jest określony przez atrybut **Type** elementu **ReturnType** (Function).

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>ReturnType (FunctionImport), element (CSDL)

Element **ReturnType** (FunctionImport) w języku definicji schematu koncepcyjnego (CSDL) Określa zwracany typ dla funkcji, która jest zdefiniowana w elemencie FunctionImport. Typ zwracany funkcji można także określić przy użyciu atrybutu **ReturnValue** .

Zwracane typy mogą być dowolną kolekcją typu jednostki, typu złożonego lub **EdmSimpleType**,

Zwracany typ funkcji jest określany przy użyciu atrybutu **Type** elementu **ReturnType** (FunctionImport).

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **ReturnType** (FunctionImport).

| Nazwa atrybutu | Jest wymagana | Value                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**       | Nie          | Typ zwracany przez funkcję. Wartość musi być kolekcją typu ComplexType, EntityType lub EDMSimpleType.                                                                                      |
| **Elementy**  | Nie          | Jeśli funkcja zwraca kolekcję typów jednostek, wartość **obiektu EntitySet** musi być zestawem jednostek, do którego należy kolekcja. W przeciwnym razie atrybut **EntitySet** nie może być używany. |

 

> [!NOTE]
> Do elementu **ReturnType** (FunctionImport) można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład używa elementu **FunctionImport** , który zwraca książki i wydawców. Należy zauważyć, że funkcja zwraca dwa zestawy wyników i w związku z tym dwa elementy **ReturnType** (FunctionImport) są określone.

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>RowType, element (CSDL)

Element **RowType** w języku definicji schematu koncepcyjnego (CSDL) definiuje nienazwaną strukturę jako parametr lub typ zwracany dla funkcji zdefiniowanej w modelu koncepcyjnym.

Element **RowType** może być elementem podrzędnym następujących elementów:

-   CollectionType
-   Parametr
-   ReturnType (funkcja)

Element **RowType** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Właściwość (co najmniej jedna)
-   Elementy adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Do elementu **RowType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje funkcję zdefiniowaną przez model, która używa elementu **CollectionType** , aby określić, że funkcja zwraca kolekcję wierszy (jak określono w elemencie **RowType** ).

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

Element **schematu** jest głównym elementem definicji modelu koncepcyjnego. Zawiera definicje obiektów, funkcji i kontenerów, które tworzą model koncepcyjny.

Element **schematu** może zawierać zero lub więcej z następujących elementów podrzędnych:

-   Użyciu
-   EntityContainer
-   Typ entityType
-   EnumType
-   Skojarzenie
-   ComplexType
-   Funkcja

Element **schematu** może zawierać zero lub jeden z elementów adnotacji.

> [!NOTE]
> Element **Function** i elementy adnotacji są dozwolone tylko w CSDL v2 i nowszych.

 

Element **schematu** używa atrybutu **przestrzeni nazw** do definiowania przestrzeni nazw dla typu jednostki, typu złożonego i obiektów skojarzenia w modelu koncepcyjnym. W przestrzeni nazw żadne dwa obiekty nie mogą mieć takiej samej nazwy. Przestrzenie nazw mogą obejmować wiele elementów **schematu** i wiele plików. csdl.

Przestrzeń nazw modelu koncepcyjnego różni się od przestrzeni nazw XML elementu **Schema** . Przestrzeń nazw modelu koncepcyjnego (zgodnie z definicją atrybutu **Namespace** ) jest kontenerem logicznym dla typów jednostek, typów złożonych i typów skojarzeń. Przestrzeń nazw XML (wskazywana przez atrybut **xmlns** ) elementu **schematu** jest domyślną przestrzenią nazw dla elementów podrzędnych i atrybutów elementu **Schema** . Przestrzenie nazw XML w formularzu https://schemas.microsoft.com/ado/YYYY/MM/edm (gdzie RRRR i MM reprezentują odpowiednio rok i miesiąc) są zarezerwowane dla CSDL. Elementy niestandardowe i atrybuty nie mogą znajdować się w przestrzeniach nazw, które mają ten formularz.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Schema** .

| Nazwa atrybutu | Jest wymagana | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Tak         | Przestrzeń nazw modelu koncepcyjnego. Wartość atrybutu **Namespace** służy do tworzenia w pełni kwalifikowanej nazwy typu. Na przykład jeśli obiekt **EntityType** o nazwie *Customer* znajduje się w prostej przestrzeni nazw. przykład. model, wówczas w pełni kwalifikowana nazwa elementu **EntityType** to SimpleExampleModel. Customer. <br/> Następujące ciągi nie mogą być używane jako wartość atrybutu **Namespace** : **System**, **przejściowy**lub **EDM**. Wartość atrybutu **przestrzeni nazw** nie może być taka sama jak wartość atrybutu **Namespace** w elemencie schematu SSDL. |
| **Alias**      | Nie          | Identyfikator używany zamiast nazwy przestrzeni nazw. Na przykład jeśli obiekt **EntityType** o nazwie *Customer* znajduje się w prostej. przykładowej przestrzeni nazw, a wartość atrybutu **alias** jest *modelem*, można użyć model. Customer jako w pełni kwalifikowanej nazwy typu **EntityType.**                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> Do elementu **schematu** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **schematu** , który zawiera element **EntityContainer** , dwa elementy **EntityType** i jeden element **skojarzenia** .

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="typeref-element-csdl"></a>TypeRef, element (CSDL)

Element **TypeRef** w języku definicji schematu koncepcyjnego (CSDL) zawiera odwołanie do istniejącego typu nazwanego. Element **TypeRef** może być elementem podrzędnym elementu CollectionType, który służy do określenia, że funkcja ma kolekcję jako parametr lub typ zwracany.

Element **TypeRef** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Elementy adnotacji (zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **TypeRef** . Należy zauważyć, że atrybuty **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**i **Collation** mają zastosowanie tylko do **EDMSimpleTypes**.

| Nazwa atrybutu                                                     | Jest wymagana | Value                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**                                                           | Nie          | Nazwa typu, do którego się odwołuje.                                                                                                                                                                                          |
| **Wymaga**                                                       | Nie          | **Wartość true** (wartość domyślna) lub **Fałsz** w zależności od tego, czy właściwość może mieć wartość null. <br/> [!NOTE]                                                                                                                |
| > W CSDL V1 właściwość typu złożonego musi mieć `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | Nie          | Wartość domyślna właściwości.                                                                                                                                                                                              |
| **MaxLength**                                                      | Nie          | Maksymalna długość wartości właściwości.                                                                                                                                                                                       |
| **FixedLength**                                                    | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg o stałej długości.                                                                                                                          |
| **Dokładne**                                                      | Nie          | Precyzja wartości właściwości.                                                                                                                                                                                            |
| **Zasięgu**                                                          | Nie          | Skala wartości właściwości.                                                                                                                                                                                                |
| **SRID**                                                           | Nie          | Identyfikator odwołania do systemu przestrzennego. Prawidłowe tylko dla właściwości typów przestrzennych. Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy wartość właściwości będzie przechowywana jako ciąg Unicode.                                                                                                                               |
| **Sortowanie**                                                      | Nie          | Ciąg określający sekwencję sortowania, która ma być używana w źródle danych.                                                                                                                                                   |

 

> [!NOTE]
> Do elementu **CollectionType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład pokazuje funkcję zdefiniowaną przez model, która używa elementu **TypeRef** (jako element podrzędny elementu **CollectionType** ), aby określić, że funkcja akceptuje kolekcję typów jednostek **działu** .

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
 

 

## <a name="using-element-csdl"></a>Używanie elementu (CSDL)

Element **using** w języku definicji schematu koncepcyjnego (CSDL) importuje zawartość modelu koncepcyjnego, który istnieje w innej przestrzeni nazw. Ustawiając wartość atrybutu **Namespace** , można odwoływać się do typów jednostek, typów złożonych i typów skojarzeń, które są zdefiniowane w innym modelu koncepcyjnym. Więcej niż jeden element **using** może być elementem podrzędnym elementu **schematu** .

> [!NOTE]
> Element **using** w CSDL nie działa tak samo jak instrukcja **using** w języku programowania. Importując przestrzeń nazw za pomocą instrukcji **using** w języku programowania, nie ma wpływu na obiekty w pierwotnej przestrzeni nazw. W obszarze CSDL zaimportowana przestrzeń nazw może zawierać typ jednostki pochodzący od typu jednostki w pierwotnej przestrzeni nazw. Może to mieć wpływ na zestawy jednostek zadeklarowane w pierwotnej przestrzeni nazw.

 

Element **using** może mieć następujące elementy podrzędne:

-   Dokumentacja (dozwolone zero lub jeden element)
-   Elementy adnotacji (dozwolone zero lub więcej elementów)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **using** .

| Nazwa atrybutu | Jest wymagana | Value                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Tak         | Nazwa zaimportowanej przestrzeni nazw.                                                                                                                                                |
| **Alias**      | Tak         | Identyfikator używany zamiast nazwy przestrzeni nazw. Chociaż ten atrybut jest wymagany, nie jest wymagane, aby był używany zamiast nazwy przestrzeni nazw do kwalifikowania nazw obiektów. |

 

> [!NOTE]
> Do elementu **using** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

 

### <a name="example"></a>Przykład

Poniższy przykład demonstruje element **using** używany do importowania przestrzeni nazw, która jest zdefiniowana w innym miejscu. Należy zauważyć, że przestrzeń nazw dla elementu **schematu** jest wyświetlana `BooksModel`. Właściwość `Address` elementu**EntityType** `Publisher` jest typem złożonym, który jest zdefiniowany w przestrzeni nazw `ExtendedBooksModel` (zaimportowanym przy **użyciu elementu using** ).

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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

Atrybuty adnotacji w języku definicji schematu koncepcyjnego (CSDL) to niestandardowe atrybuty XML w modelu koncepcyjnym. Oprócz posiadania prawidłowej struktury XML, następujące elementy muszą mieć wartość true atrybutów adnotacji:

-   Atrybuty adnotacji nie mogą znajdować się w żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.
-   Do danego elementu CSDL można zastosować więcej niż jeden atrybut adnotacji.
-   W pełni kwalifikowane nazwy jakichkolwiek dwóch atrybutów adnotacji nie mogą być takie same.

Atrybuty adnotacji mogą służyć do dostarczania dodatkowych metadanych dotyczących elementów w modelu koncepcyjnym. Do metadanych zawartych w elementach adnotacji można uzyskać dostęp w czasie wykonywania przy użyciu klas w przestrzeni nazw System. Data. Metadata. Edm.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EntityType** z atrybutem adnotacji (**CustomAttribute**). W przykładzie pokazano również element adnotacji zastosowany do elementu typu jednostki.

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
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
 

Poniższy kod pobiera metadane w atrybucie adnotacji i zapisuje go w konsoli programu:

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
 

W powyższym kodzie założono, że plik `School.csdl` znajduje się w katalogu wyjściowym projektu i dodano następujące instrukcje `Imports` i `Using` do projektu:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a>Elementy adnotacji (CSDL)

Elementy adnotacji w języku definicji schematu koncepcyjnego (CSDL) to niestandardowe elementy XML w modelu koncepcyjnym. Oprócz posiadania prawidłowej struktury XML, następujące elementy adnotacji muszą być prawdziwe:

-   Elementy adnotacji nie mogą znajdować się w żadnej przestrzeni nazw XML zarezerwowanej dla CSDL.
-   Więcej niż jeden element adnotacji może być elementem podrzędnym danego elementu CSDL.
-   W pełni kwalifikowane nazwy jakichkolwiek dwóch elementów adnotacji nie mogą być takie same.
-   Elementy adnotacji muszą występować po wszystkich innych elementach podrzędnych danego elementu CSDL.

Elementy adnotacji mogą służyć do dostarczania dodatkowych metadanych dotyczących elementów w modelu koncepcyjnym. Począwszy od .NET Framework w wersji 4, metadane zawarte w elementach adnotacji są dostępne w czasie wykonywania przy użyciu klas w przestrzeni nazw System. Data. Metadata. Edm.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EntityType** z elementem adnotacji (**CustomElement**). Przykład pokazuje również atrybut adnotacji zastosowany do elementu typu jednostki.

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
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
 

Poniższy kod pobiera metadane w elemencie adnotacji i zapisuje go w konsoli programu:

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
 

W powyższym kodzie przyjęto założenie, że plik szkoły. csdl znajduje się w katalogu wyjściowym projektu i dodano następujące instrukcje `Imports` i `Using` do projektu:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a>Typy modelu koncepcyjnego (CSDL)

Język definicji schematu koncepcyjnego (CSDL) obsługuje zestaw abstrakcyjnych typów danych pierwotnych o nazwie **EDMSimpleTypes**, które definiują właściwości w modelu koncepcyjnym. **EDMSimpleTypes** to serwery proxy dla typów danych pierwotnych, które są obsługiwane w magazynie lub środowisku hostingu.

W poniższej tabeli przedstawiono typy danych pierwotnych, które są obsługiwane przez CSDL. W tabeli wymieniono również zestawy reguł, które mogą być stosowane do poszczególnych **EDMSimpleType**.

| EDMSimpleType                    | Opis                                                | Odpowiednie aspekty                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **EDM. Binary**                   | Zawiera dane binarne.                                      | MaxLength, FixedLength, nullable, wartość domyślna                                |
| **EDM. Boolean**                  | Zawiera wartość **true** lub **false**.                  | Dopuszcza wartość null, wartość domyślna                                                        |
| **EDM. Byte**                     | Zawiera 8-bitową liczbę całkowitą bez znaku.                  | Precyzja, wartość null, wartość domyślna                                             |
| **EDM. DateTime**                 | Przedstawia datę i godzinę.                                | Precyzja, wartość null, wartość domyślna                                             |
| **EDM. DateTimeOffset**           | Zawiera datę i godzinę przesunięcia w minutach od GMT. | Precyzja, wartość null, wartość domyślna                                             |
| **EDM. Decimal**                  | Zawiera wartość liczbową ze stałą dokładnością i skalą.   | Precyzja, wartość null, wartość domyślna                                             |
| **EDM. Double**                   | Zawiera liczbę zmiennoprzecinkową z dokładnością do 15 cyfr   | Precyzja, wartość null, wartość domyślna                                             |
| **EDM. float**                    | Zawiera liczbę zmiennoprzecinkową z dokładnością do 7 cyfr.   | Precyzja, wartość null, wartość domyślna                                             |
| **EDM. GUID**                     | Zawiera unikatowy identyfikator 16-bajtowy.                      | Precyzja, wartość null, wartość domyślna                                             |
| **EDM. Int16**                    | Zawiera wartość 16-bitową liczbę całkowitą ze znakiem.                    | Precyzja, wartość null, wartość domyślna                                             |
| **EDM. Int32**                    | Zawiera podpisaną 32-bitową liczbę całkowitą.                    | Precyzja, wartość null, wartość domyślna                                             |
| **EDM. Int64**                    | Zawiera podpisaną 64-bitową liczbę całkowitą.                    | Precyzja, wartość null, wartość domyślna                                             |
| **EDM.**                    | Zawiera 8-bitową liczbę całkowitą ze znakiem.                     | Precyzja, wartość null, wartość domyślna                                             |
| **EDM. String**                   | Zawiera dane znakowe.                                   | Unicode, FixedLength, MaxLength, Collation, Precision, nullable, wartość domyślna |
| **EDM. Time**                     | Zawiera godzinę.                                    | Precyzja, wartość null, wartość domyślna                                             |
| **EDM. Geography**                |                                                            | Nullable, default, SRID                                                  |
| **EDM. geographyPoint względem**           |                                                            | Nullable, default, SRID                                                  |
| **EDM. GeographyLineString**      |                                                            | Nullable, default, SRID                                                  |
| **EDM. GeographyPolygon**         |                                                            | Nullable, default, SRID                                                  |
| **EDM. GeographyMultiPoint**      |                                                            | Nullable, default, SRID                                                  |
| **EDM. GeographyMultiLineString** |                                                            | Nullable, default, SRID                                                  |
| **EDM. GeographyMultiPolygon**    |                                                            | Nullable, default, SRID                                                  |
| **EDM. Geographycollection**      |                                                            | Nullable, default, SRID                                                  |
| **EDM. geometria**                 |                                                            | Nullable, default, SRID                                                  |
| **EDM. GeometryPoint**            |                                                            | Nullable, default, SRID                                                  |
| **EDM. GeometryLineString**       |                                                            | Nullable, default, SRID                                                  |
| **EDM. GeometryPolygon**          |                                                            | Nullable, default, SRID                                                  |
| **EDM. GeometryMultiPoint**       |                                                            | Nullable, default, SRID                                                  |
| **EDM. GeometryMultiLineString**  |                                                            | Nullable, default, SRID                                                  |
| **EDM. GeometryMultiPolygon**     |                                                            | Nullable, default, SRID                                                  |
| **EDM. GeometryCollection**       |                                                            | Nullable, default, SRID                                                  |

## <a name="facets-csdl"></a>Zestawy reguł (CSDL)

Aspekty w języku definicji schematu koncepcyjnego (CSDL) przedstawiają ograniczenia dotyczące właściwości typów jednostek i typów złożonych. Zestawy reguł są wyświetlane jako atrybuty XML dla następujących elementów CSDL:

-   Właściwość
-   TypeRef
-   Parametr

W poniższej tabeli opisano aspekty, które są obsługiwane w CSDL. Wszystkie aspekty są opcjonalne. Niektóre z wymienionych poniżej zestawów reguł są używane przez Entity Framework podczas generowania bazy danych z modelu koncepcyjnego.

> [!NOTE]
> Aby uzyskać informacje na temat typów danych w modelu koncepcyjnym, zobacz typy modelu koncepcyjnego (CSDL).

| Aspekcie               | Opis                                                                                                                                                                                                                                                   | Stosuje się do                                                                                                                                                                                                                                                                                                                                                                           | Używany do generowania bazy danych | Używane przez środowisko uruchomieniowe |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| **Sortowanie**       | Określa sekwencję sortowania (lub sekwencję sortowania), która ma być używana podczas wykonywania operacji porównania i porządkowania na wartościach właściwości.                                                                                                               | **EDM. String**                                                                                                                                                                                                                                                                                                                                                                       | Tak                              | Nie                  |
| **Obsługują** | Wskazuje, że wartość właściwości powinna być używana do optymistycznych kontroli współbieżności.                                                                                                                                                                    | Wszystkie właściwości **EDMSimpleType**                                                                                                                                                                                                                                                                                                                                                     | Nie                               | Tak                 |
| **Domyślne**         | Określa wartość domyślną właściwości, jeśli nie podano żadnej wartości podczas tworzenia wystąpienia.                                                                                                                                                                       | Wszystkie właściwości **EDMSimpleType**                                                                                                                                                                                                                                                                                                                                                     | Tak                              | Tak                 |
| **FixedLength**     | Określa, czy długość wartości właściwości może się różnić.                                                                                                                                                                                                  | **EDM. Binary**, **EDM. String**                                                                                                                                                                                                                                                                                                                                                       | Tak                              | Nie                  |
| **MaxLength**       | Określa maksymalną długość wartości właściwości.                                                                                                                                                                                                           | **EDM. Binary**, **EDM. String**                                                                                                                                                                                                                                                                                                                                                       | Tak                              | Nie                  |
| **Wymaga**        | Określa, czy właściwość może mieć wartość **null** .                                                                                                                                                                                                     | Wszystkie właściwości **EDMSimpleType**                                                                                                                                                                                                                                                                                                                                                     | Tak                              | Tak                 |
| **Dokładne**       | Dla właściwości typu **Decimal**określa liczbę cyfr, jaką może mieć wartość właściwości. Dla właściwości typu **Time**, **DateTime**i **DateTimeOffset**określa liczbę cyfr ułamkowych części sekundy wartości właściwości. | **EDM. DateTime**, **EDM. DateTimeOffset**, **EDM. Decimal**, **EDM. Time**                                                                                                                                                                                                                                                                                                              | Tak                              | Nie                  |
| **Zasięgu**           | Określa liczbę cyfr z prawej strony punktu dziesiętnego dla wartości właściwości.                                                                                                                                                                      | **EDM. Decimal**                                                                                                                                                                                                                                                                                                                                                                      | Tak                              | Nie                  |
| **SRID**            | Określa identyfikator systemu odwołań do systemu przestrzennego. Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).                                                              | **EDM. Geography, EDM. geographyPoint względem, EDM. GeographyLineString, EDM. GeographyPolygon, EDM. GeographyMultiPoint, EDM. GeographyMultiLineString, EDM. GeographyMultiPolygon, EDM. Geographycollection, EDM. Geometry, EDM. GeometryPoint, EDM. GeometryLineString, EDM. GeometryPolygon, EDM. GeometryMultiPoint, EDM. GeometryMultiLineString, EDM. GeometryMultiPolygon, EDM. GeometryCollection** | Nie                               | Tak                 |
| **Unicode**         | Wskazuje, czy wartość właściwości jest przechowywana w formacie Unicode.                                                                                                                                                                                                    | **EDM. String**                                                                                                                                                                                                                                                                                                                                                                       | Tak                              | Tak                 |

>[!NOTE]
> Podczas generowania bazy danych z modelu koncepcyjnego Kreator generowania bazy danych rozpozna wartość atrybutu **StoreGeneratedPattern** w elemencie **Property** , jeśli znajduje się w następującej przestrzeni nazw: https://schemas.microsoft.com/ado/2009/02/edm/annotation. Obsługiwane wartości atrybutu to **tożsamość** i **obliczana**. Wartość **tożsamości** spowoduje utworzenie kolumny bazy danych o wartości tożsamości wygenerowanej w bazie danych. Wartość **obliczana** spowoduje wygenerowanie kolumny z wartością obliczaną w bazie danych.

### <a name="example"></a>Przykład

Poniższy przykład przedstawia aspekty stosowane do właściwości typu jednostki:

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="https://schemas.microsoft.com/ado/2009/02/edm/annotation" />
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
