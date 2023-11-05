## Factory Pattern

- ���丮�� **��ü, �Լ�, ��**�� �� �� �ִ� (��ȯ�ϴ�) �����̴�.

- �� ������ �Լ��� �� ����, Ŭ������ �� ���� �ִ�.

- Ư�� ��Ȳ�� � **��ü, ��, �Լ�**�� ��������� ���� ��ü���� ������ �߻�ȭ�ϰ�

- ���õ� ������ �ϳ��� �Լ�(���丮)�� ������ �� �ְ� �����ش�

- �������, Component A, Component B, Component C�� �ִٰ� ����

- �� �� � ���� ��� �־�޶�� �䱸�� �ִٰ� ����

- �� ������Ʈ���� ������ �� ������ ���뼺�� ���� ������ ���� ���� ���� ������ ���� �� ������Ʈ���� import �ؼ� ����Ѵٰ� ����

```js
import { sumDecimal } from "...";
```

- ���⿡ �߰������� 2���� ���� ����� �߰��ȴٰ� ����

- �׷��ٸ� �Ʒ�ó�� �� ������Ʈ���� import �ؼ� ���ǹ��� ���ؼ� ó���� �� �ִ�

```js
import { sumDecimal } from '...';
import { sumBinary } from '...';

...

if(������){
  sumBinary(...)
}else{
  sumDecimal(...)
}
```

- ���⼭ �� �ٸ� �䱸������ 8���� ���� ����� �߰��ȴٰ� ����

- �׷��ٸ� ������ ó�� 8���� ������ ���� �Լ��� �����ϰ� ������Ʈ���� import ���� ������ ���ǹ��� �ش� �б⸦ �߰����־�� �Ѵ�

- �׷��ٺ��� �̰� �³�? ��� ������ �� ���� �ִµ� �� ���� �ٷ� ���丮 ������ ������ �����̴�

- **����� ����**�� �ϴ� �͵��� **����**�� ���� ���̰� ���� ��Ȳ�̴�

- �̸� ������ �Լ��� �и����� �� �ִµ�, �� �Լ��� �ٷ� ���丮�� �ȴ�

- �ٷ� �Ʒ�ó�� ���丮 ������ ������ �� �ִ�

- Factory ������ �ϴ� �Լ��� �����ϰ� �ش� �Լ��� �ʿ��� ������ import �ؼ� ����Ѵ�

<img src='./images/factory/factory01.png' >

- ��������� ���丮 ������ �Ʒ��� ���� ������ ������ �ǰ�

- �������� ���� ������ ���� �ȴ�

<img src='./images/factory/factory02.png' >

- �Ʒ�ó�� �ܼ��� ���ǿ� ���� �б� ó���� �� �� �ƴ϶� �����ο� ���� ��� ���Ǹ� �����ϴ� �뵵�� ��� �����ϴ�.

<img src='./images/factory/factory03.png' >

- �� �ٸ� ���÷δ� �Ʒ�ó�� Custom Hook�� ����ؼ� ���ǿ� ���� ó���� �� ���� �ִ�

  - ���⼭�� hook�� ���丮 ������ �Ѵ�

<img src='./images/factory/factory04.png' >

- �Ʒ�ó�� ������Ʈ ���ؿ����� ������ �����ϴ�

- �Ʒ� ������ ���� ����ó�� ������Ʈ ������ ���丮�� ����� ���� �ְ�

- ������ ó�� ������Ʈ ���� �Լ� ������ ���丮�ε� ������ �� �ִ�

- ���� Ÿ�Խ�ũ��Ʈ�� ����Ѵٸ� props�� ���� interface�� �����־�� �Ѵ�

<img src='./images/factory/factory05.png' >

- ��, ���丮 ������ ���ǿ� ���� �޶����� ����� �ƶ��� ���� ������ �� ���� ���� ��ų�� �ִ� ���̵��

<br/>

- �Ʒ� �ڵ�� ���丮 �Լ��� ����ϴ� ���÷� Animal�� ����ϴ� Cat�� Dog��� Ŭ������ �ִ�

- ���丮 �Լ��� ����� ����� �Ǵ� �������� ����� �޶�� ���ڸ� �����ϸ� �ش� �ν��Ͻ��� ������ش�

```js
class Animal {
  speak() {
    console.log("speak");
  }
}

class Cat extends Animal {
  speak() {
    console.log("Cat");
    super.speak();
  }
}

class Dog extends Animal {
  speak() {
    console.log("Dog");
    super.speak();
  }
}

// ���丮 �Լ�
const factory = (animal) => {
  if (animal === "Cat") {
    return Cat();
  } else if (animal === "Dog") {
    return Dog();
  }
};

const cat = factory("Cat");
cat.speak();

const dog = factory("Dog");
dog.speak();
```

- ���丮�� Ŭ������ ���� ���� �ִ�

```js
class Factory {
  factory(animal) {
    if (animal === "Cat") {
      return Cat();
    } else if (animal === "Dog") {
      return Dog();
    }
  }
}

const factory = new Factory();

const cat = factory.speak("Cat");

const dog = factory.speak("Dog");
```

- �� �ٸ� ���ô� �Ʒ��� ����

- �Ʒ� �ڵ�� �Ź��� ����� ���丮 Ŭ������ �ִ� ���丮 �����̴�

```js
// �Ź� ���� Base Class
class Shoe {
  constructor(attrs) {
    this._attrs = attrs || {};
  }

  getName() {
    return this_.attrs?.name;
  }

  getSize() {
    return this_.attrs?.size;
  }

  getBrand() {
    return this.constructor?.name; // �ش� Ŭ������ �̸�
    // �Ʒ� Nike Ŭ�������� Shoe Ŭ������ ��� ��
    //  �ش� �Լ� ȣ���ϸ� Nike ��µȴ�
  }
}

// �� Ŭ���� ��� �Ź� ���� Base Class�� ��ӹ޴´�
class Nike extends Shoe {}
class Puma extends Shoe {}
class Adidas extends Shoe {}

const data = [
  { type: "Nike", attrs: { name: "SB", size: 300 } },
  { type: "Puma", attrs: { name: "Jada", size: 290 } },
  { type: "Adidas", attrs: { name: "Super Start", size: 270 } },
  { type: "Nike", attrs: { name: "Airfore", size: 260 } },
];

// Factory Class
class ShowFactory {
  // �� �Ź� �귣���� ���۷����� ���� �ִ�.
  // ���� ������ ����Ѵ� (���丮 ���Ͽ����� ���� ������ �ʿ�)
  // switch ���� ����ϱ⵵ �ϰ� �̷��� mapping�� �� �� �� �ִ�.
  typeMap = {
    nike: Nike,
    puma: Puma,
    adidas: Adidas,
  };

  create(props) {
    try {
      const Brand = this.typeMap[props?.type?.toLowerCase()]; // � Ÿ���� Ŭ������ ������ ������
      return new Brand(props.attrs);
    } catch (e) {
      console.log("error creating new shoes", e);
    }
  }
}

const factory = new ShoewFactory(); // ���丮 �ν��Ͻ� ����

const nike1 = factory.create({
  type: "Nike",
  attrs: { name: "Cortez", size: 265 },
});

console.log(nike1);
console.log(nike1.getBrand());
console.log(nike1.getSize());

// ���丮 ������ map�� ���Ǳ⵵ �Ѵ�
const items = data.map((item) => factory.create(item));
console.log(items);
```

<br/>

- �Ʒ� ���ô� React���� Lazy Loading�� ���丮 ������ �����ϴ� �����̴�

- ���� ������ �ε尡 �ǰ�, ��� ������Ʈ�� ����Ʈ�� ������ �� ��� �ڵ带 �ε��Ѵ�

- �Ʒ� �ڵ忡�� import�� �����ϴ� ����� ������ ����

  - ���⼭ ����ϴ� import�� �츮�� ���� ����ϴ� `import 'module'` �Ǵ� `import * from 'module'` �� �ƴ� `import('module')` �� ǥ�����̴�

  - `import('module')` �� ���� �ÿ� ����� import �ϴ� ���� �ƴ�, ��Ÿ�� �ÿ� �������� ����� import �ϸ�,

  - ���� module�� �ٷ� �����ϴ°� �ƴ϶� �ش� ����� ��� �ִ�(resolve �ϴ�) Promise ��ü�� ��ȯ�Ѵ�

  - �������� ����� �������� ������ �ð��� �ҿ�Ǵ� �����̱� ������ �̰� �񵿱�� ó���ϱ� ����.

  - ��, ������ ���� ����ؾ� �ȴ�

    ```js
    import("module").then((module) => {
      // module�� ����Ͽ� ���ϴ� ����
    });
    ```

  - ������ ���⼭ �� module�� �ܼ� ��ƿ�� �Լ���� ���������, �츮���� ������Ʈ��� import�� �� ������ setState �� �Ͽ� React�� Render Cycle�� ������ �ʿ䰡 �ְ�

  - �� ������ ���� ���ִ� ���� React.lazy�� React.Suspense �̴�

- �׸��� �̸� import�� �صθ�, �̸� �ش� ����� ��Ʈ��ũ�� ���� �ҷ����� �ǰ� (��������� �ʴ���)

- �ٸ� ������ lazy �ȿ��� import�� �� ���� �̹� ��Ʈ��ũ �󿡼��� �ε�� �����͸� �ٷ� ����� �� �����Ƿ� �ð��� ����ȴ�.

- ��, �ܼ��� ������Ʈ�� �������� import �ϸ� Promise�� ���� ������Ʈ ��ü�� ���޵ȴ�.

- �̶� ����Ʈ�� ������Ʈ�� ������ �� ���� Promise�� ���� �޾ƿ� ������Ʈ ��ü�� Promise ������ ������ �մ�.

- �̰��� �����ִ� ���� �ٷ� lazy �Լ��̴�.

- �����ϸ�, ���� �ε��� import �Լ������� ����ϴ�.

- �ٸ�, �񵿱��� ������Ʈ ����������Ŭ ������ ���� �ε��� ������Ʈ�� ����� ���޹ޱ� ���ؼ��� lazy �Լ��� ����ؾ� �Ѵ�.

- ����, ������ lazy�� ����, Promise�� ���� ������Ʈ ��ü�� ������ ���� �Լ��� ���� �����ص� �ȴ� (�̰��� �Ϲ����� API(�񵿱�) �����͸� �ε��ϴ� �Ͱ� ����ϴ�.)

```js
const LazyImageModal = lazy(() => import("./components/ImageModal"));
// Code Splitting�� ���� �ڵ尡 ���ҵǾ� �ִ�

function App() {
  const [showModal, setShowModal] = useState(false);

  useEffect(() => {
    import("./components/ImageModal");
    //import�� �صθ�, �̸� �ش� ����� ��Ʈ��ũ�� ���� �ҷ����� �ȴ� (��������� �ʴ���)
    // �׷��� ������ LazyImageModal�� ����Ʈ �� �� ��Ʈ��ũ�� ���ؼ� �ʿ��� �ڵ带 �ٿ���� �ʾƵ� �ȴ�
  }, []);

  return (
    <div className="App">
      <Header />
      <InfoTable />
      <ButtonModal
        onClick={() => {
          setShowModal(true);
        }}
      >
        �ø��� ���� ����
      </ButtonModal>
      <SurveyChart />
      <Footer />
      <Suspense fallback={null}>
        {showModal ? (
          <LazyImageModal
            closeModal={() => {
              setShowModal(false);
            }}
          />
        ) : null}
      </Suspense>
    </div>
  );
}

export default App;
```

- ������ ���� ������Ʈ�� �̸� import �������� ���� ���� ������Ʈ�� preload ���� �ʿ䰡 �ִٸ� �Ź� import�� ���� ���ִ� ���� ���ŷӴ�

- �� �� ���丮 ������ ����Ѵ�

- ���丮 ������ �����ϸ� �Ʒ��� ���� �ڵ带 ������ �� �ִ�

- �ڵ忡 ���� ������ ������ ����

```js
// �ܼ� ������Ʈ ���� �ε� �ڵ� (�̷��� �ϸ� Suspense �ȿ��� �ش� ������Ʈ�� ���Ǵ� ���� import�� �˴ϴ�.)
const LazyImageModal = lazy(() => import("./components/ImageModal"));

// �� �ڵ常 ���ԵǸ� �ܼ� �ڵ� ���� �� ���� �ε� �ϻ� preloading�� �� �� �ƴ���?
// �׷��� �ش� ������Ʈ�� ���� ���Ǳ� ���� ������ import(��, preload)�ϱ� ���ؼ���
// ������ ������ �ܼ� ������Ʈ�� import�ϴ� �ڵ尡 �ʿ��մϴ�.

// �� �۾��� ���ִ� �ڵ带 ���丮 ���� ���� ������ useEffect���� ���� import ������ ȣ������µ�,
// ������ �̸� import ������ �ش� ������Ʈ�� preload��� Ŀ���� �Ӽ��� ����� �־�����ϴ�.

// �̷��� �ϸ�, useEffect �ȿ��� ���� import�� ȣ���� �ʿ����,
// LazyImageModal.preload();
// �̷��Ը� ȣ���ϸ� �˴ϴ�.
LazyImageModal.preload = () => import("./components/ImageModal");
```

- ��, lazy(() => import()) �� �ϴ� ���� ������Ʈ�� import �Ǵ� ���� �ƴϴ�.

- �ش� �ڵ�� Ư�� ������Ʈ(�Ǵ� ���)�� �������� �ε� �ϰڴٴ� ǥ���̰� ���� ���Ͽ��� ����(�ڵ� ����)�Ѵ�.

- �׸��� Suspense �ȿ��� �ش� ����� ���Ǵ� ���� ���ҵ� �ڵ带 �ε��Ͽ� ������ �ϴ� ����Դϴ�.

- ������, �츮�� ���ϴ°� ���Ǵ� ���� ���ҵ� �ڵ带 �ε��ϴ� ���� �ƴ�, ����ϱ� �� ������ �̸� �ڵ带 �ε��Ͽ� ����ϴ� �������� ������(���ҵ� �ڵ尡 �ٿ�ε�Ǵ� �ð�)���� ����ϰ��� �ϴ� ���̰�

- �װ��� ���ؼ��� �������� ������ �ε��ϴ� �ڵ��� import ������ ���� ȣ�����־�� �Ѵ�.

- �̰��� lazy���� ȣ���ϴ� import�� �ٸ���.

- lazy ���� import ������ �ش� ������Ʈ�� ���Ǵ� ������ �ش� �ڵ带 �ڵ����� �ε��ϱ� ���� ���Ǵ� �ڵ��̰�, ���� ����ϴ� ���� ȣ���ϴ� import�� �ش� ������Ʈ�� �������� ������� �׳� �ε��ϴ� ������.

- �̷����ϸ�, �ش� �ڵ�� �ٿ�ε�ǰ� �ð��� ���� ���� �ش� �ڵ带 ����ϴ� ������ ���� ���� ���� �ٿ�ε� ���� �ʾƵ� ������ �ٿ�ε�� �ڵ带 �״�� Ȱ���� �� �ְԵȴ�.

- ��, �����̰� ������� ��

- �ڵ忡�� LazyImageModal.preload �� import ������ �ִ� ���� �װ��� ���� ���̰�,

- useEffect���� LazyImageModal.preload()�� ȣ���ϴ� ���� preload�� ���� import ������ ���� �����Ͽ� �ڵ带 �̸� �ε��� �α� �����̴�.

- ��ü �ڵ�� ������ ����

```js
// ���丮 ����
// �ܼ� ������Ʈ ���� �ε� �ڵ� (�̷��� �ϸ� Suspense �ȿ��� �ش� ������Ʈ�� ���Ǵ� ���� import�� �ȴ�)
function lazyWithPreload(importFunction) {
  const Component = React.lazy(importFunction);
  // ������ �Ʒ� �ڵ�� ����
  // �ڵ� ���ø��� ����
  // const LazyImageModal = lazy(() => import('./components/ImageModal'));

  // �� �Ʒ� �ڵ�� �����ϴ�
  // const Component = lazy(() => import('./components/ImageModal'));

  Component.preload = importFunction; // () => import('./components/ImageModal')
  // preload��� �Ӽ��� �߰��Ѵ�
  // useEffect hook���� ȣ���ϱ� ���� �߰�

  return Component;
}

const LazyImageModal = lazyWithPreload(() => import("./components/ImageModal"));

function App() {
  const [showModal, setShowModal] = useState(false);

  useEffect(() => {
    LazyImageModal.preload();
    // import('./components/ImageModal')�� �����ϴ�
    // �Ȱ��� �������� ������Ʈ �ε�Ǵ� ���� Ȯ���� �� �ִ�
  }, []);

  return (
    <div className="App">
      <Header />
      <InfoTable />
      <ButtonModal
        onClick={() => {
          setShowModal(true);
        }}
      >
        �ø��� ���� ����
      </ButtonModal>
      <SurveyChart />
      <Footer />
      <Suspense fallback={null}>
        {showModal ? (
          <LazyImageModal
            closeModal={() => {
              setShowModal(false);
            }}
          />
        ) : null}
      </Suspense>
    </div>
  );
}

export default App;
```

<br/>

### ����

- ���丮 ������ �������δ�

- ������ ������Ʈ���� ���� ������ Ŭ���̳�Ʈ�� ���� �ٷ� �ʿ䰡 ����

- ������ ������ ��� ���丮�� ���ܳ��� Ŭ���̾�Ʈ�� �����ϰ� ���丮�� ������Ʈ�� �����޶�� ��û�ϸ�, ���丮�� ��û�� �ν��Ͻ��� �������ش�

- Ŭ���� ���� �������� ���� ������ Ȯ���� ���� ���Ŀ� ���� ������ �����ϴ�

### ����

- ���丮 ������ �������δ�

- �ܼ��� ���ǹ��� �ִٴ� ������ ���� �ٸ� �ƶ��� ���� �������� ���丮ȭ �Ѵٰų�

- ������ �ƶ��� ���� ������ ������ �ϳ��� ���丮�� ���� �������ν� ������ ���丮 ��ü�� �����Ǿ�

- ���丮�� �������� �� ���̵� ����Ʈ�� ������ �� ���� �Ǵ� ���輺�� �ִ�

## Reference

- [Design pattern, Factory Pattern, ����������, ���丮 ����](https://www.youtube.com/watch?v=AmwEIt0vhxA&t=1s)

- [�����ڰ� �˾ƾ��� ���������� | ep2. Factory Pattern | �ڹٽ�ũ��Ʈ ���丮 ����](https://www.youtube.com/watch?v=L78FbkyOcyk)

- [����Ʈ���忡 ���������� �����1 - ���丮 ����(factory pattern)](https://www.youtube.com/watch?v=eSLrZbPHgoI&t=100s)

- [Lazy Loading - ���丮 ����](https://github.com/dkmqflx/web-performance-optimization-inflearn/blob/master/part1/section2.md)
