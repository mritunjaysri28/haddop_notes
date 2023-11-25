# HIVE

### DDL

* Create Database
    ```sql
    create database database_name;
    ```

## Internal Table
 * Step-1> Create Table
    ```sql
     create table tablename 
     (col1 int,col2 string,col3 int) 
     row format delimited fields terminated by ','
     lines terminated by 
     comment 'Table Description' 
     tblproperties( "skip.header.line.count"="1"
     , "creator"="creater_name"
     , "created_at"="2019-06-06 11:00:00"
     );
     ```

* Step-2 > Load data 
    * From local file to Table

    ```
    load data local inpath 'filepath' into table tablename; 	
    ```
    * From HDFS to Table

    ```
    load data inpath 'filepath' into table tablename; 	
    ```

* Step-3 > Fetch Record Table

    ```sql
    Select * from tablename;
    ```

## External Table
* Step-1 > Create a hdfs directory
```
hdfs dfs -mkdir hdfsPath
```

* Step-2 > Load data in HDFS
```
hdfs dfs -put filePath hdfsPath
```

* Step-3 > Create External Table

    ```sql
       create table external tablename 
        (col1 int,col2 string,col3 int) 
        row format delimited fields terminated by ','
        lines terminated by '\n'
        location '/hdfsPath' 
        comment 'Table Description' 
        tblproperties( "skip.header.line.count"="1"
        , "creator"="creater_name"
        , "created_at"="2019-06-06 11:00:00"
        ); 

        --location '/hdfsPath' => Load data in external path from file mention in location
    ```

## Export Data from Hive

* Export to hdfs

    ```sql 
    INSERT OVERWRITE DIRECTORY 'hdfsPath' ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' SELECT * FROM database.tablename
    ```

* Export to local
    ```sql
    INSERT OVERWRITE LOCAL DIRECTORY 'hdfsPath' ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' SELECT * FROM database.tablename
    ```

## Partitioning

* Step-1 > For dynamic partition set below properties
    ```
    set hive.exec dynamic.partition=true;
    set hive.exec dynamic.partition.mode=nonstrict;
    ```
* Step-2 > Create table partition
    ```sql
    create table tablename(col1 int,col2 string) partitioned by (col3 string) row format delimited fields terminated by ',';
    ```
* Step-3 > Load Data in Table
    ```
    load data local inpath 'filePath' overwrite into table tableName partition(city="value");
    ```

    insert into transaction_part select transaction_id,cust_id,tran_date,Qty,Rate,Tax,total_amt where Qty>2

* Insert into partition table
    ```sql
    insert overwrite table partition_table partition(col1,col2) select col1,col2,col3 from transaction;
    ```
