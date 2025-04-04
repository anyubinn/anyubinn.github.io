---
title: "[프로그래머스 120831] 짝수의 합"
categories: [프로그래머스, Lv0]
tag: [Java]
date: 2025-03-04 15:30:00 +0900
toc_sticky: true
---
### 짝수의 합
> Lv0

***

#### 문제
정수 `n`이 주어질 때, `n`이하의 짝수를 모두 더한 값을 return 하도록 solution 함수를 작성해주세요.

#### 제한사항
0 < `n` ≤ 1000

#### 입출력 예

| angle | result |
|-------|--------|
| 10    | 30     |
| 4     | 6      |

#### 입출력 예 설명
입출력 예 설명 #1
- `n`이 10이므로 2 + 4 + 6 + 8 + 10 = 30을 return 합니다.

입출력 예 설명 #2
- `n`이 4이므로 2 + 4 = 6을 return 합니다.

#### 코드
```java
class Solution {
  public int solution(int n) {
    int answer = 0;
    for(int i=0;i<=n;i++){
      if(i%2==0){
        answer+=i;
      }
    }
    return answer;
  }
}
```
