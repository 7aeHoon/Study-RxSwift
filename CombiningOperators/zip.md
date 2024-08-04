zip
===

> 여러 Observable 시퀀스를 결합하여 새로운 Observable 시퀀스를 생성.  
> 각 Observable에서 방출된 요소들을 순차적으로 결합하여 튜플로 만듦.
> 즉, 각 Observable이 동일한 인덱스에 있는 요소들을 묶어서 새로운 Observable을 생성.  
> 주의! 모든 Observable이 완료되어야 결합된 시퀀스도 완료되고 종료.  
> 주의! 만약 결합된 시퀀스 중 하나에서 에러가 발생하면, 결합된 시퀀스 전체가 에러를 방출하고 종료.  

&nbsp;

![zip](https://github.com/user-attachments/assets/23ab4dc0-3594-478a-a818-fe769f06a5f6)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

enum MyError: Error {
   case error
}

let numbers = PublishSubject<Int>()
let strings = PublishSubject<String>()

Observable.zip(numbers, strings)
    // tuple로 이벤트 전달
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// next((1, "one"))
numbers.onNext(1)
strings.onNext("one")

// 항상 짝을 맞춰서 이벤트를 전달하기 때문에 이벤트 방출 없음
numbers.onNext(2)

// 짝이 맞춰지는 순간 이벤트 방출
// next((2, "two"))
strings.onNext("two")

// 하나라도 onCompleted가 호출되면 이후 이벤트 방출 없음
numbers.onCompleted()
strings.onNext("three")

// 그러나 하나라도 onError가 호출되면 즉시 error 이벤트 방출
// error(error)
strings.onError(MyError.error)

// 모두 onCompleted가 호출되어야 completed 이벤트 방출
// completed
strings.onCompleted()
```
