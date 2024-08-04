scan
====

> 시퀀스에서 발생하는 각 요소를 누적하여 중간 결과를 방출하는 역할.  
> scan은 기본적으로 fold 또는 reduce 연산과 유사하게 작동하지만, 중요한 차이점은 누적된 결과를 중간 단계마다 방출한다는 것.  

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

// - 출력결과 -
// next(1)
// next(3)
// next(6)
// next(10)
// next(15)
// completed 
Observable.range(start: 1, count: 5)
    .scan(0, accumulator: +)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```