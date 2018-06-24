---
layout: post
title: "iOS AutoLayout"
date: 2018-06-24
categories:
  - iOS
description: Extension을 활용한 AutoLayout. 
image: /assets/images/0624/main.png
image-sm: /assets/images/0624/main_th.png
---


## 코드로 AutoLayout 만들기.
---


<br/>
<br/>
### 라이브러리와 스토리보드없이 AutoLayout잡기..란..?

저번 편에서 이어서 핵데이를 준비하며 공부했던 내용을 써보려한다. 나는 iOS를 개발할 때, 스토리보드를 애용하는 편이다.(apple에서도 스토리보드 사용을 권장하는건 안비밀🤫) 다시 한번 제약을 말하자면 "**1.오픈소스를 사용하지 않는다. 2.스토리보드를 사용하지 않는다.**" 스토리보드를 사용하지 않고 AutoLayout을 잡아야한다.. 그건 **SnapKit**으로 밖에 안해봤는데.. SnapKit은... 라이브러리네..? 하핳...   
가장 기본적인 하나하나 다 오토레이아웃을 추가하는 방법이 있지만, 그렇게되면 코드가 굉장히 길어지고, 나중에 코드 관리하기가 어려워진다. 그래서 각종 swift관련된 AutoLayout자료를 찾아봤다. 크게 VisualFormat과 Constarint를 주는 2가지 방법이있는데 VisualFormat보다는 Constraint가 Extension을 이용해 간편하게 쓸 수 있다. 이번 편에는 그 방법에대해 써보려고 한다.

  
<br />
<br />


### 1. isActive = true?      
  

먼저, 코드로 AutoLayout을 어떻게 쓰는지 보자.  

<div style="background-color: #EDEDED">
{% highlight swift %}  

    let redView = UIView()
            redView.backgroundColor = .red
    
            view.addSubview(redView)
            
            redView.topAnchor.constraint(equalTo: view.topAnchor).isActive = true
            redView.trailingAnchor.constraint(equalTo: view.trailingAnchor).isActive = true
            redView.leadingAnchor.constraint(equalTo: view.leadingAnchor).isActive = true
            redView.bottomAnchor.constraint(equalTo: view.bottomAnchor).isActive = true
            
{% endhighlight %}
</div>  

지금 이 코드는 superView에 **top,leading,traing,bottom**을 다 붙인 코드로 일일이 다 4개를 넣어 줘야한다. 또 반복되는 코드가 보인다. 
이걸 줄여주려면?!  
<br />
  
  
<div style="background-color: #EDEDED">
{% highlight swift %}  

    NSLayoutConstraint.activate([
                redView.topAnchor.constraint(equalTo: view.topAnchor),
                redView.trailingAnchor.constraint(equalTo: view.trailingAnchor),
                redView.leadingAnchor.constraint(equalTo: view.leadingAnchor),
                redView.bottomAnchor.constraint(equalTo: view.bottomAnchor)
                ])
            
{% endhighlight %}
</div>  
NSLayoutConstraint.activate 로 중복을 조금은 줄여줄 수 있다.  
지금은 뷰가 하나기때문에 덜 복잡해 보이지만 우리가 보통 만드는 앱에 뷰는 하나가 아니기때문에 조금 더 간편하게 만들어보자!

  
<br />
<br />
  
  
### 2. UIView + Extension.  
  
UIView의 Extension으로 AutoLayout을 편하게 줄 수 있는 함수를 만들어 보자.  

![extension 샘플 코드](/assets/images/0624/extension.png)
top/leading/trailing/bottom을 매개변수를 받아준다. width나 height같이 다른 constraint로 인해 4개를 꼭 다 안줘도 될 경우도 있기때문에 optional로 만들어준다.  
padding과 size도 함께 넣어준다.  

<br />

![redView 적용 코드](/assets/images/0624/redViewAnchor.png)
이제 아까 redView에 만들어준 Extension 함수를 적용해보자.  
자아- 한 줄로 정리가 됐다!!  
만약에 top이나 leading이나 주지 않아도된다면 nil이라고 써주면 된다.
  
     
<br />

만약에 redView에 top/bottom은 superView 위,아래에 붙이고, trailing은 12만큼 떨어트리고 싶으면
![redView 응용 코드](/assets/images/0624/redView2.png)  
이렇게 써주면 된다. height은 0을 줘도 top/bottom을 줬기때문에 잘 적용된다!

<br />
<br />

### 3. 응용편!  
  
내가 redview에 autolayout준 것 처럼 그냥 SuperView에 딱 붙게 만들고 싶다면  
![fillsSuperView 코드](/assets/images/0624/fill.png)  
앞에 만들어놓은 anchor를 이용해서 fillsSuperView 함수를 만들어줬다.  
superView에 top/leading/trailing/bottom이 딱 맞는 autoLayout이 만들어진다.
<div style="background-color: #EDEDED">
{% highlight swift %}  

    redView.fillsSuperView()
            
{% endhighlight %}
</div>  
띠용-!! 아~~~주 간단해졌드아!  


### 4. 마무리  

지금은 하나의 redView에만 줘서 '이게 그렇게 간단한건가?' 싶을수도 있지만, 하나의 앱을 만들 때 적용하면 코드가 **확연히** 줄어들게된다.  
https://www.letsbuildthatapp.com/ 여기의 강의를 참고했는데, 개인적으로 이 강의를 추천한다. 영어도 이해하기 어렵지 않고, 설명이 쉬운 편이다.(이 분은 스토리보드를 아예 사용하지 않고 코드를 짰었는데, 최근엔 스토리보드를 조금 사용하기 시작하신 것 같다 띠-용)  
코드와 스토리보드 모두 장단점이있지만, 어떤 상황에도 당황하지 않고 할 줄아는 개발자가 되려면 다방면으로 공부해야겠다.  
핵데이를 준비하면서 스토리보드로 할 줄 아니까 코드로 오토레이아웃을 공부하지 않았던 나를 자책하며 급하게 공부를 했다. 역시 발등에 **겁나 큰 불똥**이 떨어지니 퇴근하고 그리고 또 주말에 틈틈이 공부해 확실히 많이 늘었다는 생각이 들었다.  


  
  
    


*※ 출처:  [LBTA_ Making Programmatic Auto Layout Easy through Extensions](https://www.letsbuildthatapp.com/course_video?id=2832)*   
*애플공식문서:  [AutoLayout](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/index.html)* 

<br />


