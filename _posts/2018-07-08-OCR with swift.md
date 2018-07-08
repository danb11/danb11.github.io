---
layout: post
title: "OCR Tesseract in iOS"
date: 2018-07-08
categories:
  - iOS
description: Tesseract를 이용해 OCR기능을 추가합시. 
image: /assets/images/0708/main.png
image-sm: /assets/images/0708/main_th.png
---


## OCR? Tesseract!
---


<br/>
<br/>
### OCR이 뭐야...

단도직입적으로 **Optical character recognition** (also optical character reader, OCR) 은 단어 그대로 이미지로된 문자(인쇄 혹은 손글씨)를 Text로 바꿔주는 장치라고 한다. 개인적으로 OCR이라는 단어는 처음 들어봤다. 지금 회사에서 개발하고 있는 애플리케이션에 추가되어야하는 기능이라 라이브러리를 이용해 추가하게되었다.  
안드로이드에서 먼저 **Tesseract**로 ocr을 구현해 iOS에서도 같은 라이브러리를 사용했다. Ray Wenderlich 글을 많이 참고해 거의 번역글이라도 봐도 무관하나 조금 더 간단히 이미지 -> 문자로 바뀌는 코드만 설명해볼까 한다.  
  
*참고: Ray Wenderlich [Tesseract OCR Tutorial for iOS](https://www.raywenderlich.com/163445/tesseract-ocr-tutorial-ios)* 

  
<br />
<br />


### 1. Tesseract 설치      
  

나는 cocoapods을 이용해서 Tesseract를 설치해줬다. 일단 나는 cocoapods를 사용해서 cocoapods로 설치하는 방법으로 설명하려고한다. ~~(초간단합니다!)~~ 다른방법은 *[Tesseract GitHub](https://github.com/gali8/Tesseract-OCR-iOS)* 에서 참고하면 좋을 것 같다.  

<div style="background-color: #EDEDED">
{% highlight swift %}  

    sudo gem install cocoapods
            
{% endhighlight %}
</div>  
1. cocoapods이 설치되어있지않다면, 터미널을 켜고 프로젝트가 있는 디렉토리에서 cocoapods을 설치해준다.  

<div style="background-color: #EDEDED">
{% highlight swift %}  

    pod init
            
{% endhighlight %}
</div>  
2. 초기화를 한번 해준다  

<div style="background-color: #EDEDED">
{% highlight swift %}  

    use_frameworks!
    platform :ios, '11.0'
    
    target '본인앱이름' do
      use_frameworks!
      pod 'TesseractOCRiOS', '4.0.0'
    end
            
{% endhighlight %}
</div>  
3. PodFile에 들어가서 Tesseract를 추가해준다.  

<div style="background-color: #EDEDED">
{% highlight swift %}  

    pod install
            
{% endhighlight %}
</div>  
4. pod 설치완료! pod을 처음 설치했다면 원래 파란아이콘의 .xcodeproj이 아니라 흰색 아이콘의 .xcworkspace 파일이 생겼을거다. 앞으로는 이 파일에 작업을 해야한다!  

이제 xcode에서 몇 가지 설정만 해주면 된다.(1번, 2번 사진은 Ray Wenderlich의 이미지를 사용했다.)  

![xcode 세팅1](/assets/images/0708/bitcode1.png)
1. Build Settings에서 enable bitcode를 **No**로 설정해준다.  
<br />
![xcode 세팅2](/assets/images/0708/bitcode2.png)
2. pods의 TesseractOCRiOS 타겟의 Build Settings도 마찬가지로 enable bitcode를 **No**로 바꿔준다.  
<br />
![xcode 세팅3](/assets/images/0708/setting.png)
3-1. Link Binary With Libraries에 이미지와 같이 **libstdc++.dylib, CoreImage.framework, and TesseractOCR.framework**를 +버튼을 눌러서 추가해준다.
3-2. Copy Bundle Resources에 tessdata 언어팩을 다운받아 추가해준다. 나는 *[다운](https://github.com/tesseract-ocr/tessdata)* 받아서 넣어줬다.(꼭x1000000 폴더로 넣어줘야한다.) ~~바보같이 파일로 넣어서 끙끙거린 1인~~  
<br />
이제 세팅은 정말 끝! 코드로 넘어가보자 :)

  
<br />
<br />
  
  
### 2. Tesseract 코드를 써보자.  
  
Tesseract 코드를 먼저 써보자.  

<div style="background-color: #EDEDED">
{% highlight swift %}  

    import TesseractOCR
            
{% endhighlight %}
</div>  
    
1. 먼저! Tesseract를 import해주자.  

![xcode 세팅2](/assets/images/0708/reco.png)
2. image를 파라미터로 받는 performImageRecognition함수를 만들어준다.  
천천히 코드를 보면, G8Tesseract 객체를 초기화한다.(eng는 영어이고 다른 언어가 필요하다면 tessdata를 추가해서 사용하면된다.)  
engine mode는 3가지가 있는데 .tesseractOnly는 가장 빠르지만 정확도가 낮다. .cubeOnly는 더 많은 인공지능을 사용하기 때문에 속도가 조금 느리다. 마지막으로 .tesseractCubeCombined는 앞의 엔진 두 개 다 이용하기때문에 가장 느리지만 가장 정확하다. 용도에 맞게 사용하면 된다.  
단락인식은 자동으로 해줬고, 텍스트를 잘 인식해주기위해 흑백으로 바꿔주는 코드를 넣어줬다. 그리고 이제 **recognize()**해주면 문자인식!  
textView에 이미지에서 인식한 text를 넣어줬다. Tesseract에 관련된 코드는 이게 다이다. 생각보다 쉽다!


<br />
<br />

### 3. 카메라를 켜줍니다.  
  
내가 이번에 하려고 한건 OCR을 사용하는 법이기 때문에 imagePicker에 관한 내용은 Ray Wenderlich 코드를 참고하면 좋을 것 같다.  
![xcode 세팅2](/assets/images/0708/imagepicker.png)
간단히 설명하자면, 엑션시트에 카메라켜는버튼 & 사진첩여는버튼 을 만들어주고, 사진첩 이미지를 선택하거나 카메라를 켜서 사진을 선택하면 **performImageRecognition(:_)**를 불러 문자로 바꿔주는 코드이다. 
<br />
<br />

### 4. 마무리  

OCR이라는 생소한 기능이지만, 하나하나 혼자 할 수 있는 것이 늘어가는 뿌듯함이 생각보다 크다!  
전에 2가지 글은 라이브러리를 사용하지 않는 글이었지만 갑자기 라이브러리로 큰 뿌듯함을 느껴 되게 이중적인 것 같다 읔... 하지만 OCR이라는 엄청난 기능을 내가 이해하고 사용한 경험이 재미있고 신기했다! 시간이될 때 오픈소스를 뜯어봐야겠다.😂 ~~또 하나의 부채 추가요~ 흡... 언젠가느으으으은~~ 


  
<br />
<br />


<br />


