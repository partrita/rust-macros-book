# Config 매크로

## 개요

이 크레이트는 yaml 설정을 애플리케이션에서 사용할 수 있는 구조체로 변환할 수 있게 해주는 매크로를 포함하고 있습니다.

## 사용법

함수형 `config!` 매크로를 호출하거나 (가급적 비어 있는) 구조체에 속성 매크로 `#[config_struct]`를 주석으로 달아 사용합니다.
기본적으로, 매크로는 `configuration/config.yaml`에서 설정을 찾습니다.
이는 'path' 속성을 사용하여 덮어쓸 수 있습니다: `config!(path = "a/path/to.yaml")` 또는 `#[config_struct(path = "a/path/to.yaml")]`.

## 사용 예시

```rust
use config_macro::config;

config!(path  = "./configuration/config.yaml");

// 이제 new를 호출하고 값의 해시맵에 접근할 수 있습니다
let c = Config::new();
```

## 기능 (Features)

주석 매크로는 'struct' 기능 뒤에 숨겨져 있습니다.

## 주의사항

현재는 YAML 파일에서만 작동하며, 중첩을 지원하지 않습니다.
따라서 이것은 작동합니다:

```yaml
user: "admin"
```

하지만 이것은 작동하지 않습니다:

```yaml
database:
  user: "admin"
```

게다가, 설정의 모든 속성을 `String`으로 읽습니다.
