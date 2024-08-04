of
=====

> 여러 개의 값을 방출하는 Observable을 생성하는 데 사용.  
> 단일 값이 아닌 여러 개의 값을 방출하고자 할 때 유용.  

```swift

import UIKit
import RxSwift

let disposeBag = DisposeBag()
let apple = "🍏"
let orange = "🍊"
let kiwi = "🥝"

// 2개 이상의 이벤트를 방출.

// - 출력결과 -
// next(🍏)
// next(🍊)
// next(🥝)
// completed
Observable.of(apple, orange, kiwi)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// just와 같이 그대로 방출.

// - 출력결과 -
// next([1, 2])
// next([3, 4])
// next([5, 6])
// completed
Observable.of([1, 2], [3, 4], [5, 6])
    .subscribe { print($0) }
    .disposed(by: disposeBag)

```
