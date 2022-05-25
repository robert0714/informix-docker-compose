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