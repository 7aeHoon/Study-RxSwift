throttle
====================

> 일정 시간 간격 동안 방출되는 이벤트를 제어하여 지나치게 많은 이벤트가 빠르게 처리되지 않도록 하는 데 사용.  
> 주어진 시간 간격 내에서 첫 번째 이벤트만을 방출하거나, 마지막 이벤트만을 방출하는 방식으로 동작.  
> 이벤트 방출 속도를 제어하고 시스템 자원 사용을 최적화.
> 주로 버튼이 빠르게 클릭됬을 때 클릭 수를 조절하기 위해 사용(남발방지).

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

let buttonTap = Observable<String>.create { observer in
    DispatchQueue.global().async {
        for i in 1...10 {
            observer.onNext("Tap \(i)")
            Thread.sleep(forTimeInterval: 0.3)
        }
        
        Thread.sleep(forTimeInterval: 1)
        
        for i in 11...20 {
            observer.onNext("Tap \(i)")
            Thread.sleep(forTimeInterval: 0.5)
        }
        
        observer.onCompleted()
    }
    
    return Disposables.create {
        
    }
}

// - 출력결과 -
// next(Tap 1)
// next(Tap 4)
// next(Tap 7)
// next(Tap 10)
// next(Tap 11)
// next(Tap 12)
// next(Tap 14)
// next(Tap 16)
// next(Tap 18)
// next(Tap 20)
// completed
buttonTap
    .throttle(.milliseconds(1000), scheduler: MainScheduler.instance)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
### latest 설정

- true일 때
![throttle](https://github.com/user-attachments/assets/c63785be-139b-4dcc-b31c-74b825de1af4)

- false일 때
![throttleFalse](https://github.com/user-attachments/assets/bb26d140-9d83-49bc-a5bf-d7aa6cbe626a)

```swift
let disposeBag = DisposeBag()

func currentTimeString() -> String {
   let f = DateFormatter()
   f.dateFormat = "yyyy-MM-dd HH:mm:ss.SSS"
   return f.string(from: Date())
}


Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)
   .debug()
   .take(10)
   .throttle(.milliseconds(2500), latest: true, scheduler: MainScheduler.instance)
   .subscribe { print(currentTimeString(), $0) }
   .disposed(by: disposeBag)


Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)
   .debug()
   .take(10)
   .throttle(.milliseconds(2500), latest: false, scheduler: MainScheduler.instance)
   .subscribe { print(currentTimeString(), $0) }
   .disposed(by: disposeBag)
```


