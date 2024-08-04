retry
=====

> Observable 시퀀스가 오류를 방출했을 때, 일정 횟수만큼 시퀀스를 다시 구독하여 재시도.  
> 기본적으로 무제한으로 재시도하지만 매개변수로 정해진 횟수를 지정할 수 있음.  

&nbsp;

<img width="775" alt="catch" src="https://github.com/user-attachments/assets/53e7d6ae-674d-41d0-ac0e-e19216cc9b8e">

&nbsp;

```swift
import UIKit
import RxSwift

let bag = DisposeBag()

enum MyError: Error {
    case error
}

var attempts = 1

let source = Observable<Int>.create { observer in
    let currentAttempts = attempts
    print("#\(currentAttempts) START")
    
    if attempts < 3 {
        observer.onError(MyError.error)
        attempts += 1
    }
    
    observer.onNext(1)
    observer.onNext(2)
    observer.onCompleted()
    
    return Disposables.create {
        print("#\(currentAttempts) END")
    }
}

source
    // 성공할 때까지 무한 실행
    .retry()
    // 입력된 수만큼 반복 실행
    //.retry(maxAttemptCount: Int)
    .subscribe { print($0) }
    .disposed(by: bag)
```
