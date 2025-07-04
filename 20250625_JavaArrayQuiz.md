# 🗓️ 2025년 6월 25일 자바 배열 문제 풀이

## 📌 1. 배열 수정 문제

## 🧠 과제 목적 간단 요약

> 배열에 저장된 문자열 중에서 사용자가 입력한 이름을 수정하는 프로그램입니다.
> 이름이 존재할 경우 수정하고,
> 존재하지 않는 이름을 입력하면 안내 메시지를 출력합니다.

---

## 🆚 주요 차이점 정리

<details>
<summary>ArrayQuiz02.java</summary>

```java
package chap1_2.array;

import java.util.Arrays;
import java.util.Scanner;

public class ArrayQuiz02 {
    public static void main(String[] args) {

        String[] members = {"영웅재중", "최강창민", "시아준수", "믹키유천", "유노윤호"};

        System.out.println("* 변경 전 정보: " + Arrays.toString(members));
        Scanner scanner = new Scanner(System.in);
        boolean flag = false;

        while (true) {
            System.out.println("- 수정할 멤버의 이름을 입력하세요.");

            String target = scanner.nextLine();

            for (int i = 0; i < members.length; i++) {
                if (members[i].equals(target)) {
                    System.out.println(target + "의 별명을 변경합니다.");
                    System.out.print(">> ");
                    String new_name = scanner.nextLine();
                    members[i] = new_name;
                    System.out.println("- 변경 완료!");
                    System.out.println("* 변경 후 정보: " + Arrays.toString(members));
                    flag = true;
                    break;
                }
            }
            if (flag) break;
            System.out.println(target + "은(는) 없는 이름입니다.");
        }


        // 1. 수정 타겟의 이름을 입력받는다.
        // 2. 해당 이름이 있는지 탐색해본다.
        // 3. 탐색에 성공하면 해당 이름의 인덱스를 알아온다.
        // 4. 탐색에 실패하면 실패한 증거를 확보한다.
        // 5. 성공했을 시 수정을 원하는 새로운 이름을 입력받는다.
        // 6. 찾은 인덱스를 통해 새로운 이름으로 수정한다.
        // 7. 위 내용을 수정이 정확히 완료될때까지 반복한다.
    }
}

```
</details>

<details>
<summary>ArrayQuizRef02.java</summary>

```java
package chap1_2.array;

import java.util.Arrays;
import java.util.Scanner;

public class ArrayQuizRef02 {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        String[] tvxq = {"영웅재중", "최강창민",
            "시아준수", "믹키유천", "유노윤호"};

        System.out.println("* 변경 전 정보: " + Arrays.toString(tvxq));
        
        while (true) {
            System.out.println("- 수정할 멤버의 이름을 입력하세요.");
            String targetName = sc.nextLine();

            int index = -1;
            for (int i = 0; i < tvxq.length; i++) {
                if (tvxq[i].equals(targetName)) {
                    index = i;
                    break;
                }
            } // end for

            if (index != -1) {
                System.out.printf("%s의 이름을 변경합니다.\n", targetName);
                System.out.print(">> ");
                String newName = sc.nextLine();

                tvxq[index] = newName;
                System.out.println("- 변경 완료!");
                System.out.println("# 변경 후 정보: " + Arrays.toString(tvxq));
                break;
            } else {
                System.out.printf("%s은(는) 없는 이름입니다.\n", targetName);
            }
        } // end while

    } // end main
}
```
</details>

| 항목         | 내 풀이 (`ArrayQuiz02.java`)   | 선생님 풀이 (`ArrayQuizRef02.java`) | 개선 포인트       |
| ---------- | --------------------------- | ------------------------------ | ------------ |
| **변수명**    | `members`, `target`, `flag` | `tvxq`, `targetName`, `index`  | 명확한 의미 전달    |
| **루프 제어**  | `boolean flag` 로 제어         | `index != -1` 로 조건 분기          | 불필요한 변수 제거   |
| **배열 탐색**  | 변경 즉시 `break` + flag로 반복 종료 | `index`를 통해 유효성 먼저 판별 후 변경 수행  | 명확한 논리 흐름    |
| **출력 메시지** | 직접 문장 작성                    | `printf()`로 가독성 및 일관성 향상       | 사용자 응답에 친절함  |
| **구조적 설명** | 없음                          | 주석으로 절차 설명                     | 학습용 코드로 더 적합 |

---

## 📌 코드 스타일 차이

### 🧑‍💻 내 풀이 (간단하지만 개선 여지 있음)

```java
public static void main(String[] args) {
    boolean flag = false;

    while (true) {
        String target = scanner.nextLine();

        for (int i = 0; i < members.length; i++) {
            if (members[i].equals(target)) {
                // ...
                flag = true;
                break;
            }
        }

        if (flag) break;
        System.out.println("없는 이름입니다.");
    }
}
```

* ❌ `flag` 변수 사용으로 흐름이 살짝 복잡해짐
* ❌ 유효성 검사와 변경 로직이 혼합되어 있음

---

### 👨‍🏫 선생님 풀이 (구조적이고 명확함)

```java
public static void main(String[] args) {
    int index = -1;

    for (int i = 0; i < tvxq.length; i++) {
        if (tvxq[i].equals(targetName)) {
            index = i;
            break;
        }
    }

    if (index != -1) {
        // 변경 수행
    } else {
        // 실패 메시지
    }
}
```

* ✅ 탐색 → 유효성 판별 → 변경 수행 흐름이 분명함
* ✅ 구조적으로 `추상화된 사고` 연습에 도움됨
* ✅ 메시지 출력도 더 명확하고 사용자 친화적

---

## ✅ 개선 방향

| 항목         | 설명                                |
|------------| --------------------------------- |
| 변수명 구체화    | `target → targetName`, `flag` 제거  |
| 불필요한 상태 제거 | `flag` 없이 `index`로 판별하는 방식 채택     |
| 출력 메시지 개선  | `System.out.printf()`를 활용하면 더 깔끔  |
| 코드 절차 주석   | 선생님처럼 단계별 로직 설명 주석 추가하면 나중 복습에 도움 |
| 로직 분리 연습   | 탐색 → 유효성 → 처리 과정을 분리하면 로직 이해가 쉬워짐 |

---

## 📌 2. 배열 삭제 문제

## 🧠 과제 목적 간단 요약

> 배열에 저장된 문자열 중에서 사용자가 입력한 이름을 삭제하는 프로그램입니다.
> 이름이 존재할 경우 삭제하고, 배열 크기도 1 줄이며,
> 존재하지 않는 이름을 입력하면 안내 메시지를 출력합니다.

---

## 🆚 주요 차이점 비교
<details>
<summary>ArrayQuiz03.java</summary>

```java
package chap1_2.array;

import java.util.Arrays;
import java.util.Scanner;

public class ArrayQuiz03 {

    public static void main(String[] args) {

        String[] members = {"영웅재중", "최강창민", "시아준수", "믹키유천", "유노윤호"};

        System.out.println("* 변경 전 정보: " + Arrays.toString(members));
        Scanner scanner = new Scanner(System.in);
        boolean flag = false;


        while (members.length != 0) {
            System.out.println("삭제할 이름을 입력하세요.");
            System.out.print(">> ");
            String target = scanner.nextLine();

            for (int i = 0; i < members.length; i++) {
                if (members[i].equals(target)) {
                    for (int j = i; j < members.length - 1; j++) {
                        members[j] = members[j + 1];
                    }

                    String[] temp = new String[members.length - 1];
                    for (int k = 0; k < temp.length; k++) {
                        temp[k] = members[k];
                    }

                    members = temp;

                    System.out.println("- 삭제 후 배열: " + Arrays.toString(members));
                    flag = true;
                    break;
                }
            }

            if (!flag){
                System.out.println(target + "은(는) 없는 이름입니다.");
            }

            flag = false;
        }

    }
}

```
</details>

<details>
<summary>ArrayQuizRef03.java</summary>

```java
package chap1_2.array;

import java.util.Arrays;
import java.util.Scanner;

public class ArrayQuizRef03 {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        String[] tvxq = {"영웅재중", "최강창민", "시아준수", "믹키유천", "유노윤호"};

        System.out.println("* 변경 전 정보: " + Arrays.toString(tvxq));

        while (true) {
            System.out.println("- 삭제할 멤버의 이름을 입력하세요.");
            String targetName = sc.nextLine();

            int index = -1;
            for (int i = 0; i < tvxq.length; i++) {
                if (tvxq[i].equals(targetName)) {
                    index = i;
                    break;
                }
            } // end for

            if (index != -1) {

                // 삭제 여부 질문
                System.out.printf("진짜로 %s님을 삭제할까요? [y/n]\n", targetName);
                System.out.print(">> ");
                String answer = sc.nextLine();
                if (answer.equalsIgnoreCase("y")) {
                    for (int i = index; i < tvxq.length - 1; i++) {
                        tvxq[i] = tvxq[i + 1];
                    }

                    String[] temp = new String[tvxq.length - 1];
                    for (int i = 0; i < temp.length; i++) {
                        temp[i] = tvxq[i];
                    }
                    tvxq = temp;
                    temp = null;

                    System.out.println("- 삭제 완료!");
                    System.out.println("# 삭제 후 정보: " + Arrays.toString(tvxq));
                    break;
                }
            } else {
                System.out.printf("%s은(는) 없는 이름입니다.\n", targetName);
            }
        } // end while
    }
}

```
</details>


| 항목      | 내 풀이 (`ArrayQuiz03`) | 선생님 풀이 (`ArrayQuizRef03`)      | 개선 포인트                     |
| ------- | -------------------- | ------------------------------ | -------------------------- |
| 배열 변수명  | `members`            | `tvxq`                         | 동일한 의미지만 선생님 코드는 정체성이 분명 |
| 삭제 전 확인 | ❌ 없음                 | ✅ `"정말 삭제할까요? [y/n]"` 확인 절차 있음 | 사용자 친화성 향상                 |
| 인덱스 찾기  | 삭제와 동시에 반복문에서 처리     | 인덱스를 먼저 찾고 나서 분기 처리            | **탐색과 처리 로직 분리**로 가독성 ↑    |
| 삭제 방식   | 복사 및 줄이기 직접 구현       | 동일한 방식이나 **좀 더 명확한 주석과 구조**    | 주석이 친절해서 추적이 쉬움            |
| 불필요 변수  | `flag` 사용            | `flag` 없이 조건 분기로 처리            | **로직이 간결해짐**               |
| 실패 메시지  | `"없는 이름입니다."` 단순 출력  | `"OO은(는) 없는 이름입니다."` 구체적 출력    | 사용자 메시지 품질 향상              |

---

## 💡 코드 스타일 차이

### 👀 내 풀이 (핵심 구조)

```java
public static void main(String[] args) {
    for (int i = 0; i < members.length; i++) {
        if (members[i].equals(target)) {
            for (int j = i; j < members.length - 1; j++) {
                members[j] = members[j + 1];
            }
            String[] temp = new String[members.length - 1];
            for (int k = 0; k < temp.length; k++) {
                temp[k] = members[k];
            }
            members = temp;
            flag = true;
            break;
        }
    }
}
```

* ❌ `flag` 사용과 `없음 메시지` 출력 조건이 다소 혼란스러움

---

### ✅ 선생님 풀이 (구조적 개선)

```java
public static void main(String[] args) {
    int index = -1;
    for (int i = 0; i < tvxq.length; i++) {
        if (tvxq[i].equals(targetName)) {
            index = i;
            break;
        }
    }

    if (index != -1) {
        // 삭제 여부 확인
        // 삭제 로직
    } else {
        System.out.printf("%s은(는) 없는 이름입니다.\n", targetName);
    }
}
```

* ✅ 탐색 → 조건 판별 → 처리 순서가 **명확**
* ✅ 사용자 메시지나 입력 대응이 **친절하고 구체적**
* ✅ 코드에 **불필요한 상태 변수 없이 깔끔**

---

## ✨ 개선 방향

| 항목              | 설명                                                      |
| --------------- |---------------------------------------------------------|
| `flag` 대신 조건 분기 | `index = -1` 방식을 활용하면 훨씬 더 직관적                          |
| 사용자 경험 개선       | 삭제 확인(`y/n`) 추가, 입력 안내 메시지 개선                           |
| 변수 네이밍          | `target` → `targetName`, `members` → `tvxq` 등 **의미 부여** |
| 주석 작성 습관        | 선생님처럼 단계별 주석이 있으면 나중에 읽기 좋음                             |

---

## 📌 마무리 요약

* 나의 구현은 **전체 흐름과 기능이 잘 작동**하고 있지만,
* 선생님 코드는 **코드 구조, 변수 명명, 사용자 피드백 면에서 더 깔끔하고 설계적**이다.
* 다음 구현에서는 `flag` 변수 없이 조건 분기로 바꾸는 것부터 연습해야겠다.

