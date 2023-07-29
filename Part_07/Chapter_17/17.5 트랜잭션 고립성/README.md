# 17.5 트랜잭션 고립성

<br/>

## 트랜잭션 동시성 허용의 이점

트랜잭션 처리 시스템은 보통 **여러 트랜잭션이 동시에 수행되는 것**을 허용하나, 이는 데이터의 일관성과 관련된 여러 가지 복잡한 문제를 발생시킨다.

동시에 여러 트랜잭션의 동시성과 일관성을 모두 보장하기 위해서는 추가적인 노력이 필요하다.

트랜잭션이 한 번에 하나씩 `순차적으로(serially)` 실행되도록 하면 문제는 간단해지지만, 동시성을 허용함으로써 다음 두 가지 이점을 얻을 수 있다.

<br/>

### 처리율과 자원 이용률 향상

CPU와 디스크는 서로 병렬적으로 동작할 수 있기 때문에, I/O 작업은 CPI 처리가 필요한 작업과 병렬적으로 처리될 수 있다.

이러한 CPU와 I/O 시스템의 병렬성은 여러 트랜잭션을 동시에 실행할 수 있도록 해주어,  
결국 시스템의 `처리율(throughput)`, 즉 **주어진 시간에 처리되는 트랜잭션의 수**를 높인다.

마찬가지로 프로세서와 디스크가 작업을 처리하지 않고 정지해 있는 휴식 시간을 줄일 수 있어, 시스템의 `이용률(utilization)` 또한 증가한다.

<br/>

### 대기 시간 감소

트랜잭션의 실행 시간은 각각 다른데, 만약 트랜잭션이 순차적으로 실행된다면  
**짧은 트랜잭션이 긴 트랜잭션이 끝날 때까지 오랜 시간을 기다리고 있어야 하는** 상황이 발생할 수 있다.

이는 트랜잭션 수행의 지연을 초래하게 된다.

따라서 동시 수행은 트랜잭션의 예기치 않은 지연을 줄일 수 있으며,  
`평균 응답 시간(average response time)`, 즉 **트랜잭션이 요청된 후에 완료될 때까지 걸리는 평균 시간**도 줄일 수 있다.

<br/>

> 데이터베이스에서 동시 수행을 적용하게 된 동기  
> == 운영체제에서 `다중 프로그래밍(multi-programming)`을 사용하게 된 동기

<br/>
<br/>

## 동시성 제어 기법, Concurrency control schemes

> mechanisms to achieve **isolation**

> 여러 트랜잭션이 동시에 수행될 때 `고립성`이 보장되지 않으면,  
> 개별 트랜잭션이 정상적으로 처리되고 있더라도 데이터베이스 `일관성`은 깨질 수 있다.

- 데이터베이스 시스템은 데이터베이스의 일관성을 유지하기 위해 트랜잭션 간의 상호작용을 반드시 제어해야 한다.
- 이는 동시성 제어 기법이라고 하는 다양한 기법으로 처리할 수 있다. (18장에서 설명)

<br/>
<br/>

## 스케줄, schedule

> specify **the order** in which instructions of concurrent transactions are executed  
> 스케줄은 실행 중인 트랜잭션들이 **어떤 순서에 따라** 실행되는지를 보여준다.

일련의 트랜잭션 스케줄은 반드시 그 트랜잭션의 모든 명령어를 포함하고 있어야 하며,  
명령어는 개별 트랜잭션의 명령어 순서를 따라야 한다.

<br/>

- A transaction that **succesfully complete**s its execution will have a `commit instruction` as the last statement
- A transaction that **fails to complete** its execution will have an `abort instruction` as the last statement

<br/>

### e.g., 은행 시스템 예시

`트랜잭션 T1` : 계좌 A에서 계좌 B로 $50을 이체한다.

```
T1 : read(A);
		 A := A - 50;
		 write(A);
		 read(B);
		 B := B + 50;
		 write(B).
```

<br/>

`트랜잭션 T2` : 계좌 A의 잔액의 10%를 계좌 B로 이체한다.

```
T2 : read(A);
		 temp := A * 0.1;
		 A := A - temp;
		 write(A);
		 read(B);
		 B := B + temp;
		 write(B).
```

<br/>

#### Serial Schedule 1

> a serial schedule in which T1 is followed by T2

<p align="center"><img width="490" alt="Serial Schedule 1" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/c99b4f0a-fdef-4c02-b0c6-68cb135c880c">

<br/>
<br/>

#### Serial Schedule 2

> a serial schedule in which T2 is followed by T1

<p align="center"><img width="300" alt="Serial Schedule 2" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/88cb0bb1-89df-4379-a080-6cc83e3d37a9">

<br/>
<br/>

#### Serializable Schedule 3

> the following schedule **is not a serial schedule**, but it is `equivalent` to Schedule 1

<p align="center"><img width="300" alt="Serializable Schedule 3" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/4cc8b34c-dc8a-451b-b751-73a4d5e4dbd5">

<br/>
<br/>

#### Schedule 4

> the following schedule does not preserve the value of (A+B)

<p align="center"><img width="830" alt="Schedule 4" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/2320c32e-0b78-4df4-bb55-e386ba1f1246">

→ `비일관성(inconsistent) 상태`

<br/>
<br/>
<br/>

## 직렬성, Serializability

동시 수행한 스케줄의 결과가 **트랜잭션을 하나씩 순차적으로 수행하는 스케줄의 실행 결과와 동일**하게 함으로써 데이터베이스의 일관성을 보장할 수 있다.

이런 스케줄을 `직렬 가능(serializable) 스케줄`이라고 한다.

<br/>

> **serial execution** of a set of transactions preserves database consistency  
> → a schedule is `serializable` if it is equivalent to a serial schedule

*→ 6절에서 이어서 설명*
