## Singleton Pattern

- Ư�� Ŭ������ �ν��Ͻ��� ���� �Ѱ��� �����Ѵٴ� ���� �����ϴ� ��������

- Singleton ������ ����� Ŭ������ �ν��Ͻ��� �̸� ������ ���ų� ���� �� �����ϴ� ���� �����ϴ�

- �ٸ� Ŭ�����鿡�� �̱������� ������ ��ü�� ������ ���� ������ ������ �� �� ����.

- �Ʒ��� ���� ��쿡 �ʿ��ϴ�

  - �ϳ��� ������Ʈ�� ���ҽ��� ���� �����ϰų�

  - �ش� ������Ʈ�� �ܺ� ��Ʈ��ũ�� ����Ǵµ�, �� �ϳ��� �־�� �ϴ� ���

<br/>

- �ڵ�� ��Ÿ���� �Ʒ��� ���� �ۼ��� �� �ִ�

```js
class Singleton {
  static instance;

  constructor() {
    if (!Singleton.instance) {
      Singleton.instance = this;
    }
    return Singleton.instance;
  }
}

// ��� ���� ������Ʈ�� ����Ų��
const s1 = new Singleton();
const s2 = new Singleton();

console.log("is same ?", s1 === s2); // true
```

- �Ʒ�ó�� static �Լ��� �ν��Ͻ��� ������ �� �ֵ��� �ۼ��� �� �ִ�

```js
class Singleton {
  constructor() {
    if (Singleton.instance) {
      return console.warn('Warning: Singleton Class already instantiated');
    }

    Singleton.instance = this;
    this.version = Date.now();
    this.config = 'test';
  }

  static getinstance() {
    if (!this.instance) {
      this.instance = new Singleton();
    }

    return this.instance;
  }
}
// 1. �ϳ��� ��ü �ν��Ͻ��� ����
// 2. static �Լ��� ��ü ���� (private�� constructor ������ ����)

const s1 = Singleton.getInstance()
const s2 = Singleton.getInstance()

console.log(s1 ==== s2) // true

```

- �Ʒ��� Singleton���� ������ ����� Ŭ������ �����̴�

```js
class SingletonCat {
  static instance;

  constructor(name) {
    this.name = name;

    if (!Singleton.instance) {
      Singleton.instance = this;
    }
    return Singleton.instance;
  }

  speak() {
    console.log(this.name);
  }
}

// ��� cat1����, cat1�� �ι� ��µȴ�
// ��, �̹� 'cat1'�� ���޵� �ν��Ͻ��� ������� �ֱ� ������ �ߺ��Ǿ ����� �� �� ����
const cat1 = new SingletonCat("cat1");
const cat2 = new SingletonCat("cat2");
console.log("is same ?", cat2 === cat2); // true
```

- Ÿ�Խ�ũ��Ʈ�� �Ʒ�ó�� �ۼ��� ���� �ִ�.

```js

export default class Singleton{
  private constructor(){}

  private static instance : Singleton | undfined

  static getInstance():Singleton{

    if(this.instance ===  undefined) this.intance = new Singleton()

    return this.instance

  }
}


const s = Singleton.getInstance()


```

- `export default new Obj`�� ���� �� �ϳ��� �ν��Ͻ��� �����ǵ��� �ص� �̱��� ������� �۵��� �� ������

- �ش� ����� ���α׷� ���� �߿� �ش� ��ü�� ������ ����� ������� �������� ������ �����ȴٴ� ������ �ִ�.

- ������ �̱��� ������ �����ϸ� �̷� �κб����� ��� �����ϴ�

---

## Reference

- [Singleton ����](https://patterns-dev-kr.github.io/design-patterns/singleton-pattern/)
- [����������, Singleton Pattern, �̱��� ����](https://www.youtube.com/watch?v=-oy5jOd5PBg)
- [�����ڰ� �˾ƾ��� ���������� | ep1. Singleton Pattern | �ڹٽ�ũ��Ʈ �̱��� ����](https://www.youtube.com/watch?v=M4q3sY81gR8)
- [TypeScript�� ���� GoF�� ������ ����: 9. Singleton](https://www.youtube.com/watch?v=70zEzNF1NHU)
