[890, 210, 940, 910] 이렇게 있는데 911 히고 제일 차이가 적지만 큰 수를 고르고 싶어. 910이지? 이거 찾는 알고리즘
```swift
func findClosestNumberGreaterThanTarget(in numbers: [Int], target: Int) -> Int? {
    return numbers
        .filter { $0 > target }
        .min(by: { ($0 - target) < ($1 - target) })
}
```
