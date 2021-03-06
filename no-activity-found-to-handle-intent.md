# 錯誤：No Activity found to handle Intent

在使用隱式意圖\(Implicit Intent\)的時候，需要注意可能沒有 Activity 可以處理此 Intent 的情況發生。 例：

```java
String shareTxt = "Hello Android";
Intent shareIntent = new Intent(Intent.ACTION_VIEW);
shareIntent.setType("text/plain");
shareIntent.putExtra(Intent.EXTRA_TEXT, shareTxt);
startActivity(shareIntent);
```

如果沒有 Activity 可以處理即會產生 **ActivityNotFoundException** 錯誤:

```java
02-17 13:54:19.180 14657-14657/? E/AndroidRuntime: FATAL EXCEPTION: main
    Process: com.example.freeman.testintent, PID: 14657
    java.lang.RuntimeException: Unable to start activity ComponentInfo{com.example.freeman.testintent/com.example.freeman.testintent.MainActivity}: android.content.ActivityNotFoundException: No Activity found to handle Intent { act=android.intent.action.VIEW typ=text/plain (has extras) }
        at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2778)
        at android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:2856)
        at android.app.ActivityThread.-wrap11(Unknown Source:0)
        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1589)
        at android.os.Handler.dispatchMessage(Handler.java:106)
        at android.os.Looper.loop(Looper.java:164)
        at android.app.ActivityThread.main(ActivityThread.java:6494)
        at java.lang.reflect.Method.invoke(Native Method)
        at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:438)
        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:807)
     Caused by: android.content.ActivityNotFoundException: No Activity found to handle Intent { act=android.intent.action.VIEW typ=text/plain (has extras) }
        at android.app.Instrumentation.checkStartActivityResult(Instrumentation.java:1937)
        at android.app.Instrumentation.execStartActivity(Instrumentation.java:1616)
        at android.app.Activity.startActivityForResult(Activity.java:4487)
        at android.support.v4.app.BaseFragmentActivityApi16.startActivityForResult(BaseFragmentActivityApi16.java:54)
        at android.support.v4.app.FragmentActivity.startActivityForResult(FragmentActivity.java:67)
        at android.app.Activity.startActivityForResult(Activity.java:4445)
        at android.support.v4.app.FragmentActivity.startActivityForResult(FragmentActivity.java:732)
        at android.app.Activity.startActivity(Activity.java:4806)
        at android.app.Activity.startActivity(Activity.java:4774)
        at com.example.freeman.testintent.MainActivity.onCreate(MainActivity.java:25)
        at android.app.Activity.performCreate(Activity.java:7009)
        at android.app.Activity.performCreate(Activity.java:7000)
        at android.app.Instrumentation.callActivityOnCreate(Instrumentation.java:1214)
        at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2731)
        at android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:2856) 
        at android.app.ActivityThread.-wrap11(Unknown Source:0) 
        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1589) 
        at android.os.Handler.dispatchMessage(Handler.java:106) 
        at android.os.Looper.loop(Looper.java:164) 
        at android.app.ActivityThread.main(ActivityThread.java:6494) 
        at java.lang.reflect.Method.invoke(Native Method) 
        at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:438) 
        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:807)
```

## 處理方式一：用 Intent.resolveActivity 檢查是否有可以處理的 Activity:

```java
if (shareIntent.resolveActivity(getPackageManager()) != null) {
    startActivity(shareIntent);
} else {
    Toast.makeText(this, "No app can perform this action.", Toast.LENGTH_SHORT).show();
}
```

## 處理方式二：用Intent.createChooser

用這個方法即使沒有程式可以處理也會告知使用者，不會出現錯誤。

```java
 startActivity(Intent.createChooser(shareIntent, "Chooser Title"));
```

![](https://user-images.githubusercontent.com/1331862/36363488-7c7c38d4-1578-11e8-853e-3023aa594b60.png)

