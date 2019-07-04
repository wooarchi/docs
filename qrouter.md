### qrouter access
- 호라이즌에서 해당되는 router의 qrouter id 확인
![image](https://user-images.githubusercontent.com/13977405/60639404-87dc0000-9e5d-11e9-9c84-d2a9a0cc8f97.png)
- controller 접속 후 sudo 권한 흭득
```
$ sudo su -
```

- qrouter 접속
```
$ ip netns qrouter-772f82d5-a344-44d2-a2b8-df5ef903cfca /bin/sh
```

- 168.78.82.243 과 통신 확인
```
$ ping  168.78.82.243
```
