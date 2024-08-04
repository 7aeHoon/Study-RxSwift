AsyncSubject
============

> 자신에게 발행된 이벤트 중 가장 마지막 이벤트 1개 전달.  
> 구독한 시점에 이벤트가 전달되는 것이 아닌 onCompleted 이벤트가 전달되면 마지막 이벤트 전달.  

```swift

import UIKit
import RxSwift

let bag = DisposeBag()

enum MyError: Error {
   case error
}

let subject = AsyncSubject<Int>()

subject.subscribe { print($0) }
    .disposed(by: bag)

subject.onNext(1)
subject.onNext(2)
subject.onNext(3)

// 이 시점에서 구독자에게 3 이벤트 전달
subject.onCompleted()

// 에러 이벤트를 전달했을 때는 에러 이벤트만 전달. onNext이벤트 미전달
//subject.onError(MyError.error)

```
