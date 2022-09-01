# Connect to Oracle from Databricks

Azure Databricks is a data analytics platform optimized for the Microsoft Azure cloud services platform. Azure Databricks offers three environments for developing data-intensive applications: Databricks SQL, Databricks Data Science Engineering, and Databricks Machine Learning.
This section describes one of the data sources, Oracle, that can be used in Databricks. If you wish, you can consult the following [link](https://docs.databricks.com/data/data-sources/index.html) for other sources of information of interest.

## Pre requirements

To achieve this, it is important to follow the following prerequisites:
1. Download library
It is required to download the library from the official Oracle source, in the following [link](https://www.oracle.com/database/technologies/appdev/jdbc-ucp-21-3-downloads.html) under the authorization of the required equipment such as security and architecture. The jar version must be the one compatible with the JDK version installed on Databricks. You should go to the section "**Oracle Database 21c (21.3) JDBC Driver & UCP Downloads**" and it is suggested to choose: ojdbc8.jar, which is certified with JDK8 which is the current version of JAVA on Databricks.

You can use the following statement to validate the Java version in Databricks
`%sh javac -version`

<img src="https://user-images.githubusercontent.com/92458075/188024618-ebbeaf16-1727-452d-ae19-57b762e10a9b.png" alt="drawing" width="500"/>
![image](https://user-images.githubusercontent.com/92458075/188024618-ebbeaf16-1727-452d-ae19-57b762e10a9b.png)

 
2. Install library

Once the library is downloaded, it is required to install it on the cluster that will be used to connect to the Oracle source. To install it, you must go to the ** Cluster / [Cluster name] / Libraries / Install New** section. In **Install** **Library**, choose the option Upload in Library Source and Jar in Library Type, click on **Drop JAR here** and select the Oracle jar. Finally, click **Install**.

<img src="https://user-images.githubusercontent.com/92458075/188024652-972ae4e1-549a-4dc1-86ef-2300c1013fd3.png" alt="drawing" width="500"/>
![image](https://user-images.githubusercontent.com/92458075/188024652-972ae4e1-549a-4dc1-86ef-2300c1013fd3.png)

3. Reference library

It is recommended to declare the global variables that refer to the driver and the database. Regarding the driver, it is recommended to review the [documentation](https://docs.oracle.com/cd/E11882_01/appdev.112/e13995/oracle/jdbc/OracleDriver.html).


```
driver = "oracle.jdbc.driver.OracleDriver"
url = "jdbc:oracle:thin:@<hostname>:1521/<db>"
```

 
## Configuration

The base configuration for Oracle corresponds to the following statement, which uses the previously declared driver and url variables. [Link to documentation](https://docs.databricks.com/data/data-sources/oracle.html).

```
df = spark.read.format ( "jdbc" ) \
.option ( "driver" , driver) \
.option ( "user", "[usuario]") \
.option ( "password", "[contrasena]") \
.option ( "url" , url) \
.option("dbtable", "[schema].[nombre_tabla]") \
load()
```


If you receive an error similar to ORA-01882: timezone region not found at oracle.jdbc.driver.T4CTTIoer11.processError(T4CTTIoer11.java:630), add the Oracle parameter associated with timezone, which corresponds to `oracle.jdbc.timezoneAsRegion=false`


```
df = spark.read.format ( "jdbc" ) \
.option ( "driver" , driver) \
.option ( "user", "[usuario]") \
.option ( "password", "[contrasena]") \
.option ( "url" , url) \
.option("dbtable", "[schema].[nombre_tabla]") \
.option("oracle.jdbc.timezoneAsRegion", "false")
.load()
```


To see the query made, use the following statement.
[display (df)]()

## References

-	[Documentación sobre Azure Databricks](https://docs.microsoft.com/es-es/azure/databricks/scenarios/what-is-azure-databricks)
-	[Azure Databricks Best Practices webinar](https://info.microsoft.com/ww-ondemand-azure-databricks-best-practices.html)
