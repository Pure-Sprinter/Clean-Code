## 변수 Variables

### 1. 의미있고 발음하기 쉬운 변수 이름 사용하기

**Bad**

```javascript
const yyyymmdd = moment().format("YYYY/MM/DD");
```

**Good**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

### 2. 동일한 유형의 변수에 동일한 어휘를 사용하기

**Bad**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Good**

```javascript
getUser();
```

### 3. 검색가능한 이름을 사용하기

**Bad**

```javascript
setTimeout(blastOff, 86400000);
```

**Good**

```javascript
const MILLISECONDS_IN_A_DAY = 86400000;
setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

### 4. 의도를 나타내는 변수들을 사용하기

**Bad**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Good**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

### 5. 자신만 알아볼수 있는 작명을 피하기

**Bad**

```javascript
const locations = ["서울", "성남", "수원"];
locations.forEach((l) => {
  doStuff();
  doSomeOtherStuff();

  dispatch(l);
});
```

**Good**

```javascript
const locations = ["서울", "성남", "수원"];
locations.forEach((location) => {
  doStuff();
  doSomeOtherStuff();

  dispatch(location);
});
```

### 6. 문맥상 필요없는 것들을 쓰지 말기

**Bad**

```javascript
const Car = {
  carMake: "BMW",
  carModel: "M3",
  carColor: "blue",
};

function paintCar(car) {
  car.carColor = "red";
}
```

**Good**

```javascript
const Car = {
  make: "BMW",
  model: "M3",
  color: "blue",
};

function paintCar(car) {
  car.color = "red";
}
```

### 7. 기본 매개변수가 short circuiting 트릭이나 조건문보다 깔끔하게 하기

기본 매개변수는 종종 short circuiting 트릭보다 깔끔합니다. 기본 매개변수는 매개변수가 undefined 일때만 적용됩니다. '', "", false, null, 0, NaN 같은 falsy한 값들은 기본 매개변수가 적용되지 않습니다.

**Bad**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**Good**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```
