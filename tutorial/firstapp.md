---
title: "'Hello World' 앱"
layout: tutorial
---

"Hello World" 앱을 빠르게 구현하기 위한 페이지입니다.

[이전에 생성한](createapp.html) **myapp** 프로젝트 시작해보겠습니다.

**app/views/App/Index.html** 템플릿을 수정하여 이 폼을
포함된 `flash.html` 템플릿 아래에 추가하세요:
{% highlight html %}
<form action="/App/Hello" method="GET">
    <input type="text" name="myName" /><br/>
    <input type="submit" value="Say hello!" />
</form>
{% endhighlight %}
현재 작업을 보기 위해 페이지를 새로고침하세요.

![The Say Hello form](/img/AlohaForm.png)

아무 데이터를 입력하세요.

![Route not found](/img/HelloRouteNotFound.png)

에러가 날 것입니다. 이제 메소드를 **app/controllers/app.go**에 추가하세요:

{% highlight go %}
func (c App) Hello(myName string) revel.Result {
    return c.Render(myName)
}
{% endhighlight %}

다음으로 뷰를 만들어야 합니다. 다음 내용으로 **app/views/App/Hello.html** 파일을 만드세요:

{% capture ex %}{% raw %}
{{set . "title" "Hello page"}}
{{template "header.html" .}}

<h1>Hello {{.myName}}</h1>
<a href="/">폼으로 돌아가기</a>

{{template "footer.html" .}}
{% endraw %}{% endcapture %}
{% highlight htmldjango %}{{ex}}{% endhighlight %}

마지막으로 **conf/routes** 파일 `App.Index` 항목 아래에 다음 내용을 추가합니다.

{% capture ex %}{% raw %}
GET     /App/Hello     App.Hello
{% endraw %}{% endcapture %}
{% highlight htmldjango %}{{ex}}{% endhighlight %}

페이지를 새로고침하면 인사말이 표시됩니다:

![Hello revel](/img/HelloRevel.png)

마지막으로 몇 가지 유효성 검사를 추가해보겠습니다.
myName은 필수이고 최소 3글자 이상이어야합니다.

이렇게 하려면 [유효성 검사 모듈](/manual/validation.html)을 사용합시다. **app/controllers/app.go**에서 메소드를 다음과 같이 수정합니다.
{% highlight go %}
func (c App) Hello(myName string) revel.Result {
    c.Validation.Required(myName).Message("이름은 필수입니다!")
    c.Validation.MinSize(myName, 3).Message("이름이 너무 짧습니다!")

    if c.Validation.HasErrors() {
        c.Validation.Keep()
        c.FlashParams()
        return c.Redirect(App.Index)
    }

    return c.Render(myName)
}
{% endhighlight %}

이제 유효한 이름을 입력하지 않으면 사용자를 `Index()`로 보냅니다.
이름과 유효성 검사 에러가 임시 쿠키인 [플래시](/manual/sessionflash.html#flash)에 저장됩니다.

제공된 `flash.html` 템플릿에 오류 또는 플래시 메세지가 표시됩니다:

{% capture ex %}{% raw %}
{{if .flash.success}}
<div class="alert alert-success">
    {{.flash.success}}
</div>
{{end}}

{{if or .errors .flash.error}}
<div class="alert alert-error">
    {{if .flash.error}}
        {{.flash.error}}
    {{end}}
    {{if .errors}}
    <ul style="margin-top:10px;">
        {{range .errors}}
            <li>{{.}}</li>
        {{end}}
    </ul>
    {{end}}
</div>
{{end}}
{% endraw %}{% endcapture %}
{% highlight htmldjango %}{{ex}}{% endhighlight %}

유효성 검사에 실패한 이름을 함께 폼에 입력하면 잘못된 이름을 유지하여 사용자가 다시 입력하기 전에 수정할 수 있게해줍니다.
**app/views/App/Index.html** 템플릿의 폼을 다음과 같이 수정하세요:

{% capture ex %}{% raw %}
<form action="/App/Hello" method="GET">
    {{with $field := field "myName" .}}
        <input type="text" name="{{$field.Name}}" value="{{$field.Flash}}"/><br/>
    {{end}}
    <input type="submit" value="Say hello!" />
</form>
{% endraw %}{% endcapture %}
{% highlight htmldjango %}{{ex}}{% endhighlight %}

이제 이름에 한 글자만 입력하면:

![Example error](/img/HelloNameNotLongEnough.png)

성공입니다. 오류가 있을 경우 입력했던 데이터가 저장되어 다시 수정할 수 있습니다.

<hr>

- [메뉴얼](/manual/concepts.html)을 더 읽어보세요
- [예제](/examples/) 앱들을 보세요
