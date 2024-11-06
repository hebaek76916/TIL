## "COW swift struct compiler optimization"
compiler가 struct를 COW(Copy on Write) Optimization 하는 방법.

https://forums.swift.org/t/large-struct-cow-optimization/46921

```swift
struct DestinationDTO {
    let id: String
}

struct MeetingDTO {
    let id: String
}

struct Meeting {
    let id: String
    let destinationId: String
    
    init(meeting: MeetingDTO, destinations: [DestinationDTO]) {
        self.id = meeting.id
        ...
    }
}

let destinations = [DestinationDTO(id: "1"), DestinationDTO(id: "2")])

let meeting = Meeting(meeting: MeetingDTO(id: "21"), destinations: destinations]
```

Q. `DestinationDTO`가 아주 아주 큰 struct라고 가정할때, `Meeting`에서는 `init`을 `destinations`을 인자로 받아서 
`destinationId`를 생성할때밖에 안쓰는데, COW가 될까?

A. 
| Collections defined by the standard library like arrays, dictionaries, and strings use an optimization to reduce the performance cost of copying.
| Instead of making a copy immediately, 
| these collections share the memory where the elements are stored between the original instance and any copies. 
-> 컴파일러 최적화에 의해 COW 일어나지 않음
