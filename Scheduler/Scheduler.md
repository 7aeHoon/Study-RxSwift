Scheduler
=========

> 작업을 어느 스레드에서 실행할지, 언제 실행할지를 결정.  
> subscribe(on: )은 Observable의 구독이 어느 스케줄러에서 실행될지를 결정.  
> observe(on: )은 선언 이후 연산자가 어느 스케줄러에서 실행될지를 결정.  

&nbsp;

![scheduler](https://github.com/user-attachments/assets/db5acfbe-9e4a-4e19-979f-fd560385c7c6)

&nbsp;

```swift
import UIKit
import RxSwift

// Scheduler의 종류

// 1. MainScheduler
//      - 메인 쓰레드에서 수행
//      - MainScheduler.instance: synchronous
//      - MainScheduler.asyncInstance: asynchronous

// 2. CurrentThreadScheduler
//      - 현재 쓰레드에서 수행
//      - default

// 3. SerialDispatchQueueScheduler
//      - 특정 DispatchQueue에서 수행
//      - Serial하게 작업을 처리

// 4. ConcurrentDispatchQueueScheduler
//      - 특정 DispatchQueue에서 수행
//      - Concurrent하게 작업을 처리

// 5. OperationQueueScheduler
//      - 특정 NSOperationQueue에서 수행

let disposeBag = DisposeBag()
let backgroundScheduler = ConcurrentDispatchQueueScheduler(queue: .global())

// Observable이 생성되고 연산자가 호출되는 시점은 구독 이후부터
// 별도로 설정하지 않는다면 기본 Scheduler로 currentScheduler를 사용
Observable.of(1, 2, 3, 4, 5, 6, 7, 8, 9)
    .filter { num -> Bool in
        print(Thread.isMainThread ? "Main Thread" : "Background Thread", ">> filter")
        return num.isMultiple(of: 2)
    }
    // 호출되는 순서에 따라 영향을 끼침
    // 각각의 연산자에 따른 스레드를 변경하고 싶을 때
    .observe(on: backgroundScheduler)
    .map { num -> Int in
        print(Thread.isMainThread ? "Main Thread" : "Background Thread", ">> map")
        return num * 2
    }
    // Observable의 처음 스레드를 설정
    // 언제 호출되어도 상관없음
    // 처음 시퀀스를 MainScheduler.instance에 전달하도록 설정
    .subscribe(on: MainScheduler.instance)
    .subscribe {
        print(Thread.isMainThread ? "Main Thread" : "Background Thread", ">> sub")
        print($0)
    }
    .disposed(by: disposeBag)
```
