distinctUntilChanged
====================

> 연속적으로 방출되는 값 중에서 이전 값과 동일하지 않은 값만을 방출.  
> 즉, 연속된 값들 중에서 이전과 동일한 값이 방출되지 않도록 필터링하여, 값의 중복을 제거하고 변화가 있을 때만 이벤트를 방출.  
> 데이터 스트림에서 중복된 연속값을 제거하고 변화가 있을 때만 반응하도록 하는 데 유용.  

&nbsp;

<img width="700" alt="distinctUntilChanged" src="https://github.com/user-attachments/assets/40047256-e6e2-43b4-8e9d-b18bd99e6eff">

&nbsp;

```swift
import UIKit
import RxSwift

struct Person {
    let name: String
    let age: Int
}

let disposeBag = DisposeBag()
let numbers = [1, 1, 3, 2, 2, 3, 1, 5, 5, 7, 7, 7]
let tuples = [(1, "하나"), (1, "일"), (1, "one")]
let persons = [
    Person(name: "Sam", age: 12),
    Person(name: "Paul", age: 12),
    Person(name: "Tim", age: 56)
]

// - 출력결과 -
// next(1)
// next(3)
// next(2)
// next(3)
// next(1)
// next(5)
// next(7)
// completed
Observable.from(numbers)
    .distinctUntilChanged()
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// .distinctUntilChanged(comparer:)
// 클로저의 리턴값을 사용하여 값을 비교
// true일 경우 방출하지 않고, false일 경우 방출
// next(1)
// next(2)
// next(2)
// next(3)
// completed
Observable.from(numbers)
    .distinctUntilChanged({ a, b in
        print(a, b)
        return !a.isMultiple(of: 2) && !b.isMultiple(of: 2)
    })
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// .distinctUntilChanged(keySelector:)
// next((1, "하나"))
// completed
Observable.from(tuples)
    .distinctUntilChanged({ $0.0 })
    // .distinctUntilChanged({ $0.1 })
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// .distinctUntilChanged(keyPath:)
// - 출력결과 -
// next(Person(name: "Sam", age: 12))
// next(Person(name: "Tim", age: 56))
// completed
Observable.from(persons)
    .distinctUntilChanged(at: \.age)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
