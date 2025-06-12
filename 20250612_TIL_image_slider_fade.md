# 🧪 이미지 자동 슬라이드 Fade 전환 개선 기록

## 📅 2025년 6월 12일

## 📌 목적
- 기존 이미지 슬라이드 코드에서 **fade 전환 효과**를 구현하려 했지만 잘 되지 않아,  
- **선생님 코드와 비교**하여 내가 작성한 코드를 어떻게 개선할 수 있는지 정리함.

---

## 🧾 선생님 코드의 핵심 특징

- 전환 효과를 자연스럽게 구현하기 위해 다음 방식을 사용:
  1. 이미지 opacity를 `0`으로 설정 (사라지게)
  2. 일정 시간 후 이미지 소스를 변경하고 opacity를 `1`로 설정 (다시 나타나게)
  3. CSS `transition`으로 부드럽게 효과 처리

- 이미지 변경과 타이머 재시작 등을 함수로 잘 분리하여 코드 구조가 명확함

---

## 🛠️ 내 코드 개선 포인트

| 항목 | 현재 코드 | 개선 방향 |
|------|-----------|------------|
| 이미지 전환 | `src`만 즉시 변경 | `opacity`를 0으로 줄이고, 이미지 교체 후 다시 1로 |
| 전환 효과 | 없음 | CSS에서 `transition`으로 부드럽게 처리 |
| 함수 분리 | `insertImageSource`만 존재 | `updateImage`, `restartSlideShow` 등 기능별 함수 분리 필요 |
| 자동 슬라이드 | 단순 `setInterval` | 클릭 시 `clearInterval` 후 `setTimeout`으로 재시작 |

---

## ✅ 개선된 함수 예시

### 🎬 이미지 전환 함수
```js
function updateImage(index) {
  $image.style.opacity = 0;

  setTimeout(() => {
    currentIndex = index;
    $image.setAttribute('src', images[index]);
    $image.style.opacity = 1;
  }, 1000);
}
```

### 🔁 슬라이드 재시작 함수

```js
function restartSlideShow() {
  clearInterval(slideId);
  slideId = null;

  setTimeout(() => {
    startSlideShow();
  }, 1000);
}
```

---

## 🎨 필요한 CSS 추가

```css
#slider img {
  transition: 1s;
}
```

> `transition`이 없으면 `opacity` 변경이 즉시 적용되어 fade 효과가 나타나지 않음

---

## 🧠 오늘의 교훈

* JS의 `setTimeout`과 CSS의 `transition`을 조합하면 **자연스러운 화면 전환 효과**를 줄 수 있다.
* 화면 전환을 부드럽게 만들기 위해서는 상태 변화(전/후)를 분리하는 로직이 중요하다.
* 코드의 역할을 **기능 단위 함수로 분리하면** 가독성과 유지보수성이 좋아진다.

---

## 📁 참고 파일

* 📝 내 코드: `4-1-timer\practice1.html`
* ✅ 선생님 코드: `4-1-timer\practice1-refactored.html`
