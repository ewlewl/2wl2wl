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

**명령어 실행**
```bash
$top
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

## ps 옵션

사용자와 관련된 프로세스만 출력합니다.

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

#### System V 계열

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

