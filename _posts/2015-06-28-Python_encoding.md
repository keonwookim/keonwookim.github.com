---
layout: post
title: "Python - 한글 인코딩"
---

#Python 한글 인코딩

멀티바이트로 적용되는 한글을 이용하게 되면  인코딩 문제에 어떻게든 마주치게 되는것 같다. (python 3버전대에서는 unicode로 통일되어서 그나마 괜찮고 한다.) python의 기본 인코딩은 ascii 이다. 때문에 python 스크립트를 ascii 인코딩이라 가정하고 읽어서 파싱을 하게된다. 그러나 스크립트내에 한글이 들어간다면 ascii로 해석이 불가능하기 때문에 에러를 내뱉고 실행이 중지될것이다. 이것의 해결방법은 다음과 같다.

{% highlight python %}
#-*- coding: utf-8 -*-
{% endhighlight %}

스크립트 파일에 위의 내용을 입력하고 실행하면 된다. 위의 내용을 첫줄에 입력함으로서 이 스크립트는 ascii 가 아닌 utf-8로 인코딩 되었다고 알려주게 된다.
물론 위의 방법은 스크립트 내의 인코딩 방식을 알려주는것이며, 여전히 문자열은 유니코드로 취급되지 않고 1바이트의 문자로 취급되게 된다. 이 문제를 해결하려면 다음과 같이 하면 된다.

{% highlight python %}
a = u"안녕하세요"
{% endhighlight %}

문자열의 앞에 'u'를 붙여주면 된다. 하지만 모든 문자열에 'u'를 명시적으로 적어주기는 불편하고 번거롭다. 해서 인코딩 디코딩시 사용할 기본 인코딩을 ascii 대신 utf-8로 바꿔 줄수있다. 다음과 같이 이용하면된다.

{% highlight python %}
#-*- coding: utf-8 -*-
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
{% endhighlight %}

위의 내용을 스크립트의 최상단에 넣어주면 된다.
