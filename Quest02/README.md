# Quest 02. 프로그래밍의 기초

## Introduction

- 이번 퀘스트를 통해 리눅스의 기본적인 구조와 기능에 대해 공부할 수 있습니다.

## Topics

- 리눅스의 기본 커맨드
    - `cd`, `pwd`, `ls`, `cp`, `mv`, `mkdir`, `rm`, `touch`, `ln`, `echo`, `cat`, `tail`, `find`, `ps`, `kill`, `grep`, `wc`, `df`, `du`
    - 파이프(`|`) 문자
- 리눅스의 기본적인 디렉토리 구성
    - `/bin`, `/usr/bin`, `/boot`, `/dev`, `/etc`, `/home`, `/lib`, `/mnt`, `/proc`, `/root`, `/sbin`, `/usr/sbin`, `/tmp`, `/usr`, `/var`
- 쉘과 환경변수와 퍼미션
    - sh, bash, zsh
    - `.bash_profile`, `.bashrc`, `.zshrc`
    - `env`, `set`, `unset`, `export`
    - `chmod`, `chown`, `chgrp`
    - setuid, Sticky bit
- 운영체제의 기초
    - 프로세스와 쓰레드
    - 파일 시스템
- 리눅스의 배포판
    - Ubuntu, Debian, Redhat Enterprise, CentOS, Gentoo, Amazon Linux
    - 패키지 시스템: `apt`(.deb), `yum`(.rpm)
- vi
    - `i`, `w`, `q`, `u`, `d`, `p` 명령

## Resources

- [The Linux command line for beginners](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview)
- [The Linux Directory Structure, Explained](https://www.howtogeek.com/117435/htg-explains-the-linux-directory-structure-explained/)
- [Unix / Linux - What is Shells?](https://www.tutorialspoint.com/unix/unix-what-is-shell.htm)
- [zsh](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH)
- [About systemd](https://www.infoworld.com/article/2832405/what-is-systemd-and-why-does-it-matter-to-linux-users.html)
- [About linux distributions](https://thebloggingpot.com/2018/05/23/different-linux-distributions-explained/)
- [RPM and YUM package management](https://developer.ibm.com/technologies/linux/tutorials/l-lpic1-102-5/)
- [File editing with vi](https://developer.ibm.com/technologies/linux/tutorials/l-lpic1-103-8/)

## Checklist

- **리눅스의 파이프 문자는 어떤 역할을 하나요?**
    
    파이프는 리다이렉션(stdout을 다른 곳으로 전달하는)의 한 종류로, 둘 이상의 명령을 결합하는데 사용한다.
    
    파이프를 기준으로 앞 명령의 출력(stdout)은 그 다음 명령의 입력이 된다.
    
    - 예시
        
        ```bash
        $ ll
        total 44
        drwxr-x--- 5 ubuntu ubuntu 4096 Nov 27 11:30 ./
        drwxr-xr-x 4 root   root   4096 Nov 22 17:18 ../
        -rw------- 1 ubuntu ubuntu  865 Nov 27 11:32 .bash_history
        -rw-r--r-- 1 ubuntu ubuntu  220 Jan  6  2022 .bash_logout
        -rw-r--r-- 1 ubuntu ubuntu 3771 Jan  6  2022 .bashrc
        drwx------ 2 ubuntu ubuntu 4096 Nov  1 03:38 .cache/
        -rw-r--r-- 1 ubuntu ubuntu  807 Jan  6  2022 .profile
        drwx------ 2 ubuntu ubuntu 4096 Nov  1 03:36 .ssh/
        -rw-r--r-- 1 ubuntu ubuntu    0 Nov 22 17:18 .sudo_as_admin_successful
        drwxr-xr-x 2 ubuntu ubuntu 4096 Nov 27 11:30 .vim/
        -rw------- 1 ubuntu ubuntu 4222 Nov 22 17:51 .viminfo
        
        $ ll | grep ^d
        drwxr-x--- 5 ubuntu ubuntu 4096 Nov 27 11:30 ./
        drwxr-xr-x 4 root   root   4096 Nov 22 17:18 ../
        drwx------ 2 ubuntu ubuntu 4096 Nov  1 03:38 .cache/
        drwx------ 2 ubuntu ubuntu 4096 Nov  1 03:36 .ssh/
        drwxr-xr-x 2 ubuntu ubuntu 4096 Nov 27 11:30 .vim/
        
        $ ll | grep ^d | wc -l
        5
        ```
        
    
- **리눅스의 셸은 어떤 역할을 하나요? bash와 zsh는 어떻게 다른가요?**
    - 쉘은 Unix 시스템, 커널에 대한 인터페이스를 제공한다.
        
        사용자의 입력을 수집하고 해당 입력을 기반으로 프로그램을 실행하고, 실행이 완료되면 해당 프로그램의 출력이 표시된다.
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/573b8c1c-be37-434b-9e90-f8fc23a4202c/Untitled.png)
        
    - bash보다 zsh에 약간의 기능이 추가되었다.
        - cd + tab 자동 완성 디테일 (숨긴 폴더는 안나옴)
        
    - 쉘은 다음과 같이 종류가 크게 두가지로 나누어져 있다.
        - **Bourne shell (%)**
            - Bourne shell (sh)
            - Korn shell (ksh)
            - Bourne Again shell (bash)
            - POSIX shell (sh)
        - **C shell ($)**
            - C shell (csh)
            - TENEX/TOPS C shell (tcsh)
    
    - bash와 zsh 모두 **Bourne shell**의 ****확장된 버전이다.
- **리눅스의 권한 체계는 어떻게 이루어져 있나요?**
    
    ll은 ls -alF의 alias로, (~/.bashrc 파일에 기본적으로 설정되어 있다) 다음과 같이 현재 디렉토리의 전체 파일의 모든 정보를 확인할 수 있다.
    
    ```bash
    $ ll
    total 8
    drwxrwxr-x 2 ubuntu ubuntu 4096 Nov 27 16:07 ./
    drwxr-x--- 6 ubuntu ubuntu 4096 Nov 27 16:06 ../
    -rw-r--r-- 1 root   root      0 Nov 27 16:07 root.file
    -rw-rw-r-- 1 ubuntu ubuntu    0 Nov 27 16:07 ubuntu.file
    ```
    
    각각이 의미하는 것은 다음과 같다.
    
    ![https://blog.kakaocdn.net/dn/QZB01/btqJBdiAZ3u/KnPuTZNPbyyaJfqdYoXQQ1/img.jpg](https://blog.kakaocdn.net/dn/QZB01/btqJBdiAZ3u/KnPuTZNPbyyaJfqdYoXQQ1/img.jpg)
    
    가장 앞 부분은 크게 4 섹션으로 나눌 수 있다.
    
    d / rwx / rwx / rwx
    
    - d는 directory를 의미하고, 이 자리가 -로 되어 있으면 file임을 의미한다.
    - rwx 3자리는 각각 소유자, 그룹, 그 외 사용자를 의미한다.
    
    예를 들어
    
    ```bash
    -rw-r--r-- 1 root   root      0 Nov 27 16:07 root.file
    -rw-rw-r-- 1 ubuntu ubuntu    0 Nov 27 16:07 ubuntu.file
    ```
    
    - `root.file`은 root만 write 권한이 부여 되어 있고, root 그룹 멤버와 나머지 사용자들은 read 권한만 있는 것을 확인할 수 있다.
    - `ubuntu.file`은 ubuntu 유저와 ubuntu 그룹에 대해서 write 권한이 있고, 그 외 유저는 read만 가능하다.
    
    > **그룹에 속한 유저가 궁금하다면?**
    > 
    > 
    > `groups` 명령어는 현재 자신이 접속한 유저가 속한 그룹을 알려준다.
    > 
    > `groups <사용자>` 는 해당 사용자가 속한 그룹을 알려준다.
    > 
    
- **프로세스와 쓰레드는 무엇인가요?**
    
    [참고](https://velog.io/@raejoonee/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80-%EC%8A%A4%EB%A0%88%EB%93%9C%EC%9D%98-%EC%B0%A8%EC%9D%B4)
    
    - 프로세스는 프로그램의 작업 단위이고, 운영체제에서 직접 관리하고, 메모리에 올라간다.
    - 스레드는 프로세스에서 만들어지는 실행 흐름이고, 하나의 프로세스는 하나 이상의 스레드를 가진다.
    
    - ps
        
        > e, A : 전체 프로세스에 대한 정보
        > 
        > 
        > L : 스레드 정보인 LWP, NLWP 출력
        > 
        > f : 모든 정보 보기
        > 
        
        ```bash
        $ ps -ALf
        UID          PID    PPID     LWP  C NLWP STIME TTY          TIME CMD
        root           1       0       1  0    1 Nov27 ?        00:00:09 /sbin/init
        root           2       0       2  0    1 Nov27 ?        00:00:00 [kthreadd]
        ...
        ```
        
    
- **현재 실행되고 있는 프로세스들 중 PID가 1인 프로세스는 어떤 역할을 할까요? init과 systemd는 무엇이고 어떻게 다른가요?**
    - **linux server의 부팅 과정은 다음과 같다.**
        1. Hardware
        2. Bootloader
        3. kurnel 로드
        4. INIT
        
        [https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb76nWu%2FbtrnXY6h11a%2F3HzpNDIpRvckGeBzIk0dS1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb76nWu%2FbtrnXY6h11a%2F3HzpNDIpRvckGeBzIk0dS1%2Fimg.png)
        
    
    가장 마지막 단계 즉, INIT은 커널에 의해 시작이 되며, 시스템이 실행될 때 까지 계속 실행되는 프로세스이다.
    
    (커널이 INIT을 실행하지 못하면 [커널 패닉](https://ko.wikipedia.org/wiki/%EC%BB%A4%EB%84%90_%ED%8C%A8%EB%8B%89)이 발생된다고 한다.)
    
    - INIT은 주로 다음과 같은 역할을 한다.
        - 파일 시스템 초기화
        - 네트워크 및 기본적인 시스템 동작을 위한 시스템 서비스 실행
        - 백그라운드 서비스 실행
    
    - 다음과 같이 INIT 프로세스는(systemd)는 모든 프로세스의 부모 프로세스 임을 확인 할 수 있다.
        - 실습 환경 : ubuntu 22.04
            
            ```bash
            $ pstree
            systemd─┬─ModemManager───2*[{ModemManager}]
                    ├─acpid
                    ├─2*[agetty]
                    ├─amazon-ssm-agen─┬─ssm-agent-worke───8*[{ssm-agent-worke}]
                    │                 └─8*[{amazon-ssm-agen}]
                    ├─chronyd───chronyd
                    ├─cron
                    ├─dbus-daemon
                    ├─irqbalance───{irqbalance}
                    ├─multipathd───6*[{multipathd}]
                    ├─networkd-dispat
                    ├─polkitd───2*[{polkitd}]
                    ├─rsyslogd───3*[{rsyslogd}]
                    ├─snapd───8*[{snapd}]
                    ├─sshd───sshd───sshd─┬─bash───pstree
                    │                    └─bash───bash
                    ├─systemd───(sd-pam)
                    ├─systemd-journal
                    ├─systemd-logind
                    ├─systemd-network
                    ├─systemd-resolve
                    ├─systemd-udevd
                    ├─udisksd───4*[{udisksd}]
                    └─unattended-upgr───{unattended-upgr}
            ```
            
        
    - 리눅스에는 다양한 Init 프로그램이 있다.
        - **SysV**, Upstart, Runit, **Systemd**
            
            (질문에서의 init은 SysV)
            
    
    - `init`
        - 가장 오래된 Init 프로그램
        - **service** : init.d에서 서비스 관리 시 사용하는 명령 (클라이언트)
        - 직렬 방식으로 다른 프로세스를 실행 하기 때문에, 비교적 부팅 속도가 늦어진다.
        - 초기화 명령이 쉘스크립트로 작성되어 있다.
    - `systemd`
        - **[sytemctl](https://www.lesstif.com/system-admin/systemd-system-daemon-systemctl-24445064.html)** : systemd의 클라이언트
        - 프로세스 관리가 더욱 **상태 지향적**이다. 
        systemctl status 명령어로 active, inactive, failed 등 서비스(데몬) 상태를 확인할 수 있다.
        - 초기화 명령이 systemd에서 정의한 특정 구문으로 실행되고, 각각 **unit** 단위로 관리한다
        - 실행 중 필요한 메모리가 init 보다 작다.
        - cron, snapshot 기능 등을 추자거으로 지원한다.
    - **주요 차이점**
        - 다른 프로세스 실행 : init은 직렬 방식 / systemd는 병렬 방식
        - systemd는 부팅 시 다른 프로세스를 실행시켜 주는 것 뿐만 아니라 더욱 다양한 기능이 있어서 서비스 관리에 init 보다 용이하다.
        - systemd가 메모리 사용량이 더 적다.
        
- **파일시스템이란 무엇일까요? 어떤 것이 있을까요? 지금 다루는 운영체제는 어떤 파일시스템을 쓰고 있나요?**
    - 파일 시스템은
        
        운영체제와 모든 데이터, 프로그램의 **저장과 접근을 위한 기법**을 제공한다.
        
        시스템 내의 모든 파일에 관한 정보를 제공하는 **계층적 디렉터리 구조**이고, 파일 및 파일의 메타데이터, 디렉터리 정보 등을 관리한다.
        
    
    - 사용 중인 파일 시스템 확인
        
        (ubuntu 기준)
        
        ```bash
        # df 명령어로 디스크 확인, -t 옵션으로 타입까지 확인
        $ df -t
        Filesystem     Type  1K-blocks    Used Available Use% Mounted on
        /dev/root      **ext4**    7941576 1582804   6342388  20% /
        tmpfs          tmpfs   2007544       0   2007544   0% /dev/shm
        tmpfs          tmpfs    803020     836    802184   1% /run
        tmpfs          tmpfs      5120       0      5120   0% /run/lock
        /dev/xvda15    vfat     106858    5329    101529   5% /boot/efi
        tmpfs          tmpfs    401508       4    401504   1% /run/user/1000
        ```
        
    
    - **type**
        - ext4 : 최대 16TB 까지 지원됨 ext2,3 보다 개선된 성능을 갖고 있다고 함.
        - xfs : RedHat 계열 **7~** (centos …)
- **리눅스의 배포판이란 무엇일까요? 여러 가지 배포판들은 어떻게 생겨났을까요?**
    - 리눅스란 **커널**을 의미한다.
        
        리눅스 배포판이란 리눅스라는 커널을 사용하는 OS를 뜻한다.
        
    
    - [리눅스 배포판의 계보도](https://upload.wikimedia.org/wikipedia/commons/1/1b/Linux_Distribution_Timeline.svg)
    
    - 리눅스는 처음에 [소스 코드](https://ko.wikipedia.org/wiki/%EC%86%8C%EC%8A%A4_%EC%BD%94%EB%93%9C)로만 배포되었다가 나중에 다운로드 가능 [플로피 디스크](https://ko.wikipedia.org/wiki/%ED%94%8C%EB%A1%9C%ED%94%BC_%EB%94%94%EC%8A%A4%ED%81%AC) 이미지 한 쌍, 즉 하나는 리눅스 커널 자체를 포함한 부팅 가능한 이미지, 다른 하나는 파일 시스템 설정을 위한 [GNU](https://ko.wikipedia.org/wiki/GNU)
     유틸리티 및 도구들이 모여있는 이미지로 배포되었다. 
    특히 사용 가능한 소프트웨어의 양이 늘어나는 동안 설치 절차가 복잡했기 때문에 배포판들이 이를 단순케 하기 위해 우후죽순 생겨나기 시작했다.
- **리눅스의 패키지 시스템이란 무엇일까요? 이러한 시스템이 생긴 이유는 무엇일까요? deb과 rpm은 어떤 차이가 있을까요? RPM이 있는데 yum과 같은 시스템이 나온 이유는 무엇일까요?**
    - 패키지는
        
        리눅스 시스템에서 소프트웨어를 실행하는데 필요한 파일들(실행 파일, 설정 파일, 라이브러리 등)이 담겨 있는 설치 파일 묶음
        
    - 패키지 시스템은
        
        패키지(소프트웨어) 설치, 삭제 업그레이드 등의 매니징을 위한 시스템
        
    
    - deb, rpm의 차이점
        - Debian 계열 (Debian, Ubuntu 등) : .deb 파일
        - RedHat 계열 (RedHat, Fedora, CentOS) : .rpm 파일
    
    - yum이 나온 이유
        - RPM 패키지는 컴파일되어 설치한 실행파일, 설정파일, 라이브러리 등을 하나로 묶어 놓은 파일
        - YUM은 RPM과 다르게 의존성 문제를 해결해 줄 수 있는 추가 기능이 있다. 
        A 패키지를 설치하기전에 필요한 B 패키지까지 모두 한번에 설치가 가능하며, 의존도를 자동으로 찾고 알아서 설치해준다. 대
- **vi는 어떤 에디터인가요? vi와 vim은 어떻게 다를까요? vi는 왜 모든 리눅스의 기본 에디터가 되었을까요?**
    - vi란 문서를 편집하는 에디터이다.
        
        vi라는 이름은 한줄씩 편집하는 라인 에디터가 아니라 한 화면에서 편집할 수 있는 Visual Editor에서 그 이름이 유래 되었다고 한다.
        
    
    - vi는 ed에서 파생되어 자유 소프트웨어가 아니었기 때문에 [AT&T](https://namu.wiki/w/AT%26T)의 라이선스 없이는 코드 수정이 불가능했다. 따라서 vi를 오픈소스화한 여러 vi의 복제판들이 등장했는데, 그 중 하나가 vim이었다.
    
    - vim 안에는 vi 의 기능들도 다 포함되어 있기에, 사실사 이 둘의 차이는 vim의 추가 기능이다.
    
    - vim은 유닉스의 표준 규격인 POSIX에 포함되어 있고, 우리는 대부분 POSIX 인증을 받은 OS를 사용하기 때문에 어디서나 사용할 수 있게된 것이다.

## Quest

- 인스턴스 생성
    - t3.nano 등급으로 EC2 인스턴스를 생성해 봅시다! Amazon Linux 2, Ubuntu 두 가지를 각각 생성해 봅니다.
    - EC2 생성 과정에서 .pem 파일이 하나 생기는데, 이는 저에게 슬랙을 통해 공유해 주시면 됩니다.
    - 세 배포판은 어떻게 다른가요?
- 리눅스 연습
    - Amazon Linux 2 인스턴스에서 위의 Topics의 기본 커맨드를 연습해 봅니다.
    - 리눅스의 기본 디렉토리들에 어떤 정보들이 있는지 둘러 봅니다.
    - zsh를 설치하고 `.zshrc` 파일을 포함해 여러 가지 설정을 해 봅니다.
    - Topics의 환경변수나 퍼미션 관련 커맨드를 연습해 봅니다.
    - 현재 실행되고 있는 프로세스들과 마운트 된 파일시스템들을 확인해 봅니다.
    - vi를 열어 여러 가지 기본 명령과 간단한 편집 방법을 연습해 봅니다.
- 생성한 인스턴스 중 Ubuntu는 완전히 종료(Terminate)하고, Amazon Linux 2는 일단 꺼둡니다.

## Advanced

- **리눅스 외의 POSIX 호환 운영체제에는 어떤 것들이 있을까요? 그러한 운영체제들은 어떤 용도로 쓰일까요?**
    - POSIX란?
        - **UNIX 계열 운영체제 간의 이식성을 높이기 위해 1980년대 후반 POSIX(Portable operating system interface, ‘파직스’) 표준이 탄생**
        - 한마디로 정리하자면 
        **서로 다른 UNIX OS 의 공통 API 를 정리하여 이식성이 높은 유닉스 응용 프로그램을 개발하기 위한 목적으로 IEEE 가 책정한 앱 인터페이스 규격**
    
    - [호환 운영 체제](https://en.wikipedia.org/wiki/POSIX#Formerly_POSIX-certified)
- **윈도우를 제외하고, 최근에 발표된 운영체제들 중 POSIX에 호환되지 않는 운영체제도 있을까요?**
