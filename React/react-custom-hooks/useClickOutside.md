## useClickOutside

- Modal�� ���� Ư�� ���� ���� Ŭ������ �� ó���ϱ� ���� hook�̴�

- ������ useEventListener hook�� ����Ѵ�

```js
import useEventListener from './useEventListener';

export default function useClickOutside(ref, cb) {
  useEventListener(
    'click',
    (e) => {
      if (ref.current == null || ref.current.contains(e.target)) return; // ref�� ��� Ŭ���ϸ� return
      cb(e);
    },
    document
  );
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
import { useRef, useState } from 'react';
import useClickOutside from './useClickOutside';

export default function ClickOutsideComponent() {
  const [open, setOpen] = useState(false);
  const modalRef = useRef();

  useClickOutside(modalRef, () => {
    if (open) setOpen(false);
  });

  return (
    <>
      <button onClick={() => setOpen(true)}>Open</button>
      <div
        ref={modalRef}
        style={{
          display: open ? 'block' : 'none',
          backgroundColor: 'blue',
          color: 'white',
          width: '100px',
          height: '100px',
          position: 'absolute',
          top: 'calc(50% - 50px)',
          left: 'calc(50% - 50px)',
        }}
      >
        <span>Modal</span>
      </div>
    </>
  );
}
```
