README-language identification & multi-lingual apps.txt

Date: 2019-04-16 (Tue)

# App Installation & Verification
## How to install
$ adb connect 192.168.0.50
$ adb devices
$ adb install LID_debug-01.00.01.apk
[100%] /data/local/tmp/LID_debug-01.00.01.apk
	pkg: /data/local/tmp/LID_debug-01.00.01.apk
Success
$ adb install multilingual.02.00.00.apk 
[100%] /data/local/tmp/multilingual.02.00.00.apk
	pkg: /data/local/tmp/multilingual.02.00.00.apk
Success
$


Both apps are necessary to communicate with the LID server.

## How to launch the multilingual app
$ adb shell am start -n com.kt.gigagenie.multilingual/.MainActivity

Press the mic button on the bottom left to speak.
If it works properly, the LID server log shows 200.

183.98.154.45 - - [16/Apr/2019 04:55:21] "POST / HTTP/1.1" 200 -
2019-04-16	04:55:36	test@test.com	04_55_35.wav	 : {"cn":"0.000001","en":"0.000004","jp":"0.000000","kr":"0.999995"}

In addition, the app displays the responded values in '{"cn":"0.000001","en":"0.000004","jp":"0.000000","kr":"0.999995"}'

# Troubleshooting

## Check to see how this works
$ adb logcat | grep -i "LID"

The log may show the app is connecting to a wrong IP address.

## How to list all the installed packages
adb shell 'pm list packages -f"

## How to check a newly installed app on adb

List all the installed package and save them to old_install.txt
$ adb shell 'pm list packages -f'
$ nano old_install.txt

After installing the app,
$ adb install multilingual.02.00.00.apk 
[100%] /data/local/tmp/multilingual.02.00.00.apk
	pkg: /data/local/tmp/multilingual.02.00.00.apk
Success
$

List all the installed package and save them to new_install.txt
$ adb shell 'pm list packages -f'
$ nano new_install.txt

Get the difference of old_install.txt and new_install.txt.
$ diff old_install.txt new_install.txt 
44a45
> package:/data/app/com.kt.gigagenie.multilingual-1/base.apk=com.kt.gigagenie.multilingual

## Problem: App installation fails with the error message
Failure [INSTALL_FAILED_VERSION_DOWNGRADE]

$ adb install -r LID_debug-01.00.01.apk 
[100%] /data/local/tmp/LID_debug-01.00.01.apk
	pkg: /data/local/tmp/LID_debug-01.00.01.apk
Failure [INSTALL_FAILED_VERSION_DOWNGRADE]
$

### Cause
Ref: Failure [INSTALL_FAILED_VERSION_DOWNGRADE], can not deploy Android app, https://forums.xamarin.com/discussion/125000/failure-install-failed-version-downgrade-can-not-deploy-android-app

The previous version was not properly uninstalled.
The prev version can be uninstalled in the settings or with $ adb uninstall (if the name is known).
Or the cache needs to be cleared properly from the prev version.

I've tried both, but didn't work. I checked with the developer.
Well, the name of the prev version of the app is supposed to be "Language Identification".
But it's actually "China Voice Recognition", the old name of the app.

### Solution
I removed "China Voice Recognition" app and the problem was solved when I reinstalled the app.
