## Prototype Pattern

- ������Ÿ�� ������ ��ü�� �����ϴ� ����

- �Ϲ������� ��ü�� ������ Ŭ���� �̸� �տ� new Ű���带 ����ؼ� �����ǳ�

- ������ ������Ÿ�� ������ ��� new�� Ŭ������ �̸��� �ƴ� ������ ��ü�� ���ؼ� �� �ٸ� ��ü�� �����Ѵ�

- �̶� ������ü�� ����� ��ü�� ���� ��� ���°��� �������� �ʰ� �������̴�

- ���� ��ü�� ���� ���� ���� ���� ������� ���纻�� ���°����� �Ҵ��ؾ� �Ѵ�

- ��, ���� �߿� ������ ��ü�� �ٸ� ��ü�� �����ϴ� ��������

- Prototype���� ���� ��ü�� ���¸� �����ص� ���� ��ü�� ���´� ������� �ʵ��� �ؾ� �Ѵ� (Deep Copy: ���� ����)

## ���̾�׷�

- �Ʒ��� ������Ÿ�� ������ ���̾�׷��� �����̴�

<img src='./images/prototype/prototype02.png'>

- ���� Prototype �������̽��� ������Ÿ�� ������ ���� �������̽��� �̹� ������ ��ü�κ��� �� �ٸ� ���ο� ��ü�� �����س��� copy��� api�� ������ �ִ�

- �׸��� Shape �������̽��� ������ ���� api�� ������ �ִµ�, ��ü���� ������ ���� Ŭ������ Porint, Line, Group �̴�.

- �� �� ���� Ŭ������ ��� Prototype�� Shape �������̽��� �����ϰ� �ִ�.

- Point Ŭ������ Ư�� ������ ���� ����Ʈ ��ǥ�� �ǹ��ϰ�

- Line�� �� ���� ����Ʈ ��ü�� ǥ���Ǵ� ���� �ǹ��Ѵ�

- �׸��� Group�� Shape �������̽��� ��ü�� ���� �� ���� �� �ִ� Ŭ������

- Shape �������̽��� ȣȯ�Ǵ� ���� ���� ��ü���� �̿��ؼ� ���ο� ������ �����ϱ� ���� Ŭ�����̴�

## ���̾�׷� ����

- �Ʒ� �ڵ带 ���ؼ� �簢���� ���� ������ Group Ŭ������ �̿��ؼ� ����� �� ���̴�

- �� �簢���� ���� ��ü���� Ŭ������ �������� ������ Point�� Line Ŭ������ ��ü�� �����ؼ� ���ο� �簢�� ��ü�� ������ �� �� �ִ�.

```ts
// Prototype �������̽�

export default interface Prototype {
  copy(): this;
}
```

- ���ο� ��ü�� �����ؼ� �����ϴ� �޼ҵ�� ��ȯŸ���� this �̴�.

- �̷��� �ϸ� �ڱ� �ڽ��� Ÿ�Կ� ���� ��ü�� ��ȯ��ų �� �ִ�

```ts
// Shape �������̽�

export default interface Shape {
  draw(canvas: HTMLCanvasElement): this; // ���� ĵ������ �׷��ִ� �޼ҵ�
  moveOffset(dx: number, dy: number): this; // ���� ��ü�� ������ �Ÿ���ŭ �̵������ִ� �޼���
}
```

- �� �޼��� ��� ��ȯ���� ��ȯ Ÿ���� this ����ߴµ� �̷��� �ϸ� �Ʊ� �ڽſ� ���� Ÿ������ ��ü�� ��ȯ��ų �� �ִ�

- �켱 Point Ŭ������ �����Ѵ�

```ts
// Point.ts

import Prototype from "./Prototype";
import Shape from "./Shape";

export default class Point implements Prototype, Shape {
  // x, y �ʵ�, �����ڸ� ���ؼ� �߰�
  constructor(private x: number, private y: number) {}

  // x, y �ʵ带 ������ �� �ִ� �޼���
  setX(v: number) {
    this.x = v;
    return this;
  }

  setY(v: number) {
    this.y = v;
    return this;
  }

  // x,y �ʵ� ���� ���� �� �ִ� �޼���
  getX(v: number) {
    return this.x;
  }

  getY(v: number) {
    return this.y;
  }

  // ���ο� Point ��ü�� x, y ������ ���ڸ� �����ؼ� �����ϰ� �� ����� ��ȯ
  copy(): this {
    const result = new Point(this.x, this.y);
    return result as this;
  }

  draw(canvas: HTMLCanvasElement): this {
    const context = canvas.getContext("2d");
    context.beginPath();
    context.arc(this.x, this.y, 4, 0, 2 * Math.PI); // x,y��ǥ�� ���� �׷��ش�
    context.fill();

    return this;
  }

  // x,y ��ǥ�� ���ڷ� ���� �̵� �Ÿ���ŭ �����ش�
  moveOffset(dx: number, dy: number): this {
    this.x += dx;
    this.y += dy;
    return this;
  }
}
```

- ������ Line Ŭ�����̴�

```ts
// Line.ts

import Prototype from "./Prototype";
import Shape from "./Shape";
import Point from "./Point";

export default class Point implements Prototype, Shape {
  // ���� �������� ������ ���� �ʵ� �߰�
  private pt1: Point;
  private pt2: Point;

  constructor(pt1: Point, pt2: Point) {
    // ���ο� Point ��ü ����
    // ���纻�� ������, ���� �����ص� ������ ������ ���� �ʴ´�
    this.pt1 = pt1.copy();
    this.pt2 = pt2.copy();
  }

  // �������� ������ ���� �ʵ尪�� ������ �� �ִ� �޼���
  setPt1(pt: Point): this {
    this.pt1 = pt.copy();
    return this;
  }
  setPt1(pt: Point): this {
    this.pt2 = pt.copy();
    return this;
  }

  // �������� ������ ���� �ʵ尪�� ���� �� �ִ� �޼���
  getPt1() {
    return this.pt1;
  }

  getPt2(v) {
    return this.pt2;
  }

  // �����ؾ��ϴ� �������̽� �޼��� ����
  copy(): this {
    const result = new Line(this.pt1, this.pt2); // �������� ���� ��ü ����
    return result as this;
  }

  draw(canvas: HTMLCanvasElement): this {
    const context = canvas.getContext("2d");
    context.beginPath();
    context.moveTo(this.pt1.getX(), this.pt1.getY());

    // �������� ������ �̿��ؼ� ���� �׷��ش�
    context.lineTo(this.pt2.getX(), this.pt2.getY());
    context.stroke();
    return this;
  }

  moveOffset(dx: number, dy: number): this {
    // ���� �����ϴ� �������� ������ ��ġ�� �̵������ְ� �ִ�
    this.pt1.moveOffset(dx, dy);
    this.pt2.moveOffset(dx, dy);

    return this;
  }
}
```

- �׸��� ������ Group Ŭ�����̴�

```ts
// Group.ts

import Prototype from "./Prototype";
import Shape from "./Shape";
import Point from "./Point";

export default class Group implements Prototype, Shape {
  // Group Ŭ���� ��ü�� ���� �� �ִ� Shape ��ü���� ������ �� �ִ� �迭
  private shapes = new Array<Shape | Prototype>();

  // Group�� �����ϴ� ���� ��ü�� �߰��ϴ� �޼���
  add(shape: Shape | Prototype): this {
    this.shape.push((shape as Prototype).copy()); // ���� ����
    return this;
  }

  // �����ؾ��ϴ� �������̽� �޼��� ����
  copy(): this {
    const result = new Group();

    // ���� ������ �����ϴ� ��� �������� ���ο� Group�� add �޼��� ȣ���ؼ� �߰��Ѵ�
    this.shapes.forEach((shape) => {
      result.add(shape);
    });
    return result as this; // ���ο� Group ��ü ��ȯ
  }

  draw(canvas: HTMLCanvasElement): this {
    this.shapes.forEach((shape) => {
      (shape as Shape).draw(canvas);
    });
    return this;
  }

  moveOffset(dx: number, dy: number): this {
    this.shapes.forEach((shape) => {
      (shape as Shape).moveOffset(dx, dy);
    });
    return this;
  }
}
```

- �׸��� �Ʒ�ó�� canvas�� �������ش�

```html
<canvas width="600" height="600"> </canvas>
```

- ���� ������ �ۼ��� Ŭ�������� ����ؼ� canvas�� �׸� �� �ִ�

```ts
// index.ts

const domCanvas = document.querySelector("canvas");

const pt1 = new Point(100, 100);
pt1.draw(domCanvas);

const pt2 = new Point(200, 100);
pt2.draw(domCanvas);

// �� �� ���� �̿��� �ϳ��� �� ��ü ����
const line1 = new Line(pt1, pt2);
line1.draw(domCanvas);

// �簢�� ���� ����, �װ��� ������ �����Ǿ� �ִ�
const pt3 = new Point(200, 200);
const pt4 = new Point(100, 200);

const line2 = new Line(pt3, pt3);
const line3 = new Line(pt3, pt4);
const line4 = new Line(pt4, pt1);

const rect = new Group(); // Group Ŭ���� ��ü ����

rect.add(line1).add(line2).add(line3).add(line4); // ������ ���� �߰�
rect.draw(domCanvas);

// �̷��� ������ ���簢���� �����ؼ� ���ο� ��ü�� �����Ѵ�
const cloneRect = rect.copy();
// �׸��� ������ ��ü�� �� ���, ������ ������ ���� �ʴ´�
cloneRect.moveOffset(200, 200);
cloneRect.draw(domCanvas);
```

- �����غ��� Prototype ������ ��ü�� Ŭ������ �̸����� �������� �ʰ� ���� �߿� ������ ��ü�� ���ؼ� ������ ���°����� ���ο� ��ü�� �����ϴ� ����

- ���� ��ü�� ���� ���� ���縦 ����ؼ� ���濡 ���� ���� �ٸ� �ʿ� ������ ���� �ʴ´�.

- �ƿ﷯ �ռ� �� �������� �簢�� ����� ��ü�� ������ ���Ҵµ� Ŭ���� ���̾�׷������� �� �簢���� ���� Ŭ������ ��𿡵� �������� �ʴ´�

- ��� ������ Ŭ�������� ��ü�� �����ؼ� �簢�� ��ü�� �����ߴµ� ��ó�� ���ο� Ŭ������ �߰����� �ʰ� ���ο� ������ ��ü�� ������ �� �ִ� ��Ȳ�� ����� �� �� �ִٸ�

- Prototype ������ �����ؼ� �������� ũ�� ������ �� �ֽ��ϴ�

## Prototype in JavaScript

- �Ʒ�ó�� �迭�� 1000�� �ִٰ� �����غ���

```js

const arr1 = []
const arr2 = []

...

const arr1000 = []

arr1.push()
arr2.push()
arr1000.push()

```

- ������ �迭�� push��� �Լ��� ȣ���ϰ� �ִµ�

- arr1 ���� arr100���� ��� �迭 ��ü�� push �Լ��� �ְ�

- �� �ܿ��� �� �迭 ��ü�� �迭 ��ü�� ���� �� �ִ� ���� �޼ҵ尡 �ִ�

- �̷��� �Ǿ��� ���� ������ ��ü���� �Լ��� �ߺ��ǰ� ���� �� �ִٴ� ���̰�

- �� ��� �޸� ����� �̾��� �� �ִ�

- �׷��� �ڹٽ�ũ��Ʈ�� Array.prototype ��� ��ü �ȿ�

- ���� ������ �迭��ü�� �ߺ����� ���� �� �ִ� ��ü���� ��� �� ��Ƶд�

- �׷��� �� �迭 ��ü�� push �Լ��� ȣ���ϸ�, ������ ��ü�� push �Լ��� �����ϰ� ȣ���ϴ� ���� �ƴ϶�

- Array.prototype ��ü �ȿ� �ִ� push �Լ��� ȣ���ϴ� ���̴�

- �� ����� ���� ��� ��ü�� �Լ��� �����ϴ� �� ���� ȿ�����̴�

- �ڹٽ�ũ��Ʈ�� �̷��� ������Ÿ���̶�� ������ ������ ������ �Ǿ� �ִ�.

- Prototype (���� �Ǵ� ��)

  - ��, ������Ÿ���� ��� ��ü�� �������� ���(�Լ�)�� �����̶�� ������ �� �ִ�

- �Լ����� ������Ÿ���� �ִ� (Function.prototype)

- �Ʒ� �ڵ带 ���� bind �Լ��� ����Ѵ�

```js
const foo = () => console.log("hello world");
foo.bind();
```

- ���� �Լ��� ��ü�ΰ��ϰ� ������ �� �ִµ�

- �Լ��� prototype�� �ִ� bind�� ȣ���ϴ� ���̴�

- �ڹٽ�ũ��Ʈ�� �ִ� ��ü���� �̷��� ������Ÿ���� ������ �ִٰ� �� �� �ִ�

- �׸��� �Ʒ�ó�� �̷��� ������Ÿ���� �ֻ��� ������Ÿ�� Object.prototype�̶�� ���� ������Ÿ������ ������

<img src='./images/prototype/prototype01.png'>

- ������ �Ʒ�ó�� ������Ÿ�Կ� ������ �� �ִ�

```js
const foo = () => console.log("hello world");

foo.__proto__; // Function ������Ÿ��
foo.__proto__.__proto__; // �ֻ��� Object ������Ÿ��
```

- �Ʒ��� ���� prototype�� ����� �� �ִ�

```js
// ������ �Լ�
function MyClass(value) {
  this.value = value;
}

MyClass.prototype.getValue = function () {
  return this.value;
};
MyClass.prototype.setValue = function (value) {
  return this.value;
};

new MyClass(111).getValue();
new MyClass(112).getValue();
new MyClass(113).getValue();
```

- new MyClass()�� ���ؼ� ��ü�� �����ϰ� getValue()�� ȣ���Ѵ�

- 3���� ��ü�� ������� ������ getValue()�� 3����� ������ �� ������

- MyClass�� prototype�� getValue()�� �����߱� ������ getValue()�� ������ �޸� �ּҸ� ����Ű�� �ȴ�

- �׸��� ������Ÿ���� ����ؼ� ����� ������ ���� �ִ�

```js
function p() {}

p.prototype.getname = function () {
  return this.name;
};

function c(age) {
  this.age = age;
}

c.prototype = Object.create(p.prototype);
// Object.create ����ϸ� ������Ÿ���� ������ �� �ִ�
```

- ������ �̷��� �ڵ带 �ۼ��ϴ� ���� �ͼ����� �ʰ� ������ �� �ֱ� ������ �Ʒ�ó�� prototype ��� class�� extends�� ����ؼ� ����� ǥ���� �� �ִ�

```js
class P {
  constructor() {
    this.name = "parent";
  }

  getname() {
    return this.name;
  }
}

class C extends P {
  constructor(age) {
    super();
    this.age = age;
  }
}
```

- ���� ����Ʈ, ��, ����Ʈ�� ���� �����ӿ�ũ�鿡�� �ڵ带 �ۼ��ϴ� ���� ���� Ŭ������ ���򿡴� ���Ⱑ ��ƴ�

- ������Ʈ ������ �ڵ带 �ۼ��Ѵ�

- ��, Component ������ Function ������� �� �� �ִ�

---

### Reference

- [�ڹٽ�ũ��Ʈ�� �����ϴ� ������ ���� (�ϻ��� ���������� 3��)](https://www.youtube.com/watch?v=geHtO_whED8&t=1)
- [TypeScript�� ���� GoF�� ������ ����: 18. Prototype](https://www.youtube.com/watch?v=tLf6Yh_y3LM)
