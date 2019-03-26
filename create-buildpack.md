### 빌드팩 배포
아래 작업은 모두 paas inception vm에서 수행하는 것으로 함
- 패스워드 확인
```
$ cd deployment
$  cat cf-vars.yml|grep admin_password
cf_admin_password: *******************
```
- CF 로그인
```
$ cf login
API endpoint: 

Email> admin

Password> 
Authenticating...
OK
```

- 빌드팩 조회: 마지막 position 확인
```
$ cf buildpacks
Getting buildpacks...

buildpack                      position   enabled   locked   filename                           stack
ruby_buildpack-v1-7-19         1          true      false    ruby-buildpack-v1.7.19.zip
java_buildpack-v4-12           2          true      false    java-buildpack-v4.12.zip
go_buildpack-v1-8-23           3          true      false    go-buildpack-v1.8.23.zip
python_buildpack-v1-6-17       4          true      false    python-buildpack-v1.6.17.zip
staticfile_buildpack-v1-4-28   5          true      false    staticfile-buildpack-v1.4.28.zip
dotnet_core_buildpack-v2-0-7   6          true      false    dotnet-core-buildpack-v2.0.7.zip
nodejs_buildpack-v1-6-25       7          true      false    nodejs-buildpack-v1.6.25.zip
php_buildpack-v4-3-56          8          true      false    php-buildpack-v4.3.56.zip
binary_buildpack-v1-0-21       9          true      false    binary-buildpack-v1.0.21.zip
staticfile_buildpack           10         false     false    staticfile-buildpack-v1.4.28.zip
java_buildpack                 11         false     false    java-buildpack-v4.12.zip
ruby_buildpack                 12         false     false    ruby-buildpack-v1.7.19.zip
dotnet_core_buildpack          13         false     false    dotnet-core-buildpack-v2.0.7.zip
nodejs_buildpack               14         false     false    nodejs-buildpack-v1.6.25.zip
go_buildpack                   15         false     false    go-buildpack-v1.8.23.zip
python_buildpack               16         false     false    python-buildpack-v1.6.17.zip
php_buildpack                  17         false     false    php-buildpack-v4.3.56.zip
binary_buildpack               18         false     false    binary-buildpack-v1.0.21.zip
egov_buildpack                 19         false     false    egov-buildpack-v3.5.zip
```
- 빌드팩 배포: 포지션은 조회한 빌드팩의 마지막 숫자 다음 번호 입력
```
$ cf create-buildpack java_buildpack-v4-1-5 {빌드팩 경로} {Postion} --enable
```
