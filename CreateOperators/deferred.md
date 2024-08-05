deferred
========

> 지연된 지연된 생성(deferred creation)을 제공하는 Observable을 생성하는 데 사용.  
> 구독할 때마다 새로운 Observable을 생성하도록 하는데, 이는 각 구독자에게 개별적으로 새로운 시퀀스를 제공.  

&nbsp;

<img width="691" alt="deferred" src="https://github.com/user-attachments/assets/4574eeb2-2fef-4db1-8b77-52c99c1a99b9">

&nbsp;

```swift

import UIKit
import RxSwift

let disposeBag = DisposeBag()
let animals = ["🐶", "🐱", "🐹", "🐰", "🦊", "🐻", "🐯"]
let fruits = ["🍎", "🍐", "🍋", "🍇", "🍈", "🍓", "🍑"]
var flag = true

// 특정 조건에 따른 Observable 생성
let factory = Observable.deferred {
    flag.toggle()
    if flag {
        return Observable.from(animals)
    } else {
        return Observable.from(fruits)
    }
}

// - 출력결과 -
// next(🍎)
// next(🍐)
// next(🍋)
// next(🍇)
// next(🍈)
// next(🍓)
// next(🍑)
// completed
factory.subscribe { print($0) }
    .disposed(by: disposeBag)

//- 출력결과 -
//next(🐶)
//next(🐱)
//next(🐹)
//next(🐰)
//next(🦊)
//next(🐻)
//next(🐯)
//completed
factory.subscribe { print($0) }
    .disposed(by: disposeBag)

```
