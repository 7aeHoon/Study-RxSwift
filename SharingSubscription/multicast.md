multicast
=========

> 하나의 Observable을 여러 구독자와 공유할 수 있도록 만들어주는 연산자.  
> ConnectableObservable을 반환하며, 이를 통해 여러 구독자가 동일한 Observable 시퀀스를 공유할 수 있음.  
> Observable이 방출한 이벤트를 subject에 저장. 이후 subject가 connect될 때 이벤트 방출.  
> connect 메서드를 호출할 때까지 이벤트를 방출하지 않음.  

&nbsp;

![multicast](https://github.com/user-attachments/assets/b745ff62-9ce2-4033-aea3-35d7b53d5463)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()
let subject = PublishSubject<Int>()

let source = Observable<Int>
    .interval(.seconds(1), scheduler: MainScheduler.instance)
    .take(5)
    // subject로 이벤트 전달, connect되면 구독자에게 이벤트 전달
    .multicast(subject)

// 일반적으로 각 구독자는 개별 시퀀스로 작동(unicast)

// - 출력 결과 -
// 🔵 next(0)
// 🔵 next(1)
// 🔵 next(2)
// 🔴 next(2)
// 🔵 next(3)
// 🔴 next(3)
// 🔵 next(4)
// 🔴 next(4)
// 🔵 completed
// 🔴 completed

source
    .subscribe { print("🔵", $0) }
    .disposed(by: disposeBag)

// 구독이 3초 지연되었기 때문에 처음 이벤트 0, 1은 받지 못함
source
    .delaySubscription(.seconds(3), scheduler: MainScheduler.instance)
    .subscribe { print("🔴", $0) }
    .disposed(by: disposeBag)

// connect를 호출한 후 구독자에게 이벤트 방출
source
    .connect()
    .disposed(by: disposeBag)
```
