## 타입스크립트 타입선언 vs 타입 표명

- DOM의 특정 property에 접근하려고 할 때 해당하는 property가 없다는 에러가 나타나는 경우가 있다

```ts
const el = document.getElementById("email");
el.value; // (method) Document.getElementById(elementId: string): HTMLElement | null

// property 'value' does not exist on type 'HTMLElement'.ts(2339)
```

- 그 이유는 getElementById의 return 타입을 보면 이해할 수 있는데, getElementById의 return 타입 아래와 같이` HTMLElement | null` 이고 HTMLElement에는 vlalue라는 속성이 없기 때문에 에러가 발생하는 것이다

- 이러한 경우에 **타입 표명 (Type Assertion)**으로 해결할 수 있다

```ts
const el = document.getElementById("email") as HTMLInputElement;

const el = <HTMLInputElement>document.getElementById("email");

el.value;
```

- 일반적으로 DOM에 접근할 때 많이 사용하는 `querySelector`를 사용할 때도 이러한 에러를 마주치게 된다

```ts
const el2 = document.querySelector(".email");
el2?.value;

// (method) ParentNode.querySelector<Element>(selectors: string): Element | null (+4 overloads)
```

- querySelector 같은 경우에는 위 에러에서 확인할 수 있듯이 제네릭을 사용할 수 있는데 querySelector의 타입을 보면 다음과 같다

```ts

querySelector<E extends Element = Element>(selectors: string): E | null;

```

- 따라서 제네릭으로 return 타입을 지정해주면 타입 표명 없이 코드를 작성할 수 있다.

```ts
const el2 = document.querySelector<HTMLInputElement>(".email");
el2?.value;
```

--

## Reference

- [타입스크립트 타입선언 vs 타입 표명](https://www.youtube.com/watch?v=AG_hB-0ozIE)
