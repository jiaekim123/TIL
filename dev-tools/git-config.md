# 한 PC에서 git 계정 여러 개 사용하기

한 PC에서 회사 github 계정과 개인 github 계정을 사용할 경우, 매번 계정을 변경하는게 귀찮을 수 있다.

여러 github 계정을 설정하는 방법에 대해 알아보자.

## 1. git 폴더를 분리하자.

개인적으로, 회사 프로젝트들이 모여있는 git 폴더와 개인 공부용 프로젝트들이 모여있는 git 폴더를 분리해서 관리하고 있다.
예를 들어 회사 폴더는 `~/git/`에 프로젝트 파일들을 모아두고, 개인 폴더는 `~/git-jiae`와 같이 폴더를 분리한다.

```
$ cd ~
$ mkdir git
$ mkdir git-{name}
```

## 2. git global config 설정

git global config 설정으로는 자주 사용하는 github 계정으로 설정한다.

회사 프로젝트를 사용하는 빈도가 훨씬 높기 때문에 나는 global 계정 설정은 회사 계정으로 설정했다. 아래 설정은 커밋할 때 계정에 대한 정보가 어떤걸로 입력될지를 설정한다.

```
git config --global user.name "{이름}"
git config --global user.email "{이메일}"
```

## 3. ssh 키를 생성하자.

github 계정에 로그인하고 