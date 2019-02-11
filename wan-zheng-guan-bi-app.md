# 完整退出 APP

為了一次關閉所有 Activity，利用作業系統的方式來關閉 APP 這一個 process。

```java
android.os.Process.killProcess(android.os.Process.myPid());
```



