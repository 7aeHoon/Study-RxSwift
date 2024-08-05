BehaviorSubject
===============

> 구독자에게 현재 상태 또는 최신 상태를 전달하기 위해 사용.  
> PublishSubject와 달리 초기값을 가질 수 있으며, 새로운 구독자에게 항상 최신값을 전달.  

&nbsp;

![behavior](https://github.com/user-attachments/assets/f45c9582-1ee7-43d3-804a-96539e561f60)

&nbsp;

```swift

import UIKit
import RxSwift

let disposeBag = DisposeBag()

enum MyError: Error {
   case error
}

// value 0이 저장된 상태로 존재.
// next이벤트로 0값 전달.
let behavior = BehaviorSubject<Int>(value: 0)

// 구독자
// 초기값 이벤트0 전달.
behavior.subscribe { print("Behavior1", $0) }
    .disposed(by: disposeBag)

// next 이벤트1 전달
// 마지막으로 발행된 값 1을 저장.
behavior.onNext(1)

// 구독자
// 마지막으로 발행된 값 1을 next이벤트를 통해 전달받음.
behavior.subscribe { print("Behavior2", $0) }
    .disposed(by: disposeBag)

// 저장된 값이 있더라도 completed와 error가 호출된 상태면 저장된 값을 전달하지않는다.
behavior.onCompleted()
behavior.onError(MyError.error)

// 이벤트를 전달받지 않음.
behavior.subscribe { print("Behavior3", $0) }
    .disposed(by: disposeBag)
```
