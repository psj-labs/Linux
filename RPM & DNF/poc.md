# RPM & DNF 

## 개요

- 본 문서는 RHEL 계열 Linux 환경에서  
  RPM과 DNF를 이용한 패키지 관리 방법을 정리한 학습 문서입니다.
- rpm 명령어를 통해 패키지의 조회, 정보 확인 방식을 살펴보고  
  dnf 명령어를 통해 설치, 업데이트, 제거, 저장소 관리까지 실습합니다.

---

## RPM 개요

- RPM은 RedHat 사에서 제공하는 리눅스용 프로그램 배포 기술입니다.
- 프로그램의 설치, 검증, 삭제 등의 기능을 제공합니다.
- 단독 설치 시 의존성 문제를 직접 해결해야 합니다.

---

## RPM 패키지 조회 명령어

- RPM 패키지 질의 명령어는 다음과 같습니다.
```text
rpm -qa  
시스템에 설치된 모든 패키지 출력  

rpm -qi 패키지명  
패키지의 상세 정보 출력  

rpm -ql 패키지명  
패키지에 포함된 파일 목록 출력  

rpm -qf 파일명  
특정 파일이 포함된 패키지 확인  
```
---

## 설치된 패키지 전체 확인
```text
rpm -qa
```
![01](/RPM%20&%20DNF/imgs/01.png)

- 시스템에 설치된 모든 패키지 목록이 출력됩니다.

---

## 특정 패키지 상세 정보 확인

`python3-simpleline` 패키지의 상세 정보를 확인합니다.
```text
rpm -qi python3-simpleline
```
![02](/RPM%20&%20DNF/imgs/02.png)

- 패키지 버전, 설치 시간, 라이선스 정보를 확인할 수 있습니다.

---

## openssh-server 패키지 정보 확인
```text
rpm -qi openssh-server
```
![03](/RPM%20&%20DNF/imgs/03.png)

- SSH 서버 패키지의 상세 정보를 확인할 수 있습니다.

---

## RPM 패키지 설치 및 삭제

- RPM 설치 관련 옵션은 다음과 같습니다.

-i : 설치  
-U : 업그레이드 (미설치 시 install과 동일)  
-F : 설치된 경우만 업그레이드  
-v : 설치 과정 출력  
-h : 진행 상태 출력  

- 패키지 삭제 명령어는 다음과 같습니다.
```text
rpm -e 패키지명  
```
- 의존성 문제로 인해 실무에서는 사용 빈도가 낮습니다.

---

## DNF 개요

- DNF는 RPM 기반 패키지 관리 도구입니다.
- 의존성을 자동으로 해결합니다.
- 현재 RHEL 계열 배포판의 표준 패키지 관리자입니다.

---

## DNF 패키지 목록 확인
```text
dnf list
```
- 패키지 조회 옵션은 다음과 같습니다.
```text
installed : 설치된 패키지  
updates : 업데이트 가능한 패키지  
available : 설치 가능한 패키지  
패키지명 : 특정 패키지 상태 확인  
```
---

## Repository 목록 확인
```text
dnf repolist
```
![04](/RPM%20&%20DNF/imgs/04.png)

- 시스템에 등록된 저장소 목록이 출력됩니다.

---

## 패키지 검색

sql 문자열이 포함된 패키지를 검색합니다.
```text
dnf search sql
```
![06](/RPM%20&%20DNF/imgs/06.png)

- 패키지명 또는 설명에 문자열이 포함된 패키지가 출력됩니다.

---

## 패키지 파일 목록 확인

iproute 패키지에 포함된 파일을 확인합니다.
```text
dnf repoquery -l iproute
```
![07](/RPM%20&%20DNF/imgs/07.png)

---

## 특정 파일 소속 패키지 확인
```text
dnf provides /usr/sbin/ip
```
![08](/RPM%20&%20DNF/imgs/08.png)

- 해당 파일이 어떤 패키지에 포함되어 있는지 확인할 수 있습니다.

---

## 패키지 설치
```text
dnf install -y python3
```
![09](/RPM%20&%20DNF/imgs/09.png)

- `-y` 옵션은 모든 질문에 자동으로 yes를 응답합니다.

---

## 패키지 업데이트
```text
dnf update -y python3
```
![10](/RPM%20&%20DNF/imgs/10.png)

- 최신 버전이 이미 설치된 경우 업데이트가 수행되지 않습니다.

---

## 패키지 제거
```text
dnf remove python3
```
![11](/RPM%20&%20DNF/imgs/11.png)
![12](/RPM%20&%20DNF/imgs/12.png)

- 패키지와 관련된 의존성도 함께 제거될 수 있습니다.

---

## 캐시 초기화
```text
dnf clean all
```
![13](/RPM%20&%20DNF/imgs/13.png)

- 패키지 메타데이터와 캐시를 정리합니다.
- 필자는 실습에서 실행하지 않았습니다.

---

## 그룹 패키지 개요

- 그룹 패키지는 여러 패키지를 묶은 논리적 단위입니다.
- Environment 그룹과 Package 그룹으로 나뉩니다.
- Environment 그룹은 OS 구성의 기본 단위입니다.

---

## 그룹 목록 확인
```text
dnf group list
```
![14](/RPM%20&%20DNF/imgs/14.png)

---

## 그룹 패키지 설치

- Server with GUI 그룹을 설치합니다.
```text
dnf group install -y "Server with GUI"
```
![15](/RPM%20&%20DNF/imgs/15.png)

---

## Repository 관리

- 저장소 목록 확인
```text
dnf repolist all
```
![16](/RPM%20&%20DNF/imgs/16.png)

---

## Repository 활성화
```text
dnf config-manager --set-enabled ha
```
![17](/RPM%20&%20DNF/imgs/17.png)

- 지정한 저장소가 활성화됩니다.

---

## Repository 비활성화
```text
dnf config-manager --set-disabled ha
```
![18](/RPM%20&%20DNF/imgs/18.png)

- 저장소가 다시 비활성화됩니다.

---

## 정리

- RPM은 패키지 단위 관리 도구입니다.
- DNF는 RPM 기반의 의존성 해결 패키지 관리자입니다.
- 실무에서는 RPM보다 DNF 사용이 권장됩니다.
- Repository 관리는 패키지 설치 범위를 결정합니다.
- 그룹 패키지는 대규모 환경 구성에 유용합니다.
