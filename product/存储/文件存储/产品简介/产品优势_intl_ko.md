### CFS 특징

#### 통합 관리

- CFS는 표준 POSIX와 파일 시스템 접근 구문(예를 들어 데이터 강일치성과 파일 잠금)을 제공합니다. Tencent Cloud CVM 인스턴스는 표준 작업 시스템을 사용하여 명령을 탑재할 수 있으며 NFS v3.0/v4.0 프로토콜을 통해 CFS를 탑재할 수 있습니다.
- CFS는 콘솔 인터페이스를 제공하기 때문에 파일 시스템을 빠르고 간편하게 생성 및 구성할 수 있어 복잡한 파일 시스템 배포와 유지보수 시스템 작업을 줄일 수 있습니다.

#### 자동 확장

- CFS는 파일 용량 크기에 따라 자동으로 파일 시스템 저장 용량을 확장하고, 동시에 요청과 응용프로그램 작업을 중단하지 않기 때문에 필요한 저장 리소스를 독점적으로 사용할 뿐만 아니라 번거로운 관리 작업을 줄일 수 있습니다.


#### 안전성과 신뢰성

- CFS는 가용성과 신뢰성이 높으며 모든 CFS 인스턴스는 가용 영역에서 여러 개의 중복 사본을 갖고 있습니다.
- CFS는 POSIX 권한을 통해 파일 시스템 접근을 엄격하게 제어할 수 있습니다. 기본 네트워크 또는 VPC 네트워크를 사용할 때 권한 그룹에 매칭하여 접근 권한 제어를 실현합니다.

#### 저렴한 비용

- CFS는 동적으로 요구 용량을 조정할 수 있기 때문에 사전에 스토리지를 조정할 필요가 없습니다. 사용량에 따라 비용을 지불하기만 하면 기본 비용 또는 배포, 후기 운영 관리 비용은 발생하지 않습니다.
- 여러 개의 CVM은 NFS 프로토콜을 통해 하나의 저장 공간을 공유할 수 있기 때문에 다른 저장 서비스를 중복 구매하거나 캐시를 고려할 필요가 없습니다.


### CFS와 CBS의 적용 시나리오 차이

유형 | CFS | CBS
------- | ------- | -------
처리량 | 단일 클라이언트: 100MB/s(한도 1.5GB/s) | 한도 600MB/s
공유 가능성 | 여러 클라이언트 공유  | 공유 불가
중복 | 3개 | 3개
사용 방식 | 탑재 후 직접 사용 | 파일 시스템을 스스로 설치해야 함


### CFS와 CVM을 사용하여 구축한 NAS의 TCO 대비

유형 | CFS | CVM으로 구촉한 NAS
------- | ------- | -------
사용 가능한 저장량 | 1TB | 1TB
전체 구매 가능한 저장량 | 1TB | 2TB(85%의 디스크 이용률을 고려하여 1205GB 크기의 프리미엄 클라우드 디스크 2개를 마스터/슬레이브로 구입)
사용 가능한 저장량 | 1TB | 1TB
저장 리소스 비용(1년) | 7127(사용량 기반 요금제 0.58CNY/GB/월) | 4300(연/월정액 요금제 0.35CNY/GB/월)
컴퓨팅 리소스 비용(1년) | 0CNY(파일 서버슬 직접 구축할 필요 없이 바로 탑재 사용) | 23744CNY(CVM 인스턴스 2대를 마스터/슬레이브로 사용, 사양: 시리즈1-표준형-8 코어-32GB 메모리)
총 TCO(1년) | 7004 | 28044
매월 GB당 비용 | 0.58 | 2.28





