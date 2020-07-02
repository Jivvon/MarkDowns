#### I/O Management

- **바이트** 단위로 정보를 주고받으면 **문자형** 장치
- 블록 단위로 정보를 주고받으면 블록형 장치



### 하드디스크

- 디스크
- 암
- 컨트롤러
  - **컨트롤 레지스터** control register : 장치 제어
  - **스테이터스 레지스터** status register
  - **버퍼** buffer : 입출력될 내용 임시 저장



### 입출력 관리 기능 구조

![스크린샷 2020-06-16 오후 4.46.15](/Users/jiwon/Library/Application Support/typora-user-images/스크린샷 2020-06-16 오후 4.46.15.png)

**디바이스 드라이버는 커널 코드가 되어야 한다.**

##### 디바이스 드라이버 인터페이스의 장점

- OS 개발자가 H/W 정보에 대해 알 필요가 없다.
- 커널 용량을 줄일 수 있다.



### 입출력 관리 방식

- **프로세스 제어 메모리 접근**
  - **폴링 (Programmed I/O)**
  - **인터럽트 기반 I/O**
- **직접 메모리 접근 Direct Memory Access (DMA)**



#### 폴링

폴 : 스테이터스 레지스터 비트 값을 체크하면서 상태를 계속 물어보는 것

모드 체인지 o 컨텍스트 스위치 x

##### 디바이스 제어

> 1. 입출력 요청이 시스템 콜로 바뀌어서 커널 I/O 서브시스템에 전달된다
> 2. 커널 I/O 서브시스템은 기본 출력장치를 알아내서 해당 디바이스 드라이버 함수를 호출한다
> 3. 프로세서가 디바이스 드라이버 코드를 실행한다

##### 출력하는 순서

> 4. 디바이스 드라이버는 busy bit가 1인지 확인 → 0으로 바뀌면 
> 5. OS는 control register의 write bit를 1로 셋팅 & 커맨드 레디 비트를 1로 셋팅
> 6. 컨트롤러는 커맨드 레디 비트가 1임을 알게되면 busy bit를 1로 셋팅하고 커맨드 확인 (control register 조회)
> 7. 데이터를 버퍼에 넣어주면 디스크가 동작을 시작한다
> 8. 끝나면 컨트롤러가 busy bit와 커맨드 레디 비트를 0으로 셋팅
> 9. 디바이스 드라이버는 busy bit가 0이 되면 커널 I/O 서브시스템으로 리턴d



#### 인터럽트 기반 I/O 방식

종료를 기다리지 않고 디바이스 드라이버에서 프로세스 관리로 넘어간다.

모드 체인지 o 컨텍스트 스위치 최소 1번



#### Direct Memory Access (DMA)

I/O 장치와 메모리 사이의 데이터 전송을 프로세서 개입 없이 수행한다. (DMA 컨트롤러)

프로세서는 데이터 전송의 시작/종료만 관여

##### DMA 컨트롤러

- Data Count : 몇 개의 바이트가 입출력
- Data Register : 데이터 일시 저장
- Address Register : 메모리 주소 알려줌
- Control Logic : 작업 지시 - I/O가 끝나면 인터럽트를 걸어 OS에게 알림

##### 입력 요청, Address Register 20000, Data Count 1024를 OS가 DMA 컨트롤러에게 주었다.

> 1. 데이터 한 바이트를 읽어 데이터 레지스터에 넣었다가
> 2. bus를 경유해 메인 메모리 20000번지에 이 데이터를 저장하고
> 3. 데이터 카운트 1 감소, 어드레스 레지스터 1 증가
> 4. 1~3 반복하면 마지막 1024번째 데이터는 21023번지에 저장된다.



### 커널 I/O 서브시스템

##### 목표

1. 입출력 때문에 성능이 저하되지 않도록. **커널의 효율성 증대** : 타임 쉐어링, 멀티 프로그래밍, 스와핑
2. 여러 입출력 장치를 제어하는 방법의 **일반성** : 여러 장치의 공통적인 기능 담당

##### 하는 일

1. 장치를 예약하고 할당

2. 디바이스 스케줄링

3. 에러 핸들링 : I/O 다시시작 or 어플리케이션에 오류 발생 알림

4. 버퍼링 : 입출력할 내용을 메모리에 일시 저장

   1. 장치의 처리 속도와 컴퓨터 실행 속도차이를 보정하기 위해
   2. 트랜스퍼 사이즈 미스매치 해결을 위해
   3. 카피 시멘틱을 위해 (복사하여 내용 유지)

5. 캐싱 : 최근에 엑세스된 내용 임시 저장 (저속 장치에 대한 성능 향상 위해)

6. 스풀링

   ​	주변 장치와 컴퓨터 간에 데이터를 전송할 때 처리 지연을 단축하기 위해 **보조기억장치에 파일 형태로 출력 내용을 저장**



### 커널 코드 실행하는 방법 (커널 엔트리 포인트)

1. 인터럽트 : 비동기적 이벤트 발생을 디바이스가 OS에게 알리는 방법

   ​	**PIC**(Programmable Interrupt Controller)를 통해 CPU와 장치 연결

   ​	인터럽트 핸들러는 장치 번호에 해당하는 **IDT**(Interrupt Descriptor Table) 테이블의 엔트리로 간다.

   ​	인터럽트 핸들러는 급한건 ISR에서 직접 실행하고, 아닌건 유저모드로 모드체인지할 때 해준다.

   ​	OS는 클럭 인터럽트가 걸릴 때마다 커널에 들어가 몇 번 걸렸는지 카운트한다.

2. 트랩 : 동기적 이벤트 발생을 디바이스가 OS에게 알리는 방법

   ​	트랩은 유저 프로세스를 실행하다가 발생한다. (프로세서가 스스로 인터럽트)

   ​	page fault만 제외하고 프로세스 종료 (page fault는 메모리 처리)

   - divided by zero
   - invalid machine code
   - page fault
   - segmentation fault
   - protection fault

3. 시스템 콜

   ​	시스템 콜 테이블에는 커널 함수의 주소가 있다.

시스템콜과 인터럽트의 차이 : **시스템 콜은 테이블 엔트리를 검사하는 과정이 반복적으로 두 번 일어난다**



## 파일관리

### 파일 시스템

1. 부트 블록
2. 슈퍼블록
3. 아이노드 리스트
4. 데이터 블록들 영역
   1. **데이터 블록 구성 (파일 얼로케이션)**

##### 파일 데이터 블록들 구성하는 방법

1. Contiguous Allocation
2. Chained Allocation
   1. 베드섹터 에러
3. Indexed Allocation

### 디스크 전체 빈 블록 관리

1. Counting
2. LinkedList
3. Grouping (유닉스)
   1. 슈퍼블록(슈퍼블록은 여러 개의 블록으로 구성됨) 내에 블록 하나를 인덱스 블록으로 사용하자 -> 4096 / 4 = 1024개 저장가능
   2. 1024개가 넘으면 데이터 블록의 TOP 블록을 임시 인덱스 블록으로 사용한다
4. Bit vector(Bit map, 리눅스)



### 유닉스 파일 시스템

1. 부트 블록
2. 슈퍼블록
3. 아이노드 리스트
   1. 아이노드 구성
      1. 링크 카운트
      2. **파일 어드레스**
4. 데이터 블록들 영역



##### 파일 저장 방법 in 유닉스

1. 파일 시작 부분의 첫 10개 블록 번호는 아이노드가 직접 저장. (다이렉트 블록 번호)
2. 10개가 넘어가면 인덱스 블록 사용 (싱글 인다이렉트 블록)
3. 1034(1024 + 10)개가 넘어가면 더블 인다이렉트 필드 사용
4. 약 1MB가 넘어가면 트리플 인다이렉트 필드

`유닉스가 최대로 저장할 수 있는건 40KB + 4MB + 4GB + 4TB`

##### 파일 저장 방법 in 리눅스

**버추얼 파일 시스템 (VFS)**

- ext 파일 시스템 (UFS 확장)



## Disk Scheduling

### 디스크 구조

![스크린샷 2020-06-16 오후 7.08.13](/Users/jiwon/Library/Application Support/typora-user-images/스크린샷 2020-06-16 오후 7.08.13.png)

디스크 디바이스 드라이버는 OS에게 블록번호를 받아 물리 주소(surface, track, #sector)로 엑세스한다.

디스크 성능을 좌우하는 것은 Seek time이다. (Seek time이 rotational delay나 data transfer time보다 길기 때문)

### 알고리즘

- FIFO (First In First Out)
- SSTF (Shortest Seek Time First)
  - Throughput ↑ , Starvation 발생 가능. 
- SCAN : head 진행 방향에서 가까운 순서대로.
  - 마지막 실린더 도착 후 반대로 진행
  - Throughput ↑ generally.
- Look (엘레베이터)
  - SCAN에서 마지막 실린더까지 가지 않고 반대로 진행



## RAID

여러 개의 물리 디스크를 하나의 논리 디스크로 사용

#### RAID 0

- → 방향으로 저장. (비트 인터리브드(bit interleaved))
- 한 디스크에서 장애시, 데이터 손실 발생

#### RAID 1

- RAID 0이 2 세트 (복사)

#### RAID 3

- RAID 0 + 패리티 비트(parity bit)
- 비트 인터리브드(bit interleaved) + 패리티 비트(parity bit)
- 한 디스크에 장애 발생시, 패리티 정보를 이용하여 복구 가능하다.

#### RAID 4

- RAID 3 - 비트 인터리브드(bit interleaved)
- 정통적인 방법으로 한 디스크에 b0b1b2b3를 연속해서 저장한다.

#### RAID 5

- RAID 4에서 패리티 값들을 모은 블록들을 여러 디스크에 흩어서 저장

#### RAID 6

- RAID 5에서 패리티 비트 2개

#### RAID 01, 10

- RAID 01 : RAID 0 두 개를 RAID 1로 구성
- RAID 10 : RAID 1 두 개를 RAID 0로 구성



## 디스크 캐시 (버퍼 캐시)

디스크 캐시는 메모리에 할당한 저장 공간이다. (하드보다 빠르잖아)

4KB 크기의 블록들을 여러 개 저장해야 한다. (OS 입장에서는 블록 단위로 주고받으니까)

캐시가 가득차면 블록을 내보내야하는데, 이를 **캐시 교체**라고 한다.

#### 캐시 교체 알고리즘

- LRU (Latest Recently Used). generally
- LFU (Latest Frequently Used)





## 가상메모리

#### Page Table

모든 프로세스는 각자 페이지 테이블이 있음

\#Page는 저장 안 됨. (순서대로)

- Virtual Address : #Page(20bits) + Offset(12bits)

- Page Table Entry : P + M + R + U + W + COW + #Page Frame(p', 20bits)

#### TLB

TLB 목적은 #Page → #Frame 이므로 Control bit는 저장하지 않는다.

Page Table과 달리 제한된 공간이므로 #Page 필요

##### Page 교체 정책

- FIFO (RR 스타일로 페이지 제거됨)
- 최적
- LRU (Least Recently Used) : 지역성
- Clock Policy (second chance algorithm) : use bit
- Enhanced Clock Policy : page fault 횟수는 Clock Policy와 같지만 성능이 더 좋다. (use bit + modified bit)

#### 쓰레싱

##### 원인 

- 전체 메모리 크기가 지역성 크기의 합보다 작거나, working set 크기보다 작으면 일어난다.

##### 해결방법

- swap-out

