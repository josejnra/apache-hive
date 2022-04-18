```shell
schematool -initSchema -dbType postgres -passWord hive -userName hive
```

```shell
hive
```

```hive
CREATE TABLE IF NOT EXISTS employee ( eid int, name String,
salary String, destination String)
COMMENT 'Employee details'
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '\t'
LINES TERMINATED BY '\n'
STORED AS TEXTFILE;
```

## Referencies
- [Hive Installation](https://sparkbyexamples.com/apache-hive/apache-hive-installation-on-hadoop/)
- [Schematool](https://docs.cloudera.com/documentation/enterprise/6/6.3/topics/cdh_ig_hive_schema_tool.html)
