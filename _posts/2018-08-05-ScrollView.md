---
layout: post
title: "ScrollView"
date: 2018-08-05
categories:
  - iOS
description: ScrollView에 오토레이아웃 주기.
image: /assets/images/0805/main.png
image-sm: /assets/images/0805/main_th.png
---


## ScrollView야 길어져라
---


<br/>
<br/>
### ScrollView는 왜...

스크롤뷰는 사용 방법이 복잡하지는 않지만 사용할 때마다 구글링을하고 한 번에 스크롤을 성공시키지 못하는 참 신기한 뷰이다. 스크롤뷰 안의 하나의 컴포넌트라도 오토레이아웃이 없으면 스토리보드의 오토레이아웃들이 **와다다다** 빨간색이 떠버린다(~~엄청난 공포~~). 그래도 방법을 터득해 스크롤뷰 안에 다양한 내용이 와도 만들 요령을 터득했다! 오늘은 그 간단한 방법에 대해 써보려고한다.
    
  
  
<br />
<br />


### 1. 스크롤뷰안에 컨테이너뷰를 넣자.      

![시작 스토리보드](/assets/images/0805/01.png)
1.스토리보드에 **ScrollView**를 추가해준다.  
2.**ScrollView**안에  **View**를 넣어준다.(이미지에서 view는 ContainerView이다.)  
3.상단의 이미지처럼 ScrollView는 SuperView에 **Top,Bottom,Leading,Trailing**을 맞추고, ContainerView는 ScrollView에 맞춰준다.  
4.* **가장 중요한 부분** * ContainverView는 ScrollView에 **Equal Height**을 **Priority를 250**로 준다.

  
<br />
<br />
  
  
### 2. 안에 내용들을 넣어보자.   
  
  
![내용 넣은 스토리보드](/assets/images/0805/02.png)
1.편의상 내용을 그룹핑해 한 뷰에 넣어줬다. 왼쪽에 뷰 구성을 보면 이해가 더 빠를 거다.   
2.각각 오토레이아웃을 맞춰준다.  


<br />
<br />



### 3. 높이를 늘이자!  
공지를 쓰면서 길이가 늘어나면 뷰의 높이도 늘어나게 해주는 레이아웃을 추가해줘야 한다. 그냥 고정하면 TextView 높이는 그대로이고, 스크롤이 생기기 때문에 답답해진다. TextView높이를 늘이

![notiveTextView 오토레이아웃](/assets/images/0805/03.png)
1.notice뷰 안에 TextView의 Top,Bottom을 설정해준다.  
2.TextView의 높이를 **>= 77**이라고 해준다. 이건 부호 그대로 77보다 같거나 크다는 뜻이다. **Priority도 250**로 해준다.  
3.TextView의 **Scrolling Enabled** 체크를 꺼준다. (TextView의 스크롤이 아닌 전체 스크롤이 늘어날 거기 때문에! 😝) 

    
<br />

![notiveView 오토레이아웃](/assets/images/0805/04.png)
1.TextView의 SuperView인 notice의 높이의 **Priority를 250**으로 준다.


<br />
<br />


### 4. 완성!!  
![완성gif](/assets/images/0805/finish.gif)  
완성이다. 두구두구 TextView에 따라 높이가 늘어나는 걸 볼 수 있다. 물론 같은 뷰 안에 모든 요소들이 다 오토레이아웃이 잘 잡혀있어야 한다!  
  

<br />
<br />


### 4. 마무리  
  
중요도를 조절해줌으로써 스크롤뷰를 편하게 다룰 수 있다. 하지만 아무래도 오토레이아웃자체도 익숙하게 다루는데 시간이 좀 걸리고, 어떻게 주는게 효과적인지 생각하면서 해야 하기 때문에 번거롭기도 하다. 하지만 한번 하기 시작하면 그렇게 어렵지 않다!(~~정말로 눈누😝~~)  
  
질문과 다양한 피드백은 언제든지 환영합니다.😉😉   


  
<br />
<br />


<br />


