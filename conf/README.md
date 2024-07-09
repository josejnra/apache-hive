Connect to beeline
```shell
docker exec -it hiveserver2 beeline -u 'jdbc:hive2://hiveserver2:10000/'
# If beeline is installed on host machine, HiveServer2 can be simply reached via:
beeline -u 'jdbc:hive2://localhost:10000/'
```

```shell
# init schema
schematool -initSchema -dbType postgres -passWord hive -userName hive
```

```shell
hive
```

```hive
CREATE TABLE IF NOT EXISTS employee ( eid int, name String, salary String, destination String)
COMMENT 'Employee details'
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '\t'
LINES TERMINATED BY '\n'
STORED AS PARQUET;



<!-- STORED AS TEXTFILE; -->
```

```hive
INSERT INTO employee ( eid, name, salary , destination) VALUES (1, 'joe', '10000', 'Brazil');
```

## Offical Hive docker image

```shell
# connect to beeline
docker exec -it hiveserver2 beeline -u 'jdbc:hive2://hiveserver2:10000/'
```
## Referencies
- [Hive Installation](https://sparkbyexamples.com/apache-hive/apache-hive-installation-on-hadoop/)
- [Schematool](https://docs.cloudera.com/documentation/enterprise/6/6.3/topics/cdh_ig_hive_schema_tool.html)
- [Hive Official Docker Image](https://github.com/apache/hive/blob/master/packaging/src/docker/README.md)
