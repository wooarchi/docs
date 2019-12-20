# DB 백업/복구 가이드
## 사전 준비사항
- CF CLI 설치 
- Mysql client 설치

## 데이터베이스 백업
데이터베이스 백업을 위해서는 서비스와 바인딩된 앱이 있어야합니다. </br>
바인딩 된 앱을 경유(터널링 구성)하여 데이터베이스를 백업받거나 복구하기 떄문입니다.

- Service 정보 확인
cf services 와 바인딩된 앱을 확인합니다. 아래 예시에서는 test-db와 test-php 앱이 바인딩되어 있습니다.
```
$ cf services
Getting services in org 42-88 / space 42-88 as kepri-mng...

name          service   plan    bound apps   last operation
service-001   p-mysql   100mb                create succeeded
test-db       p-mysql   100mb   test-php     create succeeded
```

- 바인딩된 service 접속정보 확인
```
$ cf env test-php
Getting env variables for app test-php in org 42-88 / space 42-88 as kepri-mng...
OK

System-Provided:
{
 "VCAP_SERVICES": {
  "p-mysql": [
   {
    "binding_name": null,
    "credentials": {
     "hostname": "10.20.0.6",
     "jdbcUrl": "jdbc:mysql://10.20.0.6:3306/cf_ead49b80_d1ee_4e03_858b_7e659013a0b9?user=Q9LfOKp3EWSDTC31\u0026password=8OYy7PqXb9sGdKP0",
     "name": "cf_ead49b80_d1ee_4e03_858b_7e659013a0b9",
     "password": "8OYy7PqXb9sGdKP0",
     "port": 3306,
     "uri": "mysql://Q9LfOKp3EWSDTC31:8OYy7PqXb9sGdKP0@10.20.0.6:3306/cf_ead49b80_d1ee_4e03_858b_7e659013a0b9?reconnect=true",
     "username": "Q9LfOKp3EWSDTC31"
    },
    "instance_name": "test-db",
    "label": "p-mysql",
    "name": "test-db",
    "plan": "100mb",
    "provider": null,
    "syslog_drain_url": null,
    "tags": [
     "mysql"
    ],
    "volume_mounts": []
   }
  ]
 }
}

{
 "VCAP_APPLICATION": {
  "application_id": "b8a0c683-26ac-444d-aaef-fedb4f0387b6",
  "application_name": "test-php",
  "application_uris": [
   "test-php.kepri-dev.crossent.com"
  ],
  "application_version": "1ac2e7e5-ebbf-484c-831c-ee8ea778281a",
  "cf_api": "https://api.kepri-dev.crossent.com",
  "limits": {
   "disk": 1024,
   "fds": 16384,
   "mem": 256
  },
  "name": "test-php",
  "space_id": "1e414a5a-7195-4e90-9446-79cfcf61fe6f",
  "space_name": "42-88",
  "uris": [
   "test-php.kepri-dev.crossent.com"
  ],
  "users": null,
  "version": "1ac2e7e5-ebbf-484c-831c-ee8ea778281a"
 }
}

No user-defined env variables have been set

No running env variables have been set

No staging env variables have been set
```

- cf ssh 터널링 구성 (위의 결과로 정보 참조)
```
# cf ssh -N -L (로컬에서 접속할포트):(접속DB 호스트):3306 (어플리케이션 이름)
$ cf ssh -N -L 3306:10.20.0.6:3306 test-php
```
- mysqdump 명령어로 데이터베이스 백업(--single-transaction, --skip-add-locks 옵션을 사용해야합니다.)
```
# mysqldump -h 127.0.0.1 -P 3306 -u 유저 -p패스워드 --single-transaction --skip-add-locks 데이터베이스명 > 백업파일명.sql
$ mysqldump -h 127.0.0.1 -P 3306 -u Q9LfOKp3EWSDTC31 -p8OYy7PqXb9sGdKP0  --single-transaction --skip-add-locks cf_ead49b80_d1ee_4e03_858b_7e659013a0b9 > test-db2.sql
```

## 데이터베이스 복구
- cf ssh 터널링 구성
```
# 터널링 구성을 위한 정보 조회
# cf env (어플리케이션 이름)
$ cf env test-php
Getting env variables for app test-php in org 42-88 / space 42-88 as kepri-mng...
OK

System-Provided:
{
 "VCAP_SERVICES": {
  "p-mysql": [
   {
    "binding_name": null,
    "credentials": {
     "hostname": "10.20.0.6",
     "jdbcUrl": "jdbc:mysql://10.20.0.6:3306/cf_ead49b80_d1ee_4e03_858b_7e659013a0b9?user=Q9LfOKp3EWSDTC31\u0026password=8OYy7PqXb9sGdKP0",
     "name": "cf_ead49b80_d1ee_4e03_858b_7e659013a0b9",
     "password": "8OYy7PqXb9sGdKP0",
     "port": 3306,
     "uri": "mysql://Q9LfOKp3EWSDTC31:8OYy7PqXb9sGdKP0@10.20.0.6:3306/cf_ead49b80_d1ee_4e03_858b_7e659013a0b9?reconnect=true",
     "username": "Q9LfOKp3EWSDTC31"
    },
    "instance_name": "test-db",
    "label": "p-mysql",
    "name": "test-db",
    "plan": "100mb",
    "provider": null,
    "syslog_drain_url": null,
    "tags": [
     "mysql"
    ],
    "volume_mounts": []
   }
  ]
 }
}

{
 "VCAP_APPLICATION": {
  "application_id": "b8a0c683-26ac-444d-aaef-fedb4f0387b6",
  "application_name": "test-php",
  "application_uris": [
   "test-php.kepri-dev.crossent.com"
  ],
  "application_version": "1ac2e7e5-ebbf-484c-831c-ee8ea778281a",
  "cf_api": "https://api.kepri-dev.crossent.com",
  "limits": {
   "disk": 1024,
   "fds": 16384,
   "mem": 256
  },
  "name": "test-php",
  "space_id": "1e414a5a-7195-4e90-9446-79cfcf61fe6f",
  "space_name": "42-88",
  "uris": [
   "test-php.kepri-dev.crossent.com"
  ],
  "users": null,
  "version": "1ac2e7e5-ebbf-484c-831c-ee8ea778281a"
 }
}

No user-defined env variables have been set

No running env variables have been set

No staging env variables have been set

# 터널링 구성
# cf ssh -N -L (로컬에서 접속할포트):(접속DB 호스트):3306 (어플리케이션 이름)
$ cf ssh -N -L 3306:10.20.0.6:3306 test-php
```

- database 삭제 후 생성 
```
# Mysql 원격 접속
$ mysql -h 127.0.0.1 -P 3306 -u 유저 -p패스워드 데이터베이스명
mysql -h 127.0.0.1 -P 3306 -u Q9LfOKp3EWSDTC31 -p8OYy7PqXb9sGdKP0 cf_ead49b80_d1ee_4e03_858b_7e659013a0b9

# 데이터베이스 삭제
drop database cf_ead49b80_d1ee_4e03_858b_7e659013a0b9;

# 데이터베이스 생성
create database cf_ead49b80_d1ee_4e03_858b_7e659013a0b9;
```

- database 복구
```
# mysql -h 127.0.0.1 -P 3306 -u 유저 -p패스워드 데이터베이스명 < 백업파일.sql
mysql -h 127.0.0.1 -P 3306 -u Q9LfOKp3EWSDTC31 -p8OYy7PqXb9sGdKP0 cf_ead49b80_d1ee_4e03_858b_7e659013a0b9 < test-db2.sql

```
