interval
========

> 일정한 시간 간격으로 정수 값을 방출하는 Observable을 생성.  
> 주로 타이머 기능을 구현하거나 주기적으로 반복되는 작업을 수행할 때 유용.  
> 방출되는 정수 값은 0부터 시작하여 시간이 지남에 따라 1씩 증가.  
> 각각의 interval은 자신만의 타이머를 사용.

```swift
import UIKit
import RxSwift

let interval = Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)
    
let subscription1 = interval.subscribe { print("1 >> ", $0) }

// 5초 후 dispose
DispatchQueue.main.asyncAfter(deadline: .now() + 5) {
    subscription1.dispose()
}

var subscription2: Disposable?

DispatchQueue.main.asyncAfter(deadline: .now() + 2) {
    subscription2 = interval.subscribe { print("2 >> ", $0) }
}

// 7초 후 dispose
DispatchQueue.main.asyncAfter(deadline: .now() + 7) {
    subscription2?.dispose()
}
```
