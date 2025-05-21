## ✏️ 실습 주제: 박스 모델 이해, 텍스트 스타일 적용

### 🧪 실습 요구사항
![박스모델실습2](box_model_practice2.png)

### ✅ 내가 작성한 코드
practice2.html
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Title</title>
    <link rel="stylesheet" href="practice2.css">
</head>
<body>
    <nav>
        <ul>
            <li><a href="#">[주문] 긴급 재난지원금을 쿠X에서 사용할 수 있나요?</a></li>
            <li><a href="#">다슬기를 키우고 싶은데 어떤 환경이 필요한가요?</a></li>
            <li><a href="#">달팽이가 집을 나갔습니다. 어떻게 하죠?</a></li>
        </ul>
    </nav>
</body>
</html>
```

practice2.css
```css
@import url("common.css");

nav {
    background-color: #bebbbb;
    width: 500px;
    margin: 10px auto;
}
ul {
    padding: 20px 0 20px 20px;
}
li {
    margin: 26px 0;
    padding: 10px 0;
}
a {
    color: #fff;
    font-size: 20px;
    text-decoration: none;
    padding: 10px 5px;
}
a:hover {
    color: #eaea6b;
    background-color: #87ceeb;
}
```



### 🔍 피드백 & 정답 코드 비교

* ✅ 좋은 점

  * 실습 문제의 html 구조를 잘 표현했음
  * margin을 이용하여 각 li 태그들의 거리를 벌려줬음
  * a 태그의 폰트 속성을 바꾼 건 잘함함


* ❗ 아쉬운 점

  * ul태그를 nav 태그로 한번 더 감쌌음 → 이유 없는 코드 부풀림
  * margin을 li 태그들의 위 아래에 모두 적용 → 자연스럽게 만들기 위해 padding을 추가하는 등 코드가 더 길어짐
  * 인라인 속성이라 a태그의 눌리는 부분이 작음 → `display: block;` 적용



* ✅ 개선된 코드

해답 html
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Title</title>
    <link rel="stylesheet" href="practice2.css">
</head>
<body>
<!--    ul.board>li*3>a-->
    <ul class="board">
        <li><a href="#">[주문] 긴급재난지원금을 쿠X에서 사용할 수 있나요?</a></li>
        <li><a href="#">다슬기를 키우고 싶은데 어떤 환경이 필요한가요?</a></li>
        <li><a href="#">달팽이가 집을 나갔습니다. 어떻게 하죠??</a></li>
    </ul>
</body>
</html>
```

해답 css
```css
@import url("common.css");
a {
    /* a태그 리셋 */
    color: inherit;
    text-decoration: none;
}

.board {
    background: gray;
    width: 70%;
    margin: 20px auto;
    font-size: 3em;
    color: #fff;
    padding: 15px;
}
.board li {
    /*border: 4px solid yellow;*/

    /* 각 요소들의 거리를 벌릴 때 margin */
    margin-bottom: 15px;
}
.board li:last-child {
    /* 제일 아래쪽은 .board의 패딩, li의 마진
     이 합쳐져서 더 넓어지므로 빼줌*/
    margin-bottom: 0;
}
.board li a {
    /* 인라인 속성이라 a태그의 눌리는 부분이 작음 */
    display: block;
    /*background: red;*/
}
.board li a:hover {
    color: yellow;
    background: skyblue;
}
```

---

### 💡 배운 점 정리

* html의 구조를 더 생각하면 불필요한 태그 추가를 피할 수 있음
* 각 li의 bottom에 margin을 넣고, 제일 아래에 있는 li만 margin을 빼주면 깔끔하게 만들 수 있음.
* a 태그는 인라인 속성을 가지고 있기 때문에 `display:block;`으로 설정해주면 눌리는 부분이 커짐.

---

### 🧠 다음에 적용할 것

* 불필요한 부분을 줄이는 연습하기
* 각 태그 별로 여러 속성을 적용하는 연습하기
