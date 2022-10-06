# 01 도커 이미지와 컨테이너

## 도커 구성요소

### - Client

- 도커 명령어가 클라이언트 역할을 수행

### - Docker Host: 도커 엔진이 설치된 서버

- Docker Daemon: 도커 엔진
- Container: 도커 엔진에서 실행되는 프로세스
- Image: 컨테이너 실행에 필요한 파일과 설정값 등을 포함하고 있는 것
  - 직접 build 하거나
  - pull로 땡겨 오거나
  - Registry에 저장된 이미지를 pull로 땡겨 올 수 있다.
  - 실행시키면 Container가 된다.

### - Registry

- 도커 이미지를 저장하는 곳

### - 도커 이미지와 컨테이너

- 이미지와 컨테이너는 도커에서 사용하는 가장 기본적인 단위
- 이미지과 컨테이너는 1:N 관계
- 이미지는 컨테이너를 실행하기 위한 파일과 설정값 등을 포함하고 있는 것
  - 읽기 전용
- 컨테이너는 이미지를 실행한 상태
  - 컨테이너에서 무엇을 하든 이미지는 영향 받지 않음
- 작업 사이클
  - 도커 파일 빌드(BUILD)
  - 이미지 생성
  - 이미지 실행(RUN)
  - 컨테이너 생성
- 관계 예시
  - 이미지 vs 컨테이너
  - 클래스 vs 인스턴스
  - 프로그램 vs 프로세스

### - 도커 이미지 저장소

- 도커 이미지를 관리하고 공유하기 위한 서버 어플리케이션
  - 도커허브, AWS ECR 등...

# 02 컨테이너 라이프 사이클

## 라이프 사이클

- created: 컨테이너가 생성되었지만 아직 시작되지 않은 상태, create로 만듦
- running: 컨테이너가 실행 중인 상태, create에서 start로 시작, run으로 만들면 바로 시작됨
- paused: 컨테이너가 일시 중지된 상태, pause로 일시 중지<->unpause로 재개
- stopped: 컨테이너가 중지된 상태, stop으로 중지<->start로 재시작
- deleted: 컨테이너가 삭제된 상태, rm으로 삭제

# 실습

```bash
tf output
ssh -i ~/credentails/tedioabs/fastcampus.pem ubuntu@13.125.233.131
docker -vs
docker ps
docker ps -a
docker run nginx
ctrl + c

docker create nginx
docker start "name or id"
```
