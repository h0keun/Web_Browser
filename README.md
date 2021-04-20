### Web_Browser

+ ConstraintLayout
+ URL 로딩(EditText : inputType textURI)
+ WebView(imeOptions actionDone)
+ Navigation(hisotry 관리 Home, back, forward)
+ onBackPressed()

💡 tip. 스코프함수 apply로 여러번 접근(호출)할 필요 없이 깔끔하게 가능

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
