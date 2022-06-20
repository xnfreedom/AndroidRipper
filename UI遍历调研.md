# 20220620UI遍历调研
代码库：
 reverse-unina / AndroidRipper 
https://github.com/reverse-unina/AndroidRipper
git@github.com:xnfreedom/AndroidRipper.git


获取当前打开应用的首个activity:
adb shell "dumpsys window | grep mCurrentFocus"
adb shell monkey -p com.android.settings -vvv 1
=> adb shell am start com.android.settings/.Settings$StorageUseActivity


adb shell am instrument -e package  it.unina.android.ripper -w it.unina.android.ripper/android.test.InstrumentationTestRunner
adb shell am instrument -e package  it.unina.android.ripper -w it.unina.android.ripper.test/android.support.test.runner.AndroidJUnitRunner



adb shell am instrument -e package  it.unina.android.ripper -w it.unina.android.ripper/android.test.InstrumentationTestRunner
it.unina.android.ripper.RipperTestCase:INSTRUMENTATION_RESULT: shortMsg=Process crashed.
INSTRUMENTATION_CODE: 0

kdk.android.simplydo


adb shell pm list instrumentation


/media/xiangyanli/US100 512G/Code/github/AndroidRipper/AndroidRipper$  ./gradlew clean && ./gradlew assDeb && adb install ./app/build/outputs/apk/debug/app-debug.apk
/media/xiangyanli/US100 512G/Code/github/RipperApi26$ ./gradlew clean && ./gradlew assDeb && adb install ./app/build/outputs/apk/debug/app-debug.apk



/media/xiangyanli/US100 512G/Code/github/RipperNew$ ./gradlew clean && ./gradlew assDeb && adb install ./app/build/outputs/apk/debug/app-debug.apk
=>cg6 指定minsdk=19
能正常打开运行,但是没有UI遍历操作的效果，instrumentation界面也是hang住，没有日志打印
pro6 crash

cg6:
06-20 11:50:30.365 13461 13511 I TestRunner: started: testApplication(it.unina.android.ripper.RipperTestCase)
06-20 11:50:30.375  1371  6231 W ActivityManager: Unable to start service Intent { act=.IAndroidRipperService cmp=it.unina.android.ripper_service/.AndroidRipperService } U=0: not found
06-20 11:50:30.377  1371  6231 W ActivityManager: Unable to start service Intent { act=.IAnrdoidRipperServiceCallback cmp=it.unina.android.ripper_service/.AndroidRipperService } U=0: not found
06-20 11:50:32.649 13461 13511 I AndroidRipper: [RipperTestCase] Ready to operate after restarting...




Resources$NotFoundException compat_button_padding_horizontal_material is not a Drawable

官网上去下载 https://github.com/reverse-unina/AndroidRipper  Android Ripper 2017.10 Latest， 
java -jar AndroidRipper.jar app-debug.apk default.properties 去运行

## 定制ripperDriver
/media/xiangyanli/US100 512G/Code/github/AndroidRipper/AndroidRipperDriver$ ./gradlew makeReleaseJar
AndroidRipperDriver$  cp ./app/build/intermediates/bundles/release/classes.jar  /home/xiangyanli/下载/AndroidRipper-2017.10/
unzip -q classes.jar -d classes
unzip -q AndroidRipper.jar -d AndroidRipper
定制AndroidRipperStarter：
cp ./classes/it/unina/android/ripper/driver/AndroidRipperStarter.class ./AndroidRipper/it/unina/android/ripper/driver/AndroidRipperStarter.class
cp ./classes/it/unina/android/ripper/planner/HandlerBasedPlanner.class AndroidRipper/it/unina/android/ripper/planner/HandlerBasedPlanner.class 
cp -r  ./classes/it/unina/android/ripper/tools/strace AndroidRipper/it/unina/android/ripper/tools/
cp -r  ./classes/it/unina/android/ripper/tools/tcpdump AndroidRipper/it/unina/android/ripper/tools/


 
 cd AndroidRipper && zip -qr AndroidRipper_0620.jar * && mv AndroidRipper_0620.jar ../


 adb uninstall it.unina.android.ripper
 adb uninstall kdk.android.simplydo


/home/xiangyanli/下载/AndroidRipper-2017.10$ java -jar AndroidRipper_0620.jar app-debug.apk default.properties


拉起ripper_service:


1. adb shell am start it.unina.android.ripper_service/.MainActivity
2. adb shell am startservice  -a it.unina.android.ripper_service.ANDROID_RIPPER_SERVICE

为什么这个拉不起来？？？ TT可以拉起。android7上版本太低？？
adb shell "am startservice --user 0 -a 'it.unina.android.ripper_service.ANDROID_RIPPER_SERVICE'  -n 'it.unina.android.ripper_service/.MainActivity'"


ripper.apk installed!
adb chmod 777...
[UNCAUGHT-EXCEPTION] null
java.lang.reflect.InvocationTargetException
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base/java.lang.reflect.Method.invoke(Method.java:566)
	at org.eclipse.jdt.internal.jarinjarloader.JarRsrcLoader.main(JarRsrcLoader.java:58)
Caused by: java.lang.NoClassDefFoundError: org/apache/commons/lang3/ArrayUtils
	at it.unina.android.ripper.tools.actions.Actions.adbSuShell(Actions.java:1003)
	at it.unina.android.ripper.driver.AbstractDriver.startup(AbstractDriver.java:856)
	at it.unina.android.ripper.driver.random.RandomDriver.rippingLoop(RandomDriver.java:173)
	at it.unina.android.ripper.driver.AbstractDriver.startRipping(AbstractDriver.java:321)
	at it.unina.android.ripper.driver.AndroidRipperStarter.startRipping(AndroidRipperStarter.java:559)
	at it.unina.android.ripper.boundary.AndroidRipper.startRipping(AndroidRipper.java:92)
	at it.unina.android.ripper.boundary.AndroidRipper.main(AndroidRipper.java:80)
	... 5 more
Caused by: java.lang.ClassNotFoundException: org.apache.commons.lang3.ArrayUtils
	at java.base/java.net.URLClassLoader.findClass(URLClassLoader.java:471)
	at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:588)
	at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:521)
	... 12 more
java.lang.RuntimeException: Tool return 143
	at it.unina.android.ripper.tools.lib.WrapProcess.waitForSuccess(WrapProcess.java:86)
	at it.unina.android.ripper.tools.logcat.LogcatDumper.run(LogcatDumper.java:74)




jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore /home/xiangyanli/下载/AndroidRipper-2017.10/tools//debug.keystore -storepass android -keypass android /home/xiangyanli/下载/AndroidRipper-2017.10/app-debug.apk_2022-06-20_17-58-54/temp/ar.apk androiddebugkey

jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore /home/xiangyanli/下载/AndroidRipper-2017.10/tools//debug.keystore -storepass android -keypass android /home/xiangyanli/下载/AndroidRipper-2017.10/app-debug_signed.apk androiddebugkey



ar.apk  it.unina.android.ripper
aut.apk kdk.android.simplydo
ripper.apk it.unina.android.ripper
temp  kdk.android.simplydo



[1655720000833] Start ripper...

it.unina.android.ripper.RipperTestCase:[1655720022918] Connect...
[1655720022918] Connecting to Android Ripper Service...
[1655720022941] Connecting to Android Ripper Service...
[1655720022947] Connecting to Android Ripper Service...
[1655720022975] Connecting to Android Ripper Service...
[1655720022987] Connecting to Android Ripper Service...
[1655720022991] Connecting to Android Ripper Service...
[1655720022997] Connecting to Android Ripper Service...
[1655720023001] Connecting to Android Ripper Service...
[1655720023007] Connecting to Android Ripper Service...
[1655720023039] Connecting to Android Ripper Service...
[1655720023101] Connecting to Android Ripper Service...
[EXCEPTION][AbstractDriver.startup] Android Ripper Service Connection Error
java.lang.RuntimeException: Tool return 143
	at it.unina.android.ripper.tools.lib.WrapProcess.waitForSuccess(WrapProcess.java:86)
	at it.unina.android.ripper.tools.logcat.LogcatDumper.run(LogcatDumper.java:74)

==》 AbstractDriver 连接不上socket 看不到效果
