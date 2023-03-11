# 3.7 집계 함수

<br/>

## 집계 함수(aggregate function)

- 입력 : 값의 모음(collection, 즉 집합 혹은 다중 집합)
- 결과 : 단일 값을 반환

<br/>

SQL은 다섯 개의 표준 내장 집계 함수를 제공한다.

(대부분의 DBMS는 부가적으로 많은 집계 함수를 제공함)

- 평균 : `avg`
- 최솟값 : `min`
- 최댓값 : `max`
- 총합 : `sum`
- 개수 : `count`

<br/>

`sum`과 `avg`의 입력은 숫자의 모음이어야 하지만, 다른 연산자는 문자열과 같이 숫자가 아닌 데이터 타입의 모음에서도 동작한다.

<br/>
<br/>
<br/>

# 3.7.1 기본 집계

## 평균(avg)

e.g., 질의 : “컴퓨터 과학과 교수들의 평균 급여를 구하라”

```sql
select avg(salary)
from instructor
where dept_name = 'Comp. Sci.';
```

<br/>

### as

DBMS는 집계로 인해 생성된 결과 릴레이션의 속성에 임의의 이름을 부여할 수 있다.

하지만 속성에 의미 있는 이름을 부여하기 위해서는 다음과 같이 `as` 절을 사용할 수 있다.

```sql
select avg(salary) as avg_salary
from instructor
where dept_name = 'Comp. Sci.';
```

<br/>

### distinct

집계 함수를 구하기 전에 중복을 제거해야 하는 경우도 있는데, 이때 키워드 `distinct`를 집계 함수 표현에 사용하면 된다.

<br/>

e.g., 질의 : “2018년 봄 학기에 어떤 과목을 가르치는 교수의 수를 구하라”

```sql
select count(distinct ID) # 교수가 하나 이상의 과목을 가르치더라도 결과에는 한 번만 포함된다.
from teaches
where semester = 'Spring' and year = 2018;
```

<br/>
<br/>

## 개수(count)

릴레이션에서 몇 개의 튜플이 있는지 알아보기 위해 집계 함수 `count`를 자주 사용한다.

SQL에서 이 함수의 표기는 `count(*)`이다.

<br/>

e.g., 질의 : “course 릴레이션에서 튜플의 개수를 구하라”

```sql
select count(*)
from course;
```

<br/>

### distinct과 all

> SQL은 <b>count(*)에서</b> distinct를 사용하는 것을 금하고 있다.

<br/>

max와 min에서는 distinct를 사용하는 것이 가능하지만, 그 결과는 변하지 않는다.

중복 보유를 명시하기 위해 distinct 대신 키워드 all을 사용할 수 있지만, **all은 기본값이기 때문에** 그렇게까지 할 필요는 없다.

<br/>
<br/>
<br/>

# 3.7.2 그룹단위 집계


튜플들의 단일 집합이 아니라 **복수의 튜플 집합에 대해 집계 함수를 적용하고 싶을 때**가 있는데, 이때 `group by` 절을 사용할 수 있다.

- group by 절에 주어진 속성(들)은 그룹을 형성하기 위해 사용된다.
- **group by 절에서 모든 속성이 같은 값을 가지는 튜플들은 하나의 그룹으로 묶인다.**

<br/>

> e.g., 질의 : “각 학과의 평균 급여를 구하라”

```sql
select dept_name, avg(salary) as avg_salary
from instructor
group by dept_name;
```

1. dept_name 속성으로 그룹화한 instructor 릴레이션의 튜플

<p align="center"><img width="360" alt="결과1" src="https://user-images.githubusercontent.com/86337233/224487323-3da9e9d4-6a36-4051-a158-db9ddd886bb7.png">

<br/>
<br/>

2. 각 그룹에 대해 집계 연산을 수행하여 얻은 결과

<p align="center"><img width="220" alt="결과2" src="https://user-images.githubusercontent.com/86337233/224487327-4ccc5f78-8f34-4109-8278-8ec7f83ecabe.png">

<br/>
<br/>

> e.g., 질의 : “모든 교수의 평균 급여를 구하라” 

```sql
select avg(salary)
from instructor;
```

**이 경우에 group by 절은 제외되므로 전체 릴레이션을 하나의 그룹으로 간주한다.**

<br/>

> e.g., 질의 : “2018년 봄 학기에 각 학과에서 어떤 과목을 가르친 교수의 수를 구하라”

- teaches 릴레이션에는 어떤 교수가 어떤 과목 분반을 어느 학기에 했는지에 대한 정보가 존재한다.
- 이 정보는 각 교수가 소속된 학과명을 알아내기 위해 instructor 릴레이션과 `join`되어야 한다.

```sql
select dept_name, count(distinct ID) as instr_count
from instructor, teaches
where instructor.ID = teaches.ID
  and semester = 'Spring'
  and year = 2018
group by dept_name;
```

<p align="center"><img width="220" alt="결과3" src="https://user-images.githubusercontent.com/86337233/224487328-5a4480e3-20a0-4252-92f0-e89639606224.png">

<br/>
<br/>

---

<br/>

SQL 질의가 그룹화를 사용할 때, 집계되지 않고 select 절에 나타나는 속성은 오직 group by 절에 존재하는 속성이다.

> **즉, group by 절에 없는 속성은 집계 함수에 대한 인자로만 select 절 안에 나타날 수 있다.**

그렇지 않다면 오류로 간주한다.

<br/>

예를 들어, 다음 질의는 ID가 group by 절에 나타나지 않았고, select 절에서 집계되지 않은 상태로 나타나기 때문에 잘못된 질의다.

```sql
# erroneous query
select dept_name, ID, avg(salary)
from instructor
group by dept_name;
```

<br/>

위 질의에서

- dept_name에 따라 정의된 특정 그룹의 각 교수는 각기 다른 ID를 가질 수 있고,
- 각 그룹에 대해 하나의 튜플만 출력되기 때문에

출력해야 할 ID 값을 선택하는 특별한 방법은 없다.

<br/>

결과적으로 이러한 경우는 SQL에서 허용되지 않는다.

<br/>
<br/>
<br/>

# 3.7.3 Having 절

튜플보다 그룹에 대한 조건을 적용하는 것이 때로는 더 유용할 수 있다.

<br/>

예를 들어, `교수들의 평균 급여가 $42,000을 넘는 학과`에만 관심이 있다고 하자.

이 조건은 하나의 튜플에만 적용될 수 없고 group by 절이 만든 각 그룹에 적용된다.

<br/>

이러한 질의를 표현하기 위해 SQL은 `having` 절을 사용한다.

SQL은 **그룹이 형성된 다음에 having 절의 술어를 적용**하기 때문에 집계 함수가 사용될 수 있다.

```sql
select dept_name, avg(salary) as avg_salary
from instructor
group by dept_name
having avg(salary) > 42000;
```

<p align="center"><img width="220" alt="결과4" src="https://user-images.githubusercontent.com/86337233/224487329-d84e793b-5b23-4512-9d2e-7394ae740b96.png">

<br/>
<br/>

> select 절에서의 경우와 같이 `having` 절에 집계 함수와 함께 나타나지 않은 속성들은 반드시 `group by` 절에 나타나야 한다.

그렇지 않다면 잘못된 질의다.

<br/>

### 집계 함수, group by, having

`집계 함수`, `group by`, `having` 절을 포함하는 질의의 의미는 다음과 같은 연산의 순서로 정의된다.

<br/>

1. 집계가 없는 질의의 경우와 마찬가지로, `from` 절은 릴레이션을 얻기 위해 먼저 수행된다.


2. `where` 절이 존재하면, where 절의 술어는 from 절의 결과 릴레이션에 적용된다.


3. where 절의 술어를 만족하는 튜플은
    - `group by` 절이 존재하면 group by 절에 의해 그룹화된다.
    - group by 절이 없다면 where 절의 술어를 만족하는 전체 튜플의 집합은 하나의 그룹으로 간주된다.


4. `having` 절이 존재한다면 각각의 그룹에 적용된다.
    - having 절의 술어를 만족하지 못하는 그룹은 제거된다.


5. `select` 절은 남아 있는 그룹을 사용하여 질의의 결과 튜플을 생성하는데, 이때 그룹별로 집계 연산을 적용하여 단일 결과 항을 만든다.

<br/>

e.g., 질의 : “2017년에 개설한 각 과목 분반에 대해서, 그 분반에 적어도 두 명의 학생이 있으면 해당 분반에 등록한 학생들의 평균과 전체 학점(tot_cred)을 구하라”

```sql
select course_id, semester, year, sec_id, avg(tot_cred)
from student, takes
where student.ID = takes.ID and year = 2017
group by course_id, semester, year, sec_id
having count(ID) >= 2;
```

<br/>
<br/>
<br/>

# 3.7.4 널 값과 불리언 값의 집계

### 널 값의 집계

널 값이 존재할 때 널 값은 집계 함수의 처리를 복잡하게 만든다.

<br/>

예를 들어, instructor 릴레이션의 몇몇 튜플에 salary에 대해 널 값을 가진다고 가정하여 모든 급여의 총합을 구하는 다음 질의를 고려해보자.

```sql
select sum(salary)
from instructor;
```

- 위 질의에서 합해지는 값은 일부 튜플이 salary에 대해 널 값을 가지고 있으므로 널 값을 포함한다.
- 전체 합이 null이라고 말하기보다는, SQL 표준은 `sum 연산은 입력의 null 값을 무시한다`고 말한다.

<br/>

> <b>count(*)를 제외한 모든 집계 함수는 입력 값에서 널을 무시한다.</b>

- 널 값을 무시한 결과로, 값의 모음은 비어 있을 수도 있다.
- **빈 모음에 대한** `count`**는 0**으로 정의되고, 다른 모든 집계 연산은 빈 모음이 적용되었을 때 널 값을 반환한다.

<br/>

### 불리언 값의 집계

**Boolean 데이터 타입**은 `true`, `false`, `unknown` 값을 가질 수 있다.

`some`과 `every`와 같은 집계 함수는 불리언 값의 모음에 적용될 수 있고,  
그 값들의 이접(disjunction)(`or`)과 연접(conjunction)(`and`)을 각각 계산할 수 있다.
