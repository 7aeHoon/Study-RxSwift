flatMapLatest
============

> 소스 Observable이 방출하는 각 아이템에 대해 새로운 Observable을 생성하고 이 Observable이 방출하는 아이템들로 결과 Observable을 생성.  
> 소스 Observable이 새로운 아이템을 방출할 때마다 이전에 생성된 Observable이 방출하는 아이템들을 무시.  
> 가장 최근에 생성된 Observable만의 아이템들을 방출.  

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
