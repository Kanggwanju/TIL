# 🗓️ 2025년 6월 2일 - Up Down Game

## 🔍 문제 설명

업다운 게임을 통해 JavaScript의 `조건문`, `반복문`, `변수 활용`, `사용자 입력 처리` 등을 종합적으로 연습하는 실습입니다.

### 📌 v1.0 시나리오
- 1 ~ 100 사이의 랜덤한 숫자가 정답으로 주어진다.
- 사용자는 해당 범위 내에서 숫자를 입력한다.
- 입력한 숫자가 정답보다 작으면 "UP", 크면 "DOWN" 메시지를 출력한다.
- 정답을 맞출 때까지 반복 입력받는다.
- 정답을 맞추면 성공 메시지를 출력하고 게임 종료.

### 📌 v1.1 시나리오
- 사용자는 입력 기회가 **5번**으로 제한된다.
- 5번 안에 정답을 맞추지 못하면 게임 종료 메시지를 출력한다.

### 📌 v1.2 시나리오
- 게임 시작 전에 사용자는 난이도를 선택한다.
- 난이도는 아래 3가지로 나뉜다:

| 난이도 | 범위         | 기회 수 |
|--------|--------------|---------|
| 상     | 1 ~ 1000     | 20      |
| 중     | 1 ~ 100      | 10      |
| 하     | 1 ~ 10       | 5       |

---

## 🙋‍♀️ 내 풀이

```js
// 선택한 숫자
let selectedNumber = 0;

// 1 ~ 100, 랜덤 번호
let minNumber = 1;
let maxNumber;
let randomNumber;

// 제한 횟수
let maxAttempts;
let attempt = 1;

// 선택한 레벨
let selectedLevel = '';

while (true) {
  selectedLevel = prompt(`~~~~~ 난이도 설정 ~~~~~\n[ 1. 상 (3자리수) | 2. 중 (2자리수) | 3. 하 (1자리수)] `);
  if (selectedLevel === '상' || +selectedLevel === 1) {
    alert('난이도가 1. 상으로 선택되었습니다.');
    maxNumber = 1000;
    maxAttempts = 20;
    break;
  } else if (selectedLevel === '중' || +selectedLevel === 2) {
    alert('난이도가 2. 중으로 선택되었습니다.');
    maxNumber = 100;
    maxAttempts = 10;
    break;
  } else if (selectedLevel === '하' || +selectedLevel === 3) {
    alert('난이도가 3. 하으로 선택되었습니다.');
    maxNumber = 10;
    maxAttempts = 5;
    break;
  } else {
    alert('정해진 난이도 중에 선택해 주세요');
  }
}

randomNumber = Math.floor(Math.random() * (maxNumber - minNumber + 1)) + minNumber;
alert(`입력 기회가 ${maxAttempts}가(이) 되었습니다.`);

while (true) {
  if (attempt <= maxAttempts) {
    selectedNumber = +prompt(`숫자를 입력하세요! [ ${minNumber} ~ ${maxNumber} ] ${randomNumber}`);
    if (selectedNumber >= minNumber && selectedNumber <= maxNumber) {
      if (selectedNumber < randomNumber) {
        minNumber = selectedNumber + 1;
        alert('UP!!');
      } else if (selectedNumber > randomNumber) {
        maxNumber = selectedNumber - 1;
        alert('DOWN!!');
      } else if (selectedNumber === randomNumber) {
        alert('정답입니다!');
        break;
      }
    } else {
      alert('범위에 맞는 값을 입력해주세요!');
      if (attempt === 1) {
        attempt = 0;
      } else {
        attempt--;
      }
    }
  } else {
    alert(`기회가 모두 소진되었습니다. 정답은 ${selectedNumber}이었지롱~ ㅋㅋ`);
    break;
  }
  alert(`${maxAttempts - attempt}번의 기회가 남았습니다.`);
  attempt++;
}
```

---


## ✨ 실습하면서 느낀 점

- 난이도 선택에서 나는 `if-else` 문을 사용했지만, 선생님은 `switch` 문을 사용해서 **입력이 올바른 경우만 처리하고, 아닌 경우는 `continue`로 다시 입력을 유도**해 가독성이 좋았음.
- **기회 수 차감 처리**에서 나는 `attempt`를 조작했지만, 선생님처럼 `continue`를 활용했으면 더 자연스럽고 깔끔했을 것 같았음.
- **입력값 검증 + 정답 판별 + 기회 소진 로직을 모두 하나의 if문 안에 중첩**해서 작성하다보니 **3중첩 if문**이 되어 가독성이 떨어졌고, 기능 분리가 필요함을 느꼈음.

---

## 👨‍🏫 선생님 코드

```js
// 난이도 입력기회 상수
const DIFFICULT = 4;
const NORMAL = 6;
const EASY = 10;

const MIN = 1, MAX = 100;

// 최소 최대값 변수
let minValue = MIN, maxValue = MAX;

// 1 ~ 100 사이의 무작위 난수 생성
let secret = Math.floor(Math.random() * MAX) + MIN;

// 입력 남은 기회 총 카운트 저장
let countDown;

while (true) {
  // 난이도 선택 입력창
  let level = prompt(`
  난이도를 선택하세요!
  [1. 상 (${DIFFICULT}번의 기회) | 2. 중 (${NORMAL}번의 기회) | 3. 하 (${EASY}번의 기회)]
  `);

  switch (level) {
    case '상':
    case '1':
      countDown = DIFFICULT;
      break;
    case '중':
    case '2':
      countDown = NORMAL;
      break;
    case '하':
    case '3':
      countDown = EASY;
      break;
    default:
      alert('난이도를 다시 입력해주세요!');
      continue;
  }
  break; // default가 아닐 경우 작동
}

while (true) {
  // 사용자가 입력한 정답을 저장
  let rangeText = `${minValue} ~ ${maxValue}`;
  if (minValue === maxValue) {
    rangeText = minValue;
  }
  let userAnswer = +prompt(`# 숫자를 입력하세요! [ ${rangeText} ]`);

  // 입력값 검증
  if (userAnswer < minValue || userAnswer > maxValue) {
    alert('범위 안의 값을 입력하세요.');
    continue;
  }

  // 정답을 판별하는 조건 로직
  if (secret === userAnswer) {
    alert('정답입니다!');
    break;
  } else if (secret > userAnswer) {
    alert('UP!!');
    minValue = userAnswer + 1;
  } else {
    alert('DOWN!!');
    maxValue = userAnswer - 1;
  }

  countDown--;

  // 입력 횟수 소진 게임 종료
  if (countDown === 0) {
    alert('기회가 모두 소진되었습니다. ㅋㅋㅋㅋ Bye~');
    break;
  } else {
    if (countDown === 1) {
      alert('마지막 기회입니다. 집중하세요!!');
    } else {
      alert(`${countDown}번의 기회가 남았습니다.`);
    }
  }
}
```

---

## ✅ 배운 점 요약

* `switch` + `continue`를 사용한 제어 흐름이 깔끔하고 가독성 좋음
* `if` 중첩을 피하고, 기능별로 분리해서 로직을 구성하면 이해하기 쉬워짐
* 입력값 검증 시 `continue`를 활용하면 복잡한 attempt 조정 없이도 자연스럽게 흐름 제어 가능


