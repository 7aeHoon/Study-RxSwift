take(while:)
=========

> 특정 조건이 충족되는 동안만 요소를 방출하고, 조건이 충족되지 않으면 이후의 모든 요소를 무시하고 시퀀스를 종료.  
> 조건이 참인 동안 요소를 계속 방출하고, 조건이 거짓이 되면 방출을 중지하는데 유용.  

&nbsp;

![takeWhile](https://github.com/user-attachments/assets/cf82f3f6-6c1b-4d14-a4a8-78c5e0b89c80)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// - 출력결과 -
// next(1)
// completed
Observable.from(numbers)
    // 홀수만 방출, 2에서 false이므로 이후 이벤트 방출하지 않음
    .take(while: { !$0.isMultiple(of: 2) })
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
