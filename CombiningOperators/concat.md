concat
======

> 두 개 이상의 Observable 시퀀스를 결합하여 하나의 시퀀스로 만드는 데 사용.  
> 첫 번째 Observable 시퀀스가 완료된 후 두 번째 Observable 시퀀스를 연결하여 방출.
> 여러 시퀀스를 순차적으로 방출할 때 유용.  

&nbsp;

![concat](https://github.com/user-attachments/assets/80491b6c-bbbf-4886-ae28-6b1b1b336489)

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()
let fruits = Observable.from(["🍏", "🍎", "🥝"])
let animals = Observable.from(["🐶", "🐱", "🐹"])

// - 출력결과 -
// next(🍏)
// next(🍎)
// next(🥝)
// next(🐶)
// next(🐱)
// next(🐹)
// completed
Observable<String>
    .concat([fruits, animals])
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// 출력결과 동일
fruits.concat(animals)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
