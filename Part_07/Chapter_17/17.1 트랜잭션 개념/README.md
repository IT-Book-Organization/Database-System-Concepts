# 17.1 트랜잭션 개념

<br/>

### 트랜잭션, transaction

> 다양한 데이터 항목에 접근하고 갱신하는 프로그램 수행의 **단위**

- 한 트랜잭션은 `begin transaction`과 `end transaction` 사이에서 실행되는 연산으로 구성되어 있다.
- 보통 한 트랜잭션은 고급 데이터 조작 언어(주로 SQL)나 JDBC나 ODBC를 사용해  
  데이터베이스에 접근하는 프로그래밍 언어로 작성된 사용자 프로그램으로 수행된다.

<br/>
<br/>

## ACID property

### 원자성, atomicity

> 트랜잭션의 모든 연산이 정상적으로 수행 완료되거나, 어떠한 연산도 수행되지 않은 원래 상태가 되도록 해야 한다.

> Either all operations of the transaction are properly reflected in the database or none are.

- `all-or-none` : 전부 아니면 전무
- 트랜잭션은 나눌 수 없기 때문에, 전부 실행되거나 전부 실행되지 않아야 한다.

<br/>

### 일관성, consistency

> 고립 상태(= 동시에 수행되는 트랜잭션이 없는 상태)에서
> 트랜잭션 수행이 데이터베이스의 일관성을 보존해야 한다.

> Atomic execution of a transaction in isolation preserves the consistency of the database.

- 만약 한 트랜잭션이 일관된 상태의 데이터베이스에서 원자적이고 고립된 상태로 실행되었다면  
  데이터베이스는 **트랜잭션이 종료된 후에도 일관된 상태여야 한다.**
- 데이터 무결성 제약 조건(주 키 제약 조건, 참조 키 제약 조건 등) 뿐만 아니라, SQL을 사용해 기술할 수 없는 응용 프로그램 자체의 일관성 조건을 보장해야 한다. → 이런 제약 조건을 지키는 것은 트랜잭션을
  만드는 프로그래머의 책임이다.

<br/>

### 고립성, isolation

> 여러 트랜잭션이 동시에 수행되더라도, 각 트랜잭션은 시스템에서 다른 트랜잭션이 동시에 수행되고 있는지를 알지 못하는 것과 같아야 한다.

> Although multiple transactions may execute concurrently, each transaction must be unaware of other concurrently
> executing transactions. Intermediate transaction results must be hidden from other concurrently executed transactions.

- 모든 트랜잭션 Ti와 Tj의 쌍에 대해 / 데이터베이스 시스템은 Ti에게 Ti가 시작되기 전에 Tj가 수행을 끝마쳤거나,  
  아니면 Ti가 수행을 끝마친 후에 Tj가 수행을 시작하는 것과 같이 되도록 보장해야 한다.
- 데이터베이스 시스템은 **동시에 수행되는 다른 데이터베이스 명령어의 영향을 받지 않고** 트랜잭션이 올바르게 수행할 수 있도록 특별한 조치를 해야 한다.
- 고립성을 유지하는 것은 데이터베이스 시스템의 성능에 부정적인 영향을 미치기 때문에 몇몇 응용 프로그램은 고립성을 지키지 않기도 한다.

<br/>

### 지속성, durability

> 트랜잭션이 성공적으로 수행 완료되고 나면, 트랜잭션에 의해 변경된 데이터베이스 내용은 시스템에 오류가 발생한다고 하더라도 **영구적으로 반영되어야 한다.**

> After a transaction completes successfully, the changes it has made to the database persistent, even if there are
> system failures.
