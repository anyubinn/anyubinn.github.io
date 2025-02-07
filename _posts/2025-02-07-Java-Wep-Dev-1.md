---
title: "[1장] 웹 프로그래밍의 시작"
categories: [개인공부, 자바 웹 개발 워크북]
tags: [Java, 웹 개발, 서블릿, JSP, MVC 패턴, Spring, HTTP, WAS, 웹 프로젝트, 웹 애플리케이션, 데이터베이스]
date: 2025-02-07 16:00:00 +0900
toc_sticky: true
---
### 1.1 자바 웹 개발 환경 만들기
***
#### <mark style='background-color: #f1f8ff'> 웹 프로젝트의 기본구조 </mark>
웹 프로젝트는 `브라우저, 웹서버(WAS), 데이터베이스`로 구성

**웹 프로젝트의 기본 구조**
- `브라우저(클라이언트)`
  - HTML, CSS, JavaScript를 이용해 사용자 요청을 서버로 전송하고, 응답을 화면에 출력
- `웹 서버(WAS)`
  - 클라이언트의 요청을 받아 정적(웹 서버) 또는 동적(WAS) 데이터를 제공
- `데이터베이스`
  - 관계형 데이터베이스(RDBMS)에서 SQL을 이용해 데이터를 저장 및 관리

#### <mark style='background-color: #f1f8ff'> 인텔리제이를 이용한 프로젝트 생성 </mark>
**프로젝트의 경로 설정**

프로젝트 시작 시 경로가 길게 출력됨. 이를 수정하기 위한 방법은 아래와 같음
<img width="871" alt="Image" src="https://github.com/user-attachments/assets/faf33a5d-8305-4e35-b7b5-c6558a3bb9ea" />

1. [Edit Configurations...]에 들어간다.
2. 상단의 탭 메뉴에서 [Deployment]를 선택한다.
3. 기존의 '...war'라는 이름으로 지정된 것을 [-]를 눌러 제거한다.
4. [+]를 눌러 'Artifact'를 '(exploded)'가 포함된 항목으로 지정한다.
5. 실행될 때의 경로로 지정되는 'Application Context'의 값은 '/'로 지정한다.

**변경된 코드의 반영**

일반 자바 프로그램과 달리 gradle을 이용해서 컴파일 등의 작업이 처리되고 프로젝트의 build/libs 폴더 안에 내용은 톰캣을 통해서 실행되므로 코드 변경 후 다시 톰캣을 재시작 해야 함. 톰캣의 재시작을 최소화 하기 위해서는 아래와 같은 방법이 필요함
<img width="880" alt="Image" src="https://github.com/user-attachments/assets/3b56c755-cf1d-42fa-96b4-c7907b771d1d" />

1. [Edit Configurations...]에 들어간다.
2. 'On Update action'과 'On frame deactivation' 설정을 위와 같이 조정한다.

.jsp는 소스코드를 변경하는 것만으로 반영되지만 자바코드의 경우 반영되지 않음

'services' 항목에서 [Deploy All]을 실행해서 현재 코드를 다시 빌드하고 build/libs 폴더의 내용물을 수정해야 반영됨

**서블릿 코드 작성하기**
- `서블릿(Servlet)`
  - HttpServlet을 상속받아 브라우저 요청을 처리하는 Java 클래스

```java
package com.zerock.w1;

import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet(name = "myServlet", urlPatterns = "/my")
public class MyServlet extends HttpServlet {
    
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");

        PrintWriter out = response.getWriter();
        out.println("<html><body>");
        out.println("<h1>MyServlet</h1>");
        out.println("</body></html>");
    }
}
```

- `@WebServlet` 
  - 특정 URL과 서블릿을 매핑
- `doGet()`
  - GET 요청을 처리하는 메서드
- `PrintWriter`
  - 브라우저에 HTML 응답을 출력

### 1.2 웹 기본 동작 방식 이해하기
***
#### <mark style='background-color: #f1f8ff'> Request(요청) / Response(응답) </mark>
브라우저는 서버에 데이터를 요청(Request)하고, 서버는 처리 후 응답(Response)를 보냄
- `GET 방식`
  - 주소(URL)에 데이터를 포함해 전송 (조회 및 공유 용도)
- `POST 방식`
  - 데이터를 별도로 전송 (회원가입, 로그인 등 보안이 필요한 작업)

**정적 vs 동적 데이터**
- `정적 데이터`
  - HTML, CSS, 이미지 (웹 서버에서 제공)
- `동적 데이터`
  - 요청에 따라 생성되는 데이터 (WAS에서 처리)

#### <mark style='background-color: #f1f8ff'> HTTP 라는 약속 </mark>
- `프로토콜(protocol)`
  - 웹에서 데이터를 주고받는 `통신 규약`
  - 기본적으로 `Header(헤더) + Body(본문)` 구조
  - `비연결성`
    - 요청-응답이 끝나면 연결을 종료하여 서버 부하를 줄임
![image](https://www.beusable.net/blog/wp-content/uploads/2021/02/image-7.png)

#### <mark style='background-color: #f1f8ff'> 자바 서버 사이드 프로그래밍 </mark>
**자바 웹 개발 기술**
- `Servlet`
  - Java 기반의 서버 프로그래밍 기술 (API 제공)
- `JSP`
  - HTML 코드 안에 Java를 포함할 수 있는 기술 (내부적으로 Servlet으로 변환됨)
- `서블릿 컨테이너(Tomcat)`
  - 서블릿 실행 및 생명주기 관리
  - 개발자가 직접 객체를 생성하지 않고, 컨테이너가 관리

#### <mark style='background-color: #f1f8ff'> JSP를 이용해서 GET/POST 처리하기 </mark>
- `GET`
  - 조회 및 입력 화면 제공 (쿼리 스트링 방식 사용)
- `POST`
  - 등록, 수정, 삭제 등의 작업 수행 (데이터가 URL에 노출되지 않음)
- 최근에는 `JSP에서 직접 요청을 처리하지 않고, 서블릿을 통해 처리`하는 방식이 권장됨 → `웹 MVC 패턴` 활용

### 1.3 Web MVC 방식
***
#### <mark style='background-color: #f1f8ff'> MVC 구조와 서블릿/JSP </mark>
웹 개발에서 `서블릿`과 `JSP`의 장단점
- `서블릿`
  - Java 코드 활용 가능하지만, HTML을 처리하기 어렵고 코드가 복잡해짐
- `JSP`
  - HTML 처리에 적합하지만, Java 코드와 혼재되어 유지보수가 어려움

이를 해결하기 위해 `MVC 패턴`을 적용하여 역할을 분리
- `Model(모델)`
  - 데이터 및 비즈니스 로직 관리
- `View(뷰, JSP)`
  - 사용자에게 화면을 출력
- `Controller(컨트롤러, 서블릿)`
  - 요청을 처리하고 데이터를 가공하여 뷰에 전달

**MVC 흐름**
1. 사용자가 브라우저에서 요청
2. 서블릿(Controller)이 요청을 받아 데이터(Model)을 준비
3. 서블릿이 JSP(View)로 데이터를 전달하여 화면을 생성
4. JSP가 결과를 브라우저에 출력
![image](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FYbumV%2FbtsjTv9eboD%2FDqQ8fPmaFTJV12M0LCSPIK%2Fimg.png)

#### <mark style='background-color: #f1f8ff'> MVC 구조로 다시 설계하는 계산기 </mark>
**MVC 원칙**
- 브라우저는 반드시 `서블릿(Controller)`을 호출
- JSP는 직접 호출되지 않고 `Controller를 통해서만 접근 가능`

**GET 입력 화면의 설계**
1. 브라우저가 `/input`을 호출
2. `InputController` 서블릿이 요청을 처리하고 input.jsp로 포워딩
3. `input.jsp`에서 HTML 입력 화면을 생성

**POST 처리의 설계**
1. `input.jsp`에서 `<form>`을 통해 `/calcResult`로 데이터 전송
2. `CalcResultServlet` 서블릿에서 `num1`,`num2`를 받아 계산
3. 결과를 `calcResult.jsp`에 전달하여 출력

#### <mark style='background-color: #f1f8ff'> 컨트롤러에서 뷰 호출 (RequestDispatcher 활용)</mark>
```java
package com.zerock.w1.calc;

import java.io.IOException;
import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet(name = "inputController", urlPatterns = "/calc/input")
public class InputController extends HttpServlet {
    
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("InputController...doGet...");

        RequestDispatcher dispatcher = request.getRequestDispatcher("/WEB-INF/calc/input.jsp");
        
        dispatcher.forward(request, response);
    }
}
```
- `RequestDispatcher`
  - 서블릿에서 JSP로 요청을 전달(포워딩)하는 객체
- `/WEB-INF/` 내부의 JSP는 직접 접근할 수 없고 반드시 서블릿을 통해 호출

#### <mark style='background-color: #f1f8ff'> PRG 패턴(Post-Redirect-GET) </mark>
- `PRG 패턴`
  1. 사용자가 POST 방식으로 요청 (ex. 게시글 등록)
  2. 서버에서 데이터 처리 후 `Redirect 응답`
  3. 브라우저가 GET 방식으로 이동하여 결과 화면을 출력
![image](https://velog.velcdn.com/images/bone0825/post/216a7f91-2a2d-4d21-bd6d-f26bad3bbe48/image.png)

**PRG 패턴의 장점**
- POST 요청의 중복 실행 방지 (새로고침 시 중복 등록 문제 해결)
- 사용자가 처리 완료 후 새로운 단계로 이동하는 느낌 제공

**PRG 패턴 예시 (게시판 등록)**
- `todo/register (POST)` → 서버에서 등록 처리
- Redirect → `/todo/list (GET)`로 이동하여 목록 화면 표시

**Todo 웹 애플리케이션 설계 (와이어 프레임)**
- `목록 화면 (GET)`
  - `/todo/list`에서 모든 할 일 조회
- `등록 화면 (GET → POST → Redirect → GET)`
  - `/todo/register (GET)`
    - 등록 폼 표시
  - `/todo/register (POST)`
    - 데이터 저장 후 목록으로 이동
- `조회 화면 (GET)`
  - `/todo/read`에서 특정 Todo 상세 조회
- `수정 화면 (GET → POST → Redirect → GET)`
  - `/todo/modify (GET)`
    - 수정 폼 표시
  - `/todo/modify (POST)`
    - 수정 처리 후 목록으로 이동
- `삭제 (POST → Redirect → GET)`
  - `/todo/remove (POST)`
    - 삭제 처리 후 목록으로 이동

**구현 목록 정리**

| 기능     |동작 방식|컨트롤러(org.zerock.w1.todo)|JSP|
|--------|---|---|---|
| 목록     |GET|TodoListController|WEB-INF/todo/list.jsp|
| 등록(입력) |GET|TodoRegisterController|WEB-INF/todo/register.jsp|
| 등록(처리) |POST|TodoRegisterController|Redirect|
| 조회     |GET|TodoReadController|WEB-INF/todo/read.jsp|
| 수정(입력) |GET|TodoModifyController|WEB-INF/todo/modify.jsp|
| 수정(처리) |POST|TodoModifyController|Redirect|
| 삭제(처리) |POST|TodoRemoveController|Redirect|

### 1.4 HttpServlet
***
**HttpServlet의 특징**
- `doGet()`, `doPost()` 등을 제공하여 GET/POST 요청을 쉽게 분리 가능
- WAS(톰캣)가 객체를 자동 생성 및 관리하여 개발자가 신경 쓸 필요 없음
- 멀티 스레드 환경에서 동시 요청을 처리할 수 있도록 설계됨
- `GenericServlet`은 HTTP에 특화되지 않은 요청/응답 기능을 제공하는 반면, `HttpServlet`은 HTTP 전용 기능을 지원
![image](https://blog.kakaocdn.net/dn/boogO2/btrtKGZSli2/fkMPEPmUOO30zY4AdPdAGk/img.png)

GenericServlet과 HttpServlet의 가장 큰 차이는 GenericServlet의 경우 HTTP 프로토콜에 특화되지 않는 요청(Request)과 응답(Response)에 대한 기능을 정의하고 있다는 것임

#### <mark style='background-color: #f1f8ff'> HttpServlet의 라이프사이클 </mark>
1. 브라우저가 특정 경로를 호출하면 톰캣이 해당 서블릿 클래스를 로딩하고 객체 생성
2. `init()` 실행 → 서블릿 초기화 (필요 시 load-on-startup 옵션 사용)
3. 브라우저 요청을 `HttpServletRequest`, `HttpServletResponse` 객체로 전달받아 처리
4. GET/POST 방식에 맞춰 `doGet()`, `doPost()` 실행 (서블릿 객체는 하나만 생성되며 동일 객체로 여러 요청 처리)
5. 톰캣 종료 시 `destroy()` 실행

#### <mark style='background-color: #f1f8ff'> HttpServletRequest의 주요 기능 </mark>
**HttpServletRequest 주요 기능**
HTTP 요청 정보를 제공 (헤더, 파라미터, 쿠키 등)

| 기능         | 메소드                                                                   | 설명                                 |
|------------|-----------------------------------------------------------------------|------------------------------------|
| HTTP 헤더 관련 | getHeaderNames()<br>getHeader(이름)                                     | HTTP 헤더 내용들을 찾아내는 기능               |
| 사용자 관련     | getRemoteAddress()                                                    | 접속한 사용자의 IP 주소                     |
| 요청 관련      | getMethod()<br>getRequestURL()<br>getRequestURI()<br>getServletPath() | GET/POST 정보, 사용자가 호출에 사용한 URL 정보 등 |
| 쿼리 스트링 관련  | getParameter()<br>getParameterValues()<br>getParameterNames()         | 쿼리 스트링 등으로 전달되는 데이터를 추출하는 용도       |
| 쿠키 관련      | getCookies()                                                          | 브라우저가 전송한 쿠키 정보                    |
| 전달 관련      | getRequestDispatcher()                                                |                                    |
| 데이터 저장     | setAttribute()                                                        | 전달하기 전에 필요한 데이터를 저장하는 경우에 사용       |

**주요 메소드**
- `getParameter(name)`
  - 쿼리 스트링에서 값 추출 (String 반환, 없는 경우 null)
- `getParameterValues(name)`
  - 동일한 이름의 여러 파라미터 처리
- `setAttribute(key, value)`
  - JSP로 데이터 전달 시 사용
- `getRequestDispatcher().forward()`
  - 다른 서블릿/JSP로 요청 전달

#### <mark style='background-color: #f1f8ff'> HttpServletResponse의 주요 기능 </mark>
HttpServletRequest가 주로 `읽는`기능을 제공한다면 HttpServletResponse는 반대로 `쓰는`기능을 담당

**HttpServletResponse의 주요 기능**

| 기능      | 메소드              | 설명                             |
|---------|------------------|--------------------------------|
| MIME 타입 | setContentType() | 응답 데이터의 종류를 지정(이미지/html/xml 등) |
| 헤더 관련   | setHeader()      | 특정 이름의 HTTP 헤더 지정              |
| 상태 관련   | setStatus()      | 404, 200, 500 등 응답 상태 코드 지정    |
| 출력 관련   | getWriter()      | PrintWriter를 이용해서 응답 메시지 작성    |
| 쿠키 관련   | addCookie()      | 응답 시에 특정 쿠키 추가                 |
| 전달 관련   | sendRedirect()   | 브라우저에 이동을 지시                   |

**주요 메소드**
- `setContentType()`
  - 응답 데이터 타입 지정 (HTML, JSON 등)
- `setStatus()`
  - HTTP 상태 코드 설정 (200, 404, 500 등)
- `getWriter()`
  - `PrintWriter`를 이용해 응답 메시지 작성
- `sendRedirect()`
  - 클라이언트들에게 새로운 URL로 이동하도록 응답
    - 페이지 새로고침 방지 및 완료 후 새로운 흐름을 만들 때 사용
