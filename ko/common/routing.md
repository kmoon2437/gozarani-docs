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
여기서 두번째 인자의 `Closure`는 첫번째 인자로 `\ZXE\Request` 객체를 받아야 합니다.(사실 타입을 지정해줄 필요는 없습니다)
```php
$router->get('/gozarani',function(\ZXE\Request $req){
    return '<h1>An nyeong haseyo</h1>';
});
```

이때 `Closure` 객체가 문자열을 반환하면, 고자라니는 해당 문자열을 그대로 출력합니다.
### 사용 가능한 http 메서드
고자라니의 라우터는 다음의 http 메서드에 해당하는 라우터를 등록할 수 있습니다.
```php
$router->get(string $path,$callback);
$router->post(string $path,$callback);
$router->put(string $path,$callback);
$router->patch(string $path,$callback);
$router->delete(string $path,$callback);
```

때로는 모든 http 메서드에 대응하는 라우트가 필요할 때가 있습니다.
그럴 땐 `$router->all(string $path,$callback)` 함수를 사용할 수 있습니다.
```php
$router->all('/gozarani',function(\ZXE\Request $req){
    return '<h1>An nyeong haseyo</h1>';
});
```
### 파라미터
몇몇 사이트를 보면 `https://namu.wiki/w/BASE64`,`https://github.com/kmoon2437/gozarani` 처럼 url에 직접 파라미터를 넣게 되는데,
이런 라우트는 아래와 같이 사용합니다.
```php
$router->get('/pok8/:level',function(\ZXE\Request $req){
    return '<h1>'.$req->param['level'].'단 폭☆8</h1>';
});
```

특정 파라미터를 선택적으로 넣고 싶다면 아래와 같이 파라미터명 뒤에 `?` 를 입력합니다.
```php
$router->get('/pok8/:level?',function(\ZXE\Request $req){
    return '<h1>'.($req->param['level'] ?? 3).'단 폭☆8</h1>';
});
```

정규식을 통해 파라미터의 포맷을 제한하려면 다음과 같이 변수명 뒤에 정규식 패턴을 입력합니다.
```php
$router->get('/pok8/:level([0-9]+)',function(\ZXE\Request $req){
    return '<h1>'.$req->param['level'].'단 폭☆8</h1>'; // level이 숫자가 아니면 404 응답이 돌아옴
});
```
