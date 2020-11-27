# Indexing

## Contents

1. [Basic Concepts][link1]
2. [Ordered Indicies][link2]
3. [B+ Tree Index Files][link3]
4. [B+ Tree Extensions][link4]
   
## 1. Basic Concepts

* indexing 은 더 빠르게 원하는 데이터를 찾기 위해 사용된다.

* <details><summary>더보기</summary>

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

* 정렬된 상태의 인덱스 구조로 저장하는 방법.
  
* <details><summary>Dense and Sparse Indices</summary>

  - Dense index
  
  - Sparse index
</details>

* <details><summary>Multilevel Indices</summary>

</details>

* <details><summary>Index Update</summary>

</details>

* <details><summary>Secondary Indices</summary>

</details>

* <details><summary>Indices on Multiple keys</summary>

</details>

## 3. B+ Tree Index Files

* <details><summary>b+ tree</summary>
  
  - index sequential file 은 파일이 커지면 커질 수록 degrade 된다.
  - 파일을 재구성 하면 되지만 빈번한 재구성은 좋지 못해
  - b+ tree 구조는 인덱스 구조로 널리 쓰인다.
  - root -> leaf 까지 모든 path 길이가 같다.
  - insertion 과 deletion 에 performance overhead 둔다
  - overhead 는 잦은 파일 수정 있을 때 좋다. (파일 재구성 비용 피할 수 있어서)
  - 노드들 반은 비어있다. -> 낭비되는 공간 있다.
  - 하지만 performance 때문에 참자
</details>

* <details><summary>Structure of a B+ Tree</summary>

  - B+ tree index 는 multilevel index 이지만 index-sequential file 은 아니다.
  - 일단 search key 중복 없다고 가정
  - 일반적인 Node 구조: P1 | K1 | P2 | K2 ... | K(n-1) | P(n)
  - leaf 구조: value 개수가 [(n - 1)/2] ~ (n - 1) 개 가질 수 있다. 최소 [(n - 1)/2] 개
  - B+ Tree 가 dense index 로 사용되면 leaf node 에 모든 search key 가 다 있을 것이다.
  - P(n) leaf node 의 n 번째 Pointer: 다음 leaf node 를 가리킨다. => sequential processing 효율적으로 하게
  - internal node 는 multilevel index 를 형성한다.
  - internal node 는 leaf node 와 같은데 다른 점은 leaf node 는 data 를 가리키는데 none leaf node 는 node 를 가리킨다.
  - internal node 는 [n / 2] ~ n 개의 pointer 를 갖고 있다. leaf node 는 p(n) 때문에 다른가 보다.
  - 
</details>

## 4. B+ Tree Extensions

[link1]: #user-content-1-basic-concepts
[link2]: #user-content-2-ordered-indicies
[link3]: #user-content-3-b-tree-index-files
[link4]: #user-content-4-b-tree-extensions
