# **Quest 06. 컨테이너**

# Quest 06. 컨테이너

## Introduction

- 이번 퀘스트에서는 컨테이너 기술과 그 활용에 대해 알아보겠습니다.

## Topics

- 컨테이너 기술
- Docker
- docker-compose

## Resources

- [Docker 101 Tutorial](https://www.docker.com/101-tutorial)
- [Docker for beginners](https://docker-curriculum.com/)
- [docker-compose](https://docs.docker.com/compose/)

## Checklist

- **컨테이너는 어떻게 동작하나요? 다른 배포판을 사용할 수 있게 하는 원리가 무엇일까요?**
    - 컨테이너는 호스트 OS의 커널을 공유하며 단순히 하나의 격리되어 있는 **`프로세스`**로써 동작하기 때문이다.
    - 도커는 컨테이너라는 가상의 '격리 환경'을 만들기 위해 리눅스의 `**namespace**`과 `**cgroup**`이라는 기능을 사용한다. (namespace와 cgroup으로 만들어진 컨테이너를 LXC라고 부른다)
        - **namespace**: 프로세스를 독립시켜주는 가상화 기술이다. 각 컨테이너에서 실행된 프로세스가 시스템(user, 파일, 네트워크, 호스트명, 프로세스)등에 대해 독립할 수 있게 해준다.
            - pid name spaces : 프로세스 격리 처리 (독립된 프로세스 공간 할당)
            - net name spaces : 네트워크 인터페이스
            - ipc name spaces : IPC 자원에 대한 엑세스 관리
            - mnt name spaces : 파일 시스템 포인트 관리
            - uts name spaces : host name 할당
        - **cgroups**: **자원**(CPU, 메모리, network bandwidth)**에 대한 제어**를 가능하게 해주는 리눅스 커널의 기능이다.
            - 메모리
            - CPU
            - Network
            - Device
            - I/O
            
- **도커 컨테이너에 호스트의 파일시스템이나 네트워크 포트를 연결하려면 어떻게 해야 할까요?**
    1. 파일 시스템
        
        ![https://docs.docker.com/storage/images/types-of-mounts.png](https://docs.docker.com/storage/images/types-of-mounts.png)
        
        - **volume** : 호스트의 특정 공간에 공유할 폴더를 생성하는 방식
            
            ```bash
            docker run <option> -v <volume-name>:<container-route> <image-name>
            docker run --name my-redis -d -p 8080:8080 -v my-redis:/data redis
            ```
            
        - **bimd** **mount** : 특정 volume을 생성하지않고 호스트의 특정 경로의 폴더를 공유하는것
            
            ```bash
            docker run <option> -v <host-route>:<container-route> <image-name>
            docker run --name my-node -d -p 8080:8080 -v $(pwd):/app/src node:10
            ```
            
    2. 네트워크 포트
        
        [https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbAcgfr%2Fbtq0Xo0u4iN%2FZcLrKsCo3qa2gdd2z5YsT0%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbAcgfr%2Fbtq0Xo0u4iN%2FZcLrKsCo3qa2gdd2z5YsT0%2Fimg.png)
        
        기본적으로 도커 컨테이너는 외부에 노출되지 않는다.
        
        만약 웹서버 등 외부에 노출해야 한다면 포트 포워딩 방식을 사용할 수 있다.
        
        - docker run `-**publish**`
            
            ```bash
            docker run <option> -p <container port>:<host port> <image-name>
            docker run -p 80:
            ```
            
            [더 다양한 방법](https://docs.docker.com/config/containers/container-networking/#published-ports)
            
        
        위  -p 플래그를 사용하는 방법은 dockr run 명령어 시 사용할 수 있는 방법이다.
        
        하지만 dockerfile로 작성한 이미지에 특정 포트를 오픈할 수 있다.
        
        - dockerfile **`expose`**
            
            ```bash
            FROM openjdk:8
            COPY Test.class /root
            
            EXPOSE 8080
            ```
            
        
- 도커 컨테이너에서 런타임에 환경변수를 주입하려면 어떻게 해야 할까요?
    - [전체 방법](https://docs.docker.com/compose/environment-variables/)
    - docker run **`-e` or** ****`--env-file`****
        
        명령로 도커 컨테이너를 실행할 때는 
        
        - e 플래그로 직접적으로 환경 변수를 주입하거나
        - env-file이라는 옵션을 사용하여 미리 파일에 정의해 놓은대로 환경 변수를 주입해 줄 수 있다.
    - dockerfile **`enviorment` or `env_file`**
        
        dockerfile에 환경변수를 정의할 수도 있다.
        
    - 하지만 container를 실행시키는 환경에서 docker-compose 혹은 kubernetes를 이용한다면 그 환경에서 환경 변수를 관리하는 방법에 맞추어 관리하는 것이 좋다.
        
        kubernetes의 경우 configmap이나 secret으로 관리한다.
        
- 도커 컨테이너의 stdout 로그를 보려면 어떻게 해야 할까요?
    - docker logs 명령어
    
- 실행중인 도커 컨테이너에 들어가 bash 등의 쉘을 실행하고 로그 등을 보려면 어떻게 해야 할까요?
    - docker exec
    - docker exec <contianer id> -it /bin/bash

## Quest

- 도커를 설치하고 그 사용법을 익혀 보세요.
- Quest 05에서 작업한 서버를 컨테이너 기반으로 띄울 수 있게 수정해 보세요. (docker-compose는 사용하지 않습니다)
- 컨테이너를 Docker Hub에 올리고, EC2에서 해당 컨테이너를 띄워서 서비스 해 보세요.
- docker-compose를 사용하여, 빌드와 서버 업/다운을 쉽게 할 수 있도록 고쳐 보세요.

## Advanced

- 도커 외의 컨테이너 기술의 대안은 어떤 것이 있을까요?
    - LXC
    - docker엔진 하위 기술인 containerd를 이용
- 맥이나 윈도우에서도 컨테이너 기술을 사용할 수 있는 원리는 무엇일까요?
