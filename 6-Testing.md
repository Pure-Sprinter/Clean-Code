## 테스트 Testing

테스트는 배포하는 것보다 중요합니다. 테스트 없이 배포한다는 것은 당신이 짜놓은 코드가 언제든 오작동해도 이상하지 않다는 얘기와 같습니다. 테스트에 얼마나 시간을 투자할지는 당신이 함께 일하는 팀에 달려있지만 Coverage 100%라는 것은 개발자들에게 높은 자신감과 안도감을 줍니다.

테스트 코드를 작성하지 않는다는 것은 그 무엇도 변명이 될 수 없습니다. 테스트 프레임워크를 골랐다면 이제부터는 팀의 목표를 모든 새로운 기능/모듈을 짤 때 테스트 코드를 작성하는것으로 합니다. 만약 테스트 주도 개발 방법론이 당신에게 맞는 방법이라면 훌륭한 개발 방법이 될 수 있습니다. 그러나 중요한 것은 당신이 어떠한 기능을 개발하거나 코드를 리팩토링할 때 당신이 정한 Coverage 목표를 달성하는 것에 있습니다.

**Bad**

```javascript
const assert = require("assert");

describe("MakeMomentJSGreatAgain", () => {
  it("handles date boundaries", () => {
    let date;

    date = new MakeMomentJSGreatAgain("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);

    date = new MakeMomentJSGreatAgain("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);

    date = new MakeMomentJSGreatAgain("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**Good**

```javascript
const assert = require("assert");

describe("MakeMomentJSGreatAgain", () => {
  it("handles 30-day months", () => {
    const date = new MakeMomentJSGreatAgain("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);
  });

  it("handles leap year", () => {
    const date = new MakeMomentJSGreatAgain("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);
  });

  it("handles non-leap year", () => {
    const date = new MakeMomentJSGreatAgain("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```
