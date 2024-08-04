from
=====

> 배열과 같은 시퀀스(Sequence) 형태의 데이터를 받아서 각 요소를 순차적으로 방출하는 Observable을 생성하는 데 사용.  
> 여러 값을 담고 있는 컬렉션의 각 요소를 개별적으로 방출하고 완료.  

```swift

import UIKit
import RxSwift

let disposeBag = DisposeBag()
let fruits = ["🍏", "🍎", "🍋", "🍓", "🍇"]

// - 출력결과 -
// next(🍏)
// next(🍎)
// next(🍋)
// next(🍓)
// next(🍇)
// completed
Observable.from(fruits)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// of와 차이점은 "이벤트를 받은 그대로 전달하는가?"

// - 출력결과 -
// next([1, 2, 3, 4, 5, 6])
// completed
Observable.of([1, 2, 3, 4, 5, 6])
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// - 출력결과 -
// next(1)
// next(2)
// next(3)
// next(4)
// next(5)
// next(6)
// completed
Observable.from([1, 2, 3, 4, 5, 6])
    .subscribe { print($0) }
    .disposed(by: disposeBag)

```
