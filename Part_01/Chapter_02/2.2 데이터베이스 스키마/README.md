# 2.2 데이터베이스 스키마

<br/>

데이터베이스에 대해서 언급할 때, 아래 두 개념을 잘 구별해야 한다.

- `데이터베이스 스키마(database schema)` : 데이터베이스의 논리적 설계


- `데이터베이스 인스턴스(database instance)` : 어느 한순간에 데이터베이스에 저장되어 있는 데이터의 스냅샷(snapshot)

<br/>

## 릴레이션 스키마(relation schema)

이는 속성과 그 속성이 가지는 도메인의 명세로 구성된다.

<br/>

> 릴레이션의 개념 ≈ 프로그래밍 언어에서 변수의 개념

> 릴레이션 스키마의 개념 ≈ 프로그래밍 언어의 타입 정의(type definition)

<br/>

## 릴레이션 인스턴스(relation instance)

> 릴레이션 인스턴스의 개념 ≈ 프로그래밍 언어에서 변수의 값

<br/>

릴레이션 인스턴스의 튜플도 릴레이션이 변경됨에 따라 변하게 된다.

**하지만 일반적으로 릴레이션의 스키마는 변하지 않는다.**

<br/>
<br/>

## e.g.,

### instructor 릴레이션

<p align="center"><img width="360" alt="instructor" src="https://user-images.githubusercontent.com/86337233/222119110-440a5c7b-73e9-4e89-a48a-b117200ca842.png">

<br/>
<br/>

### department 릴레이션

<p align="center"><img width="290" alt="department" src="https://user-images.githubusercontent.com/86337233/222122536-a6fbe085-6c18-481f-a3f0-dd8b267a3ab6.png">

<br/>
<br/>

department 릴레이션의 스키마는 다음과 같다.

```
department(dept_name, building, budget)
```

<br/>
<br/>

`dept_name` 속성은 위 두 스키마에 모두 나타나는 것을 볼 수 있다.

릴레이션 스키마에서 공통적인 속성을 사용하는 것은, **서로 다른 릴레이션에 있는 튜플을 연관시키는 방법** 중 하나다.

