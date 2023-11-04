## usePrevious

- ���� state ���� �����ϰ� ���� �� ����ϴ� hook

```js
// usePrevious.js
import { useRef } from 'react';

export default function usePrevious(value) {
  const currentRef = useRef(value); // currentRef�� ���� ó�� ������ ���� �ȴ�
  const previousRef = useRef();

  // value�� update�Ǿ� ���ο� value�� ���޵� ��Ȳ
  if (currentRef.current !== value) {
    previousRef.current = currentRef.current; // ���� �� <- ���� ��
    currentRef.current = value; // ���� �� <- ������Ʈ �� ��
  }

  return previousRef.current;
}
```

### Usage

```js
import { useState } from 'react';
import usePrevious from './usePrevious';

export default function PreviousComponent() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('Kyle');
  const previousCount = usePrevious(count); // ����ڰ� ������Ʈ �� ���� ���޵ȴ�

  return (
    <div>
      <div>
        {count} - {previousCount}
      </div>
      <div>{name}</div>
      <button onClick={() => setCount((currentCount) => currentCount + 1)}>
        Increment
      </button>
      <button onClick={() => setName('John')}>Change Name</button>
    </div>
  );
}
```
