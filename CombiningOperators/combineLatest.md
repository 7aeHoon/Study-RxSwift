combineLatest
=============

> 여러 Observable 시퀀스를 결합하여 새로운 Observable 시퀀스를 생성.  
> 각 Observable이 새로운 값을 방출할 때마다, 가장 최근의 값을 조합하여 새로운 값을 방출.
> 따라서 모든 Observable이 최소 한 번씩 값을 방출할 때까지는 값이 방출되지 않음.  
> 주의! 모든 Observable이 완료되어야 결합된 시퀀스도 완료되고 종료.
> 주의! 만약 결합된 시퀀스 중 하나에서 에러가 발생하면, 결합된 시퀀스 전체가 에러를 방출하고 종료.  

&nbsp;

![스크린샷 2024-08-02 오전 3 10 46](https://github.com/user-attachments/assets/ef0ec77e-a237-45e2-b014-dfca889bb1ee)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

enum MyError: Error {
   case error
}

let greetings = PublishSubject<String>()
let languages = PublishSubject<String>()

Observable.combineLatest(greetings, languages)
    // tuple로 이벤트 전달
    .subscribe { print($0) }
    .disposed(by: disposeBag)


// next(("Hello", "World"))
greetings.onNext("Hello")
languages.onNext("World")

// next(("Hello", "Korea"))
languages.onNext("Korea")

// next(("South", "Korea"))
greetings.onNext("South")

greetings.onCompleted()

// greetings이 onCompleted 되었지만 가장 최신 이벤트 값을 유지 중
// next(("South", "Africa"))
languages.onNext("Africa")

// greetings, languages 모두 onCompleted된 후 onCompleted 이벤트 방출
// completed
languages.onCompleted()

// 만약 하나라도 onError 이벤트가 방출될 경우 즉시 onError 이벤트 방출되고 종료
greetings.onError(MyError.error)
```
