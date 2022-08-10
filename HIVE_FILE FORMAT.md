# Hive File Formats

## 1. TEXTFILE FORMAT

TEXTFILE format is a famous input/output format used in Hadoop. In Hive if we define a table as TEXTFILE it can load data of from CSV (Comma Separated Values), delimited by Tabs, Spaces and JSON data. This means fields in each record should be separated by comma or space or tab or it may be JSON(JavaScript Object Notation) data.

By default, if we use TEXTFILE format then each line is considered as a record.

```sql
create table table_name (schema of the table) 
row format delimited 
fields terminated by ','  
stored as TEXTFILE;
```

## 2. SEQUENCE FILE FORMAT

We know that Hadoop’s performance is drawn out when we work with a small number of files with big size rather than a large number of files with small size. If the size of a file is smaller than the typical block size in Hadoop, we consider it as a small file. Due to this, a number of metadata increases which will become an overhead to the NameNode. To solve this problem sequence files are introduced in Hadoop. Sequence files act as a container to store the small files.

Sequence files are flat files consisting of binary key-value pairs. When Hive converts queries to MapReduce jobs, it decides on the appropriate key-value pairs to be used for a given record. Sequence files are in the binary format which can be split and the main use of these files is to club two or more smaller files and make them as a one sequence file.

In Hive we can create a sequence file by specifying STORED AS SEQUENCEFILE in the end of a CREATE TABLE statement.

There are three types of sequence files:

1. Uncompressed key/value records.
1. Record compressed key/value records – only ‘values’ are compressed here
1. Block compressed key/value records – both keys and values are collected in ‘blocks’ separately and compressed. The size of the ‘block’ is configurable.

```sql
create table seqbasetab (id int , name string , sal int ,loc string)
row format delimited 
fields terminated by '\t'
lines terminated by '\n'
stored as SEQUENCEFILE;

insert overwrite table seqbasetab select * from basetab;
```

## 3. RC FILE FORMAT (RECORD COLUMNAR)

RCFILE stands of Record Columnar File which is another type of binary file format which offers high compression rate on the top of the rows. RCFILE is used when we want to perform operations on multiple rows at a time. RCFILEs are flat files consisting of binary key/value pairs, which shares many similarities with SEQUENCEFILE. RCFILE stores columns of a table in form of record in a columnar manner. It first partitions rows horizontally into row splits and then it vertically partitions each row split in a columnar way. RCFILE first stores the metadata of a row split,  as the key part of a record, and all the data of a row split as the value part. This means that RCFILE encourages column oriented storage rather than row oriented storage. This column oriented storage is very useful while performing analytics. It is easy to perform analytics when we “hive’ a column oriented storage type. Facebook uses RCFILE as its default file format for storing of data in their data warehouse as they perform different types of analytics using Hive.

```sql
create table rcbasetab (id int , name string , sal int ,loc string)
row format delimited 
fields terminated by '\t'
lines terminated by '\n'
stored as RCFILE;

insert overwrite table rcbasetab select * from basetab;
```

## 4. ORC FILE FORMAT (OPTIMISED RECORD COLUMNAR)

ORC stands for Optimized Row Columnar which means it can store data in an optimized way than the other file formats. ORC reduces the size of the original data up to 75%(eg: 100GB file will become 25GB). As a result the speed of data processing also increases. ORC shows better performance than Text, Sequence and RC file formats. An ORC file contains rows data in groups called as Stripes along with a file footer. ORC format improves the performance when Hive is processing the data.

```sql
create table orcbasetab (id int , name string , sal int ,loc string)
row format delimited 
fields terminated by '\t'
lines terminated by '\n'
stored as ORCFILE;

insert overwrite table orcbasetab select * from basetab;
```

Thus you can use the above four file formats depending on your data.
For example,

* If your data is delimited by some parameters then you can use TEXTFILE format.
* If your data is in small files whose size is less than the block size then you can use SEQUENCEFILE format.
* If you want to perform analytics on your data and you want to store your data efficiently for that then you can use RCFILE format.
* If you want to store your data in an optimized way which lessens your storage and increases your performance then you can use ORCFILE format.

## 5. PARQUET FILE FORMAT (OPTIMISED RECORD COLUMNAR)

```sql
create table parquetbasetab (id int , name string , sal int ,loc string)
row format delimited 
fields terminated by '\t'
lines terminated by '\n'
stored as PARQUETFILE;

insert overwrite table parquetbasetab select * from basetab;
```

BETTER FILE FORMAT : ORC, PARQUET
