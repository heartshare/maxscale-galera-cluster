## Requirements

- MaxScale: 192.168.10.222, port 3308

- schema: sbtest

- table: sbtest

- u/p: sbtest/password

## Create data for sbtest table

- Creating sbtest schema

```
mysql> CREATE SCHEMA sbtest;
mysql> CREATE USER sbtest@'%' IDENTIFIED BY 'password';
mysql> GRANT ALL PRIVILEGES ON sbtest.* to sbtest@'%';
```
- Creating 1000000 records into sbtest table

`sysbench --db-driver=mysql --mysql-host=192.168.10.222 --mysql-port=3308 --mysql-user=sbtest  --mysql-password=password  --mysql-db=sbtest --range_size=1000   --table_size=1000000 --tables=4 --threads=8 --events=0 --time=60   --rand-type=uniform /usr/share/sysbench/oltp_read_only.lua prepare`

sysbench 1.0.17 (using system LuaJIT 2.0.4)

Initializing worker threads...

Creating table 'sbtest3'...
Creating table 'sbtest2'...
Creating table 'sbtest1'...
Creating table 'sbtest4'...
Inserting 1000000 records into 'sbtest1'
Inserting 1000000 records into 'sbtest4'
Inserting 1000000 records into 'sbtest2'
Inserting 1000000 records into 'sbtest3'
Creating a secondary index on 'sbtest4'...
Creating a secondary index on 'sbtest1'...
Creating a secondary index on 'sbtest2'...
Creating a secondary index on 'sbtest3'...

Ở đây, chúng ta tạo 04 tables (sbtest1-4), với mỗi tables gồm 1000000 rows.

## Read data

`sysbench --db-driver=mysql --mysql-host=192.168.10.222 --mysql-port=3308 --mysql-user=sbtest  --mysql-password=password  --mysql-db=sbtest --range_size=1000   --table_size=1000000 --tables=4 --threads=8 --events=0 --time=60   --rand-type=uniform /usr/share/sysbench/oltp_read_only.lua run`

sysbench 1.0.17 (using system LuaJIT 2.0.4)

Running the test with following options:
Number of threads: 8
Initializing random number generator from current time


Initializing worker threads...

Threads started!

SQL statistics:
    queries performed:
        read:                            274694
        write:                           0
        other:                           39242
        total:                           313936
    transactions:                        19621  (326.59 per sec.)
    queries:                             313936 (5225.50 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          60.0759s
    total number of events:              19621

Latency (ms):
         min:                                    4.61
         avg:                                   24.47
         max:                                  633.40
         95th percentile:                       38.25
         sum:                               480222.14

Threads fairness:
    events (avg/stddev):           2452.6250/39.54
    execution time (avg/stddev):   60.0278/0.03


Phần kết quả, chúng ta để chỗ `transactions` và chỗ latency

