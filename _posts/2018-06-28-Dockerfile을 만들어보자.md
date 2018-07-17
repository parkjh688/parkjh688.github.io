---
layout : post
title : "Dockerfile 을 만들어보자 "
date : 2018-06-28 01:00:00 +0900
author : viking
category: Docker
---

Docker image를 사용하려면 이미지는 `Dockerfile`을 만들고 `docker build` 를 하거나, **Dockerhub** 과 같은 Docker registry 에서 `docker pull` 명령어로 다운로드 하면된다.

회사에서 개발한 챗봇을 배포하기 위해 Dockerfile 을 직접 만들어야해서 Docker 를 시작하게 되었다. :flushed:

<br><strong><span style="color:red">그렇다면 Dockerfile 이란 무엇일까?</span></strong><br><br>

`Dockerfile` 은 Docker 이미지 설정 파일이다. Dockerfile 을 build 명령어를 이용해서 설정한 그대로 이미지를 생성할 수 있다.

간단하게 ubuntu:16.04 를 기반으로 python3.6, git 을 설치하는
Docker image를 생성한다고 한다면 Dockerfile 은 아래 example 과 같다.

```
FROM ubuntu:16.04

RUN sed -i 's/archive.ubuntu.com/kr.archive.ubuntu.com/g' /etc/apt/sources.list

# update apt-get
RUN apt-get update

RUN apt-get install -y software-properties-common

# install python 3.6
RUN add-apt-repository ppa:jonathonf/python-3.6 && \
    apt-get update && \
    apt-get install -y build-essential python3.6 python3.6-dev python3-pip python3.6-venv

# install git
RUN apt-get install -y git
```

linux 기반이기 때문에, apt-get 로 설치를 한다.

위 example 과 같이 Dockerfile 명령어를 이용한다.
Dockerfile 명령어에는 FROM, MAINTAINER, RUN, CMD, WORKDIR 이 있다

- FROM : 어떤 이미지를 기반으로 하는지 설정하는 명령어.
example 에서는 `FROM ubuntu:16.04`을 이용해 ubuntu:16.04 이미지를 기반으로 설정하도록 하였다.
- MAINTAINER : 이미지를 생성한 사람의 정보를 설정하는 명령어.
형식은 자유이고, 생략도 할 수 있다. example 에서는 생략했다.
`MAINTAINER Junghyun <junghyun@viking.com>` 형식으로 사용하면 된다.
- RUN : 셸 스크립트 혹은 명령을 실행하게 하는 명령어.
RUN 을 실행할 때마다 레이어가 생성된다.
Dockerfile 을 구글에서 서치하다보면 아래와 같이 한 번에 여러 패키지를 install 하는 경우가 많다.
<br><br>
  ```
  # install python 3.6
  RUN add-apt-repository ppa:jonathonf/python-3.6 && \
      apt-get update && \
      apt-get install -y build-essential python3.6 python3.6-dev python3-pip python3.6-venv
  ```
  <br>
  왜 한 번에 묶어서 RUN 명령어를 사용할까?

  Docker 의 이미지는 스토리지 엔진에 따라서 레이어의 개수가 제한되어 있는 경우가 있기 때문이다. 42개, 127개 등 스토리지 엔진에 따라서 제한 개수가 다르다.
  <br>
  레이어 개수와 관련된 이유 이외에도 위의 Dockerfile 내용을 보면 주석으로 `# install python 3.6` 이라고 쓰여있다. <strong>`RUN`</strong> 하는 모든 내용이 python 3.6 을 install 하는 것과 관련이 있다. 따라서 설치 모듈 별로 나누기 위해서 하나의 RUN 명령어에 묶어 둔다.
  <br>

  2 개의 layer 가 생길 것을 1개의 레이어로 대체한 것이다.

* CMD : {color:#f6c342}*docker run*{color} 실행 시 명령어를 주지 않았을 때 사용*할 default 명령을 설정, ENTRYPOINT의 default 파라미터를 설정할 때 사용하는 명령어.
CMD 는 한 개의 명령어만 실행된다. 따라서 가장 마지막 CMD 만 실행이 된다.
CMD 는 세 가지 form 이 있다.
  * CMD ["executable","param1","param2"] (exec form, this is the preferred form)
  * CMD ["param1","param2"] (as default parameters to ENTRYPOINT)
  * CMD command param1 param2 (shell form)

* ENTRYPOINT : {color:#f6c342}*docker run*{color} 실행 시 실행되는 명령어.
ENTRYPOINT 는 2가지 형태가 있다.
  * ENTRYPOINT [“executable”, “param1”, “param2”] (exec form, preferred)
  * ENTRYPOINT command param1 param2 (shell form)
<br>


CMD와 ENTRYPOINT 에 대해서 헷갈리기 쉬운데,
https://docs.docker.com/engine/reference/builder/#cmd
https://docs.docker.com/engine/reference/builder/#entrypoint
를 보면 이해하는데 도움이 될 것이다.
CMD와 ENTRYPOINT 에 대해 terminal 에서 실험해보고 향후에 더 자세하게 포스팅 하기로 하겠다. :sunglasses:


* WORKDIR : working directory 설정.
* EXPOSE : 포트 포워딩.
<br>
여기까지는 Dockerfile 작성하는 법을 명령어 위주로 간단히 설명해보았다.

---
회의 시간에 Docker 패키징이 얼마나 진행되었는지 이야기하다가 배운 것이 몇 개 있어서 적어보겠다.

아까 그 예제를 다시 가져와서 보자.
```{.python}
FROM ubuntu:16.04

RUN sed -i 's/archive.ubuntu.com/kr.archive.ubuntu.com/g' /etc/apt/sources.list

# update apt-get
RUN apt-get update

RUN apt-get install -y software-properties-common

# install python 3.6
RUN add-apt-repository ppa:jonathonf/python-3.6
RUN apt-get update
RUN apt-get install -y build-essential python3.6 python3.6-dev python3-pip python3.6-venv

# install git
RUN apt-get install -y git
```

<br>

제일 첫 번째로 실행하게 될 `RUN sed -i 's/archive.ubuntu.com/kr.archive.ubuntu.com/g' /etc/apt/sources.list` 을 한 이유가 뭐였냐면, 사실은 구글링해서 어떤 stackoverflow 인지 블로그인지에서 긁어온 코드이긴하다.

Dockerfile 을 계속 build 하면서 테스트하는데 우분투가 자꾸 없는 repo 라고 하고, 귀찮게 굴어서 위에서도 말했듯이 이미지는  **Dockerhub** 과 같은 Docker registry 에서 받을 수 있기 때문에 repository 를 바꿔주기 위해 한 것이다.

`sed` 가 하는 일은 pyCharm 이나 atom editor 에 있는 replace 기능과 같은 기능인데  <strong>`'s/archive.ubuntu.com/kr.archive.ubuntu.com/g'`</strong> 이렇게 써있는 모든 문자열을 <strong>`/etc/apt/sources.list`</strong> 이걸로 바꾸라는 뜻이다. -i 옵션을 추가해서 결과를 화면에 출력하는 대신 해당 파일을 변경하여 저장하도록 했다.

이로써 속도가 훨씬 빨라졌다. :grin:


<br>
그 다음 배운 것은 <strong>apt-get upgrade</strong> 과 <strong>apt-get update</strong>의 차이점!

글씨는 분명 다르게 생겼으나.. 내가 아는 차이점은 그게 전부..
진짜 <strong>apt-get upgrade</strong> 과 <strong>apt-get update</strong> 차이점은 아래와 같다.
<br>

```
apt-get update :
우분투에서 사용 가능한 패키지들과 그 패키지들의 버전 리스트를 업데이트 하는 명령어.
패키지 버전을 업그레이드 하지는 않고, 최신 버전 패키지가 있는지 확인 후 버전 리스트만 업데이트 한다.
```

```
apt-get upgrade :
우분투에 설치해놓은 패키지들을 최신 버전으로 업그레이드 하는 명령어.
```

linux 명령어로 directory 를 만들 때 사용하는 mkdir 의 옵션에 대해서도 하나 알게되었다. `mkdir -p` 는 내가 들었던 설명을 그대애애애로 쓰자면

```
$ mkdir a/b/c/d
mkdir: a/b/c: No such file or directory
```
이렇게 오류가 나는데
```
$ mkdir -p a/b/c/d
$ ls
a
```
이렇게 디렉토리가 만들어진다.

mkdir 의 -p 옵션이 하는 일이 <strong>상위(parents) 경로도 함께 생성</strong>해주는 일이다.

마지막으로 touch 명령어에 대해 배웠는데, 사실 이거는 대학교 2학년 UNIX 시간에 배웠지만, 뇌의 깊은 주름 사이에 껴서 기억이 안났다. 배웠다는 사실만 기억이 났었다. :sweat_smile:

touch 명령어는 <strong>빈 파일을 작성</strong>, <strong>파일의 타임스탬프 변경</strong>하는 일을 한다.


1. 빈 파일 작성
: <파일명>을 가진 파일이 존재하지 않을 떄, 아래와 같이 `touch <파일명>` 커맨드를 입력하면 <파일명> 을 가진 사이즈가 0인 파일이 생긴다.
<br>
    ```
    $ touch viking.py
    $ls -al
    total 0
    drwxr-xr-x   3 junghyun  staff   96  6 28 11:33 .
    drwxr-xr-x  12 junghyun  staff  384  6 28 11:33 ..
    -rw-r--r--   1 junghyun  staff    0  6 28 11:33 viking.py
    ```

<br>
2. 파일의 타임스탬프 변경
: <파일명> 을 가진 파일이 이미 존재할 때, 아래와 같이 `touch <파일명>` 커맨드를 입력하면 파일의 타임 스탬프 값이 현재 시간으로 바뀐다.
<br>
    ```
    $ touch viking.py
    $ls -al
    total 0
    drwxr-xr-x   3 junghyun  staff   96  6 28 11:33 .
    drwxr-xr-x  12 junghyun  staff  384  6 28 11:33 ..
    -rw-r--r--   1 junghyun  staff    0  6 28 11:38 viking.py
    ```
