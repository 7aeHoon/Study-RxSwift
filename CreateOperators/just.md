just
=====

> 단일 값을 방출하고 완료하는 Observable 시퀀스를 생성하는 데 사용.  
> 특히 단일 이벤트를 처리할 때 간단하고 유용하게 사용.  

```swift

import UIKit
import RxSwift

let disposeBag = DisposeBag()
let element = "😀"

// 하나의 항목을 방출하는 Observable생성.

// - 출력결과 -
// next(😀)
// completed
Observable.just(element)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// 전달된 값을 그대로 방출

// - 출력결과 -
// next([1, 2, 3])
// completed
Observable.just([1, 2, 3])
    .subscribe { print($0) }
    .disposed(by: disposeBag)

```
