## useAcync

- �񵿱����� ��û�� ó���� �� ����� �� �ִ� hook

```js
// useAsync.js

import { useCallback, useEffect, useState } from 'react';

// �񵿱������� ������ callback �Լ��� �����Ѵ�
export default function useAsync(callback, dependencies = []) {
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState();
  const [value, setValue] = useState();

  // 1. dep�� ���ϸ� useCallback ���� Wrapping�� �Լ��� ���Ѵ�
  const callbackMemoized = useCallback(() => {
    setLoading(true);
    setError(undefined);
    setValue(undefined);

    callback()
      .then(setValue)
      .catch(setError)
      .finally(() => setLoading(false));
  }, dependencies);

  // 2. useCallback���� Wrapping �� �Լ��� ���ϸ� �ٽ� useEffect hook�� ����ȴ�
  useEffect(() => {
    callbackMemoized();
  }, [callbackMemoized]);

  return { loading, error, value };
}
```

### Usage

```js
import useAsync from './useAsync';

export default function AsyncComponent() {
  // �񵿱������� ������ calback �Լ��� �����Ѵ�
  const { loading, error, value } = useAsync(() => {
    return new Promise((resolve, reject) => {
      const success = false;
      setTimeout(() => {
        success ? resolve('Hi') : reject('Error');
      }, 1000);
    });
  });

  return (
    <div>
      <div>Loading: {loading.toString()}</div>
      <div>{error}</div>
      <div>{value}</div>
    </div>
  );
}
```

- �������� script�� �߰��� ���� hook�� ����� �� �ִ�

```js
import useAsync from './useAsync';

export default function ScriptComponent() {

  const { loading, error, value } = useAsync(() => {
    const script = document.createElement('script');
    script.src = url;
    script.async = true;

    return new Promise((resolve, reject) => {
      script.addEventListener('load', resolve);
      script.addEventListener('error', reject);
      document.body.appendChild(script);
    });
  }, [url]);



  if (loading) return <div>Loading</div>
  if (error) return <div>Error</div>

  return (
    return <div>{window.$(window).width()}</div>
  );
}
```

- �Ʒ�ó�� useScript ��� ������ Custom Hook���� ó���� �� �ִ�

```js
import useScript from './useScript';

export default function ScriptComponent() {
  const { loading, error } = useScript(
    'https://code.jquery.com/jquery-3.6.0.min.js'
  );

  if (loading) return <div>Loading</div>;
  if (error) return <div>Error</div>;
  return <div>{window.$(window).width()}</div>;
}
```

```js
import useAsync from '../9-useAsync/useAsync';

export default function useScript(url) {
  return useAsync(() => {
    const script = document.createElement('script');
    script.src = url;
    script.async = true;

    return new Promise((resolve, reject) => {
      script.addEventListener('load', resolve);
      script.addEventListener('error', reject);
      document.body.appendChild(script);
    });
  }, [url]);
}
```
