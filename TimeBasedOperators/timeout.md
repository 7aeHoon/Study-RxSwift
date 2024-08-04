timeout
=======

> 특정 시간 내에 Observable이 이벤트를 방출하지 않으면 에러를 방출하도록 설정.  
> 타임아웃이 발생하면 에러를 방출.  
> other 파라미터를 사용하면 타임아웃이 발생했을 때 대체 Observable로 전환 가능.  

&nbsp;

![timeout](https://github.com/user-attachments/assets/9744048a-63d3-4942-94d6-230b51e83b98)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

let subject = PublishSubject<Int>()

// 3초안에 next 이벤트를 방출하지 못하면 error 이벤트 방출

// - 출력 결과 -
// error(Sequence timeout.)
subject
    .timeout(.seconds(3), scheduler: MainScheduler.instance)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// next 이벤트를 전달받을 경우
// Observable이 subject에 next 이벤트 전달
Observable<Int>
    // 구독 후 2초 후, 주기는 1초마다
    .timer(.seconds(2),
           period: .seconds(1),
           scheduler: MainScheduler.instance)
    // next 이벤트를 전달
    .subscribe{ subject.onNext($0) }
    .disposed(by: disposeBag)

// next 이벤트를 전달받을 경우
// Observable이 subject에 next 이벤트 전달
// 그러나 주기가 4초이므로 subject는 3초 이내에 next 이벤트를 전달받지 못하여 error 방출
Observable<Int>
    // 구독 후 2초 후, 주기는 4초마다
    .timer(.seconds(2),
           period: .seconds(4),
           scheduler: MainScheduler.instance)
    // next 이벤트를 전달
    .subscribe{ subject.onNext($0) }
    .disposed(by: disposeBag)

// error 방출 대신 other Observable의 이벤트 방출 후 completed
subject
    .timeout(.seconds(3),other: Observable<Int>.just(0) ,scheduler: MainScheduler.instance)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
