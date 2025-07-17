# 🧪 2025년 7월 17일 - Spring API 실습 정리
> 학생 점수 관리 프로그램 Spring API 실습을 진행

## 📌 오늘의 핵심 키워드

* `@JsonProperty`
* 마스킹 처리
* `FormData` → JSON 변환
* 입력값 유효성 검사
* JSP 표현식 `${}` 주의
* `Model` 객체를 통한 데이터 전달

---

## 1. @JsonProperty로 응답 필드명 변경하기

프론트엔드에서 JSON 응답 필드명을 요청한 형태로 바꿔야 할 때 사용합니다.

```java
@JsonProperty("sum")
private int total;
```

> `total`이라는 필드를 JSON 응답에서 `"sum"`이라는 이름으로 내려줍니다.

---

## 2. 닉네임 마스킹 처리

이름 길이에 따라 마스킹 방식이 달라져야 함을 실습을 통해 배움.

```java
private static String maskNickName(String originName) {
    // 이름이 1글자인 사람 처리
    if (originName.length() <= 1) return originName;

    // 이름이 2글자인 사람 처리
    if (originName.length() == 2) {
        return originName.charAt(0) + "*";
    }

    // 나머지 경우
    char first = originName.charAt(0);
    char last = originName.charAt(originName.length() - 1);
    String middle = "*".repeat(originName.length() - 2);

    return first + middle + last;
}
```

---

## 3. FormData로 폼 데이터 한 번에 추출하기

JavaScript에서 `form` 데이터를 한 번에 추출하는 `FormData` 객체 사용법을 학습함.

```javascript
document.getElementById('createBtn').addEventListener('click', e => {
  e.preventDefault(); // 새로고침 방지
  
  const $form = document.getElementById('score-form');
  
  // formData객체 생성
  const formData = new FormData($form);
  const scoreObj = Object.fromEntries(formData.entries());
  console.log(scoreObj);
  
  fetchPostScore(scoreObj); // 서버로 POST요청 전송
});
```

---

## 4. 입력값 검증을 위한 Bean Validation

```java
public class ScoreCreateRequest {

    @NotEmpty(message = "이름은 필수값입니다.")
    @Pattern(regexp = "^[가-힣]+$", message = "이름은 한글로만 작성하세요!")
    private String studentName;

    @NotNull(message = "국어점수는 필수값입니다.")
    @Min(value = 0, message = "국어 점수는 0점 이상이어야합니다.")
    @Max(value = 100, message = "국어 점수는 100점 이하여야합니다.")
    private Integer korean;
}
```

* `@NotEmpty`: 공백문자, null 둘 다 허용하지 않음
* `Integer`를 사용하는 이유: `int`는 기본값이 0이라 null 여부 검사가 불가능함

---

## 5. JSP 표현식 `${}` 주의점

```jsp
${id}       // 서버에서 내려준 값
\${stuId}   // JS 변수명으로 사용 (이스케이프 처리 필요)
```

> JSP에서는 `${}`는 서버 렌더링 대상이므로,
> 클라이언트 JS 변수와 구분하려면 `\${}` 형태로 써야 함.

---

## 6. 서버에서 클라이언트로 데이터 전달 - Model 사용

```java
@GetMapping("/{id}")
public String detailPage(@PathVariable("id") Long id, Model model) {
    model.addAttribute("stuId", id); // JSP에서 사용할 데이터 전달
    return "score/score-detail";     // 뷰 이름 반환
}
```

> 경로 변수 `id`를 `Model` 객체에 담아 JSP로 전달합니다.

---

## ✅ 오늘의 학습 요약

| 항목          | 학습 내용                                             |
| ----------- | ------------------------------------------------- |
| 응답 커스터마이징   | `@JsonProperty` 사용하여 JSON 필드명 변경                  |
| 입력 검증       | `@NotEmpty`, `@NotNull`, `@Min`, `@Max` 등의 유효성 검사 |
| 마스킹 처리      | 이름 길이에 따라 다르게 마스킹 적용                              |
| 클라이언트 요청 처리 | `FormData → JSON → fetch POST` 흐름 이해              |
| 데이터 전달      | `Model`을 통한 서버 → JSP 데이터 전송                       |
| JSP 문법 주의   | `${}` 표현식과 JS 변수의 구분 필요                           |

