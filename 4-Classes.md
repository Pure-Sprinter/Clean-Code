## 클래스 Classes

### 1. ES5의 함수보다 ES2015/ES6의 클래스를 사용하세요

기존 ES5의 클래스에서 이해하기 쉬운 상속, 구성 및 메소드 정의를 하는 것은 매우 어렵습니다. 매번 그런 것은 아니지만 상속이 피요한 경우라면 클래스를 사용하는 것이 좋습니다. 하지만 당신이 크고 더 복잡한 객체가 필요한 경우가 아니라면 클래스보다 작은 함수를 사용하세요

**Bad**

```javascript
const Animal = function (age) {
  if (!(this instanceof Animal)) {
    throw new Error("Instantiate Animal with `new`");
  }

  this.age = age;
};

Animal.prototype.move = function () {};

const Mammal = function (age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error("Instantiate Mammal with `new`");
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function (age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error("Instantiate Human with `new`");
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**Good**

```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() {
    /* ... */
  }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() {
    /* ... */
  }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() {
    /* ... */
  }
}
```

### 2. 메소드 체이닝을 사용하세요

자바스크립트에서 메소드 체이닝은 매우 유용한 패턴이며 jQuery 나 Lodash 같은 많은 라이브러리에서 이 패턴을 찾아볼 수 있습니다. 이는 코드를 간겨랗고 이해하기 쉽게 만들어줍니다. 이런 이유들로 메소드 체이닝을 쓰는 것을 권하고, 사용해본 뒤 얼마나 코드가 깔끔해졌는지 꼭 확인해보길 바랍니다. 클래스 함수에서 단순히 모든 함수의 끝에 'this'를 리턴해주는 것으로 클래스 메소드를 추가로 연결할 수 있습니다.

**Bad**

```javascript
class Car {
  constructor() {
    this.make = "KIA";
    this.model = "SONATA";
    this.color = "white";
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car();
car.setColor("black");
car.setMake("Ford");
car.setModel("F-150");
car.save();
```

**Good**

```javascript
class Car {
  constructor() {
    this.make = "KIA";
    this.model = "SONATA";
    this.color = "white";
  }

  setMake(make) {
    this.make = make;
    return this;
  }

  setModel(model) {
    this.model = model;
    return this;
  }

  setColor(color) {
    this.color = color;
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    return this;
  }
}

const car = new Car()
  .setColor("black")
  .setMake("Ford")
  .setModel("F-150")
  .save();
```

### 3. 상속보다 조합(Composition)을 사용하세요

Gang of four의 Design Patterns 에서 유명한 전략으로 당신은 가능하다면 상속보다는 조합을 사용해야 합니다. 상속을 사용했을 때 얻을 수 있는 이득보다 조합을 사용했을 때 얻을 수 있는 이득이 많기 때문입니다. 이 원칙의 요점은 당신이 계속 상속을 사용해서 코드를 작성하고자 할 때, 만약 조합을 이용하면 더 코드를 잘 짤 수 있지 않을까 생각해보라는 것에 있습니다. 때때로는 이것이 맞는 전략이기 때문이죠.

"그럼 대체 상속을 언제 사용해야 되는 건가요?"라고 물어볼 수 있습니다. 이건 당신이 직면한 문제 상황에 달려있지만 조합보다 상속을 쓰는게 더 좋을 만한 예시를 몇개 들어보겠습니다.

1. 당신의 상속관계가 "has-a" 관계가 아니라 "is-a" 관계일 때 (사람 -> 동물 vs. 유저 -> 유저정보)
2. 기반 클래스의 코드를 다시 사용할 수 있을 때 (인간은 모든 동물처럼 움직일 수 있습니다.)
3. 기반 클래스를 수정하여 파생된 클래스 모두를 수정하고 싶을 때 (이동시 모든 동물이 소비하는 칼로리를 변경하고 싶을 때)

**Bad**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// 이 코드가 안좋은 이유는 Employees가 tax data를 "가지고" 있기 때문입니다.
// EmployeeTaxData는 Employee 타입이 아닙니다.
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**Good**

```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```
