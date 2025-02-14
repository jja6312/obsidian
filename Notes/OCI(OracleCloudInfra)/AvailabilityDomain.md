# AD: Availability Domain : 가용 도메인

#### 1. 📌한줄 정의

- Region에 속하는 독립적인 데이터 센터이다. OCI에서는 3개의 AD를 가진다.

#### 2. 쉽게 설명

- 오라클 클라우드에서 빌려주는 컴퓨터들을 모아 놓은 건물 하나

#### 3. 🔍 특징

- 주요 리소스
  1. 컴퓨팅 리소스
     - VM Instance
     - Block Storage
     - Container Engine for Kubenetes(OKE)
  2. 네트워크 리소스
     - Virtual Cloud Network(VCN)
       - AD 내부에서 작동하는 가상 네트워크 환경
       - 서브넷, 게이트웨이, 라우팅 테이블 등이 포함된다.
     - 로드밸런서
  3. DB리소스
     - Oracle Autonomus Database
       - AD 내에서 고성능 데이터베이스 서비스가 제공된다.
     - Oracle Database RAC(Real Application Clusters)
       - DB복제 및 고가용성을 위해 여러 AD 혹은 FD에 분산 배치될 수 있다.
  4. 스토리지 리소스
     - Object Storage
     - File Storage
  5. 보안 및 관리 리소스
     - IAM(Identity and Access Management)
     - Monitoring 및 Logging
- 3개의 Fault Domain을 가진다.
