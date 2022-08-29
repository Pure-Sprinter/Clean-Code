## 유형 별 클린코드 체크포인트

아래의 체크포인트를 보면서 스스로가 좋은 코드를 짰는지 화인하게 됩니다.

### 1. 변수 Variables

- [x] : 의미있고 발음하기 쉬운 변수 이름 사용하기
- [x] : 동일한 유형의 변수에 동일한 어휘를 사용하기
- [x] : 검색가능한 이름을 사용하기
- [x] : 의도를 나타내는 변수들을 사용하기
- [x] : 자신만 알아볼수 있는 작명을 피하기
- [x] : 문맥상 필요없는 것들을 쓰지 말기
- [x] : 기본 매개변수가 short circuiting 트릭이나 조건문보다 깔끔하게 하기

### 2. 함수 Functions

- [x] : 함수 인자는 2개 이하가 이상적이다
- [x] : 함수는 하나의 행동만 해야합니다
- [x] : 함수명은 함수가 무엇을 하는지 알 수 있어야 합니다
- [x] : 함수는 단일 행동을 추상화 해야합니다
- [x] : 중복된 코드를 작성하지 마세요
- [x] : Object.assign을 사용해 기본 객체를 만드세요
- [x] : 매개변수로 분기(플래그)를 사용하지 마세요
- [x] : 사이드 이펙트를 피하세요 (return [...cart, {요소}]로 사용하기)
- [x] : 전역 함수를 사용하지 마세요
- [x] : 명령어 프로그래밍보다 함수형 프로그래밍을 지향하세요
- [x] : 조건문을 캡슐화하세요
- [x] : 부정조건문을 사용하지 마세요
- [x] : 조건문 작성을 피하세요
- [x] : 타입-체킹을 피하세요
- [x] : 과도한 최적화를 지양하세요
- [x] : 죽은 코드를 지우세요

### 3. 객체와 자료구조 Objects and Data Structures

- [x] : getter와 setter를 사용하세요
- [x] : 객체에 비공개 멤버를 만드세요

### 4. 클래스 Classes

- [x] : ES5의 함수보다 ES2015/ES6의 클래스를 사용하세요
- [x] : 메소드 체이닝을 사용하세요
- [x] : 상속보다 조합(Composition)을 사용하세요

### 5. 객체 지향 원칙 SOLID

- [x] : 단일 책임 원칙 (Single Responsibility Principle, SRP)
- [x] : 개방/폐쇄 원칙 (Open/Closed Principle, OCP)
- [x] : 리스코프 치환 원칙 (Liskov Substitution Principle, LSP)
- [x] : 인터페이스 분리 원칙 (Interface Segregation Principle, ISP)
- [x] : 의존성 역전 원칙 (Dependency Inversion Principle, DIP)

### 6. 테스트 Testing

- [x] : 테스트 코드를 주제별로 잘 분리하였는지

### 7. 동시성 Concurrency

- [x] : Callback 대신 Promise를 사용하세요
- [x] : Async/Await는 Promise보다 더욱 깔끔합니다

### 8. 에러 처리 Error Handling

- [x] : 단순히 에러를 확인만 하지마세요
- [x] : Promise가 실패된 것을 무시하지 마세요

### 9. 포맷팅 Formatting

- [x] : 일관된 대소문자를 사용하세요
- [x] : 함수 호출자와 함수 피호출자는 가깝게 위치시키세요

### 10. 주석 Comments

- [x] : 비즈니스 로직이 복잡한 경우에만 주석을 다세요
- [x] : 주석으로 된 코드를 남기지 마세요
