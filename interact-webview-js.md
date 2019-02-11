# WebView 和 JS 傳參數

source: [https://github.com/HsuHsiaoHsuan/InteractWithJS](https://github.com/HsuHsiaoHsuan/InteractWithJS "InteractWithJS")

## JS 呼叫 Android WebView

1. 設定 WebView 啟用 JavaScript
   ```java
   WebSettings webSettings = webView.getSettings();
   webSettings.setJavaScriptEnabled(true);
   webSettings.setJavaScriptCanOpenWindowsAutomatically(true);
   ```
2. 建立一個內部類別 \(inner class\) 裡面宣告各種讓 JavaScript 方法並且加上 **@JavascriptInterface** 這個 annotation

   ```java
   class JsInterface {
           private Context mContext;
           JsInterface(Context context) {
               mContext = context;
           }

           @JavascriptInterface
           public void showToast(String msg) {
               Toast.makeText(mContext, msg, Toast.LENGTH_SHORT).show();
           }

           @JavascriptInterface
           public void showToolbar(final boolean showIt) {
               runOnUiThread(new Runnable() {
                   @Override
                   public void run() {
                       mToolbar.setVisibility(showIt ? View.VISIBLE : View.GONE);
                   }
               });
           }

           @JavascriptInterface
           public void openAndroidDialog(String title, String msg) {
               AlertDialog.Builder builder = new AlertDialog.Builder(mContext);
               builder.setMessage(msg)
                       .setTitle(title);
               AlertDialog dialog = builder.create();
               dialog.show();
           }

           @JavascriptInterface
           public void lockBackKey(boolean lockIt) {
               lockBackKey = lockIt;
           }
       }
   ```

3. 設定 WebView 的 JavaScript 介面，並且取名稱叫 **JsTest**

   ```java
   webView.addJavascriptInterface(new JsInterface(this), "JsTest");
   webView.loadUrl("file:///android_asset/jsinteract.html");
   ```

4. 在網頁中的 JavaScript 呼叫方法為，**\(JavaScript 介面名稱\).\(@JavascriptInterface定義的函式名稱\)**

   ```js
   <script language="javascript">
       // 呼叫 Android 顯示快顯訊息
       function showAndroidToast(toast) {
           JsTest.showToast(toast);
       }

       // 顯示 Android 標題列, showIt: true|false
       function showToolbar(showIt) {
           JsTest.showToolbar(showIt);
       }

       // 呼叫 Android 顯示對話
       function openAndroidDialog() {
           JsTest.openAndroidDialog("title from html", "msg from html");
       }

       // 鎖住 Android 返回鍵, lockIt: true|false
       function lockBackKey(lockIt) {
           JsTest.lockBackKey(lockIt)
       }

       // 從 Android 傳進來的文字訊息
       function calledFromAndroid(msg){
           document.getElementById("mytext").innerHTML = msg;
       }
   </script>
   ```

## Android WebView 呼叫網頁 JavaScript

1. 呼叫且傳遞訊息至網頁 JavaScript，必須要使用 Runnable 去執行

   ```java
   bCallJs.setOnClickListener(new View.OnClickListener() {
       @Override
       public void onClick(View view) {
           webView.post(new Runnable() {
               @Override
               public void run() {
                   webView.loadUrl("javascript:calledFromAndroid('msg from Android Java')");
               }
           });
       }
   });
   ```

## 程式畫面

![](https://user-images.githubusercontent.com/1331862/35819131-cb7b8b7a-0adc-11e8-9374-dd7d0befa66e.png)

更多資料

1. [http://blog.csdn.net/carson\_ho/article/details/64904635](http://blog.csdn.net/carson_ho/article/details/64904635)
2. [http://blog.csdn.net/carson\_ho/article/details/64904691](http://blog.csdn.net/carson_ho/article/details/64904691)



