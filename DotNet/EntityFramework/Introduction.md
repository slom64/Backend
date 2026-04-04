- Entity framework is called persistence framework where we define or objects and leave the implementation details for dealing with objects to the framework.
- It enble us to integrate with different types of databases.
- In `.NET` we have single `DbContext` that contain all the tables of database as `DbSet<Entity>`. You can separate them based on domain.




```xml
<!--Things to download -->
install-package EntityFramework

// edit App.config
<connectionStrings>
	<add name="DbContextClass" connectionString="data source=.\SQLEXPRESS; initial catalog=DataBaseName; integrated security=SSPI" providerName="System.Data.SqlClient"/>
</connectionStrings>

<!-- Enable code migration for database "We do it only once"-->
enable-migrations

```