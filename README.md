# 테이블 뷰 컨트롤러를 이용해 할 일 목록 만들기 (12장)
테이블 뷰 컨트롤러(Table View Controller)는 사용자에게 목록 형태의 정보를 제공해 줄 뿐만 아니라 '목록'의 특정 항목을 선택하여 세부 사항을 표시할 때 유용하며 테이블 뷰 컨트롤러를 사용한 대표적인 앱으로는 알람, 메일, 연락처 등이 있습니다.

목록 기능은 항목을 추가할 수 있는 추가 버튼 기능뿐만 아니라 항목을 삭제하거나 이동할 수 있는 기능, 항목을 선택하면 내용을 볼 수 있는 기능을 구현합니다. 

---

## 완성된 모습

  ![스크린샷 2022-06-09 오후 1 58 39](https://user-images.githubusercontent.com/106981296/173359479-45c7f48f-f02d-4ef1-873a-cdb1fd0bf02f.png)

---

## 주요 기능

* 목록 추가하기  

![스크린샷 2022-06-09 오후 1 59 22](https://user-images.githubusercontent.com/106981296/173361450-10958bb4-fe01-492f-a72b-7a53dc2d7e6a.png)   

![스크린샷 2022-06-09 오후 1 59 29](https://user-images.githubusercontent.com/106981296/173362390-2129b93e-3867-4628-b82f-1d829d972f8d.png)     



* 목록 삭제하기  
 
![스크린샷 2022-06-09 오후 1 59 36](https://user-images.githubusercontent.com/106981296/173362546-18df28da-e0bc-4124-b89b-7bf5e1d3d8ac.png)  

![스크린샷 2022-06-09 오후 1 59 03](https://user-images.githubusercontent.com/106981296/173362576-dc154c19-48ab-4de2-8d63-b404fd12f35a.png)  


* 목록 순서 바꾸기
---

## 전체 소스
* TableViewController.swift  
![스크린샷 2022-06-09 오후 2 00 29](https://user-images.githubusercontent.com/106981296/173361161-8b9c8f6e-cca3-4516-b1a0-646cf30dd4bc.png)  
![스크린샷 2022-06-09 오후 2 00 46](https://user-images.githubusercontent.com/106981296/173361181-1d4adfd9-acdb-463f-845e-196f4b8c4f16.png)  
![스크린샷 2022-06-09 오후 2 00 57](https://user-images.githubusercontent.com/106981296/173361202-eea6c216-1402-4af1-8ce4-3e80b793b4f3.png)  



* AddViewController.swift
![스크린샷 2022-06-09 오후 2 01 15](https://user-images.githubusercontent.com/106981296/173361044-56ce5c9b-0e3a-4190-b5ce-d66f8f388afd.png)  




* DetailViewController.swift
![스크린샷 2022-06-09 오후 2 01 47](https://user-images.githubusercontent.com/106981296/173360790-a2e0292d-e87b-460c-ab24-54b5e3933cf3.png)  


---

## 주요 코드 해석 

* TableViewController.swift

```SWIFT
//이미지 파일을 외부변수인 'items'와 'ItemsImageFile'로 선언해 모든 클래스에서 이미지를 사용할 수 있습니다.
var items = [ "책 구매", 철수와 약속", "스터디 준비하기"]   
var itemsImageFile = [ "cart.png", "clock.png", pencil.png] 
```

```SWIFT
//셀의 텍스트 레이블에 items을 대입하기( "책 구매", "철수와 약속", "스터디 준비하기" )
cell.textLable?.text = items[(indexPath as NSIndexPath).row]

//셀의 이미지 뷰에 itemsImageFile을 대입하기( "cart.png", "clock.png", "pencil.png")
cell.imageView?.image = UIImage(named: itemsImageFile[(indexPath as NSIndexPath).row])
```
```SWIFT
//선택한 셀을 삭제하기
items.remove(at: (indexPath as NSIndexPath).row)
itemsImageFile.remove(at: (indexPath as NSIndexPath).row)

//바 버튼으로 목록 삭제 동작 코딩하기( 오른쪽 화면에 [Add]버튼 왼쪽 화면에 [edit]버튼 ) 
self.navigationItem.rightBarButtonItem = self.editButtonItem
self.navigationItem.leftBarButtonItem = self.editButtonItem
```
```SWIFT
//이동할 아이템의 위치를 itemToMove에 저장하기
let itemToMove = items[(fromIndexPath as NSIndexPath).row]

//이동할 아이템의 이미지를 itemImageToMove에 저장하기
let itemImageToMove = itemsImageFile[(fromindexPath as NSIndexPath).row]

//삭제한 아이템 뒤의 아이템들의 인덱스 재정렬
items.remove(at: (fromindexPath as NSIndexPath).row)

//삭제된 아이템을 이동할 위치로 삽입. 삽입한 아이템 뒤의 아이템들의 인덱스가 재정렬
items.insert(itemToMove, at: (to as NSIndexPath).row)   
```


* AddViewController.swift

```SWIFT
//items에 텍스트 필드의 텍스트 값을 추가
items.append(tfAddItem.text!)

//itemsImageFile에는 무조건 'clock.png' 파일을 추가
itemsImageFile.append("clock.png")
```

~~~SWIFT
//텍스트 필드의 내용을 삭제한다.
tfAddItem.text=""

//루트 뷰 컨트롤러, 즉 테이블 뷰로 돌아갑니다.
_ = navigationcontroller?.popViewController(animated: true)

//추가된 내용을 목록으로 불러들입니다.
override func ViewWillAppear(_ animated: Bool) {
    tvListView.reloadData()
```  

* DetailViewController.swift
```SWIFT
//Main View에서 받을 텍스트를 위해 변수 receiveItem을 선언합니다.
var receiveItem = ""

//뷰가 노출될 때마다 이 내용을 레이블의 텍스트로 표시합니다.
lblItem.text = receiveItem

//Main View에서 변수를 받기 위한 함수를 추가합니다.
func receiveItem(_ item: String)
{
    receiveItem = item
}



---
출처: Do it! 스위프트로 아이폰 앱 만들기 입문
