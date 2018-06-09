---
layout: post
title: "iOS Decodable"
date: 2018-06-09
categories:
  - iOS
description: Decodable에 대해 알아봅시다. 
image: /assets/images/0605/main.png
image-sm: /assets/images/0605/main_th.png
---


## Decodable로 데이터를 Parsing 해봅시다.
---


<br/>
<br/>
### 핵데이 그리고 라이브러리는 안됩니다!

저번달 네이버 핵데이 iOS에 운좋게 참가했었다. 내가 선택한 주제에서 멘토님께서 조금 더 제약을 추가하셨다. 그 중에 내가 가장 당황했던 것은 **1.오픈소스를 사용하지 않는다. 2.스토리보드를 사용하지 않는다.** 였다. 그래서 이 두 가지를 빠르게 깊이 공부해야 했다.  
swift에서 **Alimofire**라는 라이브러리는 iOS개발자들 사이에서 아주 흔하게 사용되어왔다.~~(물론 아닌 개발자분도 있을 것이다.)~~ 나 또한 api통신하고 데이터를 가공할 때 **Alimofire, ObjectMapper, Swifty Json** 등을 사용해왔다. 그래서 라이브러리의 도움없이 네트워킹한 데이터를 가져와 가공하는 법을 공부했다.  
그.래.서 이번 글에서 라이브러리없이 네트워킹하고 모델링하는 법을 써볼까 한다.🤓

  
<br />
<br />


### 1. 데이터가 어떻게 생겼나~      


[샘플 더미 데이터](https://api.letsbuildthatapp.com/jsondecodable/courses)  
*※ 출처:[LBTA](https://www.letsbuildthatapp.com/)*  
  

![샘플 데이터 이미지](/assets/images/0605/dummy.png)
샘플 더미 데이터를 클릭하면 이런 화면이 나올 것이다.  
이걸 바탕으로 우리가 가져올 데이터가 어떤 타입인지 어떤 내용이 들어있는지 알 수 있다.  

  
![데이터 분석 이미지](/assets/images/0605/courseArray..jpeg)
샘플 데이터는 간단한 편이지만 복잡한 데이터가 들어 있는 경우가 많기때문에 나는 손으로 이렇게 정리하면 이해하기 쉬웠다.  
이 데이터는 id, name, link, imageUrl, number_of_lessons 라는 속성을 가지고있는 **Course**라는 객체가 4개가 들어있는 **Array**이다.

  
  
<br />
<br />
  
  
### 2. 모델을 만들자.  
  
이제 데이터를 넣어줄 모델을 코드로 만들어보자.

<span style="background-color: #EDEDED">
{% highlight swift %}

struct Course : Decodable {
    var id: Int = 0
    var name: String = ""
    var link: String = ""
    var imageUrl: String?
    var numberOfLesson: Int = 0
    
    private enum CodingKeys : String, CodingKey {
        case id = "id"
        case name = "name"
        case link = "link"
        case imageUrl = "imageUrl"
        case numberOfLesson = "number_of_lessons"
    }
}
{% endhighlight %}
</span>

여기서 **Decodable**이 뭘까 궁금증이 생길 것이다.  
이번에 새로 공부하며 Codable이란 것을 알게되었다. Codable에는 Decodable과 Encodable이 있는데 말 그대로 데이터를 네트워크에서 가져올 때 내가 원하는 타입으로 변환해 가져오고, 보낼 수 있는 것이다.  
나는 서버에 보낼 일은 없고 가져와서 사용할 예정이라 Decodable을 사용했다. 코드처럼 struct에 Decodable 프로토콜을 추가해주면 된다.  
*[Codable 애플 문서](https://www.letsbuildthatapp.com/)*  
  
**CodingKeys**는 서버에서 사용하는 변수명과 내가 사용할 변수명이 다를 경우 같게써줘 코딩키를 맞추는 작업이다.



<br />
<br />

### 3. 데이터를 가져오자.  
 
  
이제 우리가 만들어준 Course 구조에 서버의 데이터 내용을 넣어보자!  
  
<span style="background-color: #EDEDED">
{% highlight swift %}

class ApiCenter {
    
    static func getCourses(completion: @escaping ([Course]) -> Void) {
        
        let jsonUrlString = "https://api.letsbuildthatapp.com/jsondecodable/courses"
        
        guard let url = URL(string: jsonUrlString) else { return }
        
        URLSession.shared.dataTask(with: url) { (data, response, error) in
            DispatchQueue.main.async {
                if let err = error {
                    print("errrrorrrrr", err)
                    return
                }
                
                guard let data = data else { return }
                
                do {
                    let decoder = JSONDecoder()
                    let course = try decoder.decode([Course].self, from: data)
                    completion(course)
                }
                    
                catch let jsonErr {
                    print("Error serializing json:", jsonErr)
                }
            }
        }.resume()
    }
}
{% endhighlight %}
</span>
  
앞에서 체크했듯이 우리가 가져올 데이터는 **Course가 4개 들어있는 Array**이기 떄문에 반환받는 데이터의 타입은 **[Course]**가 된다.
swift4이후로 Decodable을 사용하면 URLSession으로 데이터를 가공해주는 코드가 확연히 줄어들었다.  

<br />


### 4. 데이터가 잘 들어왔나~
  
두구두구-!  
swift는 typeCasting에 아주 엄격한 언어이기 때문에 하나라도 다른 타입이 들어오면 아예 디코딩을 못한다.  
자, 확인해봅시다.  

<span style="background-color: #EDEDED">
{% highlight swift %}
    var courses: [Course]?

    override func viewDidLoad() {
        super.viewDidLoad()
        
        getCourseData()

    }
    
    func getCourseData() {
        
        ApiCenter.getCourses { (courseData) in
            self.courses = courseData
            dump(self.courses)
            
        }
        
    }
{% endhighlight %}
</span>  
  
데이터를 가져오는 함수 getCourseData를 viewDidLoad에 넣어줘 뷰가 로드되면서 가져올 수 있도록 했다. **dump**는 print와 달리 객체의 값까지 다 하단 디버그창에서 확인할 수 있다.
  
  
![샘플 데이터 이미지](/assets/images/0605/dump.png)  
데이터가 아름답게 잘 들어왔뜨아! Decodable을 공부하면서 너무 많은 에러메세지를 보아서 이렇게 데이터가 나오면 세상 그렇게 기쁠 수가 없다.🤩  
  
    
  
### 5. 타입이 틀렸다면..?  
  
잘 들어온걸 확인했지만, 개인적으로 공부하면서 type이 mismatch되었다는 에러를 너무 많이보았습니다. 저의 불찰이지만 다시 한번 짚고 넘어가보자  
  
<span style="background-color: #EDEDED">
{% highlight swift %}
struct Course : Decodable {
    var id: String = ""
    var name: String = ""
    var link: String = ""
    var imageUrl: String?
    var numberOfLesson: Int = 0
    
    private enum CodingKeys : String, CodingKey {
        case id = "id"
        case name = "name"
        case link = "link"
        case imageUrl = "imageUrl"
        case numberOfLesson = "number_of_lessons"
    }
}
{% endhighlight %}
</span>  
  
Course의 id를 Int에서 String으로 바꿔보았다.  

  
  <span style="background-color: #EDEDED">
  {% highlight swift %}
Error serializing json: typeMismatch(Swift.String, Swift.DecodingError.Context(codingPath: [_JSONKey(stringValue: "Index 0", intValue: 0), CodingKeys(stringValue: "id", intValue: nil)], debugDescription: "Expected to decode String but found a number instead.", underlyingError: nil))

  {% endhighlight %}
  </span>  

감.동. 정확히 **typeMismatch**로 "id"의 IntValue가 nil이라고 알려준다. 역시 swift는 스릉흔드...💕  
  
  
    
### 5. 라이브러리의 달콤함
  
물론 좋은 라이브러리를 사용하면 코드가 확실히 줄어드는 경우도 있고, 복잡한 애니메이션을 간단하게 가져와 쓸 수 있다. 정-말 무공무진하고 어메이징한 라이브러리들을 잘 사용하면 앱의 퀄리티가 높아지기도 한다. 하지만 핵데이때 만난 멘토님은 이제 시작하는 초보개발자가 라이브러리의 편리함만 찾는 것은 좋지 않은 습관이라는 생각해 라이브러리 사용을 제한했다고 한다.    
나도 라이브러리를 지향하는 것은 아니지만 막연히 이 경우는 이 라이브러리를 사용하는게 습관되어있던 것 같다. 라이브러리를 사용하지 않고 swift로도 충분히 할 수 있는 코드를 짤 수 있도록 더 노력해야겠다.~~(내가 오픈소스를 만드는 날도 곧 오기를..!!)~~  
  
  
다양한 피드백은 언제든 환영합니다! 🤟🏻
  
    


<br />


