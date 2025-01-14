<!--info-header-start--><h1>Get Return Type <img src="https://img.shields.io/badge/-%EB%B3%B4%ED%86%B5-d9901a" alt="보통"/> <img src="https://img.shields.io/badge/-%23infer-999" alt="#infer"/> <img src="https://img.shields.io/badge/-%23built--in-999" alt="#built-in"/></h1><blockquote><p>by Anthony Fu <a href="https://github.com/antfu" target="_blank">@antfu</a></p></blockquote><p><a href="https://tsch.js.org/2/play/ko" target="_blank"><img src="https://img.shields.io/badge/-%EB%8F%84%EC%A0%84%ED%95%98%EA%B8%B0-3178c6?logo=typescript&logoColor=white" alt="도전하기"/></a> &nbsp;&nbsp;&nbsp;<a href="./README.md" target="_blank"><img src="https://img.shields.io/badge/-English-gray" alt="English"/></a>  <a href="./README.zh-CN.md" target="_blank"><img src="https://img.shields.io/badge/-%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87-gray" alt="简体中文"/></a>  <a href="./README.ja.md" target="_blank"><img src="https://img.shields.io/badge/-%E6%97%A5%E6%9C%AC%E8%AA%9E-gray" alt="日本語"/></a>  <a href="./README.pt-BR.md" target="_blank"><img src="https://img.shields.io/badge/-Portugu%C3%AAs%20(BR)-gray" alt="Português (BR)"/></a> </p><!--info-header-end-->

내장 제네릭 `ReturnType<T>`을 이를 사용하지 않고 구현하세요.

예시:

```ts
const fn = (v: boolean) => {
  if (v)
    return 1
  else
    return 2
}

type a = MyReturnType<typeof fn> // should be "1 | 2"
```

<!--info-footer-start--><br><a href="../../README.ko.md" target="_blank"><img src="https://img.shields.io/badge/-%EB%8F%8C%EC%95%84%EA%B0%80%EA%B8%B0-grey" alt="돌아가기"/></a> <a href="https://tsch.js.org/2/answer/ko" target="_blank"><img src="https://img.shields.io/badge/-%EC%A0%95%EB%8B%B5%20%EA%B3%B5%EC%9C%A0%ED%95%98%EA%B8%B0-teal" alt="정답 공유하기"/></a> <a href="https://tsch.js.org/2/solutions" target="_blank"><img src="https://img.shields.io/badge/-%EC%A0%95%EB%8B%B5%20%EB%B3%B4%EA%B8%B0-de5a77?logo=awesome-lists&logoColor=white" alt="정답 보기"/></a> <!--info-footer-end-->

## 풀이

```ts
type MyReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
```

## 학습 내용

1. 제네릭 extends
   - 제네릭에 extends를 사용하면 교집합처럼 사용 되어 제한 조건을 걸 수 있다.
   - T extneds (...args: never[]) => unknown: 인자를 받지 않고 어떤 값이라도 반환하지만 그 타입을 알 수 없는 함수로 제약한다.
   - 
```ts
     // ex
     type MyType<T extends string> = T;
     type Test1 = MyType<"hello">; // ✅ "hello"라는 특정 문자열 타입
     type Test2 = MyType<string>; // ✅ 일반 string 타입
```

2. unkown 반환 
  - 어떠한 값이라도 반환 함 void와 반대개념

3. 제네릭과 타입 지정
  <> 안의 제네릭은 타입을 동적으로 받아 그 타입을 제어하는 부분, = 이후에는 그 제네릭을 어떻게 처리할지 혹은 타입을 어떻게 정의할지 설정

4. infer
   - 타입을 자동으로 추론하게 해주는 키워드. 주로 조건부 타입에서 사용
   - infer R: **R**은 추론된 반환 타입
