## useSize

```js
import { useState, useEffect } from 'react';

export default function useSize(ref) {
  const [size, setSize] = useState({});

  useEffect(() => {
    if (ref.current == null) return;

    // ResizeObserver : ����� ũ�� ��ȭ ����
    const observer = new ResizeObserver(([entry]) =>
      setSize(entry.contentRect)
    );
    observer.observe(ref.current);
    return () => observer.disconnect(); // disconnect
  }, []);

  return size;
}
```

### Usage

```js
import { useRef } from 'react';
import useSize from './useSize';

export default function SizeComponent() {
  const ref = useRef();
  const size = useSize(ref);

  return (
    <>
      <div>{JSON.stringify(size)}</div>
      <textarea ref={ref}></textarea>
    </>
  );
}
```
