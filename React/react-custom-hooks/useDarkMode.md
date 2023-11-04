## useDarkmode

- darkmode�� ����ϴ� hook����

- ������ ����� useStorage useMediaQuery�� ����Ѵ�

```js
// useDarkMode

import { useEffect } from 'react';
import useMediaQuery from '../16-useMediaQuery/useMediaQuery';
import { useLocalStorage } from '../8-useStorage/useStorage';

export default function useDarkMode() {
  const [darkMode, setDarkMode] = useLocalStorage('useDarkMode');
  const prefersDarkMode = useMediaQuery('(prefers-color-scheme: dark)');
  const enabled = darkMode ?? prefersDarkMode;
  // ó������ darkMode ���� undefined�̹Ƿ� prefersDarkMode�� ���� �����ȴ�
  // prefersDarkMode�� true

  useEffect(() => {
    document.body.classList.toggle('dark-mode', enabled);
  }, [enabled]);

  return [enabled, setDarkMode];
}
```

### Usage

```js
import useDarkMode from './useDarkMode';
import './body.css';

export default function DarkModeComponent() {
  const [darkMode, setDarkMode] = useDarkMode();

  return (
    <button
      onClick={() => setDarkMode((prevDarkMode) => !prevDarkMode)}
      style={{
        border: `1px solid ${darkMode ? 'white' : 'black'}`,
        background: 'none',
        color: darkMode ? 'white' : 'black',
      }}
    >
      Toggle Dark Mode
    </button>
  );
}
```

```js
// useStorage.js

import { useCallback, useState, useEffect } from 'react';

// key : 'useDarkMode' ����
export function useLocalStorage(key, defaultValue) {
  return useStorage(key, defaultValue, window.localStorage);
}

export function useSessionStorage(key, defaultValue) {
  return useStorage(key, defaultValue, window.sessionStorage);
}

// key : 'useDarkMode' ����
function useStorage(key, defaultValue, storageObject) {
  const [value, setValue] = useState(() => {
    const jsonValue = storageObject.getItem(key);
    if (jsonValue != null) return JSON.parse(jsonValue);

    if (typeof defaultValue === 'function') {
      return defaultValue();
    } else {
      return defaultValue;
    }
  });

  // setValue�� value �����Ű�� �Ʒ� useEffect hook�� ����ȴ�
  useEffect(() => {
    if (value === undefined) return storageObject.removeItem(key);
    storageObject.setItem(key, JSON.stringify(value));
  }, [key, value, storageObject]);

  const remove = useCallback(() => {
    setValue(undefined);
  }, []);

  // localStorage�� ��ϵ� ����
  // localStorage�� �� ����ϴ� �Լ� return �޴´�
  return [value, setValue, remove];
}
```

```js
// useMediaQuery.js

import { useState, useEffect } from 'react';
import useEventListener from './useEventListener';

// '(prefers-color-scheme: dark)' ����
export default function useMediaQuery(mediaQuery) {
  const [isMatch, setIsMatch] = useState(false);
  const [mediaQueryList, setMediaQueryList] = useState(null);

  useEffect(() => {
    const list = window.matchMedia(mediaQuery); // media query ��ü

    setMediaQueryList(list);
    setIsMatch(list.matches); // �ܼ��� �����ϴ� �̵�� �����̱� ������ �׻� true �� �ȴ�
  }, [mediaQuery]);

  useEventListener('change', (e) => setIsMatch(e.matches), mediaQueryList);

  return isMatch; // �׻� true ��ȯ
}
```

```js
// useEventListener.js

import { useEffect, useRef } from 'react';

export default function useEventListener(
  eventType,
  callback,
  element = window
) {
  const callbackRef = useRef(callback);

  useEffect(() => {
    callbackRef.current = callback;
  }, [callback]);

  useEffect(() => {
    if (element == null) return;
    const handler = (e) => callbackRef.current(e);
    element.addEventListener(eventType, handler);

    return () => element.removeEventListener(eventType, handler);
  }, [eventType, element]);
}
```
