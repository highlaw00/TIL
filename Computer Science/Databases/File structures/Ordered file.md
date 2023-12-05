### Sorted Files

대부분의 파일 구조가 이 파일 구조를 택하고 있다.
`Ordering key`를 정해 해당 키를 기준으로 레코드를 정렬해 저장한다.

`Sorted Files`의 이점

- 레코드를 순서대로 읽는 것이 매우 쉽다. (정렬이 필요 없음)
- 정렬된 레코드이기 때문에 이분 탐색을 사용한 접근이 가능하다.

`Sorted Files`의 삽입과 삭제

- `Sorted Files`는 반드시 정렬된 상태를 유지해야하기 때문에 삽입과 삭제의 비용이 크다.
- 삽입의 경우 삽입될 위치의 레코드를 한칸씩 뒤로 밀어야 된다는 점.
  - 이것을 방지하고자, **블럭의 공간을 임의로 비워두는 전략**을 사용한다.
- 삭제의 경우엔 `deletion marker`를 사용해 비용을 절감할 수 있다.

`Transaction File` 도입 및 사용

- `Sorted Files`에서 수정될 때의 문제를 해결하기 위해 도입된 방법
- 수정될 내용을 임시로 저장하는 `Transaction File`(`overflow file`)을 선언해 사용
- 삽입 시... `TF`에 순서대로 삽입
- 삭제 시... `deletion mark`를 `main file`에 기록
- 수정(Key 값이 바뀌어 순서가 바뀌는 경우)... 해당 레코드를 지우고, 새 레코드 `TF`에 삽입
- 수정(Key가 아닌 값이 바뀌는 경우) ... 바로 수정하여 파일에 적용
- 탐색 시... `main file`을 우선적으로 탐색. 없다면 `TF`를 탐색
- `Transaction File`이 충분히 커지는 경우, 탐색 등의 연산 효율성을 위해 `reorganization` 과정을 거쳐야한다.
  - `TF`를 정렬한 뒤, `main file`과 병합하여 `reorganized file`을 생성.
  - `TF`와 `main file`은 백업 파일이 된다.
