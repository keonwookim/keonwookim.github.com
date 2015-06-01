---
layout: post
title: "Javascript - with"
---

# with?
지난번 포스트에서 살펴본 eval()과 함께 with는 strict mode에서 사용이 제한된다. 이번 포스트에서는 with가 무엇인지 알아보고 왜 strict mode에서 제한되는지 (쓰면안되는지..) 알아보도록 하자. 

with 구문은 스코프 체인을 연장해준다. 무슨말인고 하면 with 구문을 사용하면 스코프체인을 생성하게 된다. 용법은 다음과 같다.

{% highlight javascript %}

with (expression) {
	//statements
}

with (expression) 
	statement;

{% endhighlight %}

위 예제와 같은 형태로 사용한다. expression으로 인자를 넘겨줄수있고 해당 블럭안에 스코프 체인이 추가되게된다. 예제를 보도록하자.

{% highlight javascript %}

var dooray = {
	name = "orange",
	domain = "orange.dooray.com"
}

with (dooray) {
	console.log(name);
	console.log(domain);
}

{% endhighlight %}

위 예제와 같이, dooray 객체에대한 스코프 체인이 생김으로서 dooray의 속성들을 바로 접근할수 있게 된다.

# with is evil?

그렇다면 with는 왜 사용을 권하지 않는지에 대해서 알아보도록 하자. 우선 성능면에서 장단점을 삺펴보자. with를 쓴다면 앞의 예제에서 볼수 있듯이 인자로 넘긴 객체의 속성을 바로 접근하기 때문에 소스코드가 짧아진다. 파일사이즈가 작아지면서 성능상으로 이득을 가져올수 있다. (파싱하는 비용이 적게 들것이다.) 단점으로는 스코프 체인이 생성되면서 생기는 오버헤드가 존재할것이다. 또한 상위 스코프 객체에 접근할일이 생긴다면 해당 객체를 찾는 비용이 늘어나게 된다. 성능상으로 문제가 생길수 있지만 이보다 더 안좋은 상황이 벌어질수가 있다. 안좋은 상황이라 함은 모호성이다. 아래 예제를 보자.

{% highlight javascript %}

function f(x, o) {
	with (o) {
		print(x);
	}
}

{% endhighlight %}

예제의 print(x); 에서 x 는 어떤 x인가? f 함수에서 인자로 받은 x일 수도 있고, o객체의 속성일 수도 있다. 이런 모호성은 작성자만이 무엇인지 알수 있게 하고, 심지어 나중에는 기억이 안나서 한참 해메는 상황을 발생시킬수 있을것이다.
여기까지 with에 대해서 알아보았다. 위의 내용에서 보았듯이 가능하면 안쓰는것이 좋을것 같다.

