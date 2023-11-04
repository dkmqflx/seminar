## useOnlineStatus

- ���� �������� �¶�������, �������� �������� �� �� �ִ� hook

- useEventListner hook�� ����Ѵ�

```js
import { useState } from 'react';
import useEventListener from './useEventListener';

export default function useOnlineStatus() {
  const [online, setOnline] = useState(navigator.onLine);

  useEventListener('online', () => setOnline(navigator.onLine));
  useEventListener('offline', () => setOnline(navigator.onLine));

  return online;
}
```

### Usage

```js
import useOnlineStatus from './useOnlineStatus';

export default function OnlineStatusComponent() {
  const online = useOnlineStatus();

  return <div>{online.toString()}</div>;
}
```
