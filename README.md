# 리눅스 명령어(top, ps, jobs, kill) 가이드

## 목차
1. [top 명령어](#top-명령어)
2. [ps 명령어](#ps-명령어)
3. [jobs 명령어](#jobs-명령어)
4. [kill 명령어](#kill-명령어)
5. [참고자료](#참고자료)

## top 명령어

'top' 명령어는 리눅스 시스템의 CPU 사용량, 메모리 사용량 등 전반적인 상황을 실시간으로 모니터링할 수 있는 명령어이다.

Windows의 작업관리자와 비슷한 기능을 제공하기 때문에 기본적으로 top명령어를 사용하면 데몬 별 CPU, RAN 사용량 등 3초마다 실시간으로 모니터링을 통해 서버 상태를 확인할 수 있다.

### 명령어 실
```bash
$top
```
[그림1] top 명령어 사용 예시

![1](https://github.com/ewlewl/2wl2wl/assets/166885748/db1897af-983b-4794-b1e2-7b575cbf20e2)

터미널에서 top 명령어를 입력하면 [그림1]과 같이 출력된다.

CPU사용량, 메모리 사용량 등을 나타내주며 실시간으로 정보가 업데이트된다.

[그림1]의 세부 단락 필드의 의미는 아래와 같다.

---
-PID: 프로세스 ID (PID)
-USER: 프로세스를 실행시킨 사용자 ID

-PRI: 프로세스의 우선순위 (priority)

-NI: NICE 값. 일의 nice value값이다. 마이너스를 가지는 nice value는 우선순위가 높음.

-VIRT: 가상 메모리의 사용량(SWAP+RES)

-RES: 현재 페이지가 상주하고 있는 크기(Resident Size)

-SHR: 분할된 페이지, 프로세스에 의해 사용된 메모리를 나눈 메모리의 총합.

-S: 프로세스의 상태 [S(sleeping), R(running), W(swapped out process), Z(zombies)]

-%CPU: 프로세스가 사용하는 CPU의 사용율

-%MEN: 프로세스가 사용하는 메모리의 사용율

-COMMAND: 실행된 명령어

---

