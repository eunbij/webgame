### 1. 순서도 그리기

- 프로그램 절차를 만들 때의 원칙
  1. 프로그램 절차의 개수는 정해져 있어야 한다
  2. 각 절차는 항상 같은 내용이어야 한다
  3. 모든 가능성을 고려해야 한다
  4. 예시는 절차를 검증하는 데 사용한다

![IMG_1382.heic](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70dfee49-79d3-445d-8316-90df9f02b340/IMG_1382.heic)

- 절차
  1. 몇 명이 참가할지 선택 → 2) 참가자 순서를 정하기 → 3) 첫 번째 사람이 제시어 말하기 → 4) 다음 사람이 단어를 말하기 → 5) 단어가 올바른가?
  — 예→ 4)로 돌아가서 반복
  — 아니오 → 6) 틀렸다고 표시한다 → 끝

### 2. HTML 화면 만들기

```html
<body>
  <div><span id="order">1</span>번째 참가자</div>
  <div>제시어 : <span id="word"></span></div>
  <input type="text" />
  <button>입력</button>
  <script></script>
</body>
```

### 3. 값 입력받아 변수에 저장하기

1. 몇 명이 참가할지 선택

- prompt()에 전달받은 값을 변수에 저장

```jsx
const number = prompt("몇 명이 참가하나요?");
```

<aside>
✅ 대화상자

prompt : 입력한 메시지가 문자열 형태로 전달 | 입력하지 않고 취소를 누르면 null 전달

alert : 단순한 알림창. 확인을 누르기 전까지 다음 스크립트 실행이 중단됨

confirm : 확인을 받을 때 사용. 확인=true, 취소=false

</aside>

### 4. HTML 태그 선택하기

```jsx
const $input = document.querySelector("input");
console.log($input);
const $button = document.querySelector("button");
console.log($button);
```

<aside>
✅ HTML 태그 선택하기

```jsx
// 여러개의 태그를 선택할 때
document.querySelectorAll(’선택자 || .클래스’)
// 하나의 태그만 선택
document.querySelector(’선택자 || #아이디’)
// 태그 안에 들어 있는 다른 태그 선택
document.querySelector(’선택자 내부선택자 내부선택자...’)
```

</aside>

### 5. 태그에 이벤트 달기

```jsx
const onClickButton = () => {
  console.log("버튼 클릭");
};
const $button = document.querySelector("button");
$button.addEventListener("click", onClickButton);
// onClickButton은 콜백 함수 = 특정 작업이 실행되고 난 뒤에 추가로 실행되는 함수를 의미 | 버튼을 클릭한 후 추가적으로 실행되므로
```

<aside>
✅ 콜백함수 onClickButton

- 특정 작업이 실행되고 난 뒤에 추가로 실행되는 함수를 의미
- 버튼을 클릭한 후에 onClickButton이 추가적으로 실행되므로 콜백 함수에 해당
</aside>

<aside>
✅ 이벤트 연결

```jsx
태그.addEventListener(’이벤트 이름’, 리스너함수);
```

</aside>

### 6. 첫 단어를 입력한 사람인지 판단하기

```jsx
// 순서도에 맞추에 코드 재정렬
const number = Number(prompt("몇 명이 참가하나요?"));
const $button = document.querySelector("button");
const $input = document.querySelector("input");

const onClickButton = () => {};
const onInput = () => {};

$button.addEventListener("click", onClickButton);
$input.addEventListener("input", onInput);
```

사용자가 *이벤트를 발생*시키지 않으면 다음 절차로 넘어가지 않으므로 순서도를 한 번 끊어주는 게 좋음

**첫 번째 참가자인가?
= 제시어가 비어 있는가?
⇒ 아무도 단어를 입력하지 않았으므로 값을 입력하는 사람이 곧 첫 번째 참가자라는 의미**

```jsx
const number = Number(prompt('몇 명이 참가하나요?'));
const $button = document.querySelector('button');
const $input = document.querySelector('input');
**let word; //제시어**

const onClickButton = ()=>{
    **if(!word){ //제시어가 비어 있는가?
        //비어 있음
    }else{
        //비어있지 않음
    }**
};
const onInput = (event)=>{
    **console.log('글자 입력',event.target.value);**
};

$button.addEventListener('click',onClickButton);
$input.addEventListener('input',onInput);
```

<aside>
✅ 매개변수 event 객체를 제공해서 이벤트 관련 정보 얻기

```jsx
const 리스너함수 = (event) => {
  console.log(event.target.value); //input태그에 입력된 값 가져오기
};
```

</aside>

변수

word : 제시어를 저장

newWord : 입력한 단어가 제시어가 되고 이 제시어를 저장

```jsx
const number = Number(prompt('몇 명이 참가하나요?'));
const $button = document.querySelector('button');
const $input = document.querySelector('input');
**const $word = document.querySelector('#word');**
let word; //제시어
**let newWord; //현재 단어**

const onClickButton = ()=>{
    if(!word){ //제시어가 비어 있는가?
        //비어 있음
        **word = newWord; // 입력한 단어가 제시어가 됨**
        **$word.textContent = word;** // **화면에 제시어 표시**
    }else{
        //비어있지 않음
    }
};
const onInput = (event)=>{
    **newWord = event.target.value; // 입력하는 단어를 현재 단어로**
};

$button.addEventListener('click',onClickButton);
$input.addEventListener('input',onInput);
```

<aside>
✅ textContent 속성을 사용하여 일반 태그의 내부 값 가져오기

```jsx
태그.textContent; //태그 내부의 문자를 가져옴
태그.textContent = 값; //태그 내부의 문자를 해당 값으로 설정함
```

</aside>

### 7. 올바른 단어인지 판단하기

<aside>
✅ 문자열[자릿수]

문자열의 길이와 상관없이 항상 마지막 문자를 가져오려면, 글자 수에서 1을 빼면 된다

</aside>

**접근 순서**

문자열 길이 구하기 → 마지막 문자 가져오기 → 제시어와 현재 단어 비교하기
⇒ word의 마지막 문자를 구한 뒤 현재 단어 newWord의 첫 번째 문자와 비교

```jsx
const number = Number(prompt("몇 명이 참가하나요?"));
const $button = document.querySelector("button");
const $input = document.querySelector("input");
const $word = document.querySelector("#word");
let word; //제시어
let newWord; // 현재 단어

const onClickButton = () => {
  if (!word) {
    //제시어가 비어 있는가?
    //비어 있음
    word = newWord; // 입력한 단어가 제시어가 됨
    $word.textContent = word; // 화면에 제시어 표시
  } else {
    //비어있지 않음
    if (word[word.length - 1] === newWord[0]) {
      // **입력한 단어가 올바른가?**
      //올바름
      word = newWord; //**현재 단어를 제시어에 저장**
      $word.textContent = word; // **화면에 제시어 표시**
    } else {
      //올바르지 않음
    }
  }
};
const onInput = (event) => {
  newWord = event.target.value; // 입력하는 단어를 현재 단어로
};

$button.addEventListener("click", onClickButton);
$input.addEventListener("input", onInput);
```

**다음 사람에게 순서를 넘긴다
→ 참가자가 3명일 경우 number = 3
→ 현재 순서가 1이라면 1을 더한 값이 2가 되어 number보다 작음
→ 2를 다음 순서로 설정**

```jsx
const number = Number(prompt('몇 명이 참가하나요?'));
const $button = document.querySelector('button');
const $input = document.querySelector('input');
const $word = document.querySelector('#word');
**const $order = document.querySelector('#order');**
let word; //제시어
let newWord; // 현재 단어

const onClickButton = ()=>{
    if(!word){ //제시어가 비어 있는가?
        //비어 있음
        word = newWord; // 입력한 단어가 제시어가 됨
        $word.textContent = word; // 화면에 제시어 표시
        **const order = Number($order.textContent);
        if(order+1 > number){
            $order.textContent = 1;
        }else{
            $order.textContent = order+1;
        }**
    }else{
        //비어있지 않음
        if(word[word.length-1]===newWord[0]){ // 입력한 단어가 올바른가?
            //올바름
            word = newWord; //현재 단어를 제시어에 저장
            $word.textContent = word; // 화면에 제시어 표시
            **const order = Number($order.textContent);
            if(order+1 > number){
                $order.textContent =1;
            }else{
                $order.textContent = order + 1;
            }**
        }else{
            //올바르지 않음
        };
    }
};
const onInput = (event)=>{
    console.log('글자 입력',event.target.value);
    newWord = event.target.value; // 입력하는 단어를 현재 단어로
};

$button.addEventListener('click',onClickButton);
$input.addEventListener('input',onInput);
```

### 8. 틀렸을 때 오류 표시하기

**올바르지 않은 단어를 입력한 경우, alert으로 알려주기**

```jsx
const number = Number(prompt('몇 명이 참가하나요?'));
const $button = document.querySelector('button');
const $input = document.querySelector('input');
const $word = document.querySelector('#word');
const $order = document.querySelector('#order');
let word; //제시어
let newWord; // 현재 단어

const onClickButton = ()=>{
    if(!word){ //제시어가 비어 있는가?
        //비어 있음
        word = newWord; // 입력한 단어가 제시어가 됨
        $word.textContent = word; // 화면에 제시어 표시
        const order = Number($order.textContent);
        if(order+1 > number){
            $order.textContent = 1;
        }else{
            $order.textContent = order+1;
        }
    }else{
        //비어있지 않음
        if(word[word.length-1]===newWord[0]){ // 입력한 단어가 올바른가?
            //올바름
            word = newWord; //현재 단어를 제시어에 저장
            $word.textContent = word; // 화면에 제시어 표시
            const order = Number($order.textContent);
            if(order+1 > number){
                $order.textContent =1;
            }else{
                $order.textContent = order + 1;
            }
        }else{
            //올바르지 않음
            **alert('올바르지 않은 단어입니다!');**
        };
    }
};
const onInput = (event)=>{
    newWord = event.target.value; // 입력하는 단어를 현재 단어로
};

$button.addEventListener('click',onClickButton);
$input.addEventListener('input',onInput);
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c5f1a0e3-29d2-4765-b6c0-9c5d9b44cc33/Untitled.png)

**사용자 편의도를 고려하여 입력창을 비우고 커서를 두는 코드 추가**

```jsx
const number = Number(prompt('몇 명이 참가하나요?'));
const $button = document.querySelector('button');
const $input = document.querySelector('input');
const $word = document.querySelector('#word');
const $order = document.querySelector('#order');
let word; //제시어
let newWord; // 현재 단어

const onClickButton = ()=>{
    if(!word){ //제시어가 비어 있는가?
        //비어 있음
        word = newWord; // 입력한 단어가 제시어가 됨
        $word.textContent = word; // 화면에 제시어 표시
        const order = Number($order.textContent);
        if(order+1 > number){
            $order.textContent = 1;
        }else{
            $order.textContent = order+1;
        }
        **$input.value = '';
        $input.focus();**
    }else{
        //비어있지 않음
        if(word[word.length-1]===newWord[0]){ // 입력한 단어가 올바른가?
            //올바름
            word = newWord; //현재 단어를 제시어에 저장
            $word.textContent = word; // 화면에 제시어 표시
            const order = Number($order.textContent);
            if(order+1 > number){
                $order.textContent =1;
            }else{
                $order.textContent = order + 1;
            }
            **$input.value = '';
            $input.focus();**
        }else{
            //올바르지 않음
            alert('올바르지 않은 단어입니다!');
            **$input.value = '';
            $input.focus();**
        };
    }
};
const onInput = (event)=>{
    newWord = event.target.value; // 입력하는 단어를 현재 단어로
};

$button.addEventListener('click',onClickButton);
$input.addEventListener('input',onInput);
```

<aside>
✅ 형식

입력태그.value : 입력창의 값을 가져옴

입력태그.value = 값 : 입력창에 값을 넣음

입력태그.focus() = 입력창을 하이라이트

</aside>

### 9. 순서도 최적화하기

![IMG_1383.heic](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7f4b97ae-ca93-4104-86c0-14ff17b7083a/IMG_1383.heic)

중복되는 부분 찾아 최적화하기

순서도 → ‘입력창을 비우고 커서를 둔다'가 중복됨

```jsx
$input.value = "";
$input.focus();
```

코드 → ‘입력한 단어가 제시어가 됨', ‘다음 사람에게 순서를 넘긴다’

```jsx
word = newWord;
$word.textContent = word;
const order = Number($order.textContent);
if (order + 1 > number) {
  $order.textContent = 1;
} else {
  $order.textContent = order + 1;
}
```

**둘을 합쳐 “제시어가 비어있거나 입력한 단어가 올바른가?”로 절차 줄이기**

판단하는 절차가 연이어 나오고 ‘예'나 ‘아니오'가 공통된 절차로 이어질 때는 절차를 하나로 만들 수 있음

**합칠 수 있는 절차인지 판단하기**

| 판단1                   | 판단2                | 결과                        | 비고 |
| ----------------------- | -------------------- | --------------------------- | ---- | --- | -------- |
| 제시어가 비어 있다      | 단어가 올바르다      | 입력한 단어가 제시어가 된다 | OR ( |     | , 또는 ) |
| 제시어가 비어 있다      | 단어가 올바르지 않다 | 입력한 단어가 제시어가 된다 | OR ( |     | , 또는 ) |
| 제시어가 비어 있지 않다 | 단어가 올바르다      | 입력한 단어가 제시어가 된다 | OR ( |     | , 또는 ) |
| 제시어가 비어 있지 않다 | 단어가 올바르지 않다 | 틀렸다고 표시한다           | OR ( |     | , 또는 ) |

**onClick 함수에서 if문의 순서를 조정하여 최적화한 순서도를 코드에 반영하기**

```jsx
// 최적화한 순서도를 코드에 반영하기
const number = Number(prompt('몇 명이 참가하나요?'));
const $button = document.querySelector('button');
const $input = document.querySelector('input');
const $word = document.querySelector('#word');
const $order = document.querySelector('#order');
let word; //제시어
let newWord; // 현재 단어

const onClickButton = ()=>{
    **if(!word ||word[word.length-1]===newWord[0]){ //제시어가 비어 있거나 입력한 단어가 올바른가?**
        //비어 있음
        word = newWord; // 입력한 단어가 제시어가 됨
        $word.textContent = word; // 화면에 제시어 표시
        const order = Number($order.textContent);
        if(order+1 > number){
            $order.textContent = 1;
        }else{
            $order.textContent = order+1;
        }
    }else{
        //올바르지 않음
        alert('올바르지 않은 단어입니다!');
    }
    $input.value = '';
    $input.focus();
};
const onInput = (event)=>{
    newWord = event.target.value; // 입력하는 단어를 현재 단어로
};

$button.addEventListener('click',onClickButton);
$input.addEventListener('input',onInput);
```

절차1이 절차2보다 우선 순위가 높음(절차1을 2보다 먼저 판단함)
