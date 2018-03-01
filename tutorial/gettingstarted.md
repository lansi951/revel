---
title: 시작하기
layout: tutorial
---


## Go 설치

Revel을 사용하려면 먼저 [Go를 설치](http://golang.org/doc/install)해야합니다.

- 공식 문서 [Go 설치 안내서](https://golang.org/doc/install)를 참조하세요.
    - [Ubuntu](https://github.com/golang/go/wiki/Ubuntu)
    - [Windows](https://golang.org/doc/install#windows)

### GOPATH 설정

만약 설치하는 도중 GOPATH를 설정하지 않았다면 지금 설정하세요.

`GOPATH`는 당신의 모든 Go 코드가 실행되는 디렉토리입니다. 설정 방법은 다음과 같습니다.

1. 디렉토리 만들기: `mkdir ~/gocode`
2. Go가 당신의 GOPATH를 적용하도록 명령: `export GOPATH=~/gocode`
3. 앞으로 모든 셸 세션에 GOPATH가 적용되도록 저장하기: `echo export GOPATH=$GOPATH >> ~/.bash_profile`

환경에 따라 다른 설정 파일 (예: *~/.bashrc*, *~/.zshrc, etc.)에 export 명령을 작성해야 할 수도 있습니다.

이제 Go 설치가 끝났습니다.

## git과 hg 설치

`go get`을 사용해 다양한 의존성을 복제하기 위해 Git과 Mercurial이 필요합니다.

* [Git 설치](http://git-scm.com/book/en/Getting-Started-Installing-Git)
* [Mercurial 설치](https://www.mercurial-scm.org/downloads)

## Revel 프레임워크 가져오기

Revel 프레임워크를 가져오려면 다음 명령을 실행하세요.

	go get github.com/revel/revel

이 명령은 몇 가지 작업을 수행합니다.

* Go는 git을 사용해 저장소를 `$GOPATH/src/github.com/revel/revel/`에 복제합니다.
* Go는 모든 의존성을 찾아 `go get`을 실행합니다.

### Revel 명령줄 도구를 가져와서 빌드

[`revel`](tool.html) 명령줄 도구는 Revel 앱에 [`build`]({{ layout.root }}/manual/tool.html#build), [`run`]({{ layout.root }}/manual/tool.html#run), [`package`]({{ layout.root }}/manual/tool.html#package) 명령을 사용할 수 있습니다.

설치하려면 `go get`을 사용하세요:

	go get github.com/revel/cmd/revel

`$GOPATH/bin`을 PATH 환경변수에 추가하세요. 그러면 어디서나 명령을 사용할 수 있습니다.

	export PATH="$PATH:$GOPATH/bin"

작동되는지 확인하세요:

	$ revel help
	~
	~ revel! http://revel.github.io
	~
	사용법: revel command [arguments]

	명령은 다음과 같습니다:

	    new         뼈대 Revel 앱을 만듭니다
	    run         Revel 앱을 실행합니다
	    build       Revel 앱을 빌드합니다 (예: 개발용)
	    package     Revel 앱을 패키징합니다 (예: 개발용)
	    clean       Revel 앱의 임시 파일을 청소합니다
	    test        명령줄에서 모든 테스트를 실행합니다

	자세한 내용은 "revel help [명령]"을 사용하세요.


<a href="createapp.html" class="btn btn-sm btn-success" role="button">다음 <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span></a> [새 Revel 앱 만들기.](createapp.html)
