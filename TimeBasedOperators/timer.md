timer
=====

> 일정 시간이 지난 후에 또는 일정 시간 간격으로 값을 방출하는 Observable을 생성하는 데 사용.  
> 두 가지 방식으로 사용 가능.
> 구독한 시점으로부터 단일 값 방출(주기를 사용하지 않았을 때)과 주기적 값 방출.  

&nbsp;

![timer](https://github.com/user-attachments/assets/2c846cc4-b6da-48a2-a87d-9251d4172c3c)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()


// 주의! interval의 주기와 다름
// 구독 후 1초 뒤에 단일 이벤트 방출

// - 출력 결과 -
// next(0)
// completed
Observable<Int>.timer(.seconds(1), scheduler: MainScheduler.instance)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// 구독 후 1초 뒤에 이벤트 방출
// 0.5초 주기로 무한 반복
Observable<Int>.timer(.seconds(1),
                      period: .milliseconds(500) ,
                      scheduler: MainScheduler.instance)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
