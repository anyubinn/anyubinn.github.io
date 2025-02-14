---
title: "[2장] 웹과 데이터베이스"
categories: [개인공부, 자바 웹 개발 워크북]
tags: [Java, 웹 개발, Spring, JDBC, SQL, 데이터베이스]
date: 2025-02-11 16:00:00 +0900
toc_sticky: true
---
### 2.1 JDBC 프로그래밍 준비
***
#### <mark style='background-color: #f1f8ff'> JDBC 프로그램의 구조 </mark>
- `JDBC`
  - 자바와 데이터베이스를 연결하는 API (`java.sql`, `javax.sql` 패키지 활용)
- `JDBC 드라이버`
  - 데이터베이스와 자바 프로그램 간의 네트워크 데이터 처리

**JDBC 프로그램 작성 순서**

1. 네트워크를 통해 데이터베이스와 연결
2. 데이터베이스에 보낼 SQL을 작성하고 전송
3. 필요하다면 데이터베이스가 보낸 결과를 받아서 처리
4. 데이터베이스와 연결을 종료

#### <mark style='background-color: #f1f8ff'> 테스트 프로그램 작성하기 </mark>
```java
package org.zerock.dao;

import java.sql.Connection;
import java.sql.DriverManager;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;

public class ConnectTests {

    @Test
    public void testConnection() throws Exception {
        Class.forName("com.mysql.cj.jdbc.Driver");

        Connection connection = DriverManager.getConnection(
                "jdbc:mysql://localhost:3306/databasename",
                "user",
                "password"
        );

        Assertions.assertNotNull(connection);

        connection.close();
    }
}
```
- `Class.forName()`
  - JDBC 드라이버 로딩
- `Connection`
  - 네트워크 연결 객체
- `DriverManager.getConnection()`
  - 데이터베이스 연결
- `Assertions.assertNotNull()`
  - 연결 확인
- `connection.close()`
  - 데이터베이스와 연결을 종료

#### <mark style='background-color: #f1f8ff'> DDL (Data Definition Language) </mark>
- `DDL(Data Definition Language)`
  - 테이블을 생성하거나 특정한 객체들을 생성할 때 사용하는 SQL
  - create/alter/drop/truncate

```sql
create table tbl_todo (
	tno int primary key auto_increment,
	title varchar(100) not null,
	dueDate date not null,
	finished tinyint default 0
);
```

#### <mark style='background-color: #f1f8ff'> DML (Data Manipulate Language) </mark>
- `DML(Data Manipulate Language)`
  - 데이터를 조작할 때 사용하는 SQL
  - select/insert/update/delete

**데이터 추가 (insert)**

테이블은 데이터의 형식이나 틀을 만드는 것이기 때문에 실제 데이터를 추가하는 작업은 별도로 진행해야 함

```sql
insert into tbl_todo (title, dueDate, finished)
values ('Test...', '2025-02-11', 1);
```

**데이터 조회 (select)**

데이터를 조회하는 SQL은 흔히 쿼리(query)라고 하며, select를 이용해서 작성
- `from`
  - 가져오려는 데이터의 대상 테이블 지정
- `where`
  - 대상의 필터링 지정
```sql
select * from tbl_todo where tno < 10;
```

**데이터 수정 (update)**

기존 데이터를 수정하려면 update문을 이용해서 처리
- `set`
  - 특정한 칼럼의 내용 수정
- `where`
  - 수정하려는 대상 데이터 지정

```sql
update tbl_todo set finished = 0, title = 'Not Yet...' where tno = 3;
```

**데이터 삭제 (delete)**

데이터를 삭제하려면 delete문을 이용해서 처리
- `from`
  - 삭제하려는 데이터의 대상 테이블 지정
- `where`
  - 조건에 해당하는 데이터 삭제

```sql
delete from tbl_todo where tno > 5;
```

### 2.2 프로젝트 내 JDBC 구현
***
#### <mark style='background-color: #f1f8ff'> Lombok 라이브러리 </mark>
자바 코드에서 반복적인 작업을 자동으로 처리해주는 라이브러리
- `getter/setter`
  - `@Getter`, `@Setter`, `@Data`로 자동 생성
- `toString()`
  - `@ToString`으로 자동 생성
- `equals()/hashCode()`
  - `@EqualsAndHashCode`로 자동생성
- `생성자 자동 생성`
  - `@AllArgsConstructor`, `@NoArgsConstructor`로 생성자 생성
- `빌더 생성`
  - `@Builder`로 빌더 패턴 코드 생성

**Lombok 라이브러리 추가**

JDK21 이상에서는 annotationProcessor가 적용되지 않으므로 그 이하 버전을 사용해야 함

```
dependencies {
    compileOnly 'org.projectlombok:lombok:1.18.24'
    annotationProcessor 'org.projectlombok:lombok:1.18.24'

    testCompileOnly 'org.projectlombok:lombok:1.18.24'
    testAnnotationProcessor 'org.projectlombok:lombok:1.18.24'
}
```

### 2.3 웹 MVC와 JDBC의 결합
***
#### <mark style='background-color: #f1f8ff'> ModelMapper 라이브러리 </mark>
- VO와 DTO의 차이점
  - `VO`
    - 주로 읽기 위주의 작업을 위해서만 사용
  - `DTO`
    - 주로 데이터 전송을 위해 사용
- `ModelMapper 라이브러리`
  - DTO ↔ VO 변환을 도와주는 라이브러리
  - getter/setter 등을 이용해서 객체의 정보를 다른 객체로 복사

#### <mark style='background-color: #f1f8ff'> Log4j2와 @Log4j2 </mark>
System.out.println() 대신 `Log4j2`를 활용해 더 효율적인 로그 관리 가능
- `Log4j2` 주요 기능
  - `레벨(level)`
    - 개발/운영 환경에 따라 로그 레벨 설정 가능
  - `어펜더(Appender)`
    - 로그 출력 방식을 설정 (콘솔, 파일 등)
![image](https://myblog.opendocs.co.kr/wp-content/uploads/2015/03/log4j.png)

```
dependencies {
    implementation group: 'org.apache.logging.log4j', name: 'log4j-core', version: '2.17.2'
    implementation group: 'org.apache.logging.log4j', name: 'log4j-api', version: '2.17.2'
    implementation group: 'org.apache.logging.log4j', name: 'log4j-slf4j-impl', version: '2.17.2'
}
```

**log4j2.xml 설정파일**

Log4j2 라이브러리의 설정은 log4j2.xml이라는 파일을 이용해서 설정함

```
<?xml version="1.0" encoding="UTF-8" ?>
<Configuration status="WARN">
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Root level="info">
            <AppenderRef ref="Console"/>
        </Root>
    </Loggers>
</Configuration>
```

**@Log4j2 어노테이션**

@Log4j2 어노테이션을 추가하고 System.out.println() 대신 log.info()와 같은 코드로 변경

```java
package com.zerock.jdbcex.service;

import com.zerock.jdbcex.dao.TodoDAO;
import com.zerock.jdbcex.domain.TodoVO;
import com.zerock.jdbcex.dto.TodoDTO;
import com.zerock.jdbcex.util.MapperUtil;
import lombok.extern.log4j.Log4j2;
import org.modelmapper.ModelMapper;

@Log4j2
public enum TodoService {
    INSTANCE;

    private TodoDAO dao;
    private ModelMapper modelMapper;

    TodoService() {
        dao = new TodoDAO();
        modelMapper = MapperUtil.INSTANCE.get();
    }

    public void register(TodoDTO todoDTO) throws Exception {
        TodoVO todoVO = modelMapper.map(todoDTO, TodoVO.class);

//        System.out.println("todoVO: " + todoVO);
        log.info(todoVO);

        dao.insert(todoVO);
    }
}
```
