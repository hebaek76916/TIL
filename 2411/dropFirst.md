
ViewController의 viewDidLoad에서 bindViewModel을 설정할 때, 초기 설정 시 sink가 동작하지 않도록 하기 위해 dropFirst()를 사용했습니다.
이렇게 하면 View가 로드된 후 isMoved 상태가 변경될 때만 애니메이션이 트리거됩니다.

dropFirst는 Swift의 Combine 프레임워크에서 제공하는 연산자 중 하나로, 퍼블리셔가 방출하는 첫 번째(또는 지정한 수의) 값을 무시하고 이후의 값들만 처리하도록 도와줍니다. 이 연산자는 특히 초기 상태를 무시하고, 이후에 발생하는 상태 변화에만 반응하고자 할 때 유용하게 사용됩니다.

```swift
private func bindViewModel() {
    // ViewModel의 isMoved 상태 변화에 따라 애니메이션 트리거
    viewModel.$isMoved
        .dropFirst() // 첫 번째(초기) 값을 무시
        .receive(on: RunLoop.main)
        .sink { [weak self] isMoved in
            guard let self = self else { return }
            //Do Binding Action
        }
        .store(in: &cancellables)
}
```
