# Linux 서비스(Service) 및 데몬(Daemon) 관리

## 개요

- 본 문서는 Linux 환경에서 서비스(Service)와 데몬(Daemon)의 개념과  
  systemctl 명령어를 이용한 서비스 관리 방법을 정리한 학습 문서입니다.
- 서비스의 활성화 여부와 실행 상태의 차이를 이해하고  
  enable, disable, start, stop 동작을 실습을 통해 확인합니다.
- 또한 target 개념을 통해  
  시스템 부팅 모드(CLI / GUI)를 전환하는 방법을 함께 정리합니다.

---

## 서비스와 데몬 개념

- 서비스와 데몬은 구분하지 않아도 됩니다.
- 이 둘은 사용자의 요청 시점이 아니라  
  임의의 시점(보통 시스템 부팅 시점)에  
  background process로 시작되어 사용자나 프로세스에 서비스를 제공하는 프로그램입니다.

---

## 서비스 동작 방식

### Stand Alone 방식

- 서비스가 스스로 listen 합니다.
- 항상 메모리에 상주합니다.
- 서비스 요청에 즉시 대응할 수 있습니다.
- 서비스 요청이 매우 드물거나 idle 한 경우 메모리를 낭비합니다.

### Super Daemon 방식

- 서비스가 직접 listen 하지 않습니다.
- 메모리에 상주하지 않습니다.
- 서비스 요청이 있을 때 xinetd에 의해 호출됩니다.
- 현재는 거의 사용되지 않는 방식입니다.

---

## 서비스 상태 조회

- 모든 서비스의 등록 상태를 확인하는 명령어는 다음과 같습니다.
```text
systemctl list-unit-files
```
![01](/Service%20관리/imgs/01.png)

- 여러 서비스들의 상태를 확인할 수 있습니다.
- 주요 상태는 다음과 같습니다.

enabled : 부팅 시 자동 실행  
disabled : 부팅 시 자동 실행 안 함  
static : enable 불가, 종속 유닛  
generated : 종속 서비스로만 운영  

---

## 서비스 활성화 여부 확인

- 특정 서비스의 enable 상태를 확인하는 명령어는 다음과 같습니다.
```text
systemctl is-enabled crond.service
```
![02](/Service%20관리/imgs/02.png)

- enabled로 출력됩니다.
- 이는 crond.service가  
  시스템 부팅 시 자동으로 실행되도록 설정되어 있다는 의미입니다.

---

## 서비스 실행 상태 확인

- 서비스가 현재 실행 중인지 확인하는 명령어는 다음과 같습니다.
```text
systemctl is-active crond.service
```
![03](/Service%20관리/imgs/03.png)

- active로 출력됩니다.
- 이는 현재 이 순간에 서비스가 실제로 실행 중이라는 의미입니다.

---

## 서비스 등록 해제

- 서비스의 자동 실행을 해제하는 명령어는 다음과 같습니다.
```text
systemctl disable crond.service
```
![04](/Service%20관리/imgs/04.png)

- 서비스 관련 파일이 제거됩니다.
- 재부팅 시 자동 실행되지 않습니다.


## disable 상태 확인
```text
systemctl is-enabled crond.service
```
- disabled로 출력됩니다.
- 이는 부팅 시점 기준 설정입니다.
```text
systemctl is-active crond.service
```
- active로 출력됩니다.
- 현재 세션에서는 실행 중입니다.

---

## 서비스 재등록

- 다시 서비스 자동 실행을 설정합니다.
```text
systemctl enable crond.service
```
![05](/Service%20관리/imgs/05.png)
```text
systemctl is-enabled crond.service
```
- enabled 상태임을 확인할 수 있습니다.

---

## 서비스 실행 및 종료

- 서비스 실행 관련 명령어는 다음과 같습니다.
```text
systemctl start 서비스명  
systemctl stop 서비스명  
systemctl restart 서비스명  
```
- 해당 동작은 현재 세션에만 적용됩니다.

---

## 서비스 중지 실습
```text
systemctl stop crond.service
```
![06](/Service%20관리/imgs/06.png)
```text
systemctl is-enabled crond.service  
enabled 상태입니다.
```
```text
systemctl is-active crond.service  
inactive 상태입니다.
```
---

## 서비스 재실행
```text
systemctl start crond.service
```
![07](/Service%20관리/imgs/07.png)
```text
systemctl is-active crond.service  
active 상태입니다.
```
---

## systemctl 추가 옵션

try-restart : 실행 중일 때만 재시작  
reload : 설정 재적용  
status : 상태 확인  
disable --now : disable과 stop 동시 수행  

---

## disable과 stop 동시 수행
```text
systemctl disable --now crond.service
```
![08](/Service%20관리/imgs/08.png)
```text
systemctl is-enabled crond.service  
disabled 상태입니다.
```
```text
systemctl is-active crond.service  
inactive 상태입니다.
```
---

## start --now 옵션
```text
systemctl start --now crond.service
```
![09](/Service%20관리/imgs/09.png)

- 서비스는 실행되지만 enable 상태는 아닙니다.
- 재부팅 시 자동 실행되지 않습니다.

---

## ntsysv 명령어

- 메뉴 기반으로 서비스 관리를 수행할 수 있습니다.
```text
ntsysv
```
![10](/Service%20관리/imgs/10.png)

---

## target 확인
```text
systemctl list-units --type target --all
```
![11](/Service%20관리/imgs/11.png)

- target은 기존 init의 run level과 동일한 개념입니다.
- 더 다양한 시스템 모드를 제공합니다.

---

## 주요 target 종류

multi-user.target : CLI 모드  
graphical.target : GUI 모드  
rescue.target : 단일 사용자 모드  
emergency.target : 응급 복구 모드  

---

## 현재 target 확인
```text
systemctl get-default
```
![12](/Service%20관리/imgs/12.png)

- 현재는 graphical.target 상태입니다.

---

## CLI 모드로 변경
```text
systemctl set-default multi-user.target
```
![13](/Service%20관리/imgs/13.png)
```text
systemctl get-default
```
- multi-user.target으로 변경되었습니다.

---

## CLI 모드 부팅 확인

![14](/Service%20관리/imgs/14.png)

---

## GUI 모드로 복구
```text
systemctl set-default graphical.target
```
![15](/Service%20관리/imgs/15.png)

![16](/Service%20관리/imgs/16.png)

---

## 정리

- 서비스와 데몬은 background process로 동작합니다.
- enable은 부팅 시 자동 실행 여부입니다.
- start와 stop은 현재 세션 기준 동작입니다.
- target은 시스템 부팅 모드를 결정합니다.
- CLI와 GUI 모드는 자유롭게 전환할 수 있습니다.
