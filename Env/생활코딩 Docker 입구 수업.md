## 03. 이미지 pull

- 도커 레지스트리는 도커 이미지를 관리하는 저장소로, 필요한 이미지를 다운로드 받으 수 있다

- 대표적으로 도커 허브가 있으며, 도커 허브에서 필요한 이미지를 다운로드 받을 수 있다.

- 이미지를 다운로드 받는 행위를 pull 이라고 한다

- 이미지를 실행하는 행위를 run 이라고 한다

- 이미지는 여러개의 컨테이너를 가질 수 있다.

- 이미지를 다운로드 받는 명령어

  - [docker image pull](https://docs.docker.com/reference/cli/docker/image/pull/)

- 이미지를 잘 다운로드 받았는지 확인하는 명령어

  - [docker image ls](https://docs.docker.com/reference/cli/docker/image/ls/)

<br/>

## 04. 컨테이너 run

- 컨테이너를 만드는 명령어

  - [docker container run](https://docs.docker.com/reference/cli/docker/container/run/)

- httpd 기반으로 컨테이터 만들어지고 실행된다

```shell

$ docker run httpd

```

- 우리가 만든 컨테이너를 볼 수 있다

```shell

$ docker ps

```

- 하나의 이미지는 여러개의 컨테이너를 만들 수 있다

```shell

$ docker run --name ws2 http


$ docker ps
# sw2라는 NAMES을 갖는 컨테이너가 추가된 것을 확인할 수 있다

```

- 실행 중인 컨테이너를 끄고 싶은 경우

  - [docker container stop](https://docs.docker.com/reference/cli/docker/container/stop/)

```shell

$ docker stop ws2
# 컨테이너 id를 적어도 된다

$ docker ps
# ws2가 더 이상 보이지 않는다

$ docker ps -a
# ws2 컨테이너도 보인다
# stop 했다고 컨테이너가 삭제되는 건 아니다

$ docker start ws2
# 우리가 중지시켰던 컨테이너가 다시 켜진다

$ docker logs ws2
# 로그를 다시 볼 수 있다
# https://docs.docker.com/reference/cli/docker/container/logs/

$ docker logs -f ws2
# 실시간으로 로그를 확인할 수 있다
```

- 컨테이너를 삭제하는 명령어

  - [docker container rm](https://docs.docker.com/reference/cli/docker/container/rm/)

```shell
$ docker rm ws2
# 만약 실행중이라면 에러가 발생한다

$ docker stop ws2
# 먼저 stop 한 다음

$ docker rm ws2
# 삭제한다
```

- 이미지를 삭제하는 명령어

  - [docker image rm](https://docs.docker.com/reference/cli/docker/image/rm/)

```shell
$ docker rmi httpd
```

<br/>

## 05. 네트워크

- 도커를 이용하면 웹 서버가 컨테이너에 설치된다

- 컨테이너가 설치된 운영체제를 도커 호스트라고 한다

- 하나의 도커 호스트에는 여러개의 컨테이너가 만들어질 수 있다.

- 컨테이너와 호스트 모두 독립적인 실행환경이기 때문에 독립적인 포트와 파일 시스템을 가지고 있다

- 만약 클라이언트에서 바로 호스트에 접속하면 컨테이너에 접속이 되지 않는다

- 호스트와 컨테이너는 연결이 끊겨 있기 때문

- 호스트와 컨테이너를 연결시켜주는 명령어가 아래 명령어

  - 그리고 이를 포트포워딩이라고 한다

```shell

$ docker run -p 80:80 httpd
# 앞의 80은 호스트의 포트
# 뒤에 80은 컨테이너의 포트

```

<img src='./images/생활코딩 Docker 입구 수업/01.png'>

- 만약 접속 경로가 8000으로 바뀌면 아래처럼 수정해준다

```shell

$ docker run -p 8000:80 httpd
# 앞의 80은 호스트의 포트
# 뒤에 80은 컨테이너의 포트

```

<br/>

## 06. 명렁어 실행

- 컨테이너 안에서 실행하는 명령어

  - [docker container exec](https://docs.docker.com/reference/cli/docker/container/exec/)

```shell

$ docker exec ws3 pwd
# ws3라는 컨테이너에서 pwd라는 명령어를 실행한다

$ docker exec ws3 ls



```

- 터미널과 컨테이너를 연결해서 지속적으로 명령어를 입력하고 싶은 경우 `-it` 옵션과 준다

```shell
$ docker exec ws3 -it ws3 /bin/sh
# shell은 사용자가 입력한 명령어를 운영체제에 전달하는 역할
# bin/bash 을 실행할 수 도 있다.

$ exit
# 명령어를 입력하면 이 때 부터 입력하는 명령어는 호스트를 대상으로 실행된다.

```

<br/>

## 07. 호스트와 컨테이너의 파일 시스템 연결

- 컨테이너에 직접 접근해서 파일을 수정했다고 하자

- 만약 컨테이너가 사라지게 되면 작업한 것이 물거품이 된다

- 컨테이너를 사라지지 않게 하는 방법도 있겠지만,

- 컨테이너르 사용하는 이유는 필요할 때 언제든지 생성해서 사용할 수 있고, 필요 없으면 제거할 수 있다는 점

- 따라서 아래 그림과 같이 `/Desktop/htdocs`와 컨테이너에 있는 `/apache2/htdocs/`를 연결하고

- 호스트 쪽에서 수정이 이루어져 있을 때 컨네이너의 파일 시스템에 반영될 수 있게 한다면

- 컨테이너가 날라가도 소스코드는 여전히 호스트에 남아있기 때문에 안전하게 개발을 할 수 있게 된다

<img src='./images/생활코딩 Docker 입구 수업/02.png'>

```shell

$ docker run -p 8888:80 -v ~/Desktop/htdocs:/usr/local/apache2/htdocs httpd

# -v 옵션으로 호스트에 있는 Desktop/htdocs와 컨테이너에 있는 /usr/local/apache2/htdocs를 연결

```

---

## Reference

- [생활코딩 Docker 입구 수업](https://opentutorials.org/course/4781)

- [도커 로드맵](https://seomal.com/map/1/129)
