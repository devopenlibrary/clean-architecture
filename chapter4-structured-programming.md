# 4장 구조적 프로그래밍

_에츠허르 비버 데이크스트라는 1930년 로테르담에서 태어났다. 그는 1955년 프로그래머로 일한지 3년이 지났을 때, 프로그래밍에 대한 지적 도전이 이론 물리에 대한 지적 도전보다 더 큰 의미가 있다는 결론을 내렸다._

_데이크스트라는 진공관 시대에 자신의 경력을 시작했다. 이 시대는 컴퓨터가 거대하고, 쉽게 손상되며, 느리고, 결과마저 믿을 수 없는 극도로 제한적인 기계였다. 데이크스트라는 이처럼 원시적인 환경에서 위대한 발견을 해냈다._

## 증명

데이크스트라가 초기에 인식한 문제는 **프로그래밍은 어렵다는 것**이었다. 모든 프로그램은 인간의 두뇌로 감당하기에는 너무 많은 세부사항을 담고 있었다.

데이크스트라는 이러한 문제를 증명이라는 수학적인 원리를 적용하여 해결하려했다. 증명을 통해 입증된 구조를 이용하고, 이들 구조를 코드와 결합시키며, 그래서 코드가 올바르다는 사실을 스스로 증명하게 되는 방식이었다.

그 연구 방법으로 단순한 알고리즘에 대한 기본적인 증명이 필요했고 그렇게 하기 위해 모듈을 재귀적으로 분해하는 과정이 필요했다. 하지만, goto 문장은 이러한 과정(모듈을 더 작은 단위로 분해)에 방해가 되는 경우가 많았다.

> goto 문은 프로그램의 실행 흐름을 코드 내 특정 위치(레이블)로 직접 이동시키는 명령어이다.

반면, goto 문장을 사용하더라도 모듈을 분해할 때 문제가 되지 않는 경우도 있었다. 이런 **goto 문장의 '좋은' 사용방식은 if/then/else, do/while과 같은 분기와 반복이라는 단순한 제어 구조에 해당하는 사실을 발견했다**.

만약 모듈이 이러한 단순한 제어 구조만을 사용한다면 증명가능한 단위로까지 모듈을 재귀적으로 분해하는 것이 가능해 보엿다.

사실 이러한 제어 구조는 뵘과 야코피니가 먼저 발견했는데, 이 두명은 모든 프로그램을 순차, 분기, 반복 이라는 세 가지 구조만으로 표현할 수 있다는 사실을 증명했다.

이 발견은 놀라웠다. 이 발견은 **"모듈을 증명 가능하게 하는 제어구조와 모든 프로그램을 만들 수 있는 제어 구조의 최소 집합과 동일하다"** 는 것을 의미했기 때문이다. 구조적 프로그래밍은 이렇게 탄생했다.

데이크스트라는 각 제어 구조를 다음과 같이 증명했다.

- 단순한 열거법을 이요해 **순차** 구문이 올바름을 입증할 수 있다는 사실을 보여주었다.
- 열거법을 재적용하는 방식으로 처리해, **분기**를 통한 각 경로가 수학적으로 적절한 결과를 만들어낸다면, 증명은 신뢰할 수 있게 된다.
- **반복**은 귀납법과 열거법을 사용하여 증명하였다.

## 해로운 성명서

1968년 데이크스트라는 CACM(Communications of the ACM)편집자에게 편지를 썼고, 그 내용이 같은 해 3월호에 실렸다. 그리고 프로그래밍 세계는 불이 붙었다. 그렇게 전쟁이 시작되었고 전쟁은 10년 이상 지속되었다.

결론은 데이크스트라가 승리했다. 현재의 우리 모두 구조적 프로그래머이다.

## 기능적 분해

구조적 프로그래밍을 통해 모듈을 증명 가능한 더 작은 단위로 재귀적으로 분해할 수 있게 되었고, 이는 결국 **모듈을 기능적으로 분해할 수 있음**을 뜻했다.

즉, **거대한 문제 기술서를 받더라도 문제를 고수준의 기능들로 분해할 수 있다. 그리고 이 고수준의 기능들은 저수준의 기능들로 분해**할 수 있다. 이는 끝없이 재귀적으로 가능하다.

이것을 통해 프로그래머는 대규모 시스템을 모듈과 컴포넌트로 나눌 수 있고, 모듈과 컴포넌트는 입증 가능한 아주 작은 단위의 기능들로 세분화 할 수 있다.

## 엄밀한 증명은 없었다

하지만 끝내 증명은 이루어지지 않았다. 대개의 프로그래머들은 세세한 기능 하나하나를 엄밀히 증명하는 고된 작업에서 이득을 얻으리라고 보지 않았다. 

오늘날 엄밀한 증명이 고품질의 소프트웨어를 생산하기 위한 적절한 방법이라고 믿는 프로그래머는 이제 거의 없다.

다행히도 무언가가 올바른지를 입증할 때 사용되는 상당한 성공한 또 다른 전략으로, **과학적 방법**이 있다.

## 과학이 구출하다

과학과 수학은 근본적으로 다른데, 과학 이론과 법칙은 그 **올바름을 절대 증명할 수 없기 때문이다.** 즉, 과학적 접근은 반증은 가능하지만 증명은 불가능하다.

과학은 서술된 내용이 사실임, 올바른지를 증명하는 방식이 아니라 서술이 틀렸음을 반증하는 방식으로 동작한다. 각고의 노력으로도 반례를 들 수 없는 서술이 있다면 목표에 부합할 만큼은 참이라고 본다.

결론적으로 수학은 증명 가능한 서술이 참임을 입증하는 원리인 반면, 과학은 증명 가능한 서술이 거짓임을 반증하는 원리라고 볼 수 있다.

## 테스트

데이크스트라는 **"테스트는 버그가 있음을 보여줄 뿐, 버그가 없음을 보여줄 수는 없다"**고 말한 적이 있다. 다시말해 어떤 프로그램이 맞다고 증명할 수 있는 방법은 없다. 테스트에 충분한 노력을 들였다면 프로그램이 목표에 부합할 만큼은 충분히 참이라고 여기는 것 뿐이다.

이는 앞서 말한 과학적 방법인 반증을 반복하여 찾아냄으로써 그 서술에 대해 입증하는 과정과 매우 유사하다. 따라서 **소프트웨어를 만드는 과정은 수학적인 시도보다는 과학에 가깝다.**

이러한 관점에서 구조적 프로그래밍은 프로그램을 증명 가능한 기능으로 재귀적으로 분해할 것을 강요한다. 그러고 테스트를 통해 분해한 기능들이 거짓인지를 증명하려고 시도한다. 만일 테스트들이 충분히 실패한다면, 이 기능들은 목표에 부합할 만큼은 충분히 참이라고 여기게 된다.

## 결론

구조적 프로그래밍이 오늘날까지 가치 잇는 이유는 프로그래밍에서 반증 가능한 단위를 만들어 낼 수 있는 능력 때문이다. 또한 현대적 언어가 아무런 제약없는 goto 문장은 지원하지 않는 이유이기도 하다. 

**가장 작은 기능에서부터 가장 큰 컴포넌트에 이르기까지 모든 수준에서의 소프트웨어는 과학과 같다. 따라서 각 모듈, 컴포넌트는 서비스가 쉽게 반증 가능하도록, 즉 쉽게 테스트 가능하도록 만들어져야한다.**