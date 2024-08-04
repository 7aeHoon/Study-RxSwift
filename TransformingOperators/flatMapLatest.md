flatMapLatest
============

> flatMapFirst는 원래 Observable이 방출하는 첫 번째 요소에 대해서만 변환을 수행하고, 해당 변환이 완료될 때까지 다음 요소의 변환을 무시.  
> 새로운 Observable을 병합하기 전에 이전 Observable의 시퀀스가 완료될 때까지 대기.

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
let trigger = PublishSubject<Void>()

sourceObservable
    .flatMapLatest { circle -> Observable<String> in
        switch circle {
        case redCircle:
            return Observable<Int>.interval(.milliseconds(200), scheduler: MainScheduler.instance)
                .map { _ in redHeart}
                // 트리거가 이벤트를 방출하기 전까지 이벤트 방출
                .take(until: trigger)
        case greenCircle:
            return Observable<Int>.interval(.milliseconds(200), scheduler: MainScheduler.instance)
                .map { _ in greenHeart}
                // 트리거가 이벤트를 방출하기 전까지 이벤트 방출
                .take(until: trigger)
        case blueCircle:
            return Observable<Int>.interval(.milliseconds(200), scheduler: MainScheduler.instance)
                .map { _ in blueHeart}
                // 트리거가 이벤트를 방출하기 전까지 이벤트 방출
                .take(until: trigger)
        default:
            return Observable.just("")
        }
    }
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// redCircle 이벤트 방출
sourceObservable.onNext(redCircle)

// greenCircle 이벤트 방출하고 기존 innerObservable인 redCircle 이벤트 방출 종료
DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
    sourceObservable.onNext(greenCircle)
}

// blueCircle 이벤트 방출하고 기존 innerObservable인 greenCircle 이벤트 방출 종료
DispatchQueue.main.asyncAfter(deadline: .now() + 2) {
    sourceObservable.onNext(blueCircle)
}

// trigger 이벤트 방출하고 .take(until: trigger)에 따른 sourceObservable 이벤트 종료
DispatchQueue.main.asyncAfter(deadline: .now() + 10) {
    trigger.onNext(())
}
```