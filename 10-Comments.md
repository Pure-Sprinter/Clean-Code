## 주석 Comments

### 1. 비즈니스 로직이 복잡한 경우에만 주석을 다세요

주석을 다는 것은 사과해야할 일이며 필수적인 것이 아닙니다. 좋은 코드는 코드 자체로 말합니다.

**Bad**

```javascript
function hashIt(data) {
  // 이건 해쉬입니다.
  let hash = 0;

  // lengh는 data의 길이입니다.
  const length = data.length;

  // 데이터의 문자열 개수만큼 반복문을 실행합니다.
  for (let i = 0; i < length; i++) {
    // 문자열 코드를 얻습니다.
    const char = data.charCodeAt(i);
    // 해쉬를 만듭니다.
    hash = (hash << 5) - hash + char;
    // 32-bit 정수로 바꿉니다.
    hash &= hash;
  }
}
```

**Good**

```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // 32-bit 정수로 바꿉니다.
    hash &= hash;
  }
}
```

### 2. 주석으로 된 코드를 남기지 마세요

버전 관리 도구가 존재하기 때문에 코드를 주석으로 남길 이유가 없습니다.

**Bad**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Good**

```javascript
doStuff();
```

### 3. 코드 기록을 주석으로 남기지 마세요

버전 관리 도구를 이용해야하는 것을 꼭 기억하세요. 죽은 코드도 불필요한 설명도 특히 코드의 기록에 대한 주석도 필요하지 않습니다. 코드의 기록에 대해 보고 싶다면 git log를 사용하세요!

**Bad**

```javascript
/**
 * 2016-12-20: 모나드 제거했음, 이해는 되지 않음 (RM)
 * 2016-10-01: 모나드 쓰는 로직 개선 (JP)
 * 2016-02-03: 타입체킹 하는부분 제거 (LI)
 * 2015-03-14: 버그 수정 (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**Good**

```javascript
function combine(a, b) {
  return a + b;
}
```

### 4. 코드의 위치를 설명하지 마세요

이건 정말 쓸데 없습니다. 적절한 들여쓰기와 포맷팅을 하고 함수와 변수의 이름에 의미를 부여하세요.

**Bad**

```javascript
////////////////////////////////////////////////////////////////////////////////
// 스코프 모델 정의
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: "foo",
  nav: "bar",
};

////////////////////////////////////////////////////////////////////////////////
// actions 설정
////////////////////////////////////////////////////////////////////////////////
const actions = function () {
  // ...
};
```

**Good**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar",
};

const actions = function () {
  // ...
};
```
