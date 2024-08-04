replay, replayAll
=================

> Observable을 ConnectableObservable로 변환하여, 새로운 구독자가 이전에 방출된 이벤트를 재생(replay).  
> 내부적으로 ReplaySubject를 사용하여 최근 N개의 이벤트를 캐시하고, 새로운 구독자에게 이 이벤트들을 재생.  
> 멀티캐스팅 방식으로 여러 구독자가 동일한 이벤트 시퀀스를 공유.  
> 이벤트 방출을 시작하기 위해 connect 반드시 호출.  

&nbsp;

![replay](https://github.com/user-attachments/assets/7b2e1c64-6e9e-4473-a812-efca369be41a)

&nbsp;

```swift
import UIKit
import RxSwift

let bag = DisposeBag()

let source = Observable<Int>
    .interval(.seconds(1), scheduler: MainScheduler.instance)
    .take(5)
    // ReplaySubject<Int>.create(bufferSize: 5)과 같은 동작
    // .replayAll()는 버퍼 사이즈에 제약이 없으나, 사용 지양
    .replay(5)

source
    .subscribe { print("🔵", $0) }
    .disposed(by: bag)

source
    .delaySubscription(.seconds(3), scheduler: MainScheduler.instance)
    .subscribe { print("🔴", $0) }
    .disposed(by: bag)

source.connect()
```
