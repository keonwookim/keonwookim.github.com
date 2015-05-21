---
layout: post
title: "Javascript - Undefined and null"
---

# Undefined and Null

## Comparison between Undefined and Null

Javascript 에는 undefined와 null 이라는 것이 존재한다. undefined와 null은 얼핏 보기에 비슷해보인다.  최근 Javascript 를 사용할 일이 많아지면서 그런가보다 하고 사용했었다. 이번 포스트에는 undefined와 null에 대해서 정리해보고자 한다. 컴퓨터 관련 전공자라면 'null'이라는 키워드는 자주 보았을것이다. 보통 null은 '값이 없다' 또는 '아무 값도 나타내지 않는다.' 라는 의미를 가진다.

Javascript의 경우를 다음의 예제를 통해서 보자

{% highlight javascript %}
var dooray;
var orange = null;
{% endhighlight %}

위의 두 변수 선언은 차이가 있다. 첫번째 줄의 경우 dooray라는 변수를 선언만하고 값을 할당하지 않았다. 두번째 경우는 orange라는 변수를 선언하고 null이라는 값을 할당하였다. 눈치 챘겠지만 첫번째의 경우가 undefined의 경우이다. 선언만 하고 값을 할당하지 않은 변수에 접근을 하면 undefined를 뱉는다. 두번째의 경우는 orange에 어떤 유효한 값을 대입하지 않았을 뿐 null이라는 값을 가지게 된다.

객체의 경우에는 살짝 다른데, 일반 변수에서는 선언되지 않은 변수에 접근하면 오류를 뱉지만, 객체에서는 선언되지 않은 객체 프로퍼티도 undefined 이다.

## Typecasting of Undefined and Null
Javascript는 유연한 형변환을 지원한다. undefined와 null의 경우에도 형변환을 지원한다.

### Boolean
undefined와 null이 boolean 타입으로 형변환될 경우 다음과 같이 형변환된다.

	undefined => false
    null => false

둘다 false로 형변환된다.

### 숫자
undefined와 null이 숫자 타입으로 형변환될 경우 다음과 같이 형변환된다.

	undefined => NaN
    null => 0

숫자의 경우에는 차이를 보인다. undefined의 경우에는 NaN으로 형변환이 된다. 잘 알다시피, NaN은 'Not a Number'의 약자로 숫자가 아닌값을 나타낸다.

### 문자열
undefined와 null이 문자열 타입으로 형변환될 경우 다음과 같이 형변환된다.

	undefined => "undefined"
    null => "null"
단순히 각각 "undefined"와 "null"로 변환되어 반환된다.

## Equality Check of Undefined

undefined를 체크하는 방법에 대해서 알아보자.

{% highlight javascript %}
var orange;

if (orange === undefined) {
	//실행됨
} else {
	//실행되지 않음
}
{% endhighlight %}

orange는 선언만 되었을뿐 아무런 값이 할당되지 않았으므로 undefined 이다.

다음은 typeof를 이용한 방법이다.

{% highlight javascript %}
var dooray;
if (typeof dooray === 'undefined') {
	//실행됨
}
{% endhighlight %}

dooray 역시 선언만 되었을뿐 아무런 값이 할당되지 않았으므로 undefined 이다.


