flatMap
=======

> 원래의 Observable이 방출하는 각 요소를 Observable로 변환한 후, 이 새로운 Observable들이 방출하는 값을 하나의 Observable 시퀀스로 병합하여 방출.  
> 이 과정을 "flattening"이라고 부르며, 여러 Observable 스트림을 하나의 스트림으로 합치는 효과.  
> 주의! 순서를 보장하지 않음.

&nbsp;

![flatMap](https://github.com/user-attachments/assets/85c289d8-f93c-4a4b-8f66-e5d3f564cbd2)

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
// next(💚)
// next(❤️)
// next(💚)
// next(💙)
// next(❤️)
// next(💚)
// next(💙)
// next(❤️)
// next(💚)
// next(💙)
// next(💚)
// next(💙)
// next(💙)
// completed

Observable.from([redCircle, greenCircle, blueCircle])
    // flatMap 내부의 Observable은 innerObservable이고,
    // 그것들을 모두 합쳐서 하나의 Observable을 생성
    .flatMap { circle -> Observable<String> in
        switch circle {
        case redCircle:
            return Observable.repeatElement(redHeart).take(5)
        case greenCircle:
            return Observable.repeatElement(greenHeart).take(5)
        case blueCircle:
            return Observable.repeatElement(blueHeart).take(5)
        default:
            return Observable.just("")
        }
    }
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
