flatMapFirst
============

> flatMapFirst는 원래 Observable이 방출하는 첫 번째 요소에 대해서만 변환을 수행하고, 해당 변환이 완료될 때까지 다음 요소의 변환을 무시.  
> 새로운 Observable을 병합하기 전에 이전 Observable의 시퀀스가 완료될 때까지 대기.

&nbsp;

<img width="700" alt="flatmapFirst" src="https://github.com/user-attachments/assets/29392580-8b0c-4884-9044-d2aae9ec8304">

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

let redCircle = "🔴"
let greenCircle = "🟢"
let blueCircle = "🔵"

let redHeart = "❤️"
let greenHeart = "💚"
let blueHeart = "💙"

// - 출력결과 -
// next(❤️)
// next(❤️)
// next(❤️)
// next(❤️)
// next(❤️)
// completed
Observable.from([redCircle, greenCircle, blueCircle])
    // innerObservable 중 가장 먼저 이벤트를 방출하는 Observable만 전달
    // 나머지 Observable은 무시
    .flatMapFirst { circle -> Observable<String> in
        switch circle {
        case redCircle:
            return Observable.repeatElement(redHeart)
                .take(5)
        case greenCircle:
            return Observable.repeatElement(greenHeart)
                .take(5)
        case blueCircle:
            return Observable.repeatElement(blueHeart)
                .take(5)
        default:
            return Observable.just("")
        }
    }
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

let redCircle = "🔴"
let greenCircle = "🟢"
let blueCircle = "🔵"

let redHeart = "❤️"
let greenHeart = "💚"
let blueHeart = "💙"

let sourceObservable = PublishSubject<String>()

sourceObservable
    .flatMapFirst { circle -> Observable<String> in
        switch circle {
        case redCircle:
            return Observable<Int>.interval(.milliseconds(200), scheduler: MainScheduler.instance)
                .map { _ in redHeart}
                .take(10)
        case greenCircle:
            return Observable<Int>.interval(.milliseconds(200), scheduler: MainScheduler.instance)
                .map { _ in greenHeart}
                .take(10)
        case blueCircle:
            return Observable<Int>.interval(.milliseconds(200), scheduler: MainScheduler.instance)
                .map { _ in blueHeart}
                .take(10)
        default:
            return Observable.just("")
        }
    }
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// 첫 번째로 바로 이벤트 방출
sourceObservable.onNext(redCircle)

// redCircle이 먼저 방출을 시작하고 있기 때문에 greenCircle 방출은 무시
DispatchQueue.main.asyncAfter(deadline: .now() + 0.5) {
    sourceObservable.onNext(greenCircle)
}

// redCircle이 먼저 방출을 시작하고 있기 때문에 blueCircle 방출은 무시
DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
    sourceObservable.onNext(blueCircle)
}

// redCircle이 먼저 방출을 시작(2초 동안)하고 종료된 시점(2초 후)이기 때문에 blueCircle 방출
// 방출 주기가 서로 겹치지 않아서 가능
DispatchQueue.main.asyncAfter(deadline: .now() + 2) {
    sourceObservable.onNext(blueCircle)
}
```
