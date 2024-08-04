ReplaySubject
=============

> 특구독자가 구독을 시작할 때 이전에 발행된 이벤트들을 재생.  
> 구독자가 구독을 시작하기 전에 발행된 이벤트들도 구독자에게 전달될 수 있음.  

```swift

import UIKit
import RxSwift

let disposeBag = DisposeBag()

enum MyError: Error {
   case error
}

// 버퍼의 사이즈만큼 이벤트를 저장.
// PublishSubject나 BehaviorSubject와는 달리 .create로 생성.
let replaySubject = ReplaySubject<Int>.create(bufferSize: 3)

// 10개의 이벤트를 전달
(1...10).forEach { replaySubject.onNext($0)}

// 이벤트를 전달 후 구독한 상태
// 8 9 10이 버퍼에 존재하여 8 9 10 이벤트 전달
replaySubject.subscribe { print("Observer1 ", $0) }
    .disposed(by: disposeBag)

replaySubject.subscribe { print("Observer2 ", $0) }
    .disposed(by: disposeBag)

// 8이 삭제되고 11이 버퍼에 추가됨
replaySubject.onNext(11)

replaySubject.subscribe { print("Observer3 ", $0) }
    .disposed(by: disposeBag)

replaySubject.onCompleted()

// 새로운 구독자일 경우 우선 버퍼의 이벤트를 전달하고 onCompleted또는 onError 전달
replaySubject.subscribe { print("Observer4 ", $0) }
    .disposed(by: disposeBag)

```
