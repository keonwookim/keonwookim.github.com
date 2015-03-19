---
layout: post
title: "2015/03/19 장애 전파 프로그램이 장애가 있어요! "
---

## 들어가기 전에
지난주 금요일 (2015/03/15) "장애내용을 JAM과 메일로 전파하는 프로그램 구현해주세요." 라는 미션을 받았다. Netty 프레임워크를 이용해서... 급작스러운 미션에, 거진 처음인 Netty 프레임워크라니! 거기다가 기간도 짧았다. 어찌저찌 하루만에 동작하는 버전을 만들었다. 내심 뿌듯했었다. 그러나 오늘(2015/03/19) 장애 전파가 되지 않는다는 제보를 받았다. 매우 당황하며 원인을 찾고, 헛다리도 짚고 하다가 amigo (김경희 책임님)의 도움으로(**감사합니다.**) 내 결과물이 원인이라는 것을 알게되었다. 내가 만든게 말썽을 일으켰으니 뭐가 문제인가 분석을 했고, 좀전에 완전히 알아냈다. 그래서 잊기전에 기록하려고 한다.

## 대략적인 구조
우선, JAM과 메일로 *장애 전파를 하는 서버*가 있다. 그리고 메신저 서버의 로그를 모니터링하다 특정 계정(장애 관련 계정)에게 메시지가 전송되면 그것을 캐치해서 장애 전파 서버로 누구에게 장애 전파 메시지를 전송해야 하는지를 알려주는 **로그 모니터링 파이썬 스크립트**가 있다. 슬프지만 내가 만든 서버가 문제가 있었으므로 구현한 서버에 무엇이 문제였는지를 알아보도록 하겠다.

## 현상
어찌된 이유인지(지금은 알고있지만..) 리퀘스트를 보내도 리퀘스트 내용이 아닌 어제 보냈던 그림과같이 메시지 및 메일이 도착했다.

![JAM msg]({{ D:\PortableJekyll\keonwookim.github.com\assets\jam.jpg }}/assets/jam.jpg)

메일 역시 같은 내용으로 전송이 되고있었다.

## 원인

**가장 큰 원인은 내 미숙함이었다.**

자세하게 얘기하자면, 리퀘스트가 들어올때마다 **HTTP content를 저장하는 StringBuilder 객체가 초기화 되지 않은것이 이유**였다. 초기화 안했던것은 아니었다. 다만 호출이 되지 않았을뿐... 소스코드를 보면서 좀더 자세하게 들여다보자.
{% highlight java %}
@Override protected void channelRead0(ChannelHandlerContext ctx, Object msg) {

        if (msg instanceof HttpRequest) {
            HttpRequest request = this.request = (HttpRequest) msg;
            if (HttpHeaders.is100ContinueExpected(request)) {
                send100Continue(ctx);
            }
            buf.setLength(0);					//문제의 부분
        }

        if (msg instanceof HttpContent) {
            HttpContent httpContent = (HttpContent) msg;
            ByteBuf content = httpContent.content();


            if (content.isReadable()) {
                buf.append(content.toString(CharsetUtil.UTF_8));


            }
            if (msg instanceof LastHttpContent) {
                GLoggerFactory.getInstance().setLoggerClass(TtsNotificationLogger.class);
                GMCoreServer core = new GMCoreServer(host, port, (short) 1, isSecure, new ConnectionLostListener() {
                    @Override
                    public void onConnectionLost() {
                        System.out.println("connection lost");
                    }
                });

                String jsonRequest = buf.toString();
                NotiContent notiContent = getRequestContents(jsonRequest);
                NotificationController noti = new NotificationController();
                noti.sendNotification(core, notiContent);
                //buf.setLength(0);						//바꾼 위치
            }
        }
    }

{% highlight %}

파이썬 스크립트로부터 HTTP 리퀘스트를 받으면 해당 메소드로 진입하게 된다. 문제의 부분을 보자. 분명히 멤버 변수인 buf (StringBuilder)를 초기화 해주고 있다. 그러나 실제 동작은 예전데이터가 망령처럼 나타나서 장애 메시지 및 메일로 전송되고 있었다! Netty 프레임워크의 example code 에서는 분명 저 위치였는데 동작했었다!! buf.setLength(0) 의 위치를 바꿔보라는 아미고의 말에 수긍하기 어려웠지만 옮겼다. 그랬더니 되는것이 아닌가! 눈으로는 목격했지만 머리로는 이해가 되지 않았다. 그래서 좀더 살펴보기로 했다.

좀더 깊숙히 들어가서 상황 설명을 하자면, 나는 URL 라우팅을 하고싶었다. 해당 서버 IP로 아무렇게나 리퀘스트를 날려도 수행되게 할수는 없으니까... 방법을 잘 몰랐기에 구글에 문의했다. 고맙게도 누군가 만들어 놓은 라이브러리가 있었다. 이름은 [netty-router](http://github.com/sinetja/netty-router). 내가 직접 짜는거보다 훨씬 깔끔하게 이용할 수 있을것 같았다. 그래서 이용했는데 **이게 해당 버그의 원인이었다!**

{% highlight java %}
public class TtsNotificationInitializer extends ChannelInitializer<SocketChannel> {

    private static final Router router = new Router()
            .POST("/sendnoti", new TtsNotificationServerHandler());			//after

    private static final Handler handler = new Handler(router);				//after
    private final SslContext sslCtx;
    public TtsNotificationInitializer(SslContext sslCtx) {
        this.sslCtx = sslCtx;
    }
    @Override
    public void initChannel(SocketChannel ch) {
        ChannelPipeline p = ch.pipeline();
        if (sslCtx != null) {
            p.addLast(sslCtx.newHandler(ch.alloc()));
        }
        p.addLast(new HttpRequestDecoder());
        // Uncomment the following line if you don't want to handle HttpChunks.
        //p.addLast(new HttpObjectAggregator(1048576));
        p.addLast(new HttpResponseEncoder());
        // Remove the following line if you don't want automatic content compression.
        //p.addLast(new HttpContentCompressor());
        p.addLast(handler.name(), handler);							//after
        //p.addLast(new TtsNotificationServerHandler()); 			//before
    }
}
{% highlight %}

서버를 초기화 해주는 클래스이다. 이전 소스코드는 before, netty-router를 이용하면서 고친 소스코드는 after로 표기했다. Router와 Handler를 위와 같이 생성해서 ChannelPipeline 에 추가해주면 간단하게 완성된다. 이렇게 함으로써 서버는 **sendnoti로 POST요청 들어올 때만 서비스**를 해주게 된다. 여기까진 좋았다. 위에 보았던 소스코드 일부분을 다시보자.

{% highlight java %}
@Override protected void channelRead0(ChannelHandlerContext ctx, Object msg) {

        if (msg instanceof HttpRequest) {
            HttpRequest request = this.request = (HttpRequest) msg;
            if (HttpHeaders.is100ContinueExpected(request)) {
                send100Continue(ctx);
            }
            buf.setLength(0);					//문제의 부분
        }
{% highlight %}

다시 문제의 부분이다. buf.setLength(0) 가 수행되지 않은 이유를 설명하겠다. netty-router를 이용하기 전에는, 요청이 들어오면 **msg가 HttpRequest의 구현체(DefaultHttpRequest)** 로 넘어온다. 때문에 **(msg instanceof HttpRequest)** 가 참이 되고, if 블록 안의 코드가 수행된다. 하지만, netty-router를 이용하면 **msg가 Routed 라는 객체 타입**으로 넘어오게된다. 때문에 **(msg instanceof HttpRequest)** 가 거짓이 된다. 결과적으로 if 블록은 수행되지 않게되고, 이로인해 잘못된 동작을 하고 있었다.

## 맺으며
최대한 편하게 작성하려했지만 그때는 좀 기분이 그랬다. 장애 알림 서버가 장애라니! 이번에 깨달은건, "역시 제대로 알고 써야겠구나." 와 "테스트 할 때 제대로 해야되는구나." 두가지이다. 테스트 할때 잘 잡아내기만 했어도 이렇지는 않았을 것이다. 


