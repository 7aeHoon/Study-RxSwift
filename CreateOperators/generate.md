generate
========

> 사용자가 정의한 로직에 따라 반복적으로 값을 생성하는 Observable을 만드는 데 사용.  
> 초기값과 종료 조건을 설정하여, 값을 동적으로 생성하고 방출.  

```swift

import UIKit
import RxSwift

let disposeBag = DisposeBag()
let red = "🔴"
let blue = "🔵"

// true 조건만 방출하고, false일 경우 바로 onCompleted 호출
// initialState: 초기 값, condition: 값을 생성할 조건 정의 클로저, iterate: 다음 상태 계산 클로저

// - 출력결과 -
// next(0)
// next(2)
// next(4)
// next(6)
// next(8)
// next(10)
// completed
Observable.generate(initialState: 0) { $0 <= 10 } iterate: { $0 + 2 }
    .subscribe { print($0) }
    .disposed(by: disposeBag)


// - 출력결과 -
// next(🔴)
// next(🔴🔵)
// next(🔴🔵🔴)
// next(🔴🔵🔴🔵)
// completed
Observable.generate(initialState: red) { $0.count < 5 } iterate: { $0.count.isMultiple(of: 2) ? ($0 + red) : ($0 + blue) }
    .subscribe { print($0) }
    .disposed(by: disposeBag)

```
