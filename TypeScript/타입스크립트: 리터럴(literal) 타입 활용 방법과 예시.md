## 타입스크립트: 리터럴(literal) 타입 활용 방법과 예시

- 리터럴 타입이란 값 자체를 타입으로 사용하는 것으로

- 리터럴 타입의 `const assertion (as const)` 문법은 리터럴 타입을 사용할 수 있는 방법 중 하나이다

- 리터럴 타입을 사용하는 가장 간단한 예제는 다음과 같다

```ts
type ImageType = "png";

const imageType: ImageType = "png";

const imageTyp2: ImageType = "jpg"; // 에러 발생
```

- 유니언(union) 타입을 지정할 때도 리터럴 타입이 많이 사용된다

```ts
type Direction = "left" | "right" | "up" | "down";

function naviage(dir: Direction) {
  console.log(dir);
}

naviage("left");

naviage("back"); // 에러 발생
```

- UI 작업할 때 스타일 타입을 지정할 때도 유용하게 사용할 수 있다

- 아래는 버튼의 스타일의 타입을 정의하는 케이스이다

```ts
type btnType = "btn-default" | "btn-error" | "btn-success"; // 타입

type btnSize = "sm" | "md" | "lg"; // 사이즈

type btnColor = "red" | "blue" | "green"; // 색상

// 3 * 3 * 3 총 27가지 조합이 전달될 수 있다

// 템플릿 스트링 활용해서 만든다

// 하나의 타입으로 다양한 조합을 전달할 수 있게 된다
```

- 3가지 타입이 있으몰 `3 * 3 * 3` 총 27가지의 조합이 전달될 수 있다

- 이러한 경우 템플릿 리터럴을 활용해서 아래처럼 리터럴 타입을 지정하게 되면 하나의 타입으로 다양한 조합을 전달할 수 있게 된다

```ts
type buttonStyle = `${btnType}-${btnSize}-${btnColor}`;
```

- css, className 또는 스타일을 조합해서 다른 변수나 함수에 전달할 때 유용하게 사용할 수 있다

- 예를들어 아래와 같이 class를 추가하는 함수가 있을 때 위에서 정의한 스타일 타입을 사용할 수 있다

```ts
const addStyle = (el: HTMLElement, style: buttonStyle) => {
  el?.classList.add(style);
};
```

- 리터럴 타입에 `as const`를 사용할 수 도 있다

- 아래와 같은 객체가 있다고 하자

```ts
const languageCode = {
  en: "English",
  es: "Spanish",
  fr: "French",
  kr: "Korean",
};
```

- 객체에는 타입 적용이 안 되어있기 때문에 타입을 보면 아래와 같다

```ts
const languageCode = {
  en: "string",
  es: "string",
  fr: "string",
  kr: "string",
};
```

- const나 let일 때 모두 객체의 속성은 변경할 수 있기에 타입스크립트가 string으로 추론한 것이다.

- 아래처럼 객체의 property의 값을 string으로 변경할 수 있다

```ts
languageCode.en = "ENGLISH";
```

- 하지만 `as const` 를 사용하게 되면 리터럴 타입이 되고

```ts
const languageCode = {
  en: "English",
  es: "Spanish",
  fr: "French",
  kr: "Korean",
} as const;
```

- 타입을 확인해보면 아래와 같이 되어 있는 것을 확인할 수 있다

```ts
const languageCode: {
  readonly en: "English";
  readonly es: "Spanish";
  readonly fr: "French";
  readonly kr: "Korean";
};
```

- readonly 속성 에다가 값 자체가 타입이 되기 때문에 아래처럼 객체의 property를 수정할 수 없게 된다

```ts
languageCode.en = "ENGLISH"; // 에러 발생
```

---

## Reference

- [타입스크립트: 리터럴(literal) 타입 활용 방법과 예시](https://www.youtube.com/watch?v=BQTB9KWYb6A&t=18s)
