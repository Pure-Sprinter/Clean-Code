## 객체와 자료구조 Objects and Data Structures

### 1. getter와 setter를 사용하세요

자바스크립트는 인터페이스와 타입을 가지고 있지 않고 이러한 패턴을 적용하기가 힘듭니다. 왜냐하면 public이나 private 같은 키워드가 없기 때문이죠. 그렇기 때문에 getter 와 setter를 사용하여 객체의 데이터에 접근하는 것이 객체의 속성을 찾는 것보다 훨씬 낫습니다. 그 이유는 아래와 같습니다.

- 객체의 속성을 얻는 것 이상의 많은 것을 하고 싶을 때, 코드에서 모든 접근자를 찾아 바꾸고 할 필요가 없습니다.
- set할 때 검증로직을 추가하는 것이 코드를 더 간단하게 만듭니다.
- 내부용 API를 캡슐화할 수 있습니다.
- getting과 setting 할 때 로그를 찾거나 에러처리를 하기 쉽습니다.
- 서버에서 객체 속성을 받아올 때 lazy load 할 수 있습니다.

**Bad**

```javascript
function makeBankAccount() {
  // ...

  return {
    // ...
    balance: 0,
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**Good**

```javascript
function makeBankAccount() {
  // private으로 선언된 변수
  let balance = 0;

  // 아래 return을 통해 public으로 선언된 "getter"
  function getBalance() {
    return balance;
  }

  // 아래 return을 통해 public으로 선언된 "setter"
  function setBalance(amount) {
    // ... balance를 업데이트하기 전 검증로직
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance,
  };
}

const account = makeBankAccount();
account.setBalance(100);
```

### 2. 객체에 비공개 멤버를 만드세요

클로져를 이용하면 가능합니다.

**Bad**

```javascript
const Employee = function (name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**Good**

```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    },
  };
}

const employee = makeEmployee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```
