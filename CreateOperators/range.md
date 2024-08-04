range
=====

> 지정된 범위의 정수 값을 순차적으로 방출하는 Observable을 생성하는 데 사용.  
> 시작 값부터 끝 값까지의 정수 시퀀스를 생성하여, 각 값을 차례로 방출하고 완료.  

```swift

import UIKit
import RxSwift

let disposeBag = DisposeBag()

// 반드시 정수만
// 무조건 시작 값에서 1씩 증가하는 이벤트 생성
// start: 시작 값, count: 지정된 수

// - 출력결과 -
// next(1)
// next(2)
// next(3)
// next(4)
// next(5)
// completed
Observable.range(start: 1, count: 5)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

```
