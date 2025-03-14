---
title: "[프로그래머스 120803] 두 수의 차"
categories: [프로그래머스, Lv0]
tag: [Java]
date: 2025-02-28 11:30:00 +0900
toc_sticky: true
---
### 두 수의 차
> Lv0

***

#### 문제
정수 `num1`과 `num2`가 주어질 때, `num1`에서 `num2`를 뺀 값을 return하도록 soltuion 함수를 완성해주세요.

#### 제한사항
- -50000 ≤ `num1` ≤ 50000
- -50000 ≤ `num2` ≤ 50000

#### 입출력 예

| num1 | num2 | result |
|------|------|--------|
| 2    | 3    | -1     |
| 100  | 2    | 98     |

#### 입출력 예 설명
입출력 예 #1
- `num1`이 2이고 `num2`가 3이므로 2 - 3 = -1을 return합니다.

입출력 예 #2
- `num1`이 100이고 `num2`가 2이므로 100 - 2 = 98을 return합니다.

#### 코드
```java
class Solution {
  public int solution(int num1, int num2) {
    return num1 - num2;
  }
}
```
