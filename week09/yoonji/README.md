# 5.1 캐시, 어디에나 존재하는 것

현대 컴퓨터 시스템에서 CPU와 메모리 간의 속도 차이를 해결하기 위한 핵심 요소는 캐시.



### 나무통 원리

메모리는 CPU보다 속도가 현저히 느림  
- CPU는 빠르게 명령어와 데이터를 처리하지만, 메모리는 그에 비해 약 **100분의 1 속도**  
- CPU와 메모리 간 병목 현상을 줄이기 위해 **중간 저장소인 캐시**를 사용


### 캐시 계층 구조

최신 CPU에는 **여러 계층의 캐시(L1, L2, L3)**가 존재하며, 각각 다음과 같은 특징을 가짐:
- CPU는 명령어 또는 데이터를 불러올 때 항상 캐시를 먼저 조회
- 캐시에 없을 경우, 더 낮은 계층 또는 메인 메모리에서 검색


### 캐시-메모리 불일치 문제

캐시와 메모리 간 데이터가 불일치할 수 있음  
이를 해결하기 위한 방법:

1. 연속 기입 (Write-Through)
- **캐시와 메모리에 동시에 데이터 기록**  
- **단점**: 메모리 쓰기 지연

2. 후기 기입 (Write-Back)
- **캐시에만 먼저 기록하고 나중에 메모리에 반영**  
- **단점**: 동기화가 복잡

## 다중 캐시의 일관성 (Cache Coherency)

여러 CPU 코어가 각자 캐시를 가질 경우, **한 캐시에서 갱신된 데이터가 다른 캐시에도 반영**되어야 함  
→ 서로 다른 값을 참조하게 되어 **오류 발생**

### 메모리와 디스크의 관계 변화

최근 운영체제는 메모리를 디스크의 캐시처럼 활용함  
→ 속도 향상을 위해 자주 쓰는 데이터를 메모리에 저장

하지만 캐시가 생기면 갱신 동기화 문제가 발생해 데이터 유실 가능  
→ 이를 방지하기 위해 대부분의 입출력 라이브러리는 동기화(sync), 캐시 비우기(flush) 함수 제공

요즘은 메모리 가격 하락으로 **RAM이 디스크를 일부 대체**.
→ 다만, RAM은 전원이 꺼지면 데이터가 사라지므로 디스크를 완전히 대체할 수는 없음

## 가상 메모리와 디스크

모든 프로세스가 요청하는 메모리 크기는 물리 메모리보다 클 수 있음
→ 운영체제는 디스크 일부를 메모리처럼 사용해 이를 해결 (가상 메모리)

개발자는 실제 메모리 용량을 신경 쓰지 않고 프로그래밍 가능  
→ 운영체제가 뒤에서 자동으로 처리

## 분산 파일 시스템과 캐시

사용자 장치는 **원격 분산 파일 시스템을 직접 마운트**할 수 있음  
→ 전송된 파일은 **로컬 디스크에 저장**, 로컬 디스크는 **원격 파일 시스템의 캐시** 역할 수행

Kafka와 같은 대용량 메시지 큐도  
→ **원격 저장소 접근 전, 중간 캐시 계층**으로 간주 가능

