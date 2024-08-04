catchAndReturn
==============

> 오류가 발생했을 때 오류를 처리하거나 대체 시퀀스를 제공하는 데 사용.  
> 오류가 발생했을 때 특정 값을 반환.  
> 단순히 하나의 값을 반환하고 시퀀스를 종료.  

```swift
import UIKit
import RxSwift

let bag = DisposeBag()

enum MyError: Error {
    case error
}

let subject = PublishSubject<Int>()

subject
    // Observable이 아닌 단일 값 방출
    // subject가 방출하는 타입과 같은 타입을 리턴
    .catchAndReturn(-1)
    .subscribe { print($0) }
    .disposed(by: bag)

// - 출력 결과 -
// next(-1)
// completed
subject.onError(MyError.error)
```
