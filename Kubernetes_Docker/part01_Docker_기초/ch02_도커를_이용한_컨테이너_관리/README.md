# 01 도커 이미지와 컨테이너

## 도커 구성요소

### - Client

- 도커 명령어가 클라이언트 역할을 수행
  - build, pull, run 등

### - Docker Host: 도커 엔진이 설치된 서버

- 로컬이라면 내 컴표터지 뭐
- Docker Daemon: 도커 엔진
- Container: 도커 엔진에서 실행되는 프로세스
- Image: 컨테이너 실행에 필요한 파일과 설정값 등을 포함하고 있는 것, 컨테이너를 생성할 때 필요한 요소
  - 직접 build 하거나
  - pull로 땡겨 오거나
  - Registry에 저장된 이미지를 pull로 땡겨 올 수 있다.
  - 실행시키면 Container가 된다.

### - Registry

- 도커 이미지를 저장하는 곳

### - 도커 이미지와 컨테이너

- 기본적 사이클

  - 도커 파일을
  - 빌드하면
  - 도커 이미지가 만들어지고
  - 런 하면
  - 도커 컨테이너가 만들어진다

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

## ETC

- create, run 명령어를 실행시 도커 호스트에 이미지가 없다면 자동으로 pull을 실행한다.

## 라이프 사이클

- created: 컨테이너가 생성되었지만 아직 시작되지 않은 상태, create로 만듦
- running: 컨테이너가 실행 중인 상태, create에서 start로 시작, run으로 만들면 바로 시작됨
- paused: 컨테이너가 일시 중지된 상태, pause로 일시 중지<->unpause로 재개
- stopped: 컨테이너가 중지된 상태, stop으로 중지<->start로 재시작
- deleted: 컨테이너가 삭제된 상태, rm으로 삭제

# 실습

```bash
docker run \
-i \
-t \ # -i와 -t를 같이 사용하면 표준 입력을 활성화하고 TTY 모드를 사용한다.->주로 쉘을 실행할 때 사용
--rm \ # 컨테이너 종료시 자동으로 삭제
-d \ # 백그라운드로 실행
--name my-nginx \ # 컨테이너 이름 지정
-p 80:80 \ # 포트 바인딩
-v \ # 볼륨 바인딩

tf output
ssh -i ~/credentails/tedioabs/fastcampus.pem ubuntu@13.125.233.131
docker -vs
docker ps # 현재 실행중인 컨테이너 확인
docker ps -a # 전체 컨테이너 상태 확인
# docker inspect [container] # 컨테이너의 상세 정보 확인
docker run nginx
ctrl + c

docker create nginx
docker start "name or id"
#-----------------------
docker run ubuntu:focal
docker ps -a
docker run -i -t ubuntu:focal
docker ps
docker run -d nginx
docker run -d --name my-nignx nginx

docker run -p 80:80 -d nginx
docker ps
curl localhost:80
docker run ubuntu:focal id
docker ps -a

docker inspect 691a64c23a9f
docker pause 691a64c23a9f # 일시 중지
docker unpause 691a64c23a9f # 재개
docker stop 691a64c23a9f # 중지, sigterm 시그널 전달
# docker stop $(docker ps -a -q) # 모든 컨테이너 중지, 괄호 안에 명령어를 전달하겠다.
docker kill 691a64c23a9f # 강제 종료, sigkill 시그널 전달
docker ps -a -q # 모든 컨테이너 id 출력

docker run -d nginx
docker ps
docker rm "id"
docker rm -f "id" # 강제 삭제
docker ps -a
docker container prune # 중지된 컨테이너 삭제
docker ps -a

docker stop $(docker ps -a -q)
```

# 03 도커 엔트리 포인트와 커맨드

## 엔트리 포인트

- 도커 컨테이너가 실행할 때 고정적으로 실행되는 스크립트 혹은 명령어
- 생략 가능, 생략될 경우 커맨드에 지정된 명령어로 수행

## 커맨드

- 도커 컨테이너가 실행할 때 수행할 명령어 혹은 엔트리포인트에 지정된 명령어에 대한 인자 값

-> 실제 수행되는 컨테이너 명령어 : [엔트리포인트] [커맨드]

```bash
docker run ubuntu:focal
docker ps
docker ps -a
docker inspect "id"
docker run --entrypoint sh ubuntu:focal
docker ps -a
docker run --entrypoint sh -i -t ubuntu:focal
docker run --entrypoint echo ubuntu:focal hello world
docker inspect "id"
```

-> 커스터마이징의 의미가 있다.

# 04 환경변수

```bash
docker run -i -t -e MY_HOST=fastcampus.com ubuntu:focal bash
echo $MY_HOST
env
cat sample.env
docker run -i -t --env-file ./sample.env ubuntu:focal env
```

# 05 도커 명령어 실행

## exec

- 실행 중인 컨테이너에서 명령어를 실행
- docker exec [옵션] [컨테이너명] [명령어]

```bash
docker run -d --name my-nginx nginx
docker ps
docker exec my-nginx env
docker exec -i -t my-nginx bash # -i: 표준 입력, -t: 터미널, 세션 직접 연결해서 bash로 작업하겠다.
docker run -d -p 80:80 nginx # 모든 포트의 80번에서 80으로 접근을 허용
docker run -d -p 80 nginx # 특정 포트 번호에서 80으로 접근을 허용
curl localhost:xxxxx
docker rm -f "id"
docker run -d -p 127.0.0.1:80:80 nginx # 특정 ip에서 80번 포트에서 80으로 접근을 허용
docker ps -a
curl localhost:80
```

# 06 도커 네트워크 구조

- veth: virtual ethernet
- eth: ethernet
- expose: 그저 문서화 용도
  - docker run -d --expose 80 nginx
  - 매핑을 의미하는 화살표 표시 없음
- publish: 실제 포트를 바인딩
  - docker run -d -p 80 nginx
- 도커 네트워크 드라이버
  - docker -> container network model -> native drivers, remote drivers
    - native drivers: bridge, host, none, overlay
    - remote drivers: 3rd party plugins
  - docker networking
    - single host networking: bridge, host, none
      - bridge: docker0(default), user defined
      - none: 컨테이너가 네트워크 기능이 필요 없을 때, 보통 기본 드라이버를 네트워크 none으로 설정
      - bridge: 사용자 정의 네트워크 만들 때 사용
    - multi host networking: overlay

```bash
docker run -d -p 80:80 nginx # 모든 포트의 80번에서 80으로 접근을 허용
docker run -d -p 80 nginx # 특정 포트 번호에서 80으로 접근을 허용
curl localhost:xxxxx
docker rm -f "id"
docker run -d -p 127.0.0.1:80:80 nginx # 특정 ip에서 80번 포트에서 80으로 접근을 허용
docker ps -a
curl localhost:80
docker run -d --expose 80 --name nginx-expose nginx
docker network ls

docker run -i -t --net none ubuntu:focal
docker run -d --network=host grafana/grafana
docker ps
curl localhost:3000 # ps 했을 때는 3000번 포트가 열려있지 않은 것 처럼 보이지만, 실제로는 열려있음, 근데 호스트를 쓰다보니 ip는 없음
docker inspect "id"
docker network create --driver=bridge my-bridge-network
docker run -d --network=my-bridge-network --net-alias=hello nginx
docker run -d --network=my-bridge-network --net-alias=grafana grafana/grafana
# net-alias: 브릿지 네트워크 안에서 도메인 이름을 부여할 수 있음
docker network ls
docker ps
docker exec -it "grafana id" bash # grafana 컨테이너에 접속
docker exec -it "nginx id" bash # nginx 컨테이너에 접속
curl grafana:3000 # nginx 도메인에서 grafana 도메인으로 접속
exit
ifconfig

```

# 07 도커 볼륨

## 도커 레이어 아키텍쳐

- 도커 이미지는 여러 개의 레이어로 구성
- docker run app
  - `read write`(container layer)
    - layer 6: container layer
- docker build -t app .
  - `read only`(image layer)
    - layer 5: update entrypoint
    - layer 4: source code
    - ---> layer 1~3: 변경 없음
    - layer 3: install in pip packages
    - layer 2: changes in apt packages
    - layer 1: base ubuntu layer

# 08 도커 로그

- docker logs "id"
- docker logs --tail 10 "id" # 마지막 10줄만 보기
- docker logs -f "id" # 실시간 로그 확인
- docker logs -t "id" # 시간 표시
- cat /var/lib/docker/containers/"id"/"id"-json.log
- docker run \ -d \ --log-driver json-file \ --log-opt max-size=3m \ --log-opt max-file=5 \ nginx
  - max-size: 3mb
  - max-file: 5개

# # 기타

- nginx: 경량 웹 서버
- run: 이미지 빌드할 때 실행할 명령어
- cmd: 컨테이너가 실행될 때 실행할 명령어
  - CMD 명령어는 해당 이미지를 컨테이너로 띄울 때 디폴트로 실행할 커맨드나, ENTRYPOINT 명령문으로 지정된 커맨드에 디폴트로 넘길 파라미터를 지정할 때 사용한다.
- entrypoint: 컨테이너가 실행될 때 항상 실행할 명령어
  - ENTRYPOINT ["<커맨드>","<파라미터1>","<파라미터2>"]
