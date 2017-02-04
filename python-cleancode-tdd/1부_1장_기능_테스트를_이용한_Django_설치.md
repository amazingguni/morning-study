# 1장. 기능 테스트를 이용한 Django 설치

이 책에서 앞으로 나오는 `테스팅 고트`님은 이 책에서 성질 고약하고 비합리적인 스승이다.

## 테스팅 고트님께 복종하라! 테스트가 없으면 아무것도 하지 마라!

테스팅 고트님은 파이썬 테스팅 커뮤니티의 비공식적인 TDD 마스코트이다.

- 이 책을 통해 여러분 내면에도 테스팅 고트님이 임재하기를 바란다.

우선 이 책의 목적으 웹 사이트를 만드는 것이다.

그렇기 때문에 웹 프레임 워크를 설치하고 설정한다.

> 염소가 "테스트를 먼저 해, 테스트를 먼저 하라고" 라면서 울고 있다는 것을 항상 잊어서는 아니된다.

여기서도 먼저 테스트를 작성하고 실패하는 것을 확인한 이후에 웹 구축 단계로 넘어가도록 한다.

> 염소의 목소리로 반복해서 위 메시지를 되뇌여라. 한번에 한 테스트만 진행하며 단계를 작게 나누어서 조금씩 진행해라.

### selenium으로 테스트 생성

1. 파이어폭스 브라우저 창을 실행하기 위해 셀레늄의 webdriver를 가동한다.
2. 브라우저를 통해 로컬 PC상의 웹 페이지를 연다
3. 웹 페이지 타이틀에 `Django`가 있는지 확인한다.

```py
from selenium import webdriver

browser = webdriver.Firefox()
browser.get('http://localhost:8000')

assert 'Django' in browser.title
```

```bash
$ python functional_test.py
Traceback (most recent call last):
  File "functional_test.py", line 1, in <module>
    from selenium import webdriver
ModuleNotFoundError: No module named 'selenium'

# selenium 이 없다. 설치한다.
$ pip install selenium
Collecting selenium
  Downloading selenium-3.0.2-py2.py3-none-any.whl (915kB)
    100% |████████████████████████████████| 921kB 838kB/s
Installing collected packages: selenium
Successfully installed selenium-3.0.2
```

> geckodriver가 없다면 아래 링크에서 다운받고 path 등록
[https://github.com/mozilla/geckodriver/releases](https://github.com/mozilla/geckodriver/releases)

당연히 테스트는 실패한다.

### Django 가동 및 실행

장고를 설치한다.

```bash
$ pip install django
```

웹 사이트의 메인 컨테이너가 될 프로젝트를 만들기 위해 djngo를 가동하고 실행한다.

```bash
$ django-admin.py startproject superlists
# 아래와 같이 프로젝트가 구성된다.
$ tree
├── functional_test.py
└── superlists
    ├── manage.py
    └── superlists
        ├── __init__.py
        ├── settings.py
        ├── urls.py
        └── wsgi.py
```

`manage.py` 는 다양한 역할을 하는데 그 중에는 개발 서버를 가동하는 역할도 있다.

```bash
$ python manage.py runserver
# 다른 커멘드라인 창에서 테스트 코드를 실행
$ python3 functional_tests.py
```

통과했다

### Git Repository 실행

마지막으로 버전 관리 시스템에 커밋을 하자.

- 코드를 관리하고, 이전 버전의 코드를 확인하고, 변경 내역을 복원하고, 백업하기 위해서.

```bash
$ ls
superlists
functional_tests.py
$ mv functional_tests.py superlists
$ cd superlists
# 원격 저장소에 업데이트를 진행한다.
```

다 되었으면 commit을 한다.

```bash
$ ls
$ echo "db.sqlite3" >> .gitignore
$ git add .
$ git status
$ git rm -r --cached superlists/__pycache__
$ echo "__pycache__" >> .gitignore
$ echo "*.pyc" >> .gitignore

$ git status
$ git add .gitignore
$ git commit
# First commit: First FT and basic Django config
```
