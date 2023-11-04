## useDebounce 1

- useDebounde hook�� debounce�� ������ Hook����

- ª�� �ð� ���� ���ӵ� �Լ��� event ȣ�� ��, ȣ��Ǵ� ��� �Լ��� ��� ó������ �ʰ�

- ������ ������ ���Ŀ� �ѹ��� ó���ϴ� �����̴�

- Use Case

  - �̻��� (input) �Է°� ���ÿ� ��Ʈ��ũ ��û�� ���� �̻� ����� �����;� �� ��

  - ��ũ�� / resize �̺�Ʈ �������� �۾��ؾ� �� ��

  - ���� ���� �ڵ� ����

- ����

  - �������� ���� ���

  - ��� ���� (API / ���� ���)

```js
// useDebounce.js
import { useEffect, useState } from 'react';

function useDebounce(value, delay = 500) {
  const [debounceVal, setDebounceVal] = useState(value);

  // ����ڰ� ��� �Է��� �� ���� ���ο� value�� ���޵Ǿ�
  // �Ʒ� hook�� ����ؼ� ���ο� handler�� ����ϰ� �ȴ�
  // �׸��� ������ �Է��� ���� �� delay�� ������ ���ο� debounceVal�� set �Ѵ�
  useEffect(() => {
    const handler = setTimeout(() => {
      setDebounceVal(value);
    }, delay);

    // umount �� clear
    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);

  return debounceVal;
  // delay�� ���� ������ ����� ���� return �ȴ�
}

export default useDebounce;
```

### Usage 1

```js
import { useEffect, useState } from 'react';
import './App.css';

import useDebounce from './useDebounce';

function CountryList({ countries }) {
  if (!countries) return;
  return countries.map((country) => {
    return (
      <div key={`${country.area}`}>
        <span>{country.name.official}</span>{' '}
        <img
          style={{ width: '120px', height: '80px' }}
          src={country.flags.png}
          alt={country.name.common}
        />
      </div>
    );
  });
}

export default function App() {
  const [search, setSearch] = useState('');
  const [countries, setCountries] = useState(null);

  // ����ڰ� ���� �Է��� �� ���� onChange �Լ��� ����ǰ�
  // ����� search ���� ���޵ȴ�.
  // ������ �Է��� ���� ���� delay �Ŀ� update�� ���� ��ȯ�ȴ�
  const debounceValue = useDebounce(search);

  // debounceValue �� search�� ����� �� ���� �ٲ�� ���� �ƴ϶�
  // ������ �Է��� ���� ���� delay �Ŀ� update�� ���̱� ������,
  // ������ �Է��� ������ �Ǹ� �Ʒ� useEffect hook�� ����ȴ�
  useEffect(() => {
    const getCountries = async () => {
      return await fetch(`https://restcountries.com/v3.1/name/${debounceValue}`)
        .then((res) => {
          if (!res.ok) {
            return new Promise.reject('no country found');
          }
          return res.json();
        })
        .then((list) => {
          setCountries(list);
        })
        .catch((err) => console.error(err));
    };
    if (debounceValue) getCountries();
  }, [debounceValue]);

  return (
    <div className='App'>
      <input
        type='search'
        placeholder='Search Countries'
        onChange={(e) => setSearch(e.target.value)}
      />
      <hr />
      {search ? <CountryList countries={countries} /> : ''}
    </div>
  );
}
```

## useDebounde 2

- Custom hook���� ������ useTimeout�� ����ؼ� useDebounce�� ������ ���� �ִ�.

```js
// useTimeout.js

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

```js
// useDebounce.js
import { useEffect } from 'react';
import useTimeout from '../2-useTimeout/useTimeout';

export default function useDebounce(callback, delay, dependencies) {
  const { reset, clear } = useTimeout(callback, delay);
  useEffect(reset, [...dependencies, reset]); // dep ���� �ٲ� �� ���� reset ���ش�
  useEffect(clear, []);
  // clear ���ִ� ������ useTimeout�� callback �Լ��� �����ϴµ�
  // ������ reset �Լ��� �����ϴ��� clear(), set()���� �ٽ� set �Ǳ� ������
  // clear�� ���־�� ó�� ��������� �ʴ´�
  // ���� �ش� �κ� �ּ� ó���ϸ�, clear() �� set()�� ����Ǿ�  �ٷ� callback �Լ��� ����ȴ�
  // ���� �̷��� clear ���ִ� ���� ó���� callback �Լ��� ������� �ʵ��� �ϱ� �����̴�
}
```

### Usage 2
