## 포맷팅 Formatting

포맷팅은 주관적입니다. 여기에 있는 많은 규칙과 마찬가지로 따르기 쉬운 규칙들이 있습니다. 여기서 알아야 할것은 포맷팅에 대해 과도하게 신경쓰는 것은 의미없다는 것입니다. 자동으로 서식을 교정해주는 것에 해당하지 않는 사항에 대해서는 몇가지 지침을 따르는 것이 좋습니다.

### 1. 일관된 대소문자를 사용하세요

자바스크립트에는 정해진 타입이 없기 때문에 대소문자를 구분하는 것으로 당신의 변수나 함수명 등에서 많은 것을 알 수 있습니다. 이 규칙 또한 주관적이기 때문에 당신이 팀이 선택한 규칙들을 따르세요 중요한 것은 항상 일관성 있게 사용해야 한다는 것입니다.

**Bad**

```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**Good**

```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

### 2. 함수 호출자와 함수 피호출자는 가깝게 위치시키세요

어떤 함수가 다른 함수를 호출하면 그 함수들은 소스 파일 안에서 서로 수직으로 근접해 있어야 합니다. 이상적으로는 함수 호출자를 함수 피호출자 바로 위에 위치시켜야 합니다. 우리는 코드를 읽을 때 신문을 읽듯 위에서 아래로 읽기 때문에 코드를 작성할 때도 읽을 때를 고려하여 작성해야 합니다.

**Bad**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(user);
review.perfReview();
```

**Good**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```
