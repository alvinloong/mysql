# [MySQL Server Administration](https://dev.mysql.com/doc/refman/5.7/en/server-administration.html)

## [The MySQL Server](https://dev.mysql.com/doc/refman/5.7/en/mysqld-server.html)

### [Server System Variables](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html)

```
mysqld --verbose --help
mysqld --no-defaults --verbose --help
[mysqld]
bind-address = 127.0.0.1
#skip-grant-tables
plugin-load-add=auth_socket.so
```

# [Security](https://dev.mysql.com/doc/refman/5.7/en/security.html)

## [Access Control and Account Management](https://dev.mysql.com/doc/refman/5.7/en/access-control.html)

### [Privileges Provided by MySQL](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html)

#### [`INSERT`](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_insert)

[`INSERT`](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_insert) is also required for the [`ANALYZE TABLE`](https://dev.mysql.com/doc/refman/5.7/en/analyze-table.html), [`OPTIMIZE TABLE`](https://dev.mysql.com/doc/refman/5.7/en/optimize-table.html), and [`REPAIR TABLE`](https://dev.mysql.com/doc/refman/5.7/en/repair-table.html) table-maintenance statements.

#### [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_select)

The [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_select) privilege is needed for tables or views used with [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html), including any underlying tables in view definitions.

## [Security Plugins](https://dev.mysql.com/doc/refman/5.7/en/security-plugins.html)

### [Authentication Plugins](https://dev.mysql.com/doc/refman/5.7/en/authentication-plugins.html)

#### [Socket Peer-Credential Pluggable Authentication](https://dev.mysql.com/doc/refman/5.7/en/socket-pluggable-authentication.html)

```
INSTALL PLUGIN auth_socket SONAME 'auth_socket.so';
CREATE USER 'valerie'@'localhost' IDENTIFIED WITH auth_socket;
CREATE USER 'valerie'@'localhost' IDENTIFIED WITH auth_socket AS 'stephanie';
ALTER USER 'valerie'@'localhost' IDENTIFIED WITH auth_socket AS 'stephanie';
```

# [SQL Statements](https://dev.mysql.com/doc/refman/5.7/en/sql-statements.html)

## [Data Manipulation Statements](https://dev.mysql.com/doc/refman/5.7/en/sql-data-manipulation-statements.html)

### [SELECT Statement](https://dev.mysql.com/doc/refman/5.7/en/select.html)

```
SELECT * FROM test PARTITION (p0,p1);
```

### [CREATE PROCEDURE and CREATE FUNCTION Statements](https://dev.mysql.com/doc/refman/5.7/en/create-procedure.html)

```
delimiter //
CREATE PROCEDURE citycount (IN country CHAR(3), OUT cities INT)
BEGIN
 SELECT COUNT(*) INTO cities FROM world.city
 WHERE CountryCode = country;
END//
delimiter ;
CALL citycount('JPN', @cities);
SELECT @cities;
CALL citycount('FRA', @cities);
SELECT @cities;
CREATE FUNCTION hello (s CHAR(20))
RETURNS CHAR(50) DETERMINISTIC
RETURN CONCAT('Hello, ',s,'!');
SELECT hello('world');

USE `testdb`;
DROP procedure IF EXISTS `new_procedure`;

DELIMITER $$
USE `testdb`$$
CREATE PROCEDURE `new_procedure` ()
BEGIN
DECLARE _var varchar(20) default 'Succeed';
DECLARE _cnt int default 0;
SELECT _var, _cnt;
END$$

DELIMITER ;

CALL new_procedure();

USE `testdb`;
DROP procedure IF EXISTS `new_procedure2`;

DELIMITER $$
USE `testdb`$$
CREATE PROCEDURE `new_procedure2` ()
BEGIN
SELECT * FROM world.city;
END$$

DELIMITER ;

CALL new_procedure2();

DELIMITER $$
USE `testdb`$$
CREATE PROCEDURE `new_procedure3` ()
BEGIN
SELECT * FROM world.city;
 select 1 as col1,2 as col2;
 select 11 as col11,22 as col21;
END$$

DELIMITER ;

CALL new_procedure3();
```

## [Database Administration Statements](https://dev.mysql.com/doc/refman/5.7/en/sql-server-administration-statements.html)

### [Table Maintenance Statements](https://dev.mysql.com/doc/refman/5.7/en/table-maintenance-statements.html)

#### [ANALYZE TABLE Statement](https://dev.mysql.com/doc/refman/5.7/en/analyze-table.html)

```
ANALYZE TABLE test;
ANALYZE NO_WRITE_TO_BINLOG TABLE test;
ANALYZE LOCAL TABLE test;
```

This statement requires [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_select) and [`INSERT`](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_insert) privileges for the table.

#### [CHECK TABLE Statement](https://dev.mysql.com/doc/refman/5.7/en/check-table.html)

```
CHECK TABLE test;
CHECK TABLE test FAST QUICK;
```

[`CHECK TABLE`](https://dev.mysql.com/doc/refman/5.7/en/check-table.html) checks a table or tables for errors. 

#### [OPTIMIZE TABLE Statement](https://dev.mysql.com/doc/refman/5.7/en/optimize-table.html)

```
OPTIMIZE TABLE test;
OPTIMIZE NO_WRITE_TO_BINLOG TABLE test;
OPTIMIZE LOCAL TABLE test;
```

This statement requires [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_select) and [`INSERT`](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_insert) privileges for the table.

#### [REPAIR TABLE Statement](https://dev.mysql.com/doc/refman/5.7/en/repair-table.html)

```
REPAIR TABLE test;
REPAIR NO_WRITE_TO_BINLOG TABLE test;
REPAIR LOCAL TABLE test;
```

This statement requires [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_select) and [`INSERT`](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_insert) privileges for the table.

### [Plugin and User-Defined Function Statements](https://dev.mysql.com/doc/refman/5.7/en/component-statements.html)

#### [INSTALL PLUGIN Statement](https://dev.mysql.com/doc/refman/5.7/en/install-plugin.html)

```
INSTALL PLUGIN auth_socket SONAME 'auth_socket.so';
CREATE USER 'valerie'@'localhost' IDENTIFIED WITH auth_socket;
UNINSTALL PLUGIN auth_socket;
```

### [SET Statements](https://dev.mysql.com/doc/refman/5.7/en/set-statement.html)

#### [SET Syntax for Variable Assignment](https://dev.mysql.com/doc/refman/5.7/en/set-variable.html)

```
SET GLOBAL max_connections = 1000;
SET SESSION sql_mode = 'TRADITIONAL';
SET LOCAL sql_mode = 'TRADITIONAL';
```

### [SHOW Statements](https://dev.mysql.com/doc/refman/5.7/en/show.html)

#### [SHOW COLUMNS Statement](https://dev.mysql.com/doc/refman/5.7/en/show-columns.html)

```
SHOW COLUMNS FROM test;
SHOW COLUMNS FROM mytable FROM mydb;
SHOW COLUMNS FROM mydb.mytable;
```

#### [SHOW CREATE TABLE Statement](https://dev.mysql.com/doc/refman/5.7/en/show-create-table.html)

```
SHOW CREATE TABLE test\G
```

#### [SHOW ENGINE Statement](https://dev.mysql.com/doc/refman/5.7/en/show-engine.html)

```
SHOW ENGINE INNODB STATUS\G
```

#### [SHOW ENGINES Statement](https://dev.mysql.com/doc/refman/5.7/en/show-engines.html)

```
SHOW ENGINES\G
```

#### [SHOW INDEX Statement](https://dev.mysql.com/doc/refman/5.7/en/show-index.html)

```
SHOW INDEX FROM test\G
SHOW INDEX FROM mytable FROM mydb;
SHOW INDEX FROM mydb.mytable;
```

#### [SHOW VARIABLES Statement](https://dev.mysql.com/doc/refman/5.7/en/show-variables.html)

```
SHOW VARIABLES;
SHOW VARIABLES LIKE 'sql_quote_show_create';
SHOW SESSION VARIABLES LIKE 'sql_quote_show_create';
SHOW GLOBAL VARIABLES LIKE '%size%';
```

#### [SHOW PLUGINS Statement](https://dev.mysql.com/doc/refman/5.7/en/show-plugins.html)

```
SHOW PLUGINS
SHOW PLUGINS\g
SHOW PLUGINS\G
```

## [Utility Statements](https://dev.mysql.com/doc/refman/5.7/en/sql-utility-statements.html)

### [DESCRIBE Statement](https://dev.mysql.com/doc/refman/5.7/en/describe.html)

The [`DESCRIBE`](https://dev.mysql.com/doc/refman/5.7/en/describe.html) and [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html) statements are synonyms, used either to obtain information about table structure or query execution plans.

```
DESC test;
DESCRIBE test;
```

### [EXPLAIN Statement](https://dev.mysql.com/doc/refman/5.7/en/explain.html)

```
EXPLAIN select * from test;
```

The [`DESCRIBE`](https://dev.mysql.com/doc/refman/5.7/en/describe.html) and [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html) statements are synonyms. In practice, the [`DESCRIBE`](https://dev.mysql.com/doc/refman/5.7/en/describe.html) keyword is more often used to obtain information about table structure, whereas [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html) is used to obtain a query execution plan.

[`DESCRIBE`](https://dev.mysql.com/doc/refman/5.7/en/describe.html) is a shortcut for [`SHOW COLUMNS`](https://dev.mysql.com/doc/refman/5.7/en/show-columns.html). The description for [`SHOW COLUMNS`](https://dev.mysql.com/doc/refman/5.7/en/show-columns.html) provides more information about the output columns.

The [`DESCRIBE`](https://dev.mysql.com/doc/refman/5.7/en/describe.html) statement is provided for compatibility with Oracle.

[`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html) requires the [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_select) privilege for any tables or views accessed, including any underlying tables of views. For views, [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html) also requires the [`SHOW VIEW`](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_show-view) privilege.

# [Alternative Storage Engines](https://dev.mysql.com/doc/refman/5.7/en/storage-engines.html)

## [The MyISAM Storage Engine](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)

```
CREATE TABLE t (i INT) ENGINE = MYISAM;
```

### [MyISAM Table Problems](https://dev.mysql.com/doc/refman/5.7/en/myisam-table-problems.html)

#### [Corrupted MyISAM Tables](https://dev.mysql.com/doc/refman/5.7/en/corrupted-myisam-tables.html)

You can check the health of a `MyISAM` table using the [`CHECK TABLE`](https://dev.mysql.com/doc/refman/5.7/en/check-table.html) statement, and repair a corrupted `MyISAM` table with [`REPAIR TABLE`](https://dev.mysql.com/doc/refman/5.7/en/repair-table.html).

Typical symptoms of a corrupt table are:

- You get the following error while selecting data from the table:

  ```none
  Incorrect key file for table: '...'. Try to repair it
  ```

 The most important thing to know is whether the table became corrupted as a result of a server crash. You can verify this easily by looking for a recent `restarted mysqld` message in the error log. If there is such a message, it is likely that table corruption is a result of the server dying. 

# [Replication](https://dev.mysql.com/doc/refman/5.7/en/replication.html)

## [Configuring Replication](https://dev.mysql.com/doc/refman/5.7/en/replication-configuration.html)

### [Common Replication Administration Tasks](https://dev.mysql.com/doc/refman/5.7/en/replication-administration.html)

#### [Checking Replication Status](https://dev.mysql.com/doc/refman/5.7/en/replication-administration-status.html)

Slave

```
SHOW SLAVE STATUS\G
SELECT * FROM replication_connection_configuration\G
```

Master

```
SHOW PROCESSLIST \g
SHOW SLAVE HOSTS;
```

#### [Pausing Replication on the Slave](https://dev.mysql.com/doc/refman/5.7/en/replication-administration-pausing.html)

Slave

```
STOP SLAVE;
STOP SLAVE IO_THREAD;
STOP SLAVE SQL_THREAD;
START SLAVE;
START SLAVE IO_THREAD;
START SLAVE SQL_THREAD;
```

#### [Skipping Transactions](https://dev.mysql.com/doc/refman/5.7/en/replication-administration-skip.html)

Master

```
SHOW BINLOG EVENTS;
```

Slave

```
SELECT * FROM performance_schema.replication_applier_status_by_worker\G
SHOW RELAYLOG EVENTS;
```

##### [Skipping Transactions With GTIDs](https://dev.mysql.com/doc/refman/5.7/en/replication-administration-skip.html#replication-administration-skip-gtid) 

```
SET GTID_NEXT='aaa-bbb-ccc-ddd:N';
BEGIN;
COMMIT;
SET GTID_NEXT='AUTOMATIC';

FLUSH LOGS;
PURGE BINARY LOGS TO 'binlog.000146';
```

##### [Skipping Transactions Without GTIDs](https://dev.mysql.com/doc/refman/5.7/en/replication-administration-skip.html#replication-administration-skip-nogtid)

```
SET GLOBAL sql_slave_skip_counter = N;
CHANGE MASTER TO MASTER_LOG_FILE='master_log_name', MASTER_LOG_POS=master_log_pos;
CHANGE MASTER TO MASTER_AUTO_POSITION=0, MASTER_LOG_FILE='binlog.000145', MASTER_LOG_POS=235;
CHANGE MASTER TO MASTER_AUTO_POSITION=1;
```



# [Partitioning](https://dev.mysql.com/doc/refman/5.7/en/partitioning.html)

## [Partitioning Types](https://dev.mysql.com/doc/refman/5.7/en/partitioning-types.html)

### [RANGE Partitioning](https://dev.mysql.com/doc/refman/5.7/en/partitioning-range.html)

```
CREATE TABLE employees (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT NOT NULL,
    store_id INT NOT NULL
)
PARTITION BY RANGE (store_id) (
    PARTITION p0 VALUES LESS THAN (6),
    PARTITION p1 VALUES LESS THAN (11),
    PARTITION p2 VALUES LESS THAN (16),
    PARTITION p3 VALUES LESS THAN (21)
);
CREATE TABLE employees (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT NOT NULL,
    store_id INT NOT NULL
)
PARTITION BY RANGE (store_id) (
    PARTITION p0 VALUES LESS THAN (6),
    PARTITION p1 VALUES LESS THAN (11),
    PARTITION p2 VALUES LESS THAN (16),
    PARTITION p3 VALUES LESS THAN MAXVALUE
);
CREATE TABLE employees (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT,
    store_id INT
)
PARTITION BY RANGE ( YEAR(separated) ) (
    PARTITION p0 VALUES LESS THAN (1991),
    PARTITION p1 VALUES LESS THAN (1996),
    PARTITION p2 VALUES LESS THAN (2001),
    PARTITION p3 VALUES LESS THAN MAXVALUE
);
CREATE TABLE members (
    firstname VARCHAR(25) NOT NULL,
    lastname VARCHAR(25) NOT NULL,
    username VARCHAR(16) NOT NULL,
    email VARCHAR(35),
    joined DATE NOT NULL
)
PARTITION BY RANGE COLUMNS(joined) (
    PARTITION p0 VALUES LESS THAN ('1960-01-01'),
    PARTITION p1 VALUES LESS THAN ('1970-01-01'),
    PARTITION p2 VALUES LESS THAN ('1980-01-01'),
    PARTITION p3 VALUES LESS THAN ('1990-01-01'),
    PARTITION p4 VALUES LESS THAN MAXVALUE
);
```

In MySQL 5.7, it is also possible to partition a table by `RANGE` based on the value of a [`TIMESTAMP`](https://dev.mysql.com/doc/refman/5.7/en/datetime.html) column, using the [`UNIX_TIMESTAMP()`](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_unix-timestamp) function, as shown in this example:

```sql
CREATE TABLE quarterly_report_status (
    report_id INT NOT NULL,
    report_status VARCHAR(20) NOT NULL,
    report_updated TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
)
PARTITION BY RANGE ( UNIX_TIMESTAMP(report_updated) ) (
    PARTITION p0 VALUES LESS THAN ( UNIX_TIMESTAMP('2008-01-01 00:00:00') ),
    PARTITION p1 VALUES LESS THAN ( UNIX_TIMESTAMP('2008-04-01 00:00:00') ),
    PARTITION p2 VALUES LESS THAN ( UNIX_TIMESTAMP('2008-07-01 00:00:00') ),
    PARTITION p3 VALUES LESS THAN ( UNIX_TIMESTAMP('2008-10-01 00:00:00') ),
    PARTITION p4 VALUES LESS THAN ( UNIX_TIMESTAMP('2009-01-01 00:00:00') ),
    PARTITION p5 VALUES LESS THAN ( UNIX_TIMESTAMP('2009-04-01 00:00:00') ),
    PARTITION p6 VALUES LESS THAN ( UNIX_TIMESTAMP('2009-07-01 00:00:00') ),
    PARTITION p7 VALUES LESS THAN ( UNIX_TIMESTAMP('2009-10-01 00:00:00') ),
    PARTITION p8 VALUES LESS THAN ( UNIX_TIMESTAMP('2010-01-01 00:00:00') ),
    PARTITION p9 VALUES LESS THAN (MAXVALUE)
);
```

### [LIST Partitioning](https://dev.mysql.com/doc/refman/5.7/en/partitioning-list.html)

```
CREATE TABLE employees (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT,
    store_id INT
)
PARTITION BY LIST(store_id) (
    PARTITION pNorth VALUES IN (3,5,6,9,17),
    PARTITION pEast VALUES IN (1,2,10,11,19,20),
    PARTITION pWest VALUES IN (4,12,13,14,18),
    PARTITION pCentral VALUES IN (7,8,15,16)
);
```

### [COLUMNS Partitioning](https://dev.mysql.com/doc/refman/5.7/en/partitioning-columns.html)

#### [RANGE COLUMNS partitioning](https://dev.mysql.com/doc/refman/5.7/en/partitioning-columns-range.html)

```
CREATE TABLE r1 (
    a INT,
    b INT
)
PARTITION BY RANGE (a)  (
    PARTITION p0 VALUES LESS THAN (5),
    PARTITION p1 VALUES LESS THAN (MAXVALUE)
);
CREATE TABLE rc1 (
    a INT,
    b INT
)
PARTITION BY RANGE COLUMNS(a, b) (
    PARTITION p0 VALUES LESS THAN (5, 12),
    PARTITION p3 VALUES LESS THAN (MAXVALUE, MAXVALUE)
);
CREATE TABLE rc4 (
    a INT,
    b INT,
    c INT
)
PARTITION BY RANGE COLUMNS(a,b,c) (
    PARTITION p0 VALUES LESS THAN (0,25,50),
    PARTITION p1 VALUES LESS THAN (10,20,100),
    PARTITION p2 VALUES LESS THAN (10,30,50)
    PARTITION p3 VALUES LESS THAN (MAXVALUE,MAXVALUE,MAXVALUE)
 );
CREATE TABLE employees_by_lname (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT NOT NULL,
    store_id INT NOT NULL
)
PARTITION BY RANGE COLUMNS (lname)  (
    PARTITION p0 VALUES LESS THAN ('g'),
    PARTITION p1 VALUES LESS THAN ('m'),
    PARTITION p2 VALUES LESS THAN ('t'),
    PARTITION p3 VALUES LESS THAN (MAXVALUE)
);
ALTER TABLE employees PARTITION BY RANGE COLUMNS (lname)  (
    PARTITION p0 VALUES LESS THAN ('g'),
    PARTITION p1 VALUES LESS THAN ('m'),
    PARTITION p2 VALUES LESS THAN ('t'),
    PARTITION p3 VALUES LESS THAN (MAXVALUE)
);
ALTER TABLE employees PARTITION BY RANGE COLUMNS (hired)  (
    PARTITION p0 VALUES LESS THAN ('1970-01-01'),
    PARTITION p1 VALUES LESS THAN ('1980-01-01'),
    PARTITION p2 VALUES LESS THAN ('1990-01-01'),
    PARTITION p3 VALUES LESS THAN ('2000-01-01'),
    PARTITION p4 VALUES LESS THAN ('2010-01-01'),
    PARTITION p5 VALUES LESS THAN (MAXVALUE)
);
```

#### [LIST COLUMNS partitioning](https://dev.mysql.com/doc/refman/5.7/en/partitioning-columns-list.html)

```sql
CREATE TABLE customers_1 (
    first_name VARCHAR(25),
    last_name VARCHAR(25),
    street_1 VARCHAR(30),
    street_2 VARCHAR(30),
    city VARCHAR(15),
    renewal DATE
)
PARTITION BY LIST COLUMNS(city) (
    PARTITION pRegion_1 VALUES IN('Oskarshamn', 'Högsby', 'Mönsterås'),
    PARTITION pRegion_2 VALUES IN('Vimmerby', 'Hultsfred', 'Västervik'),
    PARTITION pRegion_3 VALUES IN('Nässjö', 'Eksjö', 'Vetlanda'),
    PARTITION pRegion_4 VALUES IN('Uppvidinge', 'Alvesta', 'Växjo')
);
CREATE TABLE customers_2 (
    first_name VARCHAR(25),
    last_name VARCHAR(25),
    street_1 VARCHAR(30),
    street_2 VARCHAR(30),
    city VARCHAR(15),
    renewal DATE
)
PARTITION BY LIST COLUMNS(renewal) (
    PARTITION pWeek_1 VALUES IN('2010-02-01', '2010-02-02', '2010-02-03',
        '2010-02-04', '2010-02-05', '2010-02-06', '2010-02-07'),
    PARTITION pWeek_2 VALUES IN('2010-02-08', '2010-02-09', '2010-02-10',
        '2010-02-11', '2010-02-12', '2010-02-13', '2010-02-14'),
    PARTITION pWeek_3 VALUES IN('2010-02-15', '2010-02-16', '2010-02-17',
        '2010-02-18', '2010-02-19', '2010-02-20', '2010-02-21'),
    PARTITION pWeek_4 VALUES IN('2010-02-22', '2010-02-23', '2010-02-24',
        '2010-02-25', '2010-02-26', '2010-02-27', '2010-02-28')
);
```

### [HASH Partitioning](https://dev.mysql.com/doc/refman/5.7/en/partitioning-hash.html)

```sql
CREATE TABLE employees (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT,
    store_id INT
)
PARTITION BY HASH(store_id)
PARTITIONS 4;
CREATE TABLE employees (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT,
    store_id INT
)
PARTITION BY HASH( YEAR(hired) )
PARTITIONS 4;
```

#### [LINEAR HASH Partitioning](https://dev.mysql.com/doc/refman/5.7/en/partitioning-linear-hash.html)

```sql
CREATE TABLE employees (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT,
    store_id INT
)
PARTITION BY LINEAR HASH( YEAR(hired) )
PARTITIONS 4;
```

### [KEY Partitioning](https://dev.mysql.com/doc/refman/5.7/en/partitioning-key.html)

```sql
CREATE TABLE k1 (
    id INT NOT NULL PRIMARY KEY,
    name VARCHAR(20)
)
PARTITION BY KEY()
PARTITIONS 2;
```

### [Subpartitioning](https://dev.mysql.com/doc/refman/5.7/en/partitioning-subpartitions.html)

```sql
CREATE TABLE ts (id INT, purchased DATE)
    PARTITION BY RANGE( YEAR(purchased) )
    SUBPARTITION BY HASH( TO_DAYS(purchased) )
    SUBPARTITIONS 2 (
        PARTITION p0 VALUES LESS THAN (1990),
        PARTITION p1 VALUES LESS THAN (2000),
        PARTITION p2 VALUES LESS THAN MAXVALUE
    );
CREATE TABLE ts (id INT, purchased DATE)
    PARTITION BY RANGE( YEAR(purchased) )
    SUBPARTITION BY HASH( TO_DAYS(purchased) ) (
        PARTITION p0 VALUES LESS THAN (1990) (
            SUBPARTITION s0,
            SUBPARTITION s1
        ),
        PARTITION p1 VALUES LESS THAN (2000) (
            SUBPARTITION s2,
            SUBPARTITION s3
        ),
        PARTITION p2 VALUES LESS THAN MAXVALUE (
            SUBPARTITION s4,
            SUBPARTITION s5
        )
    );
```

## [Partition Management](https://dev.mysql.com/doc/refman/5.7/en/partitioning-management.html)

### [Maintenance of Partitions](https://dev.mysql.com/doc/refman/5.7/en/partitioning-maintenance.html)

```
ALTER TABLE t1 REBUILD PARTITION p0, p1;
ALTER TABLE t1 OPTIMIZE PARTITION p0, p1;
ALTER TABLE t1 ANALYZE PARTITION p3;
ALTER TABLE t1 REPAIR PARTITION p0,p1;
ALTER TABLE trb3 CHECK PARTITION p1
```

# [INFORMATION_SCHEMA Tables](https://dev.mysql.com/doc/refman/5.7/en/information-schema.html)

## [The INFORMATION_SCHEMA STATISTICS Table](https://dev.mysql.com/doc/refman/5.7/en/statistics-table.html)

```
SELECT * FROM INFORMATION_SCHEMA.STATISTICS
  WHERE table_name = 'tbl_name'
  AND table_schema = 'db_name';
SHOW INDEX
  FROM tbl_name
  FROM db_name;
```

## [The INFORMATION_SCHEMA PLUGINS Table](https://dev.mysql.com/doc/refman/5.7/en/plugins-table.html)

```
SELECT
  PLUGIN_NAME, PLUGIN_STATUS, PLUGIN_TYPE,
  PLUGIN_LIBRARY, PLUGIN_LICENSE
FROM INFORMATION_SCHEMA.PLUGINS;
SHOW PLUGINS;
```

# [MySQL Glossary](https://dev.mysql.com/doc/refman/5.7/en/glossary.html)

## [.ibd file](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_ibd_file)