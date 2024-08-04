merge
======

> 여러 Observable 시퀀스를 병합하여 하나의 Observable 시퀀스로 만드는 데 사용.  
> 각 Observable이 방출하는 요소들을 동시에 방출할 수 있도록 설정.
> 순서에 상관없이 이벤트가 방출되는 즉시 이벤트 방출.  
> 주의! 모든 Observable이 완료되어야 병합된 시퀀스도 완료되고 종료.  
> 주의! 만약 병합된 시퀀스 중 하나에서 에러가 발생하면, 병합된 시퀀스 전체가 에러를 방출하고 종료.  

&nbsp;

<img width="796" alt="merge" src="https://github.com/user-attachments/assets/2638edfd-337f-485e-a156-b499b12daa44">

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

enum MyError: Error {
   case error
}

let oddNumbers = BehaviorSubject(value: 1)
let evenNumbers = BehaviorSubject(value: 2)
let negativeNumbers = BehaviorSubject(value: -1)

// BehaviorSubject이라서 가장 최근의 이벤트 1과 이벤트 2 방출

// next(1)
// next(2)
Observable.of(oddNumbers, evenNumbers)
    // 최대 수 제한 없음
    .merge()
    // maxConcurrent는 병합 가능한 Observable의 최대 수
    // 최대 수가 넘어갈 경우 큐에서 대기
    // onCompleted된 Observable이 제거되고 대기하던 Observable 저장
    // .merge(maxConcurrent: 5)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// next(3)
// next(4)
oddNumbers.onNext(3)
evenNumbers.onNext(4)

// 순차적으로 방출

// next(6)
// next(5)
evenNumbers.onNext(6)
oddNumbers.onNext(5)

// oddNumbers 종료
oddNumbers.onCompleted()

// next(8)
evenNumbers.onNext(8)

// evenNumbers 종료
evenNumbers.onCompleted()

// merge된 Observable이 모두 종료되고난 후 onCompleted 호출

// oddNumbers 에러
// 만약 merge된 Observable 중 하나라도 onError를 방출하면 즉시 onError 호출
oddNumbers.onError(MyError.error)
```
