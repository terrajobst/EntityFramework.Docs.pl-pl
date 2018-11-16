---
title: Dane przestrzenne — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 49c18758af2f2383ea082ead2f6df4c022152b36
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688810"
---
# <a name="spatial-data"></a>Dane przestrzenne

> [!NOTE]
> Ta funkcja jest nowa w programie EF Core 2.2.

Dane przestrzenne reprezentuje lokalizację fizyczną i kształt obiektów. Wiele baz danych zapewniają obsługę tego rodzaju danych, więc może być indeksowana i zapytania wraz z innymi danymi. Typowe scenariusze obejmują tworzenie zapytań dotyczących obiektów w określonej odległości od lokalizacji lub zaznaczając ten obiekt, którego obramowanie zawiera danej lokalizacji. EF Core obsługuje Mapowanie typów danych przestrzennych przy użyciu [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) przestrzenne biblioteki.

## <a name="installing"></a>Instalowanie programu

Aby można było używać danych przestrzennych z programem EF Core, musisz zainstalować odpowiedni pakiet NuGet pomocniczych. Pakiet, który należy zainstalować zależy od dostawcy, którego używasz.

EF Core dostawcy                        | Przestrzenne pakietu NuGet
--------------------------------------- | ---------------------
Microsoft.EntityFrameworkCore.SqlServer | [Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft.EntityFrameworkCore.Sqlite    | [Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft.EntityFrameworkCore.InMemory  | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite)
Npgsql.EntityFrameworkCore.PostgreSQL   | [Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>Odtwarzanie

Przestrzenne NuGet również pakietów Włącz [odtwarzanie](../managing-schemas/scaffolding.md) modeli przy użyciu właściwości przestrzennych, ale trzeba zainstalować pakiet ***przed*** systemem `Scaffold-DbContext` lub `dotnet ef dbcontext scaffold`. Jeśli nie istnieje, otrzymasz ostrzeżenia dotyczące nie możesz znaleźć mapowania typów kolumn i kolumny zostaną pominięte.

## <a name="nettopologysuite-nts"></a>NetTopologySuite (NTS)

NetTopologySuite jest biblioteką przestrzennych dla platformy .NET. EF Core umożliwia mapowanie danych przestrzennych typów w bazie danych przy użyciu typów nkty przerwania w modelu.

Aby włączyć mapowanie typów przestrzennych za pośrednictwem nkty przerwania, należy wywołać metodę UseNetTopologySuite dla dostawcy typu DbContext opcje konstruktora. Na przykład z programem SQL Server możesz wywołać go następująco.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

Istnieją różne typy danych przestrzennych. Jakiego typu, możesz użyć zależy od typów kształtami, które chcesz zezwolić. Oto hierarchii typów nkty przerwania, które służy do właściwości w modelu. Są one umieszczone w ramach `NetTopologySuite.Geometries` przestrzeni nazw. Odpowiednich interfejsów w pakiecie GeoAPI (`GeoAPI.Geometries` przestrzeni nazw) może również służyć.

* Geometrii
  * Punkt
  * LineString
  * Wielokąt
  * GeometryCollection
    * MultiPoint
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString, CompoundCurve i CurePolygon nie są obsługiwane przez nkty przerwania.

Przy użyciu podstawowego typu Geometria umożliwia dowolny typ kształtu do określonych we właściwości.

Następujących klas jednostek może być używany do mapowania z tabelami w [Wide World Importers przykładowej bazy danych](http://go.microsoft.com/fwlink/?LinkID=800630).

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public IPoint Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public IGeometry Border { get; set; }
}
```

### <a name="creating-values"></a>Tworzenie wartości

Można używać konstruktorów do tworzenia obiektów geometrii; jednak nkty przerwania zaleca używanie fabrykę geometrii zamiast tego. To pozwala określić domyślną SRID (używane przez współrzędne system odwołania przestrzennego) i zapewnia kontrolę nad bardziej zaawansowanych np. model precyzji (używane podczas obliczeń) i sekwencji współrzędnych (określa, które rzędne — wymiarów i miary — są dostępne).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326 odnosi się do WGS 84 standard używany w GPS i innych systemach geograficznego.

### <a name="longitude-and-latitude"></a>Długość i szerokość geograficzną

Współrzędne nkty przerwania są na podstawie wartości X i Y. Do reprezentowania długości i szerokości geograficznej, na użytek X dla współrzędnych i Y szerokości geograficznej. Należy zauważyć, że jest to **Wstecz** z `latitude, longitude` format, w którym zwykle widzieć te wartości.

### <a name="srid-ignored-during-client-operations"></a>SRID ignorowany podczas operacji klienta

Nkty przerwania ignoruje wartości SRID podczas wykonywania operacji. Przyjęto założenie, współrzędnych planarnych. Oznacza to, że w przypadku określenia współrzędnych pod względem długości i szerokości geograficznej, niektóre wartości takie jak odległości, długość i obszar będzie w stopniach, liczniki nie oceniane przez klienta. Dla bardziej zrozumiały wartości, należy najpierw projektu współrzędne miejsca inny układ współrzędnych przy użyciu biblioteki, takich jak [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) przed obliczeniem te wartości.

W przypadku operacji serwera oceniane przez platformę EF Core przy użyciu programu SQL, jednostka wynik będzie określana przez bazę danych.

Oto przykład użycia ProjNet4GeoAPI, aby obliczyć odległość między dwoma miast.

``` csharp
static class GeometryExtensions
{
    static readonly IGeometryServices _geometryServices = NtsGeometryServices.Instance;
    static readonly ICoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                // (3857 and 4326 included automatically)

                // This coordinate system covers the area of our data.
                // Different data requires a different coordinate system.
                [2855] =
                @"
                    PROJCS[""NAD83(HARN) / Washington North"",
                        GEOGCS[""NAD83(HARN)"",
                            DATUM[""NAD83_High_Accuracy_Regional_Network"",
                                SPHEROID[""GRS 1980"",6378137,298.257222101,
                                    AUTHORITY[""EPSG"",""7019""]],
                                AUTHORITY[""EPSG"",""6152""]],
                            PRIMEM[""Greenwich"",0,
                                AUTHORITY[""EPSG"",""8901""]],
                            UNIT[""degree"",0.01745329251994328,
                                AUTHORITY[""EPSG"",""9122""]],
                            AUTHORITY[""EPSG"",""4152""]],
                        PROJECTION[""Lambert_Conformal_Conic_2SP""],
                        PARAMETER[""standard_parallel_1"",48.73333333333333],
                        PARAMETER[""standard_parallel_2"",47.5],
                        PARAMETER[""latitude_of_origin"",47],
                        PARAMETER[""central_meridian"",-120.8333333333333],
                        PARAMETER[""false_easting"",500000],
                        PARAMETER[""false_northing"",0],
                        UNIT[""metre"",1,
                            AUTHORITY[""EPSG"",""9001""]],
                        AUTHORITY[""EPSG"",""2855""]]
                "
            });

    public static IGeometry ProjectTo(this IGeometry geometry, int srid)
    {
        var geometryFactory = _geometryServices.CreateGeometryFactory(srid);
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        return GeometryTransform.TransformGeometry(
            geometryFactory,
            geometry,
            transformation.MathTransform);
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a>Wykonanie zapytania o dane

W programie LINQ nkty przerwania metody i właściwości dostępne jako funkcje bazy danych ma być tłumaczony na SQL. Na przykład odległość i zawiera metody są tłumaczone w następujących zapytań. Tabeli na końcu tego artykułu przedstawiono składniki, które są obsługiwane przez różnych dostawców programu EF Core.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

Jeśli używasz programu SQL Server, istnieją pewne dodatkowe rzeczy, na których należy wiedzieć.

### <a name="geography-or-geometry"></a>Położenia geograficznego lub geometrii

Domyślnie przestrzenne właściwości są mapowane na `geography` kolumny w programie SQL Server. Aby użyć `geometry`, [skonfigurować typ kolumny](xref:core/modeling/relational/data-types) w modelu.

### <a name="geography-polygon-rings"></a>Pierścienie wielokąta lokalizacji geograficznej

Korzystając z `geography` typ kolumny, programu SQL Server nakłada dodatkowe wymagania dotyczące pierścienia zewnętrznego (lub powłoki) i posługiwanie się nimi pierścieniami (lub otworów). Pierścieniem musi być zorientowana zegara i wewnętrznych pierścieni zgodnie ze wskazówkami zegara. NTFS sprawdza poprawność to przed wysłaniem wartości w bazie danych.

### <a name="fullglobe"></a>FullGlobe

Program SQL Server ma typ geometrii niestandardowych do reprezentowania pełną całym świecie, korzystając z `geography` typ kolumny. Ma również sposób reprezentacji wielokątów oparte na świecie pełny (bez pierścieniem). Oba te są obsługiwane przez nkty przerwania.

> [!WARNING]
> FullGlobe i wielokątów na jego podstawie nie są obsługiwane przez nkty przerwania.

## <a name="sqlite"></a>Bazy danych SQLite

Poniżej przedstawiono niektóre dodatkowe informacje dotyczące tych przy użyciu systemu SQLite.

### <a name="installing-spatialite"></a>Instalowanie SpatiaLite

W Windows biblioteki natywnej mod_spatialite jest rozpowszechniany jako zależność pakietu NuGet. Platformami, trzeba go zainstalować osobno. Zazwyczaj jest to wykonywane przy użyciu Menedżera pakietów oprogramowania. Na przykład można użyć APT w systemie Ubuntu i Homebrew w systemie MacOS.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a>Konfigurowanie SRID

Kolumny SpatiaLite, muszą określić SRID według kolumny. Wartość domyślna to SRID `0`. Określ inną SRID, przy użyciu metody ForSqliteHasSrid.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Wymiar

Podobnie jak SRID, wymiaru (lub kolumny rzędne) jest również określona jako część kolumny. Rzędne domyślne są X i Y. Włącz dodatkowe rzędne (Z i M) przy użyciu metody ForSqliteHasDimension.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>Operacje tłumaczenia

W poniższej tabeli przedstawiono, które elementy członkowskie nkty przerwania są tłumaczone na SQL przez każdy dostawca programu EF Core.

NetTopologySuite | Program SQL Server (geometrii) | Program SQL Server (geograficzne) | Bazy danych SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
Geometry.Area | ✔ | ✔ | ✔ | ✔
Geometry.AsBinary() | ✔ | ✔ | ✔ | ✔
Geometry.AsText() | ✔ | ✔ | ✔ | ✔
Geometry.Boundary | ✔ | | ✔ | ✔
Geometry.Buffer(double) | ✔ | ✔ | ✔ | ✔
Geometry.Buffer (double, int) | | | ✔
Geometry.Centroid | ✔ | | ✔ | ✔
Geometry.Contains(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ConvexHull() | ✔ | ✔ | ✔ | ✔
Geometry.CoveredBy(Geometry) | | | ✔ | ✔
Geometry.Covers(Geometry) | | | ✔ | ✔
Geometry.Crosses(Geometry) | ✔ | | ✔ | ✔
Geometry.Difference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Dimension | ✔ | ✔ | ✔ | ✔
Geometry.Disjoint(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Distance(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Envelope | ✔ | | ✔ | ✔
Geometry.EqualsExact(Geometry) | | | | ✔
Geometry.EqualsTopologically(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.GeometryType | ✔ | ✔ | ✔ | ✔
Geometry.GetGeometryN(int) | ✔ | | ✔ | ✔
Geometry.InteriorPoint | ✔ | | ✔
Geometry.Intersection(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Intersects(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.IsEmpty | ✔ | ✔ | ✔ | ✔
Geometry.IsSimple | ✔ | | ✔ | ✔
Geometry.IsValid | ✔ | ✔ | ✔ | ✔
Geometry.IsWithinDistance (Geometry, double) | ✔ | | ✔
Geometry.Length | ✔ | ✔ | ✔ | ✔
Geometry.NumGeometries | ✔ | ✔ | ✔ | ✔
Geometry.NumPoints | ✔ | ✔ | ✔ | ✔
Geometry.OgcGeometryType | ✔ | ✔ | ✔
Geometry.Overlaps(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.PointOnSurface | ✔ | | ✔ | ✔
Geometry.Relate (Geometry, ciąg) | ✔ | | ✔ | ✔
Geometry.Reverse() | | | ✔ | ✔
Geometry.SRID | ✔ | ✔ | ✔ | ✔
Geometry.SymmetricDifference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ToBinary() | ✔ | ✔ | ✔ | ✔
Geometry.ToText() | ✔ | ✔ | ✔ | ✔
Geometry.Touches(Geometry) | ✔ | | ✔ | ✔
Geometry.Union() | | | ✔
Geometry.Union(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Within(Geometry) | ✔ | ✔ | ✔ | ✔
GeometryCollection.Count | ✔ | ✔ | ✔ | ✔
GeometryCollection [int] | ✔ | ✔ | ✔ | ✔
LineString.Count | ✔ | ✔ | ✔ | ✔
LineString.EndPoint | ✔ | ✔ | ✔ | ✔
LineString.GetPointN(int) | ✔ | ✔ | ✔ | ✔
LineString.IsClosed | ✔ | ✔ | ✔ | ✔
LineString.IsRing | ✔ | | ✔ | ✔
LineString.StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString.IsClosed | ✔ | ✔ | ✔ | ✔
Point.M | ✔ | ✔ | ✔ | ✔
Point.X | ✔ | ✔ | ✔ | ✔
Point.Y | ✔ | ✔ | ✔ | ✔
Point.Z | ✔ | ✔ | ✔ | ✔
Polygon.ExteriorRing | ✔ | ✔ | ✔ | ✔
Polygon.GetInteriorRingN(int) | ✔ | ✔ | ✔ | ✔
Polygon.NumInteriorRings | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dane przestrzenne programu SQL Server](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [Strona główna SpatiaLite](https://www.gaia-gis.it/fossil/libspatialite)
* [Dokumentacja PostGIS](http://postgis.net/documentation/)
