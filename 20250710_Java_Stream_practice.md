# 🗓️ 2025년 7월 10일


equalsIgnoreCase
대소문자를 신경쓰지 않고 비교

클래스에서 equals, 해쉬코드를 써주지 않아도
stream 연산에서는 값이 똑같으면 중복으로 보기 때문에
distinct를 사용하면 중복이 제거된다.
원칙상으로는 클래스에 equals, 해쉬코드를 오버라이드 해줘야한다.




연습 4: 모든 거래자의 이름을 리스트에 모아서
알파벳순으로 오름차정렬하여 반환
```java 
List<String> nameList = transactions.stream()
    .map(trs -> trs.getTrader().getName())
    .distinct()
//  .sorted(Comparator.reverseOrder()) // 내림차
    .sorted()
    .collect(toList());
```

정렬을 할 데이터의 스트링, 인트, 더블같은 경우에 오름차 정렬을 할 때는
.sorted()로 sorted에 파라미터를 안 넣어주면 자동으로 된다.
내림차를 하고싶을 때는 `.sorted(Comparator.reverseOrder())`






문제 7. 모든 거래에서 최고거래액은 얼마인가?
선생님 방식:
```java
int max = transactions.stream()
    .mapToInt(trs -> trs.getValue())
    .max()
    .orElse(0)
    ;
```
max를 사용하면 결과물로 OptionalInt 타입이 나온다.
Optional : NPE(NullPointerException) 방지를 위한 자바의 노력
orElse를 이용해서 int로 바꿔줌

내 방식:
```java
Integer highestTransactionValue = transactions.stream()
    .map(tr -> tr.getValue())
    .sorted((o1, o2) -> o2 - o1)
    .findFirst()
    .get();
```
내 방식은 Integer를 반환하므로 나중에 계산을 위해서는
Integer -> int로 변경하는 작업이 필요하다.






연습 8. 가장 작은 거래액을 가진 거래정보 탐색

내 방식
```java
Integer lowestTransactionValue = transactions.stream()
   .map(tr -> tr.getValue())
   .sorted((o1, o2) -> o1 - o2)
   .findFirst()
   .get();

System.out.println("최저거래액 = " + lowestTransactionValue);
```
transactions를 거래액으로 매핑,
.sorted((o1, o2) -> o1 - o2)은
.sorted()로 변경 가능, 오름차순으로 정렬
findFirst를 통해 가장 낮은 거래액을 찾고
get을 통해 가져오는 방식

내 방식은 Integer를 반환하므로 나중에 계산을 위해서는
Integer -> int로 변경하는 작업이 필요하다.


선생님 방식
1. 반복문을 이용해서 구하는 방식
```java
Transaction minTrs = transactions.get(0);
for (Transaction trs : transactions) {
   if (minTrs.getValue() > trs.getValue()) {
       minTrs = trs;
   }
}
```

2. stream을 이용한 방식
```java
Transaction minTrs = transactions.stream()
    .min(Comparator.comparing(Transaction::getValue))
    .orElse(null);
```

min 내부에 Comparator.comparing(Transaction::getValue)를
적어줌으로써 transactions 중에서 Value를 기준으로 제일 작은
Optional<Transaction>을 반환함.
orElse를 사용하여 Optional<Transaction>을 Transaction 으로 만들어줌


연습 10. 모든 거래에서 가장 작은 거래액보다
큰 거래액을 가진 거래의 평균을 계산하시오.
내 방식
```java
double averageAboveLowestTransaction = transactions.stream()
   .filter(tr -> tr.getValue() > lowestTransactionValue)
   .mapToInt(tr -> tr.getValue())
   .average()
   .getAsDouble();
```
1. 이전에 구했던 가장 작은 거래액(최소 거래액) lowestTransactionValue를
   사용하여 필터링
2. Transaction의 거래액을 mapToInt를 통해 Int로 매핑
3. average() 평균이 나오게 연산
4. getAsDouble() Double 타입으로 변환
   getAsDouble은 널 포인터 익셉션이 뜰 위험이 크므로 사용하지
   않는게 좋다고 한다.

선생님 방식
```java
// 최소 거래액 찾기
int minValue = transactions.stream()
   .mapToInt(trs -> trs.getValue())
   .min()
   .orElse(0);

System.out.println("minValue = " + minValue);

// 평균 구하기
double average = transactions.stream()
   .filter(trs -> trs.getValue() > minValue)
   .mapToInt(trs -> trs.getValue())
   .average()
   .orElse(0.0);

System.out.println("average = " + average);
```
평균을 구할 때 getAsDouble이 위험한 방식이므로
.orElse(0.0)을 통해 double 타입으로 만들어줬다.


연습 11. "Cambridge"에서 거래하는 모든 거래자의
거래정보들을 연도별로 그룹화하여 출력하시오.

내 방식
```java
CambridgeTransaction cambTransaction = new CambridgeTransaction();
transactions.stream()
   .filter(tr -> tr.getTrader().getCity().equals("Cambridge"))
   .forEach(tr -> cambTransaction.makeCamb(tr));

System.out.println(cambTransaction);
```
- 거래정보들을 Cambridge에서의 거래로 필터링
- CambridgeTransaction이라는 클래스를 새로 생성
- 그 클래스의 메서드인 makeCamb()를 이용해서 필드에 있는 Map에 데이터를 담고
- 내용을 출력하게 만들음
- 하지만 이것은 억지로 틀에 맞춰서 만들었을 뿐, transactions의 내용이 바뀌면
- 메서드도 대거 수정해야하는 수고가 필요할 것으로 예상된다.


선생님 풀이 1
```java
// 최종 연산 결과 데이터
Map<Integer, List<Transaction>> groupByYearMap = new HashMap<>();

// 2021년 거래들만 필터링
List<Transaction> trs2021 = transactions.stream()
   .filter(trs -> trs.getYear() == 2021 && trs.getTrader().getCity().equalsIgnoreCase("cambridge"))
   .collect(toList());

// 2022년 거래들만 필터링
List<Transaction> trs2022 = transactions.stream()
   .filter(trs -> trs.getYear() == 2022 && trs.getTrader().getCity().equalsIgnoreCase("cambridge"))
   .collect(toList());

// 최종 데이터 맵에 넣기
groupByYearMap.put(2021, trs2021);
groupByYearMap.put(2022, trs2022);

// 예쁘게 출력
for (Integer year : groupByYearMap.keySet()) {
   System.out.println("year: " + year);
   List<Transaction> trs = groupByYearMap.get(year);
   for (Transaction tr : trs) {
       System.out.println(tr.toStringPrettier());
   }
   System.out.println("--------------------------");
}
```
- 나와 다르게 선생님은 클래스를 새로 만들지 않음
- 2021, 2022년 거래들을 따로 필터링하여 Map에 넣어줌
- 결과적으로는 비슷하다고 생각한다.


---

선생님 풀이 2
```java
Map<Integer, List<Transaction>> groupByYearMap2 = transactions.stream()
   .filter(trs -> trs.getTrader().getCity().equalsIgnoreCase("cambridge"))
   .collect(Collectors.groupingBy(Transaction::getYear));

groupByYearMap2.forEach((key, value) -> {
   System.out.println("year: " + key);
   value.forEach(v -> System.out.println(v.toStringPrettier()));
   System.out.println("------------------------");
});
```
- 내 풀이, 선생님 풀이1과 다르게 여기서는 groupingBy를 사용하여 transactions이 변경되어도
- 확실하게 trs.getYear()를 키로 하여 맵으로 모두 묶어줌

- `.collect(Collectors.groupingBy(trs -> trs.getYear()))`는 특정 데이터를 기준으로
  리스트들을 묶어줄 수 있으니 기억해야겠다고 생각함.

- Map은 for문이 돌아가지 않지만, forEach는 가능하다는 점도 새로 알게됐음.