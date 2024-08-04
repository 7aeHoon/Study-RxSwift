withLatestFrom
==============

> 두 개의 Observable을 결합하여 하나의 새로운 Observable을 생성.  
> 주로 트리거(Trigger) 역할을 하는 Observable이 값을 방출할 때, 두 번째 Observable에서 가장 최근에 방출된 값을 가져와서 함께 결합.  
> 두 번째 Observable이 가장 최근에 방출한 값을 기억하고 있다가, 첫 번째 Observable이 값을 방출할 때 해당 값을 사용.  
> 주의! 만약 결합된 시퀀스 중 하나에서 에러가 발생하면, 결합된 시퀀스 전체가 에러를 방출하고 종료.  

&nbsp;

![withLatestFrom](https://github.com/user-attachments/assets/8c845613-1786-4f46-ad9f-25a1e5381a49)

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

trigger.withLatestFrom(data)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// data Observable의 최근 이벤트가 없음
trigger.onNext(())

data.onNext("Hello")

// data Observable의 최근 이벤트와 결합
// next(Hello)
trigger.onNext(())

// next(Hello)
trigger.onNext(())

// 최신 이벤트 갱신
data.onNext("Swift")

// next(Swift)
trigger.onNext(())

// data Observable이 onCompleted되어도 trigger는 이벤트 방출
// next(Swift)
data.onCompleted()
trigger.onNext(())

// data Observable이 onError 방출하면, trigger Observable 또한 onError 방출
// error(error)
data.onError(MyError.error)
trigger.onNext(())
```
