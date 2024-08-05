sample
======

> 두 개의 Observable을 사용하여 동작.  
> 두 번째 Observable이 값을 방출할 때마다 첫 번째 Observable의 가장 최근 값을 방출.
> 전달하는 이벤트가 이전의 이벤트와 같다면 이벤트를 전달하지 않는다.  
> withLatestFrom연산자와 호출하는 시퀀스가 반대.  

&nbsp;

![sample](https://github.com/user-attachments/assets/afd1c852-0ea8-44ad-a221-7c8a804002fd)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

enum MyError: Error {
   case error
}

let trigger = PublishSubject<Void>()
let data = PublishSubject<String>()


data.sample(trigger)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// data Observable의 최근 이벤트가 없음
trigger.onNext(())

data.onNext("Hello")

// data Observable의 최근 이벤트 존재
// next(Hello)
trigger.onNext(())

// 기존 이벤트(Hello)가 같아서 이벤트 방출 없음
trigger.onNext(())

data.onNext("Swift")
// onCompleted도 가능
// next(Swift)
trigger.onCompleted()

// data Observable이 onCompleted를 방출하면 trigger Observable도 onCompleted를 방출
// completed
data.onCompleted()
trigger.onNext(())

// data Observable이 onError를 방출하면 trigger Observable도 onError를 방출
// error(error)
data.onError(MyError.error)
```
