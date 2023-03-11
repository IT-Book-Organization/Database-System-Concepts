# 3.6 널 값

<br/>

산술 연산, 비교 연산, 집합 연산을 포함한 관계 연산을 수행할 때 `널 값(null value)`이 있으면 특별한 처리가 필요하다.

<br/>
<br/>

## 산술 연산에서의 null

> `산술 연산식(+, -, *, /)`의 결과는 **입력 값이 널이면 널**이다.

<br/>

예를 들어,

- `r.A + 5`의 표현이 있고,
- `r.A`가 특정 튜플에 대해 널일 때

→ 그 튜플에 대한 연산식의 결과는 반드시 널이어야 한다.

<br/>
<br/>

## 비교 연산에서의 null

`1 < null`의 비교를 고려해보자.

**널 값이 무엇을 나타내는지 모르기 때문에** 이 비교 값의 결과가 참이라고 하는 것은 옳지 않을 수 있고,  
결과가 거짓이라고 주장하는 것도 옳지 않다.

<br/>

> 따라서 SQL은 널 값을 포함하는 비교의 결과를 `unknown`으로 처리한다.

unknown은 true와 false 외에 추가된 제3의 논리 값을 나타낸다.

<br/>

### and, or, not

where 절의 술어는 비교의 결과에 대한 `and`, `or`, `not`과 같은 `불리언(Boolean) 연산`을 포함하기 때문에,  
다음과 같이 불리언 연산의 정의는 unknown 값을 다룰 수 있도록 확장되어야 한다.

<br/>

> and

- `true and unknown`의 결과는 unknown
- `false and unknown`의 결과는 false
- `unknown and unknown`의 결과는 unknown

<br/>

> or

- `true or unknown`의 결과는 true
- `false or unknown`의 결과는 unknown
- `unknown or unknown`의 결과는 unknown

<br/>

> not

- `not unknown`의 결과는 unknown

<br/>

---

<br/>

**r.A가 널이라면**, “`1 < r.A`”뿐만 아니라 “`not(1 < r.A)`”도 unknown이 됨을 알 수 있다.

<br/>

> **where 절의 술어가 어떤 튜플에 대해 false나 unknown으로 판명되면 그 튜플은 결과에 포함하지 않는다.**

<br/>
<br/>

## null, is not null 술어

SQL은 널 값을 테스트하는 술어 안에 특수한 키워드 `null`을 사용한다.

`is not null` 술어는 적용된 값이 널이 아닐 때 참이다.

<br/>

e.g., 질의 : “instructor 릴레이션에서 salary의 값이 널 값인 모든 교수”

```sql
select name
from instructor
where salary is null;
```

<br/>
<br/>

## select distinct 절과 집합 연산에서의 null

질의가 `select distinct` 절을 사용할 때, **중복된 튜플은 반드시 제거되어야 한다.**

<br/>

이러한 목적으로, 두 튜플의 상응하는 속성값을 비교할 때 값이 둘 다 널이 아닌 같은 값을 가지거나 **둘 다 널일 때 값이 같은 것으로 간주한다.**

<br/>

예를 들어, `{(’A’, null), (’A’, null)}`과 같이 어떤 속성이 널 값을 가지더라도 두 튜플은 같은 것으로 간주한다.

따라서 `distinct` 절을 사용하면 값이 같은 튜플 중 하나만 유지하게 된다.

<br/>

위에서 보인 널 값의 처리는 “`null = null`”의 비교 결과가 unknown이 아니라 **true**를 반환하는 것으로,  
술어에서 일반적으로 널 값을 처리하는 방식과는 다르다.

<br/>

> **일부 값이 널이라도 모든 속성값이 같으면 튜플을 같은 것으로 보는 방법**은 합집합, 교집합, 차집합 연산에서도 사용된다.
