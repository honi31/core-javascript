## 1. 데이터 타입의 종류

자바스크립트의 데이터 타입에는 크게 두가지가 있다. 바로 **기본형(원시 타입)**과 **참조형(참조 타입)**이다.

- **기본형** : 숫자, 문자열, 불리언, null, undefined, 심볼
  할당/연산 시 복제된다. -> 값이 담긴 주솟값을 바로 복제
  **불변성(immutability)**를 띈다.

- **참조형** : 객체, 함수, 배열, 날짜, 정규표현식 등..
  할단/연산 시 참조된다. -> 값이 담긴 주솟값들로 이루어진 묶음을 가리키는 주솟값을 복제

---

## 2. 데이터 타입에 관한 배경지식

모든 데이터는 바이트 단위의 식별자, 더 정확하게는 **메모리 주솟값**을 통해 서로 구분하고 연결한다.

- 변수 : '변할 수 있는 무언가', **데이터**를 의미(숫자, 문자열, 객체, 배열 등등)
- 식별자 : 어떤 데이터를 식별하는 데 사용하는 이름, **변수명**

---

## 3. 변수 선언과 데이터 할당

### 3.1 변수 선언

```javascript
var a;
```

**->** 변할 수 있는 데이터를 만든다. 이 데이터의 식별자는 a이다. **변수**란 결국 **변경 가능한 데이터가 담길 수 있는 공간**
변수를 선언하면 메모리 영역에선 어떤 작업이 수행될까? 간략하게 표로 표현했다.

| <center>주소</center> | 1002 | <center>1003</center> | <center>1004</center> | 1005 |
| :-------------------: | ---- | :-------------------- | :-------------------: | ---- |
|        데이터         |      | 이름: a <br />값:     |                       |      |

### 3.2 데이터 할당

```javascript
var a;
a = "abc";

var a = "abc";
```

-> 실제로 a 변수가 위치한 곳에 문자열 'abc'를 직접 저장하지는 않는다. **변수 할당 과정**은 **데이터를 저장 하기 위해 별도의 메모리 공간**을 다시 확보해서 문자열 'abc'를 저장하고,**그 주소를 변수 영역에 저장**하는 방식이다.

변수 영역

| <center>주소</center> | 1002 | <center>1003</center>   | <center>1004</center> | 1005 |
| :-------------------: | ---- | :---------------------- | :-------------------: | ---- |
|        데이터         |      | 이름: a <br />값: @5004 |                       |      |

데이터 영역

| <center>주소</center> | 5002 | <center>5003</center> | <center>5004</center> | 5005 |
| :-------------------: | ---- | :-------------------- | :-------------------: | ---- |
|        데이터         |      |                       |         'abc'         |      |

번거롭게 왜 변수 영역에 값을 저장하지 않고 데이터 영역에 저장하는 이유는 무엇일까요?

- 데이터 변환을 자유롭게 할 수 있게 함과 동시에 메모리를 더욱 효율적으로 관리
- 미리 확보한 공간 내에서만 데이터 변환이 가능하다면 변환한 데이터를 다시 저장하기 위해 확보된 공간을 변환된 데이터 크기에 맞게 늘리는 작업이 선행되어야함.
- 따라서, 문자열 데이터의 변환을 처리하기 위해 **변수와 데이터를 별도의 공간에 나눠서 저장**.
  문자열 'abc' 뒤에 'def'를 추가하라고 하면, 'abc'가 저장된 공간의 'abcdef'를 할당하는 것이 아닌 'abcdef'라는 문자열을 **새로 만들어 별도의 공간**에 저장하고 그 주소를 변수 영역과 연결한다.

---

## 4. 기본형 데이터와 참조형 데이터

### 4.1 불변값

- 변수(variable)와 상수(constant)를 구분하는 성질은 **'변경 가능성'**이다. 이때 변경 가능성의 대상은 **변수 영역** 메모리이다.
- 상수는 불변값이 아니다. 불변성 여부를 구분할 때의 변경 가능성의 대상은 **데이터 영역** 메모리이다.
- **기본형 데이터**(상수, 문자, boolean, null, undefined, symbol)는 모두 **불변값**이다.

**예시**

```javascript
var a = "abc";
a = a + "def";
// 기존 'abc'가 'abcdef'로 바뀌는 것이 아니라 새로운 문자열 'abcdef'를 만들어 그 주소를 변수 a에 할당
// 즉, 'abc'와 'abcdef'는 완전 별개의 데이터

var b = 5; // 변수 b에 5 할당. 데이터 영역에서 5를 찾고, 없으면 데이터 공간을 만들어 저장
var c = 5; // 같은 수 5 할당. 이미 만들어놓은 값이니 주소를 재활용
b = 7; // b의 값을 7로 변경. 기존 5를 변경하는 것이 아닌 데이터 영역에서 7을 찾거나 새로 생성해서 주소 저장
```

-> 불변값의 성질 : 문자열 값도, 숫자 값도 다른 값으로 변경할 수 없다. 변경은 새로 만드는 동작을 통해서만 이뤄지고, 한 번 만들어진 값은 가비지 컬렉션을 당하지 않는 한 변하지 않는다.

### 4.2 가변값

- 참조형 데이터의 기본적인 성질은 가변값이다.
- 설정에 따라 변경 불가능한 경우도 있고, 아예 불변값으로 활용하는 방안도 있다.

**예시 1**

```javascript
var obj1 = {
  a: 1,
  b: "bbb",
};
```

<img src="https://velog.velcdn.com/images/honi31/post/e009df5a-4ab0-43cb-995c-7cff386a8871/image.png" width="50%"/>

-> 변수 영역의 빈 공간 확보 후 obj1로 이름 지정 -> 데이터 영역(@5001) 생성, 그룹 내부의 프로퍼티들을 저장하기 위해 별도 변수 영역 마련 후 그 주소를 @5001에 저장 -> 객체 변수 영역에 각각 a, b로 이름 지정 ->a, b에 해당하는 값 데이터 저장을 위해 데이터 영역 검색 후 저장

기본형 데이터와의 차이는 '객체의 변수(프로퍼티) 영역'이 별도로 존재한다는 점.
-> 객체가 별도로 할애한 영역은 변수 영역이고 데이터 영역은 기존 메모리 공간을 그대로 활용하고 있다. 따라서 변수에는 언제든지 다른 값을 대입할 수 있으므로 참조형 데이터는 불변하지 않다고 하는 것이다.

**예시 2**

```javascript
obj1.a = 2;
```

<img src="https://velog.velcdn.com/images/honi31/post/0f8637e2-9049-4800-9096-951c5c25adf8/image.png" width="50%"/>
-> 데이터 영역에서 숫자 2 검색 후 없으므로 빈 공간에 저장 후 이 주소를 객체 변수 영역에 저장
새로운 객체가 만들어진 것이 아니라 객체 내부의 값만 변경됨!

**예시 3**
중접객체(Nested Object) : 참조형 데이터의 프로퍼티에 다시 참조형 데이터를 할당하는 경우

```javascript
var obj = {
  x: 3,
  arr: [3, 4, 5],
};
```

<img src="https://velog.velcdn.com/images/honi31/post/6ece471f-9024-43c3-81d4-6280a1a80ca5/image.png" width="50%" />

-> 위 코드의 arr 값을 str로 재할당하면 @5003을 참조하는 변수가 하나도 없게 된다. 이때 참조카운트가 0이 되므로 이 메모리 주소는 가비지 컬렉터의 수거 대상이 된다.

> 어떤 데이터에 대해 자신의 주소를 참조하는 변수의 개수를 참조 카운트라고 한다.

### 4.3 변수 복사

**- 기본형 데이터 vs. 참조형 데이터**

```javascript
var a = 10;
var b = a;

var obj1 = { c: 10, d: "ddd" };
var obj2 = obj1;
```

<img src="https://user-images.githubusercontent.com/101851472/222977018-9ff6a1cf-3424-4273-ab66-ad7574da5ba5.png" width="50%" />

-> 변수를 복사하는 과정은 기본형 데이터와 참조형 데이터 모두 같은 주소를 바라보게 되는 점에서 동일하다. 하지만, 데이터 할당 과정에서 차이가 있기 때문에 변수 복사 이후의 동작에도 큰 차이가 발생!

**- 변수 복사 이후 값 변경 결과 비교 - 객체의 프로퍼티 변경 시**

```javascript
var a = 10;
var b = a;
var obj1 = { c: 10, d: "ddd" };
var obj2 = obj1;

b = 15;
obj2.c = 20;
```

<img src="https://user-images.githubusercontent.com/101851472/222977605-f40307ed-5884-4907-a048-e5ee31f5ff28.png" width="50%" />
-> 기본형 데이터 같은 경우 @1002의 값이 달라진 반면, 참조형 데이터의 값은 변경되지 않고 obj1과 obj2는 여전히 같은 객체를 바라보고 있다.

주의) 변수의 값을 직접 변경할 때와 값이 아닌 내부 프로퍼티(obj2.c)를 변경할 때의 결과 비교임.
따라서, 같은 조건인 상태도 비교해보겠다.

**- 변수 복사 이후 값 변경 결과 비교 - 객체 자체를 변경했을 때**

```javascript
var a = 10;
var b = a;
var obj1 = { c: 10, d: "ddd" };
var obj2 = obj1;

b = 15;
obj2.c = { c: 20, d: "dd" };
```

<img src="https://user-images.githubusercontent.com/101851472/222978060-87c64d99-146c-4565-ad62-0af893cb2f75.png" width="50%" />
-> 참조형 데이터임에도 불구하고 값이 변경된 것을 볼 수 있다.

즉, 참조형 데이터가 **'가변값'** 이라고 설명할 때의 '가변'은 참조형 데이터 자체를 변경할 겨우가 아니라 **그 내부의 프로퍼티를 변경할 때만 성립**한다.

---

## 5. 불변 객체

### 5.1 불변 객체를 만드는 간단한 방법

참조형 데이터의 가변은 내부 프로퍼티를 변경할 때만 성립한다. 데이터 자체를 변경(새 데이터 할당)하고자 하면 기본형 데이터와 마찬가지로 **기존 데이터는 변하지 않는다.**
-> 내부 프로퍼티를 변경할 필요가 있을 때마다 객체 역시 불변성을 확보하는 방법이 필요.

- 매번 새로운 객체를 만들어 재할당하기로 규칙을 정하거나
- 자동으로 새로운 객체를 만드는 도구를 활용
- 불변 객체로 취급

**객체의 가변성에 따른 문제점**

```javascript
var user = {
  name: "Jaenam",
  gender: "male",
};

var changeName = function (user, newName) {
  var newUser = user;
  newUser.name = newName;
  return newUser;
};

var user2 = changeName(user, "Jung");

console.log(user.name, user2.name); // Jung Jung
console.log(user === user2); // true
```

값으로 전달받은 객체에 변경을 가하더라도 원본 객체는 변하지 않아야 하는 경우 발생 -> **불변객체 필요**

**기존 정보를 복사해서 새로운 객체를 반환하는 함수(얕은 복사)**

```javascript
// 얕은 복사
var copyObject = function (target) {
  var result = {};
  for (var prop in target) {
    result[prop] = target[prop];
  }
  return result;
};
```

copyObject를 이용한 객체 복사
프로퍼티 갯수에 상관없이 모든 프로퍼티를 복사하는 함수를 만들 수 있다.
물론 협업하는 모든 개발자들이 copyObject라는 얕은 복사를 사용하면 문제가 되지 않는다. (=user 객체가 곧 불변 객체)
하지만, 그 규칙을 지키지 않을 수도 있으므로 아예 프로퍼티를 변경할 수 없게 제약을 거는게 더 안전하다.

### 5.2 얕은 복사와 깊은 복사

**얕은 복사(shallow copy)**는 바로 아래 단계의 값만 복사하는 방법이고, **깊은 복사(deep copy)**는 내부의 모든 값들을 하나하나 찾아서 전부 복사하는 방법이다.

**중첩된 객체에 대한 깊은 복사**

```javascript
var user2 = copyObject(user);
user2.urls = copyObject(user.urls);

user.urls.portfolio = "http://portfolio.com";
console.log(user.urls.portfolio === user2.urls.portfolio); // false

user.urls.blog = "";
console.log(user.urls.blog === user2.urls.blog); // false
```

-> 객체를 복사할 때 객체 내부의 모든 값을 복사해서 완전히 새로운 데이터를 만들고자 할 때, 기본형 데이터일 경우에는 그대로 복사하면 되지만 **참조형 데이터는 다시 그 내부의 프로퍼티들을 복사**해야 함

```javascript
// 깊은 복사
var copyObjectDeep = function (target) {
  var result = {};
  if (typeof target === "object" && target !== null) {
    for (var prop in target) {
      result[prop] = copyObjectDeep(target[prop]);
    }
  } else {
    result = target;
  }
  return result;
};
```

→ 참조형 데이터가 있을 때마다 재귀적으로 함수를 호출하여 객체를 완전히 복사해야 한다!

**JSON을 활용한 간단한 깊은 복사**
(메서드나 숨겨진 프로퍼티인 **proto**나 getter/setter등과 같이 JSON으로 변경할 수 없는 프로퍼티들은 모두 무시)

```javascript
var copyObjectViaJSON = function (target) {
  return JSON.parse(JSON.stringify(target));
};
var obj = {
  a: 1,
  b: {
    c: null,
    d: [1, 2],
    func1: function () {
      console.log(3);
    },
  },
  func2: function () {
    console.log(4);
  },
};
var obj2 = copyObjectViaJSON(obj);

obj.a = 3;
obj.b.c = 4;
obj.b.d[1] = 3;

console.log(obj); // {a: 1, b: {c: null, d: [1,3], func1: f() }, func2: f() }
console.log(obj2); // {a: 3, b: {c: 4, d: [1,2]}
```

---

## 6. undefined와 null

undefined와 null은 자바스크립트에서 모두 **'없음'**을 나타낸다. 의미는 같지만 차이점이 있고 사용하는 목적 또한 다르다.

### 6.1 undefined

사용자가 명시적으로 지정할 수도 있지만, 값이 존재하지 않을 때 자바스크립트 엔진이 자동으로 부여하는 경우도 있다.
자바스크립트 엔진은 사용자가 응당 어떤 값을 지정할 것이라고 예상되는 상황임에도 실제로 그러지 않을 때 undefined를 반환한다.

- (1) 값을 대입하지 않은 변수, 즉 데이터 영역의 메모리 주소를 지정하지 않은 식별자에 접근할 때
- (2) 객체 내부의 존재하지 않는 프로퍼티에 접근하려고 할 때
- (3) return 문이 없거나 호출되지 않는 함수의 실행 결과

(1)의 경우에 대해 배열의 경우에는 조금 특이한 동작을 확인할 수 있다.

```javascript
var arr1 = [];
arr1.length = 3;
console.log(arr1); // [empty * 3]

var arr2 = new Array(3);
console.log(arr2); // [empty * 3]

var arr3 = [undefined, undefined, undefined];
console.log(arr3); // [undefined, undefined, undefined]
```

arr1,arr2는 배열에 3개의 빈 요소를 확보했지만 확보된 각 요소에 문자 그대로 어떤 값도, undefined도 할당되어 있지않다. 이처럼 '비어있는 요소'와 'undefined'를 할당한 요소는 출력 결과부터 다르다.

### 6.2 null

비어있음을 명시적으로 나타낼 때 사용한다.
주의할 점은 typeof null이 object라는 점이다. 이는 자바스크립트 자체 버그이며 어떤 변수의 값이 Null인지 판별하기 위해서는 typeof 대신 다른 방식으로 접근해야한다.

**undefined와 null의 비교**

```javascript
var n = null;
console.log(typeof n); // object

console.log(n == undefined); // true
console.log(n == null); // true

console.log(n === undefined); // false
console.log(n === null); // true
```

동등연산자(==)로는 실제로 null인지 undefined인지 비교로 알 수 없다. 따라서 일치 연산자(===)로 정확히 판별할 수 있다.
