### python buildpack 업데이트
```
.
```


### requirements.txt 설정
- repository를 사용하기전
```
dj-database-url==0.4.1
Django==1.9.7
gunicorn==19.6.0
waitress==0.9.0
whitenoise==3.2
```

- repository를 사용할 경우 (repository IP: 192.168.0.1)
  - 추가내용
  ```
  --index-url=[Respository서버 주소]/simple
  --trusted-host=[Respository서버 주소]
  ```
  - 적용
  ```
  dj-database-url==0.4.1
  Django==1.9.7
  gunicorn==19.6.0
  waitress==0.9.0
  whitenoise==3.2
  --index-url=http://192.168.0.1/simple
  --trusted-host=192.168.0.1
  ```
