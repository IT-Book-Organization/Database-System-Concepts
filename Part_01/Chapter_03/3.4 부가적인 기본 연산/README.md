# 3.4 부가적인 기본 연산

<br/>

# 3.4.1 재명명 연산

```sql
select name, course_id
from instructor, teaches
where instructor.ID = teaches.ID;
```

위 질의에서 결과는 name, course_id의 속성을 갖는 릴레이션이었다.

결과 릴레이션의 속성의 이름은 `from` **절에 있는 릴레이션의 속성 이름으로부터 파생되었다.**

<br/>

그러나 아래의 상황에서는 이와 같은 방법으로 이름을 파생시킬 수 없다.

1. from 절에 있는 두 개의 릴레이션이 같은 이름을 갖는 경우
    - 그 속성의 이름이 결과 내에서 중복될 수 있다.


2. select 절에서 산술식을 사용했을 경우
    - 그 결과의 속성은 이름을 가지지 않는다.

<br/>

또한, 앞선 예제처럼 속성 이름을 원래의 릴레이션으로부터 얻는다고 하더라고 결과에서 그 속성의 이름을 바꾸고 싶은 경우도 있다.

<br/>
<br/>

## as 절

이에 따라 SQL은 **결과 릴레이션에서 속성의 이름을 다시 짓는 방법**, 즉 `as 절(as clause)`을 제공한다.

```sql
old-name as new-name
```

<br/>

> as 절은 `select` 절과 `from` 절 둘 다에서 나타날 수 있다.

(SQL의 초기 버전에는 as 키워드가 포함되지 않았기에 일부 DBMS, 특히 Oracle은 from 절에 as와 같은 키워드를 허용하지 않음)

e.g.,

```sql
select name as instructor_name, course_id
from instructor, teaches
where instructor.ID = teaches.ID;
```

<br/>

> 릴레이션의 이름을 다시 짓는 한 가지 이유는 질의의 어디에서나 더 편리하게 사용하기 위해  
> 길이가 긴 릴레이션 이름을 길이가 짧은 것으로 바꾸기 위함이다.

e.g., 질의 : “대학교 내에서 일부 과목을 가르친 적이 있는 모든 교수에 대해, 그들의 이름과 그들이 가르쳤던 과목의 과목 아이디를 찾아라”

```sql
select T.name, S.course_id
from instructor as T, teaches as S
where T.ID = S.ID;
```

<br/>

### 테이블 별칭(table alias)

> 릴레이션의 이름을 다시 짓는 또 다른 이유는 같은 릴레이션에서 튜플을 비교할 때다.

릴레이션을 자기 자신과 카티션 곱을 수행해야 하는 경우, 릴레이션의 이름을 다시 지을 수 없다면 카티션 곱에 참여하는 릴레이션의 튜플을 구분할 수 없다.

<br/>

e.g., 질의 : “적어도 생물학과 한 교수보다 급여가 많은 모든 교수의 이름을 구하라”

```sql
select distinct T.name
from instructor as T, instructor as S
where T.salary > S.salary and S.dept_name = 'Biology';
```

- 여기서 `T`와 `S`는 instructor 릴레이션에 대해 대체되는 이름으로 선언되었다. (별칭)
    - 즉, instructor 릴레이션의 복사본이 아니다.


- T, S와 같은 식별자는 릴레이션의 이름을 다시 짓기 위해 사용되며 SQL 표준에서는 `상관 이름(correlation name)`이라고 불리기도 한다.
    - 흔히 `테이블 별칭(table alias)`, `상관 변수(correlation variable)`, `튜플 변수(tuple variable)`라고도 불린다.

<br/>

앞의 질의를 더 잘 표현하자면 “생물학과에서 가장 낮은 급여를 받는 교수보다 더 많이 버는 교수의 이름을 찾아라”라고 할 수 있다.

원래 썼던 표현이 우리가 쓰는 SQL에 더 가까우나, 후자가 더 직관적이고 SQL로 직접 표현하기도 쉽다. *(3.8.2절에서 재등장)*

<br/>
<br/>
<br/>

# 3.4.2 문자열 연산


- SQL은 문자열을 `작은 따옴표(’)`를 사용하여 표기한다.
    - e.g., `‘Biology’`


- 문자열 일부로 사용된 작은 따옴표 문자는 작은 따옴표 문자 두 개를 사용하여 표기한다.
    - e.g., “It’s right”는 `‘It’’s right’`라고 표기한다.

<br/>

SQL 표준은 문자열에 대한 등호 연산에서 대소 문자를 구분한다고 명시하고 있다.

따라서 `’comp. sci.’ = ‘Comp.Sci’` 표현의 결과는 거짓이 된다.

<br/>

> 그러나 MySQL, SQL Server와 같은 **일부 DBMS는 문자열 비교를 할 때 대소문자를 구별하지 않는다.**

이들 DBMS는 `’comp. sci.’ = ‘Comp.Sci’`의 결과는 참이 된다.

데이터베이스나 특정 속성의 수준에서 이러한 기본 동작을 변경할 수 있다.

<br/>
<br/>

## 문자열에 대한 함수들

SQL은 문자열에 대한 다양한 함수를 제공한다.

- 문자열 합병 (”`||`” 사용)
- 부분 문자열 추출
- 문자열의 길이 찾기
- 문자열을 대문자 혹은 소문자로 변경하기 (`upper(s)` 또는 `lower(s)`,이때 s는 문자열)
- 문자열 끝의 공백 제거하기 (`trim(s)`)

<br/>

서로 다른 데이터베이스 시스템에서 제공되는 문자열 함수는 다양하다.

각 데이터베이스 시스템에서 어떤 문자열 함수가 지원되는지는 해당 데이터베이스 시스템의 설명서를 참조하도록 하자.

<br/>

### like 연산자

문자열에서 `like` 연산자를 사용하여 **패턴 일치**를 수행할 수 있다.

<br/>

패턴은 다음과 같은두 개의 특수문자를 사용하여 나타낼 수 있다.

> 퍼센트(`%`) : % 문자는 **어떠한 부분 문자열**과도 일치한다.

> 밑줄(`_`) : _ 문자는 **어떠한 문자**와도 일치한다.

<br/>

패턴은 대소 문자를 구분한다. (MySQL은 제외)

일부 데이터베이스는 대소문자를 구별하지 않는 like 연산의 변종을 제공한다.

<br/>

e.g.,

- `‘Intro%’`는 “Intro”로 시작하는 어떠한 문자열과도 일치한다.
- `‘%Comp%’`는 ‘Intro. to Computer Science’, ‘Computational Biology’와 같은 **“Comp”를 부분 문자열로 포함하는** 어떠한 문자열과도 일치한다.
- `‘___’`은 정확히 세 개의 문자로 이루어진 문자열과 일치한다.
- ‘`___%’`는 세 문자 이상으로 이루어진 문자열과 일치한다.

<br/>

e.g., 질의 : “건물 이름에 ‘Watson’이라는 부분 문자열을 포함하는 모든 학과 이름을 구하라”

```sql
select dept_name
from department
where building like '%Watson%';
```

<br/>

### not like 비교 연산자

SQL은 `not like` 비교 연산자를 사용하여 일치하지 않은 문자열에 대해 검색을 할 수도 있다.

<br/>

e.g., 질의 : “건물 이름에 ‘Watson’이라는 부분 문자열을 포함하지 않는 모든 학과 이름을 구하라”

```sql
select dept_name
from department
where building not like '%Watson%';
```

<br/>

### like 비교에서의 escape 키워드

SQL은 패턴에서 (`%`와 `_`와 같은) 특수 패턴 문자를 포함할 수 있도록 `이스케이프(escape) 문자`에 관한 명시를 하고 있다.

이스케이프 문자는 특수 패턴 문자가 일반 문자처럼 사용되기를 원할 대 특수 패턴 문자 바로 앞에 사용한다.

<br/>

e.g., `역슬래시(\)` 문자를 이스케이프 문자로 사용하는 패턴

- `like ‘ab\%cd%’ escape ‘\’`는 “ab%cd”로 시작하는 모든 문자열과 일치한다.
- `like ‘ab\\cd%’ escape ‘\’`는 “ab/cd”로 시작하는 모든 문자열과 일치한다.

<br/>
<br/>
<br/>

# 3.4.3 Select 절의 속성 지정

별표 “`*`”는 select 절에서 “**모든 속성**”을 가리키기 위해 사용된다.

`select *`의 형식을 갖는 select 절은 from 절에서 선택된 결과 릴레이션의 모든 속성을 가리킨다.

<br/>

e.g.,

```sql
select instructor.* # instructor의 모든 속성을 선택하도록 가리킨다.
from instructor, teaches
where instructor.ID = teaches.ID;
```

<br/>
<br/>
<br/>

# 3.4.4 튜플 출력의 순서

SQL은 릴레이션에 있는 튜플의 출력될 순서를 사용자가 제어할 수 있도록 한다.

<br/>

## order by 절

`order by` 절은 질의의 결과 튜플이 **정렬된 순서**로 나타나도록 한다.

<br/>

e.g., 질의 : “물리학과의 모든 교수를 알파벳 순서로 나열하라”

```sql
select name
from instructor
where dept_name = 'Physics'
order by name; # 기본값 : 오름차순 정렬
```

<br/>

### desc, asc

> 기본적으로, order by 절은 **오름차순**으로 항목을 나열한다.

정렬 순서를 명시하기 위해서 내림차순을 위해서 `desc`를, 오름차순을 위해서 `asc`를 명시할 수 있다.

<br/>

> 정렬은 **복수의 속성에 대해** 이루어질 수 있다.

e.g.,

- instructor 릴레이션 전체를 salary에 대해서 내림차순으로 정렬하기를 원한다.
- 만약 여러 교수가 같은 급여를 받는다면, 교수 이름에 대한 오름차순으로 이들을 정렬할 수 있다.

```sql
select *
from instructor
order by salary desc, name asc;
```

<br/>
<br/>
<br/>

# 3.4.5 Where 절의 술어

<br/>

## between 비교 연산자

e.g., 질의 : “급여가 $90,000과 $100,000 사이에 있는 교수들의 이름을 찾으시오”

```sql
select name
from instructor
where salary >= 90000 and salary <= 100000;
```

<br/>

SQL은 where 절에서 어떤 값보다는 작거나 같고 어떤 값보다는 크거나 같은 값을 명시하기 위해 `between` 비교 연산자를 제공한다.

위의 질의문 대신, between 비교를 사용해서 아래와 같은 SQL 질의를 작성할 수 있다.

```sql
select name
from instructor
where salary between 90000 and 100000;
```

<br/>

이와 유사하게 `not between` 비교 연산자도 사용할 수 있다.

<br/>
<br/>

## 행 생성자(row constructor)

SQL에서는 n개의 값 v1, v2, … , vn을 가지는 **n차 튜플**을 나타내기 위해 `(v1, v2, … , vn)`와 같은 표현이 가능하다.

이를 `행 생성자(row constructor)`라고 한다.

<br/>

비교 연산자는 튜플에서 사용될 수 있고, 순서는 사전 순서대로 정의된다.

e.g., `a1 < b1 and a2 ≤ b2`라면 `(a1, a2) ≤ (b1, b2)`는 참이다.

<br/>

유사하게, 두 튜플의 속성이 같으면 두 튜플은 같다.

따라서 아래 질의는 다음과 같이 다시 작성될 수 있다.

```sql
select name, course_id
from instructor, teaches
where instructor.ID = teaches.ID and dept_name = 'Biology';
```

```sql
select name, course_id
from instructor, teaches
where (instructor.ID, dept_name) = (teaches.ID, 'Biology');
```

(SQL-92 표준의 일부이긴 하나 몇몇 DBMS의 경우, 특히 Oracle은 이 구문을 지원하지 않음)
