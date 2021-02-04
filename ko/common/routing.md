# 라우팅
### 라우터 클래스
환경변수 `app.main_class`에 설정된 클래스를 라우터 클래스로 사용합니다.

라우터 클래스 파일의 예시(기본으로 들어있는 `classes/App/WebRoutes.php` 파일):
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

위 코드에서 봤듯이 라우터 클래스는 `\ZXE\EntryPoint` 인터페이스를 구현해야 합니다.

기본 제공되는 라우터 파일이 있지만, 원한다면 `classes/App/GozarnaiRoutes.php` 처럼 라우터 파일을 따로 만들어 사용할 수도 있습니다.
이 경우 `gozarani.json` 파일의 `app.main_class` 변수에 사용할 라우터 클래스명 전체를 지정해야 합니다.
### 기본적인 라우트

```php
```
