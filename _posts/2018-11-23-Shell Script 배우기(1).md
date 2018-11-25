---
layout: post
title: "[ETC] - Shell Script 배우기(1)"
description: "Shell Script"
date: 2018-11-23
tags: [Shell, Sh, Shell Script]
comments: false
share: true
---

업무를 하면서 개발을 잘하지 못하는 저로써는 가장 간단하게 시간을 절약할 수 있는 방법은 반복되는 작업을 자동화 하는 방법이라는 것을 깨달았습니다. 그 중 가장 많이 쓰이면서 익히는데 시간을 많이 들이지 않는 쉘 스크립트에 대해서 학습하고 정리해보기로 했습니다.

---

## 1. Shell 이란?
쉘 스크립트를 학습하기에 앞서 쉘(Shell)에 대해서 알아보도록 하겠습니다.
쉘이란 단순하게 표현하면 사람의 명령어를 해석하여 커널에 전달 해줍니다. bash,csh, zbash등 명령어를 해석하는 방법에 따라 종류가 나뉘어집니다.
저는 bash 쉘을 기준으로 포스트를 작성하도록 하겠습니다.


## 2. 시작하기
우선 프로그래밍을 하는데 가장 기초적인 출력, 함수 사용, 변수선언에 대해서 알아보도록 하겠습니다.

### 문자 출력하기
뭐든지 시작할땐 역시 hello world를 찍어봐합니다. 
아래와 같은 스크립트를 작성하면 화면에 hello world가 세번출력됩니다.
```
#!/bin/bash
echo "hello world"
printf "hello world"
printf "%s %s" hello world
```
echo는 문장을 출력하고 줄을 자동으로 바꿉니다.  printf는 c언어의 printf 처럼 작동합니다.
위에서 작성된 스크립트에서 #!/bin/bash를 여겨봐야합니다. 
쉘 스크립트에서 '#'은 주석을 의미하지만 바로뒤에 '!'가 나오면 스크립트를 실행할 쉘을 지정하는 선언문입니다. 따라서 '#!/bin/bash'는 bash를 사용하겠다는 의미가 됩니다.


### 함수 사용하기
다른 프로그래밍 언어와 비슷하며 'function' 키워드는 생략해도 됩니다.
```
#!/bin/bash

function study_shell() {
	echo "fun shell script"
}

args_input() {
	echo "args : ${@}"
}

study_shell

args_input
args_input "shell" "script" "study"
```
함수 호출코드는 선언 코드보다 뒤에 있어야 하며 매개변수는 선언하지 않아도 됩니다.

### 변수 선언하기
변수는 '='를 이용하여 대입하며 앞뒤로 공백이 없어야 한다. 변수는 기본적으로 전역변수로 선언되며 단 함수안에서 local 키워드를 사용하여 지역변수로써 사용할 수 있습니다.
하지만 전역 변수는 선언된 스크립트 파일에서만 유효하며. 자식 스크립트에서는 사용 할 수가 없다. 자식 스크립트란 하나의 스크립트 파일에서 호출하는 스크립트를 의미합니다.
부모 스크립트에서 export 라는 명령어를 붙여주면 환경변수로 설정되어 자식 스크립트에서 이용이 가능하게 됩니다.

```
# 전역 변수
var_test="hello world"
echo ${var_test}

# 지역 변수
local_var_test() {
	local var_test="local"
	echo ${var_test}
}

local_var_test

echo ${var_test}

#환경변수 설정
export env_var="hello world"

#스크립트 경로를 입력하여 자식스크립트를 호출한다.
/temp/child_script.sh

echo ${env_var}
```

환경변수를 설정할때 유의할점은 이미 기본적으로 설정되어있는 환경변수를 피해야 합니다. 이러한 예약변수들은 아래와 같습니다.

#### 예약변수
|문자|설명|
|:---|:---|
|HOME|사용자의 홈 디렉토리|
|PATH|실행 파일을 찾을 경로|
|LANG|프로그램 사용시 기본 지원되는 언어|
|PWD|사용자의 현재 작업중인 디렉토리|
|FUNCNAME|현재 함수 이름|
|SECONDS|스크립트가 실행된 초 단위 시간|
|SHLVL|쉘 레벨(중첩된 깊이를 나타냄)|
|SHELL|로그인해서 사용하는 쉘|
|PPID|부모 프로세스의 PID|
|BASH|BASH 실행 파일 경로|
|BASH_ENV|스크립트 실행시 BASH 시작 파일을 읽을 위치 변수|
|BASH_VERSION|설치된 BASH 버전|
|BASH_VERSINFO|BASH_VERSINFO[0]~BASH_VERSINFO[5]배열로 상세정보 제공|
|MAIL|메일 보관 경로|
|MAILCHECK|메일 확인 시간|
|OSTYPE|운영체제 종류|
|TERM|로긴 터미널 타입|
|HOSTNAME|호스트 이름|
|HOSTTYPE|시스템 하드웨어 종류|
|MACHTYPE|머신 종류(HOSTTYPE과 같은 정보지만 조금더 상세하게 표시됨)|
|LOGNAME|로그인 이름|
|UID|사용자 UID|
|EUID|su 명령에서 사용하는 사용자의 유효 아이디 값(UID와 EUID 값은 다를 수 있음)|
|USER|사용자의 이름|
|USERNAME|사용자 이름|
|GROUPS|사용자 그룹(/etc/passwd 값을 출력)|
|HISTFILE|history 파일 경로|
|HISTFILESIZE|history 파일 크기|
|HISTSIZE|history 저장되는 개수|
|HISTCONTROL|중복되는 명령에 대한 기록 유무|
|DISPLAY|X 디스플레이 이름|
|IFS|입력 필드 구분자(기본값:   - 빈칸)|
|VISUAL|VISUAL 편집기 이름|
|EDITOR|기본 편집기 이름|
|COLUMNS|현재 터미널이나 윈도우 터미널의 컬럼 수|
|LINES|터미널의 라인 수|
|LS_COLORS|ls 명령의 색상 관련 옵션|
|PS1|기본 프롬프트 변수(기본값: bash\$)|
|PS2|보조 프롬프트 변수(기본값: >), 명령을 "\"를 사용하여 명령 행을 연장시 사용됨|
|PS3|쉘 스크립트에서 select 사용시 프롬프트 변수(기본값: #?)|
|PS4|쉘 스크립트 디버깅 모드의 프롬프트 변수(기본값: +)|
|TMOUT|0이면 제한이 없으며 time시간 지정시 지정한 시간 이후 로그아웃|

