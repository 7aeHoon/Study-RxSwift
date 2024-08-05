skip(until:)
============

> Observable 시퀀스에서 특정 Observable이 방출될 때까지의 요소를 무시하고, 그 이후의 요소만을 방출.  
> Observable의 요소가 지정된 Observable이 방출될 때까지 무시되며, 이후부터 방출.  
> 이벤트 스트림의 시점을 조정하거나 특정 조건이 충족될 때까지 데이터를 무시할 때 유용.  

&nbsp;

![skipUntil](https://github.com/user-attachments/assets/cfa36790-0774-4daf-ab00-b49766b7ed0e)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

let subject = PublishSubject<Int>()
let trigger = PublishSubject<Int>()

// trigger가 방출된 이후부터 이벤트 방출
subject.skip(until: trigger)
    .subscribe { print($0) }
    .disposed(by: disposeBag)


subject.onNext(1) // 무시

trigger.onNext(0) // 이때부터 방출

// - 출력결과 -
//next(3)
subject.onNext(3)
```
