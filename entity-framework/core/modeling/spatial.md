---
title: Dane przestrzenne — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: cced53edadb890e4e86753ec2628218ffc4d1d5b
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181392"
---
# <a name="spatial-data"></a>Dane przestrzenne

> [!NOTE]
> Ta funkcja została dodana w EF Core 2,2.

Dane przestrzenne reprezentują lokalizację fizyczną i kształt obiektów. Wiele baz danych zapewnia obsługę tego typu danych, dzięki czemu można je indeksować i badać wraz z innymi danymi. Typowe scenariusze obejmują zapytania dotyczące obiektów w danej odległości od lokalizacji lub wybierając obiekt, którego obramowanie zawiera daną lokalizację. EF Core obsługuje mapowanie do typów danych przestrzennych przy użyciu biblioteki przestrzennej [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .

## <a name="installing"></a>Instalowanie programu

Aby można było używać danych przestrzennych z EF Core, należy zainstalować odpowiedni pomocniczy pakiet NuGet. Który pakiet należy zainstalować zależy od dostawcy, którego używasz.

Dostawca EF Core                        | Przestrzenny pakiet NuGet
--------------------------------------- | ---------------------
Microsoft.EntityFrameworkCore.SqlServer | [Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft.EntityFrameworkCore.Sqlite    | [Microsoft. EntityFrameworkCore. sqlite. NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft.EntityFrameworkCore.InMemory  | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite)
Npgsql.EntityFrameworkCore.PostgreSQL   | [Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>Odwrócenie inżynierii

Przestrzenne pakiety NuGet umożliwiają również [Tworzenie modeli odtwarzania](../managing-schemas/scaffolding.md) z właściwościami przestrzennymi, ale należy zainstalować pakiet ***przed*** uruchomieniem `Scaffold-DbContext` lub `dotnet ef dbcontext scaffold`. W przeciwnym razie otrzymasz ostrzeżenia dotyczące nieznajdowania mapowań typów dla kolumn, a kolumny zostaną pominięte.

## <a name="nettopologysuite-nts"></a>NetTopologySuite (NKTY przerwania)

NetTopologySuite jest biblioteką przestrzenną dla platformy .NET. EF Core umożliwia mapowanie do typów danych przestrzennych w bazie danych przy użyciu typów NKTY przerwania w modelu.

Aby włączyć mapowanie do typów przestrzennych za pośrednictwem NKTY przerwania, wywołaj metodę UseNetTopologySuite w konstruktorze opcji DbContext dostawcy. Na przykład w przypadku SQL Server należy wywołać tę metodę.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

Istnieje kilka typów danych przestrzennych. Używany typ zależy od typów kształtów, które mają być dozwolone. Poniżej znajduje się hierarchia typów NKTY przerwania, których można użyć do właściwości w modelu. Znajdują się one w przestrzeni nazw `NetTopologySuite.Geometries`.

* Geometrii
  * Moment
  * LineString
  * Tworząc
  * GeometryCollection
    * Usług
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString, CompoundCurve i CurePolygon nie są obsługiwane przez NKTY przerwania.

Użycie podstawowego typu geometrii umożliwia określenie dowolnego typu kształtu przez właściwość.

Poniższe klasy jednostek mogą służyć do mapowania tabel w [przykładowej bazie danych](https://go.microsoft.com/fwlink/?LinkID=800630)na całym świecie.

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public Point Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public Geometry Border { get; set; }
}
```

### <a name="creating-values"></a>Tworzenie wartości

Można użyć konstruktorów do tworzenia obiektów geometrycznych; jednak NKTY przerwania zaleca się użycie fabryki geometrycznej. Pozwala to określić domyślny SRID (System referencyjny przestrzennej używany przez współrzędne) i zapewnia kontrolę nad bardziej zaawansowanymi elementami, takimi jak model precyzji (używany podczas obliczeń) i sekwencją współrzędnych (określa, które współrzędne są wymiarami i miary--są dostępne).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326 dotyczy WGS 84, standardowego używanego w systemach GPS i innych systemów geograficznych.

### <a name="longitude-and-latitude"></a>Długości i szerokości geograficznej

Współrzędne w NKTY przerwania są pod względem wartości X i Y. Aby przedstawić długość i szerokość geograficzną, użyj X dla długości geograficznej i Y dla szerokości geograficznej. Należy pamiętać, że jest to **wsteczne** w formacie `latitude, longitude`, w którym są zwykle wyświetlane te wartości.

### <a name="srid-ignored-during-client-operations"></a>SRID zignorowane podczas operacji klienta

NKTY przerwania ignoruje wartości SRID podczas operacji. Przyjęto założenie układu współrzędnych. Oznacza to, że jeśli określisz współrzędne w zakresie długości i szerokości geograficznej, niektóre wartości oceniane przez klienta, takie jak odległość, długość i obszar, będą znajdować się w stopniach, a nie w metrach. Aby uzyskać bardziej zrozumiałe wartości, najpierw należy umieścić współrzędne w innym układzie współrzędnych przy użyciu biblioteki, takiej jak [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) przed obliczeniem tych wartości.

Jeśli operacja jest Szacowana przez serwer EF Core za pośrednictwem SQL, jednostka wyniku zostanie określona przez bazę danych.

Oto przykład użycia ProjNet4GeoAPI do obliczenia odległości między dwoma miastami.

``` csharp
static class GeometryExtensions
{
    static readonly CoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                [4326] = GeographicCoordinateSystem.WGS84.WKT,

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

    public static Geometry ProjectTo(this Geometry geometry, int srid)
    {
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        var result = geometry.Copy();
        result.Apply(new MathTransformFilter(transformation.MathTransform));

        return result;
    }

    class MathTransformFilter : ICoordinateSequenceFilter
    {
        readonly MathTransform _transform;

        public MathTransformFilter(MathTransform transform)
            => _transform = transform;

        public bool Done => false;
        public bool GeometryChanged => true;

        public void Filter(CoordinateSequence seq, int i)
        {
            var result = _transform.Transform(
                new[]
                {
                    seq.GetOrdinate(i, Ordinate.X),
                    seq.GetOrdinate(i, Ordinate.Y)
                });
            seq.SetOrdinate(i, Ordinate.X, result[0]);
            seq.SetOrdinate(i, Ordinate.Y, result[1]);
        }
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a>Wykonanie zapytania o dane

W LINQ, metody i właściwości NKTY przerwania dostępne jako funkcje bazy danych zostaną przetłumaczone na SQL. Na przykład odległość i zawiera metody są tłumaczone w poniższych zapytaniach. W tabeli na końcu tego artykułu przedstawiono, które elementy członkowskie są obsługiwane przez różnych dostawców EF Core.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

Jeśli używasz SQL Server, musisz wiedzieć o kilku dodatkowych kwestiach.

### <a name="geography-or-geometry"></a>Geografia lub geometria

Domyślnie właściwości przestrzenne są mapowane na kolumny `geography` w SQL Server. Aby użyć `geometry`, [skonfiguruj Typ kolumny](xref:core/modeling/relational/data-types) w modelu.

### <a name="geography-polygon-rings"></a>Pierścienie wielokątów geograficznych

W przypadku używania typu kolumny `geography` SQL Server nakładają dodatkowe wymagania dotyczące pierścienia zewnętrznego (lub powłoki) i wewnętrznych pierścieni (lub dziur). Pierścień zewnętrzny musi być zorientowany w lewo i w prawo. NKTY przerwania sprawdza to przed wysłaniem wartości do bazy danych.

### <a name="fullglobe"></a>FullGlobe

SQL Server ma niestandardowy typ geometrii reprezentujący pełny Globus przy użyciu typu kolumny `geography`. Ma również sposób reprezentowania wielokątów w oparciu o pełny Globus (bez pierścienia zewnętrznego). Żadna z tych elementów nie jest obsługiwana przez NKTY przerwania.

> [!WARNING]
> FullGlobe i wielokąty oparte na nim nie są obsługiwane przez NKTY przerwania.

## <a name="sqlite"></a>Bazy danych SQLite

Poniżej przedstawiono dodatkowe informacje dotyczące tych, które są używane przez program SQLite.

### <a name="installing-spatialite"></a>Instalowanie SpatiaLite

W systemie Windows natywna biblioteka mod_spatialite jest dystrybuowana jako zależność pakietu NuGet. Inne platformy muszą zainstalować je oddzielnie. Jest to zazwyczaj realizowane przy użyciu Menedżera pakietów oprogramowania. Na przykład można użyć APT w Ubuntu i oprogramowania Homebrew na MacOS.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a>Konfigurowanie SRID

W SpatiaLite, kolumny muszą określać SRID na kolumnę. Wartość domyślna SRID to `0`. Określ inny SRID przy użyciu metody ForSqliteHasSrid.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Wymiar

Podobnie jak w przypadku SRID, wymiar kolumny (lub współrzędnej) jest również określony jako część kolumny. Domyślnymi współrzędnymi są X i Y. Włącz dodatkowe współrzędne (Z i M) przy użyciu metody ForSqliteHasDimension.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>Operacje tłumaczone

W tej tabeli przedstawiono, które elementy członkowskie NKTY przerwania są tłumaczone na SQL przez każdego dostawcę EF Core.

NetTopologySuite | SQL Server (Geometria) | SQL Server (Geografia) | Bazy danych SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
Geometria. obszar | ✔ | ✔ | ✔ | ✔
Geometry. AsBinary () | ✔ | ✔ | ✔ | ✔
Geometry. AsText () | ✔ | ✔ | ✔ | ✔
Geometria. granica | ✔ | | ✔ | ✔
Geometry. Buffer (Double) | ✔ | ✔ | ✔ | ✔
Geometry. Buffer (Double, int) | | | ✔
Geometria. centroida | ✔ | | ✔ | ✔
Geometry. Contains (Geometria) | ✔ | ✔ | ✔ | ✔
Geometry. ConvexHull () | ✔ | ✔ | ✔ | ✔
Geometry. CoveredBy (Geometria) | | | ✔ | ✔
Geometria. okładki (Geometria) | | | ✔ | ✔
Geometry. crosss (Geometria) | ✔ | | ✔ | ✔
Geometry. różnica (Geometria) | ✔ | ✔ | ✔ | ✔
Geometria. wymiar | ✔ | ✔ | ✔ | ✔
Geometria. rozłączna (Geometria) | ✔ | ✔ | ✔ | ✔
Geometria. Distance (Geometria) | ✔ | ✔ | ✔ | ✔
Geometria. koperta | ✔ | | ✔ | ✔
Geometry. EqualsExact (Geometria) | | | | ✔
Geometry. EqualsTopologically (Geometria) | ✔ | ✔ | ✔ | ✔
Geometry. Geometrytype | ✔ | ✔ | ✔ | ✔
Geometry. GetGeometryN (int) | ✔ | | ✔ | ✔
Geometria. InteriorPoint | ✔ | | ✔
Geometria. część wspólna (Geometria) | ✔ | ✔ | ✔ | ✔
Geometria. Intersects (Geometria) | ✔ | ✔ | ✔ | ✔
Geometria. IsEmpty | ✔ | ✔ | ✔ | ✔
Geometria. IsSimple | ✔ | | ✔ | ✔
Geometria. IsValid | ✔ | ✔ | ✔ | ✔
Geometry. IsWithinDistance (Geometria, Double) | ✔ | | ✔
Geometria. Długość | ✔ | ✔ | ✔ | ✔
Geometria. NumGeometries | ✔ | ✔ | ✔ | ✔
Geometria. NumPoints | ✔ | ✔ | ✔ | ✔
Geometria. OgcGeometryType | ✔ | ✔ | ✔
Geometria. nakładanie się (Geometria) | ✔ | ✔ | ✔ | ✔
Geometria. PointOnSurface | ✔ | | ✔ | ✔
Geometry. rerelacja (Geometria, ciąg) | ✔ | | ✔ | ✔
Geometry. Reverse () | | | ✔ | ✔
Geometria. SRID | ✔ | ✔ | ✔ | ✔
Geometry. SymmetricDifference (Geometria) | ✔ | ✔ | ✔ | ✔
Geometry. ToBinary () | ✔ | ✔ | ✔ | ✔
Geometry. ToText () | ✔ | ✔ | ✔ | ✔
Geometria. touch (Geometria) | ✔ | | ✔ | ✔
Geometry. Union () | | | ✔
Geometry. Union (Geometria) | ✔ | ✔ | ✔ | ✔
Geometria. w elemencie (Geometria) | ✔ | ✔ | ✔ | ✔
GeometryCollection. Count | ✔ | ✔ | ✔ | ✔
GeometryCollection [int] | ✔ | ✔ | ✔ | ✔
LineString. Count | ✔ | ✔ | ✔ | ✔
LineString. EndPoint | ✔ | ✔ | ✔ | ✔
LineString. GetPointN (int) | ✔ | ✔ | ✔ | ✔
LineString. IsClosed | ✔ | ✔ | ✔ | ✔
LineString. IsRing | ✔ | | ✔ | ✔
LineString. StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString. IsClosed | ✔ | ✔ | ✔ | ✔
Punkt. M | ✔ | ✔ | ✔ | ✔
Punkt. X | ✔ | ✔ | ✔ | ✔
Punkt. Y | ✔ | ✔ | ✔ | ✔
Punkt. Z | ✔ | ✔ | ✔ | ✔
Wielokąt. ExteriorRing | ✔ | ✔ | ✔ | ✔
Wielokąt. GetInteriorRingN (int) | ✔ | ✔ | ✔ | ✔
Wielokąt. NumInteriorRings | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dane przestrzenne w SQL Server](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [Strona główna SpatiaLite](https://www.gaia-gis.it/fossil/libspatialite)
* [Dokumentacja przestrzenna Npgsql](https://www.npgsql.org/efcore/mapping/nts.html)
* [Dokumentacja PostGIS](https://postgis.net/documentation/)
