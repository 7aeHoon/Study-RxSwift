take
=========

> Observable 시퀀스에서 지정된 개수만큼의 요소만을 방출하고, 그 이후의 요소는 무시하며 시퀀스를 종료.  
> 데이터 스트림에서 처음 N개의 요소만 필요할 때 유용하게 사용.  

&nbsp;

![take](https://github.com/user-attachments/assets/b30a021f-9635-4f30-8a77-9a6466fc3e7c)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// - 출력결과 -
// next(1)
// next(2)
// next(3)
// next(4)
// next(5)
// completed

Observable.from(numbers)
    .take(5) // 5개의 이벤트만
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
