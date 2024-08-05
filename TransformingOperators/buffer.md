buffer
======

> Observable 시퀀스의 요소를 일정한 크기 또는 시간 간격으로 그룹화하여 배열 형태로 방출.  
> timeSpan만큼 경과하거나 count개의 요소가 수집되면 버퍼링된 요소가 방출
> 크기 기반 버퍼링, 시간 기반 버퍼링, 혼합 기반 버퍼링 존재.  

&nbsp;

![buffer](https://github.com/user-attachments/assets/91a60358-6876-4b90-b6b3-2bb863bae2c5)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

// - 출력결과 -
// next([0])
// next([1, 2, 3]) -> 3개가 모여서 방출
// next([4, 5]) -> 2초가 지나서 방출
// next([6, 7])
// next([8, 9])
// completed
Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)
    // .never로 지정하면 이벤트 수집 시간은 없음
    // 2초마다 수집된 이벤트 방출
    // 수집된 이벤트가 3개면 이벤트 방출, 꼭 3개가 수집되고 방출되는 것은 아님
    .buffer(timeSpan: .seconds(2), count: 3, scheduler: MainScheduler.instance)
    .take(5)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
