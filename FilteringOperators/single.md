single
======

> Observable이 정확히 하나의 요소만 방출하는지 확인.  
> 둘 이상의 요소를 방출하면 에러를 발생.  
> 주로 하나의 값을 제공하거나, 값 없이 에러를 방출하는 상황에서 유용하게 사용.  

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// - 출력결과 -
// next(1)
// completed
Observable.just(1)
    .single()
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// - 출력결과 -
// next(1)
// error(Sequence contains more than one element.)
Observable.from(numbers)
    .single()
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// - 출력결과 -
// next(3)
// completed
Observable.from(numbers)
    .single({ $0 == 3 })
    .subscribe { print($0) }
    .disposed(by: disposeBag)

let subject = PublishSubject<Int>()

subject.single()
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// - 출력결과 -
// next(100)
subject.onNext(100)

// 비록 1개의 이벤트만 전달받는다 하더라도 아직 completed가 호출되지는 않음.
// 이후 이벤트 존재여부에 따른 completed 또는 error 이벤트 호출

// case 1
// - 출력결과 -
// error(Sequence contains more than one element.)
subject.onNext(10)

// case 2
// - 출력결과 -
// completed
subject.onCompleted()
```
