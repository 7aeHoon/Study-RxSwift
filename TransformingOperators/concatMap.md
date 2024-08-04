concatMap
=========

> 원래 Observable이 방출하는 각 요소에 대해 새롭게 innerObservable을 생성하고, 이 Observable들을 순서대로 연결하여 방출.  
> 여러 innerObservable을 순차적으로 연결하여 처리하는 역할.  
> 즉, 한 Observable의 작업이 완료된 후에야 다음 Observable을 구독하여 작업을 시작.  

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

// 원본 Observable의 redCircle, greenCircle, blueCircle 이벤트 순서대로 방출
Observable.from([redCircle, greenCircle, blueCircle])
    .concatMap { circle -> Observable<String> in
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