# Preparation for docker
```shell
sudo chmod -R 777 config
sudo chmod -R 777 informix-server
```


# JDBC syntax

```properties
jdbc:informix-sqli://localhost:9088/sysadmin:INFORMIXSERVER=informix;NEWCODESET=utf8,8859-1,819;CLIENT_LOCALE=en_US.utf8;DB_LOCALE=en_US.8859-1;

jdbc:informix-sqli://localhost:9080/testdb:informixServer=mangodsn;NEWCODESET=MS950,big5,57352;DB_LOCALE=zh_tw.big5;CLIENT_LOCALE=zh_tw.big5

jdbc:informix-sqli://localhost:9088/sysadmin:INFORMIXSERVER=informix;NEWCODESET=utf8,8859-1,819;CLIENT_LOCALE=zh_tw.utf8;DB_LOCALE=zh_tw.utf8;


```

# Batch import

```shell
DB_LOCALE=zh_tw.utf8
export DB_LOCALE
dbaccess batchmnt *.sql

```

# Locale information
```yaml
      LC_COLLATE: 'zh_tw.utf8'
      LC_CTYPE:  'zh_tw.utf8'
      LC_MONETARY:  'zh_tw.utf8'
      LC_NUMERIC:  'zh_tw.utf8'
      LC_TIME:  'zh_tw.utf8'
      SERVER_LOCALE: 'zh_tw.utf8'
```
# Wildfly Data Source Configuration Sample
## Non-XA sample
```xml
<datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" use-java-context="true" statistics-enabled="true">
    <connection-url>jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE</connection-url>
    <driver>h2</driver>
    <security>
        <user-name>sa</user-name>
        <password>sa</password>
    </security>
</datasource>
<datasource jndi-name="java:jboss/datasources/InformixDS" pool-name="InformixDS" enabled="true" use-java-context="true" statistics-enabled="true">
    <connection-url>jdbc:informix-sqli://192.168.18.30:9088/batchmnt:INFORMIXSERVER=informix;NEWCODESET=big5,8859-1,819;</connection-url>
    <driver>informix</driver>
    <security>
        <user-name>informix</user-name>
        <password>in4mix</password>
    </security>
</datasource>
<drivers>
    <driver name="h2" module="com.h2database.h2">
        <xa-datasource-class>org.h2.jdbcx.JdbcDataSource</xa-datasource-class>
    </driver>
    <driver name="informix" module="com.informix">
        <xa-datasource-class>com.informix.jdbcx.IfxXADataSource</xa-datasource-class>
    </driver>
</drivers> 
```
## Non-XA sample
```
<xa-datasource jndi-name="java:jboss/datasources/MDINFOP_XA/psjdbc" pool-name="MDINFOP_XA_psjdbc">
    <driver>informix</driver>
    <xa-datasource-class>com.informix.jdbcx.IfxXADataSource</xa-datasource-class>
    <xa-datasource-property name="IfxIFXHOST">192.168.18.30</xa-datasource-property>
    <xa-datasource-property name="PortNumber">9088</xa-datasource-property>
    <xa-datasource-property name="DatabaseName">sysadmin</xa-datasource-property>
    <xa-datasource-property name="ServerName">informix</xa-datasource-property>
    <xa-datasource-property name="IfxIFX_ISOLATION_LEVEL">2U</xa-datasource-property>
    <xa-datasource-property name="IfxIFX_LOCK_MODE_WAIT">26</xa-datasource-property>
    <xa-datasource-property name="IfxLOBCACHE">-1</xa-datasource-property>
    <xa-datasource-property name="IfxNEWCODESET">8859_15,8859-1,819</xa-datasource-property>
    <xa-datasource-property name="IfxDB_LOCALE">en_US.819</xa-datasource-property>
    <xa-pool>
        <is-same-rm-override>false</is-same-rm-override>
        <no-tx-separate-pools>true</no-tx-separate-pools>
    </xa-pool>
    <security>
        <user-name>informix</user-name>
        <password>in4mix</password>
    </security>
    <validation>
        <background-validation>true</background-validation>
        <background-validation-millis>60000</background-validation-millis>
        <check-valid-connection-sql>select 1 from sysmaster:sysdual</check-valid-connection-sql>
        <exception-sorter class-name="org.jboss.jca.adapters.jdbc.extensions.informix.InformixExceptionSorter"/>
    </validation>
    <statement>
        <prepared-statement-cache-size>128</prepared-statement-cache-size>
    </statement>
</xa-datasource>
<drivers>
    <driver name="informix" module="com.informix">
        <xa-datasource-class>com.informix.jdbcx.IfxXADataSource</xa-datasource-class>
    </driver>
</drivers> 
```
when XA-Datasource's named **DatabaseName**, values ( **sysadmin** , **sysmaster** ,**sysuser** ) , you can see the congiuration worked well. But other databases were not .
**⚠ TIP:**   You need to cinfgiure the informix database use **logging mode**
```shell
    [informix@c7de582cc2f4 ~]$ ontape -s -U commondb
    Archive to tape device '/dev/null' is complete.

    Program over.    
    [informix@c7de582cc2f4 ~]$
``` 
