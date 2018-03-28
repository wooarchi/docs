# Postgres HA 테스트

## Postgres info
- vm 형상
```
$ bosh -e paas-bosh -d postgres vms
Instance                                       Process State  AZ  IPs           VM CID                                VM Type  
postgres/616ca107-754b-4b66-91ac-ff59854fdf49  running        z1  10.10.10.210  ff0d2e7d-e9e3-4683-b8b5-c37e943e6f3e  large    
postgres/c93f1434-602c-48eb-89d1-86915ba51a64  running        z2  10.10.10.211  1de8b8ca-7f16-435d-8221-c44371145bae  large    

2 vms
```

- master 
postgres/0(10.10.10.210) - Matster
```
$ psql -h 10.10.10.210 -U admin -p 6432 -c "select application_name, state, sync_priority, sync_state from pg_stat_replication;"
Password for user admin: 
 application_name |   state   | sync_priority | sync_state 
------------------+-----------+---------------+------------
 walreceiver      | streaming |             0 | async
(1 row)
```

-slave
postgres/1(10.10.10.211) - Slave
```
$ psql -h 10.10.10.211 -U admin -p 6432 -c "select application_name, state, sync_priority, sync_state from pg_stat_replication;"
Password for user admin: 
 application_name | state | sync_priority | sync_state 
------------------+-------+---------------+------------
(0 rows)

```

## nomal test
- 테스트 계정 및 데이터베이스 생성 (md5는 수동 등록 후 pgpool 리로드)
```
$ psql -h 10.10.10.211 -U admin -d postgres -c "CREATE DATABASE testdb";
$ psql -h 10.10.10.211 -U admin -d postgres -c "CREATE USER testuser PASSWORD 'test1234'";
$ psql -h 10.10.10.211 -U admin -d postgres -c "GRANT ALL PRIVILEGES ON DATABASE testdb TO testuser";
$ psql -h 10.10.10.211 -U testuser -d testdb -c "create table testtable (testkey integer)";
```

## Failover test


## Failback test

### 
