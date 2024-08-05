filter
=========

> Observable 시퀀스에서 특정 조건을 만족하는 요소만 방출하도록 필터링.  
> 시퀀스 내에서 원하는 요소만 선택할 수 있어, 데이터 처리나 조건에 맞는 요소 추출.  

&nbsp;

![filter](https://github.com/user-attachments/assets/90c7a5b9-77f4-423b-96fe-beb13c57e1a5)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// - 출력결과 -
// next(2)
// next(4)
// next(6)
// next(8)
// next(10)
// completed
Observable.from(numbers)
    .filter { $0.isMultiple(of: 2) } // 짝수만 필터링
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
