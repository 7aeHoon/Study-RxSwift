PublishSubject
============

> 특정 시점 이후에 발생한 이벤트만을 구독자들에게 전달하는 특성.  
> 초기화 시점 이후에 발생한 이벤트들만을 구독하고 싶은 경우에 유용하게 사용.  
> 초기값이 없고, 구독자가 구독한 이후에 발생하는 이벤트들만 전달.  

&nbsp;

![publish](https://github.com/user-attachments/assets/7e65f375-85ac-48e3-b488-7f39adedc39e)

&nbsp;

```swift

import UIKit
import RxSwift

let disposeBag = DisposeBag()

enum MyError: Error {
   case error
}

// 생성 방식에서 차이가 존재.
let publish = PublishSubject<Int>()

// 구독
publish.subscribe { print("Publish", $0) }
    .disposed(by: disposeBag)

```
