## useHover

- hover �Ǿ����� üũ�� �� ����ϴ� hook

- ���ǵǾ� �ִ� useEventListener hook�� ����Ѵ�

```js
import { useState } from 'react';
import useEventListener from './useEventListener';

export default function useHover(ref) {
  const [hovered, setHovered] = useState(false);

  useEventListener('mouseover', () => setHovered(true), ref.current);
  useEventListener('mouseout', () => setHovered(false), ref.current);

  return hovered;
}
```

```js
// useEventListener.js

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

### Usage

```js
import { useRef } from 'react';
import useHover from './useHover';

export default function HoverComponent() {
  const elementRef = useRef();
  const hovered = useHover(elementRef); // true, false

  return (
    <div
      ref={elementRef}
      style={{
        backgroundColor: hovered ? 'blue' : 'red',
        width: '100px',
        height: '100px',
        position: 'absolute',
        top: 'calc(50% - 50px)',
        left: 'calc(50% - 50px)',
      }}
    />
  );
}
```
