---
layout: post
title: "컬렉션뷰 안에 컬렉션뷰넣기"
date: 2018-03-22
categories:
  - iOS
description: iOS 컬렉션뷰 안에 컬렉션뷰를 넣어보기
image: /assets/images/0322/main.png
image-sm: /assets/images/0322/main_thum.png
---


## 컬렉션뷰안에 컬렉션뷰를 넣어보았습니다.
---


<br />
<br />
### 컬렉션뷰 안에 또 컬렉션뷰?

2주에 한 번 글을 쓰는 일이 정말 쉽지 않음을 깨닫는 한 달이었다. 블로그의 주제는 2주동안 iOS 개발을 하면서 어려웠는데 해결한 문제에 대해 다루게될 것 같다. 그래서 이번 글 주제는 **컬렉션뷰 안에 컬렉션뷰 넣기**이다. (CollectionviewCell ISERT CollectionView)

![전/후 비교사진](/assets/images/0322/before.png)
사진과 같이 기존의 애플리케이션의 추가 기능이 생기면서 기존의 컬렉션뷰의 최상단에 슬라이딩광고(가로 스크롤뷰)가 추가되는 일이 발생했다. 스크롤뷰 안에 버튼도 들어갈 예정이라 그냥 iOS안의 스크롤뷰를 사용하기는 힘들었다. 테이블뷰안에 컬렉션뷰를 넣어본 경험이 있어서 그렇게할까도 생각했지만 이미 기존에 컬렉션뷰로 만들어져있는 상황이라 모든걸 바꾸기엔 위험이 컸다..  
그래서 컬렉션뷰셀 안에 가로로 넘어가는 컬렉션뷰를 추가하기로 결정했다. 처음 접하는 복잡한 구조의 컬렉션뷰라 어떻게 만들어야할지 감이 안왔다. 주변의 도움과 각종 글을 검색해 해결했다.  
아마 코드를 보면 '이렇게하면 되는구나' 쉬웠는데 막상 혼자하려니 접근이 쉽지않았다. 컬렉션뷰를 공부해봤다면 복잡한 컬렉션뷰를 만드는 이 예제를 따라해보면 컬렉션뷰를 조금 더 공부할 수 있을 것이다.


<br />
<br />


### 1. A컬렉션뷰 만들기      

편의상 가장 바깥의 컬렉션뷰를 **A컬렉션뷰** 안쪽에 가로슬라이딩이되는 컬렉션뷰를 **B컬렉션뷰**라고 부르겠다. 


![xcode스토리보드](/assets/images/0322/스토리보드.png)


1.스토리보드에서 기본적인 컬렉션뷰의 구성을 만들어준다.  
- A컬렉션뷰안에 item을 2개로 설정해준다.   
- 0번 셀에 B컬렉션뷰를 넣어준다. 그리고 각각 셀에 들어가야할 컨텐츠(label, image ...)를 넣어준다.     
- 각각 오토레이아웃을 잡아준다.    
- 컬렉션뷰A의 Delegate와 DataSource를 ViewController에 연결해준다.(스토리보드에서 컬렉션뷰A를 클릭한상태에서 Control을 누르고 viewcontroller로 드래그해주세요.)       
- **컬렉션뷰B는 delegate와 dataSource를 연결하지 않는다.** (뒤에서 코드로 해줄거에요!)

스토리보드에서 컬렉션뷰A와 컬렉션B를 구성하기 위한 여러가지(셀간격, 컬러, 컨텐츠모드 등)를 디자인에 맞게 설정해준다.

  
  
2.코드로 컬렉션뷰를 채워주자.   
- A컬렉션뷰를 만들어주기 위해 ViewController에 extension으로 collectionview를 넣어준다.  
- B컬렉션뷰는 무조건 0번 section이기 때문에 다른 셀과 section으로 구분해주면 된다.   
  
  

``
extension ViewController: UICollectionViewDataSource, UICollectionViewDelegate, UICollectionViewDelegateFlowLayout {
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        if section == 0 {
            return 1
        } else {
            return 5
        }
    }
    
    func numberOfSections(in collectionView: UICollectionView) -> Int {
        return 2
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        if indexPath.section == 0 {
            let slidingCell = collectionView.dequeueReusableCell(withReuseIdentifier: "SlidingCell", for: indexPath) as! SlidingCell
            return slidingCell
            
        } else {
            let normalCell = collectionView.dequeueReusableCell(withReuseIdentifier: "Cell", for: indexPath) as! NormalCell
            normalCell.normalLabel.text = "Normal Cell \(indexPath.row)"
            return normalCell
        }
    }
    
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        if indexPath.section == 0 {
            return CGSize(width: collectionView.frame.width, height: 160)
        } else {
            return CGSize(width: collectionView.frame.width, height: 100)
        }
    }
}

```  


여기까지 컬렉션 뷰를 그려주는 코드이다. 알아보기 편하게 text에 indexPath를 넣어주었다.
  
  
  
```
class NormalCell: UICollectionViewCell {
    
    @IBOutlet weak var normalLabel: UILabel!
    
}

class SlidingCell: UICollectionViewCell {
    
    
    @IBOutlet weak var slidingCollectionView: UICollectionView!
    
}


```

셀클래스까지 여기까지 어렵지 않게 따라할 수 있을거다.  
A컬렉션뷰에 NormalCell, SlidingCell이 들어있는 구조이다. 
아직까지 B컬렉션뷰를 만들어주지 않은 단계이다.   

<br />
<br />


### 2. B컬렉션뷰를 만들기


1.B컬렉션뷰는 A컬렉션뷰의 0번째 셀인 **SlidingCell** 클래스에 추가해준다.       


```
class SlidingCell: UICollectionViewCell {
    
    var imageArray = [#imageLiteral(resourceName: "1"),#imageLiteral(resourceName: "3"),#imageLiteral(resourceName: "2")]
    @IBOutlet weak var slidingCollectionView: UICollectionView!
    
    override func awakeFromNib() {
        slidingCollectionView.dataSource = self
    }
    
}

```  
기존에 Outlet만 만들어 놓은 SlidingCell에  B컬렉션뷰 dataSource를 Self로 추가해준다. B컬렉션뷰에 들어갈 이미지 어레이도 추가해주었다.   
    
    

````
extension SlidingCell: UICollectionViewDataSource {
    
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return imageArray.count
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let innerCell = collectionView.dequeueReusableCell(withReuseIdentifier: "InnerCell", for: indexPath) as! innerCell
        innerCell.innerImageView.image = imageArray[indexPath.item]
        innerCell.innerLabel.text = "inner Cell \(indexPath.item)"
        return innerCell
    }
    
}
````
SlidingCell클래스에 extension으로 UICollectionViewDataSource를 만들어준다.        
이미지는 imageArray에 미리 넣어둔 이미지, 그리고 텍스트는 알아보기 편하도록 셀이름과 idexPath를 넣어주었다.
     
     

````
class innerCell: UICollectionViewCell {
    
    @IBOutlet weak var innerImageView: UIImageView!
    @IBOutlet weak var innerLabel: UILabel!
    
}
````
가장 안쪽 innerCell이다 B컬렉션뷰 안에 있는 가로로 넘어갈 셀 내용이다. (이미지와, 라벨을 넣어주었다.)
      
      

<br />

### 3. 완성 그리고 ...


![완성Gif](/assets/images/0322/complate.gif)    
완성!    
아주 잘 돌아가는 모습을 볼 수 있다. 디테일하게 셀간 간격이나 컬러, 셀크기 등 다양한 요소들은 시뮬레이션을 돌려보면서 디테일하게 설정해주면 된다.(여기까지 잘 따라왔다면 충분히 할 수 있을거라 생각한다. 😜)     

  
<br />



<br />
단순히 컬렉션뷰 안에 컬렉션뷰 넣는게 뭐 어렵나 생각했는데, 셀클래스를 extexsion으로 datasource를 만든다는 자체를 생각하지 못했다.~~(아직많이부족한초보개발자😂)~~ 한번 만들어보니 어렵지 않다고 느꼈으나 아직 경험이 부족해서 스스로 다양한 시도를 많이 해봐야겠다고 느낀 업데이트였다.  
  
▶︎ 혹시 예제를 따라하다 어떤 문제가 발생하거나 잘 안된다면 편하게 밑에 **프로필**로 연락주세요!