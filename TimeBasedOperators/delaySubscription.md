delaySubscription
=================

> Observable의 구독 시점을 지연시키는 데 사용.  
> 즉, Observable이 실제로 이벤트를 방출하기 시작하기 전에 구독을 지연.  
> 구독이 시작된 이후부터는 이벤트가 방출됩니다. 지연 기간 동안 방출된 이벤트는 무시.  

&nbsp;

<img width="721" alt="delaySubscription" src="https://github.com/user-attachments/assets/89506371-c357-4c0d-826d-e1e15fb167e5">

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

// delay와 delaySubscription의 차이

// Observable이 이벤트를 방출하고 있지만, 해당 이벤트를 받는 시점이 delay됨
Observable<Int>
    .interval(.seconds(1), scheduler: MainScheduler.instance)
    .take(10)
    .delay(.seconds(5), scheduler: MainScheduler.instance)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// Observable의 구독 시점을 delay하고, 이후 구독과 동시에 이벤트 방출됨
Observable<Int>
    .interval(.seconds(1), scheduler: MainScheduler.instance)
    .take(10)
    .delaySubscription(.seconds(5), scheduler: MainScheduler.instance)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
