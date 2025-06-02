# 🗓️ 2025년 6월 2일 - Operation Game

## 🧠 문제 설명

사칙연산 게임을 통해 **자바스크립트의 조건문, 반복문, 변수 활용, 사용자 입력 처리** 등을 종합적으로 연습하는 실습입니다.

게임은 난이도 설정 → 무작위 문제 출제 → 사용자의 정답 입력 → 판별 과정을 거칩니다.

---

## 🧾 버전별 요구사항

### ✅ v1.0 시나리오

* 시스템은 1\~10 사이 무작위 정수 2개로 덧셈 문제를 출제한다.
* 사용자는 정답을 입력할 수 있다.
* 정답 여부에 따라 피드백을 주고, 계속해서 문제를 출제한다.
* 사용자가 **0을 입력하면 게임을 종료**한다.

### ✅ v1.1 시나리오

* 게임 종료 시, **정답 횟수와 오답 횟수**를 출력한다.

### ✅ v1.2 시나리오

* 덧셈 외에 **뺄셈, 곱셈 문제도 무작위로 출제**한다.

### ✅ v1.3 시나리오

* 사용자는 게임 시작 시 **난이도 설정**을 할 수 있다.

  * `상`: 세 자리 수,
  * `중`: 두 자리 수,
  * `하`: 한 자리 수 문제 출제

---

## 🔍 내 풀이

```javascript

let randomNum1;
let randomNum2;
let temp;
let maxNum;
let minNum;
let answer;
let correctAnswer;
let questionNum = 1;
let correctNum = 0;
let wrongNum = 0;
let operation;
let level = +prompt(`~~~~~ 난이도 설정 ~~~~~\n[ 1. 상 (3자리수) | 2. 중 (2자리수) | 3. 하 (1자리수) ]`);


// Math.floor(Math.random()*(maxNum - minNum) + minNum);
while (true) {
    if (level === 1) {
        maxNum = 1000;
        minNum = 100;
    } else if (level === 2) {
        maxNum = 100;
        minNum = 10;
    } else if (level === 3) {
        maxNum = 10;
        minNum = 1;
    }

    randomNum1 = Math.floor(Math.random()*(maxNum - minNum) + minNum);
    randomNum2 = Math.floor(Math.random()*(maxNum - minNum) + minNum);
    if (randomNum2 >= randomNum1) {
        temp = randomNum1;
        randomNum1 = randomNum2;
        randomNum2 = temp;
    }

    operation = Math.floor(Math.random()*(4 - 1) + 1);

    if (operation === 1) {
        correctAnswer = randomNum1 + randomNum2;
        answer = prompt(`Q${questionNum}. ${randomNum1} + ${randomNum2} = ??`);
    } else if (operation === 2) {
        correctAnswer = randomNum1 - randomNum2;
        answer = prompt(`Q${questionNum}. ${randomNum1} - ${randomNum2} = ??`);
    } else if (operation === 3) {
        correctAnswer = randomNum1 * randomNum2;
        answer = prompt(`Q${questionNum}. ${randomNum1} * ${randomNum2} = ??`);
    }
    let userInput = answer;

    // 사용자가 취소하거나 입력을 안 했을 경우
    if (userInput === null || userInput.trim() === '') {
        alert('값을 입력해주세요!');
        continue;
    }

    // 정확히 "0"을 입력했을 경우만 종료
    if (userInput.trim() === '0') {
        alert('게임을 종료합니다.');
        alert(`# 정답 횟수: ${correctNum}회, 틀린 횟수: ${wrongNum}회`);
        break;
    }

    if (+answer === correctAnswer) {
        alert('정답입니다!');
        correctNum++;
        questionNum++;
    } else if (+answer !== correctAnswer){
       alert('틀렸습니다!');
       wrongNum++;
       questionNum++;
    }
}
```

---

## 🙋‍♂️ 느낀 점

### ✅ 잘한 점

* 무조건 큰 수를 왼쪽에 배치해서 음수가 나오지 않도록 한 점이 좋았음.
* `정확히 0`을 입력해야 종료되도록 조건을 설정한 부분도 꼼꼼했음.

### ❗ 개선할 점

* **난이도 설정을 `while` 안에 넣은 것**은 비효율적임. 계속 maxNum, minNum을 재설정하게 됨.
  → `while` 바깥에서 딱 한 번 설정되도록 수정하는 것이 좋음.
* **사칙연산 처리**를 문제 출제와 함께 `if`문 하나로 처리했는데,
  → **선생님 코드처럼 "연산자 결정 → 문제 출제"를 분리**하면 더 깔끔하고 가독성이 좋았음.
* **난이도 입력에 대한 예외 처리**가 없음. 문자열이나 잘못된 숫자가 들어왔을 때 무시됨.

---

## 👨‍🏫 선생님 코드

```js
// 난이도 설정을 위한 난수설정 최소 최대값
let maxLimit, minLimit;

// 난이도 설정
while (true) {

    let level = prompt(`
      ~~~~~~ 난이도 설정 ~~~~~~
      [ 1. 상 (3자리수) | 2. 중 (2자리수) | 3. 하 (1자리수) ]
    `);

    if (level === '1') {
        maxLimit = 900;
        minLimit = 100;
    } else if (level === '2') {
        maxLimit = 90;
        minLimit = 10;
    } else if (level === '3') {
        maxLimit = 9;
        minLimit = 1;
    } else {
        alert('다시 난이도를 입력하세요!');
        continue;
    }

    break;
}

// 문제 번호 생성
let questionNumber = 1;

// 정답, 오답 횟수를 저장
let correctCount = 0;
let wrongCount = 0;

while (true) {
    // 무작위 정수 2개 생성
    let firstNum = Math.floor(Math.random() * maxLimit) + minLimit;
    let secondNum = Math.floor(Math.random() * maxLimit) + minLimit;

    // 실제 답 생성
    let realAnswer;

    // 사칙연산자를 랜덤으로 결정할 숫자 (0, 1, 2)
    let markNumber = Math.floor(Math.random() * 3);

    // 사칙 연산자
    let mark;
    if (markNumber === 0) {
        mark = '+';
        realAnswer = firstNum + secondNum;
    } else if (markNumber === 1) {
        // 같은 수가 나오면 랜덤 숫자 재생성
        if (firstNum === secondNum) {
            continue;
        }
        mark = '-';
        realAnswer = firstNum - secondNum;
    } else {
        mark = 'x';
        realAnswer = firstNum * secondNum;
    }

    // 무조건 firstNumber가 secondNumber보다 크도록 조작
    if (firstNum < secondNum) {
        let temp = firstNum;
        firstNum = secondNum;
        secondNum = temp;
    }

    // 문제 출제
    let userAnswer = +prompt(
        `Q${questionNumber}. ${firstNum} ${mark} ${secondNum} = ??`
    );

    // 문제 출제 이후 문제번호 증가
    questionNumber++;

    // 0인지 확인
    if (userAnswer === 0) {
        alert('게임을 종료합니다.');
        break;
    }

    // 정답 판별
    if (userAnswer === realAnswer) {
        alert('정답입니다!');
        correctCount++;
    } else {
        alert('틀렸습니다!');
        wrongCount++;
    }
} // end while

alert(`# 정답 횟수: ${correctCount}회, 틀린 횟수: ${wrongCount}회`);
```

---

## 💡 리팩토링 포인트 요약

| 항목     | 기존 코드                  | 리팩토링 방향                               |
| ------ | ---------------------- | ------------------------------------- |
| 난이도 설정 | while 내부에 존재           | while 외부에서 한 번만 설정                    |
| 연산자 처리 | 문제 출제와 함께 처리           | 연산자 결정 후 문제 출제                        |
| 변수명    | `temp`, `randomNum1` 등 | `num1`, `num2`, `correctCount` 등 명확하게 |
| 코드 흐름  | 긴 if문 내에 모든 처리         | 연산 로직과 출력 로직을 분리하여 가독성 향상             |

