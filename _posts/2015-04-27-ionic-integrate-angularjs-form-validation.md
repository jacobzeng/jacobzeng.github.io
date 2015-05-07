---
layout: post
title:  "Ionic: integrate angularjs form validation"
date:   2015-04-27 17:00:00
categories: ionic
tags: ionic angularjs mobile
---

Screenshot
---
![ionic-integrate-angularjs-form-validation]({{site.url}}/assets/ionic-integrate-angularjs-form-validation.png)

---

Steps
---

**Step 1:** install angulajrs-messages first, import it in index.html
{% highlight html %}
<script src="lib/angular-messages/angular-messages.js"></script>
{% endhighlight %} 

**Step 2:** make your angularjs module depends `ngMessages`

{% highlight javascript %}
angular.module('demoapp', ['ionic', 'ngMessages')
{% endhighlight %}

**Step 3:** define css

{% highlight css %}
.error-container {
    /*
    margin: 5px 0;
    */
}

.error-container:last-child {
    /*
    margin: 5px 0 0;
    */
}

.error {
    padding: 10px 16px;
    font-family: "Arial Black", Gadget, sans-serif;
    font-size: 11px;
    text-transform: uppercase;
    color: #555;
    vertical-align: middle;
}

.error i {
    font-size: 24px;
    color: #B83E2C;
    vertical-align: middle;
}

.last-error-container > .error {
    padding: 10px 16px 0;
}

.has-errors {
    border-bottom: 1px solid #B83E2C;
}

.no-errors {
    /*
    border-bottom: 3px solid green;
    */
}
{% endhighlight %}


**Step 4:** Define error message
{% highlight html %}
<script id="error-list.html" type="text/ng-template">
    <div class="error" ng-message="required">
        <i class="ion-information-circled"></i> 不能为空
    </div>
    <div class="error" ng-message="minlength">
        <i class="ion-information-circled"></i> 最短为5个字符!
    </div>
    <div class="error" ng-message="maxlength">
        <i class="ion-information-circled"></i> 最长为20个字符!
    </div>
</script>
{% endhighlight %}

**Step 5:** using in form
{% highlight html %}
<ion-view view-title="登陆">
    <ion-content scroll='true'>
        <div class='logo-container'>
            <img src='img/logo.png'>
        </div>
        <form name="loginForm" novalidate="" ng-submit="">
            <div class="list" style='margin-top:20px;'>
                <label class="item item-input" ng-class="{ 'has-errors' : loginForm.username.$invalid, 'no-errors' : loginForm.username.$valid}" ng-messages-include="error-list.html">
                    <span class="input-label">用户名:</span>
                    <input type="text" name='username' ng-model="user.username" required>
                </label>
                <div class="error-container" ng-show="loginForm.username.$error" ng-messages="loginForm.username.$error" ng-messages-include="error-list.html">
                </div>
                <label class="item item-input">
                    <span class="input-label">密码:</span>
                    <input type="password" ng-model="user.password">
                </label>
                <ion-toggle ng-model="data.isRemerberUsername" toggle-class="toggle-calm">记住用户名</ion-toggle>
            </div>
        </form>
        <div class="padding">
            <button class="button button-block button-assertive" ng-disabled='loginForm.$invalid' ng-click="login();">
                登 录
            </button>
            <p class='text-center assertive' ng-show='hasErrorMsg'>
                {{errorMsg}}
            </p>
            <p class='text-center'>
                <a href="#/register" class='dark'>没有账户?现在就注册,有惊喜</a>
            </p>
            <p style='margin-top:20px;' class='text-right'>
                <a href="#/forgetPassword" class='dark'>忘记密码</a>
            </p>
        </div>
    </ion-content>
</ion-view>
{% endhighlight %}
