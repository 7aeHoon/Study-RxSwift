catch
=====

> 오류가 발생했을 때 오류를 처리하거나 대체 시퀀스를 제공하는 데 사용.  
> 오류가 발생했을 때 다른 Observable을 반환.  
> 즉, 원본 Observable이 오류를 내면, 이를 다른 Observable로 대체하여 에러를 처리.  

&nbsp;

<img width="775" alt="catch" src="https://github.com/user-attachments/assets/53e7d6ae-674d-41d0-ac0e-e19216cc9b8e">

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

enum MyError: Error {
    case error404
}

let subject = PublishSubject<Int>()
let recovery = PublishSubject<Int>()

subject
    // Observable을 recovery로 교체
    .catch { _ in recovery }
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// subject는 에러 이벤트를 방출하고 종료
subject.onError(MyError.error404)

// catch 연산자로 인하여 recovery로 교체됨
recovery.onNext(11)
recovery.onCompleted()
```
