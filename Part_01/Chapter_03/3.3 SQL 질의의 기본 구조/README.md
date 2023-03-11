# 3.3 SQL 질의의 기본 구조

<br/>

## SQL 표현의 기본 구조

SQL 표현의 기본 구조는 `select`, `from`, `where`의 세 개의 절로 이루어진다.

- 질의는 `from` 절에 나열된 릴레이션을 입력으로 받는다.
- `where`와 `select` 절에 명시된 대로 수행해 결과로 릴레이션을 산출한다.

<br/>

> **select** 절 : 질의의 결과에 나타나야 할 속성을 나열하는 데 사용된다.

> **from** 절 : 질의를 수행하기 위해 접근해야 하는 릴레이션을 나열한다.

> **where** 절 : from 절에 있는 릴레이션의 속성과 관련된 술어로 구성된다.

<br/>

### 일반적인 SQL 질의의 형식

```sql
select A1, A2, ... , An
from r1, r2, ... , rm
where P;
```

- `Ai` : 속성
- `ri` : 릴레이션
- `P` : 술어 (where 절이 없다면 술어 P는 true)

<br/>

> 질의는 반드시 `select, from, where` 절 순서로 작성되어야 하지만,  
> **질의가 명시한 연산을 from 절, where 절, select 절 순서대로 이해하면 가장 쉽다.**

<br/>
<br/>
<br/>

# 3.3.1 단일 릴레이션에 대한 질의

instructor 릴레이션

<p align="center"><img width="360" alt="instructor" src="https://user-images.githubusercontent.com/86337233/222119110-440a5c7b-73e9-4e89-a48a-b117200ca842.png">

<br/>
<br/>
<br/>

## select, from

e.g., 질의 : “모든 교수의 이름을 찾아라”

```sql
select name # 교수의 이름은 name 속성에 있다.
from instructor; # 교수의 이름은 instructor 릴레이션에서 발견된다.
```

질의 결과 : 표제로 **name**이라는 단일 속성만으로 구성된 릴레이션

<br/>

<p align="center"><img width="110" alt="결과1" src="https://user-images.githubusercontent.com/86337233/224474068-9d1c2b54-21e4-4e99-809e-8627f9c16958.png">

<br/>
<br/>

e.g., 질의 : “모든 교수의 소속 학과 이름을 찾아라”

```sql
select dept_name # 소속 학과 이름은 dept_name 속성에 있다.
from instructor;
```

- 한 학과에 한 명 이상의 교수가 소속될 수 있으므로 학과 이름은 instructor 릴레이션에서 한 번 이상 나타날 수 있다.
- 질의 결과 : 표제로 **dept_name**이라는 단일 속성만으로 구성된 릴레이션

<p align="center"><img width="110" alt="결과2" src="https://user-images.githubusercontent.com/86337233/224474067-b387818e-34c4-4759-93fc-0f78ebd69109.png">

<br/>
<br/>

관계형 모델의 공식적이며 수학적인 정의에서는 릴레이션은 `집합`이기 때문에 중복된 튜플은 릴레이션에서 나타날 수 없다.

**그러나 SQL은 SQL 표현의 결과에서뿐만 아니라 릴레이션에서도 중복을 허용한다.**

단, 스키마에 주 키 선언이 포함된 데이터베이스 릴레이션은 중복 튜플을 포함할 수 없다. (주 키 제약 조건의 위배되기 때문)

<br/>

따라서 2번째 질의는 instructor 릴레이션에 있는 모든 튜플에 대해 각 학과 이름을 한 번씩 나열한다.

<br/>

### distinct

중복된 튜플을 제거하고 싶을 경우, `select` 뒤에 `distinct`라는 키워드를 삽입하면 된다.

```sql
select distinct dept_name # 중복 제거
from instructor;
```

질의 결과 : 각 학과 이름은 한 번만 포함된다.

<br/>

SQL에서 중복 제거를 명시적으로 하지 않으려면 `all`이라는 키워드를 사용할 수 있는데, 기본적으로 중복이 허용되기 때문에 이는 잘 사용하지 않는다.

```sql
select all dept_name # 중복을 허용하여 모든 튜플을 나열한다.
from instructor;
```

<br/>

### 산술 표현

select 절에 상수나 튜플의 속성에 적용되는 `+`, `-`, `*`, `/` 연산자를 포함하는 산술 표현을 사용할 수 있다.

<br/>

e.g., 질의 : “각 교수의 급여를 10% 인상한 것의 결과”

```sql
select ID, name, dept_name, salary * 1.1
from instructor;
```

질의 결과 : 속성 salary 1.1에 곱하는 것을 제외하고 instructor 릴레이션과 똑같은 릴레이션을 반환한다.

<br/>

또한 SQL은 **날짜 타입과 같은 특수한 데이터 타입을** 제공하며 이러한 데이터 타입에 적용 가능한 여러 산술 함수를 허용한다. *(4.5.1절에서 다룸)*

<br/>
<br/>

## where

`where` 절은 from 절의 릴레이션에 있는 튜플 중 **where 절에 명시된 술어를 만족하는 행만 반환하게 한다.**

<br/>

e.g., 질의 : “컴퓨터 과학(Computer Science)과에서 급여가 $70,000이 넘는 모든 교수의 이름을 구하라”

```sql
select name
from instructor
where dept_name = 'Comp. Sci.' and salary > 70000;
```

질의 결과

<p align="center"><img width="90" alt="결과3" src="https://user-images.githubusercontent.com/86337233/224474066-4eb5eb00-d807-4f6b-a206-c865fef51793.png">

<br/>
<br/>

### 논리 접속사

SQL에서 where 절에 `and`, `or`, `not`과 같은 논리 접속사를 사용할 수 있다.

`>`, `≥`, `=`, `<>(≠)`를 포함하는 표현식은 논리 접속사의 피연산자가 될 수 있다.

SQL에서 날짜 타입과 같은 특수 타입뿐만 아니라 문자열이나 산술 표현의 비교를 위한 연산자도 사용할 수 있다. *(3.4.5절에서 더 다룸)*

<br/>
<br/>
<br/>

# 3.3.2 복수의 릴레이션에 관한 질의

department 릴레이션

<p align="center"><img width="280" alt="department" src="https://user-images.githubusercontent.com/86337233/224474065-e15a636e-0801-495f-bbdd-01ac0ee26d18.png">

<br/>
<br/>

e.g., 질의 : “모든 교수의 이름과 함께, 그들의 학과 이름과 건물 이름을 구하라”

- instructor 릴레이션에서 **dept_name** 속성으로부터 학과 이름을 가져올 수 있다.
- department 릴레이션에서 **building** 속성으로부터 건물 이름을 가져올 수 있다.

<br/>

이 질의에 답을 하려면,

1. SQL에 접근해야 하는 릴레이션을 `from`으로 나열하고
2. 일치 조건을 `where` 절에 지정해야 한다.

<br/>

```sql
select name, instructor.dept_name, building
from instructor, department
where instructor.dept_name = department.dept_name;
```

- `instructor.dept_name`
    - dept_name 속성은 두 릴레이션에 모두 나타나고, **참고하고자 하는 속성이 어느 릴레이션에 속해 있는지 명시적으로 나타나도록** 릴레이션 이름이 앞에 붙었다.
- name, building
    - 이 둘은 릴레이션들 중 하나에서만 나타나므로 릴레이션 이름을 앞에 붙일 필요가 없다.

<br/>

질의 결과

<p align="center"><img width="310" alt="결과4" src="https://user-images.githubusercontent.com/86337233/224474062-9a886da3-3cf6-4a63-a32b-1d18115a44cd.png">

<br/>
<br/>

> 위의 명명법을 사용하려면 from 절에 존재하는 릴레이션이 고유한 이름을 가져야 한다.

이때 같은 릴레이션의 다른 두 튜플의 정보가 결합되어야 하는 경우 문제가 생길 수 있는데,  
이는 `rename` 연산을 통해 해결할 수 있다. *(3.4.1절에서 자세히 설명함)*

<br/>
<br/>

## from 절

from 절 자체는 나열된 릴레이션의 `카티션 곱(Cartesian product, ×)`을 정의한다.

**결과 릴레이션은 from 절의 모든 릴레이션에 있는 모든 속성을 다 가진다.**

<br/>

e.g.,

teaches 릴레이션

<p align="center"><img width="360" alt="teaches" src="https://user-images.githubusercontent.com/86337233/224474060-2c290b2a-54f1-46f7-a28a-be274e7fc756.png">

<br/>
<br/>

instructor와 teaches 릴레이션의 카티션 곱의 릴레이션 스키마는 다음과 같다.

```
(instructor.ID, instructor.name, instructor.dept_name, instructor.salary,
teaches.ID, teaches.course_id, teaches.sec_id, teaches.semester, teaches.year)
```

- 이 스키마에서 instructor.ID와 teaches.ID는 서로 구분된다.
- **두 스키마 중에서 한 스키마에만 나타나는 속성에 대해서는** 간단히 표기해도 어떤 릴레이션의 속성인지 바로 알 수 있기 때문에 **릴레이션 이름이 생략된다.**

<br/>

따라서 결과 릴레이션의 스키마는 다음과 같다.

```
(instructor.ID, name, dept_name, salary,
teaches.ID, course_id, sec_id, semester, year)
```

<br/>

### Cartesian product과 where 절

카티션 곱은 스스로 서로 관계가 없는 instructor와 teaches 튜플과 결합한다.

instructor의 각 튜플은 teaches의 다른 교수를 지칭하는 튜플을 포함한 모든 튜플과 결합한다.

<br/>

<p align="center"><img width="700" alt="카티션 곱" src="https://user-images.githubusercontent.com/86337233/224474057-154083a4-bef6-4fed-ae3b-6326d7465081.png">

<br/>
<br/>

이러한 카티션 곱은 거의 잘못된 것이다.

<br/>

> `where` 절의 술어는 카티션 곱에 의해 생성되는 조합을 필요에 따라 걸러내기 위해 사용된다.

instructor와 teaches에 관련된 질의는 일반적으로  
(1) instructor의 특정 튜플 t와  
(2) t가 참조하는 같은 교수를 가리키는 teaches의 튜플과 결합할 것을 기대한다.

**즉, 같은 ID 값을 가진 teaches 튜플과 instructor 튜플이 일치하기를 기대한다.**

<br/>

다음의 SQL 질의는 이러한 조건을 보장하고, 이렇게 일치한 튜플로부터 교수 이름과 과목 식별자(ID)를 출력한다.

```sql
select name, course_id
from instructor, teaches
where instructor.ID = teaches.ID;
```

- 이 질의는 일부 과목을 가르쳤던 교수만 출력한다.
- **어떤 과목도 가르치지 않았던 교수는 출력되지 않는다.**
    - 이러한 튜플도 보고 싶다면 `외부 조인(outer join)`이라고 불리는 연산을 사용해야 한다. *(4.1.3절에서 설명)*

<br/>

> 💡 질의 작성 시 적절한 where 절 조건을 포함해야 한다.

<br/>
<br/>

## where 절 - and

e.g., 질의 : “대학교 내에서 일부 과목을 가르친 적이 있는 모든 교수에 대해, 그들의 이름과 그들이 가르쳤던 과목 아이디를 찾아라”

```sql
select name, course_id
from instructor, teaches
where instructor.ID = teaches.ID and instructor.dept_name = 'Comp. Sci';
```

- 속성 **dept_name**은 instructor 릴레이션에서만 나타나기 때문에 instructor.dept_name 대신에 dept_name만 사용해도 된다.
- 질의 결과

<p align="center"><img width="190" alt="결과5" src="https://user-images.githubusercontent.com/86337233/224474051-fd2e97ba-9d53-4df5-b664-f4aa92aa468a.png">

<br/>
<br/>
<br/>

# SQL 질의의 의미

SQL 질의의 의미는 일반적으로 다음과 같다.

1. `from` 절에 나열된 릴레이션의 **카티션 곱**을 생성하라.

2. 1번의 결과에 대해 `where` 절에 명시된 술어를 적용하라.

3. 2번의 결과의 모든 튜플에 대해 `select` 절에 명시된 속성(또는 표현의 결과)을 출력하라.

<br/>

이러한 순서는 SQL 질의의 실행 방법은 아니며, 결과가 무엇인지 명확하게 하는 데 도움이 된다.

<br/>

실제 DBMS는 이런 방식으로 SQL 질의를 수행하지 않는다.

대신, where 절의 술어를 만족하는 카티션 곱의 (결과) 튜플만 생성하여 질의 수행을 최적화할 수 있다. *(이는 15장, 16장에서 학습함)*
