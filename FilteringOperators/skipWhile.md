skip(while:)
============

> Observable 시퀀스의 요소들 중 조건이 true인 동안에는 요소를 건너뛰고,  
> 조건이 false로 평가되는 첫 번째 요소부터 이후의 모든 요소를 방출.  
> 즉, 주어진 조건이 참인 동안에는 계속해서 요소를 무시하고, 조건이 거짓이 되는 순간부터 모든 요소를 방출하기 시작.  

&nbsp;

![skipWhile](https://github.com/user-attachments/assets/c74f2ae9-fba3-491e-8b78-d6b2cc98f2f2)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

Observable.from(numbers)
    // 1은 짝수가 아니므로 true, 2는 짝수 따라서 false
    // 이때부터 모든 요소를 방출

    // - 출력결과 -
    // next(2)
    // next(3)
    // next(4)
    // next(5)
    // next(6)
    // next(7)
    // next(8)
    // next(9)
    // next(10)
    // completed
    .skip { !$0.isMultiple(of: 2) }
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
