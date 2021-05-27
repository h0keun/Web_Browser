```💡 FastCampus 강의 수강 및 정리```

### Web_Browser
WebView를 다루면서 디테일한 부분들을 check

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
<img src="https://user-images.githubusercontent.com/63087903/119834830-4403af80-bf3b-11eb-953b-0c8475d55324.jpg" width="200" height="430"> <img src="https://user-images.githubusercontent.com/63087903/119834838-45cd7300-bf3b-11eb-9bf6-4967ae4b226d.jpg" width="200" height="430">

## [2021-05-12]

### layout.xml
+ 어떤 레이아웃에 이미지버튼을 추가하여 사용하고자 할 때 겪었던 문제  
  : 이미지가 들어가있는 뷰의 영역 크기만큼 버튼클릭효과를 주고싶은데 뷰 안의 이미지부분만 클릭되고 다른 영역은 클릭이 안될 때 등 
  - 위의 부분을 해결할 수 있는 디테일한 방법은 다음과 같다.(constratintlayout 기준)
    ```KOTLIN
    // 1. wrap_content나 match_parent 를 0dp로 바꾸어준다.
  
    android:layout_width="0dp"
    android:layout_height="0dp"
  
    // 2. backgroundcolor를 아래와 같이 지정해주면 자연스레 ripple효과도 들어간다.
  
    android:background="?attr/selectableItemBackground"
    android:src="@drawable/ic_home"
  
    // 3. 비율 조정
    app:layout_constraintDimensionRatio="1:1" 
    ```
+ EditText 영역을 주소입력창으로 사용할 때
  : 키보드입력후 완료 를 누르면 그것을 이벤트처리하여 사이트 이동시키기(처리는 코틀린 클래스에서 마무리해야함)
  ```KOTLIN
  android:imeOptions="actionDone"  // 키보드의 액션 설정(입력완료)
  android:importantForAutofill="no" // 자동완성 no
  android:inputType="textUri" // 주소입력
  android:selectAllOnFocus="true"  // 한방에 url 전부 선택 가능하도록
  ```
  [EditText 속성 알아보기](https://recipes4dev.tistory.com/92)
  
+ SwiperFreshlayout 사용  
  웹뷰 영역을 ``` <androidx.swiperefreshlayout.widget.SwipeRefreshLayout> ```  
  로 감싸주어 스크롤스와이프시 새로고침
+ ContentLoadingProgressBar 사용 
  웹사이트 이동간에 사용자에게 진행상황을 알려주기위함
  ```KOTLIN
  <androidx.core.widget.ContentLoadingProgressBar>
  ...
      style="@style/Widget.AppCompat.ProgressBar.Horizontal" />
  ```
  
### kotlin.kt
+ onBackPressed() 를 따로 설정하여 webview에서 사이트 이동하다가 뒤로가기버튼 눌렀을때 이전 사이트로 이동하고, 더이상 뒤로갈 사이트 없을 때 앱 종료하도록
+ WebView 공식 문서 참조 [📌](https://developer.android.com/guide/webapps/webview?hl=ko)
+ 웹사이트 이동간 프로그래스바로 상태알리기
  어렵지 않음 그냥 webview 에서 ```onPagestarted 에서 progressBar.show()``` 해주고
  ```onPageFinished 에서 progressBar.hide()``` 해주면서 url을 edittext에 set 하면됨
  
참조
```KOTLIN
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

inner class WebChromeClient : android.webkit.WebChromeClient() {

     override fun onProgressChanged(view: WebView?, newProgress: Int) {
         super.onProgressChanged(view, newProgress)

         progressBar.progress = newProgress
     }
}
```
  
💡 ```return@setOnEditorActionListener false``` 직관적으로 이해하기 쉽지만 그래도 코틀린에서 리턴문 다시 알아보기
