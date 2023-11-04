## useStateWithHistory

- ������ �߰��ϰ� ���� data history�� ���ؼ� �����Ϳ� �����ϰų�

- undo, redo �� ����� �ʿ��� �� ����� �� �ִ� Custom hook

```js
import { useCallback, useRef, useState } from 'react';

export default function useStateWithHistory(
  defaultValue,
  { capacity = 10 } = {}
) {
  const [value, setValue] = useState(defaultValue);
  const historyRef = useRef([value]);
  const pointerRef = useRef(0); // pointer ������ �Ѵ�

  const set = useCallback(
    (v) => {
      const resolvedValue = typeof v === 'function' ? v(value) : v;

      // �����߰��� ���� ������ pointer�� ����Ű�� �Ͱ� ���� ���� ���
      if (historyRef.current[pointerRef.current] !== resolvedValue) {
        // ���� ������ ���Ŀ� ��� history ���� �Ѵ�
        // ex, �⺻ capacity�� 10�ε�, 10�� �̻��� �����Ͱ� �߰��� ��쿡��
        // pointer �������� ������ �� �ֵ��� history�� �߶��ش�
        if (pointerRef.current < historyRef.current.length - 1) {
          historyRef.current.splice(pointerRef.current + 1);
        }
        // ���� �߰��� �� history�� �߰�
        historyRef.current.push(resolvedValue);

        // capacity �̻� �߰� �� ���
        // �տ� �ִ� ������ ���ش�
        while (historyRef.current.length > capacity) {
          historyRef.current.shift();
        }
        // history�� �������� pinter�� ����Ű���� �Ѵ�
        pointerRef.current = historyRef.current.length - 1;
      }
      setValue(resolvedValue);
    },
    [capacity, value]
  );

  // pointer �ڷ� �̵��ϴ� �Լ�
  const back = useCallback(() => {
    if (pointerRef.current <= 0) return;
    pointerRef.current--;
    setValue(historyRef.current[pointerRef.current]);
  }, []);

  // pointer ������ �̵��ϴ� �Լ�
  const forward = useCallback(() => {
    if (pointerRef.current >= historyRef.current.length - 1) return;
    pointerRef.current++;
    setValue(historyRef.current[pointerRef.current]);
  }, []);

  // Ư���� index�� �̵��ϴ� �Լ�
  const go = useCallback((index) => {
    if (index < 0 || index > historyRef.current.length - 1) return;
    pointerRef.current = index;
    setValue(historyRef.current[pointerRef.current]);
  }, []);

  return [
    value,
    set,
    {
      history: historyRef.current,
      pointer: pointerRef.current,
      back,
      forward,
      go,
    },
  ];
}
```

### Usage

```js
import { useState } from 'react';
import useStateWithHistory from './useStateWithHistory';

export default function StateWithHistoryComponent() {
  const [count, setCount, { history, pointer, back, forward, go }] =
    useStateWithHistory(1);
  const [name, setName] = useState('Kyle');

  return (
    <div>
      {/* count�� ���� pointer�� ����Ű�� �� */}
      <div>{count}</div>
      <div>{history.join(', ')}</div>
      <div>Pointer - {pointer}</div>
      <div>{name}</div>
      <button onClick={() => setCount((currentCount) => currentCount * 2)}>
        Double
      </button>
      <button onClick={() => setCount((currentCount) => currentCount + 1)}>
        Increment
      </button>
      <button onClick={back}>Back</button>
      <button onClick={forward}>Forward</button>
      <button onClick={() => go(2)}>Go To Index 2</button>
      <button onClick={() => setName('John')}>Change Name</button>
    </div>
  );
}
```
