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

# 2강. 리눅스 설치

## 저장 장치 이름과 표준 디렉토리

- 파티션 - 하드디스크를 논리적으로 나눈 구역, 별도의 파일 시스템을 만들 수 있음
    - 윈도우 - 각 파티션마다 각각의 드라이브 지정
    - 리눅스는 오직 1개의 루트 디렉터리만 가짐, 여타 파티션은 루트 파일 시스템의 특정 디렉터리에 마운트(부착)
    - 리눅스는 하드디스크나 주변 장치를 파일로 취급 - 거의 모든 걸 파일로 취급하여 다룸, 파일 입출력과 비슷하게 처리
- 장치 이름
    - IDE 디스크 - prefix로 hd, SCSI 디스크 - sd를 붙임 e.g., /dev/hda, /dev/sdb
    - 물리적 하드디스크가 추가될 때 알파벳 순서대로 함
    - 파티션 번호는 숫자 1부터 차례대로 붙임 e.g., /dev/hda1
    - CD 또는 DVD e.g., /dev/sr0
- 리눅스 표준 디렉터리
    - 루트 디렉터리 - 최상위 디렉터리, 파일 시스템의 바탕을 이루는 중요 디렉터리
    - 바이너리 디렉터리 - /bin, /sbin
        - 기본적인 명령의 실행 파일을 포함(/bin), 부팅 과정이나 시스템 관리에 필요한 명령의 실행 파일 포함(/sbin)
    - 부트 디렉터리 - /boot - 커널 이미지와 부트 로더의 설정 파일을 포함
    - 디바이스 디렉터리 - /dev - 장치를 접근하는 데 사용되는 ‘디바이스 파일’이 위치
    - 시스템 설정 파일 디렉터리 - /etc - 시스템의 중요한 환경 설정 파일
    - 사용자 계정 디렉터리 - /home - 사용자 계정의 홈 디렉터리가 만들어짐 e.g., /home/gonasooc
    - 공유 라이브러리 디렉터리 - /lib - 프로그램들이 사용하는 시스템 라이브러리 파일
    - 미디어 디렉터리 - /media - 이동식 저장 장치가 마운트될 때 마운트 지점을 제공
    - 시스템 정보 디렉터리 - /proc - 커널이 사용하는 가상 파일 시스템
    - 루트 계정의 디렉터리 - /root - root 계정의 홈 디렉터리
    - 사용자 디렉터리 - /usr
        - /usr/bin, /usr/sbin - 여러 가지 실행 파일
        - /user/include - 라이브러리 헤더 파일
        - /usr/lib - 실행 파일을 위한 사용자 라이브러리
    - 가변 자료 저장 디렉터리 - /var - 시스템 운영 중 필요한 가변 자료 저장, 시스템 작동 로그, 인쇄용 스풀 영역, 사용자 메일박스 등 e.g., /var/spool/client-

# 3강. 셸 사용하기

## 셸 개요

- 셸(Shell) - 명령어 해석기, 또는 명령 행 인터페이스, 사용자와 커널 사이 명령어 해석 처리, GUI로 하기 힘든 다양한 기능 수행
- 셸 스크립트를 통해 반복적으로 수행되는 작업을 셸 스크립트로 작성 가능, 셸이 셸 스크립트 파일을 읽어 처리 가능
- 기본적으로 한 개의 명령 입력 후 엔터를 눌러 명령 수행, 한 라인에 여러 명령 수행하려면 세미콜론(;)을 사용해서 순차적 처리 가능
- 많은 리눅스 배포판에서 Bash를 기본 셸로 사용
- 명령 프롬프트로 일반 사용자는 $, root 사용자는 #을 사용
- 셸 종류에 따라 에일리어스 설정, 초기화 파일, 셸 스크립트 작성법, 명렁 행 완성 기능, 명령 행 편집 기능 등 차이가 있음
- Bash 셸 - Bourne Again Shell로 Bourne 셸의 개선된 버전 - 많은 셸 스크립트의 문법이 Bourne 셸에 기반을 둠
- 셀 선택 가능 - chsh -s /bin/sh - 기본 셸 변경 가능
- 터미널 창을 대화형 셸이라고 함
- 로그인 셸과 비로그인 셸을 구별해야 함

## 셸 명령

- 명령어 옵션 인수 - 옵션과 인수는 여럿일 수 있고 선택적 또는 필수적
- 대기 명령어는 프로그램의 이름, 관리자 명령 또는 일반 사용자 명령 구분, 가장 간단한 형태의 실행은 명령의 이름만 사용 e.g., who, date, ls, pwd
- chsh 명령 - 기본 셸을 바꾸는 명령
- chsh [options] [username] - 대괄호는 생략가능, 이탤릭체는 적당한 내용으로 대체해야 함을 의미, 복수는 여러 개가 가능하다는 의미
- 짧은 옵션(-) → ls -l, ls -lat와 ls -l -a -t는 같은 것
- 긴 옵션(—) → ls —all → 긴 옵션에는 제대로 된 하나의 단어가 나옴
- ps a 처럼 대쉬가 붙지 않는 옵션도 있음
- 인수는 명령 수행 대상을 지정 e.g., cat -n /etc/passwd
- 옵션 자체도 인수를 가질 수 있음, 옵션에 인수를 제공할 땐 빈칸을 줘선 안됨
- 명령어의 종류
    - 에일리어스 - alias 명령을 통해 별칭 설정
    - 셸 예약어 - 예약된 단어로 do, while, case 등
    - 함수 - 셸에서 수행되는 함수의 정의
    - 내장 명령 - 셰 내부에 존재하는 명령 cd, echo, pwd 등
    - 일반 명령 - 실행 파일이 존재하는 명령
- alias 명령
    - e.g., alias la=’ls -A’; alias rm=’rm -i’
    - 계속 유지하려면 config에 별도 기록
    - 별칭 해제하려면 unalias
- type 명령 - 명령이 어떻게 해석되는지 알려주는 명령어 e.g, type cd; type -a ls
- which 명령 - 실행 프로그램을 환경변수 PATH를 기초로 찾아 경로 출력 e.g., which rm → where is 비슷한 명령어
- man 명령 - 메뉴얼 페이지를 보여줌, 사용법 또는 설정 파일 등 온라인 도움말
    - ls —help 처럼 간단한 도움말 가능

## 명령 히스토리

- history 명령 - 이전 수행했던 명령 행 1000개 출력, history 10을 통해 행수 제어 가능
    - 히스토리 기능을 이용해서 !!엔터 - 직전 명령 실행, !n엔터 - 히스토리 목록에서 해당 명령 실행, !string엔터 - 지정된 문자열로 시작하는 최근 명령 실행
    - 방향키 위 아래 직전 또는 직후 명령 불러옴
- 명령 행 완성 기능 - 처음 몇 자 입력 후 tab키 누르면 실행
    - 정보가 충분하지 않은 경우 tab 한번 더 누르면 모든 가능한 경우 보여주고 원래의 명령 행 유지

## 명령의 연결과 확장

- 백슬래시 - 특수 문자의 기능을 제거하는 이스케이프(escape) 문자 또는 긴 명령 행 분리할 때 사용
- 틸드(~) - ~ 또는 ~username은 사용자의 홈 디렉터리 의미
- 도트(.) - 현재 작업 디렉터리 표시
- 더블도트(..) - 현재 디렉토리의 부모 디렉터리
- 파운드(#) - # 문자 뒤에 나타나는 문자를 주석 처리
- 달러($) - $변수는 변수의 값을 추출
- 앰퍼샌드(&) - 명령&는 명령을 백그라운드로 실행
- 애스터리스크(*) - 0개 이상의 임의 문자열 대응
- 물음표(?) - 파일 이름에서 사용할 때 1개 문자와 대응
- 파이프(|) - 앞 명령의 출력을 다음 명령의 입력으로 연결
    - 명령1 | 명령2 e.g., cat /etc/passwd | sort | more
- < 또는 > - 입출력 리다이렉션에서, 즉 파일로부터 입력받을 때 또는 파일로 출력할 때 사용
- >> - 표준 출력을 파일의 끝에 덧붙일 때 사용
- 느낌표(!) - 명령 히스토리 기능을 이용할 때 사용
- 명령치환 - 명령을 수행할 때 명령의 인수로서 다른 명령의 결과를 사용
    - 두 단계로 처리할 명령어를 명령치환을 통해 한번에 처리 가능
    - ls -l $(which passwd
- 인용 부호
    - 작은따옴표는 특수 문자의 명령어로서의 의미를 제거하고 텍스트화
    - 큰따옴표 안에서는 특수문자의 의미를 해석하여 확장
    - 백슬래시는 특수문자 앞에서 특수 문자의 의미를 제거
- 수식과 변수의 확장
    - 명령 수행 전에 수식의 결과를 계산하여 전달 가능
    - $[수식] 또는 $((수식))
    - 명령 수행 전에 변수의 값을 추출하여 전달
    - echo “I am $[2023-1979] years old.”

## 셸 변수

- 셸 변수(지역 변수) - 변수가 정의된 셸에서만 사용 가능, 서브 셸로는 전달되지 않음
    - unset을 통해 삭제 가능
- 환경 변수(전역 변수) - printenv 명령을 통해 이미 설정된 환경변수 확인 가능, 변수 이름은 대문자, export -p 현재 셸의 존재하는 모든 환경 변수 출력
- 변수 설정과 환경 변수로 만드는 방법 - 변수=값;export 변수 또는 export 변수=변수
- echo $변수 는 모든 변수값을 확인할 수 있음

# 4강. 파일과 디렉터리

## 파일 시스템 탐색

- 파일 시스템 - 파일과 디렉터리의 집합을 구조적으로 관리하는 체계, 리눅스는 전체 파일 시스템을 1개의 트리 구조로 관리, 1개의 루트(/) 디렉터리만 있음
- ls [options] [names]
    - ls directory or ls file
    - 일반적으로 현재 폴더에서 ls 입력했을 때 나오는 건 ls .
- 파일의 종류
    - 정규 파일 - 실행 파일이나 이미지 파일의 경우 바이너리 형태로 저장되어 바이너리 파일이라고 함
    - 디렉터리 - 저장된 파일이나 하위 디렉터리에 대한 정보가 저장
    - 심볼릭 링크 - 소프트 링크라고도 하는데, 윈도우의 ‘바로가기’와 비슷
    - 장치파일 - 프린터, 하드디스크 등 각종 장치를 파일로 취급, 블록 디바이스 파일과 문자 디바이스 파일로 구분

## 파일과 디렉터리 관리

- mkdir -p backup/java - backup이 존재하면 하위에 java 디렉터리를 만들고, 없으면 backup이라는 parent까지도 생성
- rmdir -p dir1/dir2 - 부모 디렉터리가 비게 되는 경우 같이 삭제
- cp [options] file1 file2 - 파일이나 디렉터리 복사, 대상 파일이 존재하면 덮어쓰기 수행됨 → cp -i file1 file2 같이 interaction 옵션을 줘서 덮어쓰기 전 확인할 수 있게 가능
- cp [options] files directory → 마지막 인자가 디렉터리인 경우 여러 파일을 지정된 디렉터리에 같은 이름으로 복사
- mv [options] source target
- rm [options] files 파일을 삭제할 때 조심해야 함, alias rm=’rm -i’을 수행하여 에일리어스 설정하는 게 안전
    - -r 옵션은 디렉터리(포함된 파일과 서브 디렉터리)를 모두 함께 삭제
    - -i 옵션은 삭제 전에 물어봄
    - -f 옵션은 물어보지 않고 무조건 삭제
- 파일의 접근권한 - 읽기/쓰기/실행
    - 소유자(u)/그룹(g)/기타(o) - 이렇게 3개씩 끊어서 읽으면 됨
    - 읽기(r)쓰기(w)실행(x)
    - e.g., -rw-rw-r— → 앞에 - 이건 정규파일 케이스
- 디렉터리의 접근권한
    - 디렉터리에서의 실행 권한은 해당 디렉터리로 이동하거나 디렉터리의 정보 조회까지 포함됨
    - 즉, 적어도 읽기와 실행 권한을 갖고 있어야 디렉터리로 이동하거나 ls -l 명령 수행 가능
- chmod(change mode) - 파일 소유자가 파일의 접근권한 변경
    - chmod [options] mode files - -R 옵션을 디렉터리에 적용하면 포함된 모든 파일과 서브 디렉터리까지 권한을 변경
    - 8진수 모드로 작성 - chmod -R 755 dir1
    - 기호 모드를 작성 - chmod [ugoa][+-+][rwx]
- umask - 파일이나 디렉터리 접근권한의 기본값을 출력하거나 설정 - 8진수 기준
    - touch 명령어를 통해서 umask 값을 확인할 수 있음
    - 참고로 touch file 명령은 파일의 접근/수정 시간을 현재 시간으로 변경하며 파일이 존재하지 않으면 파일을 생성
- chown - root 사용자가 파일이나 디렉터리의 소유자 또는 소유 그룹을 변경하는 명령
    - chown [options] newowner files
- ln(link) - ln [options] 원본파일명 [대상파일명] - 기본적으로 하드링크를 만들며 -s 옵션을 사용하면 심벌릭 링크가 만들어짐
    - 하드링크 - 하나의 파일에 다른 이름을 부여, 원본 파일의 링크 카운트가 존재함, 다른 파일 시스템에는 링크할 수 없고 디렉터리에도 만들 수 없음
    - 심벌릭 링크 - 윈도우의 ‘바로 가기’와 비슷, 다른 파일 시스템 및 디렉터리 생성 가능, 원본을 삭제하면 심벌릭 링크는 의미가 없어짐

## 파일의 내용 확인

- more [options] files - 파일의 내용을 화면 단위로 출력하는 명령
- less - more 명령의 개선된 버전
- head [options] [files] - 파일의 맨 앞 부분을 출력 → 옵션 -n 숫자 또는 -숫자를 사용하면 보고 싶은 라인 수 변경 가능 e.g., head -n 5 /etc/passwd
- tail [options] [files] - 파일의 마지막 부분을 출력
    - tail -f /var/log/messages → 수시로 변경되는 파일의 경우 -f 옵션을 통해 감시 가능
- cat [options] [files] - 하나의 파일 또는 여러 파일을 연결(concatenate)시켜 출력

# 5강. 리눅스 시작과 종료

## 운영체제의 부팅

- 부팅과정
    - ROM BIOS의 펌웨어 실행 - BIOS 기반 x86 컴퓨터를 가정했을 때 하드웨어 검사 후 부트 로더를 적재
    - MBR(master boot recoder)에 있는 부트 로더가 실행 - 파티션 테이블을 조사하여 부팅 가능한 파티션 찾음 → 리눅스의 부트로더인 GRUB를 찾아 적재 → GRUB는 그래픽 인터페이스와 멀티부팅을 지원
    - 커널 이미지와 initramfs를 로드 → 커널 이미지는 /boot/vmlinuz-<kernel-version> → initramfs는 부팅 과정에서 필요한 임시 파일 시스템
    - 커널이 실행됨
    - 하드웨어 점검하고 초기화 → 메모리, 프로세서, 저장장치 등 디바이스 드라이브 로드
    - 루트 파일 시스템을 마운트하고 검사
    - 커널은 /lib/systemd/systemd 프로그램을 실행시키고 제어를 넘김 - systemd 프로세스는 시스템 운영을 위한 나머지 초기화 과정을 처리 → systemd는 부팅이 끝난 후에도 계속 수행
- 부팅 과정과 초기화
    - 요약 - BIOS → 부트 로더 → Boot device → GRUB → kernel + initramfs → systemd(/lib/system/systemd) → /etc/systemd/system/default.target

## 초기화 데몬

- 전통적 init 데몬 - System V init 데몬 - 런레벨에 따라 실행 및 중단 서비스가 정해짐 - 시간이 오래 걸리며 복잡한 초기화 스크립트로 효율적 대처가 어려움
- Upstart init 데몬 - 이벤트 기반 서비스 실행 - 복잡한 스크립트가 간단한 설정 파일로 대체
- systemd 프로세스 - 커널이 실행시키는 첫 번째 사용자 프로세스 - 모든 사용자 프로세스의 최상위 조상 프로세스(PID: 1) - 초기화 데몬으로 사용자 환경 및 서비스 벙렬, 운디멘드 활성화, 서비스 의존성 해결 등 - 시스템 운영 관리 및 셧다운까지 처리
- 유닛 - systemd가 관리하는 시스템 자원이나 서비스와 같은 시스템 구성요소 - 유닛의 동작, 의존성 등은 유닛 설정 파일에서 설정 항목으로 제어
- 기본 타깃과 런레벨 - 초기 런레벨은 0 또는 6이 되어선 안됨
- telinit 명령 - telinit 3은 런레벨을 변경하여 텍스트 모드만 지원, telinit 0은 종료, telinit 6은 재부팅
- renlevel 명령 - 이전 런레벨과 현재 런레벨 확인
- 시스템 서비스의 관리 - 과거 서비스 수행을 위한 초기화 스크립트는 서비스 유닛으로 대체, 관리자는 systemctl 명령을 사용하여 상태 보기 등 작업 수행 가능 - systemctl [options] command [units]
- 웹 콘솔의 사용 - 웹 브라우저를 이용해 리눅스 서버 관리 및 모니터링

## 시스템 종료

- 전원 관리 명령 - 호환상의 이유로 shutdown 명령도 사용 가능, 하지만 가급적 systemctl 명령을 사용하는 것이 좋음
- shutdown [options] time [message] - -r은 재부팅, -c는 예약된 셧다운 취소(time 인수 필요 없음), -k는 실제 셧다운하는 것처럼 경고 메세지만 보냄 → time 인수는 절대 시간 형식, 인수로 now는 즉시 종료 의미
- 시스템의 종료 절차 - systemd 프로세스는 모든 프로세스에게 종료를 알림 → 각 프로세스가 스스로 종료하도록 TERM 시그널 보냄 → 미종료된 게 있으면 강제 종료를 위한 KILL 시그널을 보냄 → 파일 시스템을 잠그고 루트 파일 시스템을 제외한 모든 파일 시스템 언마운트 → 시스템 호출을 통해 커널 재부팅 또는 종료 요청
- 시스템의 일시 중단
    - 일시중단(suspend) - 시스템 상태를 RAM 저장, 저전력 상태
    - 최대 절전모드(hibernate) - 시스템 상태 하드디스크 저장, 전원 끔
    - 하이브리드 슬립(hybrid-sleep) - 메모리 외에 디스크에도 시스템 상태 저장

## 데스크톱

- GUI를 제공하는 사용자 환경 - GNOME, KDE ..

# 6강. 사용자 관리

## 사용자 계정

- su [-[l]] [username] - 사용자를 전환시키는 명령
- sudo 명령 - root 또는 다른 사용자가 되어 명령을 실행하기 위한 명령

## 사용자 계정 만들기

- useradd
- chage

## 사용자 계정 수정

- usermod
- userdel

## 그룹 계정과 관리

- groupadd
- gpasswd
- groupmod
- groupdel

# 7강. 텍스트 편집

## 편집기

- 리눅스 시스템에서는 중요한 설정 정보나 셸 스크립트가 텍스트 파일로 존재함
- 리눅스 텍스트 편집기의 종류 - gedit, emacs, vi

## 파일 찾기와 문자열 검색

- locate 명령
- find 명령
- grep 명령

# 8강. 파일 시스템 관리

## 마운트와 언마운트

- 마운트 - 파일 시스템을 전체 디렉터리 구조에서 특정 디렉터리로 연결하는 것
- /etc/fstab - 리눅스 시스템이 부팅될 때 자동으로 마운트할 파일 시스템 목록을 가진 설정 파일
- mount 명령 - 파일 시스템을 마운트
- unmount - 디렉터리에 마운트되어 있는 저장 장치를 해당 디렉터리로부터 분리

## 파티션 관리

- 파티션 - 물리적 저장 장치를 논리적으로 분할한 고정 크기의 구역

## 볼륨 관리

- 볼륨은 크기가 재조정될 수 있는 파티션
- 여러 디스크를 합쳐 하나의 볼륨을 만들 수 있음

## 파일 시스템

- 저장 장치를 디렉터리 구조로 조직화하고 파일에 이름을 부여하는 등의 파일의 저장과 검색을 위한 체계
- 슈퍼 블록, inode 테이블, 데이터 블록
- mkfs, fsck, df, du

# 9강. 프로세스 관리

## 프로세스

- 커널에 등록되어 관리를 받는 ‘실행 중인 프로그램’
- systemd 프로세스는 PID가 1 - 모든 사용자 프로세스의 조상 프로세스
- 셸에서 명령 실행 → 새로운 프로세스가 만들어져 처리됨
    - 터미널 창에서 ls 싫애하면 셸은 fork() 호출하여 셸의 복사본(자식 프로세스)를 생성 → 자식 프로세스는 exec(ls)를 호출하여 ls 명령 실행
- 포그라운드 실행 중에는 다른 키보드 입력 및 실행 불가능
- 백그라운드 실행은 명령 끝에 &를 추가
- 백그라운드 프로세스 → 포그라운드 전환 fg jobld
- jobs -l 백그라운드 프로세스 상태 확인

## 프로세스의 상태

- ps - 프로세스의 현재 상태를 확인하는 스냅샷
- ps -ef → 모든 사용자의 프로세스 상태

## 프로세스 관리

- top은 ps의 동적 버전 - CPU 사용량이 많은 프로세스를 먼저 보여줌
- kill - 강제로 즉시 종료
- term - terminate 의미로 kill 명령의 기본 시그널
- int - interrupt
- nice - nice 우선순위 값을 조정하는 명령, 기본 NI 값은 0
- nohup - HUP 시그널과 무관하게 백그라운드 명령이 스스로 종료될 때까지 계속 수행시키는 명령

## cron 서비스

- 지정된 시간에 주기적으로 자동 수행되는 작업을 수행
- crond 데몬 프로그램이 서비스를 제공
- crontab - cron 작업의 수행 방법을 정의

# 10강. 소프트웨어 관리

## 패키지 관리

- 리눅스 배포판에 따라 패키징 방법이 다름
    - 데비안 계열은 DEB(.deb), 레드햇 계열은 RPM(.rpm)
- 고수준 패키지 관리 도구는 ‘저장소 설정 파일’을 통해 많은 패키지 저장소를 검색할 수 있음

## RPM 패키지 관리자

- 레드햇 계열 리눅스에서 패키지 파일의 표준 형식
- 이름-릴리즈-배포판-아키텍처.rpm
- rpm [options] [packages]

## DNF 패키지 관리자

- YUM - Yellowdog Updater Modified
    - 레드햇 리눅스 계열 배포판 주로 사용, RPM 개선
- DNF - Dandified YUM, YUM의 차세대 버전
    - 최신 리눅스 배포판의 기본 패키지 관리자

## 압축

- 다양한 압축 기술 존재 - gzip, bzip2, xz, zip 등
- gzip
    - 가장 널리 사용되는 기본 압축 프로그램
    - 초기 Unix 시스템의 압축 프로그램을 GNU 프로젝트에서 수정한 버전
- gunzip
    - .gz, .Z, .tgz, .taz 등의 확장자의 파일을 풀고 확장자를 제거
- bzip2
    - 블록 정렬 압축 알고리즘을 사용한 파일 압축 프로그램
    - gzip가 유사하나 압축 효율이 좋고 속도는 느림
- tar 명령 - 여러 파일을 하나의 아카이브 파일로 묶거나 아카이브 파일에서 파일을 추출하는 명령

# 11강. 셸 스크립트(1)

## 셸 스크립트 개요

- 스크립트 언어, 프로그래밍 언어 - 함수, 변수, 반복문, 조건문 등 사용 가능
- 셸 명령어의 집합으로 이루어진 실행 가능한 프로그램(텍스트 파일)
- 셸 스크립트 실행 방법
    - bash script_file - 서브 셸을 새로 생성하여 스크립트를 실행
    - ./script_file - 서브 셸을 새로 생성하여 스크립트를 실행
    - source script_file or script_file - 현재 사용 중인 셸 환경에서 스크립트를 실행

## 변수의 사용

- 별도의 선언 없이 변수 사용 가능
- 변수 값은 기본적으로 문자열로 취급, 연산이 필요하고 변환이 가능한 경우 정수로 다뤄짐
- read 명령 - 키보드로부터 한 라인을 읽은 후 개별 단어에 상응하는 변수를 저장

## 함수의 사용

- 셸 스크립트에서 반복적으로 사용하는 명령의 묶음
- source 명령이나 도트(.) 명령으로 실행

# 12강. 셸 스크립트(2)

## 선택 구조

- if 명령 - 참이면 then 다음의 명령 실행
- test 명령 - 조건 검사를 위해 사용하는 명령 - 조건이 만족하면 0, 아니면 1을 리턴
    - test 생략하고 사용하려면 대괄호 사용
- case 명령 - Java 언어에서 switch문과 거의 같음

## 반복 구조

- for 명령 - 모든 데이터를 한 차례씩 처리하는 제어 구조
    - do와 done 사이에 명령을 수행
- while 명령
    - do와 done 사이에 명령을 수행
- until 명령 - 조건 만족될 때까지(참이 나오기 전까지) 명령을 반복해서 수행

# 13강. 버전 관리의 깃

## 버전 관리

- 중앙 집중형 - CVS, SVN (로컬 없이 서버와 직접 연결, 관리는 쉬우나 단순한 변경조차 서버를 통해야 한다는 종속성이 있음)
- 분산형 - 머큐리얼, 바자, 깃

## 깃 사용하기

- 깃 저장소 - 특정 작업 디렉터리를 깃 저장소로 지정해야 함
- 커밋을 수행하여 특정 시점에서 스테이지의 스냅샷을 저장
- 파일을 처음 생성하면 untracked, 스테이징에 올려야 추적, 커밋은 스냅샷

## 버전 여행

- 체크아웃
    - git status 결과가 깨끗해야 체크아웃이 가능
    - git checkout HEAD~ → HEAD를 직전 커밋으로 이동
    - git checkout - → 직전 브랜치(이전 HEAD 위치)로 이동
    - git checkout branch
- 저장 공간 간 파일 비교
    - git diff

    # 14강. 브랜치의 생성과 병합

## 브랜치의 개요

- git branch -v → 브랜치마다 상세한 정보 나열
- git branch newBranchName → 주어진 인자로 브랜치를 생성하나 HEAD는 이동하지 않음
- git checkout HEAD~ 는 별도의 옵션 없이 HEAD 이동, switch는 -d 옵션으로 이동

## 브랜치 관리

- git branch —merged → 현재 작업 브랜치 기준으로 병합된(도달 가능한) 브랜치 목록 표시
- git branch —no-merged → 현재 작업 브랜치 기준으로 아직 병합되지 않은(도달 불가능한) 브랜치 목록 표시

## 브랜치 병합

- fast-forward 병합 → git merge childBranch
- 3-way 병합
- squash 병합

## 브랜치 병합 충돌과 해결

- 자동 3-way 병합을 할 수 없어서 충돌 발생하는 경우
- 충돌이 일어난 파일은 ‘병합하지 않은 경로(unmerged path)’로 표시됨

# 15강. 스태시와 버전 되돌리기

## 스태시의 개요

- 스태시 - 마지막 커밋 이후 변견된 작업 영역과 스테이지 영역을 임시 저장 가능
- git stash apply - 스태지의 저장 목록에서 특정 항목을 불러와 작업 영역에 적용하여 복원
- git stash drop은 별도 적용 없이 삭제, git stash clear는 모두 삭제

## 스태시 활용

- git stash branch branchName - 새로운 브랜치를 만들고 스태시 적용

## 리셋

- HEAD의 위치를 이전 특정 커밋으로 되돌리는 것, 버전을 과거로 완전히 되돌리는 것
- git reset —hard commit → 모두 재설정
- git reset [—mixed] commit → 작업 폴더는 그대로 남음
- git reset —soft commit → 깃 저장소만 재설정

## 리버트(revert)

- 최근 커밋부터 차례로 이전 커밋을 취소할 때 사용, 커밋 이력은 남겨둔 채 ‘커밋 취소’를 의미하는 새로운 커밋을 생성
- 한번에 한 커밋만 취소
- 리버트의 —no-commit 옵션 → 범위 내의 여러 커밋을 취소할 때취소 커밋 한 개만 생성할 수 있음