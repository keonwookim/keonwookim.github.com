---
layout: post
title: "Javascript - strict mode"
---

# Strict mode

현재 개발하고 있는 Javascript코드의 맨 윗 줄은 'use strict'라는 문구로 시작한다. 이것의 용도는 무엇인가? 대충 무언가를 제한하는 용도로 이용 되는것 같아 보인다. 이 'use strict'라는 문구는 ECMAScript5 부터 적용되는 키워드라고 한다. 다시 말해 'use strict' 키워드를 선언해 놓으면 안전한 Javascript 코딩을 위해서 제한되는 것들이 생긴다. 예를 들면, Javascript 에서는 변수를 선언하지 않고 사용해도 에러가 발생하지 않는데, 'use strict'를 선언해 놓고 변수를 선언하지 않으면 에러를 뱉어낸다. 엄격한 검사를 해주는 가이드라인 정도로 생각하면 될 것 같다.

# Strict mode를 이용하려면..
strict mode를 이용할 수 있는 방법은 두가지가 있다. 하나는 전역적으로 선언하는 방법과 함수내에서만 선언해서 사용하는 방법이다. 두가지 방법 모두 스크립트 최상위 또는 함수의 최상위에 위치하여야 한다. 다음은 전역적으로 사용하는 예제이다.

{% highlight javascript %}
"use strict";

function foo() {
	var abc;
    return abc;
}

// 전역적으로 사용되었으므로 변수 선언없이 사용할 수 없음, 에러 발생
abc = 365;
{% endhighlight %}

다음은 함수내에서 사용하는 예제이다.

{% highlight javascript %}

function bar() {
	"use strict";
    //함수내에서 사용되었기 때문에 변수 선언없이 사용할 수 없음, 에러 발생
    abc = 365;
    return abc;
}

abc = 563;

{% endhighlight %}


# Strict mode 의 제한
Strict mode에서 제한하는 경우를 살펴보도록 하겠다.

1. 변수를 선언하지 않고 사용할 때
2. 변수, 함수, 매개변수 등을 삭제하려할 때
3. 프로퍼티 중복 선언 시
4. 매개변수 중복 선언 시
5. 숫자리터럴에 8진법 수를 할당하려할 때, 특수문자를 할당하려할 때
6. 읽기전용 프로퍼티(writable:false)에 값을 할당하려할 때
7. with 키워드 사용 시
8. eval() 사용 시

