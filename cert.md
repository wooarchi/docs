### PaaS 인증서 확인
- Inception 접속 후 작업
- CF 인증서 확인
```
$ cd deployment
$ openssl crl2pkcs7 -nocrl -certfile <(sed -n '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' *vars.yml | sed -e 's/^[ \t]*//') | openssl pkcs7 -print_certs -text -noout | sed -e 's/^[ \t]*//' | grep -E "Issuer:|Subject:|Not\ After\ :" | awk '{ if ((NR % 3) == 1) printf("\n*******\n\n"); print; }' | grep After
```

- BOSH 인증서 확인
```
$ openssl crl2pkcs7 -nocrl -certfile <(sed -n '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' creds.yml | sed -e 's/^[ \t]*//') | openssl pkcs7 -print_certs -text -noout | sed -e 's/^[ \t]*//' | grep -E "Issuer:|Subject:|Not\ After\ :" | awk '{ if ((NR % 3) == 1) printf("\n*******\n\n"); print; }' | grep After
```

#### 위 명령어로 조회되는 날짜 확인
- 예시
```
Not After : Oct 24 07:54:39 2019 GMT
Not After : Oct 24 07:54:20 2019 GMT
Not After : Oct 24 07:54:34 2019 GMT
Not After : Oct 24 07:54:29 2019 GMT
Not After : Oct 24 07:54:37 2019 GMT
Not After : Oct 24 07:54:20 2019 GMT
Not After : Oct 24 07:54:20 2019 GMT
Not After : Oct 24 07:54:16 2019 GMT
Not After : Oct 24 07:54:16 2019 GMT
Not After : Oct 24 07:54:16 2019 GMT
Not After : Oct 24 07:54:17 2019 GMT
Not After : Oct 24 07:54:16 2019 GMT
Not After : Oct 24 07:54:17 2019 GMT
Not After : Oct 24 07:54:40 2019 GMT
Not After : Oct 24 07:54:40 2019 GMT
Not After : Oct 24 07:54:40 2019 GMT
Not After : Oct 24 07:54:41 2019 GMT
Not After : Oct 24 07:54:40 2019 GMT
Not After : Oct 24 07:54:41 2019 GMT

```
