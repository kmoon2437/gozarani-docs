# 설정 파일
### 개요
고자라니의 환경변수 설정파일 `gozarani.json`의 기본값을 지정하는 파일은 `config/env.php` 파일입니다.

각각의 환경변수에 대한 설명은 [설정파일 설명](./config_descriptions.md) 을 참고하십시오.
### PHP에서 환경변수값 조회
환경변수 설정값을 조회하려면 `Config::env()` 함수를 사용합니다.
```php
// 정의
public static function Config::env(string $key,$default)

// 사용 예
Config::env('app.key');
```
그런데 분명 위의 `Config::env()` 함수는 `app.key`의 값을 불러오는데, 정작 `gozarani.json` 파일을 보면 `app.key`는 보이지 않습니다.
사실 위의 `app.key`와 같이 `Config::env()` 함수와 후술할 `Config::app()` 함수는 설정값이 객체 안의 객체 안의...안의 객체에 있을 때 하위 요소를 `.`으로 연결해 접근할 수 있습니다.
### 애플리케이션 전용 설정
`config.json` 파일을 사용하면 애플리케이션 전용 설정을 사용할 수 있습니다. DB 테이블 접두사 등으로 사용하실 수 있습니다.
애플리케이션 전용 설정값을 조회하려면 `Config::app()` 함수를 사용합니다. 사용법은 `Config::env()` 함수와 같습니다.
```php
// 정의
public static function Config::app(string $key,$default)

// 사용 예
Config::app('exam.shireo');
```
