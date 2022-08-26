## 에러 처리 Error Handling

에러를 뱉는다는 것은 좋은 것입니다. 즉, 프로그램에서 무언가가 잘못되었을 때 런타임에서 성공적으로 확인되면 현재 스택에서 함수 실행을 중단하고 (노드에서) 프로세스를 종료하고 스택 추적으로 콘솔에서 사용자에게 그 이유를 알려줍니다.

### 1. 단순히 에러를 확인만 하지마세요

단순히 에러를 확인하는 것만으로 그 에러가 해결되거나 대응할 수 있게 되는 것은 아닙니다. console.log를 통해 콘솔에 로그를 기록하는 것은 에러 로그를 잃어버리기 쉽기 때문에 좋은 방법이 아닙니다. 만약에 try/catch로 어떤 코드를 감쌋다면 그건 당신이 그 코드에 어떤 에러가 날지도 모르기 때문에 감싼 것이므로 그에 대한 계획이 있거나 어떠한 장치를 해야합니다.

**Bad**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Good**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // 첫번째 방법은 console.error를 이용하는 것입니다. 이건 console.log보다 조금 더 알아채기 쉽습니다.
  console.error(error);
  // 다른 방법은 유저에게 알리는 방법입니다.
  notifyUserOfError(error);
  // 또 다른 방법은 서비스 자체에 에러를 기록하는 방법입니다.
  reportErrorToService(error);
  // 혹은 그 어떤 방법이 될 수 있습니다.
}
```

### 2. Promise가 실패된 것을 무시하지 마세요

**Bad**

```javascript
getdata()
  .then((data) => {
    functionThatMightThrow(data);
  })
  .catch((error) => {
    console.log(error);
  });
```

**Good**

```javascript
getdata()
  .then((data) => {
    functionThatMightThrow(data);
  })
  .catch((error) => {
    // 첫번째 방법은 console.error를 이용하는 것입니다. 이건 console.log보다 조금 더 알아채기 쉽습니다.
    console.error(error);
    // 다른 방법은 유저에게 알리는 방법입니다.
    notifyUserOfError(error);
    // 또 다른 방법은 서비스 자체에 에러를 기록하는 방법입니다.
    reportErrorToService(error);
    // 혹은 그 어떤 방법이 될 수 있습니다.
  });
```
