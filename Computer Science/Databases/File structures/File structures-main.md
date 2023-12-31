# File structures

## Storage Hierachy

컴퓨터의 저장 장치는 크게 세가지로 분류된다.

1. Primary storage (주 기억장치)
   - CPU가 직접 접근하는 장치로 **RAM**, **캐시** 등이 있다.
   - CPU가 직접 접근하는 만큼, 속도가 매우 빠르지만 비용이 비싸다는 단점이 있다.
2. Secondary storage (보조 기억장치)
   - HDD, Flask 메모리 등
   - 속도는 느리지만 대용량이다.
   - **DB의 레코드는 보조 기억장치에 저장된다.**
3. Tetiary storage (3차 저장장치)
   - CD-ROM, DVD 등 Offline 상태를 가질 수 있는 저장장치를 의미한다.

DB의 레코드는 보조 기억장치에 저장되고, 이것을 어떻게 저장하느냐에 따라 접근에 걸리는 시간이 결정된다. 도서관에 갔을 때 내가 읽을 책의 종류와 장르에 따라 구분된 것처럼 레코드 또한 물리적으로 잘 정돈해놓아야 추후 꺼내 쓸 때 빠르게 꺼내 쓸 수 있는 것이다.

## Primary File Organizations (주 파일 기법)

주 파일 기법이란, 레코드의 물리적 저장 방식 기법을 의미한다. 주 파일 기법에는 다음과 같은 방법이 존재한다.

1. [Heap file (Unordered file) 기법](./Heap%20file.md)
   - 데이터가 추가되는 순서대로 저장하는 방식
2. [Sorted file (Sequential file) 기법](./Ordered%20file.md)
   - 데이터를 정렬하여 저장하는 방식
   - 데이터를 정렬할 땐 정렬 기준이 필요한데 이를 `sort key`라고 칭함.
3. [Hashed file 기법](./Hashed%20file.md)
   - Hash를 이용하여 저장 위치를 정하는 기법

이 외에도, 주 파일 기법에는 트리 구조를 사용한 기법 등이 존재한다.

## RAID

[RAID](./Raid.md)
