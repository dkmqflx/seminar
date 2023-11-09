## Polymorphic 컴포넌트를 사용한 유연한 UI 개발 패턴 소개

- Polymorphic(다형성)이란 다양한 형태로 변형이 되는 것으로,

- 하나의 컴포넌트가 있고 다양한 형태로 변형되서 사용할 수 있게 만들어주는 패턴을 말한다

```tsx
/* Polymorphic Component

Text 컴포넌트 생성

요구사항
1. Text 컴포넌트는 as (prop)을 통해 다양한 태그를 렌더링 할 수 있어야 한다
2. as prop의 기본 값은 span으로 지정한다
3. as prop의 값에 따라 다른 태그를 렌더링 할 수 있어야 한다.

*/

const Text = () => {};

// Text 컴포넌트는 아래처럼 사용할 수 있어야 한다.
export default function Index() {
  return (
    <>
      <Text>Hello</Text>
      <Text as="h1">Title Text</Text>
      <Text as="b">Bold Text</Text>
      <Text as="a" href="google.com">
        Link Text
      </Text>
    </>
  );
}
```

- 위와 같이 사용할 수 있는 Text 컴포넌트를 아래처럼 필요한 props를 전달받는 방식으로 만들어 줄 수 있다.

```tsx
const Text = ({ as, children }) => {
  const Component = as || "span";

  return <Component>{children}</Component>;
};
```

- 하지만 이러한 방법은 굉장히 제한적이고,

- 타입스크립트 환경이기 때문에 타입 지정을 해주어야 하는데 as의 element의 타입이 무엇인지 알지 못한다는 문제가 있다

- any, unknown으로 지정하는 것도 좋지 않고 또한 as를 전달할 때 올바른 html element 인지도 알 수 없다.

- 사용가능한 모든 element를 타입으로 지정할 수도 없는 노릇

- 또한 아래처럼 as로 div를 선달해서 div 태그를 사용한다고 해도 실수로 a 태그의 href 속성을 전달할 수 도 있다

```tsx
<Text as="div" href="google.com">
  Link Text
</Text>
```

- 이러한 문제를 해결하기 위한 방법은 제네릭이 있다.

- Text 컴포넌트의 as로 전달되는 element가 일정하지 않기 때문에 이 부분을 일반화를 시켜주어야 한다

- 어떤 element를 전달하던지 그에 맞게 유연하게 타입처리를 해줄 수 있어야 한다

- 여기서 as는 우리가 제네릭으로 전달되는 T를 통해 타입이 결졍된다

```tsx
type TextProps<T extends React.ElementType> = {
  as?: T;
  children: React.ReactNode;
};

const Text = ({ as, children }) => {
  const Component = as || "span";

  return <Component>{children}</Component>;
};
```

- 중요한 것은 우리가 전달하는 element에 따라서 props로 전달될 수 있는 속성들의 타입을 지정해주어야 한다

- 예를들어 as로 a 태그를 전달하면 href를 prop으로 전달하고 사용할 수 있어야 한다

```tsx
type TextProps<T extends React.ElementType> = {
  as?: T;
  children: React.ReactNode;
} & React.ComponentPropWithoutRef<T>;

const Text = ({ as, children }) => {
  const Component = as || "span";

  return <Component>{children}</Component>;
};
```

- 여기서 추가적으로 해야할 것은 prop들을 전달할 때 유틸리티 타입인 Omit을 사용해서 우리가 정의한 props 타입을 제거해주어야 한다

- 즉, 이렇게 해주면 우리가 지정한 TextProps에서 as와 children는 처리를 해주고 나머지가 `Omit<React.ComponentPropWithoutRef<T>, "as" | "children">` 로 처리되는 것이다

```tsx
type TextProps<T extends React.ElementType> = {
  as?: T;
  children: React.ReactNode;
} & Omit<React.ComponentPropsWithoutRef<T>, "as" | "children">;

const Text = ({ as, children }) => {
  const Component = as || "span";

  return <Component>{children}</Component>;
};
```

- 그리고 마지막으로 Text 컴포넌트로 제네릭을 지정을 아래와 같이 해준다

```tsx
type TextProps<T extends React.ElementType> = {
  as?: T;
  children: React.ReactNode;
} & Omit<React.ComponentPropsWithoutRef<T>, "as" | "children">;

// 여기서 Text 컴포넌트에 제네릭을 사용하는 이유는 TextProps에 제네릭이 사용되고 있기 때문에
// TextProps의 T에 해당하는 타입을 전달 해주어야 하기 때문이다
const Text = <C extends React.ElementType>({ as, children }: TextProps<C>) => {
  const Component = as || "span";

  return <Component>{children}</Component>;
};
```

- 또한 reset parameter를 사용할 수 있기 때문에 아래처럼 코드를 추가해줄 수 있다

```tsx
type TextProps<T extends React.ElementType> = {
  as?: T;
  children: React.ReactNode;
} & Omit<React.ComponentPropsWithoutRef<T>, "as" | "children">;

const Text = <C extends React.ElementType>({
  as,
  children,
  ...rest
}: TextProps<C>) => {
  const Component = as || "span";

  return <Component {...rest}>{children}</Component>;
};
```

- 이렇게 작성된 코드를 아래처럼 사용할 수 있다

```ts
type TextProps<T extends React.ElementType> = {
  as?: T;
  children: React.ReactNode;
} & Omit<React.ComponentPropsWithoutRef<T>, "as" | "children">;

const Text = <C extends React.ElementType>({
  as,
  children,
  ...rest
}: TextProps<C>) => {
  const Component = as || "span";

  return <Component {...rest}>{children}</Component>;
};

export default function Index() {
  return (
    <>
      <Text>Span TExt</Text>
      <Text as="h1">h1 Text</Text>
      <Text as="div">div Text</Text>
      <Text as="a" href="https://google.com">
        This is anchor Text
      </Text>
    </>
  );
}
```

---

## Reference

- [Polymorphic 컴포넌트를 사용한 유연한 UI 개발 패턴 소개](https://www.youtube.com/watch?v=BUlvBT8lhFc&list=PL3xNAKVIm80LIjR0lOHauH6ZRkfCXbsyW&index=6)
