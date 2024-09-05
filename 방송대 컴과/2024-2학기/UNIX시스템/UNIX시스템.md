# 1강. 리눅스 소개

## 유닉스와 리눅스

- 다중 사용자, 다중 작업 지원, 뛰어난 이식성, 네트워킹 기능, 셸 스크립트, 단순하고 모듈화된 설계 등
- 리눅스는 UNIX의 무료 공개 버전 - 초기에 리눅스는 PC용 운영체제로 개발되었음
- 최초의 UNIX
    - Multics - 1969년 assembly 언어로 작성된 최초의 시분할 운영체제
    - Unics → Unix - 작고 심플한 운영체제로 다시 작성, 1973년 대부분이 c언어로 다시 작성
- BSD 계열의 Free BSD, SunOS, GNU/Linux, System V 계열의 HP-UX(HP), AIX(IBM), Solaris(Oracle), 리눅스
- 리눅스의 등장
    - 1983년 GNU 프로젝트 - 리처드 스톨만이 UNIX와 유사한 공개 운영체제를 개발하기 위해 시작
    - 1985년 GNU 선언문 발표, 1989년 GPL 발표
    - 1991년 리눅스 커널 - 리누스 토르발스가 커널을 작성하여 발표, 커널은 하드웨어를 제어하고 응용 프로그램과의 상호작용을 제공하는 운영체제의 핵심

## 리눅스 개요

- 다중사용자-다중작업 지원, 뛰어난 이식성, 모듈화, CUI&GUI 지원, 공개된 소스코드
- 여러 종류의 파일 시스템을 지원 e.g. Minix, ext 계열, FAT, FAT32, NTFS, NFS, ISO-9660 등
- 효율적 하드웨어 활용과 다양한 응용 프로그램과 소프트웨어 개발 환경 제공

## 오픈소스와 라이선스

- 반대 개념은 proprietary(closed) 소프트웨어라고 함
- 리눅스의 라이센스
    - 주로 GPL(GNU General Public License), 일부는 LGPL(GNU Lesser General Public License), X window는 MIT
    - GNU GPL - 자유롭게 사용 및 복제 및 배포, 수정 배포 시 소스 코드 공개 필수, 저작권자를 표시하고 똑같이 GPL 조건으로 배포해야 함
- 강력한 카피레프트 조건을 절충한 것이 LGPL(소스 공개 여부 조정), MPL 코드를 포함하지 않은 파일은 공개 의무 없음
- BSD, Apache, MIT 라이센스 - 배포 시 소스코드 비공개 허용, Permissive 라이선스, 코드의 재사용을 높이려는 목적

## 리눅스 배포판

- 리눅스 커널 외에 시스템 유틸리티, 응용프로그램, 설치 프로그램을 포함한 완전한 운영체제
- 리눅스 커널은 1991년 처음 개발
- Debian 계열 - Debian, Ubuntu 등
- Slackware 계열 - Slackware, SUSE 등
- Red Hat 계열 - Redhat, Fedora, CentOS, Rocky Linux 등
- Debian 리눅스 - GNU 정신에 가장 충실한 배포판, GNU의 유일한 공식적 후원
    - stable, testing, unstable - 숫자 버전 외에 코드명을 가지며 unstable 버전은 항상 코드명이 sid
- Red Hat 리눅스 - 무료 버전은 2003년 지원 중단되었으나 Fedora와 CentOS라는 오픈소스 프로젝트가 지원
    - RPM(Red Hat Package Manager) - 바이너리 및 라이브러리 등 일괄 관리, 특정 패키지나 파일 검색, 의존성 유무 확인
- CentOS 리눅스 - Red Hat Enterprise Linux(RHEL) 기반의 무료 버전, 서버용으로 많이 사용, 최근 RHEL의 업스트림 버전인 CentOS Stream만 지원
- Rocky 리눅스 - 기존 CentOS 리눅스의 대체 제품
    - CentOS의 릴리스 정책이 변경되어 기존 CentOS의 대안이 필요, 그레고리 커처가 Rocky Linux 프로젝트 시작
- SuSE 리눅스 - 독일에서 만든 배포판, 유럽에서 많이 사용, Novell 사에 의해 지원, 유료와 무료 버전 존재
- Slackware 리눅스 - 가장 먼저 대중화된, 현존하는 가장 오래된 배포판, 간결함이 설계 철학, 유닉스 자체 학습에 적합
- Ubuntu 리눅스 - Denian 리눅스에서 파생, 영국 기업 Canonical 지원, 개인 사용자에게 인기 있는 리눅스 배포판 중 하나