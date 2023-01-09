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
- ImagePullSecrets
  - 컨테이너 레지스트리에 접근하기 위한 인증 정보

## 리소스 스케줄링
## 쿠버네티스 RBAC
## 쿠버네티스 트러블슈팅

