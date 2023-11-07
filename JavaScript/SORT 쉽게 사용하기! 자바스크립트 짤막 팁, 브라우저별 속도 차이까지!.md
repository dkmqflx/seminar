## SORT ���� ����ϱ�! �ڹٽ�ũ��Ʈ ©�� ��, �������� �ӵ� ���̱���!

- �Ʒ��� ���� �迭�� �ִٰ� ����

```js
const numbers = [1, 5, 3, 4, 7, 9, 11, 33, 44, 32];
```

- �� �� sort �Լ��� �����ϸ� ������� ���ĵǴ� ���� �ƴ϶� �Ʒ��� ���� ����� ��µȴ�

```js
console.log(numbers.sort()); // [1, 11, 3, 32, 33, 4, 44, 5, 7, 9]
```

- sort �Լ��� ����� ���� �Լ��� ���ؼ� �� ���ڸ� �����־�� �Ѵ�

- �Ʒ�ó�� 1�� console.log�� ���ؼ� a�� b�� ����ϰ� 1�� ��ȯ �ϴ� �Լ��� �����ߴٰ� ����

```js
numbers.sort((a, b) => {
  console.log(a, b);
  return 1; // ������ ������� ��µȴ�
  // return -1; // ������ ������ �������� ��µȴ�
});
```

- �� �� a, b�� �Ʒ��� ����, ���� ��ġ�� ���� ���ڿ� ���� ��ġ�� ���� ���ڰ� ��µȴ�

```shell
  5 1
  3 5
  4 3
  7 4
  9 7
  11 9
  33 11
  44 33
  32 44
```

- ��, sort�� ���޵� �Լ��� �� ���� a, b�� ���ؼ� ������� 1�� ���� ����� �Ǹ� �״�� ������ �����ǰ�

- -1�� ���� ������ �Ǹ� ������ �ٲٰ� �ȴ�

- ���� ����������� ���İ� �ʹٸ� �Ʒ��� ���� �ڵ带 �ۼ��ϸ� �ȴ�

```js
numbers.sort((a, b) => {
  return a - b;
});
```

- �ۼ��� �ڵ�� ������ ���� �۵��Ѵ�

- return a-b�� �ϰԵǸ�

- 5 1�� ���ϴϱ� a - b���� 5 - 1�� �Ǵµ�

- �� ���� ����� �Ǵϱ�

- 1�� 5�� �տ� �ְ� �Ǵ� ���̴�

- �ݴ�� ����������� ������ �Ѵٰ� �ϸ�

- return b - a�� �ϰ� �Ǹ�

- 5�� 1�� ���� �� 1 - 5�� ������ �Ǵϱ�

- ������ 1, 5�� ��ġ���� 5,1�� ��ġ�� �ٲ�� �Ǵ� ���̴�

- ��, ������ �ۼ��� �ڵ�� �Ʒ��� ����

```js
numbers.sort((a, b) => {
  if (a > b) return 1; // a�� b���� ũ�� 1 ��ȯ, ���� �״�� �����ȴ�
  else if (a < b)
    return -1; // b�� a���� ũ�� -1 ��ȯ, ���� �״�� ���� ���� �ʴ´�
  else return 0;
});
```

- �� a�� b�� � ���� �� ���� ������

- ������ �� a�� b���� ū ��� 1�� �ȴٴ� ���� ������ �����ȴٴ� ���̰�

- ������ �� a�� b���� ���� ��� -1�� �ȴٴ� ���� ������ �ٲ� �ٴ� ���̴�

- �̰��� ���� �������� ������ �� �ִµ�

- ������ �� a�� b���� ū ��� -1�� �ȴٴ� ���� ������ �ٲ�ٴ� ���̰�

- ������ �� a�� b���� ���� ��� 1�� �ȴٴ� ���� ������ ���� �ȴٴ� ���̴�

- �ٸ� �̷� sort �˰����� ���������� �ٸ��� �����Ѵ�

- �׷��� ������ �� sort ���� �ִ� log�� ������ ���� ������ �ٸ��� ��µȴ�

- ���� ó�� �ۼ��� return 1�� ��ȯ�ϴ� �Լ�, �׸��� a-b, b-a�� return �ϴ� �Լ� ��� console.log�� ���� a�� b�� ������ ���� �ٸ� ���� Ȯ���� �� �ִ�

- ��ü�� sort �Լ��� ����ؼ� ���� �� �ִ�

```js
const animals = [
  { name: 'monkey', size: 'medium', weight: 7 },
  { name: 'cat', size: 'small', weight: 5 },
  { name: 'lion', size: 'big', weight: 30 },
  { name: 'mouse', size: 'small', weight: 12 },
  { name: 'monkey', dog: 'medium', weight: 28 },
];

console.time();
animals.sort((a, b) => {
  console.log(`a: ${a.name} b: ${b.name}`);
  return a.name > b.name ? 1 : -1;
});
console.timeEnd();
```

- �׸��� console.time�� console.timeEnd�� ����ؼ� ���������� �����غ���

- ������ ���������� ����� �ٸ��� ��µǴ� ���� Ȯ���� �� �ִ�

- �׸��� sort �˰����� ���������� �ٸ��� �����ϱ� ������

- �� animal sort ���� �ִ� log�� ������ ���� ������ �ٸ��� ��µȴ�

<br/>

- �� �ٸ� ���÷� ������ ���� �迭 ���� ��ü�� ���� ��, ������ ��ü�� ���� �������� ������ �� �ִ�

```javascript
const data = [
  { date: '2021-04-17', id: 5, id2: 11 },
  { date: '2021-04-21', id: 3, id2: 3 },
  { date: '2021-04-29', id: 2, id2: 21 },
  { date: '2021-05-11', id: 7, id2: 19 },
  { date: '2021-05-21', id: 7, id2: 18 },
  { date: '2021-06-21', id: 9, id2: 18 },
];
```

- `id`�� �������� ����

  ```javascript
  console.log(data.sort((a, b) => a.id - b.id));
  ```

- `id2`�� �������� ����

  ```javascript
  console.log(data.sort((a, b) => a.id2 - b.id2));
  ```

- ������ ��Ȳ�� ���� �ٸ� �������� ���Ҹ� �����ؾ��� ���� �ִµ�, �� �� sort �Լ��� ������ ������ �Ǵ� �Լ��� �����ϴ� ������� ���� �ٸ� �������� ���Ҹ� ������ �� �ִ�

- [MDN ������ sort](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)�� Ȯ���� ���� ������ ���� sort �Լ����� `compare function`�� ���޵Ǵ� ���� Ȯ���� �� �ִ�

  ```jsx
  arr.sort([compareFunction]);
  ```

- �� ��, sort �Լ��� ���������� a, b �� ������ ���ڸ� ���� �� ���� ������, �Լ��� �����ϴ� �Լ��� ����� �� �ȿ��� ���� ������ �Ǵ� ������ ����Ѵ�

- �Ʒ� �Լ��� ���� ������ �Ǵ� �Լ���,`sortby`�� ������ �Ǵ� ���� ���������ν� ���� �ٸ� �������� �����͸� ������ �� �ִ�

```jsx
// compare function
const sortFunc = (sortby) => (a, b) => a[sortby] > b[sortby] ? 1 : -1;
```

```javascript
// id�� �������� ����
console.log('sorted - id: ', data.sort(sortFunc('id')));
```

```javascript
// id2�� �������� ����
console.log('sorted - id2: ', data.sort(sortFunc('id2')));
```

---

## Reference

---

### Reference

- [SORT ���� ����ϱ�! �ڹٽ�ũ��Ʈ ©�� ��, �������� �ӵ� ���̱���!](https://www.youtube.com/watch?v=scjyeC74_4k&list=PLGk6-UFPJT2XTiBhMH7v9LXlX_JHEUmfg&index=12)
- [MDN web docs - Array.prototype.sort](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
- [Any way to extend javascript's array.sort method to accept another parameter?](https://stackoverflow.com/questions/8537602/any-way-to-extend-javascripts-array-sort-method-to-accept-another-parameter)
