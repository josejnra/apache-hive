# Apache Hive

Hive is a data warehouse infrastructure built on Hadoop. It provides a set of tools for data extraction transformation loading (ETL), a mechanism for storing, querying, and analyzing large-scale data stored in Hadoop. Hive defines a simple class SQL query language called HQL that allows users familiar with SQL to query data. At the same time, the language also allows you to familiarize yourself with MapReduce developers' custom mappers and reducers to handle the complex analysis work that the built-in mapper and reducer can't.

Hive does not have a dedicated data format. Hive works fine on Thrift, controls delimiters, and allows users to specify data formats.

Hive is a combination of three components:
- Data files in varying formats that are typically stored in the Hadoop Distributed File System (HDFS) or in Amazon S3.
- Metadata about how the data files are mapped to schemas and tables. This metadata is stored in a database such as MySQL and is accessed via the Hive metastore service.
- A query language called HiveQL. This query language is executed on a distributed computing framework such as MapReduce or Tez.

Presto only uses the first two components: the data and the metadata. It does not use HiveQL or any part of Hive’s execution environment.

### Hive Architecture

<p align="center">
    <img src="images/hive_architecture.png" alt="Hive Architecture" />
</p>

- **UI**: The user interface for users to submit queries and other operations to the system.
- **Driver**: The component which receives the queries. This component implements the notion of session handles and provides execute and fetch APIs modeled on JDBC/ODBC interfaces.
- **Compiler**: The component that parses the query, does semantic analysis on the different query blocks and query expressions and eventually generates an execution plan with the help of the table and partition metadata looked up from the metastore.
- **Metastore**: The component that stores all the structure information of the various tables and partitions in the warehouse including column and column type information, the serializers and deserializers necessary to read and write data and the corresponding HDFS files where the data is stored.
- **Execution Engine**: The component which executes the execution plan created by the compiler. The plan is a DAG of stages. The execution engine manages the dependencies between these different stages of the plan and executes these stages on the appropriate system components.


### Metastore
The Metastore provides two important but often overlooked features of a data warehouse: data abstraction and data discovery. Without the data abstractions provided in Hive, a user has to provide information about data formats, extractors and loaders along with the query. In Hive, this information is given during table creation and reused every time the table is referenced. This is very similar to the traditional warehousing systems. The second functionality, data discovery, enables users to discover and explore relevant and specific data in the warehouse. Other tools can be built using this metadata to expose and possibly enhance the information about the data and its availability. Hive accomplishes both of these features by providing a metadata repository that is tightly integrated with the Hive query processing system so that data and metadata are in sync.

Metastore is an object store with a database or file backed store. The database backed store is implemented using an object-relational mapping (ORM) solution called the DataNucleus. The prime motivation for storing this in a relational database is queriability of metadata. Some disadvantages of using a separate data store for metadata instead of using HDFS are synchronization and scalability issues. Additionally there is no clear way to implement an object store on top of HDFS due to lack of random updates to files.

### Hive Data Model
Data in Hive is organized into:

- **Tables**: These are analogous to Tables in Relational Databases. Tables can be filtered, projected, joined and unioned. Additionally all the data of a table is stored in a directory in HDFS. Hive also supports the notion of external tables wherein a table can be created on prexisting files or directories in HDFS by providing the appropriate location to the table creation DDL. The rows in a table are organized into typed columns similar to Relational Databases.
- **Partitions**: Each Table can have one or more partition keys which determine how the data is stored, for example a table T with a date partition column ds had files with data for a particular date stored in the `<table location>/ds=<date>` directory in HDFS. Partitions allow the system to prune data to be inspected based on query predicates, for example a query that is interested in rows from T that satisfy the predicate T.ds = '2008-09-01' would only have to look at files in `<table location>/ds=2008-09-01/` directory in HDFS.
- **Buckets**: Data in each partition may in turn be divided into Buckets based on the hash of a column in the table. Each bucket is stored as a file in the partition directory. Bucketing allows the system to efficiently evaluate queries that depend on a sample of data (these are queries that use the SAMPLE clause on the table).