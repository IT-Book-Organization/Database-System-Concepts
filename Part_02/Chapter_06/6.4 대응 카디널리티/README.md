# 6.4 대응 카디널리티

<br/>

> `대응 카디널리티(mapping cardinality)` 혹은 `카디널리티 비율(cardinality ratio)`은  
> 관계 집합을 통해 **다른 개체와 관련될 수 있는 개체의 수**를 나타낸다.

<br/>

개체 집합 A와 B 사이의 이진 관계 집합 R에서 대응 카디널리티는 다음 중 하나여야 한다.

- 일대일
- 일대다
- 다대일
- 다대다

<br/>

어떤 관계 집합의 적절한 대응 카디널리티는 그 관계 집합이 모델링하는 실세계의 상황을 따라야 한다.

<br/>

### 일대일(One-to-one)

A의 한 개체는 B의 **최대** 하나의 개체와 연관을 가지고, B의 한 개체는 A의 **최대** 하나의 개체와 연관을 가진다.

<p align="center"><img width="300" alt="일대일" src="https://user-images.githubusercontent.com/86337233/224700107-d7cd5237-b770-430c-a879-946065c4d4b5.png">

<br/>
<br/>

### 일대다(One-to-many)

A의 한 개체는 임의의 수(0 또는 그 이상)의 B의 개체와 연관을 가진다.

그러나 B의 한 개체는 A의 **최대** 하나의 개체하고만 연관을 갖는다.

<p align="center"><img width="300" alt="일대다" src="https://user-images.githubusercontent.com/86337233/224700099-d33565af-0160-47d7-93a7-e40de96a9411.png">

<br/>
<br/>

### 다대일(Many-to-one)

A의 한 개체는 B의 최대 한 개체와 연관을 갖는다.

그러나 B의 한 개체는 A의 임의의 수(0 또는 그 이상)의 개체와 연관을 갖는다.

<p align="center"><img width="300" alt="다대일" src="https://user-images.githubusercontent.com/86337233/224700111-7b946594-3d94-4e63-8077-3d1bb473aa78.png">

<br/>
<br/>

### 다대다(Many-to-many)

A의 한 개체는 임의의 수(0 또는 그 이상)의 B의 개체와 연관을 갖고  
B의 한 개체도 임의의 수(0 또는 그 이상)의 A의 개체와 연관을 갖는다.

<p align="center"><img width="300" alt="다대다" src="https://user-images.githubusercontent.com/86337233/224700115-42d24b94-7b19-4a88-9422-296b947f0ee7.png">

<br/>
<br/>
<br/>

## 관계 집합의 대응 카디널리티 표현

E-R 다이어그램 표기법에서 관계 집합과 연관된 개체 집합 사이에 방향을 가진 `화살표(→)`나 `직선(-)`을 이용하여 관계에 대한 카디널리티 제약을 표시한다.

화살표는 관계의 “일” 쪽으로 그린다.

<br/>

<p align="center"><img width="500" alt="관계 집합의 대응 카디널리티 표현" src="https://user-images.githubusercontent.com/86337233/224700122-af6578c8-8830-4f14-8c52-0368a730b03e.png">
