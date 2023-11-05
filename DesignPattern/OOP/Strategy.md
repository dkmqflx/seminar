## Strategy Pattern

- Strategy ������ ��������� �ܼ��� ���������� ��ü ���� ���α׷��ֿ� �ٽ��� �ܼ��ϰ� ��Ȯ�ϰ� ����� ���� �߿� �ϳ��̴�

- Strategy�� �ܾ��� ���� �������� ���α׷��ֿ��� ������ � ������ �ذ��ϴ� ����� �˰����� �ǹ��Ѵ�

- Strategy ������ � �ϳ��� ����� �����ϴ� �κ��� ���� �߿� �ٸ� ������ ȿ�������� ������ �� �ִ� ����� �����Ѵ�

- �� �ʿ��� ��� ���� ���̶� Strategy�� �ٲ� �� �ִ� �����̴�.

- �Ʒ��� ���� � �ϳ��� ����� �ִٰ� �� ��

```shell

�ܰ�1

�ܰ�2 <- �ܰ� 2�� ���� �˰����� Strategy�� �����Ͽ� �����߿� ������ �� ����

�ܰ�3

```

- ����� �����ϱ� ���ؼ��� �ܰ� 1�������� �ܰ� n���� ������� ����Ǿ�� �Ѵٰ� ����

- �̶� �ܰ� 2�� ���ؼ��� ��� ������ ���� �߿� �������� ����� �ʿ䰡 ���� �� Strategy ������ �����ؼ� ȿ�������� �� ����� ������ �� �ִ�.

- �����غ���, Stragety ������ � ��ɿ� ���ؼ� Ư�� �˰����� �����ϰ� ���ս����ִ� �����̴�

- � �˰����� �����ϰ� ���յǸ� ���� �� ���� �˰������� ���� ������ �� ���� �� �ƴ϶� ���α׷� �����߿� ��Ȳ�� �´� �˰������� ���� ������ �� �ִ�

- ���� ������ �˰����� �����ϸ鼭 ���ο� �˰����� ȿ�������� �߰��� �� �ִ�

### ���� ����

- ������ ���� ������ �Ʒ��� ���� ������ �ϸ��� ���� �޽����� ����� �Ѵٰ� ������ �غ���

  - ĳ���� -> 'Carrier has arrived'

  - ��ũ���÷� -> 'Adun Toridas'

  - Ŀ���� -> 'It is a good day to die'

- �׷��� �̷��� �� ĳ���͵��� ����ϴ� �͵��� �Լ��� �־ ��� ��ü �ȿ� ������ �� �� �ִ�

```js
const Units = {
  ĳ����: () => "Carrier has arrived",
  ��ũ���÷�: () => "Adun Toridas",
  Ŀ����: () => "It is a good day to die",
};
```

- �� ������ �Ʒ�ó�� ���� ������ ���� ������ ��Ƶ� �������� �ʿ��� �� ���� ȣ���ϸ� �ȴ�

```js
const getDialogues = (unitName) => {
  return Units[unitName]();
};
```

- �׷����� ������ �ٲ� �� �ڵ�� ������ ���� ����.

- ������ ���� ������ �θ� �ȴ�

- �׷��� ��� ���ۿ� ���� ���� ������ �̰͸� ���� �Ǵ��� �ϴ� ���̴�

- �׸��� ���� ������ �ٸ� �ڵ带 ���� ���� �ִ�

## ��Ʈ�� �ʿ��� ���� ����

- ��� ���񽺿��� �� �پ��� ��Ʈ�� ��� �������� �����Ѵٰ� ����

- ���� ����� ��Ʈ�� �� ���� �ְ� �̷� ���� ����� ��Ʈ�� �����شٰų� �ƴϸ� ��� �ñ⿡�� ���� ��Ʈ�� ������� �Ǵ� ��Ȳ�̶�� �����غ���

- �׷��� �Ʒ�ó�� �� ��Ʈ ����� �� ���� ��Ʈ ����� ���� ��Ʈ ����� �̷��� �ؾ� �ȴ�

- �̷��� �Ǹ��� �ߺ� �ڵ嵵 ����� �������� ���������� �Ź� ��Ʈ�� �ؾ� �ȴ�

```js
const chart = new BarChart();
chart.render(data);

const chart = new DonutChart();
chart.render(data);

const chart = new LineChart();
chart.render(data);
```

- �̷��� �Ǹ� �������� ��Ʈ���� �� �Ǵ� ��Ȳ�� �ȴ�

- �׷��� �������� ��Ʈ��� ���۳�Ʈ�� �� ����� ������ �и��� �� �ִµ�

- ��, ���� �������� �и��� ������ ���̴�

- �׷��� �Ʒ�ó�� �� ��Ʈ�� ���� ������ ���� ����� ��Ʈ ������Ʈ�� �� ��Ʈ�� ���� ��Ʈ���� ���� �޾Ƽ� ��Ʈ�� �����ϴ� ���̴�

```js
const chart = new Chart(new BarChart());
chart.render(data);

const chart = new Chart(new DonutChart());
chart.render(data);
```

- �� �Ʒ�ó�� ���� ����Ͻ� ������ ��Ʈ �ȿ� �ְ� ������ ������ �����ϰ� �Ǵ� ���̴�

- �Ʒ�ó�� ������ ������ �Ķ���ͷ� �ް� chartType �Ӽ����� �޾Ƽ� ���� �������� ������ render �Լ��� �θ��� ���̴�

- ���� render�� �� �ڵ�� ���� ���޵Ǵ� ��, ���� ��Ʈ�� �ν��Ͻ��� ���ǰ� �Ǿ� �־�� �Ѵ�

- ��, ��Ʈ�� ������ ���ҷμ� ��� ������ �޾Ƽ� ��� ������ �����ϴ� ���̶�� �� �� �ִ�

- ���޵Ǵ� ��Ʈ�� �ٲ��� ��Ʈ ���۳�Ʈ�� ����� ���� ���� ���̴�.

```js

class Chart[
  constructor(stargety){
    this.chartType = stargety
  }

  render(data){
    this.chartType.render(data)
  }
]

```

## Ŭ���̾�Ʈ �����

- ����Ʈ�� ����ϴ� ��� Ŭ���̾�Ʈ ������� ���� �ϴµ�

- ������� �ᱹ���� ��� url�� ��Ī�Ǵ� ���۳�Ʈ�� ������ �ϴ� ����̱� ������

- �Ʒ�ó�� �ڵ带 �ۼ��ؼ� url�� �´� ������Ʈ�� �������� �� �ִ�

- �̰͵� ���� �����̴�.

```js
// ����� ����

const routes = {
  "/post": new PostList(),
  "/post/:id": new PostItem(),
};
```

- �Ʒ� �ڵ�� ���� ������� �θ��� ������ ����

```js
// ������� �θ��� ����

const viewComponent = this.routes[path];
viewComponent.renderLayout();
```

- `this.routes[path]`�� ���� ����� ������ routes��� �����غ���

- path�� '/post'�� 'post/:id/' �� �Ǵ� ���̰�

- �̷��� ���޹��� ������Ʈ�� viewComponent ������ renderLayout�� �ϴ� ���̴�

- ���� �� ���� routes�� path�� �ش��ϴ� ������Ʈ�� renderLayout�� ���ǵǾ� �־�� �Ѵ�

- �׷��� routes�� url�� ��� ����� ��ΰ� �ٲ���

- ������� �θ��� ������ ���� ������ ����� ���� ����.

- ���� ������ ������ ���� ������ ������ ������ ���� ��� �����ؼ�

- ������ �δ� �̷� ��ĵ��� ���� ������ �����̴�

- ������ �������� ���� �� ���� ������ ������ ���� ����

- ������ �߰��ؼ� �ű�ٰ� �־� �ָ� �Ǵ� ������ ���� ������ Ȱ���� �� ���� �ִ�.

### Ŭ���� ���̾�׷�

- �Ʒ� Ŭ���� ���̾�׷��� �ǽ� ������ ���� �����̴�

  <img src='./images/strategy/strategy01.png'>

- SumPrinter�� 1���� � ������ ������ ����� �ִ� Ŭ�����̴�

- �̶� 1���� � �������� ������ SumStrategy ��� �������̽��� ���ؼ� ���´�

- �׸��� 1���� � �������� ������ ��� ��ü���� ���� �ڵ�� sumStrategy �������̽��� ������ LoopSumStrategy Ŭ������ GaussSumStragety Ŭ������ ���ؼ� �ڵ尡 �ۼ��ȴ�

- �߿��� ���� SumPrinter�� SumStrategy �������̽����� ����ؼ� �ڵ尡 �ۼ��ȴٴ� ���̴�

- ��, SumPrinter Ŭ������ ���� ������ ����ϴ� LoopSumStrategy Ŭ������ GaussSumStragety Ŭ������ ���� �� �ʿ䰡 ����.

- �̷� ���ؼ� ���Ŀ� ������ ����ϴ� ����� �����Ǿ� �ٸ� �˰����� �߰��Ǿ��� �� ������ SumPrinter Ŭ������ �ڵ带 ���� ������ �ʿ䰡 ����.

### Ŭ���� ���̾�׷� ����

- ���� ���� SumStrategy �������̽��� �Ʒ��� ���� �ۤ����Ѵ�

```js
// SumStrategy.ts

export default interface SumStrategy{
  get(N:number):number
}

```

- �׸��� SumStrategy �������̽��� ���� Ŭ���� �� LoopSumStrategy Ŭ������ �ó�� �߰����ش�

```ts
// LoopSumStragety.ts
import SumStrategy from "./SumStrategy"; // �������̽� import

export default class LoopSumStrategy implements SumStrategy {
  get(N: number): number {
    let sum = 0;

    for (let i = 0; i <= N; i++) {
      sum += i;
    }

    return sum;
  }
}
```

- SumStrategy �������̽��� �� �ٸ� ���� Ŭ���� GaussSumStragety Ŭ������ �ó�� �߰����ش�

```ts
// GaussSumStragety.ts
import SumStrategy from "./SumStrategy"; // �������̽� import

export default class GaussSumStragety implements SumStrategy {
  get(N: number): number {
    return (N * (N + 1)) / 2;
  }
}
```

- �׸��� ���� SumPrinter�� Ŭ������ �������ش�

```ts
// SumPrinter.ts

export class SumPrinter�� {
  print(strategy: SumStrategy, N: number, domOutput: Element) {
    const value = strategy.get(N); // 1���� N���� ���� ���
    domOutput.innerHTML = `1~${N} ������ ���� = ${value}`;
  }
}
```

- �� Ŭ������ 1���� n���� ������ ����ϴ� print �޼��带 �����ϰ�

- print �޼���� 3���� ���ڸ� ���޹޴´�

- ù��° ���ڴ�strategy �������̽�,

- �ι�°�� 1���� N���� ���� ����ϱ� ���� N ��,

- �׸��� �� ��° ���ڴ� ���� ����� ����� DOM ��ü�̴�

- ������ �ۼ��� �ڵ带 �Ʒ�ó�� ��� �� �ִ�

```ts
import SumPrinter from "./SumPrinter";
import LoopSumStragety from "./LoopSumStragety";
import GaussSumStragety from "./GaussSumStragety";

const printer = new SumPrinter();

const dom1 = document.crateElement("h1"); // ��� ����� ����ϱ� ���� DOM ��ü
document.body.append(dom1);

const dom2 = document.crateElement("h1"); // ��� ����� ����ϱ� ���� DOM ��ü
document.body.append(dom2);

// LoopSumStragety ����� ����ؼ� 1���� 100���� ���� �����ش�
printer.print(new LoopSumStragety(), 100, dom1);

// GaussSumStragety ����� ����ؼ� 1���� 100���� ���� �����ش�
printer.print(new GaussSumStragety(), 100, dom2);
```

- ��ó�� �ϳ��� ��ɿ� ���ؼ� ���� �ٸ� ����� �˰����� ���� �߿� ������ �� �ִ� ���� Stragety �����̴�

---

### Reference

- [��ȹ�� �ٲ��� ������~ (�ϻ��� ���������� 2��)](https://www.youtube.com/watch?v=hCUw3zNRioM&t=29s)
- [TypeScript�� ���� GoF�� ������ ����: 4. Strategy](https://www.youtube.com/watch?v=TiHzYc8I3Kk&list=PLe6NQuuFBu7H3sFnErshsfgNPE9dOZZrx&index=5)
