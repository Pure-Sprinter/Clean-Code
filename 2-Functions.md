## 함수 Functions

### 1. 함수 인자는 2개 이하가 이상적이다.

매개변수의 개수를 제한하는 것은 **함수 테스팅을 쉽게 만들어주기 때문에 중요**합니다. 매개변수가 3개 이상일 경우엔 테스트 해야하는 경우의 수가 많아지고 각기 다른 인수들로 여러 사례들을 테스트 해야합니다.

1개나 2개의 인자를 가지고 있는 것이 가장 이상적인 케이스입니다. 그리고 3개의 인자는 가능한 피해야 합니다. 그것보다 더 많다면 통합되어야 합니다. 만약 당신이 2개 이상의 인자를 가진 함수를 사용한다면 그 함수에게 너무 많은 역할을 하게 만든 것입니다. 그렇지 않은 경우라면 대부분의 경우 상위 객체는 1개의 인자만으로 충분합니다.

Javascript를 사용할 때 많은 보일러플레이트 없이 바로 객체를 만들 수 있습니다. 그러므로 당신이 만약 많은 인자들을 사용해야 한다면 객체를 이용할 수 있습니다.

함수가 기대하는 속성을 좀더 명확히 하기 위해서 es6의 비구조화 구문을 사용할 수 있고 이 구문에는 몇가지 장점이 있습니다.

1. 어떤 사람이 그 함수의 시그니쳐(인자의 타입, 반환되는 값의 타입 등)를 볼 때 어떤 속성이 사용되는지 즉시 알 수 있습니다.
2. 또한 비구조화는 함수에 전달된 인수 객체의 지정된 기본타입 값을 복제하여 이는 사이드이펙트가 일어나는 것을 방지합니다. 참고로 인수 객체로부터 비구조화된 객체와 배열은 복제되지 않습니다.
3. Linter를 사용하면 사용하지 않는 인자에 대해 경고해주거나 비구조화 없이 코드를 짤 수 없게 할 수 있습니다.

**Bad**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

**Good**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true,
});
```

### 2. 함수는 하나의 행동만 해야합니다.

함수가 1개 이상의 행동을 한다면 작성하는 것도, 테스트 하는 것도, 이해하는 것도 어려워집니다. 당신이 하나의 함수에 하나의 행동을 정의하는 것이 가능해진다면 함수는 좀 더 고치기 쉬워지고 코드들은 읽기 쉬워질 것입니다.

**Bad**

```javascript
function emailClients(clients) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Good**

```javascript
function emailClients(clients) {
  clients.filter(isClientActive).forEach(email);
}

function isClientActive(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

### 3. 함수명은 함수가 무엇을 하는지 알 수 있어야 합니다.

**Bad**

```javascript
function AddToDate(date, month) {
  // ...
}

const date = new Date();
AddToDate(date, 1);
```

**Good**

```javascript
function AddMonthToDate(date, month) {
  // ...
}

const date = new Date();
AddMonthToDate(date, 1);
```

### 4. 함수는 단일 행동을 추상화 해야합니다

추상화된 이름이 여러 의미를 내포하고 있다면 그 함수는 너무 많은 일을 하게끔 설계된 것입니다.함수들을 나누어서 재사용 가능하고 테스트하기 쉽게 만드세요

**Bad**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  });
}
```

**Good**

```javascript
function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function lexer(tokens) {
  const ast = [];
  tokens.forEach((token) => {
    ast.push(/* ... */);
  });

  return ast;
}

function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const ast = lexer(tokens);
  ast.forEach((node) => {
    // parse...
  });
}
```

### 5. 중복된 코드를 작성하지 마세요

중복된 코드를 작성하지 않기 위해 최선을 다하세요 중복된 코드가 있다는 것은 어떤 로직을 수정해야 할 일이 생겼을 때 수정해야할 코드가 한 곳 이상이라는 것을 뜻합니다.

종종 코드를 살펴보면 사소한 몇몇의 차이점 때문에 중복된 코드를 작성한 경우가 있고 이런 차이점들은 대부분 똑같은 일을 하는 분리된 함수들을 갖도록 강요합니다. 즉 중복 코드를 제거한다는 것은 하나의 함수 / 모듈 / 클래스를 사용하여 이 여러 가지 사소한 차이점을 처리할 수 있는 추상화를 만드는 것을 의미합니다.

그리고 추상화 할 부분이 남아있는 것은 위험하기 때문에 클래스 섹션에 제시된 여러 원칙들을 따라야 합니다. 잘 추상화하지 못한 코드는 중복된 코드보다 나쁠 수 있으므로 조심하세요. 즉 추상화를 잘 할 수 있다면 그렇게 하라는 의미로 중복을 피한다면 여러분이 원할 때 언제든 한곳만 수정해도 다른 모든 코드에 반영되게 할 수 있습니다.

**Bad**

```javascript
function showDeveloperList(developers) {
  developers.forEach((developers) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink,
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio,
    };

    render(data);
  });
}
```

**Good**

```javascript
function showEmployeeList(employees) {
  employees.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    let portfolio = employee.getGithubLink();

    if (employee.type === "manager") {
      portfolio = employee.getMBAProjects();
    }

    const data = {
      expectedSalary,
      experience,
      portfolio,
    };

    render(data);
  });
}
```

### 6. Object.assign을 사용해 기본 객체를 만드세요

**Bad**

```javascript
const menuConfig = {
  title: null,
  body: "Bar",
  buttonText: null,
  cancellable: true,
};

function createMenu(config) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}
```

**Good**

```javascript
const menuConfig = {
  title: "Order",
  buttonText: "Send",
  cancellable: true,
};

function createMenu(config) {
  config = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true,
    },
    config
  );
}
```

### 7. 매개변수로 분기(플래그)를 사용하지 마세요

플래그를 사용하는 것 자체가 그 함수가 한가지 이상의 역할을 하고 있다는 것을 뜻합니다. boolean 기반으로 함수가 실행되는 코드가 나뉜다면 함수를 분리하세요

**Bad**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Good**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

### 8. 사이드 이펙트를 피하세요 (Part 1)

함수는 값을 받아서 어떤 일을 하거나 값을 리턴할 때 사이드 이펙트를 만들어냅니다. 사이드 이펙트는 파일에 쓰여질 수도 있고, 전역 변수를 수정할 수 있으며, 실수로 모든 돈을 다른 사람에게 보낼 수도 있습니다.

당신은 때때로 프로그램에서 사이드 이펙트를 만들어야 할 때가 있습니다. 아까 들었던 예시 중 하나인 파일 작성을 할 때와 같이 말이죠. 이 때 여러분이 해야할 일은 파일 작성을 하는 한 개의 함수를 만드는 일입니다. 파일을 작성하는 함수나 클래스가 여러 개 존재하면 안됩니다. 반드시 하나만 있어야 합니다.

즉, 어떠한 구조체도 없이 객체 사이의 상태를 공유하거나, 무엇이든 쓸 수 있는 변경 가능한 데이터 유형을 사용하거나, 같은 사이드 이펙트를 만들어내는 것을 여러 개 만들면 안됩니다.

---

```javascript
let name = "김 진성";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();
```

---

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "김 진성";
const newName = splitIntoFirstAndLastName(name);
```

### 9. 사이드 이펙트를 피하세요 (Part 2)

자바스크립트에서는 기본타입 자료형은 값을 전달하고 객체와 배열은 참조를 전달합니다. 객체와 배열인 경우를 한번 살펴봅시다. 우리가 만든 함수는 장바구니 배열에 변화를 주며 이 변화는 구매목록에 어떤 상품을 추가하는 기능 같은 것을 말합니다. 만약 장바구니 배열을 사용하는 어느 다른 함수가 있다면 이러한 추가에 영향을 받습니다. 이것은 좋을 수도 있지만, 안좋을 수도 있습니다. 안좋은 예를 한번 상상해봅시다.

사용자가 구매하기 버튼을 눌러 구매 함수를 호출합니다. 이는 네트워크 요청을 생성하고 서버에 장바구니 배열을 보냅니다 .하지만 네트워크 연결이 좋지 않아서구매 함수는 다시 한번 네트워크 요청을 보내야 하는 상황이 생겼습니다. 이 때, 사용자가 네트워크 요청이 시작되기 전에 실수로 원하지 않는 상품의 "장바구니에 추가" 버튼을 실수로 클릭하면 어떻게 될까요? 실수가 있고난 뒤, 네트워크 요청이 시작되면 장바구니에 추가 함수 때문에 실수로 변경된 장바구니 배열을 서버에 보내게 됩니다.

가장 좋은 방법은 장바구니에 추가는 **항상 장바구니 배열을 복제하여 수정하고 복제본을 반환**하는 것입니다. 이렇게 하면 장바구니 참조를 보유하고 있는 다른 함수가 다른 변경 사항의 영향을 받지 않게 됩니다.

이 접근법에서는 2가지 포인트가 있습니다.

1. 실제로 입력된 객체를 수정하고 싶은 경우가 있을 수 있지만 이러한 예제를 생각해보고 적용해보면 그런 경우는 거의 없다는 것을 깨달을 수 있습니다. 그리고 대부분의 것들이 사이드 이펙트 없이 리팩토링 될 수 있습니다.
2. 큰 객체를 복제하는 것은 성능 측면에서 값이 매우 비쌉니다. 운좋게도 이런게 큰 문제가 되지는 않습니다. 왜냐하면 이러한 프로그래밍 접근법을 가능하게 해줄 좋은 방식이 있습니다. 이는 객체와 배열을 수동으로 복제하는 것처럼 메모리 집약적이지 않게 해주고 빠르게 복제해줍니다.

**Bad**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Good**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

### 10. 전역 함수를 사용하지 마세요

전역 환경을 사용하는 것은 자바스크립트에서 나쁜 관행입니다. 왜냐하면 다른 라이브러리들과의 충돌이 일어날 수 있고, 당신의 API를 쓰는 유저들은 운영환경에서 예외가 발생하기 전까지는 문제를 인지하지 못할 것이기 때문입니다. 예제를 하나 생각해보면 자바스크립트의 Array 메소드를 확장하여 두 배열 간의 차이를 보여줄 수 있는 diff 메소드를 사용하려면 어떻게 해야할까요? 새로운 함수를 Array.prototype에 쓸 수도 있지만, 똑같은 일을 시도한 다른 라이브러리와 충돌할 수 있습니다. 다른 라이브러리가 diff 메소드를 사용하여 첫번째 요소와 마지막 요소의 차이점을 찾으면 어떻게 될까요? 이것이 그냥 ES2015/ES6의 classes를 사용해서 전역 Array를 상속해버리는 것이 훨씬 더 나은 이유입니다.

**Bad**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter((element) => !hash.has(element));
};
```

**Good**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter((element) => !hash.has(element));
  }
}
```

### 11. 명령어 프로그래밍보다 함수형 프로그래밍을 지향하세요

자바스크립트는 Haskell처럼 함수형 프로그래밍 언어는 아니지만 함수형 프로그래밍처럼 작성할 수 있습니다. 함수형 언어는 더 깔끔하고 테스트하기 쉽습니다.

**Bad**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500,
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500,
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150,
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000,
  },
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**Good**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500,
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500,
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150,
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000,
  },
];

const totalOutput = programmerOutput
  .map((programmer) => programmer.linesOfCode)
  .reduce((acc, linesOfCode) => acc + linesOfCode, INITIAL_VALUE);
```

### 12. 조건문을 캡슐화하세요

**Bad**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**Good**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

### 13. 부정조건문을 사용하지 마세요

**Bad**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Good**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

### 14. 조건문 작성을 피하세요

조건문 작성을 피하라는 것은 매우 불가능한 일로 보입니다. 이 얘기를 처음 듣는 사람들은 대부분 if 문 없이 어떻게 코드를 짜나요? 라고 말합니다. 하지만 다형성을 이용한다면 동일한 작업을 수행할 수 있습니다. 두번째 질문은 보통 수긍을 한다고 하지만 왜 그렇게 해야하는지 물어보는 것입니다. 이에 대한 대답은 앞서 설명했었는데 함수는 단 하나의 일만 수행하여야 합니다. 당신이 함수나 클래스에 if 문을 쓴다면 그것은 그 함수나 클래스가 한가지 이상의 일을 수행하고 있다고 말하는 것과 같습니다. 하나의 함수는 딱 하나의 일만 해야합니다.

**Bad**

```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case "777":
        return this.getMaxAltitude() - this.getPassengerCount();
      case "Air Force One":
        return this.getMaxAltitude();
      case "Cessna":
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**Good**

```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

### 15. 타입-체킹을 피하세요 (Part 1)

자바스크립트는 타입이 정해져있지 않습니다. 이는 당신의 함수가 어떤 타입의 인자든 받을 수 있다는 것을 의미합니다. 이런 자바스크립트의 자유로움 때문에 여러 버그가 발생했었고 이 때문에 당신의 함수에 타입-체킹을 시도할 수도 있습니다. 하지만 타입-체킹 말고도 이러한 화를 피할 많은 방법들이 존재합니다. 첫번째 방법은 일관성 있는 API를 사용하는 것입니다.

**Bad**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**Good**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

### 16. 타입-체킹을 피하세요 (Part 2)

당신이 문자열, 정수, 배열 등 기본 자료형을 사용하고 다형성을 사용할 수 없을 때 여전히 타입-체킹이 필요하다고 느껴진다면 typescript를 도입하는 것을 고려하는 것이 좋습니다. typescript는 표준 자바스크립트 구문에 정적 타입을 제공하므로 일반 자바스크립트의 대안으로 사용하기에 좋습니다. 자바스크립트에서 타입-체킹을 할 때 문제점은 가짜 type-safety를 얻기 위해 작성된 코드를 설명하기 위해서 많은 주석을 달아야한다는 점입니다. 자바스크립트로 코드를 작성할 때에는 깔끔하게 코드를 작성하고, 좋은 테스트코드를 짜야하며 좋은 코드 리뷰를 해야합니다. 그러기 싫다면 그냥 typescript를 씁니다.

**Bad**

```javascript
function combine(val1, val2) {
  if (
    (typeof val1 === "number" && typeof val2 === "number") ||
    (typeof val1 === "string" && typeof val2 === "string")
  ) {
    return val1 + val2;
  }

  throw new Error("Must be of type String or Number");
}
```

**Good**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

### 17. 과도한 최적화를 지양하세요

최신 브라우저들은 런타임에 많은 최적화 작업을 수행합니다. 대부분 당신이 코드를 최적화 하는 것은 시간낭비일 가능성이 많습니다.

**Bad**

```javascript
// 오래된 브라우저의 경우 캐시되지 않은 `list.length`를 통한 반복문은 높은 코스트를 가졌습니다.
// 그 이유는 `list.length`를 매번 계산해야만 했기 때문인데, 최신 브라우저에서는 이것이 최적화 되었습니다.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Good**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

### 18. 죽은 코드를 지우세요

죽은 코드는 중복된 코드만큼이나 좋지 않습니다. 죽은 코드는 당신의 코드에 남아있을 어떠한 이유도 없습니다. 호출되지 않는 코드가 있다면 그 코드는 지우세요 그 코드가 여전히 필요해도 그 코드는 버전 히스토리에 안전하게 남아있을 것입니다.

**Bad**

```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**Good**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```
