
1주차 — 온톨로지 빠르게 정리
목표는 온톨로지를 설계 관점에서 이해하고, 현재 프로젝트의 그래프 구조를 설명할 수 있게 되는 것입니다.
일차	주제	핵심
1일	온톨로지 전체 구조	Ontology / Knowledge Graph / Graph DB 차이
2일	모델링 기본	Entity, Class, Instance, Property, Relation
3일	계층과 관계	subclass, hierarchy, inheritance, domain/range
4일	RDF/Triple	Subject-Predicate-Object, RDF Graph 구조
5일	추론	명시적 관계 vs 추론된 관계, OWL 개념
6일	실무 모델링	Class vs Property vs Instance 판단, 관계 설계
7일	현재 프로젝트 분석	실제 온톨로지를 뜯어서 구조도 작성


여기서는 OWL 문법을 깊게 파는 것보다 다음 질문에 답할 수 있으면 됩니다.
우리 시스템에서 Node는 왜 이 단위로 만들어지는가?

Edge는 어떤 의미인가?

Class와 Instance를 어떻게 구분하고 있는가?

같은 Entity가 중복 생성될 가능성은 없는가?

새 데이터가 들어올 때 기존 Graph와 어떻게 연결되는가?

어떤 관계가 명시적으로 저장되고
어떤 관계가 추론되는가?
특히 마지막 두 개가 그래프가 계속 쌓이는 문제와 바로 연결됩니다.
2주차 — Graph DB
여기가 실제 목표입니다.
단순히 Neo4j 사용법 같은 걸 배우는 게 아니라,
그래프가 커질수록 왜 느려지고, 무엇을 봐야 하는지

를 중심으로 잡으면 됩니다.
일차	주제
1일	Graph DB 구조
2일	Graph 탐색 원리
3일	Query / Traversal
4일	Index / Constraint
5일	그래프 증가와 성능
6일	데이터 모델 최적화
7일	현재 프로젝트 Graph DB 진단


1. Graph DB 내부 구조
먼저 관계형 DB와 차이를 확실히 잡습니다.
RDB

User
 ↓ JOIN
Order
 ↓ JOIN
Product
vs
Graph

(User)-[:ORDERED]->(Order)-[:CONTAINS]->(Product)
그래프 DB에서 중요한 것은 결국:
Node
Edge
Property
Index
Traversal
입니다.
2. Traversal을 집중적으로 보기
성능 문제의 핵심입니다.
예를 들어:
A
├─ B
├─ C
├─ D
└─ E
에서 각 노드가 다시 10개의 노드와 연결되어 있다면
Depth 1 → 10
Depth 2 → 100
Depth 3 → 1,000
Depth 4 → 10,000
식으로 탐색 범위가 커질 수 있습니다.
그래서 다음 개념을 이해해야 합니다.
Traversal Depth
Degree
Fan-out
Path
Shortest Path
Cycle
Connected Component
특히 high-degree node / supernode가 중요합니다.
3. 그래프가 계속 쌓이는 문제
여기부터 현재 프로젝트에 직접 연결하면 됩니다.
Graph DB가 커졌다고 무조건 느려지는 건 아닙니다.
문제는 보통:
① Node 수 증가
② Edge 수 증가
③ 특정 Node의 Degree 폭증
④ 중복 Entity 증가
⑤ 관계 종류 증가
⑥ Traversal Depth 증가
⑦ 조회 범위 제한 없음
입니다.
예를 들어:
Company
  ↑
  ├── Document 1
  ├── Document 2
  ├── Document 3
  ...
  └── Document 1,000,000
처럼 하나의 Company 노드에 관계가 백만 개 붙으면 해당 노드가 supernode가 됩니다.
3주차를 한다면
여기서 바로 Graph DB 최적화로 가시면 됩니다.
1주
Ontology / Graph 모델 이해

        ↓

2주
Graph DB / Traversal 이해

        ↓

3주
Graph DB Performance
3주차에서는:
- Query Execution Plan
- Index
- Composite Index
- Unique Constraint
- Cardinality
- Traversal 제한
- Supernode 문제
- Partitioning
- Graph projection
- 중복 Node 제거
- Entity Resolution
- Edge pruning
- TTL / 오래된 관계 관리
- Archive 전략
정도를 보면 됩니다.
제가 잡는 핵심 학습 흐름은 이것입니다.
온톨로지가
"무엇을 그래프로 만들 것인가"
를 결정

        ↓

Graph DB가
"그 그래프를 어떻게 저장하고 탐색하는가"
를 결정

        ↓

Graph Modeling이
"그래프가 커져도 버틸 수 있는가"
를 결정

        ↓

Query / Index / Pruning 등이
실제 성능을 결정
따라서 1주 온톨로지 → 2주 Graph DB → 3주 Graph DB 최적화가 현재 목적에는 훨씬 적절합니다.
특히 1주차는 RDF/OWL 공부를 길게 하지 말고, 현재 프로젝트의 그래프에서 "왜 이 노드와 엣지가 존재하는가"를 설명할 수 있는 수준까지만 끌어올리는 것을 권합니다. 2주차부터는 Graph DB에 대부분의 시간을 투자하는 게 좋습니다.




  소스