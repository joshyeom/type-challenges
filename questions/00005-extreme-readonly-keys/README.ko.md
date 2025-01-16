<!--info-header-start--><h1>Get Readonly Keys <img src="https://img.shields.io/badge/-%EB%A7%A4%EC%9A%B0%20%EC%96%B4%EB%A0%A4%EC%9B%80-b11b8d" alt="매우 어려움"/> <img src="https://img.shields.io/badge/-%23utils-999" alt="#utils"/> <img src="https://img.shields.io/badge/-%23object--keys-999" alt="#object-keys"/></h1><blockquote><p>by Anthony Fu <a href="https://github.com/antfu" target="_blank">@antfu</a></p></blockquote><p><a href="https://tsch.js.org/5/play/ko" target="_blank"><img src="https://img.shields.io/badge/-%EB%8F%84%EC%A0%84%ED%95%98%EA%B8%B0-3178c6?logo=typescript&logoColor=white" alt="도전하기"/></a> &nbsp;&nbsp;&nbsp;<a href="./README.md" target="_blank"><img src="https://img.shields.io/badge/-English-gray" alt="English"/></a>  <a href="./README.zh-CN.md" target="_blank"><img src="https://img.shields.io/badge/-%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87-gray" alt="简体中文"/></a>  <a href="./README.ja.md" target="_blank"><img src="https://img.shields.io/badge/-%E6%97%A5%E6%9C%AC%E8%AA%9E-gray" alt="日本語"/></a>  <a href="./README.pt-BR.md" target="_blank"><img src="https://img.shields.io/badge/-Portugu%C3%AAs%20(BR)-gray" alt="Português (BR)"/></a> </p><!--info-header-end-->

객체의 readonly key 유니언을 반환하는 `GetReadonlyKeys<T>` 제네릭을 구현하세요.

예시

```ts
interface Todo {
  readonly title: string
  readonly description: string
  completed: boolean
}

type Keys = GetReadonlyKeys<Todo> // expected to be "title" | "description"
```

<!--info-footer-start--><br><a href="../../README.ko.md" target="_blank"><img src="https://img.shields.io/badge/-%EB%8F%8C%EC%95%84%EA%B0%80%EA%B8%B0-grey" alt="돌아가기"/></a> <a href="https://tsch.js.org/5/answer/ko" target="_blank"><img src="https://img.shields.io/badge/-%EC%A0%95%EB%8B%B5%20%EA%B3%B5%EC%9C%A0%ED%95%98%EA%B8%B0-teal" alt="정답 공유하기"/></a> <a href="https://tsch.js.org/5/solutions" target="_blank"><img src="https://img.shields.io/badge/-%EC%A0%95%EB%8B%B5%20%EB%B3%B4%EA%B8%B0-de5a77?logo=awesome-lists&logoColor=white" alt="정답 보기"/></a> <!--info-footer-end-->


### 풀이
```ts
type GetReadonlyKeys<
  T,
  U extends Readonly<T> = Readonly<T>,
  K extends keyof T = keyof T
> = K extends keyof T ? Equal<Pick<T, K>, Pick<U, K>> extends true ? K : never : never;
```

1. `GetReadonlyKeys` 타입을 선언한다.
2. T라는 제네릭 인자를 받는다.
3. U라는 인자로 T 제네릭 인자의 타입을 모두 Readonly로 변경한다.
4. K라는 인자로 T의 key값을 모두 받아온다.
5. `K extends keyof T`라는 조건이 맞는지 확인하고 거짓이면 `never`타입을 return한다.
6. `Equal<Pick<T, K>, Pick<U, K>>` T에서 K라는 key를 뽑고, U에서 K라는 인자를 뽑아 비교한다.
7. true로 비교한 값을 narrow한다.
8. 참이라면 K를 return 거짓이라면 never를 return 

### 학습 내용
1. Readonly
- 타입 매핑을 이용하여 T 타입을 받고 그 타입의 모든 속성을 readonly로 지정한다.
```ts
type Foo = {
  bar: number;
  bas: number;
}

type FooReadonly = Readonly<Foo>; 

let foo: Foo = {bar: 123, bas: 456};
let fooReadonly: FooReadonly = {bar: 123, bas: 456};

foo.bar = 456; // 오케이
fooReadonly.bar = 456; // 오류: bar는 readonly임
```
