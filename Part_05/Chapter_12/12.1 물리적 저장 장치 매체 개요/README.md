# 12.1 물리적 저장 장치 매체 개요

저장 장치 매체는 `데이터에 접근하는 속도`, 해당 매체를 구매하기 위해 드는 `데이터 단위당 비용`, 그리고 `매체의 신뢰성`에 따라 분류할 수 있다.

- 캐시, Cache
- 메인 메모리, Main memory
- 플래시 메모리, Flash memory
- 자기 디스크 저장 장치, Magnetic-disk storage
- 광학 저장 장치, Optical storage
- 테이프 저장 장치, Tape storage

<br/>
<br/>

## 캐시, Cache

- 저장 장치 중에서 가장 빠르고 비싸다.
- 캐시 메모리는 크기가 상대적으로 작다.
- 컴퓨터 시스템 하드웨어가 캐시의 사용을 관리한다.
- 데이터베이스 시스템에서 캐시 저장 장치를 어떻게 관리하는지에 대해서는 신경을 쓸 필요는 없으나,  
  데이터베이스 시스템의 질의 처리 데이터 구조와 알고리즘을 설계하는 데 캐시 효과를 고려하는 것은 의미가 있다.

<br/>
<br/>

## 메인 메모리, Main memory

- 연산에 사용할 데이터를 저장하기 위한 저장 장치이다.
- 일반적으로, 매우 큰 데이터베이스에 대해서 그 전체를 저장하기에 메인 메모리는 너무 작거나 비싸지만,  
  많은 기업용 데이터베이스의 경우 메인 메모리에 모두 적재가 가능하다.
- `휘발성(volatile)` : 메인 메모리의 내용은 정전이나 시스템 충돌이 발생하면 잃어버리게 된다.

<br/>
<br/>

## 플래시 메모리, Flash memory

- `비휘발성(non-volatile)` : 메인 메모리와 달리 정전이 발생해도 데이터가 남아있다.
- 메인 메모리에 비교해 바이트당 비용이 더 낮지만, 자기 디스크보다 바이트당 비용은 더 비싸다.

<br/>

### SSD

`솔리드 스테이트 드라이브(solid-state drive, SSD)`는 내부적으로 플래시 메모리를 사용하여 데이터를 저장하지만,  
자기 디스크와 유사한 인터페이스를 제공하여 **데이터를 블록 단위로 저장하거나 검색할 수 있다.**

→ `블록 지향 인터페이스(block-oriented interface)`

<br/>
<br/>

## 자기 디스크 저장 장치, Magnetic-disk storage

- 온라인으로 장기간 데이터를 저장하기 위한 주된 매체이다.
- `하드 디스크 드라이브(hard disk drive, HDD)`라고도 한다.
- `비휘발성(non-volatile)`이다.

<br/>

> 자기 디스크에 저장된 데이터에 접근하려면 시스템은 먼저 데이터에 접근할 수 있는 디스크에서 메인 메모리로 데이터를 이동시켜야 한다.  
> 그 다음, 시스템이 지정한 작업을 수행한 후 수정된 데이터를 디스크에 기록해야 한다.

<br/>
<br/>

## 광학 저장 장치, Optical storage

- `DVD(Digital Video Disk)`는 레이저 광원을 사용하여 데이터를 쓰고 다시 읽는 광학 저장 매체다.
- 이는 특정 데이터에 접근하는 데 필요한 시간이 자기 디스크에 걸리는 시간에 비교해 상당히 길 수 있으므로,  
  활성 중인 데이터베이스 데이터를 저장하는 데 적합하지 않다.
- 일부 DVD 버전은 생산된 공장에서만 기록되는 읽기 전용(read-only)이며,  
  다른 버전은 단 한 번 기록(write-once)을 지원하기 때문에 한 번만 기록할 수 있고 덮어쓸 수 없다. 일부 다른 버전에서는 여러 번 재기록할 수 있다.

> 단 한 번 기록되어 여러 번 읽기 가능한 디스크를 WORM(write-once, read-many) 디스크라 한다.

<br/>
<br/>

## 테이프 저장 장치, Tape storage

- 주로 데이터 백업과 보관용으로 테이프 저장 장치를 사용한다.
- 자기 테이프는 디스크보다 저렴하며 수년 동안 데이터를 안전하게 저장할 수 있다.
- `순차 접근 저장 장치(sequential-access storgae)` → 테이프 시작부터 순차적으로 접근해야 하므로 데이터 접근이 훨씬 더 느리다.

> 반대로, 자기 디스크와 SSD 저장 장치는 **데이터가 어디에 위치하든지 데이터를 읽을 수 있으므로** 직접 접근 저장 장치(direct-access storage)라고 불린다.

<br/>
<br/>

## 저장 장치 계층

다양한 저장 매체는 `속도`와 `비용`에 따라서 계층 구조로 나타낼 수 있다.

이 둘 사이에는 합리적인 상반관계(trade-off)가 존재한다.

- 계층의 높은 단계로 갈수록 비싸지만 빠르다.
- 계층의 낮은 단계로 갈수록 비트당 비용은 감소하고, 접근 시간은 증가한다.

<br/>

<p align="center"><img width="560" alt="storage hierarchy" src="https://github.com/IT-Book-Organization/Database-System-Concepts/assets/86337233/9c1d7380-551c-4c0a-826d-6a9933cf5fa5">

<br/>
<br/>

다양한 저장 매체의 속도와 비용에 덧붙여서, 저장 장치의 휘발성에 대한 이슈가 있다.

메인 메모리를 기준으로 위쪽의 저장 장치는 휘발성 저장 장치이고, 플래시 메모리부터 아래쪽은 비휘발성 저장 장치이다.

> 안전한 보관을 위해 비휘발성 저장 장치에 데이터를 기록해야 한다.

<br/>

### 1차 저장 장치, primary storage

- 캐시
- 메인 메모리

<br/>

### 2차 저장 장치, secondary storage

`온라인 저장 장치(online storage)`라고도 한다.

- 플래시 메모리
- 자기 디스크

<br/>

### 3차 저장 장치, tertiary storage

`오프라인 저장 장치(offline storage)`라고도 한다.

- 자기 테이프
- 광디스크 주크박스
