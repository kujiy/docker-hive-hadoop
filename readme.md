
# ref
https://hshirodkar.medium.com/apache-hive-on-docker-4d7280ac6f8e


# setup
```Rb
git clone https://github.com/kujiy/docker-hive-hadoop

cd docker-hive-hadoop

docker-compose up

docker ps --format "{{.ID}}     {{.Names}}                      {{.Image}}                      {{.Ports}}"

e0b6a1e55ece    hive-server                     docker-hub-mirror.linecorp.com/bde2020/hive:2.3.2-postgresql-metastore                  0.0.0.0:10000->10000/tcp, 10002/tcp
6b77c2d26103    hive-metastore                  docker-hub-mirror.linecorp.com/bde2020/hive:2.3.2-postgresql-metastore                  10000/tcp, 0.0.0.0:9083->9083/tcp, 10002/tcp
5bd6f27a0977    hive-metastore-postgresql                       docker-hub-mirror.linecorp.com/bde2020/hive-metastore-postgresql:2.3.0                  5432/tcp
a1d6a26ff9d8    datanode                        docker-hub-mirror.linecorp.com/bde2020/hadoop-datanode:2.0.0-hadoop2.7.4-java8                  0.0.0.0:50075->50075/tcp
ca4a6e5d9b55    namenode                        docker-hub-mirror.linecorp.com/bde2020/hadoop-namenode:2.0.0-hadoop2.7.4-java8                  0.0.0.0:50070->50070/tcp

```

### hadoop dashboard
http://localhost:50070/
http://localhost:50075/

# create DB table
```Rb
docker exec -it hive-server /bin/bash

root@dc86b2b9e566:/opt# ls
hadoop-2.7.4  hive

root@dc86b2b9e566:/opt# cd ..

root@dc86b2b9e566:/# ls
bin  boot  dev employee  entrypoint.sh  etc  hadoop-data  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

root@dc86b2b9e566:/# cd employee/

root@dc86b2b9e566:/employee# hive -f employee_table.hql

root@dc86b2b9e566:/employee# hadoop fs -put employee.csv hdfs://namenode:8020/user/hive/warehouse/testdb.db/employee

```


# DB Query
```Rb
root@df1ac619536c:/employee# hive

hive> show databases;
OK
default
testdb
Time taken: 2.363 seconds, Fetched: 2 row(s)

hive> use testdb;
OK
Time taken: 0.085 seconds

hive> select * from employee;
OK
1 Rudolf Bardin 30 cashier 100 New York 40000 5
2 Rob Trask 22 driver 100 New York 50000 4
3 Madie Nakamura 20 janitor 100 New York 30000 4
4 Alesha Huntley 40 cashier 101 Los Angeles 40000 10
5 Iva Moose 50 cashier 102 Phoenix 50000 20
Time taken: 4.237 seconds, Fetched: 5 row(s)

```


# persistence data

```rb
docker-compose down
docker-compose up


docker exec -it hive-server /bin/bash

hive

hive> select * from testdb.employee;

```


