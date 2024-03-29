# 17.4 트랜잭션 원자성과 지속성

<br/>

## Transaction State

### 동작, Active

초기 상태로, 트랜잭션이 실행 중인 상태

<br/>

### 부분 커밋, Partially committed

마지막 명령문이 실행된 후의 상태

<br/>

### 실패, Failed

정상적인 실행이 더 진행될 수 없을 때 가지는 상태

<br/>

### 중단, Aborted

트랜잭션이 `롤백(rollback)`되고, 데이터베이스가 트랜잭션의 시작 전 상태로 환원되고 난 후의 상태

<br/>

> 중단된 트랜잭션에 의해 수행된 갱신 내용이 무효가 될 때 **트랜잭션이 롤백(rollback)되었다**고 한다.  
> 이 부분은 트랜잭션의 중단을 관리하는 복구 기법이 책임지며, 트랜잭션의 중단과 롤백은 주로 `로그(log)`를 유지하는 방법을 통해 이루어진다.

- 로그 : 갱신을 수행한 트랜잭션의 식별자, 수정하는 데이터 항목의 식별자, 데이터 항목의 이전 값과 새로운 값(수정 값)을 기록한다.
- **로그를 기록한 후에야 비로소 데이터베이스에 수정 내용이 반영된다.**

<br/>

### 커밋, Committed

트랜잭션이 성공적으로 완료된 후의 상태

<br/>

> 성공적으로 실행을 완료한 트랜잭션은 **커밋(commit)되었다**고 한다.

갱신을 수행한 트랜잭션이 커밋되었다면 데이터베이스는 그 트랜잭션에 의해 새로운 일관된 상태로 변경된 것이다.

<br/>
<br/>

## 트랜잭션의 상태 다이어그램

<p align="center"><img width="470" alt="트랜잭션의 상태 다이어그램" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/1c3374bf-beaf-4538-8e2b-34f3290e594f">

> 트랜잭션이 커밋(committed)되었거나 중단(aborted)되었다면 그 트랜잭션은 `종료(terminated)`되었다고 할 수 있다.

<br/>

1. 트랜잭션은 `동작(active) 상태`에서 시작한다.
2. 트랜잭션이 마지막 명령문을 실행하고 나면 `부분 커밋(partially committed) 상태`로 들어가게 된다.
    - 트랜잭션은 자신의 모든 실행을 완료했지만, 여전히 중단될 가능성을 가진다.
    - 실행 결과가 아직 메인 메모리에 존재하는 상황이기 때문에,  
      데이터베이스 시스템은 만약 실패가 발생했더라도 시스템이 재시작되었을 때 갱신 내용이 다시 수행될 수 있도록 충분한 정보를 디스크에 저장한다.
3. **복구에 필요한 정보가 모두 저장되었을 때** 트랜잭션은 `커밋(committed) 상태`로 들어가게 된다.

<br/>

---

<br/>

만약 트랜잭션이 더는 정상적인 실행을 진행할 수 없다고 판단된다면 트랜잭션은 `실패(failed) 상태`로 들어가게 된다.

이러한 트랜잭션은 반드시 롤백되어야 하며, 그 다음 트랜잭션은 `중단(aborted) 상태`로 들어간다.

<br/>

이 시점에서 시스템은 두 가지 선택 사항을 가진다.

1. **하드웨어 오류** 또는 트랜잭션 자체의 논리적 오류에 의하지 않은 **소프트웨어 오류**로 인해 중단되었을 경우
    - 트랜잭션을 재시작할 수 있다.
    - `재시작(restart)`된 트랜잭션은 새로운 트랜잭션으로 간주한다.
2. 응용 프로그램을 직접 수정해야 하는 논리적 오류가 있거나 입력이 잘못된 경우, 또는 필요한 데이터가 데이터베이스에 없는 경우
    - 트랜잭션을 `강제 종료(kill)`시킬 수 있다.
