# Vectorless RAG 쿡북 with PageIndex
#### 임베딩 한계를 넘어, 구조와 추론 기반의 Vectorless RAG 구현 가이드 

<img src="https://beat-by-wire.gitbook.io/beat-by-wire/~gitbook/image?url=https%3A%2F%2F3055094660-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYzxz4QeW9UTrhrpWwKiQ%252Fuploads%252FmMOG165PFS46vuBhJRMn%252FVectorless-RAG-Cover.png%3Falt%3Dmedia%26token%3D0fc6a0c4-70c7-4bc0-a308-35dd01b4fe5b&width=300&dpr=3&quality=100&sign=d40f32fd&sv=2" width="500" height="707"/>


## 책 소개

RAG를 진지하게 다뤄본 사람들이 공통적으로 경험하는 순간이 있다. PDF 한 권을 청킹하고 임베딩하여 벡터 DB에 적재하고, RAG 파이프라인으로 첫 답변을 받는 순간이다. 그 답변은 그럴듯하다. 문장은 자연스럽고, 인용도 그럴듯하며, 모델은 자신 있게 말한다. 그러나 며칠 뒤 "2025 Q3 iPhone 매출이 얼마인가"라는 질문에 시스템이 2025 Q2의 수치를 가져다 답하는 것을 발견한다. 오류 로그도, 경고도 없다. 그저 매끄럽게 잘못된 답을 내놓을 뿐이다.

이것이 vector RAG가 가진 가장 위험한 실패 모드다. 임베딩은 의미적으로 가까운 청크를 잘 찾지만, "Q3 2025"와 "Q2 2026"는 단어가 거의 같다. 청킹 경계가 표의 헤더와 데이터를 갈라놓으면 어떤 reranker도 그 표를 복구하지 못한다. 5개 이상의 엔티티가 등장하는 멀티홉 질문에서는 정확도가 0%로 무너진다는 Diffbot의 보고도 있다. 우리는 임베딩 모델을 더 큰 것으로 바꾸고, 청크 크기를 조정하고, Hybrid 검색과 Cohere Rerank를 붙이지만, 어느 시점에서 깨닫게 된다. 이것은 매개변수의 문제가 아니라 retrieval primitive의 문제다. 본 책은 그 깨달음에서 출발한다.

2025년 9월 VectifyAI가 공개한 PageIndex와, 그 위에 구축된 Mafin 2.5가 FinanceBench에서 98.7% 정확도를 기록한 사건은 단순한 벤치마크 갱신이 아니다. "벡터 유사도가 아닌, 문서 구조와 LLM 추론으로 답을 찾는다"는 패러다임 전환이다. 이러한 높은 성능의 이유는 RAG 임베딩 모델이 아니라 Vector 방식을 없앤 retrieval의 작동 방식 자체를 바꾼 것이다.

현재 가장 많이 활용 중인 Vector RAG 단독으로는 production-grade 정확도가 어렵고, Vectorless RAG 단독으로는 비용·지연이 부담스럽다. 트리 인덱스 · LLM 추론 네비게이션 · Verifier · 라우터가 정교하게 조합되어야 비로소 production에서 동작한다.

RAG 라이브러리의 함수 이름을 아는 것과, 그 작동 원리를 손으로 직접 이해하고 자신의 도메인에 결합해본 것은 전혀 다른 역량이다. 본 책을 마칠 때쯤이면 새로운 RAG 논문·블로그를 읽고 그 설계 원리를 꿰뚫어 볼 수 있는 눈과, 자신의 PDF·법무 계약·재무 보고서에 Vectorless RAG를 직접 결합할 수 있는 토대가 만들어져 있을 것이다.


## 목 차

저자 소개

Table of Contents (목차)

서문: 들어가며

프롤로그: Quantum Computing — 수학적 기초

Chapter 1 — RAG를 위한 Document Parser

- 1.1 RAG에서 파서가 정확도를 결정짓는 이유
- 1.2 파서의 세 가지 패러다임
- 1.3 규칙 기반 파서 — PyMuPDF4LLM과 Unstructured
- 1.4 VLM·레이아웃 기반 파서 — Docling과 LlamaParse
- 1.5 LLM 추론 기반 파서 — Structured Output
- 1.6 LLM 추론 기반 파서 — 트리 인덱스
- 1.7 한국어 특화 이슈
- 1.8 2026년 도구 비교 매트릭스
- 1.9 RAG를 위한 Document Parser Python 구현

Chapter 2 — VectorRAG

- 2.1 VectorRAG의 표준 6단계 파이프라인
- 2.2 임베딩 모델 선택 (2026-05)
- 2.3 벡터 데이터베이스 5종 비교
- 2.4 청킹 전략 7가지
- 2.5 하이브리드 검색과 리랭킹
- 2.6 컨텍스트 프롬프트와 LLM 생성
- 2.7 VectorRAG가 구조적으로 실패하는 5가지 시나리오
- 2.8 Vector RAG Python 구현

Chapter 3 — VectorlessRAG: 이론과 PageIndex 개념

- 3.1 Vectorless의 정의와 오해
- 3.2 PageIndex 트리 인덱스 구축
- 3.3 LLM 추론 기반 네비게이션
- 3.4 Mafin 2.5 사례 분석 — FinanceBench 98.7%
- 3.5 한계와 트레이드오프
- 3.6 RAG 의사결정 매트릭스
- 3.7 Vectorless RAG PageIndex Python 구현

Chapter 4 — VectorlessRAG 직접 구현

- 4.1 Traditional RAG와 그 진화 — Re-ranking·Hybrid·Agentic
- 4.2 Traditional RAG의 4가지 한계 — 근본 vs 완화 가능
- 4.3 핵심 통찰 — 인간 분석가는 어떻게 문서를 읽는가
- 4.4 Step 1: Document Tree 직접 빌드 (pymupdf4llm)
- 4.5 Step 2: LangGraph Agentic Traversal
- 4.6 케이스 스터디 — Google Bigtable 논문
- 4.7 PageIndex(managed) vs 직접 구현(custom) 비교
- 4.8 프로덕션 적용 시 7가지 고려사항
- 4.9 Vectorless RAG 원리 Python 구현

Chapter 5 — Three-Stage Tree-and-Reasoning Architecture

- 5.1 왜 Vector RAG가 전문 문서에서 구조적으로 실패하는가
- 5.2 Three-Stage 아키텍처 개요
- 5.3 Stage 1: Build the Document Tree
- 5.4 Stage 2: Traverse with Reasoning
- 5.5 Stage 3: Answer from Evidence (with Verifier)
- 5.6 Tradeoffs — 언제 이 복잡성을 지불할 가치가 있는가
- 5.7 Karpathy의 LLM Wiki — 보완 패턴
- 5.8 데모 구축 가이드 — 작게 시작하기
- 5.9 Three-Stage Tree-and-Reasoning Architecture

Chapter 6 — PageIndex SDK 실전

- 6.1 왜 PageIndex SDK인가 — Chapter 4·5와의 비교
- 6.2 SDK 설치와 Cloud 사용 흐름
- 6.3 LLM Tree Search — 핵심 메커니즘 분해
- 6.4 Expert-Guided Retrieval — PageIndex의 진짜 가치 ★
- 6.5 한국어 공공문서 적용 — KEPCO 형태 자체 생성
- 6.6 PageIndex Chat API — Zero-Setup 챗봇
- 6.7 Self-hosted Open Source — 데이터 프라이버시
- 6.8 Managed vs Self-hosted 의사결정 매트릭스
- 6.9 VectorlessRAG 4-챕터 학습 흐름 회고
- 6.10 PageIndex SDK 실전 Python 구현

Chapter 7 — 최종 비교와 Adaptive RAG

- 7.1 비교 실험의 4가지 핵심 질문
- 7.2 실험 데이터셋
- 7.3 평가 메트릭 4계층
- 7.4 실험 설계 매트릭스 — 4 백엔드 × 4 데이터셋
- 7.5 Cost-Accuracy Pareto Frontier
- 7.6 Adaptive RAG 라우터 구현 — 4 백엔드 통합
- 7.7 의사결정 트리 — 결론
- 7.9 최종 비교와 Adaptive RAG Python 구현

마치며: Beyond Vectorless RAG


## E-Book 구매

- Yes24: 
- 교보문고: 
- 알라딘: 

## Github 코드: 

https://github.com/no-wave/vectorless-rag-pageindex/






