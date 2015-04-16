---
layout: post
title:  "ionic: Geolication using ngCordova"
date:   2015-04-14 22:05:40
categories: ionic
tags: ionic ngCordova cordova
---

Install ngCordova and cordova geolocation plugin
---

Step1: run following command at root level of project for install `ngCordova` lib.

{% highlight sh%}
bower install ngCordova
{% endhighlight %}

Step 2: Import the `ngCordova` to your index.html

{% highlight html%}
<script src="lib/ngCordova/dist/ng-cordova.js"></script>
{% endhighlight %}

just above

{% highlight html%}
<script src="cordova.js"></script>
{% endhighlight %}

Step3: run following command at root level of project for install `cordova geolocation` plugin.

{% highlight sh%}
cordova plugin add org.apache.cordova.geolocation
{% endhighlight %}

---

Usage
---

Typically, stored the geo location value into local storge, updated it by `watch` in fixed period.

Step4: Add following code to services.js

{% highlight javascript%}
.factory('$localStorage', ['$window', function ($window) {
        return {
            set: function (key, value) {
                $window.localStorage[key] = value;
            },
            get: function (key, defaultValue) {
                return $window.localStorage[key] || defaultValue;
            },
            setObject: function (key, value) {
                $window.localStorage[key] = JSON.stringify(value);
            },
            getObject: function (key) {
                return JSON.parse($window.localStorage[key] || '{}');
            }
        }
    }])
.factory('geoLocation', function ($localStorage) {
    return {
        clearGeolocation: function() {
            var _position = {};
            $localStorage.setObject('geoLocation', _position)
        },
        setGeolocation: function (latitude, longitude) {
            var _position = {
                latitude: latitude,
                longitude: longitude
            }
            $localStorage.setObject('geoLocation', _position)
        },
        getGeolocation: function () {
            return glocation = {
                lat: $localStorage.getObject('geoLocation').latitude,
                lng: $localStorage.getObject('geoLocation').longitude
            }
        }
    }
})
{% endhighlight %}

Step 5: Add following code to app.js

{% highlight javascript%}
.run(function ($ionicPlatform, $ionicPlatform, $cordovaGeolocation, geoLocation, Logger) {

        $ionicPlatform.ready(function () {
            // Hide the accessory bar by default (remove this to show the accessory bar above the keyboard
            // for form inputs)
            if (window.cordova && window.cordova.plugins.Keyboard) {
                cordova.plugins.Keyboard.hideKeyboardAccessoryBar(true);
            }
            if (window.StatusBar) {
                // org.apache.cordova.statusbar required
                StatusBar.styleDefault();
            }

            $cordovaGeolocation
                .getCurrentPosition()
                .then(function (position) {
                    geoLocation.setGeolocation(position.coords.latitude, position.coords.longitude)
                }, function (err) {
                     Logger.error('Get GEO location failed', err);
                	geoLocation.clearGeolocation();
                });

            // begin a watch
            var options = {
                frequency: 1000,
                timeout: 3000,
                enableHighAccuracy: true
            };

            var watch = $cordovaGeolocation.watchPosition(options);
            watch.then(
            	null,
                function (err) {
                    Logger.error('Refresh GEO location failed', err);
                	geoLocation.clearGeolocation();
                }, function (position) {
                    geoLocation.setGeolocation(position.coords.latitude, position.coords.longitude)
                });

        });

    })
{% endhighlight %}

Step 6: Import `geoLocation` service to controller

{% highlight javascript%}
geoLocation.getGeolocation()
{% endhighlight %}


