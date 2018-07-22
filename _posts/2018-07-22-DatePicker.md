---
layout: post
title: "DatePicker"
date: 2018-07-22
categories:
  - iOS
description: DatePicker를 사용해보자
image: /assets/images/0722/main.png
image-sm: /assets/images/0722/main_th.png
---


## 날짜 선택은 DatePicker!
---


<br/>
<br/>
### 날짜를 선택해봅시다

Picker View만 사용하다가 DatePicker를 사용할 기회가 없었는데 이번에 사용할 기회가 있었다. 천천히 스윽 보면 한번에 쉽게 할 수 있을 것이다.  
    
  
  
<br />
<br />


### 1. 스토리보드에서 설정      

![xcode 스토리보드](/assets/images/0722/story.png)
1. 스토리보드에 **DatePicker**를 추가해준다.
2. **ToolBar**도 DatePicker 상단에 넣어준다. (버튼 설정은 알아서~)
3. 피커에 선택하면 나타내줄 **Label**도 넣어준다.

  
<br />
<br />
  
  
### 2. Date를 설정해보자.  
  
  
![xcode 세팅2](/assets/images/0722/dateSetting.png)
1. DatePicker와 Label의 Outlet 연결한다.  
2. Date의 minimumDate를 Date()로 설정해주면 오늘 날짜로 설정된다.

  
![액션연결](/assets/images/0722/action.png)
3. DatePicker의 Action을 연결한다.  
4. sender는 Date타입이기 때문에 Label에 String을 넣어줄 수 없다.  

  
![extension](/assets/images/0722/extension.png)
5. Extension에 Date를 String으로 바꿔주는 변수를 만들어준다. 나는 **월,일,요일,오전오후,시간**순으로 나열하고 싶어서 formatter로 설정해줬다. 나타내고싶은대로 설정해주면 된다.

<br />
<br />



### 3. 완성!!  

![완성뷰](/assets/images/0722/done.png)
1. 다 됐다. 날짜를 선택하면 Label이 바뀐다.  
    
    
<br />

![완성뷰](/assets/images/0722/dateSet.png)
2. 다른 디테일한 설정은 여기서 할 수 있다. **Mode**에서 날짜/시간/날짜+시간/타이머를 선택할 수 있다. **Local**에서는 지역(언어)를 선택할 수 있고, **Interval**은 분 최소단위를 설정한다.  


<br />
<br />


### 4. 마무리  

생각보다 **더더** 간단했다. DatePicker는 Picker보다도 더 간단하게 사용 가능하다. 앞으로도 아주 유용하게 사용할 것 같다.  
더욱 디테일한 설명은 *[애플공식문서](https://developer.apple.com/documentation/uikit/uidatepicker)* 를 참고하면 더 좋다!


  
<br />
<br />


<br />


