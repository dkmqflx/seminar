## Factory Pattern

- 팩토리는 **객체**를 찍어낼 수 있는 (반환하는) 공장이다.

- 그 공장은 함수가 될 수도, 클래스가 될 수도 있다.

- 아래 코드는 팩토리 함수를 사용하는 예시로 Animal을 상속하는 Cat과 Dog라는 클래스가 있다

- 팩토리 함수를 만들고 고양이 또는 강아지를 만들어 달라고 인자를 전달하면 해당 인스턴스를 만들어준다

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

// 팩토리 함수
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

- 팩토리를 클래스로 만들 수도 있다

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

<br/>

### 장점

- 팩토리 패턴의 장점으로는

- 복잡한 오브젝트들의 생성 과정을 클라이언트가 직접 다룰 필요가 없다

- 복잡한 과정을 모두 팩토리에 숨겨놓고 클라이언트는 간단하게 팩토리에 오브젝트를 만들어달라고 요청하면, 팩토리는 요청한 인스턴스를 리턴해준다

---

## Reference

- [Design pattern, Factory Pattern, 디자인패턴, 팩토리 패턴](https://www.youtube.com/watch?v=AmwEIt0vhxA&t=1s)
