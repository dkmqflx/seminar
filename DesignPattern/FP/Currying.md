## Ŀ��

- �Լ��� ��ȯ�ϴ� �Լ�

  - �Լ��� ���α׷��� Ư¡ ��� �Լ��� ���ڷ� ������ �� �ִ�

  - Ŀ���� �̿��ϸ� ����� �߰��� �Լ��� ��ȯ�� �� �ִ�

- ��ü���� ���α׷��ֿ��� ����� ���ؼ� �̹� ������ �ߺ� �ڵ带 ���� �� �ִµ�

- �Լ��� ���α׷��ֿ����� Ŀ���� ����ؼ� �ش� ����� ������ �� �ִ�.

- �Ʒ� ó�� �� ���� ���� ����� �����ϴ� �Լ��� �ִٰ� ����

```js
// �� ���� ���� ����� �����ϴ� �Լ�
const multiply = (a, b) => {
  return a * b;
};
```

- �� �Լ��� ��Ȱ���ؼ� 2��� �Լ��� �����Ѵٸ� �Ʒ�ó�� �ڵ带 �ۼ��� �� �ִ�

```js
// ��Ȱ���ؼ� 2��� ���ִ� �Լ��� �Ʒ�ó�� ������ �� �ִ�
const multiplyTwo = (a) => {
  return multiply(a, 2);
};
```

- ������ �̷��� �ڵ带 �ۼ��ϸ� 3���, 4��� �ϴ� �ڵ带 �Ϸ��� 2��� 3, 4�� �ִ� �Լ��� �߰������� �������־�� �Ѵ�

- �� �� Ŀ���� ����ϸ� �̷��� �ݺ������� ������ �ʿ� ���� ����� �Ǵ� ���� ���ڷ� �޴� �Լ��� ������ �� �ִ�

- �ڵ�� ������ ����

```js
// �Ʒ�ó�� �Լ��� ������ �� �ִ�
const multiplyX = (x) => {
  return (a) => {
    return multiply(a, x);
  };
};
```

- �ۼ��� �ڵ带 �Ʒ��� ���� ����� �� �ִ�

```js
const x2 = multiplyX(2); // ��� �Լ� ����
console.log(x2(10)); // 20

// �Ʒ�ó�� ���ڸ� ������ ������ �� �ִ�
multiplyX(2)(3); // 6
multiplyX(3)(3); // 9
```

- �߰������� �ڵ带 �ۼ��ؼ� �Ʒ�ó�� �����Լ��� ����������

```js
const add = (a, b) => {
  return a + b;
};

const addX = (x) => {
  return (a) => add(x, a);
};
```

- �׸��� ������ ���� ���� ������ �ִٰ� ����

  - 2x \* 3 + 4

- Ŀ���� ����ϸ� �� ������ ���� ������ ���� ����� �� �ִ�

```js
const addFour = addX(4); // (a) => add(4, a);
const multiplyThree = multiplyX(3); // (a) =>  multiply(a, 3);
const multiplyTwo = multiplyX(2); // (a) =>  multiply(a, 2);

const forumula = (x) => {
  return addFour(multiplyThree(multiplyTwo(x)));
};

formula(10); // 64
```

- `multiplyTwo(x)` === `2x`

- `multiplyThree(multiplyTwo(x))` === `multiplyThree(2x)`

- `addFour(2x*3)` === `2x + 3`

- ������ �̷��� �ۼ��� ���� �Լ��� ������ �ִµ�

- ��ȣ�� ���Ƽ� ȥ���ϱ� ���� �Լ��� ��ֵǴ� ������

- �츮�� �����ϴ� ������ ����(-->)�� �ƴ�, �ݴ� ���� (<--) �̶�� ���̴�

- �׸��� ������ �߸� ��ġ�ϸ� ������� ������ �߻��ϴ� ��찡 ����⵵ �Ѵ�

- ���� �Լ����� ������� ��ġ�� �� �ִٸ� ������ ������ �ذ��� �� �ִ�

  - `multiplyTwo`, `multiplyTwo`, `addFour` ������ ��ġ

- �̷��� �Լ��� ������� �����ϵ��� �����ִ� �Լ��� reduce�� ����ؼ� ���� �� �ִ�

```jsx
const formula = [multiplyTwo, multiplyThree, addFour].reduce(
  (prevFunc, nextFunc) => {
    return (x) => {
      return nextFunc(prevFunc(x));
    };
  }
  (k) => k // �ʱ� ��, ���� ����
);

console.log(formula(10)); // 64
```

- ù��° ������ ������ ����

  - prevFunc: `(k) => k` (initValue)

  - nextFunc: multiplyTwo

  - ��ȯ��(1):

  ```js
  (x) => {
    return multipyTwo((x) => x);
  };
  ```

- �ι�° ������ ������ ����

  - prevFunc: ��ȯ��(1)

  - nextFunc: multiplyThree

  - ��ȯ��(2):

  ```js
  (x) => {
    return multiplyThree(��ȯ��(1));
  };
  ```

- ������ ������ ������ ����

  - prevFunc: ��ȯ��(2)

  - nextFunc: addFour

  - ��ȯ��(3):

    ```js
    (x) => {
      return addFour(��ȯ��(2));
    };
    ```

- �̷��� �ڵ带 �ۼ��ϸ� �迭�� ���� ��ġ ������� �Լ��� ����Ǵ� ���� Ȯ���� �� �ִ�

- �̷��� �ۼ��� �� �� �ִ�

```js

const formula = [multiply(2), multiply(3), add(4)].reduce(
  (prevFunc, nextFunc) => {
    return (x) => {
      return nextFunc(prevFunc(x));
    };
  }
  (k) => k // �ʱ� ��, ���� ����
);

console.log(formula(10)); // 64


```

- �׸��� �̷��� �ۼ��� �Լ��� �Ʒ�ó�� �� �� �����ؼ� �ۼ��� �� �ִ�

- �� �Լ��� Compose��� �Ѥ� ��

```js
const compose = (...args) => {
  return args.reduce(
    (prevFunc, nextFunc) => {
      return (...values) => {
        return nextFunc(prevFunc(...values));
      };
    },
    (k) => k
  );
};

const formula = compose(multiplyX(2), multiplyX(3), addX(4));

formula(10); // 64
// compose�� return�ϴ� ���� (...values) => {] �Լ�
```

- ������ڸ�

1. Ŀ�� �������� �Լ� ���ڸ� �и��Ͽ� �Լ��� ��Ȱ���� �� �ִ�

2. ���� �Լ�(Higher Order Function)�� �Լ��� ���ڷ� �޾� ������ �� �Լ��� ��ȯ�Ѵ�

3. ������(Compose) �Լ��� ����ϸ� ���� ���� �Լ��� ������ �� �ִ�

---

## Referenece

- [Ŀ���� �Լ� ���� ����ġ��-currying & compose: Do it! ����Ʈ ���α׷��� ����](https://www.youtube.com/watch?v=VEAUpHod4cg&t=335)
