* [Creating Database Plugins](#creating-database-plugins)
* [Exploring Existing Database Implementations](#exploring-existing-database-implementations)
* [Deploying Database Plugins](#deploying-database-plugins)

## Creating Database Plugins

PDI uses database plugins to support specific database systems beyond generic JDBC functionality. A database plugin helps in the following areas:

* constructing connection strings
* passing connection settings to JDBC
* dialect-aware SQL generation
* detecting special abilities and limitations of JDBC drivers

A database plugin introduces a new entry in the PDI database dialog. 

![](https://help.pentaho.com/@api/deki/files/8262/database_connection3.png)

PDI database plugins consist of a single Java class that implements the interface `org.pentaho.di.core.database.DatabaseInterface`.

In order for PDI to recognize the database plugin, the class implementing `DatabaseInterface` must also be annotated with the Java annotation `org.pentaho.di.core.plugins.DatabaseMetaPlugin`.

Supply these annotation attributes.

Attribute | Description
--------- | -----------
type | A globally unique ID for database plugin
typeDescription | The label to use in the database dialog

It is recommended to extend `org.pentaho.di.core.database.BaseDatabaseMeta`, which provides default implementations for most of the methods in `DatabaseInterface`. Existing PDI database interfaces are a great source of information when developing a new database plugin.

The following section classifies some of the most commonly overridden methods. They can be roughly classified into three subject areas: information about connections, SQL dialect, and general capability flags.

1. Connection Details
    These methods are called when PDI establishes a connection to the database, or the database dialog is populated with database-specific defaults.

    * `public String getDriverClass()`
    * `public int getDefaultDatabasePort()`
    * `public int[] getAccessTypeList()`
    * `public boolean supportsOptionsInURL()`
    * `public String getURL()`

2. SQL Generation

    These methods are called when PDI constructs SQL.

    * `public String getFieldDefinition()`
    * `public String getAddColumnStatement()`
    * `public String getSQLColumnExists()`
    * `public String getSQLQueryFields()`

3. Capability Flags

    These methods are called when PDI determines the run-time characteristics of the database system. For instance, the database systems may support different notions of metadata retrieval.

    * `public boolean supportsTransactions()`
    * `public boolean releaseSavepoint()`
    * `public boolean supportsPreparedStatementMetadataRetrieval()`
    * `public boolean supportsResultSetMetadataRetrievalOnly()`

## Exploring Existing Database Implementations

PDI sources are invaluable when seeking example implementations of databases. Each of the PDI core database support classes is located in the `org.pentaho.di.core.database` package found in the `core/src` folder.

For example, here are the classes that define behavior for some major database systems.

Database | DatabaseInterface Class
-------- | -----------------------
MySQL | `org.pentaho.di.core.database.MySQLDatabaseMeta`
Oracle | `org.pentaho.di.core.database.OracleDatabaseMeta`
PostgreSQL | `org.pentaho.di.core.database.PostgreSQLDatabaseMeta`

When implementing a database plugin for a new database system, we recommended starting from an existing database class that already shares characteristics with the new database system. 

## Deploying Database Plugins

To deploy your plugin, follow the following steps.

1. Create a jar file containing your plugin class(es)
2. Create a new folder, give it a meaningful name, and place your jar file inside the folder
3. Place the plugin folder you just created in a specific location for PDI to find. Depending on how you use PDI, you need to copy the plugin folder to one or more locations as per the following list.
  * Deploying to Spoon or Carte

    Copy the plugin folder into this location: `design-tools/data-integration/plugins/databases`

    After restarting Spoon, the new database type is available from the PDI database dialog.

  * Deploying to Data Integration Server

    Copy the plugin folder to this location: `server/data-integration-server/pentaho-solutions/system/kettle/plugins/databases`

    After restarting the data integration server, the plugin is available to the server.

  * Deploying to BA Server

    Copy the plugin folder to this location: `server/biserver-ee/pentaho-solutions/system/kettle/plugins/databases`

    After restarting the BA Server, the plugin is available to the server.

4. When deploying database plugins, make sure to also deploy the corresponding JDBC drivers. See [Specify Data Connections][Specify Data Connections] for the DI Server for instructions about adding JDBC drivers.

[Specify Data Connections]: https://help.pentaho.com/Documentation/6.1/0P0/0U0/010