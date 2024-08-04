share
=====

> 하나의 Observable을 여러 구독자가 공유하도록 만드는 데 사용.  
> 즉, 소스 Observable을 멀티캐스트하여 여러 구독자가 동일한 시퀀스를 공유.  
> 기본적으로 publish와 refCount를 결합한 형태로 동작.  
> 구독자가 있을 때만 소스 Observable을 구독하고, 구독자가 없으면 소스 Observable에 대한 구독을 자동으로 해제.  
> 구독자마다 별도로 Observable을 생성하지 않고, 동일한 Observable 시퀀스를 여러 구독자가 공유하여 중복된 작업을 방지.  

&nbsp;

<img width="630" alt="share" src="https://github.com/user-attachments/assets/6f5b546f-b350-47b0-ab07-17937109ff94">

&nbsp;

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()

let source = Observable<String>.create { observer in
    let url = URL(string: "https://kxcoding-study.azurewebsites.net/api/string")!
    let task = URLSession.shared.dataTask(with: url) { (data, response, error) in
        if let data = data, let html = String(data: data, encoding: .utf8) {
            observer.onNext(html)
        }
        
        observer.onCompleted()
    }
    task.resume()
    
    return Disposables.create {
        task.cancel()
    }
}
.debug()
.share()

// share()를 사용하지 않을 때, 불필요한 Observable create 작업이 3번 호출됨
// share()를 사용하여 Observable create 작업이 1번 수행되고, 구독자에게 이벤트 방출

// 구독자 1
source
    .subscribe {print("1 >> ", $0)}
    .disposed(by: disposeBag)

// 구독자 2
source
    .subscribe {print("2 >> ", $0)}
    .disposed(by: disposeBag)

// 구독자 3
source
    .subscribe {print("3 >> ", $0)}
    .disposed(by: disposeBag)
```

```swift
import UIKit
import RxSwift

let disposeBag = DisposeBag()
let source = Observable<Int>
    .interval(.seconds(1), scheduler: MainScheduler.instance)
    .debug()
    // replay의 기본값은 0.
    .share()
    // replay로 버퍼 사이즈 설정
    // scope로
    // .share(replay: 5, scope: .whileConnected)
    // .share(replay: 5, scope: .forever)

// 구독자 1
let observer1 = source
    .subscribe { print("🔵", $0) }

// 구독자 2
let observer2 = source
    .delaySubscription(.seconds(3), scheduler: MainScheduler.instance)
    .subscribe { print("🔴", $0) }

// 시퀀스를 공유하는 구독자 1과 구독자 2
DispatchQueue.main.asyncAfter(deadline: .now() + 5) {
    observer1.dispose()
    observer2.dispose()
}

// 구독자 1과 구독자 2가 모두 dispose됬기 때문에 share의 시퀀스 종료(refCount).
// disposed

// 구독자 3
// 다시 새로운 시퀀스를 생성
DispatchQueue.main.asyncAfter(deadline: .now() + 7) {
    let observer3 = source.subscribe { print("⚫️", $0) }
    
    DispatchQueue.main.asyncAfter(deadline: .now() + 3) {
        observer3.dispose()
    }
}
```
