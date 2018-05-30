# PaaS 도메인 변경
## 절차
(1) 변경할 도메인 추가
```
$ cf create-shared-domain
```
(2) 모든앱에 변경할 도메인 라우터로 등록
```
$ cf map-route
```

(3) 기존의 도메인 및 연결된 라우트 제거
```
$ cf delete-shared-domain
```

(4) 변경할 도메인으로 PaaS 업데이트 
※ PaaS 업데이트시 변경할 도메인의 인증서 업데이트 필요 (cf-vars.yml에 재정의)
```
$ bosh deploy
```
