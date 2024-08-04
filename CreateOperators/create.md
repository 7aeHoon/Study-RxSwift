create
======

> Observable을 수동으로 생성할 때 사용.  
> 클로저를 통해 Observer를 받아서 이벤트를 방출하는 방식으로 Observable을 구현.  
> 다양한 시나리오에서 유연하게 Observable을 정의.

```swift

import UIKit
import RxSwift

let disposeBag = DisposeBag()

enum MyError: Error {
    case urlError
    case encodingError
}

// onNext는 반드시 호출은 아니지만, onCompleted 또는 onError는 반드시 호출되어야함
// onCompleted 또는 onError가 호출되면 onNext는 이후에 호출되어도 이벤트가 방출되지 않음
Observable<String>.create { observer -> Disposable in
    guard let url = URL(string: "https://www.apple.com") else {
        observer.onError(MyError.urlError)
        return Disposables.create() // 주의! Disposable이 아닌 Disposables
    }
    
    guard let html = try? String(contentsOf: url, encoding: .utf8) else {
        observer.onError(MyError.encodingError)
        return Disposables.create() // 주의! Disposable이 아닌 Disposables
    }
    
    observer.onNext(html)
    observer.onCompleted()
    
    // 이미 completed된 상태라 방출안됨
    // observer.onNext("After completed")
    
    return Disposables.create() // 주의! Disposable이 아닌 Disposables
}.subscribe { print($0) }
    .disposed(by: disposeBag)
```
