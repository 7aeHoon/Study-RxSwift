skip(duration:scheduler:)
=========================

> Observable 시퀀스의 요소를 일정 시간 동안 무시하고, 그 시간이 경과된 후부터 방출하는 요소들만 구독자에게 전달.  
> 주로 타이밍 기반의 데이터 처리가 필요할 때 사용.  

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

// 1초동안 이벤트를 방출하는 Observable 생성
// 정수 0부터 시작
let o = Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)

// 3초동안 이벤트 스킵
// 시간 측정에 오차가 존재할 수 있음
o.take(10)
    .skip(.seconds(3), scheduler: MainScheduler.instance)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
