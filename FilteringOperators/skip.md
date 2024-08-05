skip
====

> Observable 시퀀스의 처음부터 일정 개수의 요소를 무시하고, 그 이후의 요소들만 방출.  
> 초기의 몇몇 요소를 건너뛰고 나머지 요소들에 대해서만 작업을 수행할 때 유용.  

&nbsp;

![skip](https://github.com/user-attachments/assets/8a5098b1-b82d-4696-8daf-56ef88c7689b)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// - 출력결과 -
//next(4)
//next(5)
//next(6)
//next(7)
//next(8)
//next(9)
//next(10)
//completed
Observable.from(numbers)
    .skip(3) // 인덱스가 아니라 수만큼 생략, 즉 3개 생략
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
