# 🚀 Project Experience

---

### Place IR Encoder for AI briefing
**2025** | `In-domain Retriever` `Information Retrieval` `MRL` `XTR` `Multi-node Training` `DeepSpeed` `DPO`

> **네이버 플레이스 AI 브리핑 품질 및 In-domain Recall 향상**

**주요 성과**
* **도메인 특화 Retrieval 모델 학습:** 음식점(식당, 카페), 숙박시설 도메인에서 AI 브리핑(요약문) 작성을 위한 검색 모델 학습
* **검색 성능 향상:** In-domain Recall을 **0.39에서 0.65로 대폭 개선**하여 AI 브리핑 콘텐츠 다양성 확보
* **도메인 특화 데이터셋 최초 생성:** 플레이스 관점에서 함축적 의미까지 포함한 query-document pair 데이터셋 3.85M 생성
* **기술 공유:** **NAVER DAN 25** 컨퍼런스 직접 발표 및 기술 공유
* **추가 실험:** 효율적인 고도화를 위한 학습 방법론 실험(Reinforced IR)

**기술 키워드**
* **Retrieval Efficiency:** **MRL(Matryoshka Representation Learning)** 및 **XTR** 도입으로 임베딩 차원 축소(1024 -> 64) 및 검색 효율 최적화
* **in-domain Dataset Building:** 소수 핵심 쿼리에서 다양한 문서 확보를 위한 데이터셋 구축 방법론 고안(Retrieval-base -> Labeling-base)
* **Reinforced IR:** Query/Doc Generation 및 DPO Training을 IR 데이터 생성에 적용하여 in-domain 선호 문서를 주입하려고 노력
* **Scalable Training:** Ray Train과 Lightning 기반의 Multi-node Cluster 학습, Cross-Device Loss, DeepSpeed 도입으로 in-batch negative 극대화

---

### Place IR Encoder for search & QnA subagent
**2026. 02 -** | `LLM` `Data Generation` `Query Engineering`

> **Agentic Query에 대응되는 Retrieval 데이터셋 구축 및 학습**

**주요 성과**
* **2개 도메인 Agentic Query IR encoder 학습:** 식당, 미용실 도메인용 검색기 학습
* **Agentic Query 분리:** 생성된 예상 질문에서 rewrite된 쿼리들을 대상으로 빈도별/클러스터별/의미상 Dedup 적용하여 학습, 평가셋 쿼리 구축
* **Query-Doc pair dataset:** 식당 2M, 미용실 1.5M 수준 positive, hard negative, negative로 구성된 Retrieval 학습용 데이터셋 생성
* **고퀄리티 문서 생성:** 생성 -> 검증 파이프라인을 통한 퀄리티 좋은 positive, hard negative 문서 생성(prompt engineering)

**기술 키워드**
* **Data Generation:** Syntriever 방법론 도입하여 positive, hard negative 문서 생성 및 reranking용 데이터셋 생성
* **LLM / Prompt Engineering:** 데이터 생성, pair 검증, 문서 간 ranking 등의 모든 데이터셋 생성 작업에 multi-process LLM generation 작업 
---

### Planner for Place Agent System
**2026. 04 -** | `Agent` `LLM` `Design`

> **in-domain agent system 내부에서 routing 해줄 수 있는 planner 설계(진행 중)**

**주요 성과**
* **업종 분류:** 발화와 history, slot table 기준으로 업종(vertical) 분류
* **Query Rewrite:** 각 액션(QnA, Search, Reservation)에 적합한 형태로 query rewrite (진행 중)
* **Soft OOD:** in-domain local agent에서 하지 못하는 작업에 대한 ood 판별

**기술 키워드**
* **시스템 설계:** Search Agent, QnA Agent, Reserve Agent, Dialogue Agent 등 인접 agent 개발자와 함께 모듈 별 R&R 조율 및 데이터 흐름, 스키마 설계

---

### 플레이스 속성 기반 리뷰 분석(Aspect-Based Sentiment Analysis)
**2025. 12 - 2026. 01** | `LLM` `SFT` `AI judgment` `Efficiency`

> **0.4B 소형 encoder로 GPT-5급 성능 달성, 195가지 속성에 대한 복합 감성 분석(`가격-긍정`, `좌석간격-부정` 동시 도출)과 연관 부분 탐지**

**주요 성과**
* **High Efficiency:** 0.4B 규모의 모델로 GPT-5.2와 대등한 Accuracy 달성, 0.79(GPT-5.2) -> 0.79(hyperCLOVA X 1.5B SFT) -> 0.8(ModernBERT 0.4B FT)
* **사업자 대시보드 인사이트 도출:** 플레이스 리뷰 내 195가지 속성(Aspect)에 대한 감성 및 연관 구절 도출 & 하이라이팅
* **in-domain 데이터셋 구축:** 리뷰 속 속성별 감성, 속성과 연관된 부분을 가지고 있는 노블 데이터셋 구축 & 축적

**기술 키워드**
* **Model Tuning:** **HyperCLOVA X** 기반의 Decoder Instruction SFT
* **Efficiency:** LoRA, Special Token(`|aspect|`, `|positive|` 등) 도입으로 학습/추론 효율 극대화
* **Inference Optimization:** Full generation 방식과 단일 토큰 Logit 확률값 활용 방식 비교 실험을 통한 서빙 비용 최적화
* **Data/Prompt Engineering:** 소수의 골든셋 -> 최적 zeroshot prompt engineering -> 학습셋 구축
* **AI Judgment:** 테스트셋 Label이 없는 Span Detection 성능 평가

---

### 플레이스(in-domain) 멀티모달 이미지 검색기
**2024. 09 - 2024. 12** | `Multi-modal Encoder` `CLIP` `Continual Learning`

> **도메인별/목적별 special token 도입하여 in-domain 검색 의도에 최적화된 이미지 검색 시스템 구축**

**주요 성과**
* **노출 품질 개선:** 식당/여행지 플레이스탭 리뷰 이미지 노출시 쿼리 의도(풍경 vs 접사)에 맞는 최적의 이미지 우선 노출
* **Global Expansion:** 해외 플레이스탭 대응을 위한 블로그 이미지 노출 성능 향상

**기술 키워드**
* **2-Stage Training:** Embedding Space Align(Stage 1) 후 Service-specific High Quality Retrieval 학습(Stage 2) 전략 수립, stage2 직접 학습
* **Intent-aware Search:** 카테고리별 Special Token 도입으로 텍스트 검색 시 도메인 의도에 맞는 이미지 랭킹 정교화(풍경 노출 우선, 음식 접사 우선 등 도메인별 다른 점수 체계)
* **Catastrophic Forgetting 방지:** **DAS 기법**을 적용하여 신규 데이터 학습 시에도 기존 성능을 유지하는 Continual Learning 구현
* **Multi-Modal Encoder:** CLIP-based pretrained model의 multi-modal space align 학습
* **기술 공유:** NAVER DAN 24 세션에 special token 도입 내용 포함

---

### 플레이스 이미지 통합 분류 모델 (VLM)
**2023** | `VLM` `SFT` `PEFT` `Dataset Distillation` `Multi-Node Training`

> **여러 Encoder로 가져가던 이미지 분류 태스크를 하나의 Decoder 모델로 통합 및 효율화**

**주요 성과**
* **Pipeline Unification:** 흩어져 있던 7개의 모델을 하나의 통합 모델로 교체하여 관리 및 서빙 효율 극대화 (음식 분류, 분위기 분류, 이미지 퀄리티, 영수증 분류, 숙박 이미지 분류 등)

**기술 키워드**
* **VLM Exploration:** LLaVA, mPLUG-Owl 등 **VLM 기반의 Instruction SFT**를 통한 다중 태스크 처리
* **데이터셋 경량화:** Embedding Clustering 후 Centroid 기반 샘플링을 통해 데이터셋 경량화와 다양성 확보. 20장의 이미지로 한 클래스 구성

---

### 플레이스(in-domain) 이미지 품질 점수 측정기
**2022** | `Knowledge Distillation` `Image Classification`

> **모델 경량화를 통한 리소스 절감 사례 PyTorch 공식 블로그 기재**

**주요 성과**
* **Infrastructure Optimization:** 모델 사이즈 **94% 축소 (170M → 10M)** 및 추론 리소스 대폭 절감(CPU serving)
* **Model Performance:** 0-5점 척도 Regression 기준 **MAE 0.3 후반**, Classification 기준 **Acc. 99%** 달성. 플레이스 사용자 관점에서 보기 좋은 이미지 상위 노출

**기술 키워드**
* **Knowledge Distillation:** CLIP Vision Encoder의 지식을 EfficientNet으로 증류하여 모바일/에지 환경 최적화. inter class는 멀게 / intra class는 가깝게 위치 시키는 방법론 도입
* **Collaboration:** Intel과의 협업을 통해 [PyTorch 공식 블로그](https://pytorch.org/blog/ml-model-server-resource-saving/)에 리소스 절감 사례 기재
* **FrontEnd Tools:** Streamlit 기반 데모 페이지를 구축하여 기획자 및 유관 부서의 즉각적인 모델 검증 지원

---

### Chat2Order (개인 프로젝트)
**2026. 03 -** | `LLM` `Structured Output` `Streamlit` `Gemini API` `Pydantic` `Supabase`

> **소규모 판매자를 위한 메신저 주문 대화를 자동으로 분석해 엑셀 주문서로 변환하는 서비스**

**주요 성과**
* **주문 정보 자동 추출:** 판매자-고객 간 채팅 내역과 상품 카탈로그를 LLM에 입력하여 주문자·상품·수량·연락처 등 핵심 정보를 구조화된 형태로 추출
* **End-to-End 파이프라인 구축:** 채팅 파싱 → Gemini Structured Output(Pydantic 스키마) → 후처리(전화번호 정규화, 도로명 주소 API 우편번호 조회) → DB 저장 → 엑셀 다운로드 전 과정 직접 구현
* **서비스 기능 다양화:** 카탈로그 JSON 자동 생성(재고 CSV 업로드), 최근 5회 추출 이력 조회 및 재다운로드 기능 제공

**기술 키워드**
* **Structured Output:** Pydantic 스키마 기반 LLM 응답 강제 구조화로 파싱 안정성 확보
* **Gemini API:** Google GenAI SDK를 활용한 멀티턴 채팅 분석 및 JSON 모드 응답 처리
* **Backend:** Supabase를 통한 사용자 인증 및 추출 이력 저장
* **Frontend:** Streamlit으로 업로드·결과 확인·다운로드 통합 UI 
* **1인 개발:** 니즈 파악에서 시작하여 기획, 페이지 설계, 백/프론트 구현, prompt engineering, 유지 보수를 AI와 함께 진행

----

# References

- linkedin: https://www.linkedin.com/in/junho-m-60a1831b7/
- paper studies: https://github.com/jonas-jun/AiPapers/issues
- Chat2Order(side project): https://chat2order.streamlit.app/ (화면 확인을 위한 account `testuser / testuser123!`) https://github.com/jonas-jun/chat2order
- NAVER DAN 25 진행 세션: https://dan.naver.com/25/sessions/691