# 1부. TDD와 Django 개요

1. `1부`에서는 `테스트 주도 개발(TDD)`의 기본적인 내용을 다룬다.
2. 실제로 웹 어플리케이션을 구축해보고 각 단계별 테스트를 작성한다.
  - 단위 테스트
  - 셀레늄을 활용한 기능 테스트
3. `RED-GREEN-REFACTOR` 로 구성되는 TDD 흐름에 대해서 알아본다.
4. 또한 버전 관리 시스템으로 `Git`을 사용한다.
  - 언제 어떻게 커밋하고 이를 TDD 및 웹 개발 작업과 통합하는지 논의
5. 그리고 웹 프레임 워크로는 `Django`를 사용한다.
6. `pyenv` 를 사용하여 파이썬 환경을 격리시킨다.

### pyenv란?

pyenv lets you easily switch between multiple versions of Python  

- Let you change the global Python version on a per-user basis.
- Provide support for per-project Python versions.
- Allow you to override the Python version with an environment variable.

[https://github.com/yyuu/pyenv-installer](https://github.com/yyuu/pyenv-installer)

```bash
$ curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
$ pyenv update
```

아래 문구를 `.bashrc` 파일에 추가하고 터미널을 재 시작하면 pyenv 설치는 완료된다.

```bash
export PATH="~/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

### pyenv 설정하기

```bash
# 현재 최신버전인 python 3.6.0을 사용한다.
# 설치가 완료되면 pyenv 에서 3.6.0을 사용할 수 있다.
$ pyenv install 3.6.0
# 설치된 python 버전을 확인한다.
$ pyenv versions
* system (set by /Users/amazingguni/.pyenv/version)
  3.6.0
$ python --version
```

### pyenv-virtualenv 설치

[https://github.com/yyuu/pyenv-virtualenv](https://github.com/yyuu/pyenv-virtualenv)

### pyenv-virtualenv 설정

```bash
# 프로젝트별 가상 환경 생성
$ pyenv virtualenv 3.6.0 python-tdd-cleancode
# 가상환경 활성화
$ pyenv activate python-tdd-cleancode
# pip 자체 업그레이드(자주하면 좋음)
$ pip install --upgrade pip
# 가상환경 비활성화
$ pyenv deactivate
```
