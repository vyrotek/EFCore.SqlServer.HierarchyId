EntityFrameworkCore.SqlServer.HierarchyId
========================================

![build status](https://img.shields.io/github/workflow/status/efcore/EFCore.SqlServer.HierarchyId/.NET%20Core/master) [![latest version](https://img.shields.io/nuget/v/EntityFrameworkCore.SqlServer.HierarchyId)](https://www.nuget.org/packages/EntityFrameworkCore.SqlServer.HierarchyId)

Adds hierarchyid support to the SQL Server EF Core provider.

Installation
------------

The latest stable version is available on [NuGet](https://www.nuget.org/packages/EntityFrameworkCore.SqlServer.HierarchyId).

```sh
dotnet add package EntityFrameworkCore.SqlServer.HierarchyId
```

Usage
-----

Enable hierarchyid support by calling UseHierarchyId inside UseSqlServer. UseSqlServer is is typically called inside `Startup.ConfigureServices` or `OnConfiguring` of your DbContext type.

```cs
options.UseSqlServer(
    connectionString,
    x => x.UseHierarchyId());
```

Add `HierarchyId` properties to your entity types.

```cs
class Patriarch
{
    public HierarchyId Id { get; set; }
    public string Name { get; set; }
}
```

Insert data.

```cs
dbContext.AddRange(
    new Patriarch { Id = HierarchyId.GetRoot(), Name = "Abraham" },
    new Patriarch { Id = HierarchyId.Parse("/1/"), Name = "Isaac" },
    new Patriarch { Id = HierarchyId.Parse("/1/1/"), Name = "Jacob" });
dbContext.SaveChanges();
```

Query.

```cs
var thirdGeneration = from p in dbContext.Patriarchs
                      where p.Id.GetLevel() == 2
                      select p;
```

See also
--------

* [Hierarchical Data (SQL Server)](https://docs.microsoft.com/sql/relational-databases/hierarchical-data-sql-server)
* [Entity Framework documentation](https://docs.microsoft.com/ef/)
