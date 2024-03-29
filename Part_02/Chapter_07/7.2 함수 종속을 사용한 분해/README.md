# 7.2 함수 종속을 사용한 분해

<br/>

데이터베이스는 실제 세계의 개체(entity)와 관계(relationship)의 집합을 모델링하며, 일반적으로 실제 세계의 데이터에는 <b>다양한 제약 조건(규칙)</b>이 있다.

e.g.,

- 각각의 학생과 교수는 하나의 이름만 가진다.
- 각각의 학과는 하나의 예산 값을 가지며, 하나의 건물에만 연결되어 있다.

<br/>

모든 실제 세계의 제약 조건을 만족하는 릴레이션의 인스턴스를 `적법한(legal) 릴레이션`이라고 한다.

모든 릴레이션 인스턴스가 적법한 인스턴스일 때 데이터베이스에 대한 `적법한 인스턴스(legal instance)`가 된다.

<br/>
<br/>
<br/>

# 7.2.1 표기 관습

- 속성의 집합으로 그리스 문자(a.g., α)를 쓰며, 릴레이션 스키마를 참조하기 위해 로마 대문자를 쓴다.


- 릴레이션 이름은 소문자로 표기한다.


- `스키마 R`가 `릴레이션 r`를 위한 것임을 표기하기 위해 `r(R)` 표기법을 사용한다.
    - r(R) 표기법을 사용하면 릴레이션과 스키마 둘 다 참조한다.
    - 릴레이션 스키마는 속성의 집합이지만, 모든 속성의 집합이 스키마인 것은 아니다.


- 릴레이션은 어떤 주어진 시간에 특정한 값, 즉 인스턴스를 가질 수 있다. → ‘`r의 인스턴스다`’


- 속성의 집합이 수퍼 키일 때 그것을 K로 나타낸다. → ‘`K는 r(R)의 수퍼 키다`’

<br/>
<br/>
<br/>

# 7.2.2 키와 함수 종속

실제 세계 제약 조건 중에서 가장 널리 쓰이는 유형은 `키(수퍼 키, 후보 키, 주 키)` 또는 `함수 종속`으로 표현할 수 있다.

<br/>
<br/>

## 수퍼 키

2.3절에서는 수퍼 키에 대하여, **릴레이션에서 튜플을 유일하게 식별하게 해 주는 하나 혹은 그 이상의 속성의 전체적인 집합**으로 정의했다.

<br/>

주어진 r(R)에 대해 R의 부분집합 K는 다음의 조건을 만족하면 r(R)의 `수퍼 키(superkey)`다.

조건 : 만약 r(R)의 어떠한 적법한 인스턴스에서, r의 인스턴스에 존재하는 모든 튜플의 쌍 t1과 t2에 대해 <b>t1 ≠ t2이면 t1[K] ≠ t2[K]</b>가 성립해야 한다.

> 즉, 어떠한 r(R)의 적법한 인스턴스에 존재하는 서로 다른 두 개의 튜플은 **속성 집합 K에서 동일한 값을 가질 수 없다**는 뜻이다.

→ K개의 값이 r에서 튜플을 유일하게 식별한다.

<br/>
<br/>

## 함수 종속

수퍼 키는 전체 튜플을 유일하게 식별하는 속성의 집합인 반면, `함수 종속`은 **어떤 속성들의 값을 유일하게 식별하는 제약 조건**을 표현한다.

<br/>

릴레이션 스키마 r(R), α ⊆ R, β ⊆ R

> 주어진 r(R)의 인스턴스에 대해,  
> 인스턴스 내의 모든 튜플의 쌍 t1과 t2가 ‘**t1[α] = t2[β]이면, t1[α] = t2[β]이다**’라는 조건을 만족한다면  
> 인스턴스는 `함수 종속 α → β`를 만족한다고 한다.

> 만약 모든 적법한 r(R)의 인스턴스가 함수 종속을 만족한다면, 스키마 r(R) 위에서 `함수 종속 α → β를 보존(성립, 유지, 만족)한다`고 한다.

<br/>

- r(R)에서 함수 종속 K → R이 성립한다면 K는 r(R)의 수퍼 키이다.
- 즉, r(R)의 모든 적법한 인스턴스에 대해  
  인스턴스 내의 임의의 튜플 t1과 t2의 모든 쌍이 t1[K] = t2[K]를 만족하면 언제나 t1[R] = t2[R]이기 때문에(즉, t1 = t2), K는 수퍼 키다.

<br/>

우리는 두 가지 방법으로 함수 종속을 사용한다.

1. 릴레이션의 인스턴스가 주어진 함수 종속의 집합 F를 만족하는지 `검사`하기 위해 사용한다.
2. 적법한 릴레이션의 집합에 대한 제약 조건을 명시하기 위해 사용한다.
    - 우리는 주어진 함수 종속의 집합을 만족하는 릴레이션 인스턴스에만 관심을 기울인다.
    - 함수 종속 집합 F를 만족하는 스키마 r(R)의 릴레이션만으로 제한한다면,
      F는 r(R)를 `보존`(성립, 유지, 만족)한다고 말한다.

<br/>

### e.g.,

<p align="center"><img width="200" alt="릴레이션 r" src="https://user-images.githubusercontent.com/86337233/226095746-3088769b-f525-43cf-9788-3e8c78771c47.png">

<br/>

1. A → C가 만족된다.
    - A의 값이 a1인 두 개의 튜플은 동일한 C의 값, c1을 가진다.
    - A의 값이 a2인 두 개의 튜플은 동일한 C의 값, c2를 가진다.


2. 함수 종속 C → A는 성립되지 않는다.
    - 튜플 t1(a2, b3, c2, d3)과 t2(a3, b3, c2, d4)
    - t1[C] = t2[C]이지만 t1[A] ≠ t2[A]이기 때문이다.

<br/>

### trivial

> **α → β** 형태의 함수 종속은 **β ⊆ α**이면 `자명하다(trivial)`고 한다.

e.g.,

A → A : A라는 속성을 지니는 모든 릴레이션이 만족한다.

즉, 임의의 튜플 t1과 t2로 구성한 모든 쌍에서 ’t1[A] = t2[A]이라면, t1[A] = t2[A]임을 만족해야 한다’는 것이며 이는 당연히 성립하는 수식이다.

<br/>

---

<br/>

> 어떤 특정 릴레이션의 스키마가 만족하지 않는 함수 종속을 특정 시점의 릴레이션의 인스턴스가 보존할 수도 있다.

e.g.,

classroom 릴레이션에서 `room_number → capacity`가 만족한다.

- 하지만 실세계에서는 다른 건물에 있는 두 강의실이 동일한 강의실 번호를 가지지만 수용인원은 다를 수 있다.
    - 따라서 언젠가 room_number → capacity가 만족되지 않는 인스턴스가 나타날 수 있기 때문에  
      room_number → capacity는 classroom 릴레이션 스키마가 `보존`하는 함수 종속 집합에 포함되지 않는다.
- 그러나 `building, room_number → capacity`는 classroom 스키마에서 성립함을 예상할 수 있다.

<br/>

### 함수 종속의 추론

> 주어진 함수 종속의 집합 F가 릴레이션 r(R)에 대해 성립한다고 하면,  
> 다른 어떤 함수 종속 또한 그 릴레이션에 대해 성립한다는 것을 **추론**해 낼 수 있다.

e.g.,

주어진 스키마 r(A, B, C)에서 만약 함수 종속 A → B, B → C가 r에 대해 성립한다면 함수 종속 A → C 또한 r에 대해 성립함을 추론할 수 있다.

A의 어떤 값에 대응하는 B의 값은 오직 하나이고, B의 그 값에 대해서도 C에 대응하는 값은 오직 하나이기 때문이다.

<br/>

### 폐포(closure)

함수 종속 집합 F의 `폐포(closure)`는 **주어진 집합 F로부터 추론할 수 있는 모든 함수 종속의 집합**을 의미한다.

이는 `F+`라고 표현하며, F+는 F의 모든 함수 종속을 기본적으로 포함한다.
