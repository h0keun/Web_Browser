```๐ก FastCampus ๊ฐ์ ์๊ฐ ๋ฐ ์ ๋ฆฌ```

### Web_Browser
WebView๋ฅผ ๋ค๋ฃจ๋ฉด์ ๋ํ์ผํ ๋ถ๋ถ๋ค์ check

+ ConstraintLayout(constraintDimensionRatio)
+ URL ๋ก๋ฉ(EditText : inputType textURI > Shape) + ContentLoadingProgressBar
+ WebView(imeOptions actionDone) + Swiperefreshlayout
+ Navigation(hisotry ๊ด๋ฆฌ๋ฒํผ Home, back, forward) 
  / ripple effect(๋ฒํผํด๋ฆญํจ๊ณผ) ๊ฐ๋จํ๊ฒ ํ๊ธฐ์ํด  
  android:background="?attr/selectableItemBackground" ์์ฑ๋ถ์ฌ > ๋ฒ์์ข์์ง๋๊ฒ์ constraintDimensionRatio๋ก ํด๊ฒฐ
+ onBackPressed()


๐ก tip. ์ค์ฝํํจ์ apply๋ก ์ฌ๋ฌ๋ฒ ์ ๊ทผ(ํธ์ถ)ํ  ํ์ ์์ด ๊น๋ํ๊ฒ ๊ฐ๋ฅ

```KOTLIN
private fun initViews() {
  webView.webViewClient = WebViewClient()
  webView.settings.javaScriptEnabled = true
  webView.loadUrl("http://www.github.com/h0keun")
}

///////////////////////////////////////////////////
private fun initViews() {
  webView.apply {
    webViewClient = webViewClient()
    setting.javaScriptEnabled = true
    loadUrl("http://www.github.com/h0keun")
  }
}
```
<img src="https://user-images.githubusercontent.com/63087903/119834830-4403af80-bf3b-11eb-953b-0c8475d55324.jpg" width="200" height="430"> <img src="https://user-images.githubusercontent.com/63087903/119834838-45cd7300-bf3b-11eb-9bf6-4967ae4b226d.jpg" width="200" height="430">

## [2021-05-12]
## [2021-07-22]

### manifest
+ ์ธํฐ๋ท ๊ถํ ์ถ๊ฐ
```KOTLIN
<uses-permission android:name="android.permission.INTERNET"/>
```
+ http ํ์ฉ
```KOTLIN
* ๋ณด์๊ด๋ จํด์ https๋ฅผ ์ฌ์ฉํ์ฌ์ผ ํ์ง๋ง http๋ํ ํ์ฉํ๋๋ก ํ๊ธฐ์ํด ์์ฑ ์ถ๊ฐ
android:usesCleartextTraffic="true"
```
### layout.xml
+ ์ด๋ค ๋ ์ด์์์ ์ด๋ฏธ์ง๋ฒํผ์ ์ถ๊ฐํ์ฌ ์ฌ์ฉํ๊ณ ์ ํ  ๋ ๊ฒช์๋ ๋ฌธ์   
  : ์ด๋ฏธ์ง๊ฐ ๋ค์ด๊ฐ์๋ ๋ทฐ์ ์์ญ ํฌ๊ธฐ๋งํผ ๋ฒํผํด๋ฆญํจ๊ณผ๋ฅผ ์ฃผ๊ณ ์ถ์๋ฐ  
    ๋ทฐ ์์ ์ด๋ฏธ์ง๋ถ๋ถ๋ง ํด๋ฆญ๋๊ณ  ๋ค๋ฅธ ์์ญ์ ํด๋ฆญ์ด ์๋  ๋ ๋ฑ 
```KOTLIN
// 1. wrap_content๋ match_parent ๋ฅผ 0dp๋ก ๋ฐ๊พธ์ด์ค๋ค.
  
   android:layout_width="0dp"
   android:layout_height="0dp"
  
// 2. backgroundcolor๋ฅผ ์๋์ ๊ฐ์ด ์ง์ ํด์ฃผ๋ฉด ์์ฐ์ค๋  rippleํจ๊ณผ๋ ๋ค์ด๊ฐ๋ค.
  
   android:background="?attr/selectableItemBackground"
   android:src="@drawable/ic_home"
  
// 3. ๋น์จ ์กฐ์ 
   app:layout_constraintDimensionRatio="1:1" 
```
+ EditText ์์ญ์ ์ฃผ์์๋ ฅ์ฐฝ์ผ๋ก ์ฌ์ฉํ  ๋
  : ํค๋ณด๋์๋ ฅํ ์๋ฃ ๋ฅผ ๋๋ฅด๋ฉด ๊ทธ๊ฒ์ ์ด๋ฒคํธ์ฒ๋ฆฌํ์ฌ ์ฌ์ดํธ ์ด๋์ํค๊ธฐ(์ฒ๋ฆฌ๋ ์ฝํ๋ฆฐ ํด๋์ค์์ ๋ง๋ฌด๋ฆฌํด์ผํจ)
  ```KOTLIN
  android:imeOptions="actionDone"  // ํค๋ณด๋์ ์ก์ ์ค์ (์๋ ฅ์๋ฃ)
  android:importantForAutofill="no" // ์๋์์ฑ no
  android:inputType="textUri" // ์ฃผ์์๋ ฅ
  android:selectAllOnFocus="true"  // ํ๋ฐฉ์ url ์ ๋ถ ์ ํ ๊ฐ๋ฅํ๋๋ก
  ```
  [EditText ์์ฑ ์์๋ณด๊ธฐ](https://recipes4dev.tistory.com/92)
  
+ SwiperFreshlayout ์ฌ์ฉ (build.gradle ์ ๋ผ์ด๋ธ๋ฌ๋ฆฌ ์ถ๊ฐํด์ฃผ์ด์ผํจ!)  
  ์น๋ทฐ ์์ญ์ ``` <androidx.swiperefreshlayout.widget.SwipeRefreshLayout> ```  
  ๋ก ๊ฐ์ธ์ฃผ์ด ์คํฌ๋กค์ค์์ดํ์ ์๋ก๊ณ ์นจ
+ ContentLoadingProgressBar ์ฌ์ฉ 
  ์น์ฌ์ดํธ ์ด๋๊ฐ์ ์ฌ์ฉ์์๊ฒ ์งํ์ํฉ์ ์๋ ค์ฃผ๊ธฐ์ํจ
  ```KOTLIN
  <androidx.core.widget.ContentLoadingProgressBar>
  ...
      style="@style/Widget.AppCompat.ProgressBar.Horizontal" /> 
  ```
+ ๋ ์ด์์๋ถ๋ถ์์๋ ํฌ๊ฒ ์ด๋ ค์ด๊ฒ ์์ ๋ค๋ง ์๋๊ฒ ๋ง์์๋ก ๋ ์ด์๊ฒ ๊พธ๋ฐ์ ์๋ค๋๊น?  
   contraintLayout์์ ์ํฉ๋ณ ์ฌ์ฉ๊ฐ๋ฅํ ์์ฑ๋ค์๋ํด ์ ๋ถ ์ ๋ฆฌํด๋๋๊ฒ๋ ์ข์๋ฏ
  
### kotlin.kt
๐ฅ WebView ๊ณต์ ๋ฌธ์ ์ฐธ์กฐ [๐](https://developer.android.com/guide/webapps/webview?hl=ko)  
๐ฅ WebView ์์ฒด๊ฐ webpage๋ฅผ ๊ฐ์ ธ์์ ๊ทธ๋๋ก view์ ๋ฟ๋ ค์ฃผ๋๊ฒ์ด๊ธฐ ๋๋ฌธ์ ํฌ๊ฒ ์์ฑํด์ผํ  ์ฝ๋๊ฐ ๋ง์ง ์์  

+ ์น ๋ทฐ ์ฌ์ฉํ๊ธฐ(์ด๊ธฐํ) - initViews()
```KOTLIN
* initViews()๋ onCreate()์์ ํธ์ถ
* ์น๋ทฐ ํด๋ผ์ด์ธํธ์ ์นํฌ๋กฌ ํด๋ผ์ด์ธํธ ํ ๋น(inner class ์ฌ์ฉํจ!)
* ์นํ์ด์ง์ ์ํํ ์ฌ์ฉ์ ์ํด ์๋ฐ์คํฌ๋ฆฝํธ ํ์ฉ
  
private fun initViews() {
    webView.apply {
        webViewClient = WebViewClient()
        webChromeClient = WebChromeClient()
        settings.javaScriptEnabled = true
        loadUrl(DEFAULT_URL)   // companion object์์ ์ ์ธํ ์ด๊ธฐ ์คํ ํํ์ด์ง
    }
}

inner class WebViewClient : android.webkit.WebViewClient() {
...
}

inner class WebChromeClient : android.webkit.WebChromeClient() {
...
}

* ์์ํ ์ ์ด WebViewClient์ WebChromeClient ๋ฅผ ๊ทธ๋๋ก ์ฌ์ฉํ์ง ์๊ณ 
  ์ด๋ฅผ ์์ํ ํด๋์ค๋ฅผ ๋ฐ๋ก ์์ฑํ๋ค๋ ๊ฒ์ธ๋ฐ,
  ์ด๋ SwipeRefreshLayout๊ณผ ContentLoadingProgressBar๋ฅผ ๋ณด๋ค ์ ์ ํ ์๋ง๋๋ก
  ์ฌ์ฉํ๊ธฐ ์ํด ํ์ํ ๋ถ๋ถ์ ์ค๋ฒ๋ผ์ด๋ฉ ํ ๊ฒ์!
```

+ ๋ค๋ก๊ฐ๊ธฐ๋ฒํผ ํด๋ฆญ์ ์ด์ ์ฌ์ดํธ ์ด๋ ๋ฐ ๋์ด์ ๋ค๋ก๊ฐ ์ฌ์ดํธ ์์ ๋ ์ฑ ์ข๋ฃ(์๋ ๋ค๋ก๊ฐ๊ธฐ๋ฒํผ์ ๊ธฐ๋ฅ)
```KOTLIN
override fun onBackPressed() {
    if (webView.canGoBack()) {
        webView.goBack()
    } else {
        super.onBackPressed()
    }
}
```
+ ์น์ฌ์ดํธ ์๋ก๊ณ ์นจ ๋ฐ ์ด๋๊ฐ ํ๋ก๊ทธ๋์ค๋ฐ๋ก ์ํ์๋ฆฌ๊ธฐ  
```KOTLIN
* onPagestarted ์์ progressBar.show() 
  onPageFinished ์์ progressBar.hide() ํด์ฃผ๋ฉด์ 
  url์ edittext์ set
  
* SwipeRefreshLayout ์ ํตํด ์๋ก๊ณ ์นจ ํ๊ฑฐ๋ ๋ค๋ฅธ ์ฌ์ดํธ๋ก ์ด๋์
  ํ์ด์ง ๋ก๋ฉ > ํ๋ก๊ทธ๋์ค๋ฐ ์ํ์๋ฆผ(show)
  ํ์ด์ง ๋ก๋ฉ์๋ฃ > ํ๋ก๊ทธ๋์ค๋ฐ ์ํ์๋ฆผ(hide)

inner class WebViewClient : android.webkit.WebViewClient() {

     override fun onPageStarted(view: WebView?, url: String?, favicon: Bitmap?) {
         super.onPageStarted(view, url, favicon)

         progressBar.show()
     }

     override fun onPageFinished(view: WebView?, url: String?) {
         super.onPageFinished(view, url)

         refreshLayout.isRefreshing = false
         progressBar.hide()
         goBackButton.isEnabled = webView.canGoBack()
         goForwardButton.isEnabled = webView.canGoForward()
         addressBar.setText(url)
     }
}

* goBackButton()๊ณผ goForwardButton()์ ๊ฐ๊ฐ webView.goBack() ์ webView.goForward()
  ๋ฅผ ์คํํ๋ ๋ฒํผ ์ด๋ฉฐ canGoBack()๋ฉ์๋์ canGoForward()๋ฅผ ํตํด true false๋ฅผ ์ง์ ๋ฐ์
  ๋ฒํผ ํ์ฑํ ์ฌ๋ถ๋ฅผ ์ ์ดํ๋ค.

inner class WebChromeClient : android.webkit.WebChromeClient() {

     override fun onProgressChanged(view: WebView?, newProgress: Int) {
         super.onProgressChanged(view, newProgress)

         progressBar.progress = newProgress
     }
}

* progressBar์์ ์ฌ์ฉ ๋  ํ์ด์ง ๋ก๋ฉ์ ์งํ์ ๋(0~100) ๋ฅผ ๋ฐ์์ค๊ธฐ์ํด
  onProgressChanged๋ฅผ ์ค๋ฒ๋ผ์ด๋ ํ์ฌ newprogress๋ฅผ ๊ทธ๋๋ก progress ์ 
```
+ ์ฃผ์์ฐฝ ์ปจํธ๋กค(addressBar - EditText)
```KOTLIN
* ์ฃผ์์ฐฝ ์๋ ฅ์ ์ฌ๋ผ์ค๋ ํค๋ณด๋์ ๋ํด์
  xml์์ android:imeOptions="actionDone"  // ํค๋ณด๋์ ์ก์ ์ค์ (์๋ ฅ์๋ฃ)
  ์์ ๊ฐ์ ์ค์ ์ ํด์ฃผ์๊ธฐ ๋๋ฌธ์ ํค๋ณด๋ ์ํ์์ ์ํฐ๊ฐ ์์นํ๊ณณ์ ๋ฒํผ์ด
  ์ํฐ๊ฐ ์๋๋ผ ์๋ ฅ์๋ฃ๋ฒํผ์ผ๋ก ๋ฐ๋๊ฒ๋จ
  
* ์ด๋ฅผ ํตํด ์ฃผ์์๋ ฅํ '์๋ฃ' ๋ฅผ ๋๋ฅด๋ฉด ๋ฐ๋ก ์ฌ์ดํธ ์ด๋์ด ๊ฐ๋ฅํด์ง๋ฉฐ
  ์ด๋ URLUtil.isNetworkUrl()์ ์ฌ์ฉํด์ ์๋ ฅํ ๊ฐ์ด ์ค์  url์ด๋ฉด ๋ฐ๋ก ์ด๋ํ๊ณ 
  ๊ทธ๋ ์ง ์์ผ๋ฉด ์์ https:// ๋ฅผ ๋ถ์ฌ์ ์ด๋์ํด
  
* ์ต์ข์ ์ผ๋ก return@setOnEditorActionListener false ๋ฅผ ํตํด ํค๋ณด๋๋ฅผ ๋ซ์์ค๋ค.
  
addressBar.setOnEditorActionListener { v, actionId, event ->
     if (actionId == EditorInfo.IME_ACTION_DONE) {
         val loadingUrl = v.text.toString()
         if (URLUtil.isNetworkUrl(loadingUrl)) {
             webView.loadUrl(loadingUrl)
         } else {
             webView.loadUrl("http://$loadingUrl")
         }
     }

     return@setOnEditorActionListener false
}
```


๐ก inner class, enum class, data class ๋ฑ ์ฌ๋ฌ๊ฐ์ง ํด๋์ค๋ค์๋ํด ํ์คํ ์๊ฒ  
```KOTLIN
* ์ด ํ๋ก์ ํธ์์ inner class๋ก ์ ์ธํ ์ด์ 

๐ ํ์ฌ ๋ฉ์ธ ์ํฐ๋นํฐ ํด๋์ค ๋ด๋ถ ๋ฉค๋ฒ์๋ ์ ๊ทผ์ด ๊ฐ๋ฅํ๋๋ก ํด์ฃผ๊ธฐ ์ํจ
    ๋ง์ฝ inner class ๊ฐ ์๋๋ผ ๊ทธ๋ฅ class ์๋ค๋ฉด 
    progressBar๋ฅผ ๋น๋กฏํด์ goBackButton, refreshLayout ๋ฑ ์ ๋ถ ์ฌ์ฉ๋ถ๊ฐ ์์ฅ
```
๐ก companion object ๊ฐ ์ด์  ํ๋ก์ ํธ์์๋ถํฐ ๊ณ์ ์์ฃผ ๋ฑ์ฅํ๋ค. ์ด๋ถ๋ถ ํ์คํ ์ฒดํฌํ์  
