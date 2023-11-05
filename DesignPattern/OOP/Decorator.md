## Decorator Pattern

- Decoator ���� "����ϴ� ���"���� ��ĵǴ� ���� ����� ������ �������� ó���Ѵ�

- ��ü�� ����� ��ġ ���ó�� ��� �߰��� �� �ִ� ��������

- ����� ���� �߿� �������� ���� �Ǵ� Ȯ���� �� �ִ� �������� ���α׷��� Ȯ�强�� �������� ��� ��ų �� �ִ�

- �������ڸ� Decorator ������ � ��ü�� ���(���)�� ������ �� �� ��ü�� ����� ����� ���Ͻ� �� �� �ִ�

- �����Ϳ� ����� ���Ͻ� �� �� �����Ƿ� �پ��� ����� ��ø�ؼ� ������ �� ������ ����Ǵ� ����� ������ ���� �ٸ� ����� ���� �� �ִ�

- ��, ���ڷ����� ������ ������ ������ �����ϸ鼭 ����� �߰��ϰų� Ȯ���� �� �����ϰ� ����� �� �ִ�

- �ַ� ���δ� ����, �� wrapper�� ���¸� ���� �ִ�.

- �̷� ������� �ͼ��ϰԴ� �����Լ�(higher-order function)�� ���� �� �� �ִµ�,

- ��Ű ����� ������ �����Լ��� '�ϳ� �̻��� �Լ��� ���ڷ� �ްų�, �Լ��� ����� ��ȯ�ϴ� ��' �̶�� �����ϰ� �ִ�

- ����Ʈ���� ������ ���忡�� ���� �� �� �ִ� ���÷δ� debounce �Լ��� �ִ�

- debounce �Լ��� Ư�� �ð����� �ߺ��� ������ ���� �� ������ ȣ�⸸ ����� �� �ֵ��� �ϴ� ������ �Ѵ�

- ���� �� �Ʒ��� ���� ���·� ������ �ڵ�� input â�� Ű���尡 ����� ������ �˻��� ����Ǹ鼭 �Ź� ��û�� �߻��ϴ°��� �� �� �ִµ�

```js
const search = (keyword) => getList(keyword);
//...
<Input onChange={serach} />;
```

- ���⼭ �Ʒ��� ���� debounce �Լ��� ������ search �Լ��� �����ָ�

```js
const search = (keyword) => getList(keyword);
const debounceSearch = debounce(search, 500)
//...
<Input onChange={debounceSearch} />;
```

- ��û�� ����ǵ��� ó���� �� �ְ� �� ���¸� search �Լ��� debounce�� ���ڷ����� �Ǿ��� ��� �� �� �ִ�.

- �����Լ��� debounce�� ������ ���� ���·� ������ �� �ִ�.

```js
const debounce = (func, delay) => {
  let timerId;

  return (...args) => {
    if (timerId) {
      clearTimeout(timer);
    }
    timerId = setTimeout(() => {
      func(...args);
      timerId = null;
    }, delay);
  };
};
```

- ���� �Լ��� �̿��� ��ȯ�� ���� �Լ��� closure ������ timerId�� �����ϸ鼭 Ư�� �ð� ���� �ߺ��� ������ ������ ���� ������ ��ȿȭ�ϰ�

- �߰� ������Ű�� ������ �ϸ鼭�� ��������δ� ���ڷ� ���Ե� �Լ��� �����ϰ� �Ǵµ�

- �̷� ���¸� ����ؼ� �Ʒ�ó�� Ư�� ������ �����ϴ� �Լ��鿡 �α��� �ϰų� ���� ������ ���ڷ������ϴ� ������ ���� ���� ���α׷���(AOP)������ ���踦 �غ� �� ���� ���̴�

```js
const handleLoginClick = recordClickLog((/* ... */) => {
  /* ... */
});

const handleFormSubmit = validateForm((/* ... */) => {
  /* ... */
});
```

- �� �����̸� babel�̳� typescript ���� ȯ���� ����ϸ� �Ʒ��� ���� ���·� class ���ο��� �����(at sign)�� ����Ͽ� ���ڷ����͸� ���������� �������� �� �ִµ�

<img src='./images/decorator02.png'>

- class ������ ������ ����Ѵٸ� ���� �Լ��� �����ϰ� ���ڷ����͸� �����̴� ������ ���ϰ� ������ ������ �� �ִ�.

### React ���� ����ϱ�

- ����Ʈ���� ���ϰ� ����ϴ� ���ڷ����� ���� �߿� �ϳ��� ���� ������Ʈ(higher-order component)�� ���� �� �� �ִ�.

- ���� �˷��� ���÷δ� �Ʒ� �ڵ��� ���� ���ÿ� ���� �α��� ���ο� ���� ���Ǻ� �������� �ϴ� ������Ʈ�� ��ȯ�ϴ� �Լ�(`withAuth`)�� �����ϰ�

- Ư�� ������Ʈ�� ���μ� �α��� ���ο� ���� �������� ������ ���� ����� ������ �� �ִµ� �ᱹ �� ���� ���ڷ����� �����̶�� �� �� �ִ�.

<img src='./images/decorator/decorator03.png'>

- ���� ������Ʈ�� ��������� �߻��ϴ� ��Ƽ ������ �ּ�ȭ�ϱ� ���ؼ�

- ���� ���� �Լ��� ���� �����ؼ� �����ϴ� �ͺ��ٴ� ������ ������Ʈ ���·� ���ڷ����� �� �� �� �ִ�.

- �׸��� �ణ ������������ swr�̳� react query ���� ����ؼ� �Ʒ�ó�� data fetching�� �ϴ� �������� �ִٰ� �Ʒ�ó�� ����������

<img src='./images/decorator/decorator04.png'>

- ���⿡ �״�� ������ ���� suspense �ɼ��� ����� ��� ���� ������ ���İ� �Ǿ ������ ��ü�� ������ ��ġ�� �Ǵµ�

<img src='./images/decorator/decorator05.png'>

- �̸� ��ϸ� ������� suspense�� �����ϰ� �ʹٸ� �Ʒ��� ���� ������ ������Ʈ�� �и��ؾ��ϰ�

  <img src='./images/decorator/decorator06.png'>

- ������Ʈ �и��� depth�� ������� �������迡 ���� props�� �Ź� �Ű������Ѵٴ���

- ���� ������Ʈ�� ���� ��� ��������� ���� ���ŷο��� ���� �� �ִ�.

- �̶� �Ʒ��� ���� Suspense ������Ʈ�� �����ؼ� children�� �͸��Լ� ���·� ����� �� �ִ� ������ ���ڷ����͸� �����ϰ�

- ������ ������Ʈ �и� ���� ���� ��ȣ�ۿ��� �����ϵ��� ������ �� �ִ�

<img src='./images/decorator/decorator07.png'>

- ���� '�̰� ������Ʈ ������ �����ؼ� ����ص� ���� �ʳ�?', '���������� ��� �Ұų�' ��� �Ҽ��� �ִµ�

- ���¿� ������ ��Ȯ�� ���еǾ��ִ� ���踦 ������� ����ϰԵǸ�

- ������Ʈ�� �����ϴ� �帧�� �� �������� �κе� �ִ�.

<br/>

- �Ʒ� ������ ���ؼ��� ���ڷ����� ������ ������ �� �ִ�

<img src='./images/decorator/decorator01.png'>

- ������ ���̴� �ǹ��� ��Ÿũ����Ʈ��� ���ӿ� ������ ���̾� �ۼ���Ƽ �ǹ��̴�

- �� �ǹ��� �ֵ���� �� ���� �ִ�

- ��, �ֵ���� �߰����� �ʰ� �ǹ� �ϳ� ������ ���� ������ ���� �� �ִµ�

- �ֵ���� �ϸ� �� �ٸ� ������ �߰��� ���� �� �ִ�

- �׸��� �ֵ���� �̰� ���� �ٸ� �͵� �� �� �ִ�

- �̰��� �ٷ� ������ Ȯ���̶�� �Ѵ�

- �� ������ ���ʿ� �ִ� �ǹ��� �������� ���� ä �ֵ�¸� �ؼ� ���ο� ������ �߰��� ���� ���� �ְ� �Ǵ� ���̱� �����̴�

- �̰��� �ٷ� ���ڷ����� ������ ����̶�� �� �� �ִ�

- ��, ���ڷ����� ������ � ��ü�� ������ ������ ��ü�� ���� ���� ���ο� ���� �߰��ϴ� ����̴�

## ���� �ڵ�

- �Ʒ��� ���� �ڵ�� ��Ÿ�� �� �ִ�

```js
class SienceFacility {
  constructor() {
    this.units = ["Science Vessel"];
  }

  // ���� ��ȯ
  getUnits() {
    return this.units;
  }
}
```

- �ڵ带 ���� SienceFacility �ǹ����� �⺻���� Science Vessel ������ ������ �� �ִ�

```js
let sienceFacility = new SienceFacility();
sienceFacility.getUnits(); // ['Science Vessel'];
```

- �̷��� ����ϴ� SienceFacility�� �ռ� ����� �Ͱ� ���� �ֵ���� �߰��ؾ� �ϴ� ��Ȳ�� �����غ���

- �Ʒ��� ���� PhysicsLab�� �������� �� �ִ�

```js
class PhysicsLab {
  constructor(baseBuilding) {
    this.baseBuilding = baseBuilding;
    this.units = ["Battlecruiser"];
  }

  // ���� ��ȯ
  getUnits() {
    return [...this.baseBuilding.getUnits(), ...this.units];
  }
}
```

- PhysicsLab ������ SienceFacility�� ���ڷ� �޾Ƽ� �ٽ� ���� ��ȯ�ϰ� �Ǵµ�

- �� ���� ���� �ٽ� sienceFacility�� �Ҵ�ȴ�

```js
sienceFacility = new PhysicsLab(sienceFacility); // ��ȯ�� ���� �ٽ� sienceFacility�� �Ҵ�ȴ�
sienceFacility.getUnits(); // ['Science Vessel', 'Battlecruiser'];
```

- �� �ٸ� �ֵ���� �Ʒ��� �ִ� ��츦 �����غ���

```js
class CovertOps {
  constructor(baseBuilding) {
    this.baseBuilding = baseBuilding;
    this.units = ["Ghost"];
  }

  getUnits() {
    return [...this.baseBuilding.getUnits(), ...this.units];
  }
}
```

- �Ʒ��� ���� ������ �ν��Ͻ����� �߰��� �� �� �ִ�

```js
let sienceFacility = new SienceFacility();
sienceFacility.getUnits(); // ['Science Vessel'];

sienceFacility = new CovertOps(sienceFacility); // ��ȯ�� ���� �ٽ� sienceFacility�� �Ҵ�ȴ�
sienceFacility.getUnits(); // ['Science Vessel', 'Ghost'];
```

- ���ݱ��� �ۼ��� �ֵ�� ������ �ϴ� Ŭ�������� �ߺ��Ǵ� �ڵ���� ���� ������ ����� ���ؼ� �Ʒ��� ���� �������� �� �ִ�

```js
class AddOnDecorator {
  constructor(baseBuilding) {
    this.baseBuilding = baseBuilding;
  }

  getUnits() {
    return [...this.baseBuilding.getUnits(), ...this.units];
  }
}

class PhysicsLab extends AddOnDecorator {
  constructor(building) {
    super(building);
    this.units = ["Battlecruiser"];
  }
}

class CovertOps {
  constructor(building) {
    super(building);
    this.units = ["Ghost"];
  }
}
```

- ���ڷ����� ������ ������ �����̳�

- �ٷ� ������ ��ü�� ��ȭ�� ���� �ʰ�

- ��ü�� �״�� ������ ä Ȯ���� �ϴ� ����� �����ϴ� �ſ���

### �Լ��� �ڵ忡 �����ϱ�

- �Լ��������� �Ʒ��� ���� �ڵ带 �ۼ��� �� �ִ�

- ��, ��ü������ ���ڷ����� ���ϰ� �Լ��� ���α׷����� �Լ� �ռ��� ����� ������ �ذ��ϴ� ����̶�� �� �� �ִ�

```js
const pipe =
  (...fns) =>
  (X) =>
    fns.reduce((y, f) => f(y), x);

const sicenceFacility = () => {
  const units = ["Science Vessel"];
  return units;
};

const physicsLab = (baseUnits) => {
  const units = ["Battlecruiser"];
  return [...baseUnits, ...units];
};

// ��� ����

// ������ ��� -> �Լ��� �ռ��ϴ� ���
const scienceFacilityWithPhysicsLab = pipe(sicenceFacility, physicsLab)();
// sicenceFacility�� �����
// physicsLab�� �Է����� ����

console.log(scienceFacilityWithPhysicsLab);
```

### Ŭ���� ���̾�׷�

- �Ʒ��� ���ڷ����� ���Ͽ� ���� Ŭ���� ���̾�׷� �����̴�

<img src='./images/decorator/decorator08.png'>

- ���� Strings Ŭ������ ���ڿ� ���� �迭�� ���ؼ� ���� �� ������ �ֱ��� ����� ����� �Ǵ� ���빰�� �ش��Ѵ�

- Decorator Ŭ������ �� Strings�� ���� ����� ��Ÿ���� Ŭ�����̴�.

- �� Decorator Ŭ������ ��ӹ޴� �Ʒ� �� ���� Ŭ������ ��ü���� ����� ������ Ŭ�����̴�.

- �׷��� ���⼭ Strings�� Decorator�� Item�̶�� �߻� Ŭ������ ��ӹް� �ִ�.

- �� Item Ŭ������ �� �� ���� Strings�� Decorator��� ���� �ٸ� ������ �ϳ��� �������� �� �� �ֵ��� �ϴ� ��ġ�� �ȴ�

- ��, Strings�� �ƹ��� ����� ���� ������ ���빰�̰�, �� ���빰�� �������� ����� �߰��ϰ� �Ǵµ�

- ������ ���빰�� ����� Item Ŭ������ ��� �޵��������ν� ���빰�� ��Ŀ� ���� ������ ������� �ȴ�

- �ᱹ�� ���� ��ġ ������ �������� �ٸ� �� �ֵ��� �Ѵ�

- Decorator Ŭ������ ��ӹ޴� �ڽ� Ŭ�������� ��ü������ � ����� �ϴ��� �׸��� ���ؼ� �� �� �ִ�

- ����� ����� �Ǵ� ���� ���빰�� ���� Strings�� �׸�ó�� 4���� ���ڿ��� �����Ǿ� �ִµ� �� �� ����� �����ϴ�

- �� ������ Item Ŭ������ �ִ� print �޼��带 ���ؼ� ����� ������ ���̴�

- �׸��� ��Ŀ� �ش�Ǵ� SideDecorator�� �� ���빰�� ���ؼ� ���ʰ� �����ʿ� ������ ���ڸ� �ٿ��ִµ� ���⼭�� �ֵ���ǥ�� �ȴ�

- �׸��� BoxDecorator�� ������ ���빰�� ������ ���δ� ��� ���ڸ� ������� �ٿ��ش�

- �׸��� LineNumberDecorator�� ���ڿ� �տ� ���� ��ȣ�� �ٿ��ش�

## Ŭ���� ���̾�׷� ����

- ���� Item Ŭ������ �߰����ش�

- �� Ŭ������ �߻� Ŭ�����̰� �� 4���� �߻� �޼ҵ�� �� ���� �Ϲ����� �޼��尡 �����Ѵ�

```ts
// Item.ts

export default abstract class Item {
  abstract getLinesCount(): number; // ���ڿ��� �� �������� ��ȯ�Ѵ�
  abstract getLength(i: number): number; // index�� ���ڷ� �޾�, ������ index�� �ش��ϴ� ���ڿ��� ���̸� ��ȯ�Ѵ�
  abstract getMaxLength(): number; // ���� �� ���ڿ��� ���̸� ��ȯ�Ѵ�. �׸����� ���� �� ���忡�� ���� �� ���ڿ��� ���̸� ��ȯ�ϴ� ��
  abstract getString(i: number): string; // index�� ���ڷ� �޾� ������ index�� �ش��ϴ� ���ڿ��� ��ȯ�Ѵ�

  print(dom: HTMLElement): void {
    const result = [];
    const cntLines = this.getLinesCount();

    for (let i = 0; i < cntLines; i++) {
      const string = this.getString(i); // ������� ���ڿ��� ���´�
      result.push(string);
    }
    dom.innerHTML = result.join("\n");
  }
}
```

- getLinesCount�� ��� ���ڿ��� �� �������� ��ȯ�Ѵ�

  - Strings�� ��� 4

  - BoxDecorator�� ��� 6�� �ȴ�

- ������ Strings Ŭ������ �� Ŭ������ ��ĵ� ���� ���빰�� �ش��Ѵ�

- Item Ŭ������ ��ӹ޴´�

```ts
// Strings.ts

import Item from "./Item";

export default class Strings extends Item {
  private data = new Array<string>(); // ���ڿ��� ������ �迭 �ʵ�

  constructor() {
    super();
  }

  // data �ʵ忡 ���ڿ��� �߰��ϴ� �޼���
  add(str: string): void {
    this.data.push(str);
  }

  // �θ� Ŭ������ �߻� �޼��带 ����
  getLinesCount(): number {
    return this.data.length;
  }

  getLength(i: number): number {
    return this.data[i].length;
  }

  getMaxLength(): number {
    let maxLength = 0;
    this.data.forEach((item) => {
      if (item.length > maxLength) maxLength = item.length;
    });

    return maxLength;
  }

  getString(i: number) {
    return this.data[i];
  }
}
```

- �׸��� ���� ����� Ȯ�����ִ� ��Ŀ� ���� Decorator Ŭ������ �߰����ش�

```ts
// Decorator.ts

import Item from "./Item";

export default abstract class Decorator extends Item {
  constructor(protected targetItem: item) {
    super();
  }
}
```

- Item Ŭ������ ��ӹް� ����� �ؾ� �� ����� �Ǵ� �ʵ尡 �ʿ��ѵ� �̸� �����ڸ� ���ؼ� �߰����ش�

- ����� ����� �Ǵ� targetItem Ÿ���� Item�ε� Item ��ӹ޴� ��� Ŭ������ ���ؼ� ����� �� �� �ִ�

- ��, ����� ���� ���빰�� �ش��ϴ� Strings�� ���ؼ��� �� �� �ְ� ��Ŀ� ���ؼ��� ����� �� �� �ִٴ� ���̴�

- Item ���� �߻� �޼������ ���� �������� �ʾ����Ƿ� �� Ŭ������ �߻� Ŭ������ �����Ѵ�

- ���� Decorator Ŭ������ ��ӹ޴� SideDecorator Ŭ������ �߰��Ѵ�

```ts
// SideDecorator.ts

import Decorator from ".//Decorator";
import Item from "./Item";

export default class SideDecorator extends Decorator {
  constructor(targetItem: Item, private ch: string) {
    super(targetItem);
  }

  // �θ� Ŭ������ �߻� �޼��带 ����
  getLinesCount(): number {
    // ��� ����� �Ǵ� targetItem�� ���ڿ� ����
    return this.targetItem.getLinesCount();
  }

  getLength(i: number): number {
    return this.targetItem.getLength(i) + this.ch.length * 2; // ���ʰ� �����ʿ� ���� ���ڰ� �� 2���̹Ƿ�
  }

  getMaxLength(): number {
    return this.targetItem.getMaxLength() + this.ch.length * 2; // ���ʰ� �����ʿ� ���� ���ڰ� �� 2���̹Ƿ�
  }

  // ��� ����� �Ǵ� ���ڿ��� �¿쿡 �߰����ش�
  getString(i: number) {
    return (
      `<span style='color:gray'>${this.ch}</span>` +
      `${this.targetItem.getString(i)}` +
      `<span style='color'gray'>${this.ch}</span>`
    );
  }
}
```

- ������ BoxDecorator Ŭ�����̴�

```ts
// BoxDecorator.ts

import Decorator from ".//Decorator";
import Item from "./Item";

export default class BoxDecorator extends Decorator {
  constructor(targetItem: Item) {
    super(targetItem);
  }

  // �θ� Ŭ������ �߻� �޼��带 ����
  getLinesCount(): number {
    // ��� ����� �Ǵ� targetItem�� ���ڿ��� �� �Ʒ��� ���ڿ��� �߰��Ǳ� ����
    return this.targetItem.getLinesCount() + 2;
  }

  getLength(i: number): number {
    return this.targetItem.getLength(i) + 2; // ���ʰ� �����ʿ� �ڽ� ������ �ϴ� ���� ���ڰ� �߰� �Ǳ� ����
  }

  getMaxLength(): number {
    return this.targetItem.getMaxLength() + 2; // ���ʰ� �����ʿ� �ڽ� ������ �ϴ� ���� ���ڰ� �߰� �Ǳ� ����
  }

  getString(i: number) {
    const maxWidth = this.getMaxlength();

    // ó���� ������ ���ڿ��� ���ؼ��� ���м��� �߰����ش�
    if (i === 0 || i === this.getLinesCount() - 1) {
      return `<span style='color:yellow'>+${"-".repeat(maxwidth - 2)}+</span>`;
    } else {
      // �������� ���ؼ��� �¿쿡 '|' �� ���� �������� �߰����ش�
      return (
        `<span style='color:gray'>|</span>` +
        `${this.targetItem.getString(i - 1)}${" ".repeat(
          maxWidth - this.getLength(i - 1)
        )}` +
        `<span style='color'gray'>|</span>`
      );
    }
  }
}
```

- ���������� LineNumberDecorator Ŭ�����̴�

```ts
// LineNumberDecorator.ts

import Decorator from './/Decorator';
import Item from './Item';

export default class LineNumberDecorator extends Decorator {
  constructor(targetItem: Item) {
    super(targetItem);
  }

  // �θ� Ŭ������ �߻� �޼��带 ����
  getLinesCount(): number {
    return this.targetItem.getLinesCount();
  }

  getLength(i: number): number {
    return this.targetItem.getLength(i) + 6; // ���ʿ� ���γѹ��� �߰��ϱ� ���� �����ϴ� ������ 6�̱� ����
  }

  getMaxLength(): number {
    return this.targetItem.getMaxLength() + 6; // ���ʿ� ���γѹ��� �߰��ϱ� ���� �����ϴ� ������ 6�̱� ����
  }

  getString(i: number) {
    // �������� ���ؼ��� �¿쿡 '|' �� ���� �������� �߰����ش�
    return '<span style='color:green'>'
    + `${i}`.padStart(4, '0') // ���� ���ڿ� �տ� ���� �ѹ��� �߰����ش�
    + `</span> <span style='color:dimgray'>: </span> ${this.targetItem.getString(
        i
      )}`;
  }
}
```

- �̷��� �ۼ��� �ڵ带 �Ʒ��� ���� ����� �� �ִ�

```ts
import Strings from "./Strings";
import SideDecorator from "./SideDecorator";
import LineNumberDecorator from "./LineNumberDecorator";
import BoxDecorator from "./SideDecorator";

const strs = new Strings();
strs.add("Hello");
strs.add("My Name is Kim");
strs.add("I am Developer");
strs.add("This is Decorator Pattern");

const domPre = document.querySelector("pre");
// strs.print(domPre); // ������ DOM�� �ؽ�Ʈ�� �߰��ȴ�

// �Ʒ�ó�� ����� �߰����� �� �ִ�
const d1 = new SideDecorator(strs, '"');
// d1.print(domPre);

const d2 = new LineNumberDecorator(strs);
// d2.print(domPre);

const d3 = new BoxDecorator(strs);
d3.print(domPre);
```

- �̷��� ������ ����� �߰��� �� ���� �� �ƴ϶�

- ����� �߰��� �Ϳ� �� �ٽ� ����� �Ʒ�ó�� �߰��� �� �ִ�

```ts
const d1 = new SideDecorator(strs, '"');

const d2 = new LineNumberDecorator(d1);

const d3 = new BoxDecorator(d2);
d3.print(domPre); // ����� �߰��� ������ ���� ����Ǹ� �� ����� �߰��� ����� ��µȴ�
```

---

## Reference

- [����� ��� �ø���(�ϻ��� ���������� 1��)](https://www.youtube.com/watch?v=-qb5LCg2KyM&list=PLBQiFVHp3AamzWP7_f-rz9Fmun4DOGRIW&index=1)
- [����Ʈ���忡 ���������� �����3 - ���ڷ����� ���� (decorator pattern)](https://www.youtube.com/watch?v=PU9sr-q8bys)
- [TypeScript�� ���� GoF�� ������ ����: 8. Decorator](https://www.youtube.com/watch?v=nND1rvT-PtQ&list=PLe6NQuuFBu7H3sFnErshsfgNPE9dOZZrx&index=8)
