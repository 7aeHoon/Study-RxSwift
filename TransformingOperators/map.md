map
===

> Observable이 방출하는 각 요소를 특정 함수에 적용하여 새로운 값을 생성하는 데 사용.  
> 각 요소에 대해 지정된 변환을 적용한 후 변환된 요소를 방출하는 새로운 Observable을 반환.  

&nbsp;

![map](https://github.com/user-attachments/assets/28493932-fe83-46c5-9604-e7e318cd83bc)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()
let skills = ["Swift", "SwiftUI", "RxSwift"]

// - 출력결과 -
// next(Hello, Swift)
// next(Hello, SwiftUI)
// next(Hello, RxSwift)
// completed
Observable.from(skills)
    .map { "Hello, \($0)" }
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
