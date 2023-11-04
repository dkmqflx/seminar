## UseTimeout

- setTimeout�� ����ϱ� ���� hook���� ������ delay ���Ŀ� callback �Լ��� ����ǵ��� �Ѵ�

```js
import { useCallback, useEffect, useRef } from 'react';

export default function useTimeout(callback, delay) {
  const callbackRef = useRef(callback); // callback �Լ� ���
  const timeoutRef = useRef();

  // callback �Լ��� ����� �� ���� ����Ǵ� hook
  useEffect(() => {
    callbackRef.current = callback;
  }, [callback]);

  // �Ʒ� �Լ����� useCallback���� ������ ������
  // useTimeout�� ����ϴ� �ܺ� ������Ʈ���� �ٽ� ������ �� �� ����
  // callback �Լ��� �ݺ��ؼ� ���޵Ǳ� ������, ���ʿ��� �������� ���� ���ؼ� useCallback�� ����ߴ�.

  // timeout set
  const set = useCallback(() => {
    // delay �Ŀ� ����ȴ�
    timeoutRef.current = setTimeout(() => callbackRef.current(), delay);
  }, [delay]);

  // timeout clear
  // reset �����ϴ���, clear �Լ������ؼ� timeout�� ���ָ�, reset�� ������� �ʴ´�
  const clear = useCallback(() => {
    timeoutRef.current && clearTimeout(timeoutRef.current);
  }, []);

  useEffect(() => {
    set();
    // set �Լ� �����ϸ�, setTimeout�� ����� �ݹ� �Լ� ����ȴ�
    return clear;
    // unmount��, ����� setTimeout clear ���ش�.
  }, [delay, set, clear]);

  // �ٽ� timeout reset �ϴ� �Լ�
  const reset = useCallback(() => {
    clear(); // timeout clear
    set(); // �ٽ�
  }, [clear, set]);

  return { reset, clear };
}
```

### Usage

```js
import { useState } from 'react';
import useTimeout from './useTimeout';

export default function TimeoutComponent() {
  const [count, setCount] = useState(10);
  const { clear, reset } = useTimeout(() => setCount(0), 1000);

  return (
    <div>
      <div>{count}</div>
      <button onClick={() => setCount((c) => c + 1)}>Increment</button>
      <button onClick={clear}>Clear Timeout</button>
      <button onClick={reset}>Reset Timeout</button>
    </div>
  );
}
```
