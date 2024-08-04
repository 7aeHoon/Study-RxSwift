publish
=======

> multicast 연산자의 편의 버전.  
> 기본적으로 PublishSubject를 사용하여 Observable을 ConnectableObservable로 변환.  
> 여러 구독자가 동일한 이벤트 시퀀스를 공유할 수 있음.  
> connect 메서드를 호출하여 이벤트 방출을 시작.  

```swift
import UIKit
import RxSwift

let bag = DisposeBag()
let subject = PublishSubject<Int>()
let source = Observable<Int>
    .interval(.seconds(1), scheduler: MainScheduler.instance)
    .take(5)
    // 별도로 전달할 인자가 없음
    // PublishSubject을 사용할 경우, 간단하게 publish() 연산자를 사용
    .publish()

source
    .subscribe { print("🔵", $0) }
    .disposed(by: bag)

source
    .delaySubscription(.seconds(3), scheduler: MainScheduler.instance)
    .subscribe { print("🔴", $0) }
    .disposed(by: bag)

// connect 반드시 필요
source.connect()
```
