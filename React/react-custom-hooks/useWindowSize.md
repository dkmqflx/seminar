## useWindowSize

- window�� size�� ���ϴ� hook���� ������ ������ useEventListner�� �Բ� ����ؼ� ������ �� �ִ�

```js
import { useEffect, useRef } from 'react';

export default function useEventListener(
  eventType,
  callback,
  element = window
) {
  const callbackRef = useRef(callback);

  useEffect(() => {
    callbackRef.current = callback;
  }, [callback]);

  useEffect(() => {
    if (element == null) return;
    const handler = (e) => callbackRef.current(e);
    element.addEventListener(eventType, handler);

    return () => element.removeEventListener(eventType, handler);
  }, [eventType, element]);
}
```

```js
import { useState } from 'react';
import useEventListener from './useEventListener';

export default function useWindowSize() {
  const [windowSize, setWindowSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight,
  });

  useEventListener('resize', () => {
    setWindowSize({ width: window.innerWidth, height: window.innerHeight });
  });

  return windowSize;
}
```

### Usage

```js
import useWindowSize from './useWindowSize';

export default function WindowSizeComponent() {
  const { width, height } = useWindowSize();

  return (
    <div>
      {width} x {height}
    </div>
  );
}
```
