# 14.3 B+-트리 인덱스 파일

<br/>

### 인덱스 순차 파일의 단점

- 파일이 커질수록, 인덱스를 찾아서 그 데이터를 연속으로 스캔하는 성능이 감소한다.
- 성능 감소는 파일을 재구성함으로써 제거될 수 있지만, 파일 재구성에 드는 비용은 크다.

<br/>

## B+-tree index

> `균형 트리(balanced tree)` 형태이다.  
> 즉, 트리의 `루트(root)`에서 `단말 노드(leaf node)`까지 **모든 경로의 길이가 같다.**

> B+-tree의 “B” : balanced  
> → 이 속성으로 인해 검색, 삽입, 삭제의 성능을 좋게 한다.

<br/>

- root 노드를 제외한 비단말 노드는 **⎡n/2⎤~ n** 사이의 자식(children)을 가지고 있다.  
  즉, 50% 이상이 차있어야 하는 것이다. (n은 특정한 트리에 대해 고정된 값)
- leaf 노드는 **⎡(n-1)/2⎤~ n-1** 사이의 개수만큼 value를 가진다.
- special cases
    - root 노드는 2 ~ n개 사이의 자식을 갖는다.
    - root 노드가 leaf 노드라면(= 트리에 root 노드만이 존재한다면) 0 ~ (n-1) 사이의 개수만큼 value를 가진다.

<br/>

<p align="center"><img width="1000" alt="B+-tree index" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/c4dfb432-d84d-439f-8795-46c7336a7684">

<br/>
<br/>

이 구조는 삽입과 삭제 시 성능 부담과 공간 부담을 준다.

하지만 이 성능 부담은 파일의 변경이 많은 경우에 파일 재구성 비용을 피할 수 있기에 수용될 수 있다.

<br/>
<br/>

## 14.3.1 B+-트리의 구조

- B+-트리 인덱스는 다계층 인덱스이다.
- 중복 검색 키 값이 없다고 가정하자. 즉, 각각의 `검색 키(search key)`는 고유하다.

<br/>

- n-1개의 검색 키 값 K
- n개의 포인터 P

<p align="center"><img width="550" alt="node" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/c4cf55b8-0c72-448b-ac2f-c7ccab07a5ff">

<br/>

<p align="center"><img width="400" alt="pointers" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/9be418ba-0d24-4162-afe1-b52ff90c1db0">

<br/>
<br/>

> 노드 안의 검색 키 값은 **정렬된 순서**로 유지된다.  
> → K1 < K2 < K3 < … < Kn-1

<br/>

### 단말 노드(leaf node)

- 포인터 Pi는 검색 키 값 Ki를 가지는 파일 레코드를 가리킨다.
- 적어도 ⎡(n-1)/2⎤개의 value를 포함해야 한다.

> leaf 노드를 **검색 키 순서로 서로 연결하기 위해서** 포인터 Pn을 사용한다.  
> → 파일의 연속적인 처리에 대한 성능 ↑

<br/>

e.g., n = 4

<p align="center"><img width="630" alt="leaf nodes" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/8b4da33c-9a94-4335-94c7-a0a91c1c815c">

<br/>
<br/>

### 비단말 노드(non-leaf node)

- `내부 노드(internal node)`라고도 부른다.
- leaf 노드 상에서 `다계층 (희소) 인덱스(multi-level sparse index)`를 형성한다.
- 모든 포인터가 **트리 노드에 대한 포인터**인 것을 제외하고는 leaf 노드의 구조와 동일하다.
- ⎡n/2⎤ ~ n개 사이의 개수만큼 포인터를 가질 수 있다. (한 노드의 포인터 수 : 그 노드의 `팬아웃(fanout)`)

<br/>

<p align="center"><img width="550" alt="non-leaf nodes" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/029b382b-c0db-4d95-b336-a46321050fa2">

<br/>
<br/>

---

<br/>

m(≤ n)개의 포인터를 포함하는 노드

- i = 2, 3, … , m-1일 때,  
  포인터 Pi는 **Ki 보다는 작고, Ki-1보다는 크거나 같은** 검색 키 값을 포함하는 서브 트리(subtree)를 가리킨다.
- 포인터 Pm은 Km-1보다 크거나 같은 키 값을 포함하는 서브 트리를 가리킨다.
- 포인터 P1은 K1보다 작은 검색 키 값을 포함하는 서브 트리를 가리킨다.

→ 아래 예시를 같이 보면 쉽게 이해할 수 있을 것!

<br/>

e.g., B+-tree for instructor file (n = 6)

<p align="center"><img width="950" alt="b+-tree for instructor file" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/e5494ef0-277b-43a8-b58c-d9ab1f9edb87">

<br/>
<br/>
<br/>

## 14.3.2 B+-트리에서 질의

- `Point queries` : find(v)
- `Range queries` : findRange(lb, ub)

<br/>

<p align="center"><img width="1000" alt="find" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/7a09c6db-c3e2-497e-8719-8e4869650ca7">

<br/>
<br/>

### 질의 비용

- root에서 일부 leaf 노드까지 트리의 경로를 탐색해야 한다.
- 파일에 레코드 N개가 있다면, 그 경로는 ⎡log⎡n/2⎤(N)⎤보다 더 길지 않다.
- 따라서 읽어야 하는 디스크 블록의 개수가 상당히 절감된다.

> B+-트리는 각 노드가 크고(일반적으로 디스크 블록 크기), 각 노드는 많은 수의 포인터를 가질 수 있다.  
> → 굵고 짧은 형태 (↔︎ 이진트리 : 가늘고 긴 형태)

<br/>

- leaf 노드로 이동한 후, 고유한 검색 키의 단일값에 대한 질의의 경우 일치하는 레코드를 가져오기 위해 하나 이상의 임의 I/O가 필요하다.
- 범위 질의(range query)는 leaf 노드까지 순회한 후, **주어진 범위 내에 있는 모든 포인터를 검사**하는 추가 비용이 발생한다.

<br/>
<br/>

## 14.3.3 B+-트리 갱신

### 14.3.3.1 삽입

<p align="center"><img width="1100" alt="insert" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/526d428e-ecd0-4af2-a106-a6c76f2d5486">

<br/>
<br/>

“Lamport” 삽입 결과

<p align="center"><img width="1200" alt="insert result" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/e2e2ec1f-6f23-4c7e-ae31-40bc483d68ee">

<br/>
<br/>
