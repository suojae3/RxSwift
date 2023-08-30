# RxSwift

---

### [Chapter 02] Observables

1. 모든 것은 sequence이다. Observables은 단지 하나의 sequence이다. (마블다이어그램 떠올리기)
2. 이벤트는 값을 가지고 있다. 숫자, 인스턴스, 터치 등등
3. Observable이 한 번 끝나면 더이상 이벤트를 방출하지 않는다.

```swift
example(of: "just, of, from") {

	//1
	let one = 1
	let two = 2
	let three = 3

	//2 
	let observable = Observable<int>.just(one)
	let observable2 = Observable.of(one, two, three)
	let observable3 = Observable.of([one, two, three])
	let observable4 = Observable.from([one, two, three])
}
```

4. `just` 메서드는 하나의 element를 가지고 Observable 시퀀스를 만든다. 이 메서드는 observable의 static 메서드이다.
5. `observable2`는 inferred type이다. 이때 배열타입이 아니며 타입은 `Observable<Int>` 이다. (가변 파라미터를 가지고 있다)
6. `observable4`에서 `from` 메서드는 배열로부터 각각의 element를 뽑아내서 시퀸스를 만든다. 따라서 `from` 메서드는 오직 배열만 파라미터로 갖는다.
7. `Observable`은 `Subscriber`가 있기 전까지 이벤트를 처리하지않는다(그냥 시퀸스만 만들어둔다)
8. `Observable`의 각 이벤트를 핸들링할 수 있다. 핸들링하는 이벤트 타입으로 `observable`은 `next`, `error`, `completed` 세 가지를 emit한다.

```swift
example(of: "subscribe") {
	let one = 1
	let two = 2
	let three = 3
	let observable = Observable.of(one, two, three)
}

observable.subscribe { event in 
	print(event)
}

/*
--- Example of: subsvribe ---
next(1)
next(2)
next(3)
completed
*/
```

9. `next`는 각 이벤트를 handler에게 패싱한다. `error`는 에러인스턴스를 가지고 있다.

```swift
example(of: "DisposeBag") {
	let disposeBag = DisposeBag()
	
	Observable.of("A", "B", "C")
		.subscribe { 
				print($0)
		}
		.disposed(by: disposeBag)
}
```

10. subscription이 끝나면 dispose를 해주어야한다. 그렇지 않으면 Observable이 계속 이벤트를 방출하기 때문이다. 이러한 역할을 `.dispose` 메서드를 통해 해준다
11. 이때 각각의 이벤트 구독이 끝날 때마다 dispose하지 않으면 메모리 누수가 발생한다.
12. RxSwift에는 세 가지 특징이 있다. - Single, Maybe, Completable.
