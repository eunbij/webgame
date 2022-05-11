### 1. 순서도 그리기

- 발생하는 이벤트별로 순서도를 끊기
- 저장할 변수(숫자1, 연산자, 숫자2) 만들기 | 숫자버튼 클릭 시 변수에 저장 | 연산자 버튼 클릭 시 변수에 저장 | = 버튼 클릭 시 숫자1과 숫자2에 연산자를 적용해 계산하고 결과를 출력

### 2. 계산기 화면 만들고 이벤트 달기

```html
<!-- 계산기 버튼 구현 -->
<input readonly id="operator" />
<input type="number" id="result" />
<div class="row">
  <button id="num-7">7</button>
  <button id="num-8">8</button>
  <button id="num-9">9</button>
  <button id="plus">+</button>
</div>
<div class="row">
  <button id="num-4">4</button>
  <button id="num-5">5</button>
  <button id="num-6">6</button>
  <button id="minus">-</button>
</div>
<div class="row">
  <button id="num-1">1</button>
  <button id="num-2">2</button>
  <button id="num-3">3</button>
  <button id="divide">/</button>
</div>
<div class="row">
  <button id="clear">C</button>
  <button id="num-0">0</button>
  <button id="calculate">=</button>
  <button id="multiply">x</button>
</div>
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1c49168d-1f02-43db-9e4d-16c5ae96452f/Untitled.png)

**각 버튼에 이벤트 리스너 달기 (총 버튼 16개)**

1. 이벤트 리스너를 모두 달고 개선점 찾기

```jsx
let numOne = "";
let operator = "";
let numTwo = "";
const $operator = document.querySelector("#operator");
const $result = document.querySelector("#result");
document.querySelector("#num-0").addEventListener("click", () => {});
document.querySelector("#num-1").addEventListener("click", () => {});
document.querySelector("#num-2").addEventListener("click", () => {});
document.querySelector("#num-3").addEventListener("click", () => {});
document.querySelector("#num-4").addEventListener("click", () => {});
document.querySelector("#num-5").addEventListener("click", () => {});
document.querySelector("#num-6").addEventListener("click", () => {});
document.querySelector("#num-7").addEventListener("click", () => {});
document.querySelector("#num-8").addEventListener("click", () => {});
document.querySelector("#num-9").addEventListener("click", () => {});
document.querySelector("#plus").addEventListener("click", () => {});
document.querySelector("#minus").addEventListener("click", () => {});
document.querySelector("#divide").addEventListener("click", () => {});
document.querySelector("#multiply").addEventListener("click", () => {});
document.querySelector("#calculate").addEventListener("click", () => {});
document.querySelector("#clear").addEventListener("click", () => {});
```

2. 일부만 작성한 다음 규칙을 찾아 개선하기

```jsx
let numOne = '';
let operator = '';
let numTwo = '';
const $operator = document.querySelector('#operator');
const $result = document.querySelector('#result');
// 버튼 중 #num-0, #num-1 일부로 우선 접근하기
document.querySelector('#num-0').addEventListener('click',()=>{
    **if(operator){
        numTwo += '0';
    }else{
        numOne += '0';
    }
    $result.value += '0';**
});
document.querySelector('#num-1').addEventListener('click',()=>{
    **if(operator){
        numTwo += '1';
    }else{
        numOne += '1';
    }
    $result.value += '1';**
});
```

### 3. 고차 함수로 중복 제거하기

중복 제거를 위해 함수의 특성을 이용 (호출하면 값을 반환 = 함수가 함수를 반환하도록)

```jsx
const func = () => {
  return () => {
    console.log("hello");
  };
};
```

```jsx
const innerFunc = func();
innerFunc(); // hello
```

🔽hello 문자열을 다른 값으로 바꾸고 싶을 때는 [매개변수] 사용

```jsx
const func = (msg) => {
  return () => {
    console.log(msg);
  };
};
```

<aside>
✅ **고차함수**

함수를 만드는 함수

함수의 본문에서 바로 반환되는 값이 있으면 {와 return 생략 가능

```jsx
const func = (msg) => () => {
  console.log(msg);
};
```

</aside>

**이벤트 리스너를 연결하는 코드에서 숫자 제외한 나머지 부분이 같음
⇒ 고차 함수를 사용해 0~9는 매개변수로 만들고 다른 부분은 함수로 만들기**

```jsx
// 고차 함수를 사용하여 0~9 매개 변수로 만들고 다른 부분은 함수로 만들기
const onClickNumber = (number) => (event) => {
  if (operator) {
    numTwo += number;
  } else {
    numOne += number;
  }
  $result.value += number;
};
```

```jsx
// 동일한 코드 return생략하지 않을 경우
const onClickNumber = (number) => {
  return (event) => {
    if (operator) {
      numTwo += number;
    } else {
      numOne += number;
    }
    $result.value += number;
  };
};
```

```jsx
// 이벤트 리스너 수정
**const onClickNumber = (number)=>()=>{
    if(operator){
        numTwo += number;
    }else{
        numOne += number;
    };**
    **$result.value += number;**
};
document.querySelector('#num-0').addEventListener('click', **onClickNumber('0')**);
document.querySelector('#num-1').addEventListener('click', **onClickNumber('1')**);
document.querySelector('#num-2').addEventListener('click', **onClickNumber('2')**);
document.querySelector('#num-3').addEventListener('click', **onClickNumber('3')**);
document.querySelector('#num-4').addEventListener('click', **onClickNumber('4')**);
document.querySelector('#num-5').addEventListener('click', **onClickNumber('5')**);
document.querySelector('#num-6').addEventListener('click', **onClickNumber('6')**);
document.querySelector('#num-7').addEventListener('click', **onClickNumber('7')**);
document.querySelector('#num-8').addEventListener('click', **onClickNumber('8')**);
document.querySelector('#num-9').addEventListener('click', **onClickNumber('9')**);
```

onClickNumber(’숫자’)의 중복 제거하기 → 숫자의 공통점 찾기 → 숫자는 모두 버튼 안에 들어 있는 문자
**→ 버튼 클릭할 때 버튼 내부의 문자를 event.target.textContent로 가져오기**

```jsx
// event.target.textContent를 이용하여 버튼 클릭할 때 버튼 내부의 문자 가져오기
const onClickNumber = (event)=>{
    if(operator){
        numTwo += **event.target.textContent**;
    }else{
        numOne += **event.target.textContent**;
    }
    $result.value += **event.target.textContent**;
};
document.querySelector('#num-0').addEventListener('click', **onClickNumber**);
document.querySelector('#num-1').addEventListener('click', **onClickNumber**);
document.querySelector('#num-2').addEventListener('click', **onClickNumber**);
document.querySelector('#num-3').addEventListener('click', **onClickNumber**);
document.querySelector('#num-4').addEventListener('click', **onClickNumber**);
document.querySelector('#num-5').addEventListener('click', **onClickNumber**);
document.querySelector('#num-6').addEventListener('click', **onClickNumber**);
document.querySelector('#num-7').addEventListener('click', **onClickNumber**);
document.querySelector('#num-8').addEventListener('click', **onClickNumber**);
document.querySelector('#num-9').addEventListener('click', **onClickNumber**);
```

**연산자 버튼을 누를 때도 고차 함수를 사용하여 연산자 저장**

```jsx
// 연산자 버튼을 누를 때도 고차 함수를 사용하여 연산자 저장
**const onClickOperator = (op)=>()=>{
    if(numOne){
        operator = op;
        $operator.value = op;
    }else{
        alert('숫자를 먼저 입력하세요.');
    }
};**
document.querySelector('#plus').addEventListener('click',**onClickOperator('+')**);
document.querySelector('#minus').addEventListener('click',**onClickOperator('-')**);
document.querySelector('#divide').addEventListener('click',**onClickOperator('/')**);
document.querySelector('#multiply').addEventListener('click',**onClickOperator('*')**);
```

### 4. 중첩 if문 줄이기

```jsx
let numOne = "";
let operator = "";
let numTwo = "";
const $operator = document.querySelector("#operator");
const $result = document.querySelector("#result");
const onClickNumber = (event) => {
  if (operator) {
    numTwo += event.target.textContent;
  } else {
    numOne += event.target.textContent;
  }
  $result.value += event.target.textContent;
};
document.querySelector("#num-0").addEventListener("click", onClickNumber);
document.querySelector("#num-1").addEventListener("click", onClickNumber);
document.querySelector("#num-2").addEventListener("click", onClickNumber);
document.querySelector("#num-3").addEventListener("click", onClickNumber);
document.querySelector("#num-4").addEventListener("click", onClickNumber);
document.querySelector("#num-5").addEventListener("click", onClickNumber);
document.querySelector("#num-6").addEventListener("click", onClickNumber);
document.querySelector("#num-7").addEventListener("click", onClickNumber);
document.querySelector("#num-8").addEventListener("click", onClickNumber);
document.querySelector("#num-9").addEventListener("click", onClickNumber);

// 연산자 버튼을 누를 때도 고차 함수를 사용하여 연산자 저장
const onClickOperator = (op) => () => {
  if (numOne) {
    operator = op;
    $operator.value = op;
  } else {
    alert("숫자를 먼저 입력하세요.");
  }
};
document.querySelector("#plus").addEventListener("click", onClickOperator("+"));
document
  .querySelector("#minus")
  .addEventListener("click", onClickOperator("-"));
document
  .querySelector("#divide")
  .addEventListener("click", onClickOperator("/"));
document
  .querySelector("#multiply")
  .addEventListener("click", onClickOperator("*"));
document.querySelector("#calculate").addEventListener("click", () => {});
document.querySelector("#clear").addEventListener("click", () => {});
```

두 번째 숫자(numTwo) 클릭 시 첫 번째 숫자와 문자열 결합이 되므로
numTwo 값 저장 전 화면을 비우는 작업 필요

```jsx
let numOne = '';
let operator = '';
let numTwo = '';
const $operator = document.querySelector('#operator');
const $result = document.querySelector('#result');
const onClickNumber = (event)=>{
    if(operator){
        **//numTwo 값 저장 전 비우기
        if(!numTwo){
            $result.value='';
        }**
        numTwo += event.target.textContent;
    }else{
        numOne += event.target.textContent;
    }
    $result.value += event.target.textContent;
};
document.querySelector('#num-0').addEventListener('click', onClickNumber);
document.querySelector('#num-1').addEventListener('click', onClickNumber);
document.querySelector('#num-2').addEventListener('click', onClickNumber);
document.querySelector('#num-3').addEventListener('click', onClickNumber);
document.querySelector('#num-4').addEventListener('click', onClickNumber);
document.querySelector('#num-5').addEventListener('click', onClickNumber);
document.querySelector('#num-6').addEventListener('click', onClickNumber);
document.querySelector('#num-7').addEventListener('click', onClickNumber);
document.querySelector('#num-8').addEventListener('click', onClickNumber);
document.querySelector('#num-9').addEventListener('click', onClickNumber);

// 연산자 버튼을 누를 때도 고차 함수를 사용하여 연산자 저장
const onClickOperator = (op)=>()=>{
    if(numOne){
        operator = op;
        $operator.value = op;
    }else{
        alert('숫자를 먼저 입력하세요.');
    }
};
document.querySelector('#plus').addEventListener('click',onClickOperator('+'));
document.querySelector('#minus').addEventListener('click',onClickOperator('-'));
document.querySelector('#divide').addEventListener('click',onClickOperator('/'));
document.querySelector('#multiply').addEventListener('click',onClickOperator('*'));
document.querySelector('#calculate').addEventListener('click',()=>{});
document.querySelector('#clear').addEventListener('click',()=>{});
```

<aside>
✅ **중첩 if문**

```jsx
if (조건A) {
  if (조건B) {
    if (조건C) {
    }
  } else {
    if (조건D) {
      //나는 어느 때 실행되지?
      if (조건E) {
      } else {
        // 나는?
      }
      // 나는?
    } else {
    }
  }
}
```

**중첩을 제거하는 방법**

1. if문 다음에 나오는 공통된 절차를 각 분기점 내부에 넣음
2. 분기점에서 짧은 절차부터 실행하게 if문을 작성
3. 짧은 절차가 끝나면 return(함수 내부의 경우)이나 break(for문 내부의 경우)로 중단
4. else 제거 [중첩 하나가 제거됨]
5. 다음 중첩된 분기점이 나오면 1~4의 과정을 반복
</aside>

작성된 코드에 중첩 제거 적용해보기

1. 공통적으로 실행되는 부분은 $result.value += event.target.textContent 이므로
   이 부분을 각 분기점 안에 넣음

```jsx
const onClickNumber = (event)=>{
    if(operator){
        //numTwo 값 저장 전 비우기
        if(!numTwo){
            $result.value='';
        }
        numTwo += event.target.textContent;
        **$result.value += event.target.textContent;**
    }else{
        numOne += event.target.textContent;
        **$result.value += event.target.textContent;**
    }
};
```

1. 분기점이 짧은 절차 확인

   operator가 없으면 절차가 더 짧으므로 먼저 작성

   조건문은 operator → !operator가 됨

```jsx
const onClickNumber = (event)=>{
    if(**!operator**){
        numOne += event.target.textContent;
        $result.value += event.target.textContent;
    }else{
        //numTwo 값 저장 전 비우기
        if(!numTwo){
            $result.value='';
        }
        numTwo += event.target.textContent;
        $result.value += event.target.textContent;
    }
};
```

1. !operator 일 때의 절차가 마무리되면 return으로 함수 종료
2. return 아랫부분은 무조건 operator일 때만 실행되므로 else문으로 감쌀 필요 없음

```jsx
const onClickNumber = (event)=>{
    if(!operator){
        numOne += event.target.textContent;
        $result.value += event.target.textContent;
        **return;**
    }
    **// 이 아래로는 operator가 존재하는 경우에만 실행됨**
    if(!numTwo){
        $result.value='';
    }
    numTwo += event.target.textContent;
    $result.value += event.target.textContent;
};
```

### 5. 결과 계산하기

#calculate 태그에 이벤트 리스너를 달아 결과 출력

→ 문자열에 따라 다른 연산자를 사용하도록 조건문 사용

```jsx
document.querySelector('#calculate').addEventListener('click',()=>{
    **//문자열에 따라 다른 연산자를 사용하도록 조건문 사용
    if(numTwo){
        switch(operator){
            case '+':
                $result.value = parseInt(numOne) + parseInt(numTwo);
                break;
            case '-':
                $result.value = numOne - numTwo;
                break;
            case '*':
                $result.value = numOne * numTwo;
                break;
            case '/':
                $result.value = numOne / numTwo;
                break;
            default:
                break;
        }
    }else{
        alert('숫자를 먼저 입력하세요');
    }
});**
```

parseInt 사용 이유 : 입력값을 문자열로 저장하였으므로 parseInt 함수를 통해 숫자로 바꾼 후 더함. 나머지 연산자(빼기, 곱하기, 나누기)는 자동으로 문자열을 숫자로 바꾼 후 계산하므로 사용하지 않아도 됨

<aside>
✅ **eval**

- 문자열을 자바스크립트 코드처럼 실행할 수 있음
- 코드 예시

```jsx
document.querySelector("#calculate").addEventListener("click", () => {
  if (numTwo) {
    $result.value = eval(numOne + operator + numTwo);
  } else {
    alert("숫자를 먼저 입력하세요.");
  }
});
```

- 주의점 : 코드를 남용하면 해커가 악용할 가능성이 있음. eval 함수에 문자열을 입력하면 그대로 실행되므로 프로그램에 위험한 코드를 실행할 수 있음.(보안상 취약)

[eval() - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/eval)

</aside>

### 6. 계산기 초기화하기

초기화 = 처음 상태로 되돌리는 것 = 변수와 함수 모두 초기 상태로 설정

```jsx
//계산기 초기화하기
document.querySelector('#clear').addEventListener('click',()=>{
    **numOne = '';
    operator = '';
    numTwo = '';
    $operator.value = '';
    $result.value = '';**
});
```

### \* 연이어 계산하기

=를 클릭할 때 계산 결과를 numOne에 저장하고 numTwo 변수를 비워두면 연이은 계산이 가능함

→ 1+2+4를 계산한다면 1+2를 계산해서 3을 저장하고, +와 4를 누르면 각각 operator와 numTwo에 저장

→ =를 누르면 3+4가 실행됨

```jsx
document.querySelector('#calculate').addEventListener('click',()=>{
    //문자열에 따라 다른 연산자를 사용하도록 조건문 사용
    if(numTwo){
        switch(operator){
            case '+':
                $result.value = parseInt(numOne) + parseInt(numTwo);
                break;
            case '-':
                $result.value = numOne - numTwo;
                break;
            case '*':
                $result.value = numOne * numTwo;
                break;
            case '/':
                $result.value = numOne / numTwo;
                break;
            default:
                break;
        }
        **$operator.value=''; // 저장된 연산자 초기화
        numOne = $result.value; // 첫번째 계산 결과 저장
        operator = ''; // 연산자 초기화
        numTwo = ''; // 두 번째 숫자 초기화**
    }else{
        alert('숫자를 먼저 입력하세요');
    }
});
```

### 최종 코드

```jsx
let numOne = "";
let operator = "";
let numTwo = "";
const $operator = document.querySelector("#operator");
const $result = document.querySelector("#result");
// event.target.textContent를 이용하여 버튼 클릭할 때 버튼 내부의 문자 가져오기
const onClickNumber = (event) => {
  if (!operator) {
    numOne += event.target.textContent;
    $result.value += event.target.textContent;
    return;
  }
  // 이 아래로는 operator가 존재하는 경우에만 실행됨
  if (!numTwo) {
    $result.value = "";
  }
  numTwo += event.target.textContent;
  $result.value += event.target.textContent;
};
document.querySelector("#num-0").addEventListener("click", onClickNumber);
document.querySelector("#num-1").addEventListener("click", onClickNumber);
document.querySelector("#num-2").addEventListener("click", onClickNumber);
document.querySelector("#num-3").addEventListener("click", onClickNumber);
document.querySelector("#num-4").addEventListener("click", onClickNumber);
document.querySelector("#num-5").addEventListener("click", onClickNumber);
document.querySelector("#num-6").addEventListener("click", onClickNumber);
document.querySelector("#num-7").addEventListener("click", onClickNumber);
document.querySelector("#num-8").addEventListener("click", onClickNumber);
document.querySelector("#num-9").addEventListener("click", onClickNumber);

// 연산자 버튼을 누를 때도 고차 함수를 사용하여 연산자 저장
const onClickOperator = (op) => () => {
  if (numOne) {
    operator = op;
    $operator.value = op;
  } else {
    alert("숫자를 먼저 입력하세요.");
  }
};
document.querySelector("#plus").addEventListener("click", onClickOperator("+"));
document
  .querySelector("#minus")
  .addEventListener("click", onClickOperator("-"));
document
  .querySelector("#divide")
  .addEventListener("click", onClickOperator("/"));
document
  .querySelector("#multiply")
  .addEventListener("click", onClickOperator("*"));
document.querySelector("#calculate").addEventListener("click", () => {
  //문자열에 따라 다른 연산자를 사용하도록 조건문 사용
  if (numTwo) {
    switch (operator) {
      case "+":
        $result.value = parseInt(numOne) + parseInt(numTwo);
        break;
      case "-":
        $result.value = numOne - numTwo;
        break;
      case "*":
        $result.value = numOne * numTwo;
        break;
      case "/":
        $result.value = numOne / numTwo;
        break;
      default:
        break;
    }
    // 연이어 계산하기
    $operator.value = "";
    numOne = $result.value;
    operator = "";
    numTwo = "";
  } else {
    alert("숫자를 먼저 입력하세요");
  }
});
//계산기 초기화하기
document.querySelector("#clear").addEventListener("click", () => {
  numOne = "";
  operator = "";
  numTwo = "";
  $operator.value = "";
  $result.value = "";
});
```
