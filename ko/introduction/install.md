# 설치하기
## 시스템 요구사항
- PHP 7.3 이상
- Mysql 사용시 5.5 이상 또는 이에 대응하는 mariadb
- 필수 PHP확장 : JSON,PDO
- 리눅스 사용 권장(원한다면 윈도우에도 깔수 있음)
## 설치
https://github.com/kmoon2437/gozarani 에 접속한 다음, download zip을 사용해 파일을 다운받습니다.  
git clone을 사용한다면 적당히 웹루트로 쓰기에 좋은 폴더에 다운받습니다.  
download zip으로 받았다면 zip파일의 압축을 풀어줍니다. 그런 다음, 서버에 적당한 폴더를 하나 만들어 거기에 업로드합니다.
## 설정
설치를 다 했다면, 웹루트(document root)를 아까 그 적당한 폴더 아래의 public 폴더로 지정해야 합니다.  
여기에 있는 index.php는 모든 페이지 라우트에 대한 프론트 컨트롤러로 동작합니다.
## 환경변수 설정 파일
gozarani.json 파일입니다. 문서화가 되어있으니, 각 설정들의 용도가 궁금하다면 [문서](./envvars.md)를 보십시오.
## 권한(퍼미션)
storage 폴더와 그 안에 있는 모든 것들은 고자라니에 의해 읽기/쓰기가 모두 가능해야 합니다. 그렇지 않으면 고자라니는 진짜로 고자가 됩니다. 이 다시 말해서, 작동을 못 한다는 것이오.
## Rewrite 설정
웹서버가 nginx 서버라면 public/.htaccess 파일을 삭제하고, 설정파일에 다음을 추가합니다.
```
location / {
    try_files $uri $uri/ /index.php?$query_string;
}
```

Apache 서버라면 public/.htaccess 파일이 먹히기 때문에 특별히 설정을 해줄 필요 없습니다.
## 끝!
기본 설정이 끝났습니다. 이제 실컷 갖고 노시면 됩니다!(...)
