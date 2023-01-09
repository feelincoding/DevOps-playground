# kubernetes 관리

## 컨테이너 레지스트리 관리

### 컨테이너 이미지

- 컨테이너: 소프트웨어를 실생시키기 위해 필요한 모든 것을 포함한 패키지
- 컨테이너 이미지: 컨테이너를 묶는 형태
- ubuntu, nginx, mariadb, kube-apiserver...
- cloud native
- 컨테이너 이미지 레지스트리와 Pull/Push 통신
- "Registry_Url"/"Owner"/"Repo":"Tag"
  - docker pull my-registry.com/nginx/nginxmirror
  - Registry URL: port를 포함할 수 있는 Registry 서버 주소
  - Owner: 이미지 Repo의 소유자
  - Repo: 이미지가 저장되는 공간(필수)
  - TAG: 이미지 별 TAG 이름(기본값 latest)

### 이미지 레지스트리

- Public Container Registry
- Private Container Registry
  - 공개할 수 없는 내부 이미지 사용
  - 폐쇄망
  - 퍼블릭 컨테이너 레지스트리 의존성 제거
  - 장애 위협 제거
  - Rate limit 방지

### 컨테이너 이미지 in 쿠버네티스

- ImagePullPolicy
  - 컨테이너 레지스트리에서 이미지를 가져올 때 적용되는 정책
  - IfNotPresent: 이미지가 존재하지 않을 때만 가져옴, 기본값
  - Always: 항상 컨테이너의 레지스트리의 해당 이미지가 로컬의 이미자와 같은지 확인하면서 가져옴, 로컬 이미지와 레지스트리 이미지가 다를 때 이미지를 가져옴
    - 컨테이너 이미지의 Digest를 사용, Digest를 비교하는 것
  - Never: 원격 컨테이너 레지스트리로 부터 이미지를 다운 받으려는 시도를 하지 않음, 이 경우 컨테이너를 실행하기 위해서는 반드시 노드에 실행시키려는 이미지가 저장 되어 있어야 한다.
  - 기본 정책
    - Image Tag가 'lates'가 아닌 경우는 'IfNotPresent'적용
    - Image Tag가 'lates'태그 + ImagePullPolicy 미표기 시 'Always' 적용
    - Image Tag가 없는 경우 'lates'태그 적용
    - 최초 Pod 생성 때 적용
    - 생성 후 Image Tag 수정 시에는 별도로 Policy 조정 필요
- ImagePullSecrets
  - 컨테이너 레지스트리에 접근하기 위한 인증 정보
  - 사용하려는 Private Registry의 인증 정보
  - Kubernetes Secret 리소스 참조
  - Pod에 종속된다, 컨테이너마다 다른 시크릿 사용하는 것은 아님
  - Usecase
    - 오픈소스 이미지만 사용(갑작스러운 장애 대응을 위해)
    - 외부에 공개하지 않는 이미지 사용
    - 이미지에 대한 접근 권한 통제
    - 테넌트(프로젝트)별 프라이빗 이미지를 사용

## 리소스 스케줄링

### 쿠버네티스 스케줄러

- 다수의 노드 안에
  - 다수의 파드 안에
    - 다수의 컨테이너 존재

### 셀렉터, 어피니티

### 테인트, 톨러레이션

## 쿠버네티스 RBAC

### Kubernetes Authorization

- Authentication : 회사 건물 보안 게이트
- Authorization : 회사 내부 층별 권한
- ABAC 속성 기반 접근 제어
- RBAC 역할 기반 접근 제어
  - 쿠버네티스 리소스로 정의 및 관리
    - Role: 파드 생성 및 열람과 같은 특정 리소스에 대한 권한
    - RoleBinding: 권한에서 명시한 Role을 특정유저나 그룹에 적용하는 것, 바인딩 하는 것
    - => 네임스페이스에 종속
    - ClusterRole: 클러스터 전체 리소스에 대한 권한
    - ClusterRoleBinding: 클러스터 전체 리소스에 대한 권한을 특정 유저나 그룹에 적용하는 것
- Webhook을 이용한 권한 체크

### Kubernetes RBAC

### ServiceAccount

## 쿠버네티스 트러블슈팅

### 클러스터 문제

- 노드 리스트 및 상태 확인
  - 노드가 문제일 경우, Status를 Ready 에서 NotReady로 변경
  - 노드 컨디션
    - Ready
    - DiskPressure
    - MemoryPressure
    - PIDPressure
    - NetworkUnavailable
- 쿠버네티스 Components 로그 확인
- 클러스터 장애 케이스
  - 노드 다운
  - 네트워크 단절
  - 디스크 에러: 디스크가 고장나거나 가득 차거나
  - 쿠버네티스 컴포넌트 에러
  - 쿠버네티스 설정 에러
- 클러스터 장애 대응 및 방지
  - Controlplane 의 HA 보장, 단독이 아닌 복수의 Controlplane 구축(단독이면 특정 노드 다운될 경우 해당 클러스터 사용 불가)
  - Disk Snapshot: 디스크 에러 방지
  - Pod의 Replica 적용: 워커노드 장애 대응, 어플리케이션 단에서 Replica 개수를 복수로 사용
  - 어플리케이션 단에서 재시작을 보장해야 함

### 어플리케이션 문제

- Pod의 상태 확인
  - Pending
    - 스케줄링 실패
    - Describe Pod 의 Event Message 확인
    - 실제 리소스 부족여부 확인(CPU, Memory, Disk)
    - 노드 label, taint 확인
  - 
- Pod 디버깅

### 일반적인 문제해결 방법
