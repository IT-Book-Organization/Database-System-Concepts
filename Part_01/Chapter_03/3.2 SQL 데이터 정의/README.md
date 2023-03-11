# 3.2 SQL 데이터 정의

<br/>

`데이터 정의 언어(DDL)`을 이용하면 **데이터베이스의 릴레이션을 정의**할 수 있으며, 다음과 같은 각 릴레이션에 관한 정보도 명세할 수 있게 해준다.

- 각 릴레이션의 스키마
- 각 속성값의 타입
- 무결성 제약 조건
- 각 릴레이션을 위해 만들어진 인덱스의 집합
- 각 릴레이션에 대한 보안 및 권한 부여 정보
- 디스크에 저장된 릴레이션의 물리적 저장 구조

<br/>
<br/>
<br/>

# 3.2.1 기본 타입

SQL 표준은 다음의 다양한 내장 타입을 지원한다.

<br/>

### char(n)

- 사용자가 지정한 길이 n을 갖는 **고정 길이 문자**
    - e.g., char(10) 타입의 속성에 “Avi”라는 문자열을 저장하면, **10문자 길이를 맞추기 위해 일곱 개의 공백이 추가된다.**
- char 타입의 두 값을 비교할 때 두 값의 길이가 다르면, 비교하기 전에 길이가 짧은 값에 공백을 자동으로 더해 두 값이 같은 크기가 되도록 만든다.
- char 대신 `character`가 사용될 수도 있다.

<br/>

### varchar(n)

- 사용자가 지정한 최대 길이 n을 갖는 **가변 길이 문자**
    - e.g., varchar(10) 타입의 속성에 “Avi”라는 문자열을 저장하면, **공백 추가는 생기지 않는다.**
- varchar 대신 `character varying`이 사용될 수도 있다.

<br/>

### int

- **정수**(시스템에 따라 달라질 수 있는 정수의 부분 집합)
- int 대신 `integer`가 사용될 수도 있다.

<br/>

### smallint

- **작은 정수**(시스템에 따라 달라질 수 있는 정수 타입의 부분집합)

<br/>

### numeric(p, d)

- 사용자가 지정한 정밀도(precision)를 갖는 **고정 소수점 수(fixed-point number)**
    - 수는 `p`개의 숫자(및 부호)로 구성된다.
    - p개의 숫자 중 `d`개는 소수점 이하에 있는 숫자의 개수를 나타낸다.

<br/>

### real

- 시스템에 따라 달라질 수 있는 정밀도를 가지는 **부동 소수점 수**

<br/>

### double precision

- 시스템에 따라 달라질 수 있는 정밀도를 가지는 **배정밀도(double-precision) 부동 소수점 수**

<br/>

### float(n)

- 적어도 n개의 숫자로 나타낼 수 있는 정밀도를 갖는 **부동 소수점 수**

<br/>

---

<br/>

### null

각각의 타입은 특수값인 `널(null)` 값도 포함할 수 있다.

널 값은 값이 존재하지만 알 수 없거나, 값이 전혀 존재하지 않는 경우에 **값의 부재**를 나타낸다.

어떤 경우에는 널 값 입력 자체가 금지될 수 있다. *(’4.4.2 Not Null 제약 조건’에서 자세히 설명)*

<br/>

### char 타입과 varchar 타입의 비교

`char` 타입과 `varchar` 타입을 비교할 때,  
데이터베이스 세스템에 따라 비교하기 전에 길이가 같도록 varchar 타입에 여분의 공백이 더해질 수도 있고, 안 더해질 수도 있다.

따라서 char(10) 타입의 속성과 varchar(10) 타입의 속성에 “Avi”라는 동일한 문자열을 저장한 것의 비교 결과는 거짓(false)이 될 수 있다.

> 이런 문제를 없애기 위해 **항상 char 타입 대신 varchar 타입을 사용**하는 것을 추천한다.

<br/>

### nvarchar

SQL은 `nvarchar` 타입을 제공해 유니코드(Unicode)를 통해 다국어 데이터를 저장할 수 있다.

그럼에도 많은 데이터베이스는 varchar 타입으로도 (**UTF-8 기반**) 유니코드를 저장할 수 있다.

<br/>
<br/>
<br/>

# 3.2.2 기본 스키마 정의

SQL의 `create table` 문(statement)을 사용하여 어떤 릴레이션을 정의할 수 있다.

```sql
create table r
(
    A1, D1,
    A2, D2,
    ...,
    An, Dn,
    <integrity-constraint1>,
    ..,
    <integrity-constraintk>
);
```

- `r` : 릴레이션의 이름
- `Ai` : 릴레이션 r의 스키마 속성의 이름
- `Di` : 속성 Ai의 도메인
    - 속성 Ai의 타입을 지정한다.
    - 속성 Ai가 특정 도메인에 해당하는 허용된 값만 가지도록 하는 선택적인 제약 조건을 명시한다.
- 세미콜론(`;`) : DBMS에 따라 필요할 수도 있고 필요 없을 수도 있다.

<br/>

e.g., department 릴레이션의 생성

```sql
create table department
(
    dept_name varchar(20),
    building  varchar(15),
    budget    numeric(12, 2), # 총 12개의 숫자, 그중 2개는 소수점 이하 숫자
    primary key (dept_name) # 주 키 선언
);
```

<br/>
<br/>

## 무결성 제약 조건

SQL은 서로 다른 다수의 무결성 제약 조건을 지원한다.

*(더 자세한 내용은 4.4절에서 다룸)*

<br/>

### primary key(Aj1, Aj2, … , Ajm)

`주 키(primary key) 명세`는 속성 Aj1, Aj2, … , Ajm 등이 **릴레이션의 주 키를 구성한다**는 것을 나타낸다.

주 키 명세는 생략될 수 있으나, 일반적으로 각 릴레이션에 대해 주 키를 명시하는 것이 좋다.

<br/>

> 주 키 속성은 널이 아니거나 유일해야 한다.

- 어떤 튜플도 주 키 속성에 대해 널 값을 가질 수 없다.
- 어떠한 두 개의 튜플도 모든 주 키 속성에 대해 같은 값을 가질 수 없다.

<br/>

### foreign key(Ak1, Ak2, … , Akn) references s

`외래 키(foreign key) 명세`는 릴레이션의 어떤 튜플에 대한 속성값(Ak1, Ak2, … , Akn)이  
반드시 `(상대) 릴레이션 s`**가 갖고 있는 일부 튜플의 주 키 속성값에 상응해야 한다.**

<br/>

e.g.,

```sql
create table department
(
    dept_name varchar(20),
    building  varchar(15),
    budget    numeric(12, 2),
    primary key (dept_name)
);

create table course
(
    course_id varchar(7),
    title     varchar(50),
    dept_name varchar(20),
    credits   numeric(2, 0),
    primary key (course_id),
    foreign key (dept_name) references department(dept_name) # 외래 키 선언
);
```

- 각 course 튜플에 대해, 그 튜플이 지정한 학과 이름은 반드시 department 릴레이션의 주 키 속성인 (dept_name)에 존재해야 한다.
- MySQL을 포함한 일부 데이터베이스 시스템에서는 참조된 테이블의 참조된 속성이 명시적으로 나열되어야 한다.  
  foreign key (dept_name) references department`(dept_name)`

<br/>

### not null

속성에 대한 `not null` 제약 조건은 **그 속성에 대한 널 값을 불허한다.**

즉, 이 제약 조건은 그 속성의 도메인에서 널 값을 제외한다.

<br/>

e.g.,

```sql
create table instructor
(
    ID        varchar(5),
    name      varchar(20) not null,
    dept_name varchar(20),
    salary    numeric(8, 2),
    primary key (ID),
    foreign key (dept_name) references department (dept_name)
);
```

instructor 릴레이션의 name 속성에 대한 not null 제약 조건은 교수의 이름이 **널이 될 수 없음을 보장한다.**

<br/>
<br/>

SQL은 무결성 제약 조건을 위반하는 모든 데이터베이스 갱신을 막는다. (SQL이 오류를 표시하고 해당 갱신을 막음)

- 릴레이션에 새롭게 추가되거나 수정된 튜플이 주 키 속성에 대해 널 값을 가지는 경우
- 튜플이 주 키 속성에 대해 릴레이션 내의 다른 튜플과 같은 값을 가지는 경우
- 외래 키 제약 조건에 위배되는 경우
    - e.g., department 릴레이션에 없는 dept_name 값을 갖는 course 튜플의 삽입

<br/>
<br/>


## 데이터 조작 명령문

새로 생성되는 릴레이션은 초기에는 비어 있다.

릴레이션에서 튜플을 삽입, 갱신, 삭제하는 작업은 데이터 조작 명령문인 `insert`, `update`, `delete`에 의해 수행된다.

*(3.9절에서 다룸)*

<br/>
<br/>

## 릴레이션의 제거

데이터베이스에서 릴레이션을 제거하기 위한 명령문은 `drop table`이다.

이는 데이터베이스에서 제거될 릴레이션과 관련된 모든 정보를 삭제한다.

```sql
drop table r; # 릴레이션 r의 모든 튜플 뿐만 아니라 r의 스키마까지도 삭제한다.
```

r가 제거되면 create table 문을 통해 r를 다시 생성하지 않는다면 r에 튜플 삽입은 불가능하다.

<br/>

drop table 명령문은 삭제 명령어 `delete from`보다 더 과감한, 파급력이 큰 명령이라고 할 수 있다.

```sql
delete from r; # 릴레이션 r만 남기고 r의 모든 튜플을 제거한다.
```

<br/>
<br/>

## 속성의 추가와 제거

alter table 문은 이미 존재하는 릴레이션에 속성을 추가할 수 있게 한다.

**릴레이션의 모든 튜플에 새로운 속성값으로 널이 할당된다.**

```sql
alter table r add A D;
```

- `r` : 이미 존재하는 릴레이션의 이름
- `A` : 추가될 속성의 이름
- `D` : 추가될 속성의 도메인

<br/>

릴레이션에서 속성을 제거하려면 다음 구문을 사용할 수 있다.

```sql
alter table r drop A;
```

- `r` : 이미 존재하는 릴레이션의 이름
- `A` : 릴레이션의 속성 이름

<br/>

다수의 데이터베이스 시스템이 어떤 테이블을 통째로 제거하는 것을 허용할 수 있으나, 속성을 제거하는 것은 지원하지 않는 경우가 많다.
