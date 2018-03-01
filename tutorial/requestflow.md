---
title: 요청 흐름
layout: tutorial
---

[이전 페이지](createapp.html)에서 **myapp**이라고 하는 새 Revel 앱을 만들었습니다.
이 페이지에서는 Revel이 어떻게 `http://localhost:9000/`의 HTTP 요청을 처리해서
환영 메세지를 보여주는지 살펴봅니다.

## 라우트

Revel이 첫번째로 하는 일은 `conf/routes` 파일을 확인하는 것입니다 ([라우팅]({{ layout.root }}/manual/routing.html) 참조):

	GET     /     App.Index

이 구문은 Revel에서 **`/`**로 http **`GET`** 요청을 받았을 때,
**`App`** [컨트롤러]({{ layout.root }}/manual/controllers.html)의 **`Index`** 메소드를 호출하도록 합니다.

## 컨트롤러 메소드

**app/controllers/app.go** 코드를 봅시다:
```go
package controllers

import "github.com/revel/revel"

type App struct {
    *revel.Controller
}

func (c App) Index() revel.Result {
    return c.Render()
}
```

모든 [컨트롤러]({{ layout.root }}/manual/controllers.html)는 제일 먼저 [`*revel.Controller`](https://godoc.org/github.com/revel/revel#Controller)를 포함하는 `구조체`여야 합니다.
[`revel.Result`]({{ layout.root }}/manual/results.html)를 반환하는 컨트롤러의 모든 메소드는 **Action**의 일부로 사용할 수 있습니다. 위의 예제에서 **App.Index**는 **Action**입니다.

Revel 컨트롤러는 [결과]({{ layout.root }}/manual/results.html)를 만드는데 유용한 여러 가지 방법을 제공합니다.
이 예제에서는 [`Render()`](https://godoc.org/github.com/revel/revel#Controller.Render)를 호출해 Revel에게 [템플릿]({{ layout.root }}/manual/templates.html)을 찾아 렌더링해 http `200 OK`로 응답하도록 합니다.

## 템플릿

[템플릿]({{ layout.root }}/manual/templates.html)은 **app/views** 디렉토리에 있습니다.
명시적으로 템플릿 이름을 지정하지 않으면, Revel은 controller/method와 일치하는 템플릿을 찾습니다.
이 경우에는 Revel은 **app/views/App/Index.html**을 찾아서 [Go 템플릿](http://www.golang.org/pkg/html/template)으로 렌더링합니다.

{% capture ex %}{% raw %}
{{set . "title" "Home"}}
{{template "header.html" .}}

<header class="jumbotron" style="background-color:#A9F16C">
  <div class="container">
    <div class="row">
      <h1>It works!</h1>
      <p></p>
    </div>
  </div>
</header>

<div class="container">
    <div class="row">
    <div class="span6">
        {{template "flash.html" .}}
    </div>
    </div>
</div>

{{template "footer.html" .}}
{% endraw %}{% endcapture %}
{% highlight htmldjango %}{{ex}}{% endhighlight %}

Go 템플릿 패키지가 제공하는 기능 외에도 Revel은 [몇가지 유용한 기능]({{ layout.root }}/manual/templates.html#functions)을 추가시켜줍니다.

위의 템플릿 설명:

1. [set]({{ layout.root }}/manual/templates.html#set)로 새 **title** 변수를 렌더링 컨텍스트에 추가합니다.
2. **title** 변수를 사용하는 **header.html** 템플릿을 포함합니다.
3. 환영 메세지를 표시합니다.
4. [플래시]({{ layout.root }}/manual/sessionflash.html#flash) 된 메세지를 보여주는 **flash.html**을 포함합니다.
5. **footer.html**을 포함합니다

**header.html**를 보면 좀 더 많은 템플릿 태그를 볼 수 있습니다:

{% capture ex %}{% raw %}
<!DOCTYPE html>

<html>
  <head>
    <title>{{.title}}</title>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" type="text/css" href="/public/css/bootstrap-3.3.6.min.css">
    <link rel="shortcut icon" type="image/png" href="/public/img/favicon.png">
    <script src="/public/js/jquery-2.2.4.min.js"></script>
    <script src="/public/js/bootstrap-3.3.6.min.js"></script>
    {{range .moreStyles}}
      <link rel="stylesheet" type="text/css" href="/public/{{.}}">
    {{end}}
    {{range .moreScripts}}
      <script src="/public/{{.}}" type="text/javascript" charset="utf-8"></script>
    {{end}}
  </head>
  <body>

{% endraw %}{% endcapture %}
{% highlight htmldjango %}{{ex}}{% endhighlight %}

[set]({{ layout.root }}/manual/templates.html#set)으로 설정된 `.title` 변수가 사용되는 걸 볼 수 있으며,
**moreStyles**와 **moreScripts** 변수에서 템플릿을 호출할 때 포함된 JS 및 CSS 파일을 사용할 수 있습니다.

## Hot-reload

Revel은 파일이 변경됐는지 확인해서 다시 컴파일하는 [`워쳐`]({{ layout.root }}/manual/appconf.html#watchers)를 개발주기의 일부로 가지고 있습니다.

그것을 확인하기 위해 **Index.html**의 환영 메세지를 변경하세요. 

{% highlight html %}
<h1>It works!</h1>
{% endhighlight %}
to
{% highlight html %}
<h1>Hello Revel</h1>
{% endhighlight %}

브라우저를 새로고침하면 즉시 변경된 것을 볼 수 있습니다.
템플릿이 변경되어 Revel이 다시 로드된 것을 확인했습니다.

Revel watches  - [설정]({{ layout.root }}/manual/appconf.html#watchers)) 참고:

* 모든 **app/** 아래의 Go 코드
* 모든 **app/views/** 아래의 템플릿
* 라우트 파일: **conf/routes**

이 중 하나를 변경하면 Revel이 변경된 최신 코드로 실행중인 앱을 업데이트하고 컴파일합니다.
지금 시도해보세요: **app/controllers/app.go** 열어 에러를 표시하게 합니다.

수정
{% highlight go %}
return c.Render()
{% endhighlight %}
to
{% highlight go %}
return c.Renderx()
{% endhighlight %}
페이지를 새로고치면 Revel에 유용한 에러 메세지가 표시됩니다:

![A helpful error message]({{ layout.root }}/img/helpfulerror.png)

마지막으로 템플릿으로 데이터를 보내봅니다.

**app/controllers/app.go**을:
{% highlight go %}
return c.Renderx()
{% endhighlight %}
다음으로 수정:
{% highlight go %}
greeting := "Aloha World"
return c.Render(greeting)
{% endhighlight %}
그리고 **app/views/App/Index.html** 템플릿을:

{% capture ex %}{% raw %}
<h1>Hello Revel</h1>
{% endraw %}{% endcapture %}
{% highlight htmldjango %}{{ex}}{% endhighlight %}

다음으로 수정:

{% capture ex %}{% raw %}
<h1>{{.greeting}}</h1>
{% endraw %}{% endcapture %}
{% highlight htmldjango %}{{ex}}{% endhighlight %}

브라우저를 새로고침해 하와이 인사말을 볼 수 있습니다.

![A Hawaiian greeting]({{ layout.root }}/img/AlohaWorld.png)

<a href="firstapp.html" class="btn btn-sm btn-success" role="button">다음 <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span></a> ['Hello World' 앱 만들기](firstapp.html)
