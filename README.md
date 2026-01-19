# 강력한 Rust 매크로 작성하기

## 개요

이것은 매닝(Manning)의 _Write Powerful Rust Macros_ [책](http://mng.bz/e1lv)의 코드입니다.

코드는 챕터별로 구성되어 있습니다. 연습 문제 풀이는 별도의 부록 디렉토리에 위치하지 _않으며_, 대신 해당 챕터의 예제 코드 옆에 위치하며 항상 `-exercise`로 끝납니다.
연습 문제 풀이를 제외한 모든 프로젝트는 완전한 예제를 나타내거나 (많은 챕터가 단일 예제를 중심으로 구성되므로) 해당 예제의 개발 단계를 보여줍니다.
각 프로젝트에는 해당 챕터의 어느 부분과 관련되어 있는지에 대한 아이디어를 제공하는 README 파일이 포함되어 있습니다.

## 유틸리티 (Util)

`util` 디렉토리는 이 책에서 사용되는 것과 같은 절차적 매크로 설정을 생성할 수 있는 스크립트 `create_setup.sh`를 포함하고 있으며, 1개, 2개 또는 3개의 하위 디렉토리를 가질 수 있습니다.
세 가지 옵션 모두 책에서 다룹니다.

사용법:

```bash
./create_setup.sh name 2
```

## 정오표 (Errata)

이것들을 제보해주신 독자분들께 다시 한번 감사드립니다!

**1.2절 (4쪽)**

템플릿이 있는 것은 C가 아니라 C++입니다.

**1.3.2절 (7쪽)**

`#[derive(Clone)}`는 `#[derive(Clone)]`이어야 합니다.

**2.1.4절 (15쪽)**

제목의 _Nonemtpy_는 _nonempty_여야 합니다.

**2.1.4절 (18쪽)**

`$[($x:expr),+] => (:`는 `:`로 끝나지 않아야 합니다.

**2.2.4절 (30쪽)**

`println!("Poor: {the.poor?}, rich: {the.rich?}");`는 `println!(“Poor: {the_poor:?}, rich: {the_rich:?}”);`이어야 합니다.

**2.4절 (39쪽)**

_trailing comments_는 _trailing commas_여야 합니다.

**5.1절 (81쪽)**

_it makes perfect sense for the input of a function-like macro to replace the output_는 _it makes perfect sense for the output of a function-like macro to replace the input_이어야 합니다.

**5.1절 (82, 84, 87쪽)**

이 챕터에서 `quote::__private::TokenStream`은 `proc_macro2::TokenStream`이어야 합니다.

**5.1절 (84쪽)**

'못생긴 반환 타입(ugly return type)'에 대해 현재 264–265쪽을 참조하면 유용할 것입니다.

_Now focus on the code inside map._은 리스팅을 참조하면 더 명확할 것입니다: _Now focus on the code inside map in listing 5.4._

명확성을 위해, _So we need an identifier._를 다음으로 교체하세요:
_The simplest way to solve the issue is to use `TokenStream::from_str`. This method will try to turn your string into a `TokenStream` that we can use as an identifier. The other approach is to construct an identifier ourselves._

**5.3절 (90쪽)**

`composed#E`는 `composed(5)`여야 합니다.

**6.4.3절 (114쪽)**

사소함: `pub` 함수는 `pub(crate)`일 수 있습니다.

**6.4.5절 (120쪽)**

`assert_eq!(gleipnir.other_necessities.len(), 3)`는 `assert_eq!(gleipnir.other_necessities.len(), 3);`가 더 바람직합니다.

**7.5절 (138쪽)**

`use quote::{ToTokens};`는 `use quote::ToTokens;`이어야 합니다.

**7.7절 (143쪽)**

리스팅의 `let mut ast: ItemFn = syn::parse2(item).unwrap();`는 `let mut ast: ItemFn = syn::parse(item).unwrap();`이어야 합니다.

`ast.to_token_stream()`는 `ast.to_token_stream().into()`이어야 합니다. `into()`는 코드에는 있었으나 교정본에서 사라진 것 같습니다.

**7.9절 (149쪽)**

`-> TokenStream`은 `-> ReturnType`이어야 하고, `match ast.sig.output`은 `let output = match ast.sig.ouput`이어야 하며, `Ok(quote! {...} )`는 `quote! {...}`이어야 하고, `syn::parse2(output).unwrap()`이 함수의 마지막 줄이어야 합니다. 이 모든 것이 책의 코드에서는 정확했지만, 텍스트에서 뭔가 잘못되었습니다.

**7.9.2절 (155쪽)**

`proc_macro_error`는 syn 버전 1을 사용합니다. syn 버전 2를 사용하는 `proc-macro-error-2`(원래 프로젝트의 포크)가 있습니다.

**7.10절 (160-162쪽)**

Shuttle은 1장에서 더 이상 언급되지 않습니다. 이 프로젝트는 2026년 1월부터 중단되었습니다.

**8.1절 (164쪽)**

테스트들이 `builder-usage`의 `main.rs`에 추가되었습니다 (버그는 아니지만, 주어진 테스트들을 어디에 배치해야 할지 불분명합니다).

**8.3절 (175쪽)**

`.expect(concat!`는 `expect(concat!`이어야 합니다.

**10.1.2절 (237쪽)**

`Ok`와 `?`는 불필요합니다. 따라서:

```
Ok(serde_yaml::from_reader(file)
        .map_err(|e| {
            syn::Error::new(Span::call_site(), e.to_string())
        })?)
```

이것은 다음으로 변경될 수 있습니다:

```
serde_yaml::from_reader(file)
        .map_err(|e| {
            syn::Error::new(Span::call_site(), e.to_string())
        })
```

**10.7절 (258쪽)**

스택오버플로우(SO)와 레딧(Reddit) 외에도, 공식 [Rust 포럼](https://users.rust-lang.org/)도 언급되어야 합니다.

**부록 (260쪽)**

_trailing comments_는 _trailing commas_이어야 합니다.
