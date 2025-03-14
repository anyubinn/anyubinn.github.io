---
title: "[프로그래머스 120804] 두 수의 곱"
categories: [프로그래머스, Lv0]
tag: [Java]
date: 2025-03-04 11:30:00 +0900
toc_sticky: true
---
### 두 수의 곱
> Lv0

***

#### 문제
정수 `num1`, `num2`가 매개변수 주어집니다. `num1`과 `num2`를 곱한 값을 return하도록 soltuion 함수를 완성해주세요.

#### 제한사항
- 0 ≤ `num1` ≤ 100
- 0 ≤ `num2` ≤ 100

#### 입출력 예

| num1 | num2 | result |
|------|------|--------|
| 3    | 4    | 12     |
| 27  | 19    | 513     |

#### 입출력 예 설명
입출력 예 #1
- `num1`이 3, `num2`가 4이므로 3 * 4 = 12를 return합니다.

입출력 예 #2
- `num1`이 27, `num2`가 19이므로 27 * 19 = 513을 return합니다.

#### 코드
```java
class Solution {
  public int solution(int num1, int num2) {
    return num1 * num2;
  }
}
```
