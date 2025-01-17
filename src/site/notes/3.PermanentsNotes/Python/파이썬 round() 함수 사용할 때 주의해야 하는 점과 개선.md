---
{"dg-publish":true,"permalink":"/3-permanents-notes/python/round/","created":"2024-12-22T00:22:24.875+09:00","updated":"2024-12-24T16:39:30.871+09:00"}
---

#경험담 

이 문제를 구현하다 있었던 일입니다.

[18110번: solved.ac](https://www.acmicpc.net/problem/18110)

`round()` 함수를 사용해 풀려고 시도했지만 **틀렸습니다!** 가 떴습니다.
이를 알고 싶어 찾아보니, `round()` 함수는 [오사오입으로 구현](https://puleugo.tistory.com/43)되어 있다고 말했습니다.

만약에 `round()`가 우리가 생각하는 것과 전혀 다른 방식으로 구현되어 있다면 그것은 반드시 공식 문서에서 언급했을 것입니다. 
이를 어떻게 표현했는지 알고 싶어 Python 함수의 [`round()` 공식 문서](https://docs.python.org/3/library/functions.html#round)를 읽어보았습니다.

파이썬 공식 문서엔 다음과 같이 쓰여있습니다.

>만약 `ndigits` 가 생략되거나 `None`이라면, input의 `nearest integer`를 반환한다.
>`round()`를 지원하는 유형이라면, 값은 $10^{-ndigit}$ 의 배수와 가깝게 반올림된다.
>만약 10진수로 가까워지는 방법이 완전히 동일할 경우, `even`한 방법을 취한다.

## Nearest integer? even한 방법? 

이를 알기 위해 위키피디아 round 영문 문서를 찾아봤습니다. [위키피디아 영문 문서](https://en.wikipedia.org/wiki/Rounding#)를 찾아보면, 반올림을 할 때 생기는 여러가지 문제점과 반올림의 방법을 소개하고 있습니다. Python에서 제공하는 형태의 `round()`는  `nearest integer`를 반환하는 형태이므로, Rounding to integer의 **[Rounding to the nearest integer](https://en.wikipedia.org/wiki/Rounding#Rounding_to_the_nearest_integer)** 부분 중 `even` 한 방법을 보면 될 것입니다.

이를 보면 다음과 같이 설명하고 있습니다.

>긍정적/ 부정적 편향이 없고, 0에 대한 편향이 없는 tie-breaking rule을 취한다.

예를 들면,

| +11.5 | 12.5  | -11.5 | -12.5 |
| ----- | ----- | ----- | ----- |
| +12.0 | +12.0 | -12.0 | -12.0 |
0.5에서 올림하지 않고 `even`하게 올리는 이유가 무엇일까요?
입력값이 대부분 양수이거나 대부분 음수인 경우 0.5를 올림한 값을 그냥 더하게 되면 전체적인 결과가 커질 수 있습니다. `even` 하게 올리면 반올림한 숫자들을 합산할 때 생기는 오차를 최소화할 수 있습니다.

>[!Tip] Rounding half to even 을 다른 말로 뭐라 할까요?
>- **convergent rounding**
>- **statistician's rounding**
>- **Dutch rounding**
>- **Gaussian rounding**
>- **[odd–even rounding](https://en.wikipedia.org/wiki/Rounding#cite_note-7)** 
>- **bankers' rounding**


>[!Tips] Rounding의 종류
>![Pasted image 20241223050204.png](/img/user/AttachedFiles/Pasted%20image%2020241223050204.png)
>- 우리가 일상 생활에서 많이 쓰는 방법은 `Rounding half away from zero` 이라는 것을 알 수 있다.
>- 파이썬과 자바에서 `half up` 이라는 표현은 `Rounding half away from zero`를 의미한다.

## Floating point

위의 문제를 알았으니 우리가 원하는 방법으로 반올림하기 위해 다음과 같은 코드를 작성해보겠습니다.

```python
def round(n):
	return int(n) + 1 if n - int(n) >= 0.5 else int(n)
```

이렇게 한다면 `round()` 를 했을 때 소수 부분에 해당하는 부분으로 판단할 것입니다.
이렇게 하면, [18110번: solved.ac](https://www.acmicpc.net/problem/18110) 에선 전혀 문제될 것이 없습니다. 하지만 이렇게 코드를 짜게 되면 다른 문제가 생깁니다.
사실 이 문제는 [부동 소수점 표현](https://docs.python.org/ko/3/tutorial/floatingpoint.html#tut-fp-issues)에서 발생하는 문제입니다. 우리가 현실 세계에서 쓰는 소수는 10진법으로 되어있습니다. ($0.1 = 10^{-1}$ 로 표현된다.) 하지만 모든 기계는 소수를 2진법으로 인식합니다. (기계에서 $0.1 = 2^{-1}$  로 표현된다.) 실제 표기된 값과 굉장히 **가깝긴** 하겠지만, 실제 그 값이 될 순 없습니다. 기계가 쓰는 소수로 우리가 쓰는 소수를 완벽히 변환할 수 없다면 기계가 쓰는 소수와 가까운 값으로 저장해둘 것입니다. 이로 인해 오차가 발생할 것입니다.  실제로 python에서 `0.1 + 0.1 + 0.1 == 0.3` 을 실행시키면 `false`가 뜹니다.
이와 비슷한 문제로, `무한 소수`를 어떻게 정의 하는가에 대한 문제가 있습니다.
파이썬에서 특별하게 적지 않는 이상, 부동 소수점 표현은 IEEE 754 표준인 `double presicion`을 기준으로 하고 있습니다. 
Double Precision은 다음과 같은 형태로 부동 소수점이 표현된다는 것을 의미합니다.

![Pasted image 20241223010249.png](/img/user/AttachedFiles/Pasted%20image%2020241223010249.png)
https://en.wikipedia.org/wiki/Double-precision_floating-point_format
여기서 sign 은 부호를, exponent는 정수 부분을, fraction은 소수 부분을 의미합니다.
우리가 주목해야 할 부분은 소수 부분입니다. 소수 부분은 52bit로 되어있습니다.
대부분의 기계들은 1bit로 0과 1만 표현할 수 있습니다. 따라서 fraction으로 표현할 수 있는 수는 $2^{52}$ 개일 것입니다. 
반대로 현실 세계처럼 한 자리에 10개를 표현할 수 있다고 가정합니다. 그리고 그 자리가 n개 있다고 가정합시다. 이 칸들로 fraction들로 표현할 수 있는 수들을 전부 표현한다고 가정하면 다음과 같은 방정식을 만족합니다.
$$10^{n} = 2^{52}$$
양 변에 로그를 취해주면 다음과 같습니다.
$$n =52log_{10}^{2} \approx 15.955$$
우리는 fraction에서 표현하고 있는 칸의 개수가 자리 수라는 사실을 알고 있습니다.
따라서 n을 10진법의 자리수라고 표현해도 무방할 것입니다. 이는 소수 부분 중 15 자리수 정도만 저장할 수 있다는 사실을 의미합니다. 다시 말하자면, 소수 부분이 16자리를 넘어간다면, 이는 `double precision` 방법으로 저장할 수 없습니다. 파이썬은 입력 시 `double precision`을 사용할 수 없는 형태라면 `double precision` 을 사용할 수 있는 형태로 변환합니다. ($\displaystyle \frac{J}{2^n}$  형태로 변환합니다. 단 $J$는 53bit를 포함하는 정수여야 합니다. 자세한 내용은 [여기](https://docs.python.org/ko/3/tutorial/floatingpoint.html#tut-fp-issues)를 참조하세요)

따라서, **1.4999999999999999 == 1.5** 가 되기 때문에, `round(1.4999999999999999)`는 2가 되어버리고 맙니다. 그렇다면 어떻게 구현하는 것이 옳을까요?

## Decimal을 사용하자!

Decimal을 사용하면 다음과 같은 장점이 있습니다.
- 빠르고 정확하게 반올림된 10진수 부동 소수점 연산입니다.
- 사람들이 학교에서 배우는 산술과 동일한 방식으로 작동하는 산술을 제공해야 합니다. - [General Decimal Arithmetic Specification](https://speleotrove.com/decimal/decarith.html)
- 엄격하게 수치를 계산해야 하는(strict equality invariants) 곳에서 좋습니다.
- 유저가 변경할 수 있는 정밀도(기본값은 28자리)를 가지고 있습니다.
- 표준에서 필요한 모든 부분을 노출합니다. 
- exact unrounded decimal arithmetic 과 rounded floating-point arithmetic을 지원합니다.

많은 사용 방법이 있겠지만, 우리가 원하는 것은 어떻게 Decimal로 반올림하는가? 일 것입니다. 이에 따라 기본적인 사용법만 설명합니다.

- `getcontext()` - 현재 프로그램에서 decimal에 설정되어있는 변수를 보여줍니다.

```python
from decimal import *
getcontext() 
```

기본값은 다음과 같습니다.
```cmd
Context(prec=28, rounding=ROUND_HALF_EVEN, Emin=-999999, Emax=999999,
        capitals=1, clamp=0, flags=[], traps=[Overflow, DivisionByZero,
        InvalidOperation])
```

Context에 있는 key로 호출해 직접 변경할 수 있습니다.
```python
getcontext().prec = 7
```

이를 바탕으로 반올림 방법을 변경할 수 있습니다.
```python
context = getcontext()
context.rounding = ROUND_HALF_UP
```

rounding 방법을 변경했다면, 숫자를 `Decimal` 객체로 변경합니다. 그 후 `round()`를 해주면 됩니다. 
또한, Decimal 객체는 다른 자료형으로 변경할 수 있습니다.
```python
float(Decimal('1.1')) #1.1
int(Decimal('1.1')) #1
```

**주의사항**
**1. Decimal에 유리수를 반드시 String 형태로 넣어야 합니다.**
그렇지 않으면 위에서 언급한 floating point 문제가 그대로 일어납니다.
```python
round(Decimal("1.4999999"),0)
```

**2. round()에 ndigit을 설정해야 합니다.**
`round()`에 *ndigit*을 설정하지 않을 경우, 가장 가까운 정수를 돌려준다는 점입니다. 이 때 사용되는 방법도 역시 `even` 한 방법입니다. 따라서 Decimal에 설정된 원칙으로 반올림되게 만들고 싶다면 *ndigit*을 반드시 세팅해줘야 합니다.

**아쉬운 점**
**1. 메모리가 많이 듭니다.**
밑의 두 코드를 실행시켜보면 float은 24byte이지만, Decimal 객체는 104byte임을 알 수 있다.  따라서 Decimal 객체를 사용할 때는 메모리에 유의해야 한다.

```python
import sys

sys.getsizeof(1.1) #24
sys.getsizeof(Decimal('1.1')) #104
```

**2. 시간이 많이 듭니다.**

`timeit` 모듈을 사용하여 두 모듈을 비교해보겠습니다.

```python
import timeit
from decimal import Decimal

# 일반 부동소수점 연산
float_time = timeit.timeit(
    stmt="x * y + z",
    setup="x = 1.1; y = 2.2; z = 3.3",
    number=1000000
)

# Decimal 연산
decimal_time = timeit.timeit(
    stmt="x * y + z",
    setup="from decimal import Decimal; x = Decimal('1.1'); y = Decimal('2.2'); z = Decimal('3.3')",
    number=1000000
)

print(f"일반 부동소수점 연산 시간: {float_time:.6f}초")
print(f"Decimal 연산 시간: {decimal_time:.6f}초")

```

결과는 다음과 같습니다.

```cmd
일반 부동소수점 연산 시간: 0.037404초
Decimal 연산 시간: 0.128956초
```

약 3배 정도 차이를 보입니다. 
`dis.dis()`로 두 셋업을 비교한 결과는 다음과 같습니다.

![Pasted image 20241223052905.png](/img/user/AttachedFiles/Pasted%20image%2020241223052905.png)

![Pasted image 20241223052947.png](/img/user/AttachedFiles/Pasted%20image%2020241223052947.png)

Decimal을 불러오는 부분이 상당히 많습니다. 모듈에서 class를 불러오는 과정에서 시간이 많이 소요되는 것으로 추측하고 있습니다.

## 결론

결론은 다음과 같습니다.
- Python의 `round()`은 **Rounding half to even** 전략을 따르고 있다.
- 시간과 메모리가 충분할 때 우리가 생각하는 반올림을 구현하기 위해선 Decimal을 써라! 

## 참고

위의 글을 바탕으로 [18110번: solved.ac](https://www.acmicpc.net/problem/18110)의 정확한 구현은 Decimal이 맞을 것입니다. 그런데 이 문제에 한해선 그냥 단순히 정수 부분 + 소수점 부분으로 나눠도 됩니다. 왜냐면 이 문제에서 요구하는 반올림은 정수 부분까지 반올림을 하는 것이기 때문입니다. 정수 부분까지 반올림을 하기 위해선 소수점 첫째 자리가 중요해집니다. 반대로 말해서, 소숫점 둘째 자리부터 정확할 필요는 없습니다. 만약 소숫점 둘째 자리였다면 $5 \times 10^{-2}$ 이기 때문에 명확히 2진법으로 변환되지 않았겠지만, 소숫점 첫째 자리는 $2 ^{-1}$ 로 정확히 변환됩니다. 그래서 굳이 Decimal로 구현할 필요는 없습니다.

참고 내용
- [What’s New In Python 3.1 — Python 3.13.1 documentation](https://docs.python.org/3/whatsnew/3.1.html)
- [파이썬 round() 반올림 사용 시 주의점 : 네이버 블로그](https://m.blog.naver.com/PostView.nhn?blogId=herbdoc95&logNo=221574077380&proxyReferer=http:%2F%2Fblog.naver.com%2FPostView.nhn%3FblogId%3Dherbdoc95%26logNo%3D221574077380) 
- [Built-in Functions — Python 3.13.1 documentation](https://docs.python.org/3.13/library/functions.html#round)
- [Rounding - Wikipedia](https://en.wikipedia.org/wiki/Rounding)
- [python - 1 == 2 for large numbers of 1 - Stack Overflow](https://stackoverflow.com/questions/19965093/1-2-for-large-numbers-of-1)
- [Use shorter float repr when possible · Issue #45921 · python/cpython · GitHub](https://github.com/python/cpython/issues/45921)
- [보다 정확한 숫자 계산](https://slides.com/hosunglee-1/deck-10#/5/7/0)
- [Double-precision floating-point format - Wikipedia](https://en.wikipedia.org/wiki/Double-precision_floating-point_format)

Decimal에 대하여
- [019 소수점을 정확하게 계산하려면? ― decimal.Decimal - 점프 투 파이썬 - 라이브러리 예제 편](https://wikidocs.net/106276)
- [파이썬 round() 반올림 사용 시 주의점 : 네이버 블로그](https://m.blog.naver.com/PostView.nhn?blogId=herbdoc95&logNo=221574077380&proxyReferer=http:%2F%2Fblog.naver.com%2FPostView.nhn%3FblogId%3Dherbdoc95%26logNo%3D221574077380)