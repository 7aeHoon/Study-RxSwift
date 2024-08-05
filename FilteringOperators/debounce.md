debounce
====================

> 연속적으로 방출되는 값들 중 특정 시간 동안 새로운 값이 들어오지 않을 때 마지막 값을 방출.  
> 즉, 일정 시간 동안 안정화된 후에 값이 방출.  
> 만약 이벤트가 방출될 경우 debounce의 타이머는 초기화.

&nbsp;

<img width="770" alt="debounce" src="https://github.com/user-attachments/assets/6bec9104-b860-4be3-beb4-eb652b90ed56">

&nbsp;

```swift
let disposeBag = DisposeBag()

let buttonTap = Observable<String>.create { observer in
    DispatchQueue.global().async {
        // 0.3초 동안 이벤트 방출
        // 그러나 debounce의 타이머가 1초이기 때문에 구독자에게 이벤트가 전달되지 않음
        for i in 1...10 {
            observer.onNext("Tap \(i)")
            Thread.sleep(forTimeInterval: 0.3)
        }
        
        // 1초 대기하기 때문에 마지막 이벤트 10 방출
        Thread.sleep(forTimeInterval: 1)
        
        for i in 11...20 {
            // 0.5초 동안 이벤트 방출
            // 그러나 debounce의 타이머가 1초이기 때문에 구독자에게 이벤트가 전달되지 않음
            observer.onNext("Tap \(i)")
            Thread.sleep(forTimeInterval: 0.5)
        }
        
        // 마지막 이벤트 20 방출
        observer.onCompleted()
    }
    
    return Disposables.create {
        
    }
}

// - 출력결과 -
// next(Tap 10)
// next(Tap 20)
// completed
buttonTap
    .debounce(.milliseconds(1000), scheduler: MainScheduler.instance)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```
