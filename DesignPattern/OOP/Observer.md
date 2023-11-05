## Observer Pattern

- Observer�� �����ڶ�� ������ � ���°� ����Ǿ��� ���� ������ ���濡 ������ �ִ� �����ڵ鿡�� �˷��ִ� �����̴�

- �׸����� ���� ��ó�� � ���°� �ְ�, ������ ��ȭ�� ������ ���� �ִ� 4���� �����ڰ� �ִ�

- ���°� ��ȭ�Ǹ� �� �����ڿ����� ���°� ���� �ڽ��� ����Ǿ����� �˸��µ� �ʿ信 ���󼭴� ����� ���¿� ���� �����ڿ��� �����Ѵ�

<img src='./images/observer/observer08.png'>

- ������ ������ � ������ ��ȭ�� ���� ó���� ���ؼ� ���ȴ�

- �׸��� ���� ��ȭ�� �߻��ϸ� ���� ��ȭ�� ������ ������ �ִ� ��ü��

- ��, �������鿡�� ���� ��ȭ�� �˸��� �ʿ��ϴٸ� ����� ���°��� �Բ� �����Ѵ�

<br/>

- ����(State)�� ���� �����ϴ� ������(Observer)��� ������ ����

- **���� ��ȭ�� ���� �� �� �����ڰ� ����**�ϵ��� �ϴ� ������ �������� �Ʒ� �׸��� ���� ��Ÿ�� �� �ִ�

- �̷��� ������ ������ ����ϸ� �̺�Ʈ�� �Ͼ�� �� �����ڵ��� �˾����� �� �ְ� �ȴ�

- ������ ������ ������� �ʴ´ٸ� ����, �ź�, �� �ð����� ��û�� ������(polling, �ֱ������� ����) �̺�Ʈ�� �Ͼ���� Ȯ���ؾ� �Ѵ�

  - ���÷�, �˶��� ����� �� ���� �濡 �ִ� ��� ����� �˶��� ��� �����ϴ� �Ͱ� ����

- ��, �̺�Ʈ ����� ����� �����Ҷ� ���ϴ� ����� �������� ������ ����ǵ��� ���ִ� ��Ȱ�� �Ѵ�

  - ������� �˸�, �޼���, UI �̺�Ʈ ��� ��

  - ��ü (�Ǵ� ������Ʈ) �� ���� ��ȭ�� ���� ����ó���� ����� �Ҷ� ���Ǳ⵵ �Ѵ�

<img src='./images/observer/observer01.png'>

- �Ʒ��� ���� �ڵ�� �ۼ��� �� �ִ�

<img src='./images/observer/observer02.png'>

- ���ʿ� �ִ� createObserverState�� �Լ� ���� �ִ� �� �ڵ�� �Ʒ��� ����

- state

  - ���¸� ��� ��

- listeners

  - ���� ��ȭ�� ���� ���� ������ �Լ����� �����ϴ� ��

- subscribe

  - listeners�� �Լ��� ����� �� �ִ� �Լ�

- setState

  - ���¸� ��ȭ��Ű�� �Լ�

  - �Լ� �ڵ带 ���� ��ϵ� listeners�鿡�� ���ο� ���¸� �����ϴ� ����� �� �� �ִ�

- �׸��� �����ʿ� �ִ� �� ó�� subscribe �Լ��� ���� ������ ��⿡�� ���� ��ȭ�� ������ �� �ִ�.

- ���� �Ʒ� �ִ� �ڵ忡�� �� �� �ִ� �� ó��, setState�� ���ؼ� ���� ��ȭ�� �Ͼ��

- listeners�� ��ϵ� �� listener�� ���� �����ڵ鿡�� ���°� ��ȭ������ �˸��� ���̴�

- ��, subscribe�� ���ؼ� listners�� �Լ��� ����� ���ұ� ������

- setState�� ȣ��Ǹ� `observer.observers.forEach(observer => observer(state))`�� ���ؼ� ����� �Լ��� ����Ǵ� ��

<br/>

- �� �ٸ� ���÷� ������ ���� �ڵ尡 �ִ�

- ���� ����� "���� ��ü" (������ ���)

- ������ �ϴ� "���� ��ü" (������)

- ������ü�� �����Ӱ� ������ü�� ���/��� ����� �� �ְ�

- �� ���� ��ü�� ���°� �ٲ�� �ٸ� ���� ��ü�鿡�� ���¿� ������ �˸�

```js
// ������ ���
class Subject {
  constructor() {
    this.observers = [];
  }

  getObserverList() {
    return this.observers;
  }

  // ������ ���
  subscribe(observer) {
    this.observers.push(observer);
  }

  unsubscribe(observer) {
    this.observers = this.observers.filter((obs) => obs !== observer);
  }

  notifyAll() {
    this.observers.forEach((subscriber) => {
      try {
        subscriber.update(this.constructor.name);
      } catch (err) {
        console.error("error", error);
      }
    });
  }
}

// ������
class Observer {
  constructor(name) {
    this.name = name;
  }
  update(subj) {
    console.log(`${this.name}: notified from ${subj} class!`);
  }
}

const subj = new Subject();

// ������ �ν��Ͻ� ����
const a = new Observer("A");
const b = new Observer("B");
const c = new Observer("C");

// ���� ����
subj.subscribe(a);
subj.subscribe(b);
subj.subscribe(c);

console.log(sub.getObserverList());
// ������ �ν��Ͻ��� ��µȴ�

sub.notifyAll();
// `A: notified from Subject class!`
// `B: notified from Subject class!`
// `C: notified from Subject class!`

// ����
subj.unsubscribe(b);

// ������ �� ���� ��µȴ�
sub.notifyAll();
// `A: notified from Subject class!`
// `C: notified from Subject class!`
```

### Ŭ���� ���̾�׷�

- �Ʒ��� ������ ���Ͽ� ���� Ŭ���� ���̾�׷��� �� ���� �����̴�

<img src='./images/observer/observer09.png'>

- DiceGame Ŭ������ �ֻ��� ������ ��Ÿ���� �߻� Ŭ�����̴�

- DiceGame Ŭ������ �ֻ��� ���ӿ� �����ϴ� Player Ŭ���� ��ü�� ������ �߰��� �� �ִµ�

- addPlayer �޼��带 ���ؼ� �߰��� �� �ִ�

- DiceGame�� Player ������ ���� ���� `*`�� Ȯ���� �� �ִµ�

- �̰��� ���� ���� ���� Player ��ü�� DiceGame ��ü�� ���� �� ������ ��Ÿ����

- DiceGame�� �ֻ����� ������ ��, play �޼��带 ȣ���ϸ� �ֻ����� ��ȣ�� ���ӿ� ������ Player ��ü�鿡�� �˸���

- �ֻ����� ������ ���� ���� �ٷ� ���� ��ȭ�� ���� ���̴�

- DiceGame�� �߻� Ŭ�����ε� ������ ������ ���� ���� �����ϴ� ����� �پ��ϰ� �����ϱ� ���ؼ� �̷��� �߻� Ŭ������ �����ߴ�

- FairDiceGame �� DiceGame Ŭ������ ��� �޴µ�, FairDiceGame�� �ֻ����� 1���� 6���� �����ϰ� �������� �����Ѵ�

- �׸��� �� UnfairDiceGame Ŭ������ �����ϰ� �ֻ��� ��ȣ�� �������� �ʰ� Ư���� ��ȣ�� �� ���� ������ �Ұ����� �ֻ��� �������� �� �� �ִ�

  - �ٸ� UnfairDiceGame Ŭ������ �������� �ʴ´�

- Player Ŭ���� ���� �߻� Ŭ�����ε� ������ ���� �ֻ����� ���� ���� ó���ϴ� �ڵ带 �پ��ϰ� �����ϱ� ���ؼ� �߻� Ŭ������ �����ߴ�

- Player Ŭ������ ��ӹ޴� Ŭ������ OddBettingPlayer, EvenBettingPlayer, NBettingPlayer Ŭ�����̴�

- OddBettingPlayer Ŭ������ �ֻ����� ���� Ȧ���� ������ �� ������ �ϰ�

- EvenBettingPlayer Ŭ������ �ֻ����� ���� ¦���� ������ �� ������ �ϰ�

- NBettingPlayer Ŭ������ ������ ���� ������ �Ѵ�

### Ŭ���� ���̾�׷� ����

- ���� Player �߻� Ŭ������ �߰��Ѵ�

```ts
// Player.ts

export default abstract class Player {
  // Player�� �����ϴ� �ڽ� Ŭ�������� ������ �� �ֵ��� protected�� ����
  protected _winning: boolean = false;
  // �ֻ����� ���� ���� �÷��̾ ���߾��� �� true, Ʋ���� �� false�� �ȴ�

  // player�� �̸��� ���� �ʵ带 ������
  constructor(private _name: string) {}

  // �̸��� ��� ���� ������Ƽ
  get name() {
    return this._name;
  }

  // _winning ���� ��� ���� ������Ƽ
  get winning() {
    return this._winning;
  }

  // �ֻ����� ��, ��, ���� ��ȭ�� ���ؼ� �÷��̾ ��� ó�������� ���� �߻� �޼ҵ�
  // diceNumber : ������ ���� �ֻ����� ��
  abstract update(diceNumber: number): void;
}
```

- ������ DiceGame �߻� Ŭ�����̴�

```js
// DiceGame.ts

import Player from './Player'

export default abstract class DiceGame{
  // ������ �����ϴ� �÷��̾���� ������ �� �ִ� �ʵ�
  // protected �����ڷ� �����ؼ� �ڽ� Ŭ���������� �ش� �ʵ忡 ������ �� �ֵ��� �ߴ�
  protected players = new Array<Player>()

  // �÷��̾ �߰��� �� �ִ� �޼ҵ�
  addPlayer(player:Player):void{
    this.players.push(player)
  }

  // �ֻ����� ������ �߻� �޼���
  abstract play():number
}

```

- ������ DiceGame �� ��ӹ޴� FairDiceGame Ŭ�����̴�

```js
// FairDiceGame.ts

import DiceGame from "./DiceGame";

export default class FairDiceGame extends DiceGame {
  // �θ� Ŭ���� ���ǵǾ� �ִ� �� �߻� �޼��带 ����
  play(): number {
    const n = Math.floor(Math.random() * 6) + 1; // 1���� 6���� ���� ��´�

    // players �ʵ忡 �߰��� �÷��̾� ��ü���� �ϳ��� ��ȸ�ϸ鼭 player�� update �޼��带 ȣ�����ִµ�
    // ������ ���� �ֻ����� �� n�� update �޼����� ���ڷ� �ѱ�� �ִ�

    this.players.forEach((player) => {
      player.update(n);
    });

    // �׸��� ���������� �ֻ����� ���� ��ȯ�Ѵ�

    return n;
  }
}
```

- ���� Player �߻�Ŭ������ ��ӹ޴� OddBettingPlyer Ŭ������ �߰��Ѵ�

```ts
// OddBettingPlayer.ts

import Player from "./Player";

export default class OddBettingPlayer extends Player {
  constructor(name: string) {
    super(name);
  }
  // �θ� Ŭ������ �߻� �޼������� update �޼��带 �߰��Ѵ�
  // Ȧ�� �� �� �ڽ��� �̰����Ƿ� _winning ���� ������Ʈ �Ѵ�
  update(diceNumber: number): void {
    this._winning = dicenumber % 2 === 1;
  }
}
```

- �׸��� ������ EvenettingPlyer Ŭ������ �߰��Ѵ�

```ts
// EvenBettingPlayer.ts

import Player from "./Player";

export default class EvenBettingPlayer extends Player {
  constructor(name: string) {
    super(name);
  }
  // �θ� Ŭ������ �߻� �޼������� update �޼��带 �߰��Ѵ�
  // ¦�� �� �� �ڽ��� �̰����Ƿ� _winning ���� ������Ʈ �Ѵ�
  update(diceNumber: number): void {
    this._winning = dicenumber % 2 === 0;
  }
}
```

- ������ NBettingPlayer Ŭ�����̴�

```ts
// NBettingPlayer.ts

import Player from "./Player";

export default class NBettingPlayer extends Player {
  // ns�� �÷��̾ ���� �ֻ��� ��ȣ��
  constructor(name: string, private ns: Array<number>) {
    super(name);
  }
  // �θ� Ŭ������ �߻� �޼������� update �޼��带 �߰��Ѵ�
  // �ֻ����� ���� diceNumber�� ���ԵǸ� _winning�� true�� �Ѵ�
  update(diceNumber: number): void {
    this._winning = this.ns.includes(diceNumber);
  }
}
```

- ���ݱ��� �ۼ��� �ڵ带 �Ʒ��� ���� ����� �� �ִ�

```js
import FairDiceGame from './FairDiceGame';
import OddBettingPlayer from './OddBettingPlayer';
import EvenBettingPlayer from './EvenBettingPlayer';
import NBettingPlayer from './NBettingPlayer';

const diceGame = new FairDiceGame(); // ���� ����

// ������ �÷��̾� ��ü ����

const players = [
  new OddBettingPlayer('Ȧ��'),
  new EvenBettingPlayer('¦��'),
  new NBettingPlayer('����', [2, 3, 5]),
];

// �� �÷��̾ diceGame�� ���� �÷��̾�� �߰��Ѵ�
players.forEach((player) => diceGame.addPlayer(player));

// ������ ������ �÷��̾�� ȭ�鿡 ǥ���ϴ� �Լ�
const updateBoard = () => {
  const domPlayers = document.querySelector('.players');
  domPlayers.innerHTML = '';

  players.forEach((player) => {
    const domPlayer = document.createElement('div');
    domPlayer.innerText = player.name;

    if (player.winning) domPlayer.classList.add('win');

    domPlayer.append(domPlayer);
  });
};

updateBoard(); // �Լ� ȣ�����ش�

// �ֻ��� ������ �̺�Ʈ �߰�

document.querySelector('button').addEventListener('click', () => {
  const diceNumber = diceGame.play(); // �ֻ��� ���� ����� �ش� �ϴ� ��

  const domDice = document.querySelector('.dice') as HTMLElement
  domDice.innerText = diceNumber.toString()

  updateBoard(); // ȭ�� ������Ʈ
});
```

### Pub/Sub ��

- ������ ������ �̾߱��� �� ������ �ʰ� �����ϴ� ������ ����/���� (**Pub**lish/**Sub**Scribe) ���� �ִ�

- �ϳ��� ���¸� �����ϴ� �������� �Ѿ broker��� �Ҹ��� �߰��ڶ�� ������ ����

- ��Ư�� �ټ��� �����ڵ鿡�� �׵��� �����ϴ� ���ɻ翡 ���� �޼����� �����ϴ� �������̴�

  - ��Ʃ���� ������ ���� ����

- �Ʒ� �ڵ带 ���ؼ� ���� ������ ����

<img src='./images/observer/observer03.png'>

- broker�� Ư�� key�� ���еǴ� listener ���տ��� �޼����� �����ϴ� publish �Լ���

- Ư�� key�� ����Ǵ� �޼����� ������ �����ʸ� ����ϴ� subscribe�� ������ �� �ִ�.

- ���� �����ڴ� �Ʒ�ó�� ���Ŀ�� �����ʸ� ����ص����ν� ������ Ư�� �޼����� ����������, �� �޼����� ������ �� �ִ�.

<img src='./images/observer/observer04.png'>

- �̷��� ������ �����ϴ� ���� api�δ� �������� [message channel](https://developer.mozilla.org/en-US/docs/Web/API/MessageChannel), [event target](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/EventTarget)���� �ִ�.

- ����/���� �������� ����ϸ� �����ڿ� ������ �� ���踦 ���Ŀ�� �����ϰ�,

- �̵��� ������ ���縦 ��ä �������̽� Ȥ�� �޼������� �����ϱ� ������ ���� �� ��ȣ �������� �ٿ��� �� �ִ�.

<img src='./images/observer/observer05.png'>

- ����Ʈ���� ���迡�� �������� �پ��� ������ �����鿡�� �����غ� �� �ִ�.

- ������� react�� ����ϴ� ������Ʈ���� axios ��û�� ���� ���� ������ ���������� toast�� ������� �����

- �� ��û���� catch�� �ϴ°� ��ȿ�����ϰͰ��� �����ϰԴ� axios interceptor�� �������� �ؾߵ� �� ������

- react ������Ʈ�� ������ ��� �������� ���� �� ���� �ʴ´�.

- ���⿡ ����/���� �������� ��� �غ� �� �ִ�.

<img src='./images/observer/observer06.png'>

- ���� ���ʿ��� �� �� �ֵ��� errorBrokeró�� �����ϰ� ���� ���� �޼������� �߰��� broker�� �����

- Toast �Լ��� �����ؼ� toast ������ ����ϴ� react ������Ʈ���� ������ �ɾ�θ�

- axios interceptor���� broker���� ���� �޼����� publish�ϴ� ������ react ������Ʈ���� ���� ��ų �� �ְԵȴ�.

- �̴� �Ʒ�ó�� react toastify��� ���̺귯���� ������ ������Ʈ ������ �����ʾƵ� react lifecycle�� ���� toast ������Ʈ�� ������ �� �ִ°Ͱ� ����� ����

<img src='./images/observer/observer07.png'>

- ���� �̷� ������ ������ ������ ������ ���ϸ����ε� �����ϰ����� �پ��� ������ Ư�� ���·� ó���� ��ȹ�� �ִٸ� ����/���� �𵨵� ���� ������ �� �� ���� ��

---

## Reference

- [����Ʈ���忡 ���������� �����2 - ������ ����, ���� ����(observer pattern, pub sub)](https://www.youtube.com/watch?v=aH4U6bfi_Ds&t=20)
- [����������, Observer Pattern, ������ ����](https://www.youtube.com/watch?v=1dwx3REUo34&t=30)
- [�����ڰ� �˾ƾ��� ���������� | ep3. Observer Pattern | �ڹٽ�ũ��Ʈ ������ ����](https://www.youtube.com/watch?v=1UxRkggQwbs&t=29)
- [TypeScript�� ���� GoF�� ������ ����: 13. Observer](https://www.youtube.com/watch?v=aAA2t9VT-A0&list=PLe6NQuuFBu7H3sFnErshsfgNPE9dOZZrx&index=13)
- [Observer ����](https://patterns-dev-kr.github.io/design-patterns/observer-pattern/)
- [Observer ���� �˾ƺ��� (hooks�� observables)](https://www.howdy-mj.me/javascript/observer-pattern)
