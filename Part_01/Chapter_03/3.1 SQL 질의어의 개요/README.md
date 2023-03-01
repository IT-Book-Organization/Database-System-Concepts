# 3.1 SQL 질의어의 개요

<br/>

IBM은 1970년대 초반 System R 프로젝트의 일부분으로 초기에 Sequel이라고 불리는 SQL의 초기 버전을 개발했고,  
그 후 Sequel 언어는 계속 발전되다가 `SQL(Structured Query Language)`로 그 이름을 바꾸었다.

<br/>
<br/>

## SQL 언어의 구성

### 데이터 정의 언어(Data-definition language, DDL)

SQL DDL은 릴레이션 스키마를 정의하고, 릴레이션을 삭제하고, 릴레이션 스키마를 수정하는 명령어를 제공한다.

<br/>

### 데이터 조작 언어(Data-manipulation language, DML)

SQL DML은 정보를 찾기 위해 데이터베이스에 질의하고, 튜플을 삽입・삭제・수정하는 기능을 제공한다.

<br/>

### 무결성(Integrity)

SQL DDL은 데이터베이스에 저장될 데이터가 반드시 만족해야 하는 `무결성 제약 조건`을 명시하는 명령어를 포함한다.

DBMS는 무결성 제약 조건을 위반하는 갱신을 허용하지 않는다.

<br/>

### 뷰 정의(View definition)

SQL DDL은 뷰를 정의할 수 있는 명령어를 포함한다.

<br/>

### 트랜잭션 제어(Transaction control)

SQL은 트랜잭션의 시작과 끝을 명시하는 명령어를 포함한다.

<br/>

### 내장 SQL(Embedded SQL)과 동적 SQL(Dynamic SQL)

내장 SQL과 동적 SQL은 C, C++, Java와 같은 범용 프로그래밍 언어에 내장되어 사용될 수 있다.

<br/>

### 권한 부여(Authorization)

SQL DDL은 릴레이션과 뷰에 접근할 권한을 부여하는 명령어를 포함한다.

<br/>
<br/>

## DBMS

DBMS는 이번 장에서 설명하는 대부분의 SQL 표준 특성을 지원한다.

<br/>

> SQL을 특정 DBMS가 이해할 수 있도록 표현하는 것은 DBMS별로 차이가 있다.

대부분의 DBMS는 일부 SQL 표준이 아닌 특성을 지원하는 반면, 여러 고급 혹은 최신 특성은 지원하지 않기도 한다.
