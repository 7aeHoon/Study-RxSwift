repeatElement
========

> 특정 값을 무한히 반복하여 방출하는 Observable을 생성하는 데 사용.  
> 주어진 단일 값을 반복해서 계속 방출하며, 사용자가 구독을 종료할 때까지 반복.  

&nbsp;

![repeat](https://github.com/user-attachments/assets/35145437-c56b-4f07-895b-f99feda3abab)

&nbsp;

```swift

import UIKit
import RxSwift

let disposeBag = DisposeBag()
let element = "❤️"

// 무한으로 이벤트를 생성
Observable.repeatElement(element)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

// take 연산자를 사용하여 처음 5개의 이벤트만
Observable.repeatElement(element)
    .take(5)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

```
