## 객체 지향 원칙 SOLID

### 1. 단일 책임 원칙 (Single Responsibility Principle, SRP)

Clean Code에서 말하길 "클래스를 수정할 때는 수정해야하는 이유가 2개 이상 있으면 안됩니다". 이것은 하나의 클래스에 많은 기능을 쑤셔넣는 것이나 다름 없습니다. 마치 비행기를 탈 때 가방을 1개만 가지고 탈 수 있을 때처럼 말이죠. 이 문제는 당신의 클래스가 개념적으로 응집되어 있지 않다는 것이고, 클래스를 바꿔야할 많은 이유가 됩니다. 클래스를 수정하는데 들이는 시간을 줄이는 것은 중요합니다. 왜냐하면 하나의 클래스에 너무 많은 기능들이 있고 당신이 이 작은 기능들을 수정할 때 이 코드가 다른 모듈들에 어떠한 영향을 끼치는지 이해하기 어려울 수 있기 때문입니다.

**Bad**

```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**Good**

```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

### 2. 개방/폐쇄 원칙 (Open/Closed Principle, OCP)

Bertrand Meyer에 말에 의하면 "소프트웨어 개체(클래스, 모듈, 함수 등)는 확장을 위해 개방적이어야 하며 변경 시에는 폐쇄적이어야 합니다." 이것에 의미는 무엇일가요?? 이 원리는 기본적으로 사용자가 .js 소스 코드 파일을 열어 수동으로 조작하지 않고도 모듈의 기능을 확장하도록 허용해야 한다고 말합니다.

**Bad**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === "ajaxAdapter") {
      return makeAjaxCall(url).then((response) => {
        // transform response and return
      });
    } else if (this.adapter.name === "httpNodeAdapter") {
      return makeHttpCall(url).then((response) => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url) {
  // request and return promise
}

function makeHttpCall(url) {
  // request and return promise
}
```

**Good**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then((response) => {
      // transform response and return
    });
  }
}
```

### 3. 리스코프 치환 원칙 (Liskov Substitution Principle, LSP)

이것은 매우 간단하지만 강력한 원칙입니다. 리스코프 원칙이란 자료형 S가 자료형 T의 하위형이라면, 프로그램이 갖추어야할 속성들의 변경사항 없이, 자료형 T의 객체를 자료형 S의 객체로 교체할 수 있어야 한다는 원칙입니다.

이 원칙을 예를 들어 설명하자면 당신이 부모 클래스와 자식 클래스를 가지고 있을 때 베이스 클래스와 하위 클래스를 잘못된결과 없이 서로 교환하여 사용할 수 있습니다. 여전히 이해가 안간다면 정사각형-직사각형 예제를 봅시다. 수학적으로 정사각형은 직사각형이지만 상속을 통해 "is-a" 관계를 사용하여 모델링한다면 문제가 발생합니다.

**Bad**

```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach((rectangle) => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // 정사각형일때 25를 리턴합니다. 하지만 20이어야 하는게 맞습니다.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Good**

```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach((shape) => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

### 4. 인터페이스 분리 원칙 (Interface Segregation Principle, ISP)

자바스크립트는 인터페이스가 없기 때문에 다른 원칙들처럼 딱 맞게 적용할 수는 없습니다. 그러나 자바스크립트에 타입 시스템이 없다 하더라도 중요하고 관계있는 원칙입니다.

ISP에 의하면 "클라이언트는 사용하지 않는 인터페이스에 의존하도록 강요 받으면 안됩니다." 덕 타이핑 때문에 인터페이스는 자바스크립트에서는 암시적인 계약일 뿐입니다.

자바스크립트에서 이것을 보여주는 가장 좋은 예는 방대한 양의 설정 객체가 필요한 클래스입니다. 클라이언트가 방대한 양의 옵션을 설정하지 않는 것이 좋습니다. 왜냐하면 대부분의 경우 설정들이 전부 다 필요한 건 아니기 때문입니다. 설정을 선택적으로 할 수 있다면 "무거운 인터페이스 (fat interface)"를 만드는 것을 방지할 수 있습니다.

**Bad**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  animationModule() {}, // 우리는 대부분의 경우 DOM을 탐색할 때 애니메이션이 필요하지 않습니다.
  // ...
});
```

**Good**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  options: {
    animationModule() {},
  },
});
```

### 5. 의존성 역전 원칙 (Dependency Inversion Principle, DIP)

이 원칙은 두가지 중요한 요소를 가지고 있습니다.

1. 상위 모듈은 하위 모듈에 종속되어서는 안됩니다. 둘 다 추상화에 의존해야 합닏다.
2. 추상화는 세부사항에 의존하지 않습니다. 세부사항은 추상화에 의해 달라져야 합니다.

처음에는 이것을 이해하는데 어려울 수 있습니다. 하지만 Angular.js로 작업해본적이 있다면 의존성 주입 형태로 이 원리를 구현한 것을 보았을 것입니다. DIP는 동일한 개념은 아니지만 상위 모듈이 하위 모듈의 세부사항을 알지 못하게 합니다. 이는 의존성 주입을 통해 달성할 수 있습니다. DI의 장점은 모듈 간의 의존성을 감소시키는 데에 있습니다. 모듈 간의 의존성이 높을 수록 코드를 리팩토링 하는데 어려워지고 이것은 매두 나쁜 개발 패턴들 중 하나입니다.

앞에서 설명한 것처럼 자바스크립트에는 인터페이스가 없으므로 추상화에 의존하는 것은 암시적인 약속입니다. 이말인 즉슨, 다른 객체나 클래스에 노출되는 메소드와 속성이 바로 암시적인 약속(추상화)가 된다는 것이죠. 아래 예제에서 암시적인 약속은 InventoryTracker에 대한 모든 요청 모듈이 requestItems 메소드를 가질 것이라는 점입니다.

**Bad**

```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // 안좋은 이유: 특정 요청방법 구현에 대한 의존성을 만들었습니다.
    // requestItems는 한가지 요청방법을 필요로 합니다.
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach((item) => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
inventoryTracker.requestItems();
```

**Good**

```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach((item) => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ["WS"];
  }

  requestItem(item) {
    // ...
  }
}

// 의존성을 외부에서 만들어 주입해줌으로써,
// 요청 모듈을 새롭게 만든 웹소켓 사용 모듈로 쉽게 바꿔 끼울 수 있게 되었습니다.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```
