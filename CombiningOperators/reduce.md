reduce
======

> 여러 값을 결합하여 단일 결과를 생성하는 연산자.  
> 주어진 초기 값과 함께 각 요소를 순차적으로 결합하여 최종 값을 생성.

&nbsp;

![reduce](https://github.com/user-attachments/assets/3f2d1ed7-7aa2-434c-af42-bc51b2573c2d)

&nbsp;

```swift
import UIKit
import RxSwift

let bag = DisposeBag()

enum MyError: Error {
    case error
}

let o = Observable.range(start: 1, count: 5)

print("== scan")

// 중간 결과와 최종 결과

// - 출력결과 -
// next(1)
// next(3)
// next(6)
// next(10)
// next(15)
// completed
o.scan(0, accumulator: +)
    .subscribe { print($0) }
    .disposed(by: bag)

print("== reduce")

// 최종 결과만

// - 출력결과 -
// next(15)
// completed
o.reduce(0, accumulator: +)
    .subscribe { print($0) }
    .disposed(by: bag)
```
