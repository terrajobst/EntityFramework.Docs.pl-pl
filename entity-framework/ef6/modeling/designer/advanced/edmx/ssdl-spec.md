---
title: Specyfikacja SSDL - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: 35c560d88e5078a7fc4c07b76020f3ad7d0735e1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995282"
---
# <a name="ssdl-specification"></a>Specyfikacja SSDL
Język definicji schematu Store (SSDL) to oparty na standardzie XML język, który opisuje model magazynu w aplikacji Entity Framework.

W aplikacji Entity Framework metadanych modelu magazynu jest ładowany z pliku ssdl (napisanego w SSDL) do wystąpienia System.Data.Metadata.Edm.StoreItemCollection i są dostępne za pomocą metody Klasa System.Data.Metadata.Edm.MetadataWorkspace. Entity Framework używa magazynu metadanych modelu do przekształcania zapytania względem modelu koncepcyjnego polecenia specyficzne dla magazynu.

Entity Framework Designer (Projektant EF) przechowuje informacje o modelu magazynu w pliku edmx w czasie projektowania. W czasie kompilacji w Projektancie jednostki używa informacji w pliku edmx można utworzyć pliku ssdl, które są wymagane przez Entity Framework w czasie wykonywania.

Wersje SSDL są zróżnicowane według przestrzeni nazw XML.

| Wersja SSDL | Namespace XML                                     |
|:-------------|:--------------------------------------------------|
| SSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| SSDL v2      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| SSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a>Elementu Association (SSDL)

**Skojarzenia** element język definicji schematu magazynu (SSDL) określa kolumny tabeli, które uczestniczą w ograniczenie klucza obcego w bazie danych. Dwa elementy End wymagany element podrzędny Określ tabele końców skojarzenie i liczebność na każdym końcu. Opcjonalny element ReferentialConstraint określa głównym i zależnym końcach asocjacji, a także kolumn uczestniczących w programie. Jeśli nie **ReferentialConstraint** elementu, AssociationSetMapping element może służyć do określania mapowań kolumn dla skojarzenia.

**Skojarzenia** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden)
-   Końcowy (dokładnie dwa)
-   ReferentialConstraint (zero lub jeden)
-   Elementów adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **skojarzenia** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa odpowiedniego ograniczenie klucza obcego w bazie danych. |

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **skojarzenia** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **skojarzenia** element, który używa **ReferentialConstraint** elementu, aby określić kolumny, które uczestniczą w **klucza Obcego\_CustomerOrders**  ograniczenie klucza obcego:

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

## <a name="associationset-element-ssdl"></a>Obiekt AssociationSet — Element (SSDL)

**AssociationSet** element język definicji schematu magazynu (SSDL) reprezentuje ograniczenie klucza obcego między dwiema tabelami w bazie danych. Kolumny tabeli, które uczestniczą w ograniczenie klucza obcego są określone w elemencie Association. **Skojarzenia** element, który odpowiada danej **AssociationSet** element jest określony w **skojarzenia** atrybutu **AssociationSet**  elementu.

SSDL zestawów skojarzeń są mapowane do zestawów skojarzeń CSDL przez AssociationSetMapping element. Jednak jeśli zestawu skojarzeń CSDL dla danego skojarzenia CSDL jest zdefiniowana za pomocą elementu ReferentialConstraint bez odpowiedniej **AssociationSetMapping** elementu jest konieczne. W takim przypadku **AssociationSetMapping** elementu, zostanie ona zastąpiona mapowania definiuje **ReferentialConstraint** elementu.

**AssociationSet** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden)
-   END (zero lub dwóch)
-   Elementów adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **AssociationSet** elementu.

| Nazwa atrybutu  | Jest wymagany | Wartość                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| **Nazwa**        | Tak         | Nazwa ograniczenia klucza obcego, że skojarzenie ustawione reprezentuje.                          |
| **Skojarzenie** | Tak         | Nazwa skojarzenia, który definiuje kolumny, które uczestniczą w ograniczenie klucza obcego. |

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **AssociationSet** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **AssociationSet** elementu, który reprezentuje `FK_CustomerOrders` ograniczenie klucza obcego w bazie danych:

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a>Element CollectionType (SSDL)

**CollectionType** element język definicji schematu magazynu (SSDL) określa, że typ zwracany przez funkcję jest kolekcją. **CollectionType** element jest elementem podrzędnym elementu ReturnType. Typ kolekcji jest określony za pomocą elementu podrzędnego RowType:

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **CollectionType** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano funkcję, która używa **CollectionType** elementu, aby określić, że funkcja zwraca Kolekcja wierszy.

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

## <a name="commandtext-element-ssdl"></a>Element CommandText (SSDL)

**CommandText** element w język definicji schematu magazynu (SSDL) jest elementem podrzędnym elementu funkcji, która pozwala na zdefiniowanie instrukcji SQL, który jest wykonywany w bazie danych. **CommandText** elementu pozwala na dodawanie funkcji, która jest podobna do procedury składowanej w bazie danych, ale należy zdefiniować **CommandText** elementu w modelu magazynu.

**CommandText** elementu nie może mieć elementów podrzędnych. Treść **CommandText** element musi być prawidłową instrukcję SQL do podstawowej bazy danych.

Atrybuty nie są stosowane do **CommandText** elementu.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **funkcja** element z element podrzędny **CommandText** elementu. Udostępnianie **UpdateProductInOrder** działać jako metodę dla obiektu ObjectContext importując go do modelu koncepcyjnego.  

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

## <a name="definingquery-element-ssdl"></a>Element DefiningQuery (SSDL)

**DefiningQuery** element język definicji schematu magazynu (SSDL) pozwala na wykonanie instrukcji SQL bezpośrednio w bazie danych. **DefiningQuery** element jest najczęściej używany jak widok bazy danych, ale widok jest zdefiniowany w modelu pamięci masowej zamiast bazy danych. Widok zdefiniowany w **DefiningQuery** elementu mogą być mapowane na typ jednostki w modelu koncepcyjnym za pośrednictwem elementu obiekcie EntitySetMapping. Te mapowania są przeznaczone tylko do odczytu.  

Poniższa składnia SSDL pokazuje deklaracji **EntitySet** następuje **DefiningQuery** element, który zawiera zapytanie służy do pobierania tego widoku.

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

Aby włączyć scenariuszach odczytu i zapisu za pośrednictwem widoków, można użyć procedur składowanych platformy Entity Framework. Widok źródła danych lub widoku SQL jednostki można użyć jako tabeli podstawowej dla pobierania danych i przetwarzania przez procedury składowane zmiany.

Możesz użyć **DefiningQuery** elementu docelowego programu Microsoft SQL Server Compact 3.5. Chociaż program SQL Server Compact 3.5 nie obsługuje procedur przechowywanych, można zaimplementować podobne funkcje za pomocą **DefiningQuery** elementu. Jest innym miejscu, gdzie mogą być przydatne podczas tworzenia procedur składowanych do pokonania niezgodność typów danych, używany w języku programowania i te źródła danych. Można napisać **DefiningQuery** które pobiera zestaw parametrów, a następnie wywołuje procedurę składowaną z innym zestawem parametrów, na przykład procedury przechowywanej, która powoduje usunięcie danych.

## <a name="dependent-element-ssdl"></a>Element zależne (SSDL)

**Zależne** element język definicji schematu magazynu (SSDL) jest element podrzędny do elementu ReferentialConstraint, który definiuje zależne koniec ograniczenie klucza obcego (nazywany także ograniczenie referencyjne). **Zależne** element określa kolumny (lub kolumny) w tabeli, która będzie odwoływać się do kolumny klucza podstawowego (lub kolumny). **PropertyRef** elementy Określ kolumny, które są wywoływane. Główny element określa kolumny klucza podstawowego, które są przywoływane przez kolumn, które są określone w **zależne** elementu.

**Zależne** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   PropertyRef (jeden lub więcej)
-   Elementów adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **zależne** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rola**       | Tak         | Taką samą wartość jak **roli** atrybut odpowiedni element End (jeśli jest używana); w przeciwnym razie nazwę tabeli, która zawiera kolumna źródłowa odwołania. |

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **zależne** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano element Association używający **ReferentialConstraint** elementu, aby określić kolumny, które uczestniczą w **klucza Obcego\_CustomerOrders** klucza obcego ograniczenie. **Zależne** element Określa **CustomerId** kolumny **kolejności** tabeli jako zależne koniec tego ograniczenia.

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

## <a name="documentation-element-ssdl"></a>Element documentation (SSDL)

**Dokumentacji** element język definicji schematu magazynu (SSDL) może służyć do Podaj informacje dotyczące obiektu, który jest zdefiniowany w elemencie rodzica.

**Dokumentacji** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   **Podsumowanie**: Krótki opis elementu nadrzędnego. (element zero lub jeden)
-   **LongDescription**: rozbudowany opis elementu nadrzędnego. (element zero lub jeden)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **dokumentacji** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **dokumentacji** element jako element podrzędny elementu EntityType.

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

## <a name="end-element-ssdl"></a>Element end (SSDL)

**Zakończenia** element język definicji schematu magazynu (SSDL) Określa tabelę i liczba wierszy na jednym końcu ograniczenie klucza obcego w bazie danych. **Zakończenia** element może być elementem podrzędnym elementu Association lub elementu AssociationSet. W każdym przypadku możliwe podrzędnych elementów i atrybuty stosowane są różne.

### <a name="end-element-as-a-child-of-the-association-element"></a>Końcowy Element jako element podrzędny elementu Association

**Zakończenia** — element (jako element podrzędny elementu **skojarzenia** elementu) Określa tabelę i liczba wierszy na końcu ograniczenie klucza obcego z **typu** i **Liczebność** odpowiednio atrybutów. Koniec ograniczenie klucza obcego są zdefiniowane jako część skojarzenia SSDL; skojarzenie SSDL musi mieć dokładnie punktów końcowych.

**Zakończenia** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   OnDelete (zero lub jeden element)
-   Elementów adnotacji (zero lub więcej elementów)

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **zakończenia** elementu, gdy jest elementem podrzędnym elementu **skojarzenia** elementu.

| Nazwa atrybutu   | Jest wymagany | Wartość                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**         | Tak         | W pełni kwalifikowana nazwa zestawu jednostek SSDL, który znajduje się na końcu ograniczenie klucza obcego.                                                                                                                                                                                                                                                                                          |
| **Rola**         | Nie          | Wartość **roli** atrybutu w elemencie jednostki albo zależnych od odpowiadającego im elementu ReferentialConstraint (jeśli są używane).                                                                                                                                                                                                                                             |
| **Liczebność** | Tak         | **1**, **od 0 do 1**, lub **\*** w zależności od liczby wierszy, które mogą być na końcu ograniczenie klucza obcego. <br/> **1** wskazuje, że dokładnie jeden wiersz istnieje na końcu ograniczenie klucza obcego. <br/> **od 0 do 1** wskazuje, że istnieje zero lub jeden wiersz na końcu ograniczenie klucza obcego. <br/> **\*** oznacza, że wartość zero, jeden lub więcej wierszy na końcu ograniczenie klucza obcego. |

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **zakończenia** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

#### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **skojarzenia** element, który definiuje **klucza Obcego\_CustomerOrders** ograniczenie klucza obcego. **Liczebność** wartościom określonym w każdym **zakończenia** elementu wskazują tę liczbę wierszy w **zamówienia** tabeli może być skojarzony z wiersza w **klientów**  tabeli, ale tylko jeden wiersz w **klientów** tabeli może być skojarzony z wiersza w **zamówienia** tabeli. Ponadto **OnDelete** element wskazuje, że wszystkie wiersze, w **zamówienia** odwoływać się do danego wiersza w tabeli **klientów** tabeli zostanie usunięte, gdy wiersz **klientów** tabeli zostanie usunięty.

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Końcowy Element jako element podrzędny elementu AssociationSet

**Zakończenia** — element (jako element podrzędny elementu **AssociationSet** elementu) Określa tabelę na jednym końcu ograniczenie klucza obcego w źródłowej bazie danych.

**Zakończenia** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden)
-   Elementów adnotacji (zero lub więcej)

#### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **zakończenia** elementu, gdy jest elementem podrzędnym elementu **AssociationSet** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| **Obiekt EntitySet**  | Tak         | Nazwa zestawu jednostek SSDL, który znajduje się na końcu ograniczenie klucza obcego.                                      |
| **Rola**       | Nie          | Wartość jednej z **roli** atrybuty określone w jednym **zakończenia** elementu odpowiednie skojarzenia. |

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **zakończenia** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

#### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EntityContainer** element z **AssociationSet** elementu przy użyciu dwóch **zakończenia** elementy:

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

## <a name="entitycontainer-element-ssdl"></a>Element EntityContainer (SSDL)

**EntityContainer** element język definicji schematu magazynu (SSDL) zawiera opis struktury źródła danych w aplikacji Entity Framework: zestawy jednostek SSDL (zdefiniowanymi w elementów EntitySet) reprezentują tabele w bazy danych, typy jednostek SSDL (zdefiniowany w elementach typu EntityType) reprezentują wierszy w tabeli i zestawów skojarzeń (zdefiniowany w obiekcie AssociationSet elementy) reprezentują ograniczenia klucza obcego w bazie danych. Kontener jednostek modelu magazynu jest mapowany na model koncepcyjny kontener jednostek, za pośrednictwem elementu w elemencie EntityContainerMapping.

**EntityContainer** element może mieć zero lub jeden elementów dokumentacji. Jeśli **dokumentacji** element jest obecny, musi poprzedzać wszystkie inne elementy podrzędne.

**EntityContainer** element może mieć zero lub więcej z następujących elementów podrzędnych (w podanej kolejności):

-   Obiekt EntitySet
-   Obiekt AssociationSet
-   Elementów adnotacji

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **EntityContainer** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa kontenera jednostek. Ta nazwa nie może zawierać kropek (.). |

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **EntityContainer** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EntityContainer** element, który definiuje dwa zestawy jednostek i jedno skojarzenie zestawu. Należy zauważyć, że nazwy jednostki typu i skojarzenia typów są kwalifikowana przez nazwę przestrzeni nazw modelu koncepcyjnego.

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

## <a name="entityset-element-ssdl"></a>Element EntitySet (SSDL)

**EntitySet** element język definicji schematu magazynu (SSDL) reprezentuje tabelę lub widok w bazie danych. Element EntityType SSDL reprezentuje wiersz w tabeli lub widoku. **EntityType** atrybutu **EntitySet** element określa typ jednostki SSDL konkretnego, który reprezentuje wierszy w zestawie jednostek SSDL. Mapowanie między zestaw jednostek CSDL a SSDL zestaw jednostek jest określona w obiekcie EntitySetMapping elementu.

**EntitySet** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   DefiningQuery (zero lub jeden element)
-   Elementów adnotacji

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **EntitySet** elementu.

> [!NOTE]
> Niektóre atrybuty (niewymienione w tym) może być kwalifikowana za pomocą **przechowywania** aliasu. Te atrybuty są używane przez kreatora Model aktualizacji, podczas aktualizowania modelu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa zestawu jednostek.                                                              |
| **Typ entityType** | Tak         | W pełni kwalifikowana nazwa typu jednostki, dla której zestaw jednostek zawiera wystąpienia. |
| **Schemat**     | Nie          | Schemat bazy danych.                                                                     |
| **Tabela**      | Nie          | Tabela bazy danych.                                                                      |

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **EntitySet** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EntityContainer** element, który ma dwa **EntitySet** elementy i jeden **AssociationSet** elementu:

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

## <a name="entitytype-element-ssdl"></a>Element EntityType (SSDL)

**EntityType** element język definicji schematu magazynu (SSDL) reprezentuje wiersz w tabeli lub widoku bazy danych. Element EntitySet SSDL reprezentuje tabelę lub widok, w którym występują wiersze. **EntityType** atrybutu **EntitySet** element określa typ jednostki SSDL konkretnego, który reprezentuje wierszy w zestawie jednostek SSDL. Mapowanie między typem encji SSDL i typ jednostki CSDL jest określony w elemencie EntityTypeMapping.

**EntityType** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden element)
-   Klucz (zero lub jeden element)
-   Elementów adnotacji

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **EntityType** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa typu jednostki. Ta wartość jest zwykle taka sama jak nazwa tabeli, w którym typ jednostki reprezentuje wiersz. Ta wartość może zawierać nie kropki (.). |

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **EntityType** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EntityType** elementu o dwie właściwości:

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

## <a name="function-element-ssdl"></a>Function — Element (SSDL)

**Funkcja** element język definicji schematu magazynu (SSDL) określa procedury przechowywanej, która istnieje w bazie danych.

**Funkcja** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden)
-   Parametr (zero lub więcej)
-   CommandText (zero lub jeden)
-   ReturnType (zero lub więcej)
-   Elementów adnotacji (zero lub więcej)

Zwracana typ dla funkcji, należy określić albo **ReturnType** element lub **ReturnType** atrybutu (patrz poniżej), ale nie oba.

Procedury składowane, które są określone w modelu magazynu można zaimportować do modelu koncepcyjnego aplikacji. Aby uzyskać więcej informacji, zobacz [wykonywanie zapytań za pomocą procedur składowanych](~/ef6/modeling/designer/stored-procedures/query.md). **Funkcja** elementu może również służyć do definiowania funkcji niestandardowych w modelu magazynu.  

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **funkcja** elementu.

> [!NOTE]
> Niektóre atrybuty (niewymienione w tym) może być kwalifikowana za pomocą **przechowywania** aliasu. Te atrybuty są używane przez kreatora Model aktualizacji, podczas aktualizowania modelu.

| Nazwa atrybutu             | Jest wymagany | Wartość                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**                   | Tak         | Nazwa procedury składowanej.                                                                                                                                                                                  |
| **ReturnType**             | Nie          | Zwracany typ procedury składowanej.                                                                                                                                                                           |
| **Aggregate**              | Nie          | **Wartość true,** Jeśli procedura składowana ma zwracać wartości zagregowanej; w przeciwnym razie **False**.                                                                                                                                  |
| **Wbudowane**                | Nie          | **Wartość true,** Jeśli funkcja jest wbudowaną<sup>1</sup> funkcji; w przeciwnym razie **False**.                                                                                                                                  |
| **StoreFunctionName**      | Nie          | Nazwa procedury składowanej.                                                                                                                                                                                  |
| **NiladicFunction**        | Nie          | **Wartość true,** Jeśli funkcja znajduje się bez parametrów<sup>2</sup> funkcji; **False** inaczej.                                                                                                                                   |
| **IsComposable**           | Nie          | **Wartość true,** Jeśli funkcja jest konfigurowalna<sup>3</sup> funkcji; **False** inaczej.                                                                                                                                |
| **ParameterTypeSemantics** | Nie          | Wyliczenie, które definiuje semantyka typów, używany do rozpoznawania przeciążenia funkcji. Wyliczenia jest zdefiniowany w manifeście dostawcy dla definicji funkcji. Wartość domyślna to **AllowImplicitConversion**. |
| **Schemat**                 | Nie          | Nazwa schematu, w którym zdefiniowano procedury składowanej.                                                                                                                                                   |

<sup>1</sup> wbudowana funkcja jest funkcją, która jest zdefiniowana w bazie danych. Aby uzyskać informacje na temat funkcji, które są zdefiniowane w modelu magazynu Zobacz Element CommandText (SSDL).

<sup>2</sup> funkcję bez parametrów jest funkcją, która przyjmuje żadnych parametrów i, gdy zostanie wywołana, nie wymaga nawiasów.

<sup>3</sup> dwie funkcje są konfigurowalna, jeśli jedna funkcja może zwracać dane wejściowe dla innych funkcji.

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **funkcja** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **funkcja** element, który odpowiada **UpdateOrderQuantity** procedury składowanej. Procedura składowana akceptuje dwa parametry i zwracać wartości.

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

## <a name="key-element-ssdl"></a>Kluczowym elementem (SSDL)

**Klucz** element język definicji schematu magazynu (SSDL) reprezentuje klucz podstawowy tabeli w bazie danych. **Klucz** jest elementem podrzędnym elementu EntityType, który reprezentuje wiersz w tabeli. Klucz podstawowy jest zdefiniowany w **klucz** elementu odwołując się do elementów właściwości, które są zdefiniowane na **EntityType** elementu.

**Klucz** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   PropertyRef (jeden lub więcej)
-   Elementów adnotacji

Atrybuty nie są stosowane do **klucz** elementu.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EntityType** element z kluczem, który odwołuje się do jednej właściwości:

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

## <a name="ondelete-element-ssdl"></a>Element OnDelete (SSDL)

**OnDelete** element język definicji schematu magazynu (SSDL) odzwierciedla zachowanie bazy danych, po usunięciu wiersza, który uczestniczy w ograniczenie klucza obcego. Jeśli ustawiono akcję **Cascade**, a następnie również zostaną usunięte wiersze, które odwołują się wiersz, który jest usuwany. Jeśli ustawiono akcję **Brak**, a następnie nie zostaną również usunięte wiersze, które odwołują się wiersz, który jest usuwany. **OnDelete** element jest elementem podrzędnym elementu End.

**OnDelete** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden)
-   Elementów adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **OnDelete** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| **Akcja**     | Tak         | **Kaskadowe** lub **Brak**. (Wartość **ograniczeniami** jest prawidłowy, ale ma takie samo zachowanie jako **Brak**.) |

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **OnDelete** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **skojarzenia** element, który definiuje **klucza Obcego\_CustomerOrders** ograniczenie klucza obcego. **OnDelete** element wskazuje, że wszystkie wiersze w **zamówienia** odwoływać się do danego wiersza w tabeli **klientów** tabeli zostanie usunięte, gdy wiersz **Klientów** tabeli zostanie usunięty.

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

## <a name="parameter-element-ssdl"></a>Parameter — Element (SSDL)

**Parametru** język definicji schematu magazynu (SSDL) element jest elementem podrzędnym elementu funkcji, który określa parametry dla procedury przechowywanej w bazie danych.

**Parametru** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   Dokumentacja (zero lub jeden)
-   Elementów adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **parametru** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**       | Tak         | Nazwa parametru.                                                                                                                                                                                                      |
| **Typ**       | Tak         | Typ parametru.                                                                                                                                                                                                             |
| **Tryb**       | Nie          | **W**, **się**, lub **InOut** w zależności od tego, czy parametr jest danych wejściowych, danych wyjściowych lub parametr input/output.                                                                                                                |
| **Element maxLength**  | Nie          | Maksymalna długość parametru.                                                                                                                                                                                            |
| **Precyzja**  | Nie          | Dokładność parametru.                                                                                                                                                                                                 |
| **Skala**      | Nie          | Skala parametru.                                                                                                                                                                                                     |
| **SRID**       | Nie          | Identyfikator odwołania przestrzennego systemu. Prawidłowy tylko dla parametrów typów przestrzennych. Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **parametru** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **funkcja** element, który ma dwa **parametru** elementy, które określają parametry wejściowe:

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

## <a name="principal-element-ssdl"></a>Element jednostki (SSDL)

**Jednostki** element język definicji schematu magazynu (SSDL) jest element podrzędny do elementu ReferentialConstraint, który definiuje główny koniec ograniczenie klucza obcego (nazywany także ograniczenie referencyjne). **Jednostki** element określa kolumny klucza podstawowego (lub kolumny) w tabeli, która odwołuje się do niej innej kolumny (lub kolumny). **PropertyRef** elementy Określ kolumny, które są wywoływane. Element zależne Określa kolumny, które odwołują się kolumny klucza podstawowego, które są określone w **jednostki** elementu.

**Jednostki** element może mieć następujących elementów podrzędnych (w podanej kolejności):

-   PropertyRef (jeden lub więcej)
-   Elementów adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **jednostki** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rola**       | Tak         | Taką samą wartość jak **roli** atrybut odpowiedni element End (jeśli jest używana); w przeciwnym razie nazwę tabeli, która zawiera odwołuje się do kolumny. |

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **jednostki** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano element Association używający **ReferentialConstraint** elementu, aby określić kolumny, które uczestniczą w **klucza Obcego\_CustomerOrders** klucza obcego ograniczenie. **Jednostki** element Określa **CustomerId** kolumny **klienta** tabeli jako główny koniec tego ograniczenia.

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

## <a name="property-element-ssdl"></a>Property — Element (SSDL)

**Właściwość** element język definicji schematu magazynu (SSDL) reprezentuje kolumnę w tabeli w bazie danych. **Właściwość** elementy są elementami podrzędnymi typu EntityType elementy, które reprezentują wierszy w tabeli. Każdy **właściwość** elementu zdefiniowanego w **EntityType** element reprezentuje kolumnę.

A **właściwość** elementu nie może mieć żadnych elementów podrzędnych.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **właściwość** elementu.

| Nazwa atrybutu            | Jest wymagany | Wartość                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nazwa**                  | Tak         | Nazwa odpowiednią kolumnę.                                                                                                                                                                                           |
| **Typ**                  | Tak         | Typ odpowiednią kolumnę.                                                                                                                                                                                           |
| **Dopuszcza wartości null**              | Nie          | **Wartość true,** (wartość domyślna) lub **False** w zależności od tego, czy odpowiednia kolumna może mieć wartości null.                                                                                                                  |
| **defaultValue**          | Nie          | Wartość domyślna w kolumnie.                                                                                                                                                                                  |
| **Element maxLength**             | Nie          | Maksymalna długość odpowiednią kolumnę.                                                                                                                                                                                 |
| **Wartości**           | Nie          | **Wartość true,** lub **False** w zależności od tego, czy odpowiadająca wartość w kolumnie będą przechowywane jako ciąg znaków o stałej długości.                                                                                                              |
| **Precyzja**             | Nie          | Dokładność odpowiednią kolumnę.                                                                                                                                                                                      |
| **Skala**                 | Nie          | Skala odpowiednią kolumnę.                                                                                                                                                                                          |
| **Unicode**               | Nie          | **Wartość true,** lub **False** w zależności od tego, czy odpowiadająca wartość w kolumnie będą przechowywane jako ciąg Unicode.                                                                                                                   |
| **Sortowanie**             | Nie          | Ciąg, który określa kolejność sortowania, które ma być używany w źródle danych.                                                                                                                                                   |
| **SRID**                  | Nie          | Identyfikator odwołania przestrzennego systemu. Prawidłowy tylko w przypadku właściwości typów przestrzennych. Aby uzyskać więcej informacji, zobacz [SRID](http://en.wikipedia.org/wiki/SRID) i [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Element StoreGeneratedPattern** | Nie          | **Brak**, **tożsamości** (jeśli odpowiadająca wartość w kolumnie jest tożsamość, która jest generowana w bazie danych) lub **obliczane** (jeśli odpowiadająca wartość w kolumnie jest obliczana w bazie danych). Nie obowiązuje dla właściwości RowType. |

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **właściwość** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **EntityType** element z dwóch podrzędnych **właściwość** elementy:

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

## <a name="propertyref-element-ssdl"></a>Element PropertyRef (SSDL)

**PropertyRef** element język definicji schematu magazynu (SSDL) odwołuje się do właściwości zdefiniowany dla elementu EntityType, aby wskazać, że właściwość wykona jedną z następujących ról:

-   Być częścią klucza podstawowego w tabeli, która **EntityType** reprezentuje. Co najmniej jeden **PropertyRef** elementy mogą być używane do definiowania klucza podstawowego. Aby uzyskać więcej informacji zobacz element klucza.
-   Być zakończenia zależnych lub jednostki z ograniczeniem referencyjnym. Aby uzyskać więcej informacji zobacz ReferentialConstraint element.

**PropertyRef** element może mieć tylko następujących elementów podrzędnych:

-   Dokumentacja (zero lub jeden)
-   Elementów adnotacji

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty, które mogą być stosowane do **PropertyRef** elementu.

| Nazwa atrybutu | Jest wymagany | Wartość                                |
|:---------------|:------------|:-------------------------------------|
| **Nazwa**       | Tak         | Nazwa właściwości, której dotyczy odwołanie. |

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **PropertyRef** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla CSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **PropertyRef** element używany do definiowania klucza podstawowego, odwołując się do właściwości, która jest zdefiniowana na **EntityType** elementu.

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

## <a name="referentialconstraint-element-ssdl"></a>Element ReferentialConstraint (SSDL)

**ReferentialConstraint** element język definicji schematu magazynu (SSDL) reprezentuje ograniczenie klucza obcego (nazywane również ograniczenia integralności referencyjnej) w bazie danych. Kończy się głównym i zależnym ograniczenia są odpowiednio określone przez elementy podrzędne jednostki i zależnych od ustawień lokalnych. Kolumn, które uczestniczą w głównym i zależnym kończy się są przywoływane z elementami PropertyRef.

**ReferentialConstraint** element jest podrzędnym elementem opcjonalnym elementu Association. Jeśli **ReferentialConstraint** element nie jest używany do mapowania ograniczenie klucza obcego, który jest określony w **skojarzenia** AssociationSetMapping element musi być użyty w tym elemencie.

**ReferentialConstraint** element może mieć następujące elementy podrzędne:

-   Dokumentacja (zero lub jeden)
-   Podmiot zabezpieczeń (dokładnie jeden)
-   Zależnych od ustawień lokalnych (dokładnie jeden)
-   Elementów adnotacji (zero lub więcej)

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **ReferentialConstraint** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **skojarzenia** element, który używa **ReferentialConstraint** elementu, aby określić kolumny, które uczestniczą w **klucza Obcego\_CustomerOrders**  ograniczenie klucza obcego:

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

## <a name="returntype-element-ssdl"></a>Element ReturnType (SSDL)

**ReturnType** element język definicji schematu magazynu (SSDL) określa typ zwracany dla funkcji, która jest zdefiniowana w **funkcja** elementu. Można również określić typ zwracany z funkcji **ReturnType** atrybutu.

Zwracany typ funkcji jest określony za pomocą **typu** atrybutu lub **ReturnType** elementu.

**ReturnType** element może mieć następujące elementy podrzędne:

- Typ CollectionType (po jednym)  

> [!NOTE]
> Dowolna liczba atrybutów adnotacji (niestandardowe atrybuty XML) można stosować do **ReturnType** elementu. Jednak atrybutów niestandardowych, które nie mogą należeć do przestrzeni nazw XML, który jest zarezerwowany dla SSDL. W pełni kwalifikowanej nazwy dowolne dwa atrybuty niestandardowe nie może być taka sama.

### <a name="example"></a>Przykład

W poniższym przykładzie użyto **funkcja** zwracającego Kolekcja wierszy.

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


## <a name="rowtype-element-ssdl"></a>Element RowType (SSDL)

A **RowType** element język definicji schematu magazynu (SSDL) definiuje strukturę nienazwane jako zwracany typ funkcji zdefiniowanych w magazynie.

A **RowType** element jest elementem podrzędnym **CollectionType** elementu:

A **RowType** element może mieć następujące elementy podrzędne:

- Właściwości (jeden lub więcej)  

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano funkcję magazynu, która używa **CollectionType** elementu, aby określić, że funkcja zwraca Kolekcja wierszy (jak określono w **RowType** elementu).


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

**Schematu** element w język definicji schematu magazynu (SSDL) jest głównym elementem definicję modelu magazynu. Zawiera definicje dla obiektów, funkcji i kontenerów, które tworzą model magazynu.

**Schematu** element może zawierać zero lub więcej z następujących elementów podrzędnych:

-   Skojarzenie
-   Typ entityType
-   Obiekt EntityContainer
-   Funkcja

**Schematu** element używa **Namespace** atrybut do definiowania przestrzeni nazw dla obiektów typu i skojarzenia jednostki w modelu magazynu. W przestrzeni nazw nie dwa obiekty mogą mieć takiej samej nazwie.

Przestrzeń nazw modelu magazynu różni się od przestrzeni nazw XML **schematu** elementu. Przestrzeń nazw modelu magazynu (zgodnie z definicją **Namespace** atrybutu) to logiczny kontener przeznaczony dla typów jednostek i typów skojarzenia. Przestrzeń nazw XML (wskazywanym przez **xmlns** atrybutu) z **schematu** element jest domyślny obszar nazw dla elementów podrzędnych i atrybutów **schematu** elementu. Obszary nazw XML w postaci http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (gdzie RRRR i MM stanowi rok i miesiąc odpowiednio) są zarezerwowane dla SSDL. Niestandardowe elementy i atrybuty nie może być w przestrzeni nazw, które mają postać.

### <a name="applicable-attributes"></a>Odpowiednie atrybuty

W poniższej tabeli opisano atrybuty mogą być stosowane do **schematu** elementu.

| Nazwa atrybutu            | Jest wymagany | Wartość                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**             | Tak         | Przestrzeń nazw modelu magazynu. Wartość **Namespace** atrybut jest używany w celu utworzenia w pełni kwalifikowana nazwa typu. Na przykład jeśli **EntityType** o nazwie *klienta* znajduje się w przestrzeni nazw ExampleModel.Store, a następnie w pełni kwalifikowana nazwa **EntityType** jest ExampleModel.Store.Customer. <br/> Nie można użyć następujących ciągów jako wartość pozycji **Namespace** atrybut: **systemu**, **przejściowy**, lub **Edm**. Wartość **Namespace** atrybut nie może być taka sama jak wartość **Namespace** atrybutu w elemencie CSDL Schema. |
| **Alias**                 | Nie          | Identyfikator używany zamiast nazwy przestrzeni nazw. Na przykład jeśli **EntityType** o nazwie *klienta* znajduje się w przestrzeni nazw ExampleModel.Store i wartość **Alias** atrybut jest *StorageModel*, wówczas można użyć StorageModel.Customer jako w pełni kwalifikowana nazwa **typu EntityType.**                                                                                                                                                                                                                                                                                    |
| **Dostawcy**              | Tak         | Dostawca danych.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **ProviderManifestToken** | Tak         | Token, który wskazuje dostawcy, które manifest dostawcy, aby powrócić. Nie format tokenu jest zdefiniowany. Wartości dla tokenu są definiowane przez dostawcę. Uzyskać informacji dotyczących tokenów manifestu dostawcy programu SQL Server zobacz SqlClient programu Entity Framework.                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a>Przykład

W poniższym przykładzie przedstawiono **schematu** element, który zawiera **EntityContainer** element, dwa **EntityType** elementów, a drugi **skojarzenia** elementu.

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
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

Atrybuty adnotacji w język definicji schematu magazynu (SSDL) są niestandardowych atrybutów XML w modelu magazynu, które zapewniają dodatkowe metadane na temat elementów w modelu magazynu. Oprócz prawidłowe struktury XML, obowiązują następujące ograniczenia adnotacji atrybutów:

-   Atrybuty adnotacji nie może być w przestrzeni nazw XML, który jest zarezerwowany dla SSDL.
-   W pełni kwalifikowanej nazwy wszelkie atrybuty dwóch adnotacji nie może być taka sama.

Więcej niż jeden atrybut adnotacji można stosować do danego elementu SSDL. W czasie wykonywania przy użyciu klas w przestrzeni nazw System.Data.Metadata.Edm możliwy jest metadanych elementów adnotacji.

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano element EntityType, który ma atrybut adnotacja zastosowana do **OrderId** właściwości. Przykład pokazują również dodawane do elementu adnotacji **EntityType** elementu.

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

## <a name="annotation-elements-ssdl"></a>Elementów adnotacji (SSDL)

Elementy adnotacji w język definicji schematu magazynu (SSDL) są niestandardowe elementy XML w modelu magazynu, które zapewniają dodatkowe metadane na temat modelu magazynu. Oprócz prawidłowe struktury XML, elementów adnotacji obowiązują następujące ograniczenia:

-   Elementów adnotacji nie może być w przestrzeni nazw XML, który jest zarezerwowany dla SSDL.
-   W pełni kwalifikowanej nazwy dowolne elementy dwóch adnotacji nie może być taka sama.
-   Adnotacja elementów musi występować po wszystkich innych elementów podrzędnych danego elementu SSDL.

Więcej niż jeden element adnotacji może być elementem podrzędnym danego elementu SSDL. Począwszy od programu .NET Framework w wersji 4, metadanych elementów adnotacji są dostępne w czasie wykonywania przy użyciu klas w przestrzeni nazw System.Data.Metadata.Edm.

### <a name="example"></a>Przykład

W poniższym przykładzie pokazano element EntityType, który ma element adnotacji (**CustomElement**). W przykładzie pokazano również zastosować atrybut adnotacji **OrderId** właściwości.

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

## <a name="facets-ssdl"></a>Zestawy reguł (SSDL)

Aspekty w język definicji schematu magazynu (SSDL) reprezentują ograniczenia dotyczące typów kolumn, które są określone w elementach właściwości. Zestawy reguł są implementowane jako atrybuty XML w **właściwość** elementów.

W poniższej tabeli opisano aspekty, które są obsługiwane przez SSDL:

| zestaw reguł           | Opis                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Sortowanie**   | Określa kolejność sortowania (lub sekwencji sortowania) do użycia podczas przeprowadzania porównania i kolejność operacji na wartościach właściwości.                                                                                                             |
| **Wartości** | Określa, czy długość wartości kolumny mogą się różnić.                                                                                                                                                                                                  |
| **Element maxLength**   | Określa maksymalną długość wartości kolumny.                                                                                                                                                                                                           |
| **Precyzja**   | Dla właściwości typu **dziesiętna**, określa liczbę cyfr, może mieć wartości właściwości. Dla właściwości typu **czasu**, **daty/godziny**, i **DateTimeOffset**, określa liczbę cyfr ułamkowych części sekundy w wartości kolumny. |
| **Skala**       | Określa liczbę cyfr po prawej stronie przecinka dziesiętnego dla wartości kolumny.                                                                                                                                                                      |
| **Unicode**     | Wskazuje, czy wartość kolumny jest zapisywana w formacie Unicode.                                                                                                                                                                                                    |
