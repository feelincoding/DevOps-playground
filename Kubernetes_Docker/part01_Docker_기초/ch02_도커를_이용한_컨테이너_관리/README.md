# 01_도커_이미지와_컨테이너
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