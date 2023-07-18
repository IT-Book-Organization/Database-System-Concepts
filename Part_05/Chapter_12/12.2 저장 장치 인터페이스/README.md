# 12.2 저장 장치 인터페이스

<br/>

### SATA, SCSI(SAS)

- 디스크는 일반적으로 `직렬 ATA(SATA)` 인터페이스 또는 `직렬 부착 SCSI(Serial Attached SCSI, SAS)` 인터페이스를 지원한다.
    - SATA 3 : 6 gigabits/sec
    - SAS Version 3 : 12 gigabits/sec
- SAS 인터페이스는 일반적으로 서버에서만 사용된다.

<br/>

### NVMe

- `비휘발성 메모리 익스프레스(Non-volatile Memory Express, NVMe)` 인터페이스는 SSD를 더 잘 지원하기 위해 개발한 논리 인터페이스 표준이다.
- 일반적으로 `PCIe` 인터페이스와 함께 사용된다.
    - PCIe 인터페이스 : 컴퓨터 시스템 내부에서 고속 데이터 전송을 제공한다. (lower latency & higher transfer rates)
- 24 gigabits/sec

<br/>

### SAN

> 일반적으로 케이블을 통해 컴퓨터 시스템의 디스크 인터페이스에 디스크를 직접 연결하지만,  
> 고속 네트워크를 통해 **원격**에 위치한 컴퓨터에 연결할 수도 있다.

- `SAN(storage area network)` 구조에서는 많은 수의 디스크가 **고속 네트워크를 통해** 여러 서버 컴퓨터에 연결된다.
- 일반적으로 `독립적인 디스크의 중복적인 배열(redundant arrays of independent disks, RAID)`이라고 불리는 **저장 장치 조직 기법**을 사용해  
  디스크를 지역적으로 조작한다.
- SAN에 사용되는 상호 연결 기술들
    - `iSCSI` : SCSI 명령을 IP 네트워크를 통해 전송할 수 있다.
    - `FiberChannel`, `FC` : 버전에 따라 초당 1.6~12기가바이트의 전송 속도를 지원한다.
    - `InfiniBand` : 매우 낮은 대기 시간의 고대역폭 네트워크 통신을 제공한다.

<br/>

### NAS

- SAN의 대안이다.
- `NAS(network attached storage)`는 큰 디스크처럼 보이는 네트워크 저장 장치 대신,  
  NFS 또는 CIFS와 같은 **네트워크 파일 시스템 규약**을 사용하여 파일 시스템 인터페이스를 제공한다.

<br/>

### Cloud storage

- `클라우드 저장 장치(cloud storage)` : 클라우드에 데이터를 저장하고 API를 통해 접근한다. (21.7절에서 다룸)
- 데이터가 데이터베이스와 같은 위치에 있지 않은 경우, 수십에서 수백 밀리초의 매우 긴 지연 시간을 가지므로,  
  데이터베이스의 기본 저장 장치로는 그리 이상적이지 못하다.
- 응용 프로그램의 경우 객체를 저장하기 위해 종종 클라우드 저장 장치를 사용한다.
