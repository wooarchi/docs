### CF 컨테이너 호스트 OS에 DNS 서버 등록
#### Paas-inception VM에서 진행
- diego-cell 접속 (모든 diego-cell에 반영해야 합니다.)
```
$ bosh -e paas-bosh -d cf ssh diego-cell/0
```

- root 권한 흭득
```
$ sudo su
```

- resolve.conf에 nameserver 추가 (기존의 namsever를 지워서는 안됩니다.)
```
$ vi /etc/resolve.conf

nameserver 168.78.82.224
```

반영을 위해서는 앱을 재시작 해야합니다.
