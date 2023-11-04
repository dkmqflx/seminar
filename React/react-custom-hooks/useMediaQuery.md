## useMediaQuery

- media query�� ����� �� ����� �� �ִ� custom hook����

- ������ ������ useEventListener hook�� ����ؼ� ������ �� �ִ�

```js
// useEventListener.js
import { useEffect, useRef } from 'react';

export default function useEventListener(
  eventType,
  callback,
  element = window // mediaQuery ��ü�� ���޵ȴ�
) {
  const callbackRef = useRef(callback);

  useEffect(() => {
    callbackRef.current = callback;
  }, [callback]);

  useEffect(() => {
    if (element == null) return;
    const handler = (e) => callbackRef.current(e);
    element.addEventListener(eventType, handler);
    // ���޹��� media query ��ü�� �̺�Ʈ�� ����Ѵ�
    // �Ʒ� ���������� '(min-width: 200px)'�� ���މ����Ƿ�
    // width �� 200px�� ���� ��  ����� �̺�Ʈ�� ����ȴ�

    return () => element.removeEventListener(eventType, handler);
  }, [eventType, element]);
}
```

```js
// useMediaQuery
import { useState, useEffect } from 'react';
import useEventListener from './useEventListener';

// ����ϰ� ���� media query�� �����Ѵ�
export default function useMediaQuery(mediaQuery) {
  const [isMatch, setIsMatch] = useState(false);
  const [mediaQueryList, setMediaQueryList] = useState(null);

  useEffect(() => {
    const list = window.matchMedia(mediaQuery); // media query ��ü�� ��ȯ�Ѵ�
    /*
     * media�� ����� �̵������ ���ڿ��� ��ȯ�ϰ�,
     * matches�� ���� ȭ���� �̵�������� ������ ���� true, �ƴϸ� false�� ��ȯ���ش�.
     */
    setMediaQueryList(list);
    setIsMatch(list.matches); // true or false
  }, [mediaQuery]);

  useEventListener('change', (e) => setIsMatch(e.matches), mediaQueryList);

  return isMatch;
  // media query�� ���޵� '(min-width: 200px)'�� ���� �ּ� 200 �̻��� ��쿡�� true�� ��ȯ�ȴ�
  // ��  media query�� ������ �����ϴ� ��쿡�� true�� ��ȯ�ȴ�
}
```

### Usage

```js
import useMediaQuery from './useMediaQuery';

export default function MediaQueryComponent() {
  const isLarge = useMediaQuery('(min-width: 200px)'); // true or false ���� ��ȯ�ȴ�

  return <div>Large: {isLarge.toString()}</div>;
}
```
