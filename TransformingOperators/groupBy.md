groupBy
=======

> Observable 시퀀스의 요소를 주어진 기준에 따라 그룹화하여 각각의 그룹을 별도의 Observable로 변환.  

&nbsp;

![groupBy](https://github.com/user-attachments/assets/f984eb32-ee45-4ab6-b02d-af583857502a)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()
let words = ["Apple", "Banana", "Orange", "Book", "City", "Axe"]

// - 출력 결과 -
// next(["Banana", "Orange"])
// next(["Book", "City"])
// next(["Axe"])
// next(["Apple"])
// completed
Observable<String>.from(words)
    // 반환 타입은 GroupedObservable
    .groupBy { $0.count }
    // GroupedObservable을 하나의 Observable로
    .flatMap { $0.toArray() }
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
