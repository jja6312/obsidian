- OCI 테넌시
- OCI Vault
- OCID
- Service Limits

- OCI Identity

  - Compartments

- Billing & Cost Management
  - Overview
  - Cost Analysis
  - Cost and Usage Reports
  - Budgets
    - option
      - active
      - forecast
  - Scheduled Reports
- OCI COMPUTE SERVICE

  - 요구사항에 따라 베어메탈, 가상머신 모두 제공한다.
  - 블록 볼륨과 같은 다양한 OCI 서비스를 사용해 Compute 인스턴스에 대한 데이터를 지속할 수 있다.

    - File Storage
    - Object Storage
    - 엄격한 ID 및 액세스 관리를 통해 컴퓨팅 인스턴스를 안전하게 관리한다.

  - Compute Shapes
    > CPU 수와 VM 메모리를 지정하는 템플릿
    - Compute Instance
      - OCPUs, Memory, Network IO
    - Shape for Bare Metal and Virtual Machines
      - Standard
      - Dense I/O
      - GPU
      - HPC
    - Flexible Shapes
      - 일부 OCPU만 지정하고 메모리와 네트워크 양을 지정할 수 있다.

- Compute Instance

  - Advanced Configuration

    - 부트볼륨
      > 선택한 OS이미지가 포함되어 있고, 기계를 부팅하는데 사용한다.
      - 인스턴스에 존재하는 유일한 지속적인 볼륨
      - Custom boot volume size up to 32 TB(사용자 정의 부팅 볼륨 지정)
        - 리눅스 부팅볼륨은 50GB, Window는 256GB
      - BYOK encrypt at rest and in transit
        - 부트볼륨은 오라클에서 기본적으로 암호화된다.
      - 백업
        - 가동중지시간이 없음.
        - 블록레벨에서의 디스크 스냅샷
        - 수동 및 정책기반 예약 가능(증분백업, 주간 전체 백업)
        - 백업을 다른 OCI 영역으로 자동 복제할 수 있다.(단, OS에 대해서만)
      - 복원
        - 시나리오1: 새 인스턴스 생성시 부팅볼륨 복원
        - 시나리오2: 문제해결을 위해 데이터를 복원해야하는 경우 기존컴퓨터에 데이터 볼륨으로 첨부할 수 있다
      - 복제
        - 복제는 백업보다 빠르다
        - 부팅/블록볼륨 백업은 해당 볼륨에 존재하는 '데이터 만' 포함된다.
        - 복제는 데이터가 있든 없든 볼륨 블록의 블록 복제본이다.
      - 스토리지
        - 블록볼륨(고성능 일래스틱 블록스토리지. 정책기반 백업/복원/복제를 지원한다.)
        - 파일 스토리지 서비스(NFS 기반 엔터프라이즈급 파일 서비스)
        - 오브젝트 스토리지(비정형데이터를 위한 일래스틱 스토리지)
        - 아카이브 스토리지(비정형데이터의 장기간 저장소.)
    - Choose 1 of 3 Fault Domains
    - Enable Mornitoring using OCI Mornitoring Service
    - Enable Oracle Cloud Agent to automatically apply pathches/updates

    - 리눅스 기본 사용자는 OPC(배포판별로 다를수있음. Ubuntu는 Ubuntu)



- VCN
- DRG
- NAT
- VPN
- Site-to-Site VPN
- VCN Peering
- FastConnect
- Cross-VCN Routing

### instance 생성
1. puttygen 설치
2. 공개키/암호키 쌍 생성
```bash
ssh-keygen.exe -t rsa -b 4096 -f oci_labs.id_rsa
```


- 특정  compartment로 인스턴스 생성
- 부트 볼륨 백업
- 부트 볼륨 복원

### instance 관련
- instance configuration
- instance pool
- Autoscailing configuration 
- secondary VNIC 설정
	- 공식문서: https://docs.oracle.com/en-us/iaas/oracle-linux/oci-utils/index.htm#oci-network-config


### VNIC 설정시 에러.
oci-network-config ...
Failed to get API session

- 도메인과 호스트네임
	- VCN label
	- Subnet name
	- Instance name

- Resolver
	- Default Resolver
		- VCN에서 해석하는 프라이빗 도메인 네임
			- instance-name.subnet-name.vcn-name.oraclevcn.com
	- Customer Resolver
		- 특정 DNS 이름에 대해 커스텀 네임 서버로 요청 전달
		- 온프레미스와 클라우드 네트워크의 하이브리드 DNS 구성 지원

- 
- 라우팅테이블
	- 서브넷에 할당되며 security list를 할당하는 것과 비슷하다.
	- 서브넷의 모든 리소스에는 라우팅 테이블이 적용된다.
	- VCN자체에는 내부 로컬 라우팅 테이블이 있으므로 트래픽 라우팅에 대해 걱정하지 않아도 됨.
	- VCN을 떠나는 트래픽에 대한 라우팅 규칙 및 테이블을 정의하는 것만 신경써야 한다.

- 서비스게이트웨이
	- 프라이빗 서브넷에서 OCI의 특정 서비스에 인터넷을 거치지않고 연결할 수 있는 게이트웨이.

- 라우팅테이블
	- Routing rule
		- Dynamic Routing Gateway
		- Internet Gateway
		- Local Peering Gateway
		- NAT Gateway
		- Private ip
		- Service Gateway

- 블록 볼륨
	- 인스턴스당 최대 32블록볼륨 생성 가능(최대 1PB)
	- 블록 볼륨 크기를 늘리면 성능이 증가한다.
	- OCI는 60IOPS per GB , 35000 IOPS per volume을 제공한다.
	- OCI는 블록볼륨 민 오브젝트스토리지 백업을 제공한다.
	- Point-in-time direct disk cloning을 지원한다.
	- BYOK 및 다른 암호화를 통해 암호화할 수 있다.

### 지금까지한 것
1. vcn 생성(10.0.0.0/16)
2. subnet 생성
	1. public subnet(10.0.0.0/28)
	2. private subnet(10.0.0.16/28)
3. security list 생성
	1. public subnet(Default_Bastion_SecList)에는 0.0.0.0/0 22 ingress, 10.0.0.0/16 22 egress 허용
	2. private subnet에는 10.0.0.0/28 22포트 ingress 허용
4. subnet에 security list 붙이기
5. IGW 생성
6. 기존 Routing table에 Route Rule 추가(igw, destination 0.0.0.0/0)
7. bastion 인스턴스 생성
	1. bastion 서버에 public ip  주소가 정상적으로 할당되어 있는지 확인
	2. bastion 서브넷에 올바른 security list와 rule이 있는지 확인(ssh허용을 위해)
	3. 서브넷에 기본 라우팅 테이블이 할당되어 있는지 확인
	4. 올바른 키를 사용하고 있는지 확인
8. private 인스턴스 생성
9. bastionhost에서 private ssh 연결 시도
	1. puTTY를 통해 bation에 접근한 뒤 ssh 접근.(pageant.exe 로 암호키 설정해야함)
10. private 에서 네트워크연결 가능하게하기
	1. vcn 내에서 NAT 생성
	2. private 라우팅 테이블 생성 및, 해당 라우팅 테이블에 NAT에 대한 룰 추가(Destination 0.0.0.0/0)
	3. private subnet의 security list의 egress(0.0.0.0/0) 추가
11. Service Gateway생성
	1. vcn의 Service Gateway 생성(ALL SERVICE)
	2. 라우팅테이블 중, private Routing table에서 rule 추가(Service관련 )
	3. private subnet의 security list에서 egress에 Service 추가
12. 블록 볼륨
	1. 크기 및 성능 계획
		1. 실습에서는 50GB, 최저성능으로 설정함.
	2. Paravirtualization 혹은 iSCSI를 사용해 인스턴스에 첨부
		1. 실습에서는 Paravirtualization. 프라이빗 인스턴스에 read/write로 붙였다.
		2. 인스턴스에 ssh로 접근해서, fdisk -l 을 통해 확인 가능.
		3. sudo fdisk /dev/sdb 
			1. n
			2. p
			3. 1
			4. 시작섹터 끝섹터 둘다 기본값.
			----
			1. w
			2. sudo fdisk -l
			---
			1. sudo mkfs -t ext4 /dev/sdb1
			   (파티션이 형식화되면 계속 마운트 할 수 있다.)
			2. sudo mkdir /datavol
			3. sudo mount /dev/sdb1 /datavol
			4. df
			5. df -h
			---
			1. sudo vi /etc/fstab
				1. 마지막라인에 아래 추가
				   /dev/sdb1       /datavol        ext4    defaults 0      1
			2. sudo umount /datavol
				(잘 동작하는지 보기위해)
			3. sudo mount -a
			   (/etc/fstab 파일의 항목을 기반으로 모든 파일시스템을 마운트)
			4. sudo systemctl daemon-reload
			   (실습에서 하란말없엇는데 난햇음)
			5. sudo df -h
	3. iSCSI를 사용한다면 일련의 커맨드를 통해 볼륨을 인스턴스에 연결
	4. 인스턴스에서 /etc/fstab 에 볼륨 마운트