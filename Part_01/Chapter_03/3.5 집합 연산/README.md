# 3.5 집합 연산

<br/>

SQL 연산 `union`, `intersect`, `except`는 릴레이션에 대해 각각 관계 대수 연산 `⋃`, `⋂`, `-`와 같은 연산을 수행한다.

<br/>

e.g.,

1. 2017년 가을에 가르쳤던 모든 과목의 집합

- 질의문

    ```sql
    select course_id
    from section
    where semester = 'Fall' and year == 2017;
    ```

- 질의 결과 : c1 릴레이션

<p align="center"><img width="100" alt="결과1" src="https://user-images.githubusercontent.com/86337233/224483834-6d0a58a7-8bd9-454b-b044-2954a3d73adf.png">

<br/>
<br/>

2. 2018년 봄에 가르쳤던 모든 과목의 집합

- 질의문

    ```sql
    select course_id
    from section
    where semester = 'Spring' and year == 2018;
    ```

- 질의 결과 : c2 릴레이션

<p align="center"><img width="100" alt="결과2" src="https://user-images.githubusercontent.com/86337233/224483837-874f319e-acba-4446-896c-be0a501fef23.png">

<br/>
<br/>
<br/>

## 3.5.1 합집합 연산

질의 : “2017년 가을과 2018년 봄에 연속해서 강의가 되었던 **모든 과목의 집합**”

```sql
(select course_id
 from section
 where semester = 'Fall' and year == 2017)
union
(select course_id
 from section
 where semester = 'Spring' and year == 2018);
```

<br/>

질의 결과 : `c1 union c2`

<p align="center"><img width="100" alt="결과3" src="https://user-images.githubusercontent.com/86337233/224483838-86e4727f-7622-4b1a-9166-97ba5d311199.png">

<br/>
<br/>

> `union` 연산은 select 절과는 다르게 **자동으로 중복을 제거한다.**

<br/>

> 만약 모든 중복을 보유하고 싶다면 union 대신 `union all`을 써야 한다.

```sql
(select course_id
 from section
 where semester = 'Fall' and year == 2017)
union all
(select course_id
 from section
 where semester = 'Spring' and year == 2018);
```

위 결과의 중복 튜플 개수는 c1과 c2 둘 다에서 나타나는 중복된 튜플의 총 개수와 같다.

<br/>
<br/>

## 3.5.2 교집합 연산

질의 : “2017년 가을과 2018년 봄에 **모두 가르친** 모든 과목의 집합”

```sql
(select course_id
 from section
 where semester = 'Fall' and year == 2017)
intersect
(select course_id
 from section
 where semester = 'Spring' and year == 2018);
```

<br/>

질의 결과 : `c1 intersect c2`

<p align="center"><img width="100" alt="결과4" src="https://user-images.githubusercontent.com/86337233/224483839-4d08d356-2a90-41ad-aafd-79754d102594.png">

<br/>
<br/>

> `intersect`는 자동으로 중복을 제거한다.

MySQL은 intersect 연산을 구현하지 않는다. 대신 한 가지 해결책은 하위 질의를 사용하는 것이다. *(3.8.1절에서 논의)*

<br/>

> 만약 모든 중복을 보유하고 싶다면 intersect 대신 `intersect all`이라고 써야 한다.

```sql
(select course_id
 from section
 where semester = 'Fall' and year == 2017)
intersect all
(select course_id
 from section
 where semester = 'Spring' and year == 2018);
```

위 결과의 중복 튜플의 개수는 c1과 c2 둘 다에서 나타나는 중복 튜플 개수의 최솟값과 같다.

<br/>
<br/>

## 3.5.3 차집합 연산

질의 : “2017년 가을 학기**에는 있지만** 2018년 봄 학기**에는 없는** 모든 과목”

```sql
(select course_id
 from section
 where semester = 'Fall' and year == 2017)
except
(select course_id
 from section
 where semester = 'Spring' and year == 2018);
```

<br/>

질의 결과 : `c1 except c2`

<p align="center"><img width="100" alt="결과5" src="https://user-images.githubusercontent.com/86337233/224483840-6350a805-dad8-48b3-a338-05115936db75.png">

<br/>
<br/>

> `except` 연산은 첫 번째 입력 릴레이션의 모든 튜플 중에서 두 번째 입력 릴레이션에 없는 모든 튜플을 출력한다.

- 즉, 차집합 연산을 수행한다.
- **차집합 연산을 수행하기 전에 자동으로 입력의 중복이 제거된다.**

<br/>

- 일부 DBMS, 특히 Oracle은 except 대신 키워드 `minus`를 사용하는 반면, Oracle 12c는 except all 대신 키워트 `multiset except`를 사용한다.
- MySQL은 except 연산을 구현하지 않으며, 한 가지 해결책은 하위 질의를 사용하는 것이다. *(3.8.1절에서 논의)*

<br/>

> 만약 모든 중복을 보유하고 싶다면 except 대신에 `except all`이라고 써야 한다.

```sql
(select course_id
 from section
 where semester = 'Fall' and year == 2017)
except all
(select course_id
 from section
 where semester = 'Spring' and year == 2018);
```

위 결과의 중복 튜플 개수는 c1에서 중복된 개수에서 c2에서 중복된 개수를 뺀 것에서 그 차가 양수일 때와 같다.
