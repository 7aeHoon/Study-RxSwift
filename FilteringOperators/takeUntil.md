take(until:)
===========

> 특정 Observable이 방출될 때까지의 요소만을 방출하고, 그 Observable이 방출되면 시퀀스를 종료.  
> 특정 조건이 충족될 때까지의 요소를 포함하고, 그 조건이 충족된 이후의 모든 요소는 무시.  

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

let subject = PublishSubject<Int>()
let trigger = PublishSubject<Int>()

subject.take(until: trigger)
    .subscribe { print($0) }
    .disposed(by: disposeBag)


// - 출력결과 -
// next(1)
// next(2)
// completed
subject.onNext(1)
subject.onNext(2)

trigger.onNext(0)
// 무시
subject.onNext(3)
```
