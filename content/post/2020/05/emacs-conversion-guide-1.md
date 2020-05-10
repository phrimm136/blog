+++
title = "이맥스 개종 가이드 1 - 설치 및 이맥스 기초"
date = 2020-05-09T22:40:00
tags = ["emacs"]
draft = false
thumbnail = "/images/real-programmers-use-emacs.png"
+++

본 가이드는 프로그래밍 갤러리의 [이맥스 개종 가이드](<https://gall.dcinside.com/board/view/?id=programming&no=748120>)를 계승한 글입니다.

<!--more-->

> 이맥스를 쓰지 말라는 것은 손발을 잘라버리겠다는 것과 같다. - 커헠, 개종 가이드 원작자


## 서문 {#서문}

IDE 나 모던 에디터를 냅두고 왜 이맥스를 선택하냐는 물음은 항상 있어왔다. 처음부터 세팅되어있는 에디터들을 냅두고 굳이 낡은 에디터를 고집한다는 건 처음 듣기엔 그리 좋지 않은 생각이다. 그럼에도 불구하고 이맥스는 그 특유의 확장성 하나로 여러 에디터의 기능을 끌어다 쓰면서, 동시에 자신의 루틴에 최적화된 에디터를 만들어낼 수 있다는 장점으로 지금까지 살아남아왔다. 이 가이드는 입문자 여러분이 이맥스의 그 강력함을 몸소 느끼고 이맥스를 "자신의 손발"로 맞추는 데에 방향을 제시하기 위해 작성되었다.


## 설치 {#설치}

이맥스는 평범하게 패키지(혹은 프로그램) 깔듯이 설치하면 된다. 본인의 패키지 매니저가 지원하는 방식으로 설치하자.

```text
데비안 계열이라면 apt install emacs
아치 계열이라면 pacman -S emacs
맥이라면 brew install emacs
윈도우는 홈페이지 들어가서 설치 프로그램 다운 후 설치.
```


## 초기 구동 및 단축키 {#초기-구동-및-단축키}

이제 이맥스를 켜면 아래와 같은 화면이 나올 것이다.
![](/images/emacs-init.png)

본격적으로 설정을 시작하기 전에, 설정에 필요한 기초를 머리에 넣고 가자. 이맥스에서 키맵(단축키)을 지칭하는 방법은 조금 특이하다. `C-` 는 ctrl을, `M-` 는 meta(alt)를, `S-` 는 super(win, command)를 의미한다. 즉 `ctrl-x` 는 `C-x` 로 적는 것이 일반적이다.

이맥스에서 기본적인 설정을 하는데에 필요한 키맵은 다음과 같다.

```text
C-x C-f  파일, 디렉토리 열기
C-x C-s 저장
C-x C-c 이맥스 종료
M-x 입력한 명령어 실행
```

이 밖에도 이맥스에는 내장된 키맵은 매우 많고, 키맵에 등록되지 않은 명령어들은 더 많다. 이런 걸 언제 외우냐고? **안 외운다**. 순정 키맵을 그대로 쓰겠다는 사람은 미련하거나 외계인의 손을 가진 사람이라고 단언 할 수 있다. 서문에서 말했듯, 억지로 불편한 키맵에 적응하려 애쓰지 말고 자기가 원하는 대로 설정하면 된다. vim 키맵은 기본이고, 독자적인 모달 에디팅 키맵을 설정한 사람도 있다.


## 버퍼, 창 {#버퍼-창}

이맥스에는 탭이 없고 대신 버퍼라는 개념이 있다. 버퍼는 우리가 상호작용하는 내용을 의미하며, 파일을 연다는 것은 곧 파일과 연결되는 버퍼를 연다는 것과 같다. 처음 버퍼 개념을 접할 때는 버퍼=파일이라고 생각하기 쉬운데, 이맥스의 버퍼는 파일 뿐만 아니라 로그 등 입출력이 되는 모든 것은 전부 버퍼이다. `M-x` 를 누르면 밑에서 깜빡거리는 것이 보일텐데, 이것도 버퍼의 한종류로, 미니버퍼라고 한다. 버퍼를 바꾸려면 `C-x b` 를 누른 뒤 버퍼 이름을 입력하고, 버퍼를 닫으려면 `C-x k` 를 누른 뒤 버퍼 이름을 입력하면 된다.

창(window)은 이 버퍼가 들어가는 공간을 의미한다. 우리는 임의로 창을 만들 수 있으며, `C-x 2` 로 수직, `C-x 3` 으로 수평분할할 수 있고, 창을 닫을 때에는 `C-x 0` 을 누르면 된다. 창이 닫힌다고 해서 버퍼가 닫히는 것은 아니므로 안심해도 된다. 반대로 버퍼 하나를 창 여러개에 띄울 수 있다.

{{< figure src="/images/twin-buffers.png" >}}


## 설정 맛보기 {#설정-맛보기}

`C-x C-f` 를 치고 `~/.emacs.d/init.el` (윈도우는 `유저/.emacs.d`) 파일을 열어보자. 없다면 만들면 된다. 파일을 연 뒤에, 다음을 작성하고 저장하자.

```emacs-lisp
(package-initialize)

(add-to-list 'package-archives '("gnu" . "https://elpa.gnu.org/packages/"))
(add-to-list 'package-archives '("melpa" . "https://melpa.org/packages/"))
```

이맥스는 emacs-lisp 이라는 리슾의 방언의 인터프리터 위에 텍스트 편집기가 올라가 있다고 생각하면 편하다. 이맥스가 실행될 때 방금 열거나 만든 `init.el` 파일을 읽고 거기에 적혀 있는 함수를 인터프리터에 로드해, 그 함수를 쓸 수 있게 만든다. 또한 이맥스가 실행 중일 때도 얼마든지 다른 파일이나 코드를 로드할 수 있으며, 이는 나중에 패키지 설치나 설정 테스트를 손 쉽게 하는 요소로 작용한다. 저장을 했으면 `M-x eval-buffer` 을 입력해보자. 이 명령어는 현재 포커싱 되어있는 버퍼의 코드를 읽어 인터프리터에 로드하는 함수이다. 성공했다면 아무것도 뜨지 않을 것이다.

리슢코드는 함수와 인자를 괄호 안에 같이 넣는다. 이때 괄호 안 첫번째는 함수, 나머지들은 인자이다. emacs-lisp은 함수형 언어인 리슾과는 다르게 변수 선언과 할당을 지원하므로 괄호의 위치에만 적응하면 C나 파이썬 코드를 작성하듯이 작성할 수 있다.

방금 입력한 코드는 이맥스 패키지 매니저의 저장소를 추가하는 코드이다. 다른 곳에서 플러그인이라고 부르는 걸 이맥스에서는 패키지라고 부르며, 이 코드 덩어리들을 다운로드해서 인터프리터에 로드하는 것으로 패키지들을 사용할 수 있다. 다운로드한 패키지는 `.emacs.d/elpa` 폴더에 들어가고, 이맥스가 시작할 때 여기 들어있는 패키지들을 읽어 실행할 수 있는 상태로 만든다. 참고로, 다른 가이드에서 `melpa-stable` 이나 `marmalade` 를 등록하라는 경우도 있는데, [melpa-stable은 가급적이면 쓰면 안되고](<https://www.reddit.com/r/emacs/comments/etikbz/speaking%5Fas%5Fa%5Fpackage%5Fmaintainer%5Fplease%5Fdo%5Fnot/>), marmalade 는 너무 오래되었다. 현 시점에서 가장 활발한 저장소는 [melpa](<https://melpa.org/>)이므로 가급적이면 melpa에서 다운로드 받는 것이 좋다.

이제 시범삼아 패키지를 다운로드해보자. 방금 `M-x eval-buffer` 에서 보았듯, 이맥스의 내장 탐색기는 기본적으로 목록을 띄워주지 않는다. 탭을 누르면 자동완성 목록을 띄워주긴 하지만, 뭔가 시원찮다. 좀 더 익숙한 형태로 바꾸기 위해 패키지를 설치해보자.

이맥스 탐색 패키지는 [ivy](<https://github.com/abo-abo/swiper>)와 [helm](<https://github.com/emacs-helm/helm>)이 양대산맥을 이루고 있다. 이중 이 가이드에서는 ivy를 설치할 것이다. `M-x package-install` 후 ivy를 쳐서 패키지를 다운 받는다. 다운로드 후 `M-x ivy-mode` 를 입력해 ivy를 실행할 수 있다. 이맥스를 켤 때 마다 실행시키고 싶다면 `init.el` 에 `(require 'ivy) (ivy-mode 1)` 를 입력하면 된다. 물론 방금 파일에 적은 코드도 굳이 이맥스를 재실행하지 않고 로드할 수 있다. 상기 코드를 드래그 한 뒤 `M-x eval-region` 을 입력하면 드래그한 코드만 읽어서 로드할 수 있다.

{{< figure src="/images/ivy.png" >}}

다시 `M-x` 를 쳐보면 위와 유사하게 나오는 것을 볼 수 있다.

상술했듯 이맥스는 결국 거대한 리슾 인터프리터이기 때문에 모든 기능이 전부 리슾코드고 M-x로 실행한 것도 결국 리슾코드다. 상기 예시와 같이 파일에 설정 코드를 작성해나가면서 실행해볼 수 있기 때문에 repl에 대해 감을 잡아놔야 설정을 편하게 할 수 있다.