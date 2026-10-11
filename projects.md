# 🚀 Project Experience

---

## 🛠️ Projects

### Planner for Place Agent System
**2026. 03 -** | `Agentic Workflow` `A2A` `Orchestration` `SFT/DPO` `Guardrail` `LLMOps`

> **네이버 플레이스 멀티 에이전트 시스템(A2A)에서 매 턴 "무엇을, 어느 에이전트가, 어떤 순서로" 수행할지 결정하는 오케스트레이션 Planner 모듈을 설계부터 모델 학습·서빙·운영까지 end-to-end 담당**

**주요 성과**
* **Planner 런타임 설계 및 구현 (A2A 오케스트레이션)**
  * OOD 판별 → 업종 분류(vertical) → 라우팅(Search / QnA / Reservation sub-agent)의 cascade 구조를 설계하고, 이후 **단일 모델이 한 번의 호출로 분기(branch)·업종·액션을 함께 결정**하는 구조로 통합
  * sub-agent 실행 결과를 보고 다음 액션 연계 또는 유저 응답 방출(emit)을 결정하는 **post-subagent 제어 루프** 설계. 한 턴 안의 멀티 액션(검색 → QnA → 예약)과 멀티 브랜치 병렬 실행 지원
  * DST(대화 상태)와 Extractor/POI Resolver 결과를 입력으로 쓰도록 설계하고, 기준 장소(anchor)·제외 장소(excluded) 같은 **장소 역할 정보를 입력 계약에 추가**해 "A 근처", "A 말고" 류 질의 대응력 향상
* **Planner 전용 모델 학습: 9차례 이상 서빙 모델 교체**
  * Gemma-4 E4B 기반 LoRA SFT (+ DPO 보정 라운드)를 v2부터 v5-10까지 결함 축 단위 데이터 라운드로 고도화
  * **Routing 정확도 0.946 → 0.975**, branch query F1 0.629 → 0.688, 예약 연계 오류 29건 → 3건, JSON 유효율 100%
  * 26B MoE 대비 E4B 비교 실험: LoRA가 fused expert/router에 붙지 않아 학습 파라미터가 절반에 그친 원인을 규명. 동일 데이터에서 성능 우위가 없음을 확인하고 **서빙 비용이 낮은 4B급 모델을 유지**
  * 업종 라벨을 9 → 13개(헤어/네일/약국 등)로 세분화하는 relabel 수행. 결정론 규칙과 LLM 판정 후 보류된 1~3%는 사람이 직접 리뷰
* **데이터셋 합성 파이프라인**
  * 규칙 골격 → LLM 표현 변주 → 결정 함수 라벨링 → 게이트 검증 방식으로 약 7.3만(train) / 6.4천(eval) 규모 데이터셋을 버전 단위(release + sha256 manifest)로 관리
  * 데이터 생성 엔진을 효율화 & 고도화 실험: Qwen3.8-27B-FP8 단일 H100 vLLM 생성
* **평가 체계 및 결함 분석**
  * 실제 쿼리를 베이스로 GPT-5.5 사용하여 상황별 평가 데이터 구축: single-turn/multi-turn, multi-action 등의 상황
  * Live DST/Gold DST 평가 파이프라인 구축
  * **정량적, AI Judge 하이브리드** routing EM, branch query F1, action sequence F1, JSON validity 등 정량 지표에 LLM-judge(OK/환각/누락/모순)를 더한 평가 파이프라인. 정량 지표가 놓친 gold 오류를 judge로 역검출 (261건 중 253건)
  * 실서비스 trace(Langfuse) 기반 결함 triage: 재현 → 원인 계층(모델 / 규칙 / Extractor / Resolver) 귀속 → 데이터·코드 수정. 오프라인 지표가 1.000인데도 라이브에서 발생하는 결함을 계층별로 분리해 해결
  * HCX, Qwen3.6/3.8, Gemma4, gpt-oss 등 모델 비교 (업종 분류 정확도 0.85 ~ 0.987)
* **프로토타입 검증 및 데모**
  * Streamlit 기반 Planner I/O 데모: 조립된 프롬프트 → 모델 raw 출력 → 규칙 후처리 → 브랜치/DST/실행 결과를 단계별로 확인하고, 모델 버전(arm)별로 비교

**에이전트 설계 관점의 강점**
* **통제 가능한 오케스트레이션:** Pydantic `extra=forbid` 계약과 순서·유일성 불변식, guided decoding, greedy(temperature 0)로 **같은 입력에 같은 결정**을 보장
* **서비스에 필요한 가드레일:** 유해·불법·자해·해외·미지원 업종 요청을 **LLM 호출 전 규칙 게이트**에서 차단하고 차단 사유를 로그로 남김. 미차단뿐 아니라 과차단(예: 상호명 오탐)도 회귀 테스트로 고정
* **평가와 재현을 위한 실험 설계:** 프롬프트 SHA와 학습 config, 서빙 모델 계보(서빙 ↔ config ↔ 실험 run)를 테스트로 고정. 서빙 릴리스마다 재현 커맨드가 포함된 실험 카드와 판정 근거를 기록
* **변경 관리(Change Management):** 스테이징 → 라이브 프로브 → 운영 순서의 단계적 배포. 롤백 확보
* **모델링 엔지니어 관점에서의 장애 대응:** 외부 LLM 호출의 재시도·타임아웃·동시성 상한을 명시적으로 관리. 실패 시 안전한 fallback 응답을 내고, 오류는 삼키지 않고 구조화해서 노출

**기술 키워드**
* **Agentic Workflow 설계:** Search / QnA / Reservation / DST 에이전트 개발자와 R&R 조율, 에이전트 간 입출력 스키마와 시나리오별 데이터 흐름 설계
* **Model Training & Serving:** Gemma-4 LoRA SFT/DPO (H100 multi-node), DPO 메모리 최적화(262k vocab log-softmax chunking), KServe + vLLM 서빙, LoRA merge 아티팩트 검증(logit parity)
* **Evaluation & Quality:** 결함 사례를 영구 회귀셋으로 전환, 반례(counterfactual) 쌍 데이터 설계, planner 모듈 테스트 1,600여 개로 계약 검증

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
**2026. 02 - 2026. 04** | `LLM` `Data Generation` `Query Engineering`

> **Agentic Query에 대응되는 Retrieval 데이터셋 구축 및 학습**

**주요 성과**
* **2개 도메인 Agentic Query IR encoder 학습:** 식당, 미용실 도메인용 검색기 학습
* **Agentic Query 분리:** 생성된 예상 질문에서 rewrite된 쿼리들을 대상으로 빈도별/클러스터별/의미상 Dedup 적용하여 학습, 평가셋 쿼리 구축
* **추론 효율성:** 적은 자원으로 효율적인 추론을 하기 위해 MRL(1024dim > 64dim), XTR(top-k tokens retrieval) 도입 
* **Query-Doc pair dataset:** 식당 2M, 미용실 1.5M 수준 positive, hard negative, negative로 구성된 Retrieval 학습용 데이터셋 생성
* **고퀄리티 문서 생성:** 생성 -> 검증 파이프라인을 통한 퀄리티 좋은 positive, hard negative 문서 생성(prompt engineering)

**기술 키워드**
* **Data Generation:** Syntriever 방법론 도입하여 positive, hard negative 문서 생성 및 reranking용 데이터셋 생성
* **Engineering:** dimension reduction, top-k tokens retrieval
* **LLM / Prompt Engineering:** 데이터 생성, pair 검증, 문서 간 ranking 등의 모든 데이터셋 생성 작업에 multi-process LLM generation 작업 

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

### 공시 Q&A 에이전트 — dart-agent (개인 프로젝트, 진행 중)
**2026. 10 -** | `RAG` `MCP` `LangGraph` `OpenSearch` `PostgreSQL` `Neo4j` `Hybrid Search` `AWS`

> **사업보고서·재무 데이터를 대상으로 본문 검색(Vector) · 수치 조회(RDB) · 관계 조회(Graph)를 MCP 도구로 묶어 라우팅하는 금융 공시 Q&A 에이전트. 파싱부터 평가·배포까지 전 구간 직접 구현**

**진행한 것**
* **트랙 분리 설계:** 수집·파싱 어댑터만 소스별(SEC EDGAR 10-K / DART 사업보고서)로 두고 청킹 이후는 공통 파이프라인으로 설계. DART API 점검 기간에 EDGAR 트랙을 먼저 진행
* **수집:** EDGAR 10-K 원문·정정(10-K/A)·XBRL companyfacts·Exhibit 21(종속회사) 수집. 요청 한도·재시도 처리, 원본 불변 + 날짜별 스냅숏으로 재현성 확보
* **문서 파싱:** inline XBRL HTML에서 섹션 계층(16건, 1,380 섹션)과 표를 추출. 병합 셀을 펼쳐 마크다운 표로 변환하고 표 제목·단위·기준일을 붙여 수치 오인 방지
* **재무 지표 추출:** XBRL 태그 우선, 없으면 재무제표 표에서 보완하고 값의 출처(`xbrl`/`table`/`manual`)를 기록
* **청킹 전략 3종:** 고정 길이(512/64) · 섹션 단위 · 섹션 경로 접두어(contextual prefix)를 bge-m3 토크나이저 기준으로 구현, 표는 쪼개지 않도록 제약
* **인덱싱:** bge-m3 임베딩을 OpenSearch에 적재, BM25 · kNN · 하이브리드(min-max 정규화) 검색 구현. 인덱스 교체 원자화로 재적재 시 서비스 중단 방지

**진행 예정**
* **DB 구축:** PostgreSQL(재무 지표, 읽기 전용 계정), Neo4j(종속회사·임원 관계 그래프)
* **MCP 서버:** `search_filings` · `query_financials` · `query_relations` · `get_filing_section` 도구. **LLM이 SQL/Cypher를 직접 쓰지 않고** 고정 쿼리 + 파라미터 바인딩만 허용, 모든 응답에 출처(접수번호·섹션 경로) 첨부
* **에이전트:** LangGraph로 질문 분류·계획 → 도구 실행 → 충분성 판단 → 답변 구조. step 상한·재시도 정책, 투자 권유 답변 차단
* **평가:** 검색은 LLM 없이 Recall@5·MRR@10으로 청킹 3종 / BM25·벡터·하이브리드 / 임베딩 파인튜닝 전후 비교, 답변은 골든셋 80문항 LLM-judge + 수동 검수
* **DART 트랙 합류:** 사업보고서(한국어, `nori` 분석기)
* **배포:** FastAPI + Docker → AWS ECS Fargate, RDS, CloudWatch

----

### Chat2Order (개인 프로젝트) - 현재 운영 종료
**2026. 03 -** | `LLM` `Structured Output` `Railway` `Streamlit` `Gemini API` `Pydantic` `Supabase`

> **소규모 판매자를 위한 메신저 주문 대화를 자동으로 분석해 엑셀 주문서로 변환하는 서비스**

**주요 성과**
* **주문 정보 자동 추출:** 판매자-고객 간 채팅 내역과 상품 카탈로그를 LLM에 입력하여 주문자·상품·수량·연락처 등 핵심 정보를 구조화된 형태로 추출
* **End-to-End 파이프라인 구축:** 채팅 파싱 → Gemini Structured Output(Pydantic 스키마) → 후처리(전화번호 정규화, 도로명 주소 API 우편번호 조회) → DB 저장 → 엑셀 다운로드 전 과정 직접 구현
* **서비스 기능 다양화:** 카탈로그 JSON 자동 생성(재고 CSV 업로드), 최근 5회 추출 이력 조회 및 재다운로드 기능, 주문 대화 중 내용 검색 기능 제공

**기술 키워드**
* **Structured Output:** Pydantic 스키마 기반 LLM 응답 강제 구조화로 파싱 안정성 확보
* **Gemini API:** Google GenAI SDK를 활용한 멀티턴 채팅 분석 및 JSON 모드 응답 처리
* **Backend:** Supabase를 통한 사용자 인증 및 추출 이력 저장, Railway 배포
* **Frontend:** Streamlit으로 업로드·결과 확인·다운로드 통합 UI 
* **1인 개발:** 니즈 파악에서 시작하여 기획, 페이지 설계, 백/프론트 구현, prompt engineering, 유지 보수를 AI와 함께 진행

----

### Fin-Aily (개인 프로젝트)
**2026. 01 -** | `LLM` `AIOps` `GCP Cloud Run` `Pydantic` `Supabase`

> **AI 기반 기관투자자급 보고서와 투자 인사이트를 제공하는 웹서비스 구축 및 서버리스 기반 환경 구성**

**주요 성과**
* **안정적인 AI Ops 파이프라인 구축:** 기존 배포 서비스의 리소스 제약을 극복하기 위해, Google Cloud Run 기반의 서버리스 컨테이너 환경으로 백엔드 아키텍처 마이그레이션 수행
* **풀스택 AI 서비스 런칭**
  * 국내 버전: Dart API와 증권사 리포트(naver finance)를 기반으로 기관 투자자급 리포트 생성: 실적/현금흐름, 리스크 요인, 사업보고서 내용 변동 내역, 체크리스트 추출 등
  * 글로벌 버전: 공시 기반 Deep Research 보고서 제공, 증권 뉴스를 수집하고, LLM을 활용해 인사이트(핵심 요약 및 긍/부정 감성 분석)를 추출하는 end-to-end 서비스 개발
- **비용 및 성능 최적화:** LLM API 사용 비용 절감 및 응답 latency 단축을 위해 캐싱을 설계하고 서버리스 Auto-Scaling 설정으로 과금 효율성을 높임

**기술 키워드**
- **AI Ops:** github-cloud build 연동을 통한 자동화 배포(CI/CD) 파이프라인 구축 및 Cloud logging으로 에러 트래킹/트러블 슈팅 경험
- **AI pipeline 디자인:** 백엔드(GCP Cloud Run)와 LLM API 연동, 프론트엔드(Vercel)를 분리배포하여 워크로드 구성, YAML 기반 기능별 모델 설정 도입하여 각 태스크에서 가장 비용효율적인 LLM 라우팅 유연성 확보

----

# References

- linkedin: https://www.linkedin.com/in/junho-m-60a1831b7/
- paper studies: https://github.com/jonas-jun/AiPapers/issues
- Chat2Order(side project):
    - https://chat2order.up.railway.app/ (화면 확인을 위한 account `testuser / testuser123!`)
    - https://github.com/jonas-jun/chat2order
- Fin-Aily(side project):
    - https://fin-aily-kr.vercel.app/
    - https://fin-aily-us.vercel.app/
- NAVER DAN 25 진행 세션: https://dan.naver.com/25/sessions/691
