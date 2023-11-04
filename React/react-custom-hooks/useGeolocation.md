## useGelocation

- ������� ��ġ�� �� �� �ִ� hook

```js
import { useState, useEffect } from 'react';

export default function useGeolocation(options) {
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState();
  const [data, setData] = useState({});

  useEffect(() => {
    const successHandler = (e) => {
      setLoading(false);
      setError(null);
      setData(e.coords);
    };
    const errorHandler = (e) => {
      setError(e);
      setLoading(false);
    };

    // 1ȸ������ ���� ��ġ�� �� �� �ִ� ���
    navigator.geolocation.getCurrentPosition(
      successHandler, // sucess callback
      errorHandler, // fail cakkback
      options
    );

    // ����ڰ� �����̴� ���� ��� ����
    const id = navigator.geolocation.watchPosition(
      successHandler,
      errorHandler,
      options
    );
    return () => navigator.geolocation.clearWatch(id);
  }, [options]);

  return { loading, error, data };
}
```

### Usage

```js
import useGeolocation from './useGeolocation';

export default function GeolocationComponent() {
  const {
    loading,
    error,
    data: { latitude, longitude },
  } = useGeolocation();

  return (
    <>
      <div>Loading: {loading.toString()}</div>
      <div>Error: {error?.message}</div>
      <div>
        {latitude} x {longitude}
      </div>
    </>
  );
}
```
