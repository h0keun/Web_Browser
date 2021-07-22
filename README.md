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
## [2021-07-22]

### manifest
+ 인터넷 권한 추가
```KOTLIN
<uses-permission android:name="android.permission.INTERNET"/>
```
+ http 허용
```KOTLIN
* 보안관련해서 https를 사용하여야 하지만 http또한 허용하도록 하기위해 속성 추가
android:usesCleartextTraffic="true"
``
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
  
+ SwiperFreshlayout 사용 (build.gradle 에 라이브러리 추가해주어야함!)  
  웹뷰 영역을 ``` <androidx.swiperefreshlayout.widget.SwipeRefreshLayout> ```  
  로 감싸주어 스크롤스와이프시 새로고침
+ ContentLoadingProgressBar 사용 
  웹사이트 이동간에 사용자에게 진행상황을 알려주기위함
  ```KOTLIN
  <androidx.core.widget.ContentLoadingProgressBar>
  ...
      style="@style/Widget.AppCompat.ProgressBar.Horizontal" /> 
  ```
+ 레이아웃부분에서는 크게 어려운게 없음 다만 아는게 많을수록 더 이쁘게 꾸밀수 있다랄까?  
   contraintLayout에서 상황별 사용가능한 속성들에대해 전부 정리해두는것도 좋을듯
  
### kotlin.kt
🥕 WebView 공식 문서 참조 [📌](https://developer.android.com/guide/webapps/webview?hl=ko)  
🥕 WebView 자체가 webpage를 가져와서 그대로 view에 뿌려주는것이기 때문에 크게 작성해야할 코드가 많지 않음  

+ 웹 뷰 사용하기(초기화) - initViews()
```KOTLIN
* initViews()는 onCreate()에서 호출
* 웹뷰 클라이언트와 웹크롬 클라이언트 할당(inner class 사용함!)
* 웹페이지의 원활한 사용을 위해 자바스크립트 허용
  
private fun initViews() {
    webView.apply {
        webViewClient = WebViewClient()
        webChromeClient = WebChromeClient()
        settings.javaScriptEnabled = true
        loadUrl(DEFAULT_URL)   // companion object에서 선언한 초기 실행 홈페이지
    }
}

inner class WebViewClient : android.webkit.WebViewClient() {
...
}

inner class WebChromeClient : android.webkit.WebChromeClient() {
...
}

* 의아한 점이 WebViewClient와 WebChromeClient 를 그대로 사용하지 않고
  이를 상속한 클래스를 따로 작성했다는 것인데,
  이는 SwipeRefreshLayout과 ContentLoadingProgressBar를 보다 적절히 입맛대로
  사용하기 위해 필요한 부분을 오버라이딩 한 것임!
```

+ 뒤로가기버튼 클릭시 이전사이트 이동 및 더이상 뒤로갈 사이트 없을 때 앱 종료(원래 뒤로가기버튼의 기능)
```KOTLIN
override fun onBackPressed() {
    if (webView.canGoBack()) {
        webView.goBack()
    } else {
        super.onBackPressed()
    }
}
```
+ 웹사이트 새로고침 및 이동간 프로그래스바로 상태알리기  
```KOTLIN
* onPagestarted 에서 progressBar.show() 
  onPageFinished 에서 progressBar.hide() 해주면서 
  url을 edittext에 set
  
* SwipeRefreshLayout 을 통해 새로고침 하거나 다른 사이트로 이동시
  페이지 로딩 > 프로그래스바 상태알림(show)
  페이지 로딩완료 > 프로그래스바 상태알림(hide)

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

* goBackButton()과 goForwardButton()은 각각 webView.goBack() 와 webView.goForward()
  를 실행하는 버튼 이며 canGoBack()메서드와 canGoForward()를 통해 true false를 지정받아
  버튼 활성화 여부를 제어한다.

inner class WebChromeClient : android.webkit.WebChromeClient() {

     override fun onProgressChanged(view: WebView?, newProgress: Int) {
         super.onProgressChanged(view, newProgress)

         progressBar.progress = newProgress
     }
}

* progressBar에서 사용 될 페이지 로딩의 진행정도(0~100) 를 받아오기위해
  onProgressChanged를 오버라이드 하여 newprogress를 그대로 progress 에 
```
+ 주소창 컨트롤(addressBar - EditText)
```KOTLIN
* 주소창 입력시 올라오는 키보드에 대해서
  xml에서 android:imeOptions="actionDone"  // 키보드의 액션 설정(입력완료)
  위와 같은 설정을 해주었기 때문에 키보드 자판에서 엔터가 위치한곳의 버튼이
  엔터가 아니라 입력완료버튼으로 바뀌게됨
  
* 이를 통해 주소입력후 '완료' 를 누르면 바로 사이트 이동이 가능해지며
  이때 URLUtil.isNetworkUrl()을 사용해서 입력한 값이 실제 url이면 바로 이동하고
  그렇지 않으면 앞에 https:// 를 붙여서 이동시킴
  
* 최종적으로 return@setOnEditorActionListener false 를 통해 키보드를 닫아준다.
  
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


💡 inner class, enum class, data class 등 여러가지 클래스들에대해 확실히 알것  
```KOTLIN
* 이 프로젝트에서 inner class로 선언한 이유

👉 현재 메인 엑티비티 클래스 내부 멤버에도 접근이 가능하도록 해주기 위함
    만약 inner class 가 아니라 그냥 class 였다면 
    progressBar를 비롯해서 goBackButton, refreshLayout 등 전부 사용불가 였쥐
```
💡 companion object 가 이전 프로젝트에서부터 계속 자주 등장한다. 이부분 확실히 체크하자  
