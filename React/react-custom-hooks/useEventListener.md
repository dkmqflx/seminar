## useEventListener

- useEventListener�� �̺�Ʈ�� ����ϱ� ���� Custom Hook�̴�

- �Ʒ��� ���� eventType, callbck, �׸��� �̺�Ʈ�� ����� element�� ������ �� �ִ�

```js
// useEventListener.js
import { useEffect, useRef } from 'react';

export default function useEventListener(
  eventType,
  callback,
  element = window
) {
  useEffect(() => {
    if (element == null) return;
    element.addEventListener(eventType, callback);

    return () => element.removeEventListener(eventType, callback);
  }, [eventType, element, callback]);
}
```

- �ٸ� �̷��� dependency�� ��� ����ؼ� �Ѱ����� ���ϰ� �Ǵ� ��� useEffect hook�� �����ϴ� �� ����

- �Ʒ�ó�� callback �Լ��� ���� hook�� �и��� �� �ִ�

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
import useEventListener from './useEventListener';

import { useState } from 'react';
import useEventListener from './useEventListener';

export default function EventListenerComponent() {
  const [key, setKey] = useState('');

  useEventListener('keydown', (e) => {
    setKey(e.key);
  });

  return <div>Last Key: {key}</div>;
}
```
