## useUpdataEffect

- useEffect hook�� ó������ ������� �ʰ�

- dep�� ��������� ������ �� ����ǵ��� �ϴ� hook

```js
// useUpdateEffect.js
import { useEffect, useRef } from 'react';

export default function useUpdateEffect(callback, dependencies) {
  const firstRenderRef = useRef(true);

  useEffect(() => {
    // ó�� mount �� ���� true �̹Ƿ� �Ʒ� ���ǹ��� ����Ǿ� �׳� return �ǰ�
    // callback �Լ��� ������� �ʴ´�
    if (firstRenderRef.current) {
      firstRenderRef.current = false;
      return;
    }
    return callback();
  }, dependencies);
}
```

### Usage

```js
import { useState } from 'react';
import useUpdateEffect from './useUpdateEffect';

export default function UpdateEffectComponent() {
  const [count, setCount] = useState(10);
  useUpdateEffect(() => alert(count), [count]);

  return (
    <div>
      <div>{count}</div>
      <button onClick={() => setCount((c) => c + 1)}>Increment</button>
    </div>
  );
}
```
