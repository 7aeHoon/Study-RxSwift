take(for:scheduler:)
====================

> 지정된 시간 동안만 Observable에서 방출되는 요소들을 통과시키고 그 이후에는 Observable을 종료.  
> 특정 시간 동안만 데이터를 수집하거나 처리해야 할 때 유용.  

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

// 1초마다 next 이벤트 생성
let o = Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)

// 3초동안 next 이벤트를 받음
o.take(for: .seconds(3), scheduler: MainScheduler.instance)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
