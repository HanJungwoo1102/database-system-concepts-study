# Indexing

## Contents

1. [Basic Concepts][link1]
2. [Ordered Indicies][link2]
3. [B+ Tree Index Files][link3]
4. [B+ Tree Extensions][link4]
   
## 1. Basic Concepts

* indexing 은 더 빠르게 원하는 데이터를 찾기 위해 사용된다.

* <details>
  <summary>더보기</summary>

  - 도서관의 책 인덱싱, 책의 목차와 같은 방식이다.
  - 인덱스 없다면 겨우 약간의 데이터를 찾기 위해서 전 데이터를 싹 훑어야 한다.
  - 인덱싱을 막 하면은 안되 부러. 여러가지 테크닉이 있는디 그 중에 맞는거 잘 써야혀
  - 크게 indexing 하는 방법은 2가지 있다.
    1. ordered indices: 정렬된 순서
    2. hash indices: hash function 에 의해서 분포
  - indexing technique 을 평가하는 5가지 요소
    1. Access types: ?? 어떤 value 로 찾는지인가
    2. Access time: 특정 데이터를 찾는데 걸리는 시간
    3. Insertion time: 새 데이터 넣는데 걸리는 시간 (제 자리 찾아서 넣고 index 구조 변경하는데 까지)
    4. Deletion time: 특정 데이터 지우고 구조 바꾸는 데까지
    5. Space overhead: ?? index 들이 얼마나 차지하는지인가
  - searh key: 검색할 attribute 들. 그러니까 과목 인덱싱 해 놨으면 과목이 search key 인거지
</details>

## 2. Ordered Indicies

## 3. B+ Tree Index Files

## 4. B+ Tree Extensions

[link1]: #user-content-1-basic-concepts
[link2]: #user-content-2-ordered-indicies
[link3]: #user-content-3-b-tree-index-files
[link4]: #user-content-4-b-tree-extensions
