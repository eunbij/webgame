# 이벤트 루프 이해하기\_카드 짝 맞추기 게임

## 1. 순서도 그리기

concentration.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>카드 짝 맞추기</title>
    <style>
      .card {
        display: inline-block;
        margin-right: 20px;
        margin-bottom: 20px;
        width: 70px;
        height: 100px;
        perspective: 140px;
      }
      .card-inner {
        position: relative;
        width: 100%;
        height: 100%;
        text-align: center;
        transition: transform 0.8s;
        transform-style: preserve-3d;
      }
      .card.flipped .card-inner {
        transform: rotate(180deg);
      }
      .card-front {
        background: navy;
      }
      .card-front .card-box {
        position: absolute;
        width: 100%;
        height: 100%;
        border: 1px solid #000;
        backface-visibility: hidden;
      }
      .card-back {
        transform: rotate(180deg);
      }
    </style>
  </head>
  <body>
    <div id="wrapper"></div>
    <script>
      const $wrapper = document.querySelector("#wrapper");
    </script>
  </body>
</html>
```

- 자바스크립트가 아닌 CSS (flipped 클래스)를 이용하여 카드를 뒤집는 효과 구현
- 자바스크립트로 구현 가능하지만 CSS로 구현 가능한 영역은 CSS를 이용하는 것이 편리

## 2. 카드 생성하기

12장의 카드 (2장씩 \* 6가지)

6가지 색을 두 번 복사해서 이어 붙이면 12장 ⇒ 피셔-예이츠 셔플 알고리즘 이용

```jsx
const $wrapper = document.querySelector("#wrapper");

const total = 12;
const colors = [1, 2, 3, 4, 5, 6];
let colorCopy = colors.concat(colors);
let shuffled = [];

function shuffle() {
  // 피셔-예이츠 셔플
  for (let i = 0; colorCopy.length > 0; i += 1) {
    const randomIndex = Math.floor(Math.random() * colorCopy.length);
    shuffled = shuffled.concat(colorCopy.splice(randomIndex, 1));
  }
}
function createCard(i) {
  const card = document.createElement("div");
  card.className = "card"; // .card 태그 생성
  const cardInner = document.createElement("div");
  cardInner.className = "card-inner"; // .card-inner 태그 생성
  const cardFront = document.createElement("div");
  cardFront.className = "card-front"; // .card-front 태그 생성
  const cardBack = document.createElement("div");
  cardBack.className = "card-back"; // .card-back 태그 생성
  cardBack.style.backgroundColor = shuffled[i];
  cardInner.appendChild(cardFront);
  cardInner.appendChild(cardBack);
  card.appendChild(cardInner);
  return card;
}
function startGame() {
  shuffle();
  for (let i = 0; i < totla; i += 1) {
    const card = createCard(i);
    $wrapper.appendChild(card);
  }
}
startGame();
```

- 코드 설명
  total : 전체 카드 개수 저장
  colors : 6가지 색 저장
  colorCopy : colors에서 6가지 색을 두 번 복사
  shuffle 함수 : 카드를 섞어 shuffled 배열로 만듦
  concat 메서드로 colors 변수를 수정하지 않고 “배열을 복사함”
  startGame 함수로 게임 시작

게임 시작 → 바로 카드를 섞음(shuffle) → 12개의 카드를 생성(createCard) → createCard의 인덱스i를 인수로 전달해서 shuffled로부터 카드 색상을 가져오는 데 이용 → 색상은 태그의 backgroundColor CSS 속성에 넣음 → 만든 카드는 $wrapper 태그 안으로 들어감

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/637c9cca-9292-4233-aeeb-8d2176fbcf6f/Untitled.png)

## 3. 카드 잠깐 보여 주고 뒤집기

flipped 클래스를 추가하면 앞면이 보이고, flipped 클래스를 빼면 뒷면이 보임

시각적인 효과를 위해 카드별로 시간차를 두고 뒤집음 ⇒ startGame 함수 안에 작성

```jsx
document.querySelectorAll(".card").forEach((card, index) => {
  //초반 카드 공개
  setTimeout(() => {
    card.classList.add("flipped");
  }, 1000 + 100 * index);
});
setTimeout(() => {
  //카드 감추기
  document.querySelectorAll(".card").forEach((card) => {
    card.classList.remove("flipped");
  });
}, 5000);
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6d983915-bd97-4c63-8372-312d2bd4132e/Untitled.png)

## 4. 카드 클릭해서 짝 맞추기

2장의 카드를 뒤집어 색이 일치하면 그대로 / 색이 일치하지 않으면 다시 뒤집기

모든 카드가 앞면이 되면 짝 맞추기 성공 안내 메시지 표시

⇒ 생성한 카드 태그에 클릭 이벤트 리스너 추가

```jsx
**function onClickCard(){}** // 카드 태그 이벤트 리스너 추가
function startGame(){
    shuffle();
    for(let i=0; i<total; i+=1){
        const card = createCard(i);
        // 카드 태그 이벤트 리스너 추가
        **card.addEventListener('click',onClickCard);**
        $wrapper.appendChild(card);
    }
		...
}
```

카드를 클릭하면 해당 카드를 뒤집고 클릭한 카드 배열에 추가

클릭한 카드 배열 변수 선언

클릭한 카드 배열에 2장이 들어 있는지 확인

```jsx
let clicked = []; // 클릭한 카드 배열 변수 선언

function onClickCard(){ // 카드 태그 이벤트 리스너 추가
    **this.classList.toggle('flipped');
    clicked.push(this);
    if(clicked.length!==2){
        return;
    }**
}
```

- 코드 설명
  this 또는 event.target으로 클릭한 카드에 접근 가능
  [event.target](http://event.target) 사용 시에는 onClickCard 함수의 매개변수에 event도 넣어야 함
  onClickCard는 화살표 함수가 아니라 this가 클릭한 카드가 됨(addEventListner가 this를 그렇게 바꿔주는 역할 수행)
  if문 중첩 줄이기 적용
  클릭한 카드가 2장이 아닐 경우 return으로 함수 종료됨

return 아랫부분은 클릭한 카드의 개수가 두 개일 때만 실행됨

두 카드의 앞면이 똑같은 지 비교 ⇒ 태그.style.backgroundColor 비교

```jsx
function onClickCard(){ // 카드 태그 이벤트 리스너 추가
    this.classList.toggle('flipped');
    clicked.push(this);
    if(clicked.length!==2){
        return;
    }
    **const firstBackColor = clicked[0].querySelector('.card-back').style.backgroundColor;
    const secondBackColor = clicked[1].querySelector('.card-back').style.backgroundColor;
    if(firstBackColor === secondBackColor){ // 두 카드가 같은 카드면
        return;
    }
    // 두 카드가 다르면**
}
```

두 카드가 같으면 클릭 배열에서 완료 배열로 옮김

두 카드가 다르면 클릭 배열에서 빼고 뒷면으로 뒤집는 코드를 추가

⇒ 완료 배열 변수(completed) 선언

```jsx
**let completed = []; // 완료 배열 변수 선언**
...
function onClickCard(){ // 카드 태그 이벤트 리스너 추가
    this.classList.toggle('flipped');
    clicked.push(this);
    if(clicked.length!==2){
        return;
    }
    const firstBackColor = clicked[0].querySelector('.card-back').style.backgroundColor;
    const secondBackColor = clicked[1].querySelector('.card-back').style.backgroundColor;
    if(firstBackColor === secondBackColor){ // 두 카드가 같은 카드면
        **completed.push(clicked[0]);
        completed.push(clicked[1]);**
        clicked = [];
        return;
    }
    // 두 카드가 다르면
    **clicked[0].classList.remove('flipped');
    clicked[1].classList.remove('flipped');
    clicked = [];**
}
```

위 코드로는 두 카드가 다른 경우 두 번째 카드가 아예 뒤집히지 않음

→ 두 번째 카드도 앞면을 보여주고 다시 뒷면으로 되돌아가야 함 [앞면을 보이기 전에 flipped 클래스가 제거된 것이 원인]

→ 앞면을 보일 수 있는 충분한 시간 확보 필요

```jsx
// 두 카드가 다르면
**setTimeout(() => {**
    clicked[0].classList.remove('flipped');
    clicked[1].classList.remove('flipped');
    clicked = [];
**}, 500);**
```

🔽모든 카드가 짝이 맞아 앞면이 될 때 ‘축하합니다!’라는 alert 창 구현

```jsx
if(firstBackColor === secondBackColor){ // 두 카드가 같은 카드면
    completed.push(clicked[0]);
    completed.push(clicked[1]);
    clicked = [];
    **if(completed.length !== total){**
        r**eturn;
    }
    alert(`축하합니다!`);**
    return;
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6641c767-85fe-4b71-8a56-408e781be033/Untitled.png)

모든 카드가 앞면을 보이기 전에 alert이 먼저 실행되므로 시간차 추가

```jsx
if(firstBackColor === secondBackColor){ // 두 카드가 같은 카드면
    completed.push(clicked[0]);
    completed.push(clicked[1]);
    clicked = [];
    if(completed.length !== total){
        return;
    }
    **setTimeout(() => {
        alert(`축하합니다!`);
        resetGame();
    }, 1000);**
    return;
}

**function resetGame(){
    $wrapper.innerHTML = '';
    colorCopy = colors.concat(colors);
    shuffled = [];
    completed = [];
    startGame();
};**
```

짝 맞추기에 성공한 순간 clicked는 빈 배열이므로 초기화 함수에서 따로 초기화 하지 않아도 됨(헷갈릴 경우 모두 다 초기화해도 무관)

### 5. 효과 발생 중 카드 클릭 막기

발생하는 버그

1. 카드를 보여줬다 다시 뒤집는 동안 카드를 클릭할 수 없어야 하는데 카드를 클릭하면 카드가 뒤집힘
2. 짝이 맞춰진 카드를 클릭해도 다시 카드가 뒤집힘
3. 한 카드를 두 번 연이어 클릭하면 더 이상 해당 카드가 클릭되지 않음
4. 서로 다른 네 가지 색의 카드를 연달아 클릭하면 마지막 두 카드가 앞면을 보인 채 남아 있음

문제 해결을 위해 카드를 클릭할 수 있는 상황과 클릭할 수 없는 상황을 구분해야 함

⇒ 변수 선언 clickable

```jsx
let clickable = false; // 클릭 가능 여부 확인 변수 선언

function onClickCard(){ // 카드 태그 이벤트 리스너 추가
    if(!clickable || completed.includes(this) || clicked[0] === this){
        return;
    }

function startGame(){
    ...
    setTimeout(() => {
        //카드 감추기
        document.querySelectorAll('.card').forEach((card) => {
            card.classList.remove('flipped');
        });
        **clickable = true;**
    }, 5000);
}

function resetGame(){
    $wrapper.innerHTML = '';
    colorCopy = colors.concat(colors);
    shuffled = [];
    completed = [];
    **clickable = false;**
    startGame();
};
```

### 5.1 호출 스택과 이벤트 루프

클릭 이벤트는 비동기 이벤트임. 그 안에 setTimeout 같은 비동기 함수가 또 들어 있어 코드 실행 순서가 헷갈릴 수 있음

코드의 실행 순서 파악을 위해 호출 스택call stack과 이벤트 루프 event loop 를 알아야 함

호출 스택 : 동기 코드 담당

이벤트 루프 : 비동기 코드 담당

비동기 코드 실행 : 백그라운드background, 태스크 큐task queue

<aside>
💡 **백그라운드**

타이머를 처리하고 이벤트 리스너를 저장하는 공간

setTimeout 함수가 실행되면 백그라운드에서 시간을 재고 시간이 되면 setTimeout의 콜백 함수를 태스크 큐로 보냄

addEventListener로 추가한 이벤트를 저장했다가 이벤트가 발생하면 콜백 함수를 태스큐를 보냄

백그라운드에서는 실행될 콜백 함수들이 태스크 큐로 들어감

</aside>

<aside>
💡 **태스크 큐**

실행되어야 할 콜백 함수가 줄 서서 대기하는 공간

먼저 들어온 함수부터 실행됨

함수를 직접 실행하지 않고 호출 스택에서만 실행됨

</aside>

<aside>
💡 **이벤트 루프**

태스크 큐에서 호출 스택으로 함수를 이동시키는 존재

호출 스택이 비어 있으면 태스크 큐에서 함수를 하나씩 거내(들어온 순서로) 호출 스택으로 옮김

</aside>

<aside>
💡 **호출 스택**

호출 스택에 들어온 함수가 호출(실행)됨

실행이 완료된 함수는 호출 스택에서 빠져나가게 되고 이벤트 루프가 다시 태스크 큐에 있는 다음 함수를 호출 스택으로 옮김

</aside>

- 변수나 함수의 선언은 호출 스택과 이벤트 루프에 영향을 주지 않음[선언은 스코프의 영역]
- 호출 스택과 이벤트 루프는 함수 호출과 밀접한 관련
- 예시
  ```jsx
  function a() {
    b();
  }
  function b() {
    console.trace();
  }
  a();
  ```
  a 함수가 실행되고 그 안에서 b 함수가 실행됨.
  b 함수 안에서 console.trace 메서드가 실행됨 (console.trace : 함수의 호출 스택을 보여줌)
  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/048129c6-ab97-4710-9e54-a5b8f2036c7b/Untitled.png)
  (이미지) 호출된 함수의 역순
  a 보다 anonymous 함수가 먼저 실행됨
  ⇒ 소스 코드가 처음 실해될 때 호출되는 anonymous 함수
- anonymous 함수 : (크롬 브라우저 기준) 처음 파일 실행 시 실행되는 익명 함수

### 5.2 카드 짝 맞추기 게임의 호출 스택과 이벤트 루프

**호출 스택**

anonymous

startGame

shuffle(호출되면 들어왔다가 종료되면 빠져나감)

→ createCard 12번 반복 후 나감

forEach

—

setTimeout

타이머 호출 ...

startGame 함수가 끝나면 호출 스택이 비워짐 (비어있으면 태스크 큐에서 가져옴)

—

1초 타이머

**백그라운드**

addEventListner (등록하는 순간 저장됨. 12개)

—

1초 타이머

...

5초 타이머

백그라운드에는 타이머 12개와 5초 타이머가 존재 → 콜백 함수를 태스크 큐로 이동하면서 타이머는 백그라운드에서 제거됨

—

**태스크 큐**

1초 타이머(→ 호출 스택으로 올라감)

사용자가 이벤트를 발생할 경우

: 태스크 큐에 이벤트(클릭) 순서로 대기 → 호출 스택이 비어 있으니 콜백 함수를 불러 하나씩 실행 → 함수 종료 시 호출 스택에서 빠져 나감

각각의 이벤트에 따라 태스크 큐에서 호출 스택으로 이동하는 이벤트 루프에 대한 설명이 반복됨 ⇒ 코드와 함께 보면 이해하기 용이할 것으로 보임

발견한 문제점

clicked 배열에 최대 2개 카드가 존재한다 예상했으나 배열에 그 이상으로 들어있는 상황이 발생

클릭 콜백 함수가 끝나고 호출 스택이 비어 있으니 타이머 콜백 함수가 연이어 실행되는데 clicked 배열을 []로 초기화하여 다른 카드가 뒤집히지 않은 채 남아 있게 됨

clicked를 []로 초기화하기 전에 카드 클릭 이벤트 콜백 함수가 실행되는 것이 문제 → 코드 실행 순서가 정해져 있으므로 실행을 막는 것이 아닌 실행되더라도 아무 일이 발생하지 않도록 수정

카드가 2장이 될 때 clickable을 false로 만들어서 세 번째 카드부터는 클릭해도 아무 것도 하지 않고 끝나게 함

```jsx
function onClickCard(){ // 카드 태그 이벤트 리스너 추가
    ...
    // 두 카드가 다르면
    **clickable = false;**
    setTimeout(() => {
        clicked[0].classList.remove('flipped');
        clicked[1].classList.remove('flipped');
        clicked = [];
        **clickable = true;**
    }, 500);
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5b2e943a-ccd6-4300-8c2b-511693acd769/Untitled.png)

> 함수의 실행 순서를 알아야 자신이 원하는 대로 작동하는 프로그램을 만들 수 있다.
> 호출 스택과 이벤트 루프 원리를 모든 게임에 적용해보는 것으로 작동 원리 파악 훈련하는 것을 권장
