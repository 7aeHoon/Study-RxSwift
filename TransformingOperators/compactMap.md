compactMap
==========

> map과 비슷하게 각 요소를 변환하지만, 변환 결과가 nil이 아닌 경우에만 요소를 방출.  
> 요소를 변환한 후 nil을 필터링하고, 유효한 값만 포함된 새로운 Observable을 생성.  

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()
let subject = PublishSubject<String?>()

// nil인 값들을 필터링하지만 여전히 옵셔널 타입

// - 출력결과 -
// next(Optional("⭐️"))
// next(Optional("⭐️"))
// next(Optional("⭐️"))
subject
    .filter({ $0 != nil }) // 필터링
    .subscribe { print($0) }
    .disposed(by: disposeBag)

Observable<Int>.interval(.milliseconds(300), scheduler: MainScheduler.instance)
    .take(10) // 10개의 이벤트만
    .map { _ in Bool.random() ? "⭐️" : nil }
    .subscribe(onNext: { subject.onNext($0) })
    .disposed(by: disposeBag)

// !를 사용하여 강제로 언래핑할 수 있지만 compactMap을 사용하면 간단함

// - 출력결과 -
// next(⭐️)
// next(⭐️)
// next(⭐️)
subject
    .compactMap({ $0 }) // 적용
    .subscribe { print($0) }
    .disposed(by: disposeBag)

Observable<Int>.interval(.milliseconds(300), scheduler: MainScheduler.instance)
    .take(10) // 10개의 이벤트만
    .map { _ in Bool.random() ? "⭐️" : nil }
    .subscribe(onNext: { subject.onNext($0) })
    .disposed(by: disposeBag)
```
