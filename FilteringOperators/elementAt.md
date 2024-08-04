elementAt
=========

> Observable 시퀀스의 특정 인덱스에 위치한 요소를 방출하는 연산자.  
> 주어진 인덱스 위치의 요소만 방출하고, 그 외의 요소들은 무시.  

```swift

import UIKit
import RxSwift

let disposeBag = DisposeBag()
let fruits = ["🍏", "🍎", "🍋", "🍓", "🍇"]

// - 출력결과 -
// next(🍎)
// completed
Observable.from(fruits)
    .element(at: 1) // 1번 인덱스 위치의 요소를 반환
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
