## useDeepCompareEffect

- ��ü�� ����� ��, ��ü�� ����Ǵ� ��� deep compare�� ���ؼ� ���� update ���� �����ϴ� Hook

```js
import { useEffect, useRef } from 'react';
import isEqual from 'lodash/fp/isEqual'; // loadash ���̺귯�� ���

export default function useDeepCompareEffect(callback, dependencies) {
  const currentDependenciesRef = useRef();

  // dep����Ǵ��� currentDependenciesRef�� ����� ���� ���� ���ϰ�, ���� ���� ��� �Ʒ� ����
  if (!isEqual(currentDependenciesRef.current, dependencies)) {
    currentDependenciesRef.current = dependencies; // ���� ������ ����� ���� �����Ѵ�
  }

  // ���� �� ������ ������� �ʴ´�
  useEffect(callback, [currentDependenciesRef.current]);
}
```

### Usage

```js
import { useEffect, useState, useRef } from 'react';
import useDeepCompareEffect from './useDeepCompareEffect';

export default function DeepCompareEffectComponent() {
  const [age, setAge] = useState(0);
  const [otherCount, setOtherCount] = useState(0);
  const useEffectCountRef = useRef();
  const useDeepCompareEffectCountRef = useRef();

  const person = { age: age, name: 'Kyle' }; // state�� ����� �� ���� ������ �ȴ�

  useEffect(() => {
    // hook�� Increment Age�� Ŭ���� ��
    // Increment Other Count ��ư�� Ŭ���� ��
    // ��� useEffectCountRef ���� ����ȴ�
    useEffectCountRef.current.textContent =
      parseInt(useEffectCountRef.current.textContent) + 1;
  }, [person]);

  useDeepCompareEffect(() => {
    // hook�� Increment Age�� Ŭ���� ��
    // useDeepCompareEffectCountRef ���� ���������
    // Increment Other Count ��ư�� Ŭ���� ����
    // useDeepCompareEffectCountRef ���� ������� �ʴ´�

    useDeepCompareEffectCountRef.current.textContent =
      parseInt(useDeepCompareEffectCountRef.current.textContent) + 1;
  }, [person]);

  return (
    <div>
      <div>
        useEffect: <span ref={useEffectCountRef}>0</span>
      </div>
      <div>
        useDeepCompareEffect: <span ref={useDeepCompareEffectCountRef}>0</span>
      </div>
      <div>Other Count: {otherCount}</div>
      <div>{JSON.stringify(person)}</div>
      <button onClick={() => setAge((currentAge) => currentAge + 1)}>
        Increment Age
      </button>
      <button onClick={() => setOtherCount((count) => count + 1)}>
        Increment Other Count
      </button>
    </div>
  );
}
```
