---
title: "[프로그래머스 120802] 두 수의 합"
categories: [프로그래머스, Lv0]
tag: [Java]
date: 2025-03-04 15:30:00 +0900
toc_sticky: true
---
### 두 수의 합
> Lv0

***

#### 문제
정수 `num1`과 `num2`가 주어질 때, `num1`과 `num2`의 합을 return하도록 soltuion 함수를 완성해주세요.

#### 제한사항
- -50,000 ≤ `num1` ≤ 50,000
- -50,000 ≤ `num2` ≤ 50,000

#### 입출력 예

| num1 | num2 | result |
|------|------|--------|
| 2    | 3    | 5      |
| 100  | 2    | 102    |

#### 입출력 예 설명
입출력 예 설명 #1
- `num1`이 2이고 `num2`가 3이므로 2 + 3 = 5를 return합니다.

입출력 예 설명 #2
- `num1`이 100이고 `num2`가 2이므로 100 + 2 = 102를 return합니다.

#### 코드
```java
class Solution {
  public int solution(int num1, int num2) {
    return num1+num2;
  }
}
```
