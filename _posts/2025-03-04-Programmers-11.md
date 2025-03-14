---
title: "[프로그래머스 120805] 두 수의 곱"
categories: [프로그래머스, Lv0]
tag: [Java]
date: 2025-03-04 15:30:00 +0900
toc_sticky: true
---
### 두 수의 곱
> Lv0

***

#### 문제
정수 `num1`과 `num2`가 매개변수로 주어질 때, `num1`을 `num2`로 나눈 몫을 return하도록 soltuion 함수를 완성해주세요.

#### 제한사항
- 0 ≤ `num1` ≤ 100
- 0 ≤ `num2` ≤ 100

#### 입출력 예

| num1 | num2 | result |
|------|------|-------|
| 10    | 5    | 2     |
| 7  | 2    | 3     |

#### 입출력 예 설명
입출력 예 #1
- `num1`이 10, `num2`가 5이므로 10을 5로 나눈 몫 2를 return 합니다.

입출력 예 #2
- `num1`이 7, `num2`가 2이므로 7을 2로 나눈 몫 3을 return 합니다.

#### 코드
```java
class Solution {
  public int solution(int num1, int num2) {
    return num1/num2;
  }
}
```
