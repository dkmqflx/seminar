## [HTML element 확장과 쉽게 props 타입 지정 하는 방법 설명 | 리액트+타입스크립트](https://www.youtube.com/watch?v=KlH4p9FbKPg) <a id="타입스크립트-element-props"></a>

- 아래는 button elment를 반환하는 Button 컴포넌트가 있는 코드이다

- Button 컴포넌트 같은 경우에는 확장해서 사용할 수 있는데

- html button 요소에 prop을 전달하거나 상황에 따라 custom prop을 전달해서 많이 사용한다

```tsx
import React from "react";

export const Button = () => {
  return <button></button>;
};

export default function Index() {
  return (
    <div>
      <Button>OK</Button>
    </div>
  );
}
```

- 가장 흔하게 사용되는 prop이 전달되는 아래의 경우에는 props 맞는 타입을 직접 지정해주는 방법이 있다

```tsx
import React from "react";

type ButtonProps = {
  onClick: () => void;
  className: string;
  type: "submit" | "reset" | "button";
};

export const Button = ({ onClick, className, type }: ButtonProps) => {
  return <button onClick={onClick} className={className} type={type}></button>;
};

export default function Index() {
  return (
    <div>
      <Button
        onClick={() => alert("test")}
        className="btn-default"
        type="submit"
      >
        OK
      </Button>
    </div>
  );
}
```

- 하지만 이렇게 코드를 작성할 필요 없이 리액트에서 기본 타입을 제공해주기 때문에 아래처럼 제공되는 유틸리티 타입을 사용하면 된다

```tsx
import React from "react";

type ButtonProps = React.ComponentPropsWithoutRef<"button">;

export const Button = (props: ButtonProps) => {
  const { onClick, className, type } = props;

  return <button></button>;
};

export default function Index() {
  return (
    <div>
      <Button
        onClick={() => alert("test")}
        className="btn-default"
        type="submit"
      >
        OK
      </Button>
    </div>
  );
}
```

- 다만 이렇게 props를 destructuring하는 경우에는 필요한 모든 property를 가져와야 하는 단점이 있기 때문에

- 아래처럼 rest parameter를 사용해준다

- 이렇게 해서 간편하게 props의 타입을 지정해 줄 수 있고 코드도 훨씬 더 깔끔해졌다

```tsx
import React from "react";

type ButtonProps = React.ComponentPropsWithoutRef<"button">;

export const Button = (props: ButtonProps) => {
  const { children, ...rest } = props;

  return <button {...rest}>{children}</button>;
};

export default function Index() {
  return (
    <div>
      <Button
        onClick={() => alert("test")}
        className="btn-default"
        type="submit"
      >
        OK
      </Button>
    </div>
  );
}
```

- 추가적으로 custom prop을 사용하고 싶은 경우가 있을 수 있다

- 아래처럼 useAnimation이라는 props를 전달한다고 하자

- 그러면 간단하게 &를 사용해서 intersection 타입으로 만들어주면 된다

```tsx
import React from "react";

type ButtonProps = React.ComponentPropsWithoutRef<"button"> & {
  useAnimation?: boolean;
};

export const Button = (props: ButtonProps) => {
  const { children, useAnimation, ...rest } = props;

  if (useAnimation) {
    // 필요한 로직 작성
  }
  return <button {...rest}>{children}</button>;
};

export default function Index() {
  return (
    <div>
      <Button
        useAnimation={true}
        onClick={() => alert("test")}
        className="btn-default"
        type="submit"
      >
        OK
      </Button>
    </div>
  );
}
```

---

## Reference

- [HTML element 확장과 쉽게 props 타입 지정 하는 방법 설명 | 리액트+타입스크립트](https://www.youtube.com/watch?v=KlH4p9FbKPg)
