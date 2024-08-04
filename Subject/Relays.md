PublishRelay, BehaviorRelay, ReplayRelay
========================================

> Subject와 유사하지만 차이점은 onNext이벤트만 전달.  
> onCompleted와 onError는 전달하거나 전달받지 않음.  
> disposed되어야 종료.  
> 주로 UI 작업처리에 사용.  

```swift
import UIKit
import RxSwift
import RxCocoa

let bag = DisposeBag()

let publishRelay = PublishRelay<Int>()

publishRelay.subscribe { print("p1 ", $0) }
    .disposed(by: bag)

// 이벤트 전달은 onNext가 아닌 accept
publishRelay.accept(1)

// 초기값으로 이벤트1 설정
let behaviorRelay = BehaviorRelay<Int>(value: 1)

// 이벤트2로 교체
behaviorRelay.accept(2)

//구독
behaviorRelay.subscribe { print("b1 ", $0) }
    .disposed(by: bag)

// 이벤트3으로 교체
behaviorRelay.accept(3)

// 릴레이가 저장하고있는 next값을 리턴
// 읽기 전용이라 새로운 값으로 변경하고 싶다면 accept로 새로운 이벤트를 전달
behaviorRelay.value

// 버퍼의 사이즈가 3
let replayRelay = ReplayRelay<Int>.create(bufferSize: 3)

// 이벤트1부터 이벤트10까지 전달
// 버퍼에 이벤트 8 9 10 유지
(1...10).forEach { replayRelay.accept($0) }

// next로 버퍼에 존재하는 이벤트 8 9 10 전달
replayRelay.subscribe { print("r1", $0) }
    .disposed(by: bag)
```
