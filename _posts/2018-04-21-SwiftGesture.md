---
layout: post
title: "swift Gesture_ 1.TapGesture"
date: 2018-04-21
categories:
  - iOS
description: iOS Gesture에 대해 알아봅시다. 1.TapGesture
image: /assets/images/0407/main_big.png
image-sm: /assets/images/0407/main_small.png
---


## iOS의 터치 제스처에 대해 알아봅시다.
---


<br/>
<br/>
### 스마트기기에서 터치 액션이란..
  

예전에 애플에 관련된 다큐멘터리를 본 적이 있다. 아마 20살 수업 시간 때였던 것 같다. 다큐멘터리에서 아이팟을 개발하는 장면이 있다. 잡스는 사용자가 노래를 들으면서 하고자하는 행동(일시정지, 다음노래, 등)을 한번에 이동할 수 있어야한다고 주장하는 장면이 기억난다. 생각해보면 그게 다 지금의 UX의 시초가 아닐까 생각된다.  
서론이 거창했지만, iOS의 Gesture에 대해서 시리즈로 써볼까 한다. Gesture는 말 그대로 화면의 터치를 인식하는 액션이다. swift엔 총 7가지의 Gesture가 있지만 자주 사용하는 것을 위주로 써보려한다. 첫 시리즈는 TapGesture & LongPressGesture 이다.

<br />
<br />


### 1. UIGestureRecognizer 추가하기      


![gesture추가](/assets/images/0421/screen1.png)

1.동작을 추가할 뷰를 만든다.  
View, ImageView 등 동작을 테스트할 뷰에 넣어준다. 나는 핑크색 UIView를 만들었다.  

2.동작 제스처를 추가한다.  
스토리보드에서 넣고자하는 Gesture를 동작할 뷰에 드래그&드랍해서 넣어준다. 나는 Tap Gesture와 Long Press Gesture를 넣어줬다. Gesture Recognize를 추가하면 뷰컨트롤러 상단에 작은 아이콘(파란원)이 생기고 왼쪽에 파란 네모박스에 나타난다. 잘 들어갔나 확인해보자.

  
    
    
    
    
### 2. 액션 func 만들어주기  

Tap 됐을 때, Long Press 됐을 때 어떤 동작을 했으면 좋겠는지 Action 함수를 만들어 준다.
  
  
  
  
![action함수추가](/assets/images/0421/screen2.png)


1.Action 함수를 연결해준다.  
버튼 **@IBAction**과 같은 방법으로 드래그해서 IBAction을 만들어준다.
  
     
     
![버튼액션코드](/assets/images/0421/screen3.png)

2.제스처를 취했을 때, 동작할 행동을 추가해준다.   
나는 원래 핑크색이었던 colorView의 색을 Tap 했을 때는 darkGray, LongPress햇을 때는 yellow로 바꿔줬다.




<br />
<br />

### 3. 디테일한 설정  
 
  
  
![action함수추가](/assets/images/0421/screen4.png)


1.몇 번 탭? 몇 초 터치?  
몇 번 탭 or 터치, 몇 초 눌렀을 때 LongPress가 동작할 것인지 위의 이미지처럼 설정하는 곳에서 설정해준다.
  
  
<br />


### 4. 완성-!!
  
![완성gif](/assets/images/0421/gesture.gif)
자 이제 확인해 보자. 내가 만든 제스처가 잘 동작하는지 확인하면된다. 나는 LongPress를 너무 짧게 설정했는지 빠르게 노란화면이 나온다.. 잘보면서 맞춰서 설정해주면 될 것 같다.  





  
<br />


