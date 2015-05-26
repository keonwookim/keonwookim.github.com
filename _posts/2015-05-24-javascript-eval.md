---
layout: post
title: "Javascript - eval()"
---

# eval()?
eval 은 String 형태의 Javascript 코드를 동적으로 실행할 수 있게 해준다. 간단한 예로 다음과 같이 사용한다.

{% highlight javascript %}

var dateFn = "Date(2015,4,24)"
var date;
eval("date = new " + dateFn + ";");

document.write(date);
// Output: Thu May 24 00:00:00 PDT 2015

{% endhighlight %}

예제와 같은 방법으로 사용이된다. 상황에 따라서 유용하게 사용할 수 있을 것 같다는 생각이 든다. 그러나 앞선 포스트에서 언급되듯이 "use strict" 를 선언하게 되면 eval()을 사용할수 없다. 이번 포스트에서는 왜 eval()을 제한하는지에 대해서 알아보도록 하자.

# Eval is evil????

eval() 에 대해 구글에서 검색해보면 eval()에 대한 설명도 많이 나오지만.. 그 중 눈에 띄는것은 'Eval is evil' 이 눈에 들어온다. 딱봐도 좋지않게 생각한다는 것을 알수 있다. 가볍게 생각해봐도 당장 문제될것 같은 부분은 보안적인 이슈 일것이다. eval() 에 어떤 식으로든 악성코드를 포함한 문자열을 넘겨준다면 eval()은 해당 코드를 실행하게 될것이다. 그렇다. 보안적인 이슈가 발생할수 있다. 또 하나의 문제는 맨 윗줄에서 언급한  내용이다. 'eval()은 String 형태의 Javascript 코드를 동적으로 실행한다.' 이 부분인데, 동적으로 넘어온 스트링을 javascript 파서가 분석하고 실행하게 된다. 다시 말하면 Javascript 컴파일러가 최적화 할 수 없다는 말이다. 성능상의 이슈가 될수 있는 부분이다. 크게 이러한 이유들 때문에 권장해서 사용하고 있지 않은듯하다. 하지만 많은 Javascript 라이브러리들은 JSON을 Javascript 객체로 변환하는데 eval()을 쓰고 있는 듯하다. 따라서 IBM의 개발자 문서에 제시하고 있는 방법으로 다음과같은 정규식을 이용해서 JSON스트링을 검사하는 방법을 보여준다.

{% highlight javascript %}

var my_JSON_object = !(/[^,:{}\[\]0-9.\-+Eaeflnr-u \n\r\t]/.test(

    text.replace(/"(\\.|[^"\\])*"/g, ' '))) &&

    eval('(' + text + ')');

{% endhighlight %}

가장 속편한 방법은 JSON 파서를 이용하는 방법인 듯 하다.

