# ⏱️ 스탑워치 실습 개선 기록

## 📅 2025년 6월 12일

## 🎯 목표
- 내 스탑워치 코드가 잘 작동하지 않았던 이유를 분석하고,
- 선생님 코드와 비교하여 어떤 점을 개선하면 좋을지 정리함

---

## 🗂️ 비교 대상

| 항목 | 내 코드 (`practice3.html`) | 선생님 코드 (`practice3-refactored.html`) |
|------|-----------------------------|--------------------------------------------|
| 시간 표시 | `초` 단위 실수로만 표시됨 | `분:초:밀리초` 형식으로 포맷 |
| 재개 시 문제 | 재개 시 시간이 0부터 다시 시작됨 | 멈춘 시점부터 정확히 이어짐 |
| 인터벌 제어 | `startTime = new Date()` 기준 계산 | `elapsedTime`을 누적해서 계산 |
| 상태관리 | 상태 분리 없음 | `isRunning`으로 상태 명확하게 관리 |
| 코드 구조 | 직접 DOM 조작 및 흐름 제어 | 함수 분리로 가독성과 재사용성 높음 |

---

## 🧩 주요 문제 분석 (내 코드 기준)

### ❌ 문제 1: 시간 포맷 미정리
```js
$display.textContent = elapsedTime / 1000;
```

* `3.4234211...`처럼 실수 그대로 출력됨 → 사용자 입장에서 보기 불편

### ✅ 해결 방법

* `formatElapsedTime()` 함수를 만들어 `분:초:밀리초` 형식으로 보여줌

```js
const minutes = String(Math.floor(elapsedTime / 60000)).padStart(2, "0");
const seconds = String(Math.floor((elapsedTime % 60000) / 1000)).padStart(2, "0");
const millisecond = String(Math.floor((elapsedTime % 1000) / 10)).padStart(2, '0');
$display.textContent = `${minutes}:${seconds}:${millisecond}`;
```

---

### ❌ 문제 2: 재개 시 시간이 초기화됨

```js
const startTime = new Date(); // 재개할 때도 새로 시작함
```

* 재개 시 새 `Date` 객체로 다시 시작하기 때문에 기존 `elapsedTime`은 반영되지 않음

### ✅ 해결 방법

* 경과 시간 누적 변수 사용 (`elapsedTime += 10`)
* 재개 시 `setInterval()`은 `elapsedTime`을 기준으로 계속 증가시킴

---

## 🧠 개선이 필요한 코드 흐름 요약

| 기능      | 내 코드 동작           | 개선 방향                                               |
| ------- | ----------------- | --------------------------------------------------- |
| 시간 포맷   | `초`만 표시           | `분:초:밀리초`로 포맷팅                                      |
| 재개 시 시간 | 0부터 다시 시작         | 누적 시간(`elapsedTime`) 기반 증가                          |
| 인터벌 관리  | `startTime` 새로 생성 | `elapsedTime`에 계속 누적                                |
| 상태 확인   | 없음                | `isRunning` 상태 변수 활용                                |
| 로그 관리   | 로그 기록만 존재         | 로그 삭제 및 복원 기능도 고려 가능                                |
| 코드 구조   | 즉흥적 DOM 조작        | 함수화해서 역할 분리 (ex: `changeState`, `logStopwatchTime`) |

---

## 🧠 오늘의 교훈

* 내 코드도 기본적인 기능은 잘 작동했으나, **시간 계산 방식과 포맷 처리**, 그리고 **재개 기능의 처리 로직**에서 핵심적인 로직 차이가 있었음을 확인했다.
* 앞으로는 복잡한 상태를 다룰 때 `상태 변수`를 추가하여 흐름을 제어하고, UI에 보여줄 데이터를 포맷팅하는 습관을 기르자.

---

## 📁 참고 파일

* 📝 내 코드: `4-1-timer\practice3.html`
* ✅ 선생님 코드: `4-1-timer\practice3-refactored.html`

