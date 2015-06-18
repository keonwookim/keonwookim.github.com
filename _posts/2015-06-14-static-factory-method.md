---
layout: post
title: "Static Factory Method"
---

# Static Factory Method
클래스의 인스턴스를 생성하는 가장 일반적인 방법은 public생성자를 이용해서 생성하는 방법이다. 다른 하나의 방법은 클래스에 Public static factory method를 작성하는 것이다. 이것저것 검색해보다가 우연히 발견했다. 이전에 Java 공부를 위해 사두었던 Effective Java의 항목 1이다. 책 안읽은 것이 이렇게 들통난다...

Effective Java 에서는 다음 예제를 보여주고 있다.

{% highlight java %}
public static Boolean valueOf(boolean b) {
	return b? Boolean.TRUE : Boolean.FALSE;
}
{% endhighlight %}

boolean 형 값을 받아서 Boolean객체 참조를 넘겨준다.
다른 예로는 다음이 있다.

{% highlight java %}
public class Car {
	private static Car car;
    private Car() {}
    
    public static Car getInstance() {
    	if(car == null) {
        	car = new Car();
        }
        return car;
    }
}
{% endhighlight %}

위의 예제를 보면 인스턴스는 getInstance() 메서드를 통해서 인스턴스 생성할 수 있다. 또한, car가 null일 경우만 생성하므로 하나의 인스턴스만 생성된다. 싱글톤 패턴과 유사하다. 이것이 첫번째 장점이다. 호출될때마다 매번 새로운 객체를 생성할 필요가 없다. 두번째 장점으로는 생성자와는 달리, 자신이 반환하는 타입의 어떤 하위객체도 반환할 수 있다는 점이다. 세번째 장점으로는 매개변수화 타입의 인스턴스를 생성하는 코드를 간결하게 해준다. 이해가 잘 안되는데. 예제를 보자.

{% highlight java %}
	Map<String, List<String>> m = new HashMap<String, List<String>>();
{% endhighlight %}

이런 식으로 매개변수가 늘어나면 그만큼 타이핑 할 분량이 많아지고, 외관상으로도 복잡해져서 이해하기 힘들어진다. 다음 예제를 보자.

{% highlight java %}
public static <K, V> HashMap<K, V> newInstance() {
	return new HashMap<K, V>();
}
{% endhighlight %}

타입 추론을 이용해서 위와 같은 메서드를 만들어 놓는다면, 다음과 같은 형태로 간결하게 이용할 수 있다.

{% highlight java %}
Map<String, List<String>> m = HashMap.newInstance();
{% endhighlight %}

static factory method는 다음과 같은 단점을 가진다고 한다.
* 인스턴스 생성을 위해 static factory method만 갖고 있으면서 public이나 protected 생성자가 없는 클래스는 서브클래스를 가질 수 없다.
* 다른 static 메소드랑 구별하기가 쉽지 않다.