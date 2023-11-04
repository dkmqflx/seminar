## useLocalStorage

```js
// useStorage.js
import { useCallback, useState, useEffect } from 'react';

// localStorage�� ����� �� ����ϴ� hook
export function useLocalStorage(key, defaultValue) {
  return useStorage(key, defaultValue, window.localStorage);
}

// sessionStorage�� ����� �� ����ϴ� hook

export function useSessionStorage(key, defaultValue) {
  return useStorage(key, defaultValue, window.sessionStorage);
}

function useStorage(key, defaultValue, storageObject) {
  // storageObject�� localStorage or storageObject
  // useState �ȿ� �Լ��� �����ϸ�, �Ϲ� ���� ������ �� ó�� ó�� ������ �� �� ���ѹ��� ����ȴ�
  const [value, setValue] = useState(() => {
    const jsonValue = storageObject.getItem(key); // key�� �ش��ϴ� ���� �޾ƿ´�
    if (jsonValue != null) return JSON.parse(jsonValue);

    if (typeof defaultValue === 'function') {
      return defaultValue(); // defaultValue�� �Լ��� ��쿡�� �Լ� ����
    } else {
      return defaultValue; // item�� ���� ��쿡�� initialValue ��ȯ
    }
  });

  // setValue�� value �����Ű�� �Ʒ� useEffect hook�� ����ȴ�

  useEffect(() => {
    // remove �ؼ� undefined �Ǹ� �ƿ� ������ ��
    if (value === undefined) return storageObject.removeItem(key);
    storageObject.setItem(key, JSON.stringify(value));
  }, [key, value, storageObject]);

  // �ʱ�ȭ�ϴµ� ����ϴ� �Լ�
  const remove = useCallback(() => {
    setValue(undefined);
  }, []);

  return [value, setValue, remove];
}
```

### Usage

```js
import { useSessionStorage, useLocalStorage } from './useStorage';

export default function StorageComponent() {
  const [name, setName, removeName] = useSessionStorage('name', 'Kyle');
  const [age, setAge, removeAge] = useLocalStorage('age', 26);

  return (
    <div>
      <div>
        {name} - {age}
      </div>
      <button onClick={() => setName('John')}>Set Name</button>
      <button onClick={() => setAge(40)}>Set Age</button>
      <button onClick={removeName}>Remove Name</button>
      <button onClick={removeAge}>Remove Age</button>
    </div>
  );
}
```
