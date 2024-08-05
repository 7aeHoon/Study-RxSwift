switchLatest
============

> 여러 Observable 시퀀스 중 하나의 Observable을 구독하여 구독한 Observable의 이벤트를 받아서 방출.   

&nbsp;

![switchLatest](https://github.com/user-attachments/assets/0f6a8fff-9e13-478b-8a8c-9916a06af6a6)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

enum MyError: Error {
   case error
}

let a = PublishSubject<String>()
let b = PublishSubject<String>()

let source = PublishSubject<PublishSubject<String>>()

source
    // 주로 Observable을 방출하는 Observable에서 사용
    .switchLatest()
    .subscribe { print($0) }
    .disposed(by: disposeBag)


a.onNext("1")
b.onNext("b")

// a를 구독
source.onNext(a)

// next(2)
a.onNext("2")
b.onNext("b")

// b를 구독
source.onNext(b)

// next(c)
a.onNext("2")
b.onNext("c")

// onCompleted가 호출되지 않음
a.onCompleted()
b.onCompleted()

// 직접 onCompleted를 호출하여 이벤트 종료
// completed
source.onCompleted()

// onError가 호출되지 않음
a.onError(MyError.error)
// onError가 호출되는 순간 즉시 onError 이벤트 방출
b.onError(MyError.error)
```
