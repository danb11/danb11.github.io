---
layout: post
title: "swift Gesture_ 2.SwipeGesture"
date: 2018-05-26
categories:
  - iOS
description: iOS Gesture에 대해 알아봅시다. 2.SwipeGesture
image: /assets/images/0526/main.png
image-sm: /assets/images/0526/main_th.png
---


## iOS의 터치 제스처에 대해 알아봅시다.
---


<br/>
<br/>
### 쉭쉭 스와이프 제스쳐!
  

1편에 Tap과 LongPress에 이어 **SwipeGesture** 이다. 아이폰 사이즈가 커지면서 Swipe를 더 많이 사용하게 되는 것 같다. 스와이프 제스쳐를 잘 사용하면 한 손으로 기능을 동작시켜줘 사용성을 높여줄 수 있다.  
물론 기본적으로 아이폰에서 제공되는 제스쳐도 많다. 사용성이 좋지만 기본 제스쳐와 내가 추가한 제스처가 겹치지 않도록 잘 만들어줘야한다.

<br />
<br />


### 1. UIGestureRecognizer 추가하기      


![gesture추가](/assets/images/0526/storyboard1.png)

1.제스처를 체크할 Label을 만든다.  
스토리보드에서 Label을 넣고, **스와이프** 라는 텍스트를 넣어줬다.  




<br />
<br />
  
  
### 2. UISwipeGestureRecognizer를 만들어주자  
  
    
    
![제스처recognizer추가](/assets/images/0526/gesture1.png)


1.UISwipeGestureRecognizer를 코드만들어준다.
1편에서는 스토리보드에서 넣어줬던 Recognizer로 taget은 self 그리고 action에는 진짜 구현해줄 함수를 써준다.  
물론 이 함수는 라이프사이클 안에 넣어줘야한다. (나는 viewDidLoad에 넣어줬다.)
  
  
  
  
![제스처recognizer추가](/assets/images/0526/gesture2.png)



2.스와이프 제스처는??  
스와이프 제스처에는 어떤 기능이 있나 봤다. (cmd + UISwipeGestureRecognizer를 클릭하면 definition 기능을 볼 수 있다.)  
2가지 기능을 줄 수 있다.  
먼저 손가락 몇개롤 스와이프를 했을 경우인지 설정해줄 수 있고, 어떤 방향으로 스와이프를 설정해줄 것인지 정할 수 있다.
  
  
  
     
![버튼액션코드](/assets/images/0526/gesture3.png)

2.제스처를 취했을 때, 동작할 함수를 만들어준다.   
매개변수 gesture에 direction으로 .left / .right / .up / .down 을 써줄 수 있다. 나는 **left**를 써줬다.




<br />
<br />

### 3. 디테일한 설정  
 
  
  
![action함수추가](/assets/images/0526/storyboad2.png)


1.어디로 스와이프? 몇 번 터치?  
Tap, LongPress와 마찬가지로 swipe도 스토리보드에서 설정해줄 수 있다.(물론 코드도 가능하다-!)
  
  
<br />


### 4. 완성-!!
  
![완성gif](/assets/images/0526/swipe.gif)
자 이제 확인해 보자. 오른쪽으로 스와이프하면 변화가 없지만 왼쪽으로 스와이프하면? 텍스트가 바뀐다~  
응용해서 다양하게 사용할 수 있는 swipe gesture이다.    
다음에는 gesture말고 새로 공부한 것에 대해 더 많이 써봐야겠다.   



[iOS UISwipeGesture문서 보기](https://developer.apple.com/documentation/uikit/uiswipegesturerecognizer)



<br />


