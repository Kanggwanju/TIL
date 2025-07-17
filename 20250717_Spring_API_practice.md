학생 점수 관리 프로그램
Spring API 실습


@JsonProperty를 사용하면 프론트에서 필드 이름을
다르게 달라고 했을 때 바꿔서 줄 수 있음
ex)
`@JsonProperty("sum")
private int total;`



마스킹 할 때 글자 수에 따라 다르게 적용
```java
private static String maskNickName(String originName) {
    
    // 이름이 1글자인 사람 처리
    if (originName.length() <= 1) {
        return originName;
    }
    
    // 이름이 2글자인 사람 처리
    if (originName.length() <= 2) {
        return originName.charAt(0) + "*";
    }

    // 나머지 경우
    // 첫글자
    char firstLetter = originName.charAt(0);
    // 마지막 글자
    char lastLetter = originName.charAt(originName.length() - 1);

    // 완성된 마스킹 네임
    String maskingName = String.valueOf(firstLetter);

    for (int i = 1; i < originName.length() - 1; i++) {
        maskingName += "*";
    }
    maskingName += lastLetter;

    return maskingName;
}

```
- 이름 1글자, 2글자, 3글자 이상 다르게 처리해줘야함을 알게됐음.


---


```javascript
// 성적 정보 등록 이벤트
document.getElementById('createBtn').addEventListener('click', e => {

  e.preventDefault(); // form의 submit시 발생하는 새로고침 방지

  const $form = document.getElementById('score-form');
  // formData객체 생성
  const formData = new FormData($form);
  const scoreObj = Object.fromEntries(formData.entries());
  console.log(scoreObj);

  // 서버로 POST요청 전송
  fetchPostScore(scoreObj);

});
```
formData 객체를 생성하여,
form 데이터를 한번에 가져오는 방법을 새롭게 배움

---


입력값 검증
```java
public class ScoreCreateRequest {
    // 공백문자, null 둘다 허용하지 않음
    @NotEmpty(message = "이름은 필수값입니다.")
    @Pattern(regexp = "^[가-힣]+$", message = "이름은 한글로만 작성하세요!")
    private String studentName;

    @Min(value = 0, message = "국어 점수는 0점 이상이어야합니다.")
    @Max(value = 100, message = "국어 점수는 100점 이하여야합니다.")
    @NotNull(message = "국어점수는 필수값입니다.")
    private Integer korean;
}
```
- @NotEmpty: 공백문자, null 둘다 허용하지 않음
- 점수를 Integer로 한 이유는 기본값이 null이기 때문이다.
- int로 했을 경우에는 기본값이 0이기 때문에 null 체크를 못함

---

JSP 문법 주의
JSP에서 ${id}를 쓸 때는 조심해야함.
${id}는 서버가 내려주는 id이고,
\${stuId}는 stuId라는 자바스크립트의 변수임.


---

페이지 라우팅 데이터 전달
서버가 페이지 라우팅을 할 때 데이터를 내려주는 방법
JSP에게 특정 데이터(id)를 클라이언트에게 전송
페이지 라우팅을 할 때 파라미터로 Model 타입을 사용할 수 있음
이를 이용해서 클라이언트에게 데이터를 전송해줄 수 있다.

```java
@GetMapping("/{id}")
public String detailPage(@PathVariable("id") Long id, Model model) {
    // JSP에게 특정 데이터(id)를 전송
    model.addAttribute("stuId", id);
    return "score/score-detail";
}
```
@PathVariable 경로 변수를 통해 얻은 id를
model에 stuId라는 특성에 넣어줌.
`model.addAttribute("stuId", id);`

---