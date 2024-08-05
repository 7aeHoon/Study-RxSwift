window
======

> window는 요소들을 그룹화하는 대신, 요소들이 포함된 Observable 시퀀스를 방출.  
> 각 Observable 시퀀스는 일정한 크기 또는 시간 간격으로 그룹화된 요소들을 포함.  
> 크기 기반 윈도잉, 시간 기반 윈도잉, 혼합 기반 윈도잉 존재.  

&nbsp;

![window](https://github.com/user-attachments/assets/5ce89d21-c202-4a96-9b3e-d99d591810b6)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)
    .window(timeSpan: .seconds(2), count: 3, scheduler: MainScheduler.instance)
    .take(5)
    .subscribe { print($0)
        //  innerObservable
        if let observable = $0.element {
            observable
                .subscribe { print("inner: ", $0) }
                .disposed(by: disposeBag)
        }
    }
    .disposed(by: disposeBag)
```
