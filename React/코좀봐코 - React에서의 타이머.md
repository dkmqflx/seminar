- 리액트에서 타이머를 구현할 때 아래와 같이 setInterval을 사용해서 코드를 작성하면

- state가 변경되어 렌더링 될 때 마다 setInterval 카운터가 생성되는 문제점이 발생한다

- setCount를 통해서 1초마다 count를 1씩 증가하도록 했기 때문에, 1초 후 Counter 컴포넌트가 렌더링 되고

- 이 때 1초마다 count 1을 증가 시키는 새로운 setInterval 카운터가 만들어지게 된다

- 즉, 렌더링 될 때마다 새로운 setInterval 카운터가 만들어지는 상황이 반복되는 문제가 생긴다.

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  setInterval(() => {
    setCount(count + 1);
  }, 1000);

  return <h1>{count}</h1>;
}
```

- 그리고 코드를 실행시켜 보면 어느 순간 숫자가 오르락 내리락 하는 방식으로 화면에 count가 출력되는데

- 이는 각각의 렌더링 때 마다 다른 setInterval 카운터가 있기 때문이다.

- 첫번째 렌더링에서 실행되는 setInterval 카운터에 있는 setCount(count +1)는 count를 0에서 1로만 증가시키는 역할을 한다

- 그 이유는 클로저에 의해 count가 0이기 때문이다.

- 두번째 렌더링에서 새롭게 실행되는 setInterval 카운터에서 setCount(count +1)는 count값을 1에서 2로만 증가시키는 역할을 한다

- 이러한 방식으로 세번째, 네번째 렌더링이 될 때마다 setInterval 카운터가 생성되고,

- 각각의 카운터는 클로저로 인해 다른 count 값을 갖고 state값인 count를 증가시킨다

- 이러한 이유 때문에 state값인 count가 변경 될 때 마다 렌더링이 되지만, 다음번에 실행되는 setIinterval 카운터가
  
- count를 0에 1로 변경시킬 수 있는 카운터일 수도 아니면 2에서 3으로 변경시키는 카운터 일 수 있기 때문에 숫자가 제멋대로 출력되는 것이다

<br/>

- 이러한 문제는 아래와 같이 useEffect를 사용해서 해결할 수 있다

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(count + 1);
    }, 1000);

    return () => clearInterval(id);
  });

  return <h1>{count}</h1>;
}
```

- useEffect에서 count 값이 증가하기 때문에 값이 증가할 때 마다 렌더링이 일어난다

- 이 때 생성한 카운터를 clearInterval을 사용해서 clear 해주기 때문에 이전의 카운터는 없어지게 되고

- 새로운 렌더링으로 인해 만들어진 새로운 카운터에서 count 값을 증가시켜주기 때문에 정상적으로 카운터가 작동하는 것이다

- 다만 위 코드는 계속해서 렌더링 될 때 마다 카운터를 만들고 없애준다는 단점있는데

- 아래처럼 useEffect의 dependency에 빈 배열을 넣어서 렌더링이 된 이후 한번만 실행되도록 한 다음, useState의 함수형 업데이트를 통해서 해결할 수 있다.

```jsx
import { useState, useEffect } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount((count) => count + 1);
    }, 1000);

    return () => clearInterval(id);
  }, []);

  return <h1>{count}</h1>;
}
```

<br/>

- 위 코드에서는 state를 사용해서 카운터를 만들었지만 외부에서 Prop으로 전달되는 값을 사옹해서 카운터를 만들어야할 경우도 있다

- 이 때는 아래처럼 dan Dan Abramov의 useInterval 훅을 사용해서 해결할 수 있다.

```jsx
import { useState, useEffect, useRef } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  useInterval(() => {
    setCount(count + 1);
  }, 1000);

  return <h1>{count}</h1>;
}

export function useInterval(callback, delay) {
  const savedCallback = useRef();

  // Remember the latest callback.
  useEffect(() => {
    savedCallback.current = callback; // useRef에 등록되었기 때문에 새로운 값이 할당되더라도 새롭게 렌더링 되지 않는다
  }, [callback]);

  // Set up the interval.
  // 딱 한번만 실행된다
  useEffect(() => {
    function tick() {
      savedCallback.current();
    }
    if (delay !== null) {
      let id = setInterval(tick, delay);
      return () => clearInterval(id);
    }
  }, [delay]);
}
```

- 위의 코드처럼 useInterval을 사용하는 경우에는 클로저를 신경쓸 필요가 없다.

- 실행될 때 마다 받은 콜백을 별도로 저장해 놓고, tick이라는 함수에서 클로저로 받은 콜백을 사용하기 때문이다.

- 즉 useRef로 저장한 콜백함수를 setInterval, clearInterval로 처리하기 때문에 렌더링 할 때 마다 카운터가 새롭게 만들어지지 않기 때문에 Counter 컴포넌트에서는 렌더링 할 때의 시점에 관련된 코드만 작성하면 된다

- 아래 코드는 다음과 같이 동작한다

1. useInterval에 콜백함수에 전달된 setCount(count + 1)가 savedCallback.current에 등록되고 setInterval에 의해 실행된다.
2. setCount(count + 1)가 실행되면서 Counter 컴포넌트는 다시 렌더링되고, 새로운 클로저를 갖는 setCount(count + 1)이 useInterval로 전달된다.
3. 이 때 callback을 dependency로 갖는 useEffect가 다시 실행되면서 savedCallback.current에는 새로운 클로저를 갖는 setCount(count + 1)이 할당된다
4. useRef에 함수를 할당하기 때문에 새로운 값이 할당되더라도 다시 렌더링 되지 않고, setInterval을 실행하는 useEffect도 다시 실행되지 않는다
5. 즉, setInterval은 처음 한번만 tick 함수는 delay 마다 변경되기 때문에 새로운 tick 함수를 delay 마다 실행하는 것이다
  
```js
import React, { useState, useEffect, useRef } from 'react';

function Counter() {
  let [count, setCount] = useState(0);

  useInterval(() => {
    // Your custom logic here
    setCount(count + 1);
  }, 1000);

  return <h1>{count}</h1>;
}

```

- 이렇게 useInterval hook을 사용해서 선언적으로 코드를 작성할 수 있다.


## Reference

- [코좀봐코 - React에서의 타이머 part 1 : setInterval 말고 이것!](https://www.youtube.com/watch?v=2tUdyY5uBSw&t=28)
- [코좀봐코 - React에서의 타이머 part 2 : useIntervalValue Hook 만들어서 카운트다운 쉽게 구현하기](https://www.youtube.com/watch?v=zBmTqF0GpN4)
- [React에서 카운트 다운 타이머 구현](https://class.codejong.kr/t/react/371)
- [Making setInterval Declarative with React Hooks](https://overreacted.io/making-setinterval-declarative-with-react-hooks/)
- [번역 / 리액트 훅스 컴포넌트에서 setInterval 사용 시의 문제점](https://velog.io/@jakeseo_me/%EB%B2%88%EC%97%AD-%EB%A6%AC%EC%95%A1%ED%8A%B8-%ED%9B%85%EC%8A%A4-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EC%97%90%EC%84%9C-setInterval-%EC%82%AC%EC%9A%A9-%EC%8B%9C%EC%9D%98-%EB%AC%B8%EC%A0%9C%EC%A0%90)
- [React 에서 useEffect의 return 호출 조건이 이해가 안됩니다!](https://jsdev.kr/t/react-useeffect-return/5676)
