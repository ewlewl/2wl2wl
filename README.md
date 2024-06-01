# 리눅스 명령어(top, ps, jobs, kill) 가이드

## 목차
1. [top 명령어](#top-명령어)
2. [ps 명령어](#ps-명령어)
3. [jobs 명령어](#jobs-명령어)
4. [kill 명령어](#kill-명령어)
5. [참고자료](#참고자료)

## top 명령어

**'top'** 명령어는 리눅스 시스템의 CPU 사용량, 메모리 사용량 등 전반적인 상황을 실시간으로 모니터링할 수 있는 명령어이다.

Windows의 작업관리자와 비슷한 기능을 제공하기 때문에 기본적으로 top명령어를 사용하면 데몬 별 CPU, RAN 사용량 등 3초마다 실시간으로 모니터링을 통해 서버 상태를 확인할 수 있다.

#### top 명령어 기본 사용법

```bash
$ top
```
[그림1] top 명령어 사용 예시

![top](https://github.com/ewlewl/2wl2wl/assets/166885748/25cee73d-82c0-494f-b18d-807b7aa001a9)

터미널에서 top 명령어를 입력하면 [그림1]과 같이 출력된다.

CPU사용량, 메모리 사용량 등을 나타내주며 실시간으로 정보가 업데이트된다.

[그림1]의 세부 단락 필드의 의미는 아래와 같다.

|프로세스 상세 정보 | 설명 |
| ------------------ | :-------|
| PID | 프로세스 ID (PID) |
| USER | 프로세스를 실행시킨 사용자 ID |
| PRI | 프로세스의 우선순위 (priority) |
| NI | NICE 값. 일의 nice value값, 마이너스를 가지는 nice value는 우선순위가 높음 |
| VIRT | 가상 메모리의 사용량(SWAP+RES) |
| RES | 현재 페이지가 상주하고 있는 크기(Resident Size) |
| SHR | 분할된 페이지, 프로세스에 의해 사용된 메모리를 나눈 메모리의 총합 |
| S | 프로세스의 상태 [S(sleeping), R(running), W(swapped out process), Z(zombies)] |
| %CPU | 프로세스가 사용하는 CPU의 사용율 |
| %MEN | 프로세스가 사용하는 메모리의 사용율 |
| COMMAND | 실행된 명령어 |

---

top 명령어에서 중요한것은 부하가 발생하는 프로세스가 상단에 위치하므로, 상단을 주의깊게 확인할 필요가 있다. [그림1]에서 상단에 위치한 5줄의 라인은 시스템 전반적인 정보 출력을 의미한다.

**1번째 라인**

```
top - 11:16:47 up 4 days, 1 user, load average: 0.15, 0.25, 0.17
```
- 11:16:47 up 4 days, 18:58: "11:16:47"은 현재 시간을 의미하며, up 이후로 시스템 재시작 한 이후 시간을 의미한다.
- 1 user: 현재 시스템에 로그인 한 사용자의 총합
- load average: 시스템 부하률을 의미하며, 3개로 보여지는 것은 현재 값이며, 오른쪽으로 갈수록 과거의 값이다.

**2번째 라인**

```
Tasks: 73 total, 1 running, 72 sleeping, 0 stopped, 0 zombie
```

- Tasks: 시스템에서 실행중인 프로세스
- total: 현재 시스템에서 동작중인 총 프로세스의 수
- running: 현재 실행중인 프로세스의 수
- sleeping: 메모리에 적재된 예비 프로세스
- stopped: 현재 중지된 프로세스의 수
- zombie: 정상적으로 종료되지 않은 시스템에 좋지 않은 영향을 끼치는 프로세스

**3번째 라인**

```
Cpu(s): 0.0%us, 0.0%sy, 0.0%ni,100.0%id, 0.0%wa, 0.0%hi, 0.0%si, 0.0%st
```

- CPU: 이용정보
- us: 이용중인 CPU 이용률
- sy: 시스템(kernel)이 이용하는 CPU 점유율
- ni: NICE 정책에 의한 CPU 점유율
- id: CPU 미사용
- wa: 입출력 대기상태
- hi: 하드웨어 인터럽트 수
- si: 소프트웨어 인터럽트 수

**4번째 라인**

```
Mem:   1502404k total,   298388k used,  1204016k free,    75628k buffers
```

물리적인 메모리 사용량을 보여줍니다.
- total: 물리적인 메모리 총 용량
- used: 현재 시스템에서 사용하고 있는 메모리 용량
- free: 남아있는 메모리 용량(메모리 총 용량에서 사용중인 메모리 양을 뺸 값)
- buffers: 프로세스 사이에서 공유될 수 있는 메모리 용량

**5번째 라인**

```
Swap:  2047996k total,        0k used,  2047996k free,   116540k cached
```

- 물리적인 메모리가 전부 사용중일 때 HDD의 일부를 메모리처럼 추가 사용하도록 한다.

### top 명령어 예시
1. 기본 명령어로 3초마다 출력 갱신
```bash
$ top
```

2. 화면 업데이트 시간 변경
```bash
$ top -d [n초]
```
- e.g. top -d 1: 1초마다 출력 갱신

3. top 실행 전 옵션
- 순간의 정보를 확인하려면 -b 옵션 추가(batch 모드)
```bash
$ top -b
```
- top 실행 주기 설정(반복 횟수)
```bash
$ top -n
```

4. top 실행 후 명령어
- shift + p: CPU 사용률 내림차순




## ps 명령어

**ps(프로세스 상태)** 는시스템에서 실행 중인 프로세스 선택과 관련된 정보를 보기 위한 기본 Unix/Linux 유틸리티이다. 이 정보는/proc 파일 시스템, 특히 프로세스 모니터링 하에서 시스템 관리를 위한 중요한 유틸리티 중 하나이며 Linux 시스템에서 무슨 일이 일어나고 있는지 이해 하는 데 도움이 된다.

ps는 각 정보 열의 의미를 나타내는 제목 줄이 포함된 출력을 생성한다.

#### ps 명령어의 기본 사용법

리눅스 운영체제는 다양한 프로세스를 동시에 실행할 수 있다. 이러한 프로세스들을 확인하고 관리하기 위해서는 ps 명령어를 사용할 수 있다. ps 명령어는 현재 실행 중인 모든 프로세스의 정보를 보여주는 유용한 도구이다.

```bash
$ ps [option]
```

#### ps 명령어 출력 항목

| 출력항목                     | 내용 |
| --------------------------- | :-------------------- |
| USER (BSD) / UID (System V) | 프로세스 소유자의 이름 |
| PID | 프로세스의 식별 번호 |
| PPID | 부모 프로세스의 PID |
| %CPU | CPU 사용 비율의 추정치 (BSD) |
| %MEM | Memory 사용 지율의 추정치 (BSD) |
| VSZ | K 단위 또는 페이지 단위의 가상 메모리 사용량 |
| RSS | 실제 메모리 사용량 |
| TTY | 프로세스와 연결된 터미널 |
| S (System V) \ STAT (BSD) | 현재 프로세스의 상태 코드 |
| TIME | 총 CPU 사용 시간 |
| COMMAND | 프로세스의 실행 명령행 |
| STIME | 프로세스가 시작된 시간 혹은 날짜 |
| C (System V) / CP (BSD) | 짧은 기간 동안의 CPU 사용률 |
| F | 플래그 |
| PRI | 실제 실행 우선순위 |
| NI | nice 우선순위 번호 |

#### PS 명령어 옵션

| 옵션        | 설명                                                                                           |
| ---------- | :----------------------------------------------------------------------------------------------|
| -A         | 모든 프로세스를 출력 |
| -a         | 세션 리더(일반적으로 로그인 쉘)을 제외하고, 데몬 프로세스처럼 터미널에 종속되지 않은 모든 프로세를 출력 |
| -e         | 커널 프로세스를 제외한 모든 프로세스를 출력. (즉, -e 옵션이 없다면 ps 명령어는 현재 사용자(shell)이 실행 중인 프로세스만 보여줌. |
| -f         | 풀 포맷으로 보여줌. System V 스타일로 출력해주어, UID, PID, PPID 등이 함께 표시됨 |
| -l         | 긴 포맷으로 보여줌. 프로세스의 정보를 길게 보여주는 옵션으로 우선순위와 관련된 PRI와 NI 값을 확인 가능 |
| -o [value] | 출력 포맷을 지정하는 옵션으로, [value]로 pid, tty, time, cmd 등을 지정 가능 |
| -M         | 64비트 프로세스들을 보여줌 |
| -m         | 프로세스들 뿐만 아니라 커널 스레드들도 함께 보여줌 |
| -P         | 특정 PID를 지정할 때 사용 |
| -r         | 현재 실행 중인 프로세서를 보여줌 |
| -u         | 특정 사용자의 프로세스 정보를 확인할 때 사용, 사용자를 지정하지 않으면 혅 ㅐ사용자를 기준으로 정보를 출력 |
| -x         | 로그인 상태에 있는 동안 아직 완료되지 않은 프로세스들을 보여줌. -x 옵션을 통해, 자신의 터미널이 없는 프로세서들을 확인할 수 있음.|

#### 사용 예

1. 모든 프로세스 보기
```bash
$ ps -e
```
2. 상세한 프로세스 정보 보기
```bash
$ ps -ef
```
3. 사용자별로 프로세스 보기
```bash
$ ps -u [사용자명]
```
4. 긴 형식으로 프로세스 정보 보기
```bash
$ ps -l
```

#### ps 명령어의 장단점

###### 장점

- ps 명령어를 사용하면 현재 실행 중인 모든 프로세스의 정보를 한 눈에 확인할 수 있다.
- 다양한 옵션을 사용하여 원하는 조겅네 따른 프로세스만 필터링 할 수 있다.

###### 단점

- ps 명령어는 현재 상태의 스냅샷을 보여주기 때문에 실시간으로 변하는 프로세스 상태를 확인하기에는 적합하지 않을 수 있다.
- 모든 프로세스를 보여준다는 점에서 출력 결과가 많을 수 있기 때문에 원하는 정보를 찾기에 다소 어려움이 있을 수 있다.

#### 결론

ps명령어는 리눅스 시스템에서 프로세스를 확인하고 관리하는 데 유용한 도구이다. 다양한 옵션을 사용하여 원하는 조건에 따른 프로세스만 필터링하거나 상세한 정보를 확인할 수 있다. 이를 통해 시스템의 프로세스 상태를 모니터링하고 필요한 조치를 취할 수 있다.

## jobs 명령어

jobs는 셸에서 실행중인 프로세스 목록을 확인하는 명령어이다. 특히 bg, fg명령어와 함께 셸에서 실행한 프로세스들을 백그라운드나 포그라운드로 전환하는 용도로 많이 사용한다.

```bash
$ jobs [options] [job]
```

jobs 명령어는 옵션이나 인자 없이 주로 사용한다. 단, 실행중인 작업이 없다면 아무것도 출력되지 않는다.

#### job 명령어 옵션

| 옵션 | 설명 |
| ----- | :----|
| -l | 프로세스 ID와 함께 job목록을 출력 |
| -n | 마지막으로 알림 이후 변경된 job만 출력 |
| -p | job의 프로세스 ID만 출력 |  
| -r | 실행 중인 job만 출력 |
| -s | 중지된 job만 출력 |

#### jobs로 출력되는 백그라운드 작업의 상태값

| 상태 | 설명 |
| ----- | :----|
| Running | 작업이 계속 진행중임 | 작업이 계속 진행중임 |
| Done | 작업이 완료되어 0을 반환 |
| Done(code) | 작업이 종료되었으며 0이 아닌 코드를 반환 |
| Stopped | 작업이 일시 중단 |
| Stopped(SIGTSTP) | SIGTSTP 시그널이 작업을 일시 중단 |
| Stopped(SIGSTOP) | SIGSTOP 시그널이 작업을 일시 중단 |
| Stopped(SIGTTIN) | SIGTTIN 시그널이 작업을 일시 중단 |
| Stopped(SIGTTOU) | SIGTTOU 시그널이 작업을 일시 중단 |

#### jobs 명령어 장단점

###### 장점
- 현재 쉘 세션에서 실행 중인 작업 목록을 간단하게 확인할 수 있다.

###### 단점
- 백그라운드에서 실행 중인 다른 쉘 세션의 작업 목록으 확인할 수 없으며, 자세한 정보를 제공하지 않는다.

## kill 명령어

리눅스 운영체제에서는 프로세스를 관리하는 여러 가지 방법을 제공한다. 그 중 하나는 kill 명령어를 사용하여 프로세스를 종료하는 것이다. kill 명령어는 프로세스에 시그널을 보내어 원하는 동작을 수행할 수 있다. 프로세스 종료 외에도 다른 신호를 보내어 프로세스의 동작을 제어할 수 있다.

#### kill 명령어의 기본 사용법

kill 명령어를 사용하기 위해서는 먼저 종료하려는 프로세스의 프로세스 ID(PID)를 알아야 한다. 일반적으로 ps명령어를 사용하여 실행 중인 프로세스 목록을 확인한 후, 종료하려는 프로세스의 PID를 찾아야 한다. 그 다음 kill 명령어를 사용하여 해당 PID의 프로세스를 종료할 수 있다.

기본적인 kill 명령어의 구문은 다음과 같다.

```bash
$ kill [옵션] [PID]
```
여기서 [PID]는 종료할 프로세스의 식별자인 프로세스ID를 나타낸다.

#### kill 명령어 옵션

| 옵션 | 의미 |
| ----- | :---|
| -s <signal> | 특정 시그널을 사용하여 프로세스 종료. 기본적으로 SIGTERM 시그널 사용됨 |
| -l, --list | 지원되는 시그널 목록을 출력함 |
| -q, --queue | 프로세스에 시그널을 보내는 대신 시그널을 대기열에 추가함 |
| -a, --all | 현재 사용자에 속한 모든 프로세스를 종료함 |

kill 명령어의 자세한 설명과 사용 가능한 옵션은 man kill 명령어를 통해 확인할 수 있다.

```bash
$ kill -q [PID]
```

kill 명령어의 간단한 사용방법은 kill 뒤에 -q 옵션으로 프로세스아이디(PID)를 지정하고 종료 신호(Signal)를 입력하는 것이 가장 일반적이다.

```bash
$ kill -l
```

![image](https://github.com/ewlewl/2wl2wl/assets/166885748/a130d359-e845-424b-ae12-a4465a367bd4)

kill 명령어에서 -l옵션을 사용하면 사용 가능한 모든 신호(Signal)를 확인할 수 있다.

| 번호 | 시그널 이름 | 시그널 | 의미 |
| ---- | :----------------: | :---------: | :---- |
| 1 | SIGHUP | HUP | Hangup, 로그아웃 등 접속이 끊을 때 발생하는 신호로 특정 실행 중인 프로그램이 사용하는 설정 파일을 변경시키고 변화된 내용을 적용할 때 사용 됨 |
| 2 | SIGNT | INT | 현재 작동죽인 프로그램의 동작을 멈출 때 사용되며, 일반적인 값은 Ctrl + C |
| 9 | SIGKILL | KILL | 프로그램을 무조건 종료할 경우 사용 됨 |
| 11 | SIGSEGV | SEGV | 잘못된 메모리 관리시 생기는 신호 |
| 15 | SIGTERM | TERM | 실행중인 프로그램을 정상적인 종료방법으로 프로그램을 종료하는 신호로 kill 명령에서 신호를 지정하지 않으면 이 신호를 사용하여 프로그램 종료 |
| 18 | SIGCONT | CONT | 중지 되어 있는 프로그램을 재실행 하는데 사용되는 신호 |
| 19 | SIGSTOP | STOP | 프로그램을 중지 하는데 사용되는 신호 |
| 20 | SIGTSTP | TSTP | 터미널에서 중지되어 있는 신호 |

신호는 번호, 신호이름으로 신호로 보낼 수 있다.
또한, 위 표에 옵션은 자주 사용되는 신호로 강제로 종료시킬 때 사용 되는 -9 -SIGKILL 등등 옵션을 주어 사용하면 된다.

#### kill 장단점

장점: 간단한 명령어로 빠르게 프로세스를 종료 할 수 있다. 다양한 옵션을 사용하여 프로세스 동작을 제어할 수 있다.

단점: 잘못 사용하면 의도치 않은 프로세스 종료가 발생할 수 있다. -SIGKILL 시그널을 사용하여 강제종료할 경우, 프로세스가 올바르게 정리되지 않을 수 있고, 데이터 손실 등의 문제가 발생할 수 있다.

#### 결론

Linux에서 kill 명령어를 사용하여 프로세스를 종료할 수 있다. 이를 통해 원하는 프로세스를 간편하게 제어하고 운영체제의 안정성을 유지할 수 있다. 하지만 kill 명령어 사용 시 주의가 필요하며, 해당 시그널에 대한 이해와 프로세스에 대한 충분한 정보를 확인한 후 사용해야 한다.

## 참고자료
#### top 명령어
1. <https://zzsza.github.io/development/2018/07/18/linux-top/>
2. <https://velog.io/@ljk0509/EC2-top-%EB%AA%85%EB%A0%B9%EC%96%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0>
3. <https://help.iwinv.kr/manual/read.html?idx=137>
#### ps 명령어
1. <https://blog.naver.com/tmk0429/222318530824>
2. <https://tigris-data-science.tistory.com/entry/Linux-ps-%EB%AA%85%EB%A0%B9%EC%96%B4>
3. <https://co-no.tistory.com/entry/Linux-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81%EC%9D%84-%EC%9C%84%ED%95%9C-%EC%9C%A0%EC%9A%A9%ED%95%9C-ps-%EB%AA%85%EB%A0%B9-%EC%98%88%EC%A0%9C>
4. <https://gr-st-dev.tistory.com/209>
#### jobs 명령어
1. <https://www.lainyzine.com/ko/article/linux-jobs-command-usage/>
2. <https://hbase.tistory.com/265>
#### kill 명령어
1. <https://server-talk.tistory.com/432>
2. <https://gr-st-dev.tistory.com/210>
