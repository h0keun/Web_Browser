### Web_Browser
WebView를 다루면서 미쳐 신경쓰지 못한 디테일한 부분들을 check

+ ConstraintLayout(constraintDimensionRatio)
+ URL 로딩(EditText : inputType textURI > Shape) + ContentLoadingProgressBar
+ WebView(imeOptions actionDone) + Swiperefreshlayout
+ Navigation(hisotry 관리버튼 Home, back, forward) 
  / ripple effect(버튼클릭효과) 간단하게 하기위해  
  android:background="?attr/selectableItemBackground" 속성부여 > 범위좁아지는것을 constraintDimensionRatio로 해결
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
