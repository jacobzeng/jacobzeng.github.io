---
layout: post
title:  "ionic: No back button when navigating away from tab view"
date:   2015-04-08 13:00:00
categories: ionic
tags: ionic
---

>Version ionic-bower#1.0.0-rc.1

~~~
start view --> random page (out of tab) //have back button
start view --> tab view --> random page (out of tab) //no back button
                     |
                    tab1 --> nested view (in tab) //have back button
                     |
                    tab2 --> nested view (in tab) //no back button
~~~

`random page` is a single page out of tab. when navigating tab view to random page, there is no back button on random page.

While the history stack is broken in this use case, 'backView' in the history object is correct. The full history object can be seen with this log line:

{% highlight javascript %}
console.log( JSON.stringify($ionicHistory.viewHistory(), null, 4) );
{% endhighlight %}

---

Solution 1:
---

Add back button to random page manually

{% highlight html %}
<ion-nav-buttons side="left">
    <button ng-show='enableBack' class="button button-clear" ng-click="goBack()"><i class="icon ion-arrow-left-c"></i> {{lastViewTitle}}</button>
</ion-nav-buttons>
{% endhighlight %}

Add `goBack`, `enableBack` and `lastViewTitle` logic to `CTRL`

{% highlight javascript %}
$scope.goBack = function() {
    $ionicHistory.goBack();
};
$scope.lastViewTitle = '返回';
$scope.enableBack = false;
if ($ionicHistory.backView()) {
    $scope.lastViewTitle = $ionicHistory.backTitle();
    $scope.enableBack = ($ionicHistory.backView().stateId == 'tab view state id'); //avoid display double back button when navgiate start view to random page
}
{% endhighlight %}

Solution 2:
---

Modify the icon source, replace enableBack() in ionic.bundle.js with this:

{% highlight javascript %}
enabledBack: function(view) {
  //original code
  //var backView = getBackView(view);
  //return !!(backView && backView.historyId === view.historyId);

  //new code to show back
  var backView = viewHistory.backView;
  return backView != null;
},
{% endhighlight %}

*Notice that*, the solution will break your existed nav state, please check the original nav action after replace.