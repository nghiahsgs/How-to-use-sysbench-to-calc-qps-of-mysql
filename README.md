# How-to-use-sysbench-to-calc-qps-of-mysql
How to use sysbench to calc qps of mysql


Step 1: Install mysql on vps
```
Host: 1.2.3.4
Port: 3306
User: root
Password: my-secret-pw
Database: test
```
Step 2: Install sysbench on mac os
```
brew install sysbench
```
when install, see carefully the logs to get the path of sysbench on macos
```
==> Installing sysbench
==> Pouring sysbench--1.0.20_3.monterey.bottle.tar.gz
🍺  /usr/local/Cellar/sysbench/1.0.20_3: 89 files, 570.0KB
```

=> /usr/local/Cellar/sysbench/1.0.20_3 is the path of sysbench


Step 2: Prepare data
```
sysbench --db-driver=mysql --mysql-user=root --mysql_password=my-secret-pw --mysql-db=test --mysql-host=1.2.3.4 --mysql-port=3306 --tables=16 --table-size=10000 --threads=4 --time=200 --events=0 --report-interval=1 “/usr/local/Cellar/sysbench/1.0.20_3/share/sysbench/oltp_read_write.lua” prepare
```

In the command above: 
```
/usr/local/Cellar/sysbench/1.0.20_3/share/sysbench/oltp_read_write.lua => lua script in mac
--mysql-user=root   => username of mysql
--mysql_password=my-secret-pw   => password of mysql
--mysql-db=test => database name
--mysql-host=1.2.3.4 => host mysql
--mysql-port=3306 => port mysql
--tables=16 => create dummy data for 16 tables
--table-size=10000 => Each table has 10000 records
--threads=4 => macos call 4 threads
--time=200 => time to test: 200 s
--report-interval=1 => report every second
```


Step 3: Run benchmark to get QPS (query per second)
```
sysbench --db-driver=mysql --mysql-user=root --mysql_password=my-secret-pw --mysql-db=test --mysql-host=1.2.3.4 --mysql-port=3306 --tables=16 --table-size=10000 --threads=4 --time=200 --events=0 --report-interval=1 “/usr/local/Cellar/sysbench/1.0.20_3/share/sysbench/oltp_read_write.lua” run
```

```
[ 1s ] thds: 4 tps: 0.00 qps: 67.38 (r/w/o: 55.49/7.93/3.96) lat (ms,95%): 0.00 err/s: 0.00 reconn/s: 0.00
```
one second: 55 QPS for read
one second: 7 QPS for write