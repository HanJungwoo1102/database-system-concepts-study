# Indexing

## Contents

1. [Basic Concepts][link1]
2. [Ordered Indicies][link2]
3. [B+ Tree Index Files][link3]
4. [B+ Tree Extensions][link4]
   
## 1. Basic Concepts

* 왜??
  - 원하는 데이터를 더 빨리 찾기 위해 사용된다.
* index file 구성 요소
  - search key, pointer(value) 로 구성된 record (index entry) 들로 이루어져있다.
* index file 종류
  - index entries 가 어떻게 저장되어 있는지에 따라서 나뉜다.
  - 종류
    - ordered indices
    - hash indices
* index 평가 항목
  - 데이터에 할 수 있는 타입이 효과적인가 (ex: point query, range query)(범위로 찾기, 값으로 찾기)
  - access time
  - insertion time
  - delete time
  - space overhead

## 2. Ordered Indicies

* index file 안에 entry 들을 search key 로 정렬시켜놓습니다. (hash 는 정렬 X)
* index entries 가 정렬되긴 하는데 실제 data 순서에 따라서 2가지로 나뉩니다.
  - clustered index
    - 데이터들의 순서대로 index entry 저장
    - ex: 책 앞에 목차 (index 순서대로 책의 내용 전개)
  - nonclustered index
    - 데이터들의 순서와는 다르게 index entry 저장
    - ex: 책 뒤에 찾아보기 (index 순서와는 다르게 책의 내용 전개)
* ordered indices 구조들
  * <details><summary>Dense and Sparse Indices</summary>

    - Dense index
      - file 의 모든 search key, value 에 대해서 index record 를 가지고 있다.
      - file 의 모든 record 들에 대해서 갖고 있는게 아니었군
    - Sparse index
      - file 의 모든 search key, value 에 대해서 index record 갖지 않는다.
      - 어떻게 쓰나??
        - 접근 원하는 data 를 index entry 와 비교해서 근처 찾고 포인터 가리키는 곳 부터 시작해서 쭉 찾아간다.
        - 그래서 data 가 순서대로 정렬된 곳에서 사용가능하다. (sequentially ordered)
        - 어라 그냥 dense index 도 마찬가지인데 왜 모든 search key, value 에 대해서 indexing record 를 안 갖게하지??
        - index record 의 수를 줄이고 싶어서??
        - 뒤에 나올 multi level index 때문인듯 싶다. 상위 level 의 index file 들은 모든 search key, value 에 대해서 index record 를 갖는것은 아니니까??
  </details>

  * <details><summary>Secondary Indices</summary>

    - index records, 실제 데이터 record 들의 pointer 를 담고 있는 buckets, 실제 데이터들로 구성
    - index record 의 pointer 는 bucket 을 가리키고 bucket 의 포인터는 실제 record 을 가리킨다.
    - bucket 들은 모든 record 들을 다 가리키고 있어야 한다.
    - dense indices 여야 한다.
  </details>

  * <details><summary>Multilevel Indices</summary>

    - data 너무 많아서 index 만들었는데 index record 들도 너무 많아진다??
    - index 를 위한 index
    - inner index: 그냥 basic index file
    - outer index: inner index 에 대한 index file
  </details>

  * B+ tree Indices (다음 절에서 더 자세히)
  

## 3. B+ Tree Index Files

* <details><summary>B+ tree</summary>
  
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
  - internal node 중 최상위 노드인 root node 는 [ n / 2 ] 보다 적은 pointer 가질 수 있다.
  - 하지만 반드시 적어도 포인터 두개 이상 가져야한다. (tree node 가 한 개 일때는 어쩔 수 없지만)
  - b+ tree 는 balance 하다. -> path length 다 같다 -> lookup, insertion. deltion 성능 굿
  - 일반적으로 search key 는 중복됨 -> search key 나오는 만큼 leaf node 에 저장 -> internal node 에 중복된 search key 생김 -> 성능 문제
  - 다른 방법은 각각의 search key 에 대한 pointer set 를 저장하는 방법 -> 더 복잡, 비효율적
  - 대부분의 db 들은 search key 를 unique 하게 만들어놓는다. primary key 와 묶어서 key 를 만든다 -> 그럼 프라이멀 키 때문에 unique 해진다.
  - 디비에서 자동적으로 extra attribute 를 내부적으로 붙인다고 한다. -> 중복을 제거하기 위해
</details>

* <details><summary>Queries on B+ Tree</summary>

  - find(v)
    1. root node 에서 시작
    2. 노드에서 제일 작은 key 부터 비교 시작 -> v 보다 큰 애들 중에 제일 작은 애 찾기
    3. 다음 노드로 넘어가기
      - 다 v 보다 작아서 못 찾았다. -> P(m) m 은 last non null pointer
      - 찾았는데 그 값이 v 랑 같다 -> P(i+1)
      - 찾음 -> P(i)
    4. leaf node 까지 반복
    5. leaf node 라면 key 중에 v 랑 같은게 있으면 return P(i) 없으면 null return
  - findRange(lb, ub)
    1. find(lb) 랑 마지막 leaf node 구하는 데까지 로직 같다.
    2. leaf node 찾았으면 Key 중에 lb 보다 큰 애들 중에 제일 작은 애 index 찾는다. 없으면 index 를 그 leaf node 갯수 + 1 로 설정 (다음 노드로 넘어가려고)
    3. lb, ub 사이에 있는 key 찾기
      - i 가 현재 leaf node 수 보다 작고 key 가 ub 보다 작으면 resultSet 에 넣고 i += 1
      - i 가 현재 leaf node 수 보다 작은데 key 가 ub 보다 크면 다 찾은거니 끝내기
      - i 가 현재 Leaf node 수 보다 크면 현재 leaf node 에서는 다 찾은겨. 다음 leaf 노드로 넘어가기 위해 다음 leaf node 있는지 검사하고 C = C.P(n+1) 하고 i = 1 로
      - 그 외는 다 찾은 거니 끝내기
    - resultSet return
  - insert(K, P)
    1. 넣어야 할 leaf node 찾기
    2. leaf node 꽉 차있는지 확인
      - 넣을 공간 있으면 넣기
      - 꽉 차 있으면 split
    3. split
       1. 새 노드 만들기
       2. 원래 노드에 있던 P(1) K(1) ... K(n-1) 을 T 에 저장해 놓기
       3. T 에 새로 추가되는 K, P 를 맞는 위치에 넣는다.
       4. 원래 leaf node 비운다
       5. 원래 leaf node 에 P(1) ~ K([n / 2])
       6. 새 leaf node 에 P([n / 2] + 1) ~ K(n)
       7. 원래 leaf node 부모에 새 leaf node 를 붙인다. key 는 새 leaf node 에서 제일 작은 key 로
</details>

## 4. B+ Tree Extensions

## 5. Hash Indices

* hash data structure 를 이용해 index record 를 저장한다.

* <details><summary>static hashing</summary>

  - bucket 수 정해져있다. => overflow 발생
  - record 분포가 불균형하다.
  - overflow handling
    - overflow bucket (closed addressing or closed hashing)
      - linked list 로 bucket 연결
    - linear probing
      - 다음 버킷 그냥 쓰기
  - 버킷 수 많으면 disc space 낭비, 적으면 overflow 으로 인한 성능 저하
  - solution
    - rehashing
      - 새 hash 함수와 파일 re-organization
      - expensive 하고 다른 operation 방해
    - dynamic hashing
      - 아래서 계속
  </details>

* dynamic hashing
  - bucket 수가 동적으로 변하는 hash 구조
  - <details><summary>linear hashing</summary>

    - 
  </details>
  - <details><summary>extendible hashing</summary>

    - 
  </details>

* ordered index 와 비교

## 6. Multiple Key Access

## 7. Creation of Indices

## 8. Write-Optimized Index Structures

## 9. Bitmap Indices

## 10. Indexing of Spatial and Temporal Data

[link1]: #user-content-1-basic-concepts
[link2]: #user-content-2-ordered-indicies
[link3]: #user-content-3-b-tree-index-files
[link4]: #user-content-4-b-tree-extensions
