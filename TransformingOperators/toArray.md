toArray
=======

> Observable이 방출하는 여러 요소들을 하나의 배열로 모아서 방출.  
> onCompleted 이벤트를 방출할 때까지 수집된 모든 요소를 배열로 만들어 단일 이벤트로 방출.  

&nbsp;

<img width="700" alt="toArray" src="https://github.com/user-attachments/assets/eefd4809-fefc-431a-b40c-a2b5e0c7a3d8">

&nbsp;

```swift
let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

let subject = PublishSubject<Int>()

subject
    .toArray()
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// 단일 이벤트를 모아서 배열에 저장
subject.onNext(1)
subject.onNext(2)
subject.onNext(3)

// completed가 불린 시점부터 구독자에게 이벤트 방출
subject.onCompleted()

// - 출력결과 -
// success([1, 2, 3])
```
