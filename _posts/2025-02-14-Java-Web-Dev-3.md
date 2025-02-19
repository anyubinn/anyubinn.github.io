---
title: "[3장] 세션/쿠키/필터/리스너"
categories: [개인공부, 자바 웹 개발 워크북]
tags: [Java, 웹 개발, Servlet, HttpSession, Cookie, WebFilter, Authentication]
date: 2025-02-14 16:00:00 +0900
toc_sticky: true
---
### 3.1 세션과 필터
***
#### <mark style='background-color: #f1f8ff'> 무상태에서 과거를 기억하는 방법 </mark>
HTTP는 기본적으로 무상태(stateless)이므로 과거의 요청 기록을 알 수 없지만 적은 자원으로 많은 요청 처리 가능

- `세션 트랙킹(sesstion tracking)`
  - 과거의 방문 기록을 추적
- `쿠키(Cookie)`
  - 서버와 브라우저 간 요청/응답 시 주고받는 데이터 조각
  - 이름(name)과 값(value) 구조로 구성, 유효기간에 따라 브라우저 메모리 또는 파일에 저장
  - 브라우저는 서버에서 받은 쿠키를 보관하고 이후 요청 시 자동으로 전송

**쿠키를 주고받는 기본적인 시나리오**
1. 브라우저에서 최초로 서버를 호출하는 경우에 해당 서버에서 발생한 쿠키가 없다면 브라우저는 아무것도 전송하지 않음
2. 서버에서는 응답(Response) 메시지를 보낼 때 브라우저에게 쿠키를 보내주는데 이때 'SetCookie'라는 HTTP 헤더를 이용
3. 브라우저는 쿠키를 받은 후에 이에 대한 정보를 읽고, 이를 파일 형태로 보관할 것인지 메모리상에서만 처리할 것인지를 결정. 이 판단은 쿠키에 있는 유효기간(만료기간)을 보고 판단
4. 브라우저가 보관하는 쿠키는 다음에 다시 브라우저가 서버에 요청(Request)할 때 HTTP 헤더에 Cookie라는 헤더 이름과 함께 전달(쿠키에는 경로를 지정할 수 있어서 해당 경로에 맞는 쿠키가 전송)
5. 서버에서는 필요에 따라서 브라우저가 보낸 쿠키를 읽고 이를 사용

**쿠키를 생성하는 방법**
- `서버에서 자동으로 생성하는 쿠키`
  - 응답 메시지를 작성할 때 정해진 쿠키가 없는 경우 자동으로 발행
    - WAS에서 발행되며 이름은 WAS마다 고유한 이름을 사용해서 쿠키를 생성
    - 톰캣은 'JSESSIONID'라는 이름을 이용
  - 서버에서 발행하는 쿠키는 기본적으로 브라우저의 메모리상에 보관. 브라우저를 종료하면 서버에서 발행한 쿠키는 삭제됨
  - 서버에서 발행하는 쿠키의 경로는 '/'로 지정됨
- `개발자가 생성하는 쿠키`
  - 이름을 원하는대로 지정 가능
  - 유효기간 지정 가능 (유효기간이 지정되면 브라우저가 이를 파일의 형태로 보관)
  - 반드시 직접 응답(Response)에 추가해야 함
  - 경로나 도메인 등을 지정할 수 있음 (특정한 서버의 경로를 호출하는 경우에만 쿠키를 이용)

#### <mark style='background-color: #f1f8ff'> 서블릿 컨텍스트와 세션 저장소 </mark>
- `서블릿 컨텍스트(ServletContext)`
  - 각 웹 애플리케이션에 고유 메모리 영역 제공
- `세션 저장소(Session Repository)`
  - 세션 쿠키(JSESSIONID)를 관리하는 메모리 공간
  - 쿠키마다 고유한 공간을 가지며, 키-값 구조로 데이터 저장 가능
  - 주기적으로 사용되지 않는 데이터를 삭제해 메모리 관리

**HttpServletRequest의 getSession()**
- `JSESSIONID가 없는 경우`
  - 새로운 세션 번호와 공간 생성, 객체 반환
- `JSESSIONID가 있는 경우`
  - 해당 세션 공간을 찾아 접근할 객체 반환

#### <mark style='background-color: #f1f8ff'> 세션을 이용하는 로그인 체크 </mark>
- `로그인 성공 시`
  - `HttpSession`에 특정 객체를 이름(key)와 함께 저장
- `로그인 확인 시`
  - 해당 객체가 존재하면 로그인된 사용자로 간주
  - 없으면 로그인 페이지로 이동

#### <mark style='background-color: #f1f8ff'> 필터를 이용한 로그인 체크 </mark>
- `필터`
  - 특정 경로에 대해 필터링 로직 실행
  - 중복 로그인 체크코드를 제거하고 필터로 분리 가능
  - `@WebFilter` 어노테이션으로 특정 경로에 필터 적용
![imgae](https://velog.velcdn.com/images/reasonoflife39/post/26e022d3-950a-442f-9130-df8b70406743/image.png)

```java
package com.zerock.jdbcex.filter;

import java.io.IOException;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;
import lombok.extern.log4j.Log4j2;

@WebFilter(urlPatterns = {"/todo/*"})
@Log4j2
public class LoginCheckFilter implements Filter {
    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws IOException, ServletException {
        log.info("Login check filter......");
        
        chain.doFilter(req, resp);
    }
}
```

- `@WebFilter`
  - 특정한 경로를 지정해서 해당 경로의 요청에 대해서 doFilter를 실행하는 구조
- `doFilter`
  - 다음 필터나 목적지(서블릿, JSP)로 갈 수 있도록 함

**EL의 Scope와 HttpSession 접근하기**
- `EL의 스코프(scope)`
  - 저장된 객체를 특정 범위 내에서 탐색
  - `Page Scope`
    - JSP에서 <c:set>으로 저장된 변수
  - `Request Scope`
    - HttpServletRequest에 setAttribute()로 저장된 변수
  - `Session Scope`
    - HttpSession을 이용해서 setAttribute()로 저장된 변수
  - `Application Scope`
    - ServletContext를 이용해서 setAttribute()로 저장된 변수
- 탐색 순서
  - page → request → session → application의 순서대로 저장된 객체를 찾음

### 3.2 사용자 정의 쿠키
***
#### <mark style='background-color: #f1f8ff'> 쿠키의 생성/전송 </mark>
**사용자 정의 쿠키 vs 자동 발생 쿠키**

|            | 사용자 정의 쿠키                                                | WAS에서 발행하는 쿠키 (세션 쿠키) |
|------------|----------------------------------------------------------|-----------------------|
| 생성         | 개발자가 직접 newCookie()로 생성<br>경로도 지정 가능                     | 자동                    |
| 전송         | 반드시 HttpServletResponse에 addCookie()를 통해야만 전송            |                       |
| 유효기간       | 쿠키 생성할 때 초 단위로 지정할 수 있음                                  | 지정불가                  |
| 브라우저의 보관방식 | 유효기간이 없는 경우에는 메모리상에만 보관<br>유효기간이 있는 경우에는 파일이나 기타 방식으로 보관 | 메모리상에만 보관             |
| 쿠키의 크기     | 4kb                                                      | 4kb                   |

**쿠키를 사용하는 경우**
- 쿠키는 서버와 브라우저 사이를 오가므로 보안에 취약
- 로그인 상태 유지를 위해 사용
- 쿠키 유효기간을 설정하면 브라우저 종료 후에도 보관됨

#### <mark style='background-color: #f1f8ff'> 쿠키와 세션을 같이 활용하기 </mark>
**쿠키를 이용한 자동로그인**
- 로그인
  - 로그인 시 임의의 문자열을 생성하여 쿠키와 데이터베이스에 저장
  - 유효기간을 1주일로 설정
- 로그인 체크
  - 사용자의 `HttpSession`에 로그인 정보가 없을 경우 쿠키를 확인하여 유효한 값인지 검사
  - 맞으면 `HttpSession`에 사용자 정보 추가
