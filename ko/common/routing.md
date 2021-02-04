# 라우팅
### 라우트 클래스
환경변수 `app.main_class`에 설정된 클래스를 라우트 클래스로 사용합니다.

라우트 클래스 파일의 예시(기본으로 들어있는 `classes/App/WebRoutes.php` 파일):
```php
namespace App;

use \ZXE\EntryPoint;
use \ZXE\Request as Req;
use \ZXE\Response as Resp;

class WebRoutes implements EntryPoint{
    public function get_routes(\ZXE\Router &$router){
        $router->get('/',function(Req $req){
            return '<h1>Eight Gozarani!!!!</h1>';
        });

        $router->get('/index.goza',function(Req $req){
            return Resp::redirect('/');
        });
    }
}
```

위 코드에서 봤듯이 라우트 클래스는 `\ZXE\EntryPoint` 인터페이스를 구현해야 하며, `get_routes()` 함수는 첫번째 인자로 `\ZXE\Router` 객체를 참조 변수로 받아야 합니다.

기본 제공되는 라우트 파일이 있지만, 원한다면 `classes/App/GozarnaiRoutes.php` 처럼 라우트 파일을 따로 만들어 사용할 수도 있습니다.  
이 경우 `gozarani.json` 파일의 `app.main_class` 변수에 사용할 라우트 클래스명 전체를 지정해야 합니다.
### 기본적인 라우트
> Note: 여기부터는 \ZXE\Router 객체의 변수명이 $router 인 것으로 가정하고 설명합니다.

예를 들어 `http://your-domain.kr/gozarani` 에 접속해서 `<h1>An nyeong haseyo</h1>`라는 응답이 돌아오도록 할 때
다음과 같이 첫 인자에 경로를, 두번째 인자에 `Closure`를 집어넣어 다음과 같이 만들 수 있습니다.  
여기서 두번째 인자의 `Closure`는 첫번째 인자로 `\ZXE\Request` 객체를 받아야 합니다.
```php
$router->get('/gozarani',function(\ZXE\Request $req){
    return '<h1>An nyeong haseyo</h1>';
});
```

이때 `Closure` 객체가 문자열을 반환하면, 고자라니는 해당 문자열을 그대로 출력합니다.
