---
layout: post
title:  "Ionic: emulate different iPhone devices"
date:   2015-04-06 17:00:00
categories: ionic
tags: ionic
---

>Sometimes, we want to emulate different iPhone devices to check the CSS, like iPhone5s, iPhone6 or other kinds

In order for this to work, you need to have Xcode 6 installed along with the command line tools.

Notice that, make sure run `ionic build ios` before the script once you change files

~~~
#!/bin/bash
export DEVICES=`ios-sim showdevicetypes 2>&1`
export DEVICES=\"`echo $DEVICES | sed -e 's/ com./" "com./g' | sed -e 's/, /,~/g'`\"
PS3='Please enter your choice: '
options=($DEVICES)
app="`find ./platforms/ios/build/emulator/ -name *.app -print`"
escaped_app="\"$app\""

echo $app
select opt in "${options[@]}"
do
    case $opt in
        *) echo ios-sim launch "$escaped_app" --devicetypeid "`echo $opt | sed -e "s/~/ /g"`" --stderr ./platforms/ios/cordova/console.log --stdout ./platforms/ios/cordova/console.log > tmp.run.file;chmod +x tmp.run.file;./tmp.run.file;rm tmp.run.file;exit;
    esac
done
~~~


To use this, create a new file called emulateios.sh and paste in the code. Type chmod +x emulateios, then to run it just type ./emulateios.sh from inside a valid project (root level). It'll present a menu of choices, then run your currently (already built) iOS project in the selected emulator.
