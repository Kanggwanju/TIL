# 🗓️ 2025년 6월 10일 - 고차 함수 실습

JavaScript 배열 고차 함수 `filter`, `reduce`, `sort`를 이용해 거래 데이터(traders)를 다루는 실습을 진행했습니다.  
각 문제에서 제가 작성한 풀이, 선생님의 풀이, 그리고 배운 점을 함께 정리했습니다.

---

## 📁 주어진 배열
<details>
<summary>거래 데이터 보기</summary>

```javascript
const traders = [
  { trader: { name: '김철수', city: '대전' }, year: 2023, value: 500000 },
  { trader: { name: '박영희', city: '서울' }, year: 2022, value: 600000 },
  { trader: { name: '김철수', city: '대전' }, year: 2022, value: 1200000 },
  { trader: { name: '박영희', city: '서울' }, year: 2023, value: 650000 },
  { trader: { name: '뽀로로', city: '부산' }, year: 2023, value: 800000 },
  { trader: { name: '루피', city: '대전' }, year: 2022, value: 780000 },
  { trader: { name: '김철수', city: '대전' }, year: 2023, value: 1500000 },
  { trader: { name: '김철수', city: '대전' }, year: 2022, value: 2500000 },
  { trader: { name: '김철수', city: '대전' }, year: 2022, value: 500000 },
  { trader: { name: '루피', city: '대전' }, year: 2023, value: 120000 },
];
```
</details>

---

## 1️⃣ 2023년에 대전에서 발생한 거래의 총액

### ✍️ 내 풀이

```js
const daejeonAllValue = traders
  .filter(trade => trade.year === 2023 && trade.trader.city === '대전')
  .reduce((a, b) => a + b.value, 0);

console.log(`총액: ${daejeonAllValue}원`);
```

### ✅ 배운 점

* 조건에 맞는 데이터 필터링 후, `reduce`로 누적 합계 계산 가능.
* 체이닝 기법에 익숙해짐.

---

## 2️⃣ 가장 높은 거래액의 거래 정보 출력

### ❌ 내 풀이 (원본 배열 변형 문제 발생)

```js
const sortedTraders = traders.sort((a, b) => b.value - a.value);
console.log(`가장 높은 거래액 정보 - 이름: ${sortedTraders[0].trader.name}, 도시: ${sortedTraders[0].trader.city}, 거래액: ${sortedTraders[0].value}`);
```

* `sort`는 배열을 직접 바꾸기 때문에 이후 문제에서 원본이 훼손됨.

### ✅ 선생님 풀이

```js
const copyTraders = [...traders];
copyTraders.sort((a, b) => b.value - a.value);

const highestTransaction = copyTraders[0];
const { trader, value } = highestTransaction;
const { name, city } = trader;

console.log(`가장 높은 거래액 정보 - 이름: ${name}, 도시: ${city}, 거래액: ${value}원`);
```

### 💡 배운 점

* `...` 스프레드 문법으로 배열 복사 후 정렬 → 원본 보존 가능.
* 구조 분해 할당으로 코드 가독성 증가.

---

## 3️⃣ 각 도시별 총 거래액 구하기

### ✍️ 내 풀이

```js
const citiesValue = traders.reduce((resultObj, trade) => {
  switch (trade.trader.city) {
    case '서울':
      resultObj['서울'] = (resultObj['서울'] || 0) + trade.value;
      break;
    case '부산':
      resultObj['부산'] = (resultObj['부산'] || 0) + trade.value;
      break;
    case '대전':
      resultObj['대전'] = (resultObj['대전'] || 0) + trade.value;
      break;
  }
  return resultObj;
}, {});
console.log(citiesValue);
```

### ✅ 선생님 풀이

```js
const totalByCity = traders.reduce((cityObj, trs) => {
  const city = trs.trader.city;
  cityObj[city] = (cityObj[city] || 0) + trs.value;
  return cityObj;
}, {});
console.log(totalByCity);
```

### 💡 배운 점

* `short-circuit(단축 평가)`와 동적 키를 이용한 간결한 코드 작성 가능.
* `switch`보다 유지보수성 높은 접근 방식임을 깨달음.

---

## 4️⃣ 각 도시별 거래 건수 구하기

### ✍️ 내 풀이

```js
const cityTradeNum = traders.reduce((resultObj, trade) => {
  const city = trade.trader.city;
  if (city === '서울') {
    resultObj['서울'] = (resultObj['서울'] || 0) + 1;
  } else if (city === '부산') {
    resultObj['부산'] = (resultObj['부산'] || 0) + 1;
  } else if (city === '대전') {
    resultObj['대전'] = (resultObj['대전'] || 0) + 1;
  }
  return resultObj;
}, {});
console.log(cityTradeNum);
```

### ✅ 선생님 풀이

```js
const trsCountByCity = traders.reduce((cityObj, { trader }) => {
  const { city } = trader;
  if (cityObj[city] === undefined) {
    cityObj[city] = 1;
  } else {
    cityObj[city]++;
  }
  return cityObj;
}, {});
console.log(trsCountByCity);
```

### 💡 배운 점

* 구조 분해 할당으로 중첩 객체 접근을 간단하게 처리.
* `=== undefined` 비교를 통해 `||` 없이도 안전한 값 설정 가능.

---

## 5️⃣ 거래액 기준 오름차순 정렬

### ❌ 내 풀이

```js
const sortedList = traders.sort((a, b) => a.value - b.value);
console.log(sortedList);
```

* 정렬은 되었지만 `traders` 원본이 변형되어 다른 문제에 영향.

### ✅ 선생님 풀이

```js
const sortedTraders = [...traders];
sortedTraders.sort((a, b) => a.value - b.value);
console.log(sortedTraders);
```

### 💡 배운 점

* 원본 배열을 건드리지 않도록 항상 복사 후 정렬.

---

## 📌 총 정리

* `filter + reduce` 조합으로 원하는 조건의 합계 추출 가능.
* 배열 정렬 시 원본 보호를 위해 복사하는 습관 들이기.
* `reduce`는 누적값을 만들 때 유연한 키 사용이 가능하므로 객체 생성에도 유용.
* `구조 분해 할당`, `단축 평가`, `동적 키 접근`을 통해 코드를 더 간결하고 효율적으로 작성할 수 있다는 점을 배움.
