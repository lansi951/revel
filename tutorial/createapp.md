---
title: 새로운 Revel 앱 만들기
layout: tutorial
---

[`revel`]({{ layout.root }}/manual/tool.html#mew) 명령줄 도구를 사용해 GOPATH에 앱을 만들고 실행하세요:
{% highlight sh %}
$ export GOPATH="/home/me/gostuff"
$ cd $GOPATH
$ revel new myapp
~
~ revel! http://revel.github.io
~
Your application is ready:
    /home/me/gostuff/src/myapp

You can run it with:
    revel run myapp

$ revel run myapp
~
~ revel! http://revel.github.io
~
2012/09/27 17:01:54 run.go:41: Running myapp (myapp) in dev mode
2012/09/27 17:01:54 harness.go:112: Listening on :9000
{% endhighlight %}
다른 예제
{% highlight sh %}
$ revel new github.com/myaccount/myapp
$ revel run github.com/myaccount/myapp
{% endhighlight %}
[http://localhost:9000/](http://localhost:9000/)를 브라우저에서 열면 앱이 준비되었다는 알림을 볼 수 있습니다.

![Your Application Is Ready]({{ layout.root }}/img/YourApplicationIsReady.png)

- 생성된 프로젝트 구조는 [organization]({{ layout.root }}/manual/organization.html)에 설명되어 있습니다.
- HTTP 포트 설정은 [`conf/app.conf`]({{ layout.root }}/manual/appconf.html#httpport)에서 볼 수 있습니다.

<a href="requestflow.html" class="btn btn-sm btn-success" role="button">Next <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span></a> [Revel이 요청을 처리하는 방법.](requestflow.html)
