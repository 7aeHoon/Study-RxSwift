delay
=====

> Observable의 이벤트를 지정된 시간만큼 지연시킨 후 방출하는 데 사용.  
> Observable의 모든 이벤트(값, 에러, 완료)를 지정된 시간만큼 지연.  
> 즉, 이벤트는 원래 방출된 시간에 따라 처리되지만, 구독자에게 전달되기까지의 시간은 지연.  

&nbsp;

![delay](https://github.com/user-attachments/assets/cbe14b70-0a4a-4ca7-a779-edd005b564a3)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

Observable<Int>
    .interval(.seconds(1), scheduler: MainScheduler.instance)
    .take(10)
    .delay(.seconds(5), scheduler: MainScheduler.instance)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
