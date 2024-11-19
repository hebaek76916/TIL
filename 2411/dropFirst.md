# `bindViewModel`에서 `dropFirst()` 사용하기

ViewController의 `viewDidLoad`에서 `bindViewModel`을 설정할 때, 초기 설정 시 `sink`가 동작하지 않도록 하기 위해 `dropFirst()`를 사용했습니다. 이렇게 하면 View가 로드된 후 `isMoved` 상태가 변경될 때만 애니메이션이 트리거됩니다.

`dropFirst`는 Swift의 Combine 프레임워크에서 제공하는 연산자 중 하나로, 퍼블리셔가 방출하는 첫 번째(또는 지정한 수의) 값을 무시하고 이후의 값들만 처리하도록 도와줍니다. 이 연산자는 특히 초기 상태를 무시하고, 이후에 발생하는 상태 변화에만 반응하고자 할 때 유용하게 사용됩니다.

```swift
private func bindViewModel() {
    // ViewModel의 isMoved 상태 변화에 따라 애니메이션 트리거
    viewModel.$isMoved
        .dropFirst() // 첫 번째(초기) 값을 무시
        .receive(on: RunLoop.main)
        .sink { [weak self] isMoved in
            guard let self = self else { return }
            // Do Binding Action
        }
        .store(in: &cancellables)
}
```

---

## 주요 포인트

- **`dropFirst()` 사용 이유:**
  - `bindViewModel`을 `viewDidLoad`에서 설정할 때, 퍼블리셔가 초기 상태를 즉시 방출하는 것을 방지하여 불필요한 애니메이션 실행을 막습니다.

- **애니메이션 트리거 조건:**
  - View가 로드된 후 `isMoved` 상태가 변경될 때만 `sink`가 동작하여 애니메이션을 실행합니다.

## 예시 설명

위의 코드는 Combine의 `dropFirst()` 연산자를 사용하여 `isMoved` 퍼블리셔의 첫 번째 값을 무시하고, 두 번째 이후에 발생하는 값들에 대해서만 `sink` 블록이 실행되도록 설정합니다. 이를 통해 초기 설정 시 불필요한 애니메이션 트리거를 방지할 수 있습니다.

---
