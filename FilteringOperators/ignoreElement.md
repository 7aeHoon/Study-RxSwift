ignoreElements
============

> Observable 시퀀스가 방출하는 모든 next 이벤트를 무시하고, 오직 error 또는 completed 이벤트만 전달.  
> 값 자체보다는 이벤트 상태에 중점을 둬야 할 때 사용.  

&nbsp;

![ignoreElements](https://github.com/user-attachments/assets/4ca95475-4412-4efb-b713-d1ea53d08ee2)

&nbsp;

```swift

import UIKit
import RxSwift

let disposeBag = DisposeBag()
let fruits = ["🍏", "🍎", "🍋", "🍓", "🍇"]

// - 출력결과 -
// completed
Observable.from(fruits)
    .ignoreElements() // next 이벤트만 필터링
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
