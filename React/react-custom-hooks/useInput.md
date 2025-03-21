## useInput

- input을 관리하기 위한 custom hook

```js
import { useState, useCallback } from 'react';

function useInputs(initialForm) {
  const [form, setForm] = useState(initialForm);
  
  // change
  const onChange = useCallback(e => {
    const { name, value } = e.target;
    setForm(form => ({ ...form, [name]: value }));
  }, []);
  const reset = useCallback(() => setForm(initialForm), [initialForm]);
  return [form, onChange, reset];
}

export default useInputs;

```