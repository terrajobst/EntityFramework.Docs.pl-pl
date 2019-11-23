---
title: Specyfikacja SSDL — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182547"
---
# <a name="ssdl-specification"></a>Specyfikacja SSDL
Język definicji schematu magazynu (SSDL) to język oparty na języku XML, który opisuje model magazynu aplikacji Entity Framework.

W aplikacji Entity Framework metadane modelu magazynu są ładowane z pliku SSDL (zapisywanego w plikach SSDL) do wystąpienia elementu System. Data. Metadata. Edm. StoreItemCollection i są dostępne przy użyciu metod w Klasa System. Data. Metadata. Edm. MetadataWorkspace. Entity Framework używa metadanych modelu magazynu do translacji zapytań względem modelu koncepcyjnego na polecenia specyficzne dla magazynu.

Entity Framework Designer (programu EF Designer) przechowuje informacje o modelu magazynu w pliku edmx w czasie projektowania. W czasie kompilacji Entity Designer używa informacji w pliku. edmx, aby utworzyć plik SSDL, który jest wymagany przez Entity Framework w czasie wykonywania.

Wersje plików SSDL są odróżniane według przestrzeni nazw XML.

| Wersja SSDL | Przestrzeń nazw XML                                     |
|:-------------|:--------------------------------------------------|
| SSDL v1      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| SSDL v2      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| SSDL v3      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a>Element Association (SSDL)

Element **Association** w języku definicji schematu magazynu (SSDL) określa kolumny tabeli, które uczestniczą w ograniczeniu klucza obcego w źródłowej bazie danych. Dwa wymagane elementy podrzędne elementu End określają tabele na końcu skojarzenia i liczebność na każdym końcu. Opcjonalny element ReferentialConstraint określa główne i zależne zakończenia skojarzenia, a także powiązane kolumny. Jeśli żaden element **ReferentialConstraint** nie istnieje, element AssociationSetMapping musi być używany do określenia mapowań kolumn dla skojarzenia.

Element **Association** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jedna)
-   End (dokładnie dwa)
-   ReferentialConstraint (zero lub jeden)
-   Elementy adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Association** .

| Nazwa atrybutu | Jest wymagana | Wartość                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa odpowiedniego ograniczenia klucza obcego w źródłowej bazie danych. |

> [!NOTE]
> Do elementu **Association** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **skojarzenia** , który używa elementu **ReferentialConstraint** , aby określić kolumny, które uczestniczą w ograniczeniu klucza obcego **\_CustomerOrders** :

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="associationset-element-ssdl"></a>AssociationSet, element (SSDL)

Element **AssociationSet** w języku definicji schematu magazynu (SSDL) reprezentuje ograniczenie klucza obcego między dwiema tabelami w źródłowej bazie danych. Kolumny tabeli, które uczestniczą w ograniczeniu klucza obcego, są określone w elemencie skojarzenia. Element **Association** , który odnosi się do danego elementu **AssociationSet** , jest określony w atrybucie **skojarzenia** elementu **AssociationSet** .

Zestawy skojarzeń SSDL są mapowane do zestawów skojarzeń CSDL przez element AssociationSetMapping. Jeśli jednak skojarzenie CSDL dla danego zestawu skojarzeń CSDL jest zdefiniowane za pomocą elementu ReferentialConstraint, nie jest potrzebny odpowiedni element **AssociationSetMapping** . W tym przypadku, jeśli element **AssociationSetMapping** jest obecny, mapowania, które definiuje, zostaną przesłonięte przez element **ReferentialConstraint** .

Element **AssociationSet** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jedna)
-   Koniec (zero lub dwa)
-   Elementy adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **AssociationSet** .

| Nazwa atrybutu  | Jest wymagana | Wartość                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| **Nazwa**        | Tak         | Nazwa ograniczenia klucza obcego, które reprezentuje zestaw skojarzeń.                          |
| **Skojarzenie** | Tak         | Nazwa skojarzenia, która definiuje kolumny, które uczestniczą w ograniczeniu klucza obcego. |

> [!NOTE]
> Do elementu **AssociationSet** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład przedstawia element **AssociationSet** , który reprezentuje `FK_CustomerOrders` ograniczenie klucza obcego w źródłowej bazie danych:

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a>CollectionType — element (SSDL)

Element **CollectionType** w języku definicji schematu magazynu (SSDL) określa, że zwracany typ funkcji jest kolekcją. Element **CollectionType** jest elementem podrzędnym elementu ReturnType. Typ kolekcji jest określany za pomocą elementu podrzędnego RowType:

> [!NOTE]
> Do elementu **CollectionType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje funkcję, która używa elementu **CollectionType** , aby określić, że funkcja zwraca kolekcję wierszy.

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="commandtext-element-ssdl"></a>CommandText — element (SSDL)

Element **CommandText** w języku definicji schematu magazynu (SSDL) jest elementem podrzędnym elementu Function, który umożliwia zdefiniowanie instrukcji SQL, która jest wykonywana w bazie danych. Element **CommandText** umożliwia dodanie funkcji podobnego do procedury składowanej w bazie danych, ale w modelu magazynu należy zdefiniować element **CommandText** .

Element **CommandText** nie może mieć elementów podrzędnych. Treść elementu **CommandText** musi być prawidłową instrukcją SQL dla źródłowej bazy danych.

Do elementu **CommandText** nie mają zastosowania żadne atrybuty.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **funkcji** z elementem **CommandText** elementu podrzędnego. Udostępnienie funkcji **UpdateProductInOrder** jako metody w obiekcie ObjectContext przez zaimportowanie jej do modelu koncepcyjnego.  

``` xml
 <Function Name="UpdateProductInOrder" IsComposable="false">
   <CommandText>
     UPDATE Orders
     SET ProductId = @productId
     WHERE OrderId = @orderId;
   </CommandText>
   <Parameter Name="productId"
              Mode="In"
              Type="int"/>
   <Parameter Name="orderId"
              Mode="In"
              Type="int"/>
 </Function>
```

## <a name="definingquery-element-ssdl"></a>DefiningQuery — element (SSDL)

Element **DefiningQuery** w języku definicji schematu magazynu (SSDL) umożliwia wykonywanie instrukcji SQL bezpośrednio w źródłowej bazie danych. Element **DefiningQuery** jest często używany jak widok bazy danych, ale widok jest definiowany w modelu magazynu zamiast w bazie danych. Widok zdefiniowany w elemencie **DefiningQuery** można zamapować na typ jednostki w modelu koncepcyjnym za pomocą elementu elementu EntitySetMapping. Mapowania te są tylko do odczytu.  

Następująca składnia SSDL pokazuje deklarację **obiektu EntitySet** , po którym następuje element **DefiningQuery** , który zawiera zapytanie użyte do pobrania widoku.

``` xml
 <Schema>
     <EntitySet Name="Tables" EntityType="Self.STable">
         <DefiningQuery>
           SELECT  TABLE_CATALOG,
                   'test' as TABLE_SCHEMA,
                   TABLE_NAME
           FROM    INFORMATION_SCHEMA.TABLES
         </DefiningQuery>
     </EntitySet>
 </Schema>
```

Za pomocą procedur składowanych w Entity Framework można włączyć scenariusze odczytu i zapisu w widokach. Można użyć widoku źródła danych lub widoku Entity SQL jako tabeli bazowej do pobierania danych i przetwarzania zmian przez procedury składowane.

Można użyć elementu **DefiningQuery** , aby docelowa Microsoft SQL Server Compact 3,5. Chociaż SQL Server Compact 3,5 nie obsługuje procedur składowanych, można zaimplementować podobną funkcjonalność przy użyciu elementu **DefiningQuery** . Innym miejscem, gdzie może być przydatne, jest tworzenie procedur składowanych w celu pokonania niezgodności między typami danych używanymi w języku programowania a tymi źródłami danych. Można napisać **DefiningQuery** , który przyjmuje określony zestaw parametrów, a następnie wywołuje procedurę składowaną z innym zestawem parametrów, na przykład procedurą składowaną, która usuwa dane.

## <a name="dependent-element-ssdl"></a>Element zależny (SSDL)

Element **zależny** w języku definicji schematu magazynu (SSDL) jest elementem podrzędnym elementu ReferentialConstraint, który definiuje zależne zakończenie ograniczenia klucza obcego (nazywanego również ograniczeniem referencyjnym). Element **zależny** określa kolumnę (lub kolumny) w tabeli, która odwołuje się do kolumny klucza podstawowego (lub kolumn). Elementy **PropertyRef** określają, do których kolumn odwołują się odwołania. Główny element określa kolumny klucza podstawowego, do których odwołują się kolumny, które są określone w elemencie **zależnym** .

Element **zależny** może mieć następujące elementy podrzędne (w podanej kolejności):

-   PropertyRef (co najmniej jeden)
-   Elementy adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **zależnego** .

| Nazwa atrybutu | Jest wymagana | Wartość                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rola**       | Tak         | Taka sama wartość jak atrybut **roli** (jeśli jest używany) odpowiadającego elementu końcowego; w przeciwnym razie nazwa tabeli zawierającej kolumnę odwołania. |

> [!NOTE]
> Do elementu **zależnego** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element skojarzenia, który używa elementu **ReferentialConstraint** , aby określić kolumny, które uczestniczą w ograniczeniu klucza obcego **\_CustomerOrders** . Element **zależny** określa kolumnę **CustomerID** tabeli **Order** jako zależne zakończenie ograniczenia.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="documentation-element-ssdl"></a>Element dokumentacji (SSDL)

Element **dokumentacji** w języku definicji schematu magazynu (SSDL) może służyć do dostarczania informacji dotyczących obiektu, który jest zdefiniowany w elemencie nadrzędnym.

Element **dokumentacji** może zawierać następujące elementy podrzędne (w podanej kolejności):

-   **Podsumowanie**: Krótki opis elementu nadrzędnego. (zero lub jeden element)
-   **LongDescription**: obszerny opis elementu nadrzędnego. (zero lub jeden element)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Do elementu **dokumentacji** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **dokumentacji** jako element podrzędny elementu EntityType.

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="end-element-ssdl"></a>End — element (SSDL)

Element **End** w języku definicji schematu magazynu (SSDL) określa tabelę i liczbę wierszy na jednym końcu ograniczenia klucza obcego w źródłowej bazie danych. Element **End** może być elementem podrzędnym elementu Association lub AssociationSet elementu. W każdym przypadku możliwe elementy podrzędne i odpowiednie atrybuty są różne.

### <a name="end-element-as-a-child-of-the-association-element"></a>End — element jako element podrzędny elementu Association

Element **End** (jako element podrzędny elementu **Association** ) określa tabelę i liczbę wierszy na końcu ograniczenia klucza obcego z atrybutami **Type** i **liczebnością** odpowiednio. Zakończenia ograniczenia klucza obcego są zdefiniowane jako część skojarzenia SSDL; Skojarzenie SSDL musi mieć dokładnie dwa punkty końcowe.

Element **końcowy** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   OnDelete (zero lub jeden element)
-   Elementy adnotacji (zero lub więcej elementów)

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do elementu **końcowego** , gdy jest elementem podrzędnym elementu **skojarzenia** .

| Nazwa atrybutu   | Jest wymagana | Wartość                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**         | Tak         | W pełni kwalifikowana nazwa zestawu jednostek SSDL, który znajduje się na końcu ograniczenia klucza obcego.                                                                                                                                                                                                                                                                                          |
| **Rola**         | Nie          | Wartość atrybutu **roli** w głównym lub zależnym elemencie odpowiedniego elementu ReferentialConstraint (jeśli jest używany).                                                                                                                                                                                                                                             |
| **Liczebność** | Tak         | **1**, **0.. 1**lub **\*** w zależności od liczby wierszy, które mogą znajdować się na końcu ograniczenia klucza obcego. <br/> **1** wskazuje, że dokładnie jeden wiersz istnieje na końcu ograniczenia klucza obcego. <br/> **0.. 1** oznacza, że na końcu ograniczenia klucza obcego istnieje zero lub jeden wiersz. <br/> **\*** wskazuje, że na końcu ograniczenia klucza obcego istnieje zero, jeden lub więcej wierszy. |

> [!NOTE]
> Do elementu **End** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

#### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **skojarzenia** , który definiuje klucz obcy **\_CustomerOrders** ograniczenie klucza obcego. Wartości **liczebności** określone dla każdego elementu **końcowego** wskazują, że wiele wierszy w tabeli **Orders** może być skojarzonych z wierszem w tabeli **Customers** , ale tylko jeden wiersz w tabeli **Customers** może być skojarzony z wierszem w tabeli **Orders** . Ponadto element **onDelete** wskazuje, że wszystkie wiersze w tabeli **Orders** , które odwołują się do określonego wiersza w tabeli **Customers** , zostaną usunięte, jeśli wiersz w tabeli **Customers** zostanie usunięty.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

### <a name="end-element-as-a-child-of-the-associationset-element"></a>End — element jako element podrzędny elementu AssociationSet

Element **End** (jako element podrzędny elementu **AssociationSet** ) określa tabelę na jednym końcu ograniczenia klucza obcego w źródłowej bazie danych.

Element **końcowy** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jedna)
-   Elementy adnotacji (zero lub więcej)

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do elementu **końcowego** , gdy jest elementem podrzędnym elementu **AssociationSet** .

| Nazwa atrybutu | Jest wymagana | Wartość                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| **Elementy**  | Tak         | Nazwa zestawu jednostek SSDL znajdującego się na końcu ograniczenia klucza obcego.                                      |
| **Rola**       | Nie          | Wartość jednego z atrybutów **roli** określonych dla jednego elementu **końcowego** odpowiedniego elementu skojarzenia. |

> [!NOTE]
> Do elementu **End** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

#### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EntityContainer** z elementem **AssociationSet** z dwoma elementami **End** :

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitycontainer-element-ssdl"></a>EntityContainer — element (SSDL)

Element **EntityContainer** w języku definicji schematu magazynu (SSDL) opisuje strukturę bazowego źródła danych w aplikacji Entity Framework: zestawy jednostek SSDL (zdefiniowane w elementach EntitySet) reprezentują tabele w bazie danych, typy jednostek SSDL (zdefiniowane w elementach EntityType) reprezentują wiersze w tabeli, a zestawy skojarzeń (zdefiniowane w elementach AssociationSet) reprezentują ograniczenia klucza obcego w bazie danych. Kontener jednostek modelu magazynu mapuje do kontenera jednostek modelu koncepcyjnego za pomocą elementu EntityContainerMapping.

Element **EntityContainer** może mieć zero lub jeden element dokumentacji. Jeśli element **dokumentacji** jest obecny, musi poprzedzać wszystkie inne elementy podrzędne.

Element **EntityContainer** może mieć zero lub więcej następujących elementów podrzędnych (w podanej kolejności):

-   Elementy
-   AssociationSet
-   Elementy adnotacji

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EntityContainer** .

| Nazwa atrybutu | Jest wymagana | Wartość                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa kontenera jednostek. Ta nazwa nie może zawierać kropek (.). |

> [!NOTE]
> Do elementu **EntityContainer** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EntityContainer** , który definiuje dwa zestawy jednostek i jeden zestaw skojarzeń. Należy pamiętać, że typ jednostki i nazwy typów skojarzeń są kwalifikowane według nazwy przestrzeni nazw modelu koncepcyjnego.

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entityset-element-ssdl"></a>EntitySet — element (SSDL)

Element **EntitySet** w języku definicji schematu magazynu (SSDL) reprezentuje tabelę lub widok w źródłowej bazie danych. Element EntityType w SSDL reprezentuje wiersz w tabeli lub widoku. Atrybut **EntityType** elementu **EntitySet** określa konkretny typ jednostki SSDL, który reprezentuje wiersze w zestawie jednostek SSDL. Mapowanie między zestawem jednostek CSDL a zestawem jednostek SSDL jest określone w elemencie elementu EntitySetMapping.

Element **EntitySet** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   DefiningQuery (zero lub jeden element)
-   Elementy adnotacji

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EntitySet** .

> [!NOTE]
> Niektóre atrybuty (niewymienione w tym miejscu) mogą być kwalifikowane aliasem **magazynu** . Te atrybuty są używane przez kreatora aktualizacji modelu podczas aktualizowania modelu.

| Nazwa atrybutu | Jest wymagana | Wartość                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa zestawu jednostek.                                                              |
| **Elementy** | Tak         | W pełni kwalifikowana nazwa typu jednostki, dla którego zestaw jednostek zawiera wystąpienia. |
| **Schemat**     | Nie          | Schemat bazy danych.                                                                     |
| **Tabela**      | Nie          | Tabela bazy danych.                                                                      |

> [!NOTE]
> Do elementu **EntitySet** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EntityContainer** , który ma dwa elementy **EntitySet** i jeden element **AssociationSet** :

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitytype-element-ssdl"></a>EntityType — element (SSDL)

Element **EntityType** w języku definicji schematu magazynu (SSDL) reprezentuje wiersz w tabeli lub widoku źródłowej bazy danych. Element EntitySet w SSDL reprezentuje tabelę lub widok, w którym występują wiersze. Atrybut **EntityType** elementu **EntitySet** określa konkretny typ jednostki SSDL, który reprezentuje wiersze w zestawie jednostek SSDL. Mapowanie między typem jednostki SSDL i typem jednostki CSDL jest określone w elemencie EntityTypeMapping.

Element **EntityType** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Klucz (zero lub jeden element)
-   Elementy adnotacji

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **EntityType** .

| Nazwa atrybutu | Jest wymagana | Wartość                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa typu jednostki. Ta wartość jest zwykle taka sama jak nazwa tabeli, w której typ jednostki reprezentuje wiersz. Ta wartość nie może zawierać kropek (.). |

> [!NOTE]
> Do elementu **EntityType** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EntityType** z dwiema właściwościami:

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="function-element-ssdl"></a>Function — element (SSDL)

Element **Function** w języku definicji schematu magazynu (SSDL) określa procedurę składowaną, która istnieje w źródłowej bazie danych.

Element **Function** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jedna)
-   Parametr (zero lub więcej)
-   CommandText (zero lub jeden)
-   ReturnType (zero lub więcej)
-   Elementy adnotacji (zero lub więcej)

Typ zwracany dla funkcji musi być określony za pomocą elementu **ReturnType** lub atrybutu **ReturnType** (patrz poniżej), ale nie obu.

Procedury składowane, które są określone w modelu magazynu, mogą być importowane do modelu koncepcyjnego aplikacji. Aby uzyskać więcej informacji, zobacz [wykonywanie zapytań przy użyciu procedur składowanych](~/ef6/modeling/designer/stored-procedures/query.md). Element **Function** może również służyć do definiowania funkcji niestandardowych w modelu magazynu.  

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Function** .

> [!NOTE]
> Niektóre atrybuty (niewymienione w tym miejscu) mogą być kwalifikowane aliasem **magazynu** . Te atrybuty są używane przez kreatora aktualizacji modelu podczas aktualizowania modelu.

| Nazwa atrybutu             | Jest wymagana | Wartość                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**                   | Tak         | Nazwa procedury składowanej.                                                                                                                                                                                  |
| **Atrybuty**             | Nie          | Zwracany typ procedury składowanej.                                                                                                                                                                           |
| **Aggregate**              | Nie          | **True** , jeśli procedura składowana zwraca wartość zagregowaną; w przeciwnym razie **false**.                                                                                                                                  |
| **Wbudowan**                | Nie          | **Prawda** , jeśli funkcja jest wbudowaną<sup>1</sup> funkcją; w przeciwnym razie **false**.                                                                                                                                  |
| **StoreFunctionName**      | Nie          | Nazwa procedury składowanej.                                                                                                                                                                                  |
| **NiladicFunction**        | Nie          | **Prawda** , jeśli funkcja jest funkcją niladic<sup>2</sup> ; W przeciwnym razie **zwraca wartość false** .                                                                                                                                   |
| **IsComposable**           | Nie          | **Prawda** , jeśli funkcja jest funkcją składającą się z<sup>3</sup> ; W przeciwnym razie **zwraca wartość false** .                                                                                                                                |
| **ParameterTypeSemantics** | Nie          | Wyliczenie, które definiuje semantykę typu służącą do rozpoznawania przeciążeń funkcji. Wyliczenie jest zdefiniowane w manifeście dostawcy dla definicji funkcji. Wartość domyślna to **AllowImplicitConversion**. |
| **Schemat**                 | Nie          | Nazwa schematu, w którym zdefiniowano procedurę przechowywaną.                                                                                                                                                   |

<sup>1</sup> Wbudowana funkcja jest funkcją, która jest zdefiniowana w bazie danych. Aby uzyskać informacje o funkcjach, które są zdefiniowane w modelu magazynu, zobacz element CommandText (SSDL).

<sup>2</sup> funkcja niladic jest funkcją, która nie akceptuje parametrów i, gdy jest wywoływana, nie wymaga nawiasów.

<sup>3</sup> dwie funkcje są możliwe do przetworzenia, jeśli dane wyjściowe jednej funkcji mogą być danymi wejściowymi dla innej funkcji.

> [!NOTE]
> Do elementu **Function** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **funkcji** , który odnosi się do procedury składowanej **UpdateOrderQuantity** . Procedura składowana akceptuje dwa parametry i nie zwraca wartości.

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="key-element-ssdl"></a>Key — element (SSDL)

Element **Key** w języku definicji schematu magazynu (SSDL) reprezentuje klucz podstawowy tabeli w źródłowej bazie danych. **Klucz** jest elementem podrzędnym elementu EntityType, który reprezentuje wiersz w tabeli. Klucz podstawowy jest zdefiniowany w elemencie **klucza** przez odwołanie do co najmniej jednego elementu właściwości, które są zdefiniowane dla elementu **EntityType** .

Element **Key** może mieć następujące elementy podrzędne (w podanej kolejności):

-   PropertyRef (co najmniej jeden)
-   Elementy adnotacji

Do elementu **Key** nie mają zastosowania żadne atrybuty.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EntityType** z kluczem, który odwołuje się do jednej właściwości:

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="ondelete-element-ssdl"></a>Element onDelete (SSDL)

Element **onDelete** w języku definicji schematu magazynu (SSDL) odzwierciedla zachowanie bazy danych, gdy usuwany jest wiersz, który uczestniczy w ograniczeniu klucza obcego. Jeśli akcja jest ustawiona na **Kaskada**, wiersze odwołujące się do usuwanego wiersza również zostaną usunięte. Jeśli akcja ma wartość **none**, wiersze odwołujące się do wiersza, który jest usuwany, nie są również usuwane. Element **onDelete** jest elementem podrzędnym elementu końcowego.

Element **onDelete** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jedna)
-   Elementy adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **onDelete** .

| Nazwa atrybutu | Jest wymagana | Wartość                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| **Akcja**     | Tak         | **Kaskada** lub **none**. ( **Ograniczona** wartość jest prawidłowa, ale ma takie samo zachowanie jak **none**). |

> [!NOTE]
> Do elementu **onDelete** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **skojarzenia** , który definiuje klucz obcy **\_CustomerOrders** ograniczenie klucza obcego. Element **onDelete** wskazuje, że wszystkie wiersze w tabeli **Orders** , które odwołują się do określonego wiersza w tabeli **Customers** , zostaną usunięte, jeśli wiersz w tabeli **Customers** zostanie usunięty.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="parameter-element-ssdl"></a>Parameter — element (SSDL)

Element **Parameter** w języku definicji schematu magazynu (SSDL) jest elementem podrzędnym elementu Function, który określa parametry procedury składowanej w bazie danych.

Element **Parameter** może mieć następujące elementy podrzędne (w podanej kolejności):

-   Dokumentacja (zero lub jedna)
-   Elementy adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **parametru** .

| Nazwa atrybutu | Jest wymagana | Wartość                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa parametru.                                                                                                                                                                                                      |
| **Typ**       | Tak         | Typ parametru.                                                                                                                                                                                                             |
| **Wyst**       | Nie          | **W**, **out**lub **Inout** , w zależności od tego, czy parametr jest parametrem wejściowym, wyjściowym lub wejściowym.                                                                                                                |
| **MaxLength**  | Nie          | Maksymalna długość parametru.                                                                                                                                                                                            |
| **Dokładne**  | Nie          | Precyzja parametru.                                                                                                                                                                                                 |
| **Zasięgu**      | Nie          | Skala parametru.                                                                                                                                                                                                     |
| **SRID**       | Nie          | Identyfikator odwołania do systemu przestrzennego. Prawidłowe tylko dla parametrów typów przestrzennych. Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

> [!NOTE]
> Do elementu **Parameter** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **funkcji** , który ma dwa elementy **parametrów** , które określają parametry wejściowe:

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="principal-element-ssdl"></a>Element podmiotu zabezpieczeń (SSDL)

Element **Principal** w języku definicji schematu magazynu (SSDL) jest elementem podrzędnym elementu ReferentialConstraint, który definiuje główne zakończenie ograniczenia klucza obcego (nazywanego również ograniczeniem referencyjnym). Element **Principal** określa kolumnę klucza podstawowego (lub kolumny) w tabeli, do której odwołuje się inna kolumna (lub kolumny). Elementy **PropertyRef** określają, do których kolumn odwołują się odwołania. Element zależny określa kolumny, które odwołują się do kolumn klucza podstawowego, które są określone w elemencie **Principal** .

Element **Principal** może mieć następujące elementy podrzędne (w podanej kolejności):

-   PropertyRef (co najmniej jeden)
-   Elementy adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **głównego** .

| Nazwa atrybutu | Jest wymagana | Wartość                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rola**       | Tak         | Taka sama wartość jak atrybut **roli** (jeśli jest używany) odpowiadającego elementu końcowego; w przeciwnym razie nazwa tabeli, która zawiera kolumnę, do której się odwołuje. |

> [!NOTE]
> Do elementu **Principal** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element skojarzenia, który używa elementu **ReferentialConstraint** , aby określić kolumny, które uczestniczą w ograniczeniu klucza obcego **\_CustomerOrders** . Element **Principal** określa kolumnę **IDKlienta** tabeli **Customer** jako główny koniec ograniczenia.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="property-element-ssdl"></a>Property — element (SSDL)

Element **Property** w języku definicji schematu magazynu (SSDL) reprezentuje kolumnę w tabeli w źródłowej bazie danych. Elementy **Właściwości** są elementami podrzędnymi elementów EntityType, które reprezentują wiersze w tabeli. Każdy element **Właściwości** zdefiniowany dla elementu **EntityType** reprezentuje kolumnę.

Element **Właściwości** nie może mieć żadnych elementów podrzędnych.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Właściwości** .

| Nazwa atrybutu            | Jest wymagana | Wartość                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**                  | Tak         | Nazwa odpowiedniej kolumny.                                                                                                                                                                                           |
| **Typ**                  | Tak         | Typ odpowiadającej kolumny.                                                                                                                                                                                           |
| **Wymaga**              | Nie          | Wartość **true** (wartość domyślna) lub **Fałsz** w zależności od tego, czy odpowiadająca kolumna może mieć wartość null.                                                                                                                  |
| **DefaultValue**          | Nie          | Wartość domyślna odpowiedniej kolumny.                                                                                                                                                                                  |
| **MaxLength**             | Nie          | Maksymalna długość odpowiedniej kolumny.                                                                                                                                                                                 |
| **FixedLength**           | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy odpowiadająca wartość kolumny będzie przechowywana jako ciąg o stałej długości.                                                                                                              |
| **Dokładne**             | Nie          | Precyzja odpowiadającej kolumny.                                                                                                                                                                                      |
| **Zasięgu**                 | Nie          | Skala odpowiadającej kolumny.                                                                                                                                                                                          |
| **Unicode**               | Nie          | **Prawda** lub **Fałsz** w zależności od tego, czy odpowiadająca wartość kolumny będzie przechowywana jako ciąg Unicode.                                                                                                                   |
| **Sortowanie**             | Nie          | Ciąg określający sekwencję sortowania, która ma być używana w źródle danych.                                                                                                                                                   |
| **SRID**                  | Nie          | Identyfikator odwołania do systemu przestrzennego. Prawidłowe tylko dla właściwości typów przestrzennych. Aby uzyskać więcej informacji, zobacz [SRID](https://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **StoreGeneratedPattern** | Nie          | **Brak**, **tożsamość** (jeśli odpowiadająca wartość kolumny jest tożsamością wygenerowaną w bazie danych) lub **obliczana** (jeśli odpowiadająca wartość kolumny jest obliczana w bazie danych). Nieprawidłowy dla właściwości RowType. |

> [!NOTE]
> Do elementu **Property** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **EntityType** z dwoma podrzędnymi elementami **Właściwości** :

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="propertyref-element-ssdl"></a>PropertyRef — element (SSDL)

Element **PropertyRef** w języku definicji schematu magazynu (SSDL) odwołuje się do właściwości zdefiniowanej w elemencie EntityType, aby wskazać, że właściwość wykona jedną z następujących ról:

-   Należy do klucza podstawowego tabeli, która reprezentuje element **EntityType** . Aby zdefiniować klucz podstawowy, można użyć co najmniej jednego elementu **PropertyRef** . Aby uzyskać więcej informacji, zobacz klucz element.
-   Być zależnym lub głównym końcem ograniczenia referencyjnego. Aby uzyskać więcej informacji, zobacz ReferentialConstraint element.

Element **PropertyRef** może mieć tylko następujące elementy podrzędne:

-   Dokumentacja (zero lub jedna)
-   Elementy adnotacji

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **PropertyRef** .

| Nazwa atrybutu | Jest wymagana | Wartość                                |
|:---------------|:------------|:-------------------------------------|
| **Nazwa**       | Tak         | Nazwa właściwości, której dotyczy odwołanie. |

> [!NOTE]
> Do elementu **PropertyRef** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla CSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład przedstawia element **PropertyRef** służący do definiowania klucza podstawowego przez odwołanie do właściwości, która jest zdefiniowana dla elementu **EntityType** .

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="referentialconstraint-element-ssdl"></a>ReferentialConstraint — element (SSDL)

Element **ReferentialConstraint** w języku definicji schematu magazynu (SSDL) reprezentuje ograniczenie klucza obcego (nazywane również ograniczeniem integralności referencyjnej) w źródłowej bazie danych. Podmiot zabezpieczeń i zależne zakończenia ograniczenia są określane odpowiednio przez główne i zależne elementy podrzędne. Kolumny, które uczestniczą w głównej i zależnych końcach, są przywoływane z elementami PropertyRef.

Element **ReferentialConstraint** jest opcjonalnym elementem podrzędnym elementu Association. Jeśli element **ReferentialConstraint** nie jest używany do mapowania ograniczenia FOREIGN KEY, które jest określone w elemencie **Association** , do tego celu należy użyć elementu AssociationSetMapping.

Element **ReferentialConstraint** może mieć następujące elementy podrzędne:

-   Dokumentacja (zero lub jedna)
-   Podmiot zabezpieczeń (dokładnie jeden)
-   Zależne (dokładnie jeden)
-   Elementy adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Do elementu **ReferentialConstraint** można zastosować dowolną liczbę atrybutów adnotacji (niestandardowych atrybutów XML). Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **skojarzenia** , który używa elementu **ReferentialConstraint** , aby określić kolumny, które uczestniczą w ograniczeniu klucza obcego **\_CustomerOrders** :

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="returntype-element-ssdl"></a>ReturnType — element (SSDL)

Element **ReturnType** w języku definicji schematu magazynu (SSDL) Określa zwracany typ dla funkcji, która jest zdefiniowana w elemencie **Function** . Typ zwracany funkcji można także określić przy użyciu atrybutu **ReturnValue** .

Zwracany typ funkcji jest określany przy użyciu atrybutu **Type** lub **ReturnType** .

Element **ReturnType** może mieć następujące elementy podrzędne:

- CollectionType (jeden)  

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowych atrybutów XML) może zostać zastosowana do elementu **ReturnType** . Jednak atrybuty niestandardowe nie mogą należeć do żadnej przestrzeni nazw XML zarezerwowanej dla SSDL. W pełni kwalifikowane nazwy dla wszystkich dwóch atrybutów niestandardowych nie mogą być takie same.

### <a name="example"></a>Przykład

W poniższym przykładzie zastosowano **funkcję** zwracającą kolekcję wierszy.

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```


## <a name="rowtype-element-ssdl"></a>RowType — element (SSDL)

Element **RowType** w języku definicji schematu magazynu (SSDL) definiuje nienazwaną strukturę jako zwracany typ dla funkcji zdefiniowanej w sklepie.

Element **RowType** jest elementem podrzędnym elementu **CollectionType** :

Element **RowType** może mieć następujące elementy podrzędne:

- Właściwość (co najmniej jedna)  

### <a name="example"></a>Przykład

Poniższy przykład pokazuje funkcję magazynu, która używa elementu **CollectionType** , aby określić, że funkcja zwraca kolekcję wierszy (jak określono w elemencie **RowType** ).


``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="schema-element-ssdl"></a>Element schematu (SSDL)

Element **schematu** w języku definicji schematu magazynu (SSDL) jest elementem głównym definicji modelu magazynu. Zawiera definicje obiektów, funkcji i kontenerów, które tworzą model magazynu.

Element **schematu** może zawierać zero lub więcej z następujących elementów podrzędnych:

-   Skojarzenie
-   Typ entityType
-   EntityContainer
-   Funkcja

Element **schematu** używa atrybutu **przestrzeni nazw** w celu zdefiniowania przestrzeni nazw dla obiektów typ jednostki i skojarzenie w modelu magazynu. W przestrzeni nazw żadne dwa obiekty nie mogą mieć takiej samej nazwy.

Przestrzeń nazw modelu magazynu różni się od przestrzeni nazw XML elementu **Schema** . Przestrzeń nazw modelu magazynu (zgodnie z definicją atrybutu **Namespace** ) jest kontenerem logicznym dla typów jednostek i typów skojarzeń. Przestrzeń nazw XML (wskazywana przez atrybut **xmlns** ) elementu **schematu** jest domyślną przestrzenią nazw dla elementów podrzędnych i atrybutów elementu **Schema** . Przestrzenie nazw XML w formularzu https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (gdzie RRRR i MM reprezentują odpowiednio rok i miesiąc) są zarezerwowane dla SSDL. Elementy niestandardowe i atrybuty nie mogą znajdować się w przestrzeniach nazw, które mają ten formularz.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które można zastosować do elementu **Schema** .

| Nazwa atrybutu            | Jest wymagana | Wartość                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**             | Tak         | Przestrzeń nazw modelu magazynu. Wartość atrybutu **Namespace** służy do tworzenia w pełni kwalifikowanej nazwy typu. Na przykład jeśli obiekt **EntityType** o nazwie *Customer* znajduje się w przestrzeni nazw ExampleModel. Store, a następnie w pełni kwalifikowana nazwa **obiektu EntityType** to ExampleModel. Store. Customer. <br/> Następujące ciągi nie mogą być używane jako wartość atrybutu **Namespace** : **system**, **przejściowy**lub **EDM**. Wartość atrybutu **przestrzeni nazw** nie może być taka sama jak wartość atrybutu **Namespace** w elemencie schematu CSDL. |
| **Alias**                 | Nie          | Identyfikator używany zamiast nazwy przestrzeni nazw. Na przykład jeśli obiekt **EntityType** o nazwie *Customer* znajduje się w przestrzeni nazw ExampleModel. Store, a wartość atrybutu **aliasu** to *StorageModel*, można użyć StorageModel. Customer jako w pełni kwalifikowanej nazwy elementu **EntityType.**                                                                                                                                                                                                                                                                                    |
| **Dostawca**              | Tak         | Dostawca danych.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **ProviderManifestToken** | Tak         | Token wskazujący dostawcy, który manifest dostawcy ma zwrócić. Nie zdefiniowano żadnego formatu dla tokenu. Wartości dla tokenu są definiowane przez dostawcę. Aby uzyskać informacje na temat tokenów manifestu dostawcy SQL Server, zobacz SqlClient dla Entity Framework.                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element **schematu** , który zawiera element **EntityContainer** , dwa elementy **EntityType** i jeden element **skojarzenia** .

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="https://schemas.microsoft.com/ado/2009/11/edm/ssdl">
   <EntityContainer Name="ExampleModelStoreContainer">
     <EntitySet Name="Customers"
                EntityType="ExampleModel.Store.Customers"
                Schema="dbo" />
     <EntitySet Name="Orders"
                EntityType="ExampleModel.Store.Orders"
                Schema="dbo" />
     <AssociationSet Name="FK_CustomerOrders"
                     Association="ExampleModel.Store.FK_CustomerOrders">
       <End Role="Customers" EntitySet="Customers" />
       <End Role="Orders" EntitySet="Orders" />
     </AssociationSet>
   </EntityContainer>
   <EntityType Name="Customers">
     <Documentation>
       <Summary>Summary here.</Summary>
       <LongDescription>Long description here.</LongDescription>
     </Documentation>
     <Key>
       <PropertyRef Name="CustomerId" />
     </Key>
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
   </EntityType>
   <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
     <Key>
       <PropertyRef Name="OrderId" />
     </Key>
     <Property Name="OrderId" Type="int" Nullable="false"
               c:CustomAttribute="someValue"/>
     <Property Name="ProductId" Type="int" Nullable="false" />
     <Property Name="Quantity" Type="int" Nullable="false" />
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <c:CustomElement>
       Custom data here.
     </c:CustomElement>
   </EntityType>
   <Association Name="FK_CustomerOrders">
     <End Role="Customers"
          Type="ExampleModel.Store.Customers" Multiplicity="1">
       <OnDelete Action="Cascade" />
     </End>
     <End Role="Orders"
          Type="ExampleModel.Store.Orders" Multiplicity="*" />
     <ReferentialConstraint>
       <Principal Role="Customers">
         <PropertyRef Name="CustomerId" />
       </Principal>
       <Dependent Role="Orders">
         <PropertyRef Name="CustomerId" />
       </Dependent>
     </ReferentialConstraint>
   </Association>
   <Function Name="UpdateOrderQuantity"
             Aggregate="false"
             BuiltIn="false"
             NiladicFunction="false"
             IsComposable="false"
             ParameterTypeSemantics="AllowImplicitConversion"
             Schema="dbo">
     <Parameter Name="orderId" Type="int" Mode="In" />
     <Parameter Name="newQuantity" Type="int" Mode="In" />
   </Function>
   <Function Name="UpdateProductInOrder" IsComposable="false">
     <CommandText>
       UPDATE Orders
       SET ProductId = @productId
       WHERE OrderId = @orderId;
     </CommandText>
     <Parameter Name="productId"
                Mode="In"
                Type="int"/>
     <Parameter Name="orderId"
                Mode="In"
                Type="int"/>
   </Function>
 </Schema>
```

## <a name="annotation-attributes"></a>Atrybuty adnotacji

Atrybuty adnotacji w języku definicji schematu magazynu (SSDL) to niestandardowe atrybuty XML w modelu magazynu, które zapewniają dodatkowe metadane dotyczące elementów w modelu magazynu. Oprócz posiadania prawidłowej struktury XML, następujące ograniczenia mają zastosowanie do atrybutów adnotacji:

-   Atrybuty adnotacji nie mogą znajdować się w żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.
-   W pełni kwalifikowane nazwy jakichkolwiek dwóch atrybutów adnotacji nie mogą być takie same.

Do danego elementu SSDL można zastosować więcej niż jeden atrybut adnotacji. Do metadanych zawartych w elementach adnotacji można uzyskać dostęp w czasie wykonywania przy użyciu klas w przestrzeni nazw System. Data. Metadata. Edm.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element EntityType, który ma atrybut adnotacji zastosowany do właściwości **IDZamówienia** . W przykładzie pokazano również, że element adnotacji został dodany do elementu **EntityType** .

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="annotation-elements-ssdl"></a>Elementy adnotacji (SSDL)

Elementy adnotacji w języku definicji schematu magazynu (SSDL) to niestandardowe elementy XML w modelu magazynu, które zapewniają dodatkowe metadane dotyczące modelu magazynu. Oprócz posiadania prawidłowej struktury XML, następujące ograniczenia dotyczą elementów adnotacji:

-   Elementy adnotacji nie mogą znajdować się w żadnej przestrzeni nazw XML zarezerwowanej dla SSDL.
-   W pełni kwalifikowane nazwy jakichkolwiek dwóch elementów adnotacji nie mogą być takie same.
-   Elementy adnotacji muszą występować po wszystkich innych elementach podrzędnych danego elementu SSDL.

Więcej niż jeden element adnotacji może być elementem podrzędnym danego elementu SSDL. Począwszy od .NET Framework w wersji 4, metadane zawarte w elementach adnotacji są dostępne w czasie wykonywania przy użyciu klas w przestrzeni nazw System. Data. Metadata. Edm.

### <a name="example"></a>Przykład

Poniższy przykład pokazuje element EntityType, który ma element adnotacji (**CustomElement**). W przykładzie przedstawiono również atrybut adnotacji zastosowany do właściwości **IDZamówienia** .

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="facets-ssdl"></a>Aspekty (SSDL)

Zestawy reguł w języku definicji schematu magazynu (SSDL) przedstawiają ograniczenia dotyczące typów kolumn, które są określone w elementach właściwości. Zestawy reguł są implementowane jako atrybuty XML w elementach **Właściwości** .

W poniższej tabeli opisano aspekty, które są obsługiwane w programie SSDL:

| Aspekcie           | Opis                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Sortowanie**   | Określa sekwencję sortowania (lub sekwencję sortowania), która ma być używana podczas wykonywania operacji porównania i porządkowania na wartościach właściwości.                                                                                                             |
| **FixedLength** | Określa, czy długość wartości kolumny może się różnić.                                                                                                                                                                                                  |
| **MaxLength**   | Określa maksymalną długość wartości kolumny.                                                                                                                                                                                                           |
| **Dokładne**   | Dla właściwości typu **Decimal**określa liczbę cyfr, jaką może mieć wartość właściwości. Dla właściwości typu **Time**, **DateTime**i **DateTimeOffset**określa liczbę cyfr ułamkowych części sekundy wartości kolumny. |
| **Zasięgu**       | Określa liczbę cyfr z prawej strony punktu dziesiętnego dla wartości kolumny.                                                                                                                                                                      |
| **Unicode**     | Wskazuje, czy wartość kolumny jest przechowywana w formacie Unicode.                                                                                                                                                                                                    |
