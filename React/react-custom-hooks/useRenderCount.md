## useRenderCount

- ������Ʈ�� ��� �������Ǿ����� �� �� �ִ� hook

```js
import { useEffect, useRef } from 'react';

export default function useRenderCount() {
  const count = useRef(1);

  // ������ �� �� ���� ����ȴ�
  useEffect(() => count.current++);

  // ���� useEffect �ۿ� count.current++ �̷������� �ۼ��ϸ�
  // 2�� renderCount�� �����µ� �� ������ StrictMode���� ����Ʈ�� double render �ϱ� ����
  // ������ UseEffect hook�� double render ���� �ʱ� ������ useEffect hook�� ����Ѵ�
  return count.current;
}
```

### Usage

```js
import useRenderCount from './useRenderCount';
import useToggle from './useToggle'; // rendering Ƚ�� �����Ű�� ���� ����ϴ� hook

export default function RenderCountComponent() {
  const [boolean, toggle] = useToggle(false);

  const renderCount = useRenderCount();

  return (
    <>
      <div>{boolean.toString()}</div>
      <div>{renderCount}</div>
      <button onClick={toggle}>Toggle</button>
    </>
  );
}
```
