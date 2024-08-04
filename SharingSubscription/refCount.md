refCount
========

> ConnectableObservable을 일반 Observable로 변환하여, 구독자가 있을 때만 소스 Observable에 대한 구독을 유지.  
> 만약 구독자가 없으면 소스 Observable에 대한 구독을 자동으로 해제.  
> connect 메서드를 호출할 필요 없이, 구독자가 존재하는 동안 자동으로 연결.  
> disconnect되고 새로운 구독자가 생길 경우 새로운 시퀀스 생성.  

&nbsp;

<img width="835" alt="refCount" src="https://github.com/user-attachments/assets/27d91a0c-98e8-4376-ad55-f4f62798a02a">

&nbsp;

```swift
import UIKit
import RxSwift

let bag = DisposeBag()
let source = Observable<Int>
    .interval(.seconds(1), scheduler: MainScheduler.instance)
    .debug()
    .publish()
    .refCount()

// 구독자가 모두 dispose 된다해도,
// publish() 내부 ConnectableObservable은 1초마다 next 이벤트 방출 중(.interval)
// 따라서 refCount를 사용하여 내부 ConnectableObservable를 dispose

let observer1 = source
    .subscribe { print("🔵", $0) }

// 3초 후 observer1 종료
DispatchQueue.main.asyncAfter(deadline: .now() + 3) {
    observer1.dispose()
}

// 여기서 구독자가 없어서 disconnect됨

// 다시 7초 후 observer2 구독
// 새로운 시퀀스를 생성. 즉 이벤트 0부터 다시 시작
DispatchQueue.main.asyncAfter(deadline: .now() + 7) {
    let observer2 = source.subscribe { print("🔴", $0) }
    
    // 3초 후 observer2 종료
    DispatchQueue.main.asyncAfter(deadline: .now() + 3) {
        observer2.dispose()
    }
}
```
