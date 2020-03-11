---
title: Dane przestrzenne — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 5b45f83ca7f02665f52ccfe16b5af506a6046a62
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417406"
---
# <a name="spatial-data"></a><span data-ttu-id="576a2-102">Dane przestrzenne</span><span class="sxs-lookup"><span data-stu-id="576a2-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="576a2-103">Ta funkcja została dodana w EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="576a2-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="576a2-104">Dane przestrzenne reprezentują lokalizację fizyczną i kształt obiektów.</span><span class="sxs-lookup"><span data-stu-id="576a2-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="576a2-105">Wiele baz danych zapewnia obsługę tego typu danych, dzięki czemu można je indeksować i badać wraz z innymi danymi.</span><span class="sxs-lookup"><span data-stu-id="576a2-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="576a2-106">Typowe scenariusze obejmują zapytania dotyczące obiektów w danej odległości od lokalizacji lub wybierając obiekt, którego obramowanie zawiera daną lokalizację.</span><span class="sxs-lookup"><span data-stu-id="576a2-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="576a2-107">EF Core obsługuje mapowanie do typów danych przestrzennych przy użyciu biblioteki przestrzennej [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="576a2-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="576a2-108">Instalowanie programu</span><span class="sxs-lookup"><span data-stu-id="576a2-108">Installing</span></span>

<span data-ttu-id="576a2-109">Aby można było używać danych przestrzennych z EF Core, należy zainstalować odpowiedni pomocniczy pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="576a2-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="576a2-110">Który pakiet należy zainstalować zależy od dostawcy, którego używasz.</span><span class="sxs-lookup"><span data-stu-id="576a2-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="576a2-111">Dostawca EF Core</span><span class="sxs-lookup"><span data-stu-id="576a2-111">EF Core Provider</span></span>                        | <span data-ttu-id="576a2-112">Przestrzenny pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="576a2-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="576a2-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="576a2-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="576a2-114">Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="576a2-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="576a2-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="576a2-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="576a2-116">Microsoft. EntityFrameworkCore. sqlite. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="576a2-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="576a2-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="576a2-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="576a2-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="576a2-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="576a2-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="576a2-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="576a2-120">Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="576a2-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="576a2-121">Odwrócenie inżynierii</span><span class="sxs-lookup"><span data-stu-id="576a2-121">Reverse engineering</span></span>

<span data-ttu-id="576a2-122">Przestrzenne pakiety NuGet umożliwiają również [Tworzenie modeli odtwarzania](../managing-schemas/scaffolding.md) z właściwościami przestrzennymi, ale należy zainstalować pakiet ***przed*** uruchomieniem `Scaffold-DbContext` lub `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="576a2-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="576a2-123">W przeciwnym razie otrzymasz ostrzeżenia dotyczące nieznajdowania mapowań typów dla kolumn, a kolumny zostaną pominięte.</span><span class="sxs-lookup"><span data-stu-id="576a2-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="576a2-124">NetTopologySuite (NKTY przerwania)</span><span class="sxs-lookup"><span data-stu-id="576a2-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="576a2-125">NetTopologySuite jest biblioteką przestrzenną dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="576a2-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="576a2-126">EF Core umożliwia mapowanie do typów danych przestrzennych w bazie danych przy użyciu typów NKTY przerwania w modelu.</span><span class="sxs-lookup"><span data-stu-id="576a2-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="576a2-127">Aby włączyć mapowanie do typów przestrzennych za pośrednictwem NKTY przerwania, wywołaj metodę UseNetTopologySuite w konstruktorze opcji DbContext dostawcy.</span><span class="sxs-lookup"><span data-stu-id="576a2-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="576a2-128">Na przykład w przypadku SQL Server należy wywołać tę metodę.</span><span class="sxs-lookup"><span data-stu-id="576a2-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="576a2-129">Istnieje kilka typów danych przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="576a2-129">There are several spatial data types.</span></span> <span data-ttu-id="576a2-130">Używany typ zależy od typów kształtów, które mają być dozwolone.</span><span class="sxs-lookup"><span data-stu-id="576a2-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="576a2-131">Poniżej znajduje się hierarchia typów NKTY przerwania, których można użyć do właściwości w modelu.</span><span class="sxs-lookup"><span data-stu-id="576a2-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="576a2-132">Znajdują się one w przestrzeni nazw `NetTopologySuite.Geometries`.</span><span class="sxs-lookup"><span data-stu-id="576a2-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="576a2-133">Geometrii</span><span class="sxs-lookup"><span data-stu-id="576a2-133">Geometry</span></span>
  * <span data-ttu-id="576a2-134">Moment</span><span class="sxs-lookup"><span data-stu-id="576a2-134">Point</span></span>
  * <span data-ttu-id="576a2-135">LineString</span><span class="sxs-lookup"><span data-stu-id="576a2-135">LineString</span></span>
  * <span data-ttu-id="576a2-136">Tworząc</span><span class="sxs-lookup"><span data-stu-id="576a2-136">Polygon</span></span>
  * <span data-ttu-id="576a2-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="576a2-137">GeometryCollection</span></span>
    * <span data-ttu-id="576a2-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="576a2-138">MultiPoint</span></span>
    * <span data-ttu-id="576a2-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="576a2-139">MultiLineString</span></span>
    * <span data-ttu-id="576a2-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="576a2-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="576a2-141">CircularString, CompoundCurve i CurePolygon nie są obsługiwane przez NKTY przerwania.</span><span class="sxs-lookup"><span data-stu-id="576a2-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="576a2-142">Użycie podstawowego typu geometrii umożliwia określenie dowolnego typu kształtu przez właściwość.</span><span class="sxs-lookup"><span data-stu-id="576a2-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="576a2-143">Poniższe klasy jednostek mogą służyć do mapowania tabel w [przykładowej bazie danych](https://go.microsoft.com/fwlink/?LinkID=800630)na całym świecie.</span><span class="sxs-lookup"><span data-stu-id="576a2-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="576a2-144">Tworzenie wartości</span><span class="sxs-lookup"><span data-stu-id="576a2-144">Creating values</span></span>

<span data-ttu-id="576a2-145">Można użyć konstruktorów do tworzenia obiektów geometrycznych; jednak NKTY przerwania zaleca się użycie fabryki geometrycznej.</span><span class="sxs-lookup"><span data-stu-id="576a2-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="576a2-146">Pozwala to określić domyślny SRID (System referencyjny przestrzennej używany przez współrzędne) i zapewnia kontrolę nad bardziej zaawansowanymi elementami, takimi jak model precyzji (używany podczas obliczeń) i sekwencją współrzędnych (określa, które współrzędne są wymiarami i miary--są dostępne).</span><span class="sxs-lookup"><span data-stu-id="576a2-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="576a2-147">4326 dotyczy WGS 84, standardowego używanego w systemach GPS i innych systemów geograficznych.</span><span class="sxs-lookup"><span data-stu-id="576a2-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="576a2-148">Długości i szerokości geograficznej</span><span class="sxs-lookup"><span data-stu-id="576a2-148">Longitude and Latitude</span></span>

<span data-ttu-id="576a2-149">Współrzędne w NKTY przerwania są pod względem wartości X i Y.</span><span class="sxs-lookup"><span data-stu-id="576a2-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="576a2-150">Aby przedstawić długość i szerokość geograficzną, użyj X dla długości geograficznej i Y dla szerokości geograficznej.</span><span class="sxs-lookup"><span data-stu-id="576a2-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="576a2-151">Należy pamiętać, że jest to **wsteczne** w formacie `latitude, longitude`, w którym są zwykle wyświetlane te wartości.</span><span class="sxs-lookup"><span data-stu-id="576a2-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="576a2-152">SRID zignorowane podczas operacji klienta</span><span class="sxs-lookup"><span data-stu-id="576a2-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="576a2-153">NKTY przerwania ignoruje wartości SRID podczas operacji.</span><span class="sxs-lookup"><span data-stu-id="576a2-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="576a2-154">Przyjęto założenie układu współrzędnych.</span><span class="sxs-lookup"><span data-stu-id="576a2-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="576a2-155">Oznacza to, że jeśli określisz współrzędne w zakresie długości i szerokości geograficznej, niektóre wartości oceniane przez klienta, takie jak odległość, długość i obszar, będą znajdować się w stopniach, a nie w metrach.</span><span class="sxs-lookup"><span data-stu-id="576a2-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="576a2-156">Aby uzyskać bardziej zrozumiałe wartości, najpierw należy umieścić współrzędne w innym układzie współrzędnych przy użyciu biblioteki, takiej jak [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) przed obliczeniem tych wartości.</span><span class="sxs-lookup"><span data-stu-id="576a2-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="576a2-157">Jeśli operacja jest Szacowana przez serwer EF Core za pośrednictwem SQL, jednostka wyniku zostanie określona przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="576a2-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="576a2-158">Oto przykład użycia ProjNet4GeoAPI do obliczenia odległości między dwoma miastami.</span><span class="sxs-lookup"><span data-stu-id="576a2-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="576a2-159">Wykonanie zapytania o dane</span><span class="sxs-lookup"><span data-stu-id="576a2-159">Querying Data</span></span>

<span data-ttu-id="576a2-160">W LINQ, metody i właściwości NKTY przerwania dostępne jako funkcje bazy danych zostaną przetłumaczone na SQL.</span><span class="sxs-lookup"><span data-stu-id="576a2-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="576a2-161">Na przykład odległość i zawiera metody są tłumaczone w poniższych zapytaniach.</span><span class="sxs-lookup"><span data-stu-id="576a2-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="576a2-162">W tabeli na końcu tego artykułu przedstawiono, które elementy członkowskie są obsługiwane przez różnych dostawców EF Core.</span><span class="sxs-lookup"><span data-stu-id="576a2-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="576a2-163">Oprogramowanie SQL Server</span><span class="sxs-lookup"><span data-stu-id="576a2-163">SQL Server</span></span>

<span data-ttu-id="576a2-164">Jeśli używasz SQL Server, musisz wiedzieć o kilku dodatkowych kwestiach.</span><span class="sxs-lookup"><span data-stu-id="576a2-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="576a2-165">Geografia lub geometria</span><span class="sxs-lookup"><span data-stu-id="576a2-165">Geography or geometry</span></span>

<span data-ttu-id="576a2-166">Domyślnie właściwości przestrzenne są mapowane do `geography` kolumn w SQL Server.</span><span class="sxs-lookup"><span data-stu-id="576a2-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="576a2-167">Aby użyć `geometry`, należy [skonfigurować typ kolumny](xref:core/modeling/entity-properties#column-data-types) w modelu.</span><span class="sxs-lookup"><span data-stu-id="576a2-167">To use `geometry`, [configure the column type](xref:core/modeling/entity-properties#column-data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="576a2-168">Pierścienie wielokątów geograficznych</span><span class="sxs-lookup"><span data-stu-id="576a2-168">Geography polygon rings</span></span>

<span data-ttu-id="576a2-169">W przypadku używania `geography` typu kolumny SQL Server nakładają dodatkowe wymagania dotyczące pierścienia zewnętrznego (lub powłoki) i wewnętrznych pierścieni (lub dziur).</span><span class="sxs-lookup"><span data-stu-id="576a2-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="576a2-170">Pierścień zewnętrzny musi być zorientowany w lewo i w prawo.</span><span class="sxs-lookup"><span data-stu-id="576a2-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="576a2-171">NKTY przerwania sprawdza to przed wysłaniem wartości do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="576a2-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="576a2-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="576a2-172">FullGlobe</span></span>

<span data-ttu-id="576a2-173">SQL Server ma niestandardowy typ geometrii reprezentujący pełny Globus przy użyciu typu kolumny `geography`.</span><span class="sxs-lookup"><span data-stu-id="576a2-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="576a2-174">Ma również sposób reprezentowania wielokątów w oparciu o pełny Globus (bez pierścienia zewnętrznego).</span><span class="sxs-lookup"><span data-stu-id="576a2-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="576a2-175">Żadna z tych elementów nie jest obsługiwana przez NKTY przerwania.</span><span class="sxs-lookup"><span data-stu-id="576a2-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="576a2-176">FullGlobe i wielokąty oparte na nim nie są obsługiwane przez NKTY przerwania.</span><span class="sxs-lookup"><span data-stu-id="576a2-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="576a2-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="576a2-177">SQLite</span></span>

<span data-ttu-id="576a2-178">Poniżej przedstawiono dodatkowe informacje dotyczące tych, które są używane przez program SQLite.</span><span class="sxs-lookup"><span data-stu-id="576a2-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="576a2-179">Instalowanie SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="576a2-179">Installing SpatiaLite</span></span>

<span data-ttu-id="576a2-180">W systemie Windows natywna biblioteka mod_spatialite jest dystrybuowana jako zależność pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="576a2-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="576a2-181">Inne platformy muszą zainstalować je oddzielnie.</span><span class="sxs-lookup"><span data-stu-id="576a2-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="576a2-182">Jest to zazwyczaj realizowane przy użyciu Menedżera pakietów oprogramowania.</span><span class="sxs-lookup"><span data-stu-id="576a2-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="576a2-183">Na przykład można użyć APT w Ubuntu i oprogramowania Homebrew na MacOS.</span><span class="sxs-lookup"><span data-stu-id="576a2-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

<span data-ttu-id="576a2-184">Niestety, nowsze wersje programu PROJ (zależność SpatiaLite) są niezgodne z domyślnym [pakietem SQLITEPCLRAW](/dotnet/standard/data/sqlite/custom-versions#bundles)EF.</span><span class="sxs-lookup"><span data-stu-id="576a2-184">Unfortunately, newer versions of PROJ (a dependency of SpatiaLite) are incompatible with EF's default [SQLitePCLRaw bundle](/dotnet/standard/data/sqlite/custom-versions#bundles).</span></span> <span data-ttu-id="576a2-185">Można to obejść przez utworzenie niestandardowego [dostawcy SQLitePCLRaw](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) , który korzysta z biblioteki programu SQLite systemu, lub można zainstalować niestandardową kompilację SpatiaLite.</span><span class="sxs-lookup"><span data-stu-id="576a2-185">You can work around this by either creating a custom [SQLitePCLRaw provider](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) that uses the system SQLite library, or you can install a custom build of SpatiaLite disabling PROJ support.</span></span>

``` sh
curl https://www.gaia-gis.it/gaia-sins/libspatialite-4.3.0a.tar.gz | tar -xz
cd libspatialite-4.3.0a

if [[ `uname -s` == Darwin* ]]; then
    # Mac OS requires some minor patching
    sed -i "" "s/shrext_cmds='\`test \\.\$module = .yes && echo .so \\|\\| echo \\.dylib\`'/shrext_cmds='.dylib'/g" configure
fi

./configure --disable-proj
make
make install
```

### <a name="configuring-srid"></a><span data-ttu-id="576a2-186">Konfigurowanie SRID</span><span class="sxs-lookup"><span data-stu-id="576a2-186">Configuring SRID</span></span>

<span data-ttu-id="576a2-187">W SpatiaLite, kolumny muszą określać SRID na kolumnę.</span><span class="sxs-lookup"><span data-stu-id="576a2-187">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="576a2-188">Domyślny SRID jest `0`.</span><span class="sxs-lookup"><span data-stu-id="576a2-188">The default SRID is `0`.</span></span> <span data-ttu-id="576a2-189">Określ inny SRID przy użyciu metody ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="576a2-189">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="576a2-190">Wymiar</span><span class="sxs-lookup"><span data-stu-id="576a2-190">Dimension</span></span>

<span data-ttu-id="576a2-191">Podobnie jak w przypadku SRID, wymiar kolumny (lub współrzędnej) jest również określony jako część kolumny.</span><span class="sxs-lookup"><span data-stu-id="576a2-191">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="576a2-192">Domyślnymi współrzędnymi są X i Y. Włącz dodatkowe współrzędne (Z i M) przy użyciu metody ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="576a2-192">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="576a2-193">Operacje tłumaczone</span><span class="sxs-lookup"><span data-stu-id="576a2-193">Translated Operations</span></span>

<span data-ttu-id="576a2-194">W tej tabeli przedstawiono, które elementy członkowskie NKTY przerwania są tłumaczone na SQL przez każdego dostawcę EF Core.</span><span class="sxs-lookup"><span data-stu-id="576a2-194">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="576a2-195">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="576a2-195">NetTopologySuite</span></span> | <span data-ttu-id="576a2-196">SQL Server (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-196">SQL Server (geometry)</span></span> | <span data-ttu-id="576a2-197">SQL Server (Geografia)</span><span class="sxs-lookup"><span data-stu-id="576a2-197">SQL Server (geography)</span></span> | <span data-ttu-id="576a2-198">SQLite</span><span class="sxs-lookup"><span data-stu-id="576a2-198">SQLite</span></span> | <span data-ttu-id="576a2-199">Npgsql</span><span class="sxs-lookup"><span data-stu-id="576a2-199">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="576a2-200">Geometria. obszar</span><span class="sxs-lookup"><span data-stu-id="576a2-200">Geometry.Area</span></span> | <span data-ttu-id="576a2-201">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-201">✔</span></span> | <span data-ttu-id="576a2-202">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-202">✔</span></span> | <span data-ttu-id="576a2-203">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-203">✔</span></span> | <span data-ttu-id="576a2-204">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-204">✔</span></span>
<span data-ttu-id="576a2-205">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="576a2-205">Geometry.AsBinary()</span></span> | <span data-ttu-id="576a2-206">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-206">✔</span></span> | <span data-ttu-id="576a2-207">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-207">✔</span></span> | <span data-ttu-id="576a2-208">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-208">✔</span></span> | <span data-ttu-id="576a2-209">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-209">✔</span></span>
<span data-ttu-id="576a2-210">Geometry. AsText ()</span><span class="sxs-lookup"><span data-stu-id="576a2-210">Geometry.AsText()</span></span> | <span data-ttu-id="576a2-211">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-211">✔</span></span> | <span data-ttu-id="576a2-212">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-212">✔</span></span> | <span data-ttu-id="576a2-213">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-213">✔</span></span> | <span data-ttu-id="576a2-214">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-214">✔</span></span>
<span data-ttu-id="576a2-215">Geometria. granica</span><span class="sxs-lookup"><span data-stu-id="576a2-215">Geometry.Boundary</span></span> | <span data-ttu-id="576a2-216">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-216">✔</span></span> | | <span data-ttu-id="576a2-217">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-217">✔</span></span> | <span data-ttu-id="576a2-218">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-218">✔</span></span>
<span data-ttu-id="576a2-219">Geometry. Buffer (Double)</span><span class="sxs-lookup"><span data-stu-id="576a2-219">Geometry.Buffer(double)</span></span> | <span data-ttu-id="576a2-220">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-220">✔</span></span> | <span data-ttu-id="576a2-221">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-221">✔</span></span> | <span data-ttu-id="576a2-222">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-222">✔</span></span> | <span data-ttu-id="576a2-223">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-223">✔</span></span>
<span data-ttu-id="576a2-224">Geometry. Buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="576a2-224">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="576a2-225">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-225">✔</span></span> | <span data-ttu-id="576a2-226">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-226">✔</span></span>
<span data-ttu-id="576a2-227">Geometria. centroida</span><span class="sxs-lookup"><span data-stu-id="576a2-227">Geometry.Centroid</span></span> | <span data-ttu-id="576a2-228">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-228">✔</span></span> | | <span data-ttu-id="576a2-229">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-229">✔</span></span> | <span data-ttu-id="576a2-230">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-230">✔</span></span>
<span data-ttu-id="576a2-231">Geometry. Contains (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-231">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="576a2-232">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-232">✔</span></span> | <span data-ttu-id="576a2-233">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-233">✔</span></span> | <span data-ttu-id="576a2-234">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-234">✔</span></span> | <span data-ttu-id="576a2-235">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-235">✔</span></span>
<span data-ttu-id="576a2-236">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="576a2-236">Geometry.ConvexHull()</span></span> | <span data-ttu-id="576a2-237">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-237">✔</span></span> | <span data-ttu-id="576a2-238">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-238">✔</span></span> | <span data-ttu-id="576a2-239">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-239">✔</span></span> | <span data-ttu-id="576a2-240">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-240">✔</span></span>
<span data-ttu-id="576a2-241">Geometry. CoveredBy (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-241">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="576a2-242">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-242">✔</span></span> | <span data-ttu-id="576a2-243">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-243">✔</span></span>
<span data-ttu-id="576a2-244">Geometria. okładki (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-244">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="576a2-245">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-245">✔</span></span> | <span data-ttu-id="576a2-246">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-246">✔</span></span>
<span data-ttu-id="576a2-247">Geometry. crosss (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-247">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="576a2-248">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-248">✔</span></span> | | <span data-ttu-id="576a2-249">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-249">✔</span></span> | <span data-ttu-id="576a2-250">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-250">✔</span></span>
<span data-ttu-id="576a2-251">Geometry. różnica (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-251">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="576a2-252">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-252">✔</span></span> | <span data-ttu-id="576a2-253">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-253">✔</span></span> | <span data-ttu-id="576a2-254">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-254">✔</span></span> | <span data-ttu-id="576a2-255">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-255">✔</span></span>
<span data-ttu-id="576a2-256">Geometria. wymiar</span><span class="sxs-lookup"><span data-stu-id="576a2-256">Geometry.Dimension</span></span> | <span data-ttu-id="576a2-257">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-257">✔</span></span> | <span data-ttu-id="576a2-258">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-258">✔</span></span> | <span data-ttu-id="576a2-259">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-259">✔</span></span> | <span data-ttu-id="576a2-260">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-260">✔</span></span>
<span data-ttu-id="576a2-261">Geometria. rozłączna (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-261">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="576a2-262">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-262">✔</span></span> | <span data-ttu-id="576a2-263">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-263">✔</span></span> | <span data-ttu-id="576a2-264">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-264">✔</span></span> | <span data-ttu-id="576a2-265">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-265">✔</span></span>
<span data-ttu-id="576a2-266">Geometria. Distance (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-266">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="576a2-267">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-267">✔</span></span> | <span data-ttu-id="576a2-268">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-268">✔</span></span> | <span data-ttu-id="576a2-269">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-269">✔</span></span> | <span data-ttu-id="576a2-270">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-270">✔</span></span>
<span data-ttu-id="576a2-271">Geometria. koperta</span><span class="sxs-lookup"><span data-stu-id="576a2-271">Geometry.Envelope</span></span> | <span data-ttu-id="576a2-272">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-272">✔</span></span> | | <span data-ttu-id="576a2-273">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-273">✔</span></span> | <span data-ttu-id="576a2-274">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-274">✔</span></span>
<span data-ttu-id="576a2-275">Geometry. EqualsExact (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-275">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="576a2-276">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-276">✔</span></span>
<span data-ttu-id="576a2-277">Geometry. EqualsTopologically (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-277">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="576a2-278">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-278">✔</span></span> | <span data-ttu-id="576a2-279">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-279">✔</span></span> | <span data-ttu-id="576a2-280">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-280">✔</span></span> | <span data-ttu-id="576a2-281">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-281">✔</span></span>
<span data-ttu-id="576a2-282">Geometry. Geometrytype</span><span class="sxs-lookup"><span data-stu-id="576a2-282">Geometry.GeometryType</span></span> | <span data-ttu-id="576a2-283">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-283">✔</span></span> | <span data-ttu-id="576a2-284">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-284">✔</span></span> | <span data-ttu-id="576a2-285">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-285">✔</span></span> | <span data-ttu-id="576a2-286">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-286">✔</span></span>
<span data-ttu-id="576a2-287">Geometry. GetGeometryN (int)</span><span class="sxs-lookup"><span data-stu-id="576a2-287">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="576a2-288">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-288">✔</span></span> | | <span data-ttu-id="576a2-289">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-289">✔</span></span> | <span data-ttu-id="576a2-290">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-290">✔</span></span>
<span data-ttu-id="576a2-291">Geometria. InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="576a2-291">Geometry.InteriorPoint</span></span> | <span data-ttu-id="576a2-292">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-292">✔</span></span> | | <span data-ttu-id="576a2-293">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-293">✔</span></span> | <span data-ttu-id="576a2-294">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-294">✔</span></span>
<span data-ttu-id="576a2-295">Geometria. część wspólna (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-295">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="576a2-296">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-296">✔</span></span> | <span data-ttu-id="576a2-297">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-297">✔</span></span> | <span data-ttu-id="576a2-298">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-298">✔</span></span> | <span data-ttu-id="576a2-299">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-299">✔</span></span>
<span data-ttu-id="576a2-300">Geometria. Intersects (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-300">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="576a2-301">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-301">✔</span></span> | <span data-ttu-id="576a2-302">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-302">✔</span></span> | <span data-ttu-id="576a2-303">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-303">✔</span></span> | <span data-ttu-id="576a2-304">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-304">✔</span></span>
<span data-ttu-id="576a2-305">Geometria. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="576a2-305">Geometry.IsEmpty</span></span> | <span data-ttu-id="576a2-306">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-306">✔</span></span> | <span data-ttu-id="576a2-307">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-307">✔</span></span> | <span data-ttu-id="576a2-308">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-308">✔</span></span> | <span data-ttu-id="576a2-309">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-309">✔</span></span>
<span data-ttu-id="576a2-310">Geometria. IsSimple</span><span class="sxs-lookup"><span data-stu-id="576a2-310">Geometry.IsSimple</span></span> | <span data-ttu-id="576a2-311">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-311">✔</span></span> | | <span data-ttu-id="576a2-312">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-312">✔</span></span> | <span data-ttu-id="576a2-313">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-313">✔</span></span>
<span data-ttu-id="576a2-314">Geometria. IsValid</span><span class="sxs-lookup"><span data-stu-id="576a2-314">Geometry.IsValid</span></span> | <span data-ttu-id="576a2-315">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-315">✔</span></span> | <span data-ttu-id="576a2-316">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-316">✔</span></span> | <span data-ttu-id="576a2-317">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-317">✔</span></span> | <span data-ttu-id="576a2-318">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-318">✔</span></span>
<span data-ttu-id="576a2-319">Geometry. IsWithinDistance (Geometria, Double)</span><span class="sxs-lookup"><span data-stu-id="576a2-319">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="576a2-320">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-320">✔</span></span> | | <span data-ttu-id="576a2-321">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-321">✔</span></span> | <span data-ttu-id="576a2-322">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-322">✔</span></span>
<span data-ttu-id="576a2-323">Geometria. Długość</span><span class="sxs-lookup"><span data-stu-id="576a2-323">Geometry.Length</span></span> | <span data-ttu-id="576a2-324">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-324">✔</span></span> | <span data-ttu-id="576a2-325">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-325">✔</span></span> | <span data-ttu-id="576a2-326">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-326">✔</span></span> | <span data-ttu-id="576a2-327">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-327">✔</span></span>
<span data-ttu-id="576a2-328">Geometria. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="576a2-328">Geometry.NumGeometries</span></span> | <span data-ttu-id="576a2-329">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-329">✔</span></span> | <span data-ttu-id="576a2-330">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-330">✔</span></span> | <span data-ttu-id="576a2-331">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-331">✔</span></span> | <span data-ttu-id="576a2-332">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-332">✔</span></span>
<span data-ttu-id="576a2-333">Geometria. NumPoints</span><span class="sxs-lookup"><span data-stu-id="576a2-333">Geometry.NumPoints</span></span> | <span data-ttu-id="576a2-334">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-334">✔</span></span> | <span data-ttu-id="576a2-335">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-335">✔</span></span> | <span data-ttu-id="576a2-336">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-336">✔</span></span> | <span data-ttu-id="576a2-337">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-337">✔</span></span>
<span data-ttu-id="576a2-338">Geometria. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="576a2-338">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="576a2-339">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-339">✔</span></span> | <span data-ttu-id="576a2-340">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-340">✔</span></span> | <span data-ttu-id="576a2-341">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-341">✔</span></span> | <span data-ttu-id="576a2-342">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-342">✔</span></span>
<span data-ttu-id="576a2-343">Geometria. nakładanie się (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-343">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="576a2-344">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-344">✔</span></span> | <span data-ttu-id="576a2-345">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-345">✔</span></span> | <span data-ttu-id="576a2-346">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-346">✔</span></span> | <span data-ttu-id="576a2-347">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-347">✔</span></span>
<span data-ttu-id="576a2-348">Geometria. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="576a2-348">Geometry.PointOnSurface</span></span> | <span data-ttu-id="576a2-349">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-349">✔</span></span> | | <span data-ttu-id="576a2-350">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-350">✔</span></span> | <span data-ttu-id="576a2-351">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-351">✔</span></span>
<span data-ttu-id="576a2-352">Geometry. rerelacja (Geometria, ciąg)</span><span class="sxs-lookup"><span data-stu-id="576a2-352">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="576a2-353">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-353">✔</span></span> | | <span data-ttu-id="576a2-354">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-354">✔</span></span> | <span data-ttu-id="576a2-355">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-355">✔</span></span>
<span data-ttu-id="576a2-356">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="576a2-356">Geometry.Reverse()</span></span> | | | <span data-ttu-id="576a2-357">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-357">✔</span></span> | <span data-ttu-id="576a2-358">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-358">✔</span></span>
<span data-ttu-id="576a2-359">Geometria. SRID</span><span class="sxs-lookup"><span data-stu-id="576a2-359">Geometry.SRID</span></span> | <span data-ttu-id="576a2-360">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-360">✔</span></span> | <span data-ttu-id="576a2-361">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-361">✔</span></span> | <span data-ttu-id="576a2-362">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-362">✔</span></span> | <span data-ttu-id="576a2-363">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-363">✔</span></span>
<span data-ttu-id="576a2-364">Geometry. SymmetricDifference (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-364">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="576a2-365">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-365">✔</span></span> | <span data-ttu-id="576a2-366">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-366">✔</span></span> | <span data-ttu-id="576a2-367">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-367">✔</span></span> | <span data-ttu-id="576a2-368">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-368">✔</span></span>
<span data-ttu-id="576a2-369">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="576a2-369">Geometry.ToBinary()</span></span> | <span data-ttu-id="576a2-370">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-370">✔</span></span> | <span data-ttu-id="576a2-371">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-371">✔</span></span> | <span data-ttu-id="576a2-372">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-372">✔</span></span> | <span data-ttu-id="576a2-373">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-373">✔</span></span>
<span data-ttu-id="576a2-374">Geometry. ToText ()</span><span class="sxs-lookup"><span data-stu-id="576a2-374">Geometry.ToText()</span></span> | <span data-ttu-id="576a2-375">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-375">✔</span></span> | <span data-ttu-id="576a2-376">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-376">✔</span></span> | <span data-ttu-id="576a2-377">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-377">✔</span></span> | <span data-ttu-id="576a2-378">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-378">✔</span></span>
<span data-ttu-id="576a2-379">Geometria. touch (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-379">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="576a2-380">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-380">✔</span></span> | | <span data-ttu-id="576a2-381">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-381">✔</span></span> | <span data-ttu-id="576a2-382">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-382">✔</span></span>
<span data-ttu-id="576a2-383">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="576a2-383">Geometry.Union()</span></span> | | | <span data-ttu-id="576a2-384">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-384">✔</span></span> | <span data-ttu-id="576a2-385">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-385">✔</span></span>
<span data-ttu-id="576a2-386">Geometry. Union (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-386">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="576a2-387">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-387">✔</span></span> | <span data-ttu-id="576a2-388">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-388">✔</span></span> | <span data-ttu-id="576a2-389">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-389">✔</span></span> | <span data-ttu-id="576a2-390">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-390">✔</span></span>
<span data-ttu-id="576a2-391">Geometria. w elemencie (Geometria)</span><span class="sxs-lookup"><span data-stu-id="576a2-391">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="576a2-392">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-392">✔</span></span> | <span data-ttu-id="576a2-393">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-393">✔</span></span> | <span data-ttu-id="576a2-394">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-394">✔</span></span> | <span data-ttu-id="576a2-395">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-395">✔</span></span>
<span data-ttu-id="576a2-396">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="576a2-396">GeometryCollection.Count</span></span> | <span data-ttu-id="576a2-397">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-397">✔</span></span> | <span data-ttu-id="576a2-398">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-398">✔</span></span> | <span data-ttu-id="576a2-399">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-399">✔</span></span> | <span data-ttu-id="576a2-400">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-400">✔</span></span>
<span data-ttu-id="576a2-401">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="576a2-401">GeometryCollection[int]</span></span> | <span data-ttu-id="576a2-402">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-402">✔</span></span> | <span data-ttu-id="576a2-403">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-403">✔</span></span> | <span data-ttu-id="576a2-404">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-404">✔</span></span> | <span data-ttu-id="576a2-405">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-405">✔</span></span>
<span data-ttu-id="576a2-406">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="576a2-406">LineString.Count</span></span> | <span data-ttu-id="576a2-407">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-407">✔</span></span> | <span data-ttu-id="576a2-408">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-408">✔</span></span> | <span data-ttu-id="576a2-409">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-409">✔</span></span> | <span data-ttu-id="576a2-410">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-410">✔</span></span>
<span data-ttu-id="576a2-411">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="576a2-411">LineString.EndPoint</span></span> | <span data-ttu-id="576a2-412">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-412">✔</span></span> | <span data-ttu-id="576a2-413">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-413">✔</span></span> | <span data-ttu-id="576a2-414">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-414">✔</span></span> | <span data-ttu-id="576a2-415">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-415">✔</span></span>
<span data-ttu-id="576a2-416">LineString. GetPointN (int)</span><span class="sxs-lookup"><span data-stu-id="576a2-416">LineString.GetPointN(int)</span></span> | <span data-ttu-id="576a2-417">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-417">✔</span></span> | <span data-ttu-id="576a2-418">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-418">✔</span></span> | <span data-ttu-id="576a2-419">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-419">✔</span></span> | <span data-ttu-id="576a2-420">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-420">✔</span></span>
<span data-ttu-id="576a2-421">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="576a2-421">LineString.IsClosed</span></span> | <span data-ttu-id="576a2-422">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-422">✔</span></span> | <span data-ttu-id="576a2-423">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-423">✔</span></span> | <span data-ttu-id="576a2-424">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-424">✔</span></span> | <span data-ttu-id="576a2-425">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-425">✔</span></span>
<span data-ttu-id="576a2-426">LineString. IsRing</span><span class="sxs-lookup"><span data-stu-id="576a2-426">LineString.IsRing</span></span> | <span data-ttu-id="576a2-427">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-427">✔</span></span> | | <span data-ttu-id="576a2-428">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-428">✔</span></span> | <span data-ttu-id="576a2-429">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-429">✔</span></span>
<span data-ttu-id="576a2-430">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="576a2-430">LineString.StartPoint</span></span> | <span data-ttu-id="576a2-431">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-431">✔</span></span> | <span data-ttu-id="576a2-432">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-432">✔</span></span> | <span data-ttu-id="576a2-433">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-433">✔</span></span> | <span data-ttu-id="576a2-434">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-434">✔</span></span>
<span data-ttu-id="576a2-435">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="576a2-435">MultiLineString.IsClosed</span></span> | <span data-ttu-id="576a2-436">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-436">✔</span></span> | <span data-ttu-id="576a2-437">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-437">✔</span></span> | <span data-ttu-id="576a2-438">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-438">✔</span></span> | <span data-ttu-id="576a2-439">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-439">✔</span></span>
<span data-ttu-id="576a2-440">Punkt. M</span><span class="sxs-lookup"><span data-stu-id="576a2-440">Point.M</span></span> | <span data-ttu-id="576a2-441">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-441">✔</span></span> | <span data-ttu-id="576a2-442">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-442">✔</span></span> | <span data-ttu-id="576a2-443">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-443">✔</span></span> | <span data-ttu-id="576a2-444">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-444">✔</span></span>
<span data-ttu-id="576a2-445">Punkt. X</span><span class="sxs-lookup"><span data-stu-id="576a2-445">Point.X</span></span> | <span data-ttu-id="576a2-446">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-446">✔</span></span> | <span data-ttu-id="576a2-447">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-447">✔</span></span> | <span data-ttu-id="576a2-448">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-448">✔</span></span> | <span data-ttu-id="576a2-449">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-449">✔</span></span>
<span data-ttu-id="576a2-450">Punkt. Y</span><span class="sxs-lookup"><span data-stu-id="576a2-450">Point.Y</span></span> | <span data-ttu-id="576a2-451">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-451">✔</span></span> | <span data-ttu-id="576a2-452">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-452">✔</span></span> | <span data-ttu-id="576a2-453">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-453">✔</span></span> | <span data-ttu-id="576a2-454">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-454">✔</span></span>
<span data-ttu-id="576a2-455">Punkt. Z</span><span class="sxs-lookup"><span data-stu-id="576a2-455">Point.Z</span></span> | <span data-ttu-id="576a2-456">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-456">✔</span></span> | <span data-ttu-id="576a2-457">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-457">✔</span></span> | <span data-ttu-id="576a2-458">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-458">✔</span></span> | <span data-ttu-id="576a2-459">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-459">✔</span></span>
<span data-ttu-id="576a2-460">Wielokąt. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="576a2-460">Polygon.ExteriorRing</span></span> | <span data-ttu-id="576a2-461">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-461">✔</span></span> | <span data-ttu-id="576a2-462">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-462">✔</span></span> | <span data-ttu-id="576a2-463">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-463">✔</span></span> | <span data-ttu-id="576a2-464">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-464">✔</span></span>
<span data-ttu-id="576a2-465">Wielokąt. GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="576a2-465">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="576a2-466">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-466">✔</span></span> | <span data-ttu-id="576a2-467">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-467">✔</span></span> | <span data-ttu-id="576a2-468">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-468">✔</span></span> | <span data-ttu-id="576a2-469">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-469">✔</span></span>
<span data-ttu-id="576a2-470">Wielokąt. NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="576a2-470">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="576a2-471">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-471">✔</span></span> | <span data-ttu-id="576a2-472">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-472">✔</span></span> | <span data-ttu-id="576a2-473">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-473">✔</span></span> | <span data-ttu-id="576a2-474">✔</span><span class="sxs-lookup"><span data-stu-id="576a2-474">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="576a2-475">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="576a2-475">Additional resources</span></span>

* [<span data-ttu-id="576a2-476">Dane przestrzenne w SQL Server</span><span class="sxs-lookup"><span data-stu-id="576a2-476">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="576a2-477">Strona główna SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="576a2-477">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="576a2-478">Dokumentacja przestrzenna Npgsql</span><span class="sxs-lookup"><span data-stu-id="576a2-478">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="576a2-479">Dokumentacja PostGIS</span><span class="sxs-lookup"><span data-stu-id="576a2-479">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
