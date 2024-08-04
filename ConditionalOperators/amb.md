amb
===

> 여러 Observable 시퀀스 중 가장 먼저 방출되는 Observable의 이벤트만 구독하고, 나머지 Observable은 무시하는 연산자.  
> amb는 "ambiguous"의 약자로, 여러 후보 중에서 가장 먼저 이벤트를 방출하는 Observable을 선택하는 동작을 나타냄.  
> 동일한 시점에서 경쟁 시작 -> 첫 번째 방출 Observable 선택 -> 나머지 Observable 무시.  

&nbsp;

![amb](https://github.com/user-attachments/assets/72c00ec8-30a6-4cb5-847d-a5bc270be344)

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
let c = PublishSubject<String>()

// 2개의 Observable을 비교할 경우
a.amb(b)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// 3개 이상의 Observable을 비교할 경우
Observable<String>.amb([a, b, c])
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// 각 Observable 이벤트 방출

// - 출력 결과 -
// next(C)
c.onNext("C")
a.onNext("A")
b.onNext("B")

// 무시
b.onCompleted()
// 무시
a.onCompleted()

// - 출력 결과 -
// completed
c.onCompleted()

```
