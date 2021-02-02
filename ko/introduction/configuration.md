# 설정 파일
### 개요
고자라니의 설정파일 `gozarani.json`의 기본값을 지정하는 모든 파일은 `config` 폴더에 저장됩니다.

각각의 설정에 대한 설명은 [설정파일 설명](./config_descriptions.md) 을 참고하십시오.
# PHP에서 설정값 조회
환경변수 설정값을 조회하려면 `Config::env()` 함수를 사용합니다.
```php
public static function Config::env(string $key,$default)
```
### 애플리케이션 전용 설정
`config.json` 파일을 사용하면 애플리케이션 전용 설정을 사용할 수 있습니다. DB 테이블 접두사 등으로 사용하실 수 있습니다.
애플리케이션 전용 설정값을 조회하려면 `Config::app()` 함수를 사용합니다.
```php
public static function Config::app(string $key,$default)
```
