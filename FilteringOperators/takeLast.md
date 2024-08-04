takeLast
========

> Observable 시퀀스의 마지막 N개의 요소만을 방출하고, 그 이전의 요소는 무시.  
> 시퀀스가 완료된 후에 마지막 몇 개의 요소를 가져오는 데 유용.  

```swift
import UIKit
import RxSwift

enum MyError: Error {
   case error
}

let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

let subject = PublishSubject<Int>()

subject.takeLast(2)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// 마지막 이벤트 2개 9, 10을 버퍼에 저장
numbers.forEach { subject.onNext($0) }

// 마지막 이벤트 2개 10, 11을 버퍼에 저장
subject.onNext(11)

// - 출력결과 -
// next(10)
// next(11)
// completed
subject.onCompleted()

// 만약 에러 발생 시 next 이벤트는 전달되지 않고 error 이벤트만 전달
subject.onError(MyError.error)
```
