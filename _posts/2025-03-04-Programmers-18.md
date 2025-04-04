---
title: "[프로그래머스 120817] 배열의 평균값"
categories: [프로그래머스, Lv0]
tag: [Java]
date: 2025-03-04 15:30:00 +0900
toc_sticky: true
---
### 배열의 평균값
> Lv0

***

#### 문제
정수 배열 `numbers`가 매개변수로 주어집니다. `numbers`의 원소의 평균값을 return하도록 solution 함수를 완성해주세요.

#### 제한사항
- 0 ≤ `numbers`의 원소 ≤ 1,000
- 1 ≤ `numbers`의 길이 ≤ 100
- 정답의 소수 부분이 .0 또는 .5인 경우만 입력으로 주어집니다.

#### 입출력 예

| numbers                                         | result |
|------------------------------------------------|--------|
| [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]                | 5.5    |
| [89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99]   | 94.0   |

#### 입출력 예 설명
입출력 예 설명 #1
- `numbers`의 원소들의 평균 값은 5.5입니다.

입출력 예 설명 #2
- `numbers`의 원소들의 평균 값은 94.0입니다.

#### 코드
```java
class Solution {
  public double solution(int[] numbers) {
    double answer = 0;
    int size=numbers.length;
    for(int i: numbers){
      answer+=i;
    }
    return answer/size;
  }
}
```
