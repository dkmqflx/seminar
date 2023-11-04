## useFetch

- API ��û�� ���� �����͸� �޾ƿ� �� ����� �� �ִ� hook

```js
// useFetch.js

const useFetch = (url = '', options = null) => {
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(false);

  useEffect(async () => {
    setLoading(true);

    fetch(url, options)
      .then((res) => res.json())
      .then((data) => {
        setData(data);
        setError(null);
      })
      .catch((error) => {
        setError(error);
        setData(null);
      })
      .finally(() => setLoading(false));
  }, [url, options]);

  return { loading, error, data };
};

export default useFetch;
```

### Usage

```js
import useFetch from './useFetch';

const App = () => {
  const { loading, error, data = [] } = useFetch('url');

  if (error) return <p>Error!</p>;
  if (loading) return <p>Loading...</p>;

  return (
    <div>
      <ul>
        {data?.map((item) => (
          <li key={item.id}>
            <a href={item.url}>{item.title}</a>
          </li>
        ))}
      </ul>
    </div>
  );
};
```

- �Ʒ�ó�� useReducer hook�� ����ؼ��� useFetch�� ������ �� �ִ� 


```js


import { useReducer, useEffect } from 'react';

function reducer(state, action) {
  switch (action.type) {
    case 'LOADING':
      return {
        loading: true,
        data: null,
        error: null
      };
    case 'SUCCESS':
      return {
        loading: false,
        data: action.data,
        error: null
      };
    case 'ERROR':
      return {
        loading: false,
        data: null,
        error: action.error
      };
    default:
      throw new Error(`Unhandled action type: ${action.type}`);
  }
}

function useAsync(callback, deps = []) {
  const [state, dispatch] = useReducer(reducer, {
    loading: false,
    data: null,
    error: false
  });

  const fetchData = async () => {
    dispatch({ type: 'LOADING' });
    try {
      const data = await callback();
      dispatch({ type: 'SUCCESS', data });
    } catch (e) {
      dispatch({ type: 'ERROR', error: e });
    }
  };

  useEffect(() => {
    fetchData();
    // eslint ������ ���� �ٿ����� ��Ȱ��ȭ
    // eslint-disable-next-line
  }, deps);

  return [state, fetchData];
}

export default useAsync;

```