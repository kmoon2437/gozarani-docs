# 라우팅
## 라우트 클래스
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
## 기본적인 라우트
> Note: 여기부터는 `\ZXE\Router` 객체의 변수명이 `$router` 인 것으로 가정하고 설명합니다.

예를 들어 `http://your-domain.kr/gozarani` 에 접속해서 `<h1>An nyeong haseyo</h1>`라는 응답이 돌아오도록 할 때
다음과 같이 첫 인자에 경로를, 두번째 인자에 `Closure`를 집어넣어 다음과 같이 만들 수 있습니다.  
여기서 두번째 인자의 `Closure`는 첫번째 인자로 `\ZXE\Request` 객체를 받아야 합니다.(사실 타입을 지정해줄 필요는 없습니다)
```php
$router->get('/gozarani',function(\ZXE\Request $req){
    return '<h1>An nyeong haseyo</h1>';
});
```

이때 `Closure` 객체가 문자열을 반환하면, 고자라니는 해당 문자열을 그대로 출력합니다.
## 사용 가능한 http 메서드
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
## 응답 종류
> Note: `Req` 클래스는 실제로는 `\ZXE\Request` 클래스이며, `Resp` 클래스는 실제로는 `\ZXE\Response` 클래스이니 오해 마십시오.  
두 클래스 모두 use문을 사용해 각각 `Req`,`Resp` 클래스인 것처럼 사용할 수 있습니다.

고자라니의 라우터는 일반 문자열,View(템플릿),리다이렉트,알림창 4가지의 응답을 돌려줄 수 있습니다.
### 일반 문자열
아래와 같이 `Closure`에서 문자열을 반환하면 그것을 그대로 출력합니다.
```php
$router->get('/pok8',function(Req $req){
    return '<h1>1972년 11월 21일...</h1>';
});
```
### View(템플릿)
`resources/views` 안의 템플릿 파일들을 렌더링해 응답으로 전송합니다.  
아래와 같이 `Closure`에서 `\ZXE\Response::view(string $name,$title = '',array $variables = [])`를 사용합니다.
> 자세한 View 사용법은 [템플릿 엔진](./template.md)을 참고하십시오.
```php
$router->get('/pok8',function(Req $req){
    return Resp::view('big8ang');
});
```
### 리다이렉트
지정된 url로 페이지를 이동합니다.
아래와 같이 `Closure`에서 `\ZXE\Response::redirect(string $url)` 를 사용합니다.
```php
$router->get('/index.goza',function(Req $req){
    return Resp::redirect('/');
});
```
#### 영구 리다이렉트(301)
기존의 리다이렉트는 기본적으로 http 응답 코드를 `302`로 설정합니다.
http 응답 코드가 `301`인 영구 리다이렉트를 하려면 다음과 같이 `\ZXE\Response::redirect_301(string $url)` 을 사용합니다.
```php
$router->get('/index.goza',function(Req $req){
    return Resp::redirect_301('/');
});
```
### 알림창
클라이언트에서 `JavaScript`의 `alert()` 함수를 호출해 알림창을 표시합니다.
아래와 같이 `Closure`에서 `\ZXE\Response::alert(string $msg,string $url = '')` 을 사용합니다.
> Note: 두번째 인자는 생략 가능하며, 여기에 url을 집어넣으면 알림창을 닫았을때
`JavaScript`의 `location.replace()` 함수를 사용해 해당 url로 리다이렉트 합니다.
```php
$router->get('/pok8',function(Req $req){
    return Resp::alert('1972년 11월 21일....','/');
});
```
## 파라미터
몇몇 사이트를 보면 `https://namu.wiki/w/BASE64`,`https://github.com/kmoon2437/gozarani` 처럼 url에 직접 파라미터를 넣게 되는데,
이런 라우트는 아래와 같이 사용합니다.
```php
$router->get('/pok8/:level',function(Req $req){
    return '<h1>'.$req->param['level'].'단 폭☆8</h1>';
});
```

특정 파라미터를 선택적으로 넣고 싶다면 아래와 같이 파라미터명 뒤에 `?` 를 입력합니다.
```php
$router->get('/pok8/:level?',function(Req $req){
    return '<h1>'.($req->param['level'] ?? 3).'단 폭☆8</h1>';
});
```

정규식을 통해 파라미터의 포맷을 제한하려면 다음과 같이 변수명 뒤에 정규식 패턴을 입력합니다.
```php
$router->get('/pok8/:level([0-9]+)',function(Req $req){
    return '<h1>'.$req->param['level'].'단 폭☆8</h1>'; // level이 숫자가 아니면 404 응답이 돌아옴
});
```

변수명 뒤에 다음과 같이 `+` 을 넣으면 이후에 나오는 슬래시(`/`)를 무시합니다.
```php
$router->get('/pok8/:level+',function(Req $req){
    /*
    이렇게 할 시 /pok8/1972/trigger 에 접속하면
    $req->param['level'] 에 '1972/trigger' 가 할당됨
    결과적으로, <h1>1972/trigger단 폭☆8</h1> 이 응답으로 돌아옴
    */
    return '<h1>'.$req->param['level'].'단 폭☆8</h1>';
});
```

변수명 뒤에 다음과 같이 `*` 을 넣으면 `+`을 넣은 것과 동일하나, 선택 파라미터가 됩니다.
```php
$router->get('/pok8/:level*',function(Req $req){
    return '<h1>'.$req->param['level'].'단 폭☆8</h1>';
});
```
