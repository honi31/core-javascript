## 1. 실행 컨텍스트란?

**실행 컨텍스트**는 **실행할 코드에 제공할 환경 정보들을 모아놓은 객체**로, 자바스크립트의 동적 언어로서의 성격을 가장 잘 파악할 수 있는 개념이다. 여기서, 스택과 큐의 개념을 잠깐 살펴보자

**스택(Stack)**

- 출입구가 하나뿐인 깊은 우물과 같은 데이터 구조.
- 스택오버플로우 - 저장할 수 있는 데이터 용량보다 많은 양의 데이터를 넣으면 넘치는 현상, 프로그래밍 언어들은 스택이 넘칠 때 에러를 발생.

**큐(Queue)**

- 양쪽이 모두 열려있는 파이프와 같은 데이터 구조.
- 보통은 한쪽은 입력만, 다른 한쪽은 출력만을 담당한다. (양쪽 모두 가능한 큐도 존재하긴함)
  <img src = "https://velog.velcdn.com/images/honi31/post/d8da791f-c331-42f4-b8ee-c8679b3ec5d4/image.png" width="50%" >

동일한 환경에 있는 코드들을 실행할 때 필요한 환경 정보들을 모아 컨텍스트를 구성하고, 이를 **콜 스택**에 쌓아올렸다가, 가장 위에 쌓인 컨텍스트와 관련있는 코드를 실행하는 식으로 전체 코드의 환경과 순서를 보장한다.
-> 우리가 흔히 실행 컨텍스트를 구성하는 방법은 **함수를 실행**하는 것뿐이다.

**실행 컨텍스트와 콜 스택**

```javascript
// ------------------------------ (1)
var a = 1;
function outer() {
  function inner() {
    console.log(a); //undefined
    var a = 3;
  }
  inner(); // ------------------- (2)
  console.log(a); // 1
}

outer(); // -------------------- (3)
console.log(a); // 1
```

<img src="https://user-images.githubusercontent.com/101851472/223405318-da08a6d8-7b3f-4780-900a-67b4f59804ad.png" width="50%" >

**과정**
자바스크립트 코드를 실행하는 순간**((1))** 전역 컨텍스트가 콜 스택에 담긴다. **->** <br/> outer 함수를 호출하면 **((3))** 자바스크립트 엔진은 outer에 대한 환경 정보를 수집해서 outer 실행 컨텍스트를 생성한 후 콜 스택에 담는다. (콜 스택 맨 위에 outer 실행 컨텍스트가 놓인 상태가 됐으므로, 전역 컨텍스트와 관련된 코드의 실행을 일시 중단하고 대신 outer 실행 컨텍스트와 관련된 코드, 즉 outer 함수 내부의 코드들을 순차적으로 실행한다.) **->** <br/>inner 함수의 **((2))** 실행 컨텍스트가 콜 스택의 가장 위에 담기면 outer 컨텍스트와 관련된 코드의 실행을 중단하고 inner 함수 내부의 코드를 순서대로 진행한다. **->** <br/> inner 함수 실행 종료되면 콜 스택에서 제거됨 -> <br/>아래에 있던 outer 컨텍스트가 콜 스택 맨 위에 존재하게 되므로, **((2))**의 다음줄부터 이어서 실행 -> <br/> a 변수 출력하면 outer 실행 컨텍스트가 콜 스택에서 제거되고, 전역 컨텍스트만 남게됨 -> <br/> 실행을 중단했던 **((3))** 다음줄 이어서 실행 -> <br/> a 변수 값 출력하면 전역 공간에 실행할 코드가 없어 전역 컨텍스트도 제거. 콜 스택 비어진 채로 종료

어떤 실행 컨텍스트가 활성화될 때 자바스크립트 엔진은 해당 컨텍스트에 관련된 코드들을 실행하는 데 필요한 환경 정보들을 수집해서 실행 컨텍스트 객체에 저장한다. 이 객체는 자바스크립트 엔진이 활용할 목적으로 생성!

담기는 정보들

- VariableEnvironment : 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보, 선언 시점의 LexicalEnvironment의 스냅샷(특정 시점)으로, 변경 사항은 반영되지 않음.
- LexicalEnvironment : 처음에는 VariableEnvironment와 같지만 변경 사항이 실시간으로 반영됨.
- ThisBinding : this 식별자가 바라봐야 할 대상 객체.

---

## 2. VariableEnvironment

- VariableEnvironment에 담기는 정보는 LexicalEnvironment와 같지만 최초 실행 시의 스냅샷을 유지한다는 점이 다르다.
- 실행 컨텍스트를 생성할 때 VariableEnvironment에 정보를 먼저 담은 다음, 이를 그대로 복사해서 LexicalEnvironment를 만들고, 이 LexicalEnvironment를 주로 활용한다.
- 내부는 environmentRecord와 outer-EnvironmentReference로 구성되어있다.

---

## 3. LexicalEnvironment

LexicalEnvironment에 대한 한국어 번역은 문서마다 다르지만 '어휘적 환경', '정적 환경'이라는 단어가 가장 많이 등장한다. 하지만 이보다는 **'사전적인'**이 더욱 어울리는 표현이라고 생각한다.
-> '현재 컨텍스트 내부에는 a, b, c와 같은 식별자들이 있고 그 외부 정보는 D를 참조하도록 구성돼있다.' **컨텍스트를 구성하는 (수시로 변하는) 환경 정보들을 사전에서 접하는 느낌**

### 3.1 environmentRecord와 호이스팅

environmentRecord에는 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장된다.

- 컨텍스트를 구성하는 함수에 지정된 매개변수 식별자
- 선언한 함수가 있을 경우 그 함수 자체
- var로 선언된 변수의 식별자

컨텍스트 내부 전체를 처음부터 끝까지 훑으면서 **순서대로** 수집한다.
따라서, 자바스크립트 엔진은 이미 해당 환경의 변수명들을 모두 알게된다. 즉, 식별자들을 최상단으로 끌어올려놓은 다음 실제 코드를 실행한다.
-> **호이스팅**, 자바스크립트 엔진이 실제로 끌어올리지는 않지만 편의상 그런것으로 간주하자!

#### 호이스팅 규칙

매개변수와 변수에 대한 호이스팅(1) - 원본 코드

```javascript
function a(x) {
  // 수집 대상 1(매개변수)
  console.log(x); // (1) 예상 : 1
  var x; // 수집 대상 2(변수 선언)
  console.log(x); // (2) 예상 : undefined
  var x = 2; // 수집 대상 3(변수 선언)
  console.log(x); // (3) 예상 : 2
}
```

매개변수와 변수에 대한 호이스팅(2) - 매개변수를 변수 선언/할당과 같다고 간주해서 변환한 상태

```javascript
function a() {
  var x = 1;
  console.log(x); // (1)
  var x;
  console.log(x); // (2)
  var x = 2;
  console.log(x); // (3)
}
```

매개변수와 변수에 대한 호이스팅(3) - 호이스팅을 마친 상태

```javascript
function a() {
  var x; // 수집대상 1 변수 x 선언 → 메모리에서 저장 공간 확보, 주솟값을 변수 x에 연결
  var x; // 수집대상 2 변수 x 선언 → 무시
  var x; // 수집대상 3 변수 x 선언 → 무시

  x = 1; // 수집대상 1, x에 1을 할당 → 1을 별도의 메모리에 담고, 주솟값 입력
  console.log(x); // 1
  console.log(x); // 1
  x = 2; // 수집대상 3, x에 2를 할당
  // → 2을 별도의 메모리에 담고, 1을 가리키는 주솟값을 2를 가리키는 주소값으로 대치
  console.log(x); // 2
} // 모든 코드 실행 후 실행 컨텍스트 종료

a(1);
```

#### 함수 선언문과 함수 표현식

함수를 새롭게 정의할 때 사용

- 함수 선언문(function declaration) : function 정의부만 존재, 할당 명령 없음. 반드시 함수명 정의
- 함수 표현식(function expression) : 정의한 function을 별도의 변수에 할당. 함수명 정의 없어도 됨.
  - 기명 함수 표현식 : 함수명을 정의한 함수 표현식.
  - 익명 함수 표현식 : 일반적인 함수 표현식, 함수명 정의 X

```javascript
function a() {} // 함수 선언문, 함수명 a가 곧 변수명
a(); // 실행 o

var b = function () {}; // (익명) 함수 표현식, 변수명 b가 곧 함수명
b(); // // 실행 o

var c = function d() {}; // 기명 함수 표현식, 변수명: c, 함수면: d
c(); // 실행 o
d(); // error (함수명은 오직 내부에서만 접근 가능)
```

이제, 함수 선언문과 함수 표현식의 호이스팅 시 차이점을 알아보자.
함수 선언문은 전체를 호이스팅한 반면, 함수 표현식은 변수 선언문만 호이스팅 한다. 함수를 다른 변수에 값으로써 '할당'한 것이 곧 함수 표현식이고, 할당부 이후부터 실행 가능하다!

```javascript
var sum = function sum(a, b) {
  // 함수 선언문은 전체를 호이스팅.
  return a + b;
};

var multifly; // 변수는 선언문만 끌어올림.
console.log(sum(1, 2));
console.log(multifly(3, 4)); // ERROR : multifly is not a function

multifly = function (a, b) {
  // 변수의 할당부는 원래 자리에 남겨둔다.
  return a + b;
};
```

**주의할 점**
동일한 변수명에 서로 다른 값을 할당할 경우, 나중에 할당한 값이 먼저 할당한 값을 덮어씌운다 (override). -> 코드를 실행하는 중에 실제로 호출되는 함수는 오직 마지막에 할당한 함수, 즉 맨 마지막에 선언된 함수 뿐이다.
따라서 전역공간에 함수를 선언하거나 자바스크립트에서 변수/함수를 선언할 때 동일한 변수명으로 중복 선언하는 경우는 없어야한다. 또한, 안전하게 **함수 표현식으로 선언**하여 코드를 작성하자 !
사실.. 지역변수로 만들었다면 훨씬 더 안전하다 (밑에서 전역변수와 지역변수를 자세히 알아보자)

### 3.2 스코프, 스코프 체인, outerEnvironmentReference

스코프란 식별자에 대한 유효범위.

- ES5까지의 자바스크립트는 특이하게도 전역공간을 제외하면 **오직 함수에 의해서만** 스코프가 생성된다.
- ES6에서는 블록에 의해서도 스코프 경계가 발생하게 변경되었고, let, const, class, strict mode에서의 함수 선언 등에 대해서만 범위로서의 역할을 수행한다.

  - 둘을 구분하기 위해 함수 스코프, 블록 스코프

이러한 식별자의 유효범위를 안에서부터 바깥으로 차례로 검색해나가는 것을 **스코프 체인**이라 한다. -> outerEnvironmentReference로 가능

#### 스코프 체인

outerEnvironmentReference는 현재 호출된 함수가 **선언될 당시**(과거 시점)의 LexicalEnvironment를 참조한다. -> 선언하다: 콜 스택 상에서 어떤 실행 컨텍스트가 활성화된 상태
outerEnvironmentReference는 연결리스트(linked list) 형태를 띤다. 선언 시점의 LexicalEnvironement를 계속 찾아 올라가면 마지막엔 전역 컨텍스트의 LexicalEnvironment가 있을 것이다.
여러 스코프에서 동일한 식별자를 선언한 경우에는 ** 무조건 스코프 체인 상에서 가장 먼저 발견된 식별자에만 접근 가능**

```javascript
var a = 1;
var outer = function () {
  var inner = function () {
    console.log(a); //undefined
    var a = 3;
  };
  inner();
  console.log(a); // 1
};
outer(); // 전역 컨텍스트 임시 종료 -> 2번째 줄 Outer 실행 컨텍스트 시작
console.log(a); // 1
```

위 코드에 대한 스코프 체인을 간단히 요약한 표를 보자.
L.E(LexicalEnvironment) / e(environmentRecord) / o(outerEnvironmentReference) / [숫자] (코드 줄 번호)
![](https://velog.velcdn.com/images/honi31/post/45092430-dacd-4e49-a5d9-28e5e2013eb5/image.png)

전역 컨텍스트 → outer 컨텍스트 → inner 컨텍스트

점차 규모는 작아지는 반면,
스코프 체인을 타고 접근 가능한 변수의 수는 늘어난다!

**변수 은닉화** : inner 함수 내부에서 a 변수를 선언했기 때문에 inner 스코프의 LexicalEnvironment부터 검색할 수 밖에 없다. 전역 공간에서 선언한 동일한 이름의 a 변수에는 접근할 수 없음

#### 전역변수와 지역변수

전역변수(global variable) : 전역 스코프에서 선언한 변수(ex. a와 outer)
지역변수(local variable) : 함수 내부에서 선언한 변수(ex. inner와 a)

## 4. this

실행 컨텍스트의 thisBinding에는 this로 지정된 객체가 저장된다. 실행 컨텍스트 활성화 당시에 this가 지정되지 않은 경우 this에는 전역 객체가 저장된다.
그밖에는 함수를 호출하는 방법에 따라 this에 저장되는 대상이 다르다. -> 3장에서 자세히 알아보자
