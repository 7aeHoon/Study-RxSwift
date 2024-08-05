empty
======

> 빈 Observable을 생성하는 데 사용.  
> Observable은 어떠한 값도 방출하지 않고, 즉시 완료(onCompleted) 이벤트를 방출하는 특성.  
> empty는 비어 있는 데이터 흐름을 생성할 때 유용하며, 특정 상황에서의 종료 조건을 명확하게 표현.  

&nbsp;

![empty](https://github.com/user-attachments/assets/eda46cda-1d9a-4c46-8e8e-92526cb9c8ac)

&nbsp;

```swift

import UIKit
import RxSwift

let disposeBag = DisposeBag()

// - 출력결과 -
// completed
Observable<Void>.empty()
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
