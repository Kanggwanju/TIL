# Java Stream을 활용한 거래 데이터 분석 실습
> Java Stream API를 활용하여 거래 데이터를 정렬, 필터링, 그룹화하는 다양한 실습을 진행함. (2025-07-10)

## 🔑 오늘의 키워드
- `equalsIgnoreCase`, `distinct`, `sorted`, `Optional`, `orElse`, `groupingBy`
- 스트림 정렬 및 최댓값/최솟값 구하기
- 거래 데이터를 연도별로 그룹화
- `Integer` → `int` 변환 시 주의점

---

## 📌 문자열 비교: equalsIgnoreCase

```java
str1.equalsIgnoreCase(str2);
````

* 문자열을 비교할 때 **대소문자를 무시하고 비교**할 수 있음.
* `"Cambridge"` 와 `"cambridge"` 도 같다고 판별됨.

---

## 📌 distinct()와 equals/hashCode

* 클래스에서 `equals()`와 `hashCode()`를 오버라이딩하지 않아도
  **Stream 연산에서는 값이 같으면 중복으로 간주**됨
* 하지만 **원칙적으로는 오버라이딩이 필요함**

---

## 💡 연습 4: 거래자의 이름 정렬

```java
List<String> nameList = transactions.stream()
    .map(trs -> trs.getTrader().getName())
    .distinct()
    // .sorted(Comparator.reverseOrder()) // 내림차순
    .sorted()
    .collect(toList());
```

* 데이터가 스트링, 인트, 더블과 같은 경우 파라미터 없이 .sorted() 사용 가능
* `.sorted()`는 기본 오름차순 정렬
* `.sorted(Comparator.reverseOrder())`는 내림차순 정렬

---

## 💡 연습 7: 최고 거래액 구하기

### 🧪 내 방식

```java
Integer highestTransactionValue = transactions.stream()
    .map(tr -> tr.getValue())
    .sorted((o1, o2) -> o2 - o1)
    .findFirst()
    .get();
```

* `Integer`로 반환되므로 계산할 때 **int 변환**이 필요함
* `.get()`은 `null`일 경우 예외 발생 가능성 있음

### ✅ 선생님 방식

```java
int max = transactions.stream()
    .mapToInt(trs -> trs.getValue())
    .max()
    .orElse(0);
```
* 여기서 max를 사용하면 결과물로 OptionalInt 타입이 나옴
* Optional : NPE(NullPointerException) 방지를 위한 자바의 노력
* `OptionalInt` → `.orElse(0)`을 통해 안전하게 기본값 처리

---

## 💡 연습 8: 최저 거래액 구하기

### ✅ 내 방식

```java
Integer lowestTransactionValue = transactions.stream()
    .map(tr -> tr.getValue())
    .sorted()
    .findFirst()
    .get();
```

* 오름차순 정렬 후 가장 앞 요소 선택
* `.get()` 대신 `.orElse()`로 안전하게 처리하는 것이 더 좋음

### ✅ 선생님 방식

#### 방법 1: 반복문

```java
Transaction minTrs = transactions.get(0);
for (Transaction trs : transactions) {
   if (minTrs.getValue() > trs.getValue()) {
       minTrs = trs;
   }
}
```

#### 방법 2: stream + min

```java
Transaction minTrs = transactions.stream()
    .min(Comparator.comparing(Transaction::getValue))
    .orElse(null);
```

* 거래액을 기준으로 가장 적은 Optional<Transaction>을 반환
* `Optional<Transaction>` → `.orElse(null)`을 통해 안전하게 기본값 처리

---

## 💡 연습 10: 최저 거래액보다 큰 거래들의 평균 구하기

### ✅ 내 방식

```java
double averageAboveLowestTransaction = transactions.stream()
   .filter(tr -> tr.getValue() > lowestTransactionValue)
   .mapToInt(tr -> tr.getValue())
   .average()
   .getAsDouble();  // 위험
```

* `.getAsDouble()`은 값이 없을 때 예외 발생 가능

### ✅ 선생님 방식

```java
int minValue = transactions.stream()
   .mapToInt(trs -> trs.getValue())
   .min()
   .orElse(0);

double average = transactions.stream()
   .filter(trs -> trs.getValue() > minValue)
   .mapToInt(trs -> trs.getValue())
   .average()
   .orElse(0.0);  // 안전
```

---

## 💡 연습 11: Cambridge 거래자들의 거래를 연도별로 그룹화

### ✅ 내 방식

```java
CambridgeTransaction cambTransaction = new CambridgeTransaction();
transactions.stream()
   .filter(tr -> tr.getTrader().getCity().equals("Cambridge"))
   .forEach(tr -> cambTransaction.makeCamb(tr));

System.out.println(cambTransaction);
```

* `CambridgeTransaction` 클래스를 만들어 수동으로 데이터를 분류
* 구조가 고정적이라 거래 데이터 구조가 바뀌면 유지보수 어려움

---

### ✅ 선생님 방식 1: Map 수동 구성

```java
Map<Integer, List<Transaction>> groupByYearMap = new HashMap<>();

List<Transaction> trs2021 = transactions.stream()
   .filter(trs -> trs.getYear() == 2021 && trs.getTrader().getCity().equalsIgnoreCase("cambridge"))
   .collect(toList());

List<Transaction> trs2022 = transactions.stream()
   .filter(trs -> trs.getYear() == 2022 && trs.getTrader().getCity().equalsIgnoreCase("cambridge"))
   .collect(toList());

groupByYearMap.put(2021, trs2021);
groupByYearMap.put(2022, trs2022);
```

---

### ✅ 선생님 방식 2: `groupingBy` 사용

```java
Map<Integer, List<Transaction>> groupByYearMap2 = transactions.stream()
   .filter(trs -> trs.getTrader().getCity().equalsIgnoreCase("cambridge"))
   .collect(Collectors.groupingBy(Transaction::getYear));

groupByYearMap2.forEach((key, value) -> {
   System.out.println("year: " + key);
   value.forEach(v -> System.out.println(v.toStringPrettier()));
});
```

* `groupingBy`를 사용하면 연도별로 자동 그룹핑
* 가장 유지보수가 유연한 방법으로 판단됨

---

## 🧠 오늘의 회고

* Stream의 다양한 기능들 (map, filter, sorted, distinct, groupingBy 등)을 실전에서 익힘
* Optional 관련 메서드 (`orElse`, `getAsDouble`)의 위험성과 대처법 학습
* `groupingBy`는 앞으로도 데이터 분류할 때 자주 쓰일 테니 꼭 기억하기!
* 불필요하게 클래스를 만드는 것보다 Stream API의 기능을 잘 활용하는 게 더 좋을 수 있음
* `Integer` → `int` 변환 시에는 **null 체크와 예외 처리**에 주의하기

