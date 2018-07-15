---
layout : post
title : "Get started with Docker"
date : 2018-06-24 01:00:00 +0900
author : viking
category: Docker
---

#### 도커란?
> 도커란 '리눅스 기반의 컨테이너(Container) 기반의 오픈소스 가상화 플랫폼' 이다.

<br>



그렇다면 `컨테이너 (Container)` 는 무엇일까? '가상화 플랫폼' 이라면 Virtual Machine 과 다른 점은 무엇일까?

<br>

![docker_logo](https://dl.dropbox.com/s/luf0md71w2a1pzx/docker_logo.jpg)


#### 가상머신(Virtual Machine;VM) 과 도커(Docker)
 가상머신은 Host OS위에 Guest OS 전체를 가상화하여 사용하는데, 추가로 OS 를 설치하여 가상화하기 때문에 무겁고 느리다는 단점이 있다.
 아래의 이미지를 보면, Docker 는 Guest OS 없이 Host OS 의 자원(CPU, 메모리 등)을 공유하기 때문에 CPU 나 메모리는 각 프로세스가 필요한만큼 사용하게 된다.
 <br>
 ![docker_and_vm](http://dl.dropbox.com/s/cthwpox3yppiosx/docker_and_vm.png)
 <br><br>

아래 URL 블로그에 컨테이너와 이미지의 차이가 잘 설명되어있어서 https://www.shellhacks.com/docker-image-vs-container/
정리하면서 설명을 다시해보면 아래와 같다.

#### 이미지 (Image)
* 컨테이너를 실행하는데 필요한 정보들을 가지고 있는 것.
* 이미지는 혼자 `started` 하거나 `run` 하지 않는다. 반드시 `docker run` 명령어에 의해서 실행된다.
* 이미지는 `Dockerfile`로 부터 `docker build` 명령어에 의해서 만들어진다.
* 이미지는 **Dockerhub** 과 같은 Docker registry 에서 `docker pull` 명령어로 다운로드 될 수 있다.
<br>


#### 컨테이너 (Container)
* 컨테이너는 격리된 공간에서 이미지가 실행된 상태(하나의 프로세스). 따라서 1개의 이미지로 여러개의 컨테이너를 만들 수 있다.
* 컨테이너는 `docker run` 명령어에 의해 생기고 `docker ps` 명령어로 현재 실행되고 있는 컨테이너의 리스트를 확인할 수 있다.

#### 레이어 (Layer)
* 이미지를 통째로 생성하지 않고, 바뀐 부분만 생성한 뒤 부모 이미지를 계속 참조한다.
* `Dockerfile` 에서 지정한 모든 명령 (FROM , RUN , COPY 등) 은 이전 이미지가 변경되어 새로운 레이어를 만든다.
