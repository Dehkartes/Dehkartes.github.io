---
title: "Ontology / Knowledge Graph / Graph DB"
excerpt: "Ontology, Knowledge Graph, Graph DB의 역할과 차이를 간단히 정리한다."
categories:
  - Graph
tags:
  - ontology
  - knowledge-graph
  - graph-database
---

Ontology, Knowledge Graph, Graph DB는 모두 데이터를 **그래프 형태로 다룬다**는 공통점이 있다. 하지만 각각이 답하는 질문은 다르다.

```text
Ontology         무엇을 어떤 의미와 규칙으로 표현할 것인가?
Knowledge Graph  실제로 어떤 지식이 연결되어 있는가?
Graph DB         그 연결을 어떻게 저장하고 탐색할 것인가?
```

# 1. Ontology

Ontology(온톨로지)는 특정 도메인의 개념과 관계를 정의한 **의미 모델**이다.

예를 들어 회사와 문서를 다루는 시스템이라면 다음과 같은 내용을 정의할 수 있다.

- `Company`와 `Document`는 서로 다른 Class다.
- `Samsung`은 `Company`의 Instance다.
- `Document`는 `MENTIONS` 관계를 통해 `Company`를 언급할 수 있다.
- `foundedAt`은 회사의 설립일을 나타내는 Property다.

```text
(Document)-[:MENTIONS]->(Company)
```

Ontology는 데이터 자체보다 **데이터가 가져야 하는 의미와 규칙**에 초점을 둔다. 따라서 다음과 같은 질문에 답한다.

- 어떤 대상을 Node로 만들 것인가?
- Class와 Instance를 어떻게 구분할 것인가?
- 어떤 관계를 허용할 것인가?
- 각 Property에는 어떤 값이 들어갈 수 있는가?
- 기존 사실로부터 어떤 새로운 관계를 추론할 수 있는가?

RDF Schema나 OWL은 이러한 모델과 규칙을 표현하는 대표적인 방법이다.

# 2. Knowledge Graph

Knowledge Graph(지식 그래프)는 Entity와 Relation을 연결하여 만든 **실제 지식 데이터**다.

Ontology가 설계도라면 Knowledge Graph는 그 설계도에 실제 데이터를 채운 결과에 가깝다.

```text
(보고서A)-[:MENTIONS]->(삼성전자)
(삼성전자)-[:LOCATED_IN]->(대한민국)
(삼성전자)-[:INDUSTRY]->(반도체)
```

여기서 `보고서A`, `삼성전자`, `대한민국`, `반도체`는 Entity이고, `MENTIONS`, `LOCATED_IN`, `INDUSTRY`는 Entity 사이의 의미 있는 Relation이다.

Knowledge Graph의 핵심은 단순히 데이터가 연결되어 있다는 데 있지 않다. 각 연결이 **무엇을 의미하는지 해석할 수 있어야 한다**는 점이 중요하다.

다만 모든 Knowledge Graph가 반드시 정교한 Ontology를 사용하는 것은 아니다. 작은 시스템에서는 간단한 스키마나 암묵적인 규칙만으로 지식 그래프를 구성하기도 한다.

# 3. Graph DB

Graph DB는 Node와 Edge를 저장하고 탐색하는 데 최적화된 **데이터베이스 시스템**이다.

관계형 데이터베이스에서는 여러 테이블의 관계를 조회할 때 JOIN을 사용한다.

```text
User
  ↓ JOIN
Order
  ↓ JOIN
Product
```

Graph DB에서는 관계를 직접 연결하고 그 경로를 따라 탐색한다.

```text
(User)-[:ORDERED]->(Order)-[:CONTAINS]->(Product)
```

따라서 Graph DB는 다음과 같은 작업에 적합하다.

- 특정 Entity와 연결된 데이터 찾기
- 여러 단계의 관계 탐색하기
- 최단 경로 찾기
- 추천, 이상 거래 탐지, 네트워크 분석 수행하기

대표적으로 Neo4j 같은 Property Graph DB와 RDF 데이터를 저장하는 Triple Store가 있다.

Graph DB는 데이터를 저장하는 기술이므로 그 자체가 Ontology나 Knowledge Graph를 의미하지는 않는다. 소셜 네트워크나 배송 경로처럼 지식 모델이 아닌 그래프도 Graph DB에 저장할 수 있다.

# 4. 세 개념의 관계

세 개념을 하나의 시스템에 적용하면 다음과 같은 흐름이 된다.

```text
Ontology
무엇을 그래프로 표현할지 정의한다.
        ↓
Knowledge Graph
정의에 따라 실제 Entity와 Relation을 연결한다.
        ↓
Graph DB
그래프를 저장하고 빠르게 탐색할 수 있게 한다.
```

| 구분 | 핵심 역할 | 비유 |
|---|---|---|
| Ontology | 개념, 관계, 규칙 정의 | 설계도 |
| Knowledge Graph | 실제 지식과 연결 표현 | 설계도로 만든 지도 |
| Graph DB | 그래프의 저장과 조회 | 지도를 보관하고 검색하는 시스템 |

이 셋은 함께 사용되는 경우가 많지만 항상 한 묶음은 아니다.

- Ontology는 Graph DB 없이 문서나 RDF 파일로만 존재할 수 있다.
- Knowledge Graph는 엄격한 Ontology 없이 구성될 수 있다.
- Graph DB에는 Knowledge Graph가 아닌 일반 그래프 데이터도 저장할 수 있다.

# 5. 정리

가장 간단하게 구분하면 다음과 같다.

> Ontology: **의미와 규칙**  
> Knowledge Graph: **실제 지식의 연결**  
> Graph DB는 **저장과 탐색 기술**  

실무에서는 Graph DB 제품부터 선택하기보다 먼저 어떤 Entity를 만들고 어떤 Relation으로 연결할지 결정해야 한다. 이 모델링이 불명확하면 같은 Entity가 중복되거나 불필요한 Edge가 계속 늘어나고, 결국 탐색 범위와 성능에도 영향을 준다.
