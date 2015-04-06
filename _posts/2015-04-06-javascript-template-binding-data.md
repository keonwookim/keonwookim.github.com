---
layout: post
title: "jQuery template binding data"
---
## 데이터 바인딩이 안되요. - jQuery template

### 들어가기전에

나는 자바스크립트 무식자다. 해본거라곤 학부 때 조금? 하여 jQuery역시 잘 모른다. (자랑은 아니지만...) 메신저 관리페이지 관련하여 Front-end쪽을 수정 및 추가해야할 일이 생겼다. 무식자이기 때문에 약간의 두려움을 가지고 코드를 들여다 보았다. 그래도 이전에 한번 손댄적이 있었기에 낯설지 않았다. 이번에도 데드라인이 코앞이었기 때문에 마음도 급하고, 뭐를 먼저해야할지 좀 정신없이 시작한 것 같다. 어쨌거나 엄청난 삽질을 했었는데, 삽질에 대해서 이야기해보독 하겠다..

### jQuery template
사건은 jQuery template에서 시작한다. 우선 jQuery template에 대해서 알아보도록 하자. 이름에서도 느껴지듯이 template이다. 뭔가 기본틀을 만들어놓고 마구 찍어낼거같은 느낌이 든다. 그렇다. jQuery template이용해서 Javascript 데이터를 HTML로 작성된 템플릿에 바인딩 시켜서 HTML DOM내에 렌더링 할수 있다. 그럼 이쯤에서 예제를 보자.

{% highlight html %}

<div id="access_log_list" class="access_log_list fill">
    <table id="access_log_table" class="table">
        <thead>
            <tr>
                <th>접근시간</th>
                <th>관리자 번호</th>
                <th>사용자 번호</th>
                <th>접근한 IP</th>
            </tr>
        </thead>
        <tbody>

        </tbody>
    </table>
</div>
<script id="access_log_template" type="text/x-jquery-tmpl">
    <tr>
        <td width="">${search_ymdt}</td>
        <td>${member_number}</td>
        <td>${search_keyword}</td>
        <td>${access_ip}</td>
     </tr>
</script>

{% endhighlight %}

실제 사용된 JSP의 내용중 일부이다. 딱 보면 알겠지만 테이블형식으로 데이터를 출력해줄 의도로 작성되었다. 여기서 script태그로 감싸진 부분이 템블릿이다. type이 text/x-jquery-tmpl인걸 보아도 알수 있다

{% highlight javascript %}

var results = data.results;
	$('#access_log_template').tmpl(results).appendTo('#access_log_table tbody');

{% endhighlight %}

위의 템플릿과 상응하는 javascript 코드이다. data의 results를 access_log_template에 넘겨주고, 해당 템플릿은 access_log_table 테이블의 tbody 위치에다가 집어 넣겠다는 의미가 되겠다. 여기까진 좋았다. 잘 돌아갈것이라 생각했지만 예상을 가볍게 짓밟아 버렸고, 데이터가 전혀 출력되지 않았다. 여기서부터 시작이었다. 구글에서 찾은 내용으로는 분명히 저렇게 데이터를 바인딩 시키는 것이 맞았다. 하지만 안된다. 오타인가 찾아봤지만. 오타는 아니었다. 납득할수 없어서 서버만 재시작했었다. 하지만 결과는 역시 똑같았다. 이전에 이미 짜여져있던 코드에서도 템플릿을 이용하고 있었는데. 데이터 바인딩 하는게 좀 달랐다. 위 코드에서는 각 변수를 '${}' 로 묶어서 바인딩 시켜줬었는데.. '{{= }}' 로 바인딩 시키고 있었다. 한참을 이 문제로 씨름을 하다, '{{= }}'를 이용해보기로 했다. 그랬는데.. 거짓말처럼 제대로 값이 출력이 되는것이 아닌가!? 매우 허무한 마음을 가지고 퇴근은 할수 있었다...

### 결론
결론적으로 나는 엄청난 시간을 버렸다. 왜 그런건지 원인을 찾아보았다. 문제는 JSP EL(Expression Language)을 사용하면 jQuery tmplate와 같은 '${}' 를 이용한다는 점이었다! 그렇기 때문에 JSP에서 먼저 파싱이 되서 제대로된 데이터를 출력하지 못했던 것이었다. 별것 아니라고 생각되지만.. 이걸로 고생한걸 생각하면.. :(