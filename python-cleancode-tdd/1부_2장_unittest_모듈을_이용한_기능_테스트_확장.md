# 2장. unittest 모듈을 이용한 기능 테스트 확장

작업 목록(To-Do) 사이트를 만들어 본다.

- 기본적이고 간단하고 최소한의 실행 가능한 애플리캐이션을 구축할 수 있다.
- 기능 및 UI 확장도 가능하다.

중요한 것은 웹 프로그래밍과 TDD 적용 방법을 배우기에 넘나 좋다는 것이다.

## 기능 테스트를 이용한 최소 기능의 애플리케이션 설계

**기능 테스트(Functional Test, FT)**

- 사용자 관점에서 애플리케이션이 어떻게 동작(Function)하는지 실제 웹 브라우저를 실행해서 확인한다.
- 즉 FT 자체가 사양이 될 수 있음을 의미한다.
- 사용자 스토리 라고 하는 방식을 따르는 경향이 있다.

> Functional / Acceptance / End-to-End test 가 애매하긴 한데, 여기서 중요한건 이 테스트가 외부 사용자 관점에서 확인하는 테스트라는 것이다. 블랙박스 테스트라고도 할 수 있다.

FT는 사용자 스토리를 가지고 있어야 하고 이를 주석으로 작성한다.

일반 사용자도 이해할 수 있을 정도로 사용자 스토리를 작성하는 것을 목표로 한다.

아래처럼 주석으로 우선 작성한다.

```py
from selenium import webdriver

browser = webdriver.Firefox()

# 에디스(Edith)는 멋진 작업 목록 온라인 앱이 나왔다는 소식을 듣고
# 해당 웹 사이트를 확인하러 간다
browser.get('http://localhost:8000')

# 웹 페이지 타이틀과 헤더가 'To-Do'를 표시하고 있다
assert 'To-Do' in browser.title

# 그녀는 바로 작업을 추가하기로 한다

# "공작깃털 사기"라고 텍스트 상자에 입력한다
# (에디스의 취미는 날치 잡이용 그물을 만드는 것이다)

# 엔터키를 치면 페이지가 갱신되고 작업 목록에
# "1: 공작깃털 사기" 아이템이 추가된다

# 추가 아이템을 입력할 수 있는 여분의 텍스트 상자가 존재한다
# 다시 "공작깃털을 이용해서 그물 만들기"라고 입력한다 (에디스는 매우 체계적인 사람이다)

# 페이지는 다시 갱신되고, 두 개 아이템이 목록에 보인다
# 에디스는 사이트가 입력한 목록을 저장하고 있는지 궁금하다
# 사이트는 그녀를 위한 특정 URL을 생성해준다
# 이때 URL에 대한 설명도 함께 제공된다

# 해당 URL에 접속하면 그녀가 만든 작업 목록이 그대로 있는 것을 확인할 수 있다

# 만족하고 잠자리에 든다

browser.quit()
```

테스트 코드를 실행하면 당연히 실패하지만 이제 우리는 실패하는 테스트 코드를 가지게 되었다!

```bash
(python-tdd-cleancode) ➜  python-cleancode-tdd git:(master) ✗ python functional_test.py
Traceback (most recent call last):
  File "functional_test.py", line 10, in <module>
    assert 'To-Do' in browser.title
AssertionError
```

## 파이썬 기본 라이브러리의 unittest 모듈

이제 테스트 프레임워크 사용이 필요할 때다

- AssertError 메시지만으로는 어떤 문제가 있는지 파악할 수 없다
- 테스트가 실패할 때 파이어폭스 창이 닫히지 않아 수동으로 닫아줘야 한다

```py
from selenium import webdriver
import unittest

# (1) unittest.TestCase를 상속받는다.
class NewVisitorTest(unittest.TestCase):
    # (2) setUp과 tearDown은 테스트 시작 전/후에 실행된다.
    def setUp(self):
        self.browser = webdriver.Firefox()

    def tearDown(self):
        self.browser.quit()

    # (3) test라고 시작되는 모든 메소드는 테스트 메소드이며 테스트 러너에 의해 실행된다.
    #     가능한 테스트 내용을 알 수 있도록 명칭을 작성하는 것이 좋다.
    def test_can_start_a_list_and_retrieve_it_later(self):
        # 에디스(Edith)는 멋진 작업 목록 온라인 앱이 나왔다는 소식을 듣고
        # 해당 웹 사이트를 확인하러 간다
        self.browser.get('http://localhost:8000')

        # 웹 페이지 타이틀과 헤더가 'To-Do'를 표시하고 있다
        # (4) self.assertIn, assertTrue, assertEqual 등 다양한 assert 함수를 지원한다
        self.assertIn('To-Do', self.browser.title)
        self.fail('Finish the test!')
        # 그녀는 바로 작업을 추가하기로 한다

        # "공작깃털 사기"라고 텍스트 상자에 입력한다
        # (에디스의 취미는 날치 잡이용 그물을 만드는 것이다)

        # 엔터키를 치면 페이지가 갱신되고 작업 목록에
        # "1: 공작깃털 사기" 아이템이 추가된다

        # 추가 아이템을 입력할 수 있는 여분의 텍스트 상자가 존재한다
        # 다시 "공작깃털을 이용해서 그물 만들기"라고 입력한다 (에디스는 매우 체계적인 사람이다)

        # 페이지는 다시 갱신되고, 두 개 아이템이 목록에 보인다
        # 에디스는 사이트가 입력한 목록을 저장하고 있는지 궁금하다
        # 사이트는 그녀를 위한 특정 URL을 생성해준다
        # 이때 URL에 대한 설명도 함께 제공된다

        # 해당 URL에 접속하면 그녀가 만든 작업 목록이 그대로 있는 것을 확인할 수 있다

        # 만족하고 잠자리에 든다

# (5) 이 구문은 스크립트가 다른 스크립트에 의해 임포트 된 것이 아니라 커맨드라인을 통해 실행됬다는 것을 확인하는 코드이다.
if __name__ == '__main__':
    # (6) 불필요한 리소스 경고를 제거하기 위한 것
    unittest.main(warnings='ignore')

```

다시 테스트를 구동해본다.

```py
(python-tdd-cleancode) ➜  python-cleancode-tdd git:(master) ✗ python functional_test.py
F
======================================================================
FAIL: test_can_start_a_list_and_retrieve_it_later (__main__.NewVisitorTest)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "functional_test.py", line 22, in test_can_start_a_list_and_retrieve_it_later
    self.assertIn('To-Do', self.browser.title)
AssertionError: 'To-Do' not found in 'Welcome to Django'

----------------------------------------------------------------------
Ran 1 test in 3.121s

FAILED (failures=1)
```

조금 더 명확해졌다.

## Implicitly_wait 암묵적 대기

Firefox browser가 구동된 이후에 테스트를 진행하기 위해 암묵적 대기를 setUp에 추가한다.

- selenium은 비교적 안정적으로 페이지 로딩을 기다리지만 완벽하지는 않기 때문에 여기서는 3초 정도 대기하고 처리하도록 기술하고 있다.

```py
    def setUp(self):
        self.browser = webdriver.Firefox()
        self.browser.implicitly_wait(3)
```

## 다시 커밋할 시간이다.

- 기능 테스트에 우리가 설정한 스토리를 주석에 포함시켰다.
- unittest 모듈과 모듈에 포함된 테스트 함수들을 추가했다.

```bash
$ git status
$ git diff
# 커밋한 적이 있는 관리 중이였던 파일의 변경 내용을 반영하여 커밋한다.
$ git commit -a
# commit-msg : 첫 기능 테스트 주석 추가 및 unittest 사용
```

이제 작업 목록 애플리케이션을 만들기 위한 진짜 코드를 작성할 준비가 됐다.
