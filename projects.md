# 🚀 Project Experience

---

### 🔍 Place IR Encoder 고도화
`Information Retrieval` `DPO` `Large-scale Training` | **2025. 01 - Present**

> **"네이버 플레이스 AI 브리핑의 검색 품질 최적화 및 In-domain Recall 62% 향상"**

**주요 성과**
* **검색 성능 혁신:** In-domain Recall을 **0.39에서 0.65로 대폭 개선**하여 AI 브리핑 내 콘텐츠 다양성 확보
* **기술 영향력:** **NAVER DAN 25** 컨퍼런스 직접 발표 및 기술 공유

**Technical Deep-dive**
* **Retrieval Efficiency:** **MRL(Matryoshka Representation Learning)** 및 **XTR** 도입으로 임베딩 차원 및 검색 효율 최적화
* **Reinforced IR:** Query Generation 및 **DPO(Direct Preference Optimization)** Training을 IR 모델에 적용하여 사용자 선호도 기반 랭킹 고도화
* **Scalable Training:** **Ray Train**과 PyTorch Lightning 기반의 Multi-node Cluster 학습 파이프라인 구축 및 **FlagEmbedding(BGE-m3)** 방식 최적화

---

### 📝 플레이스 속성 기반 리뷰 분석 (ABSA)
`NLP` `SFT` `Efficiency` | **2025. 12 - Present**

> **"3B 소형 모델로 GPT-5급 성능 달성, 195가지 속성에 대한 정밀 감성 분석"**

**주요 성과**
* **High Efficiency:** 3B 규모의 모델로 **GPT-5.2와 대등한 Accuracy(0.79 수준)** 확보 중
* **Insight Extraction:** 플레이스 리뷰 내 195가지 속성(Aspect)에 대한 감성 및 연관 구절 자동 도출

**Technical Deep-dive**
* **Model Tuning:** **HyperCLOVA X** 기반의 Decoder Instruction SFT 및 Special Token(`|aspect|`, `|positive|` 등) 도입으로 학습/추론 효율 극대화
* **Inference Optimization:** Full generation 방식과 단일 토큰 Logit 확률값 활용 방식 비교 실험을 통한 서빙 비용 최적화
* **Data Engineering:** 속성/감성 밸런스를 고려한 데이터셋 설계 및 Incorrect case 분석을 통한 SFT 프롬프트 반복 개선

---

### 🖼️ 플레이스 멀티모달 이미지 검색기
`Multi-modal` `CLIP` `Continual Learning` | **2024. 09 - Present**

> **"검색 의도에 최적화된 이미지 노출 시스템 구축 및 NAVER DAN 24 세션 포함"**

**주요 성과**
* **노출 품질 개선:** 여행지/식당 검색 시 쿼리 의도(풍경 vs 접사)에 맞는 최적의 이미지 우선 노출
* **Global Expansion:** 해외 플레이스 검색 대응을 위한 쿼리 확장 및 이미지 매칭 품질 고도화

**Technical Deep-dive**
* **2-Stage Training:** Embedding Space Align(Stage 1) 후 Service-specific High Quality Retrieval 학습(Stage 2) 전략 수립
* **Intent-aware Search:** 카테고리별 Special Token 도입으로 텍스트 검색 시 이미지 랭킹 정교화
* **Catastrophic Forgetting 방지:** **DAS 기법**을 적용하여 신규 데이터 학습 시에도 기존 성능을 유지하는 Continual Learning 구현

---

### 🏷️ 플레이스 이미지 통합 분류 모델 (VLM)
`VLM` `Computer Vision` `Dataset Distillation` | **2023 - 2024**

> **"분산된 이미지 분류 태스크를 하나의 Encoder/Decoder 모델로 통합 및 효율화"**

**주요 성과**
* **Pipeline Unification:** 흩어져 있던 수십 개의 모델을 하나의 통합 모델로 교체하여 관리 및 서빙 효율 극대화
* **Advanced Tagging:** 음식 종류(400+ 클래스), 분위기, 영수증, 퀄리티 점수 등 정밀 태깅 구현

**Technical Deep-dive**
* **VLM Exploration:** LLaVA, mPLUG-Owl 등 **VLM 기반의 Instruction SFT**를 통한 다중 태스크 처리
* **Smart Sampling:** Embedding Clustering 후 Centroid 기반 샘플링을 통해 데이터셋 경량화 및 다양성 확보
* **Verification:** 소량의 Golden set 기반 Retrieval과 YOLO Object Detection을 결합한 하이브리드 검증 프로세스 구축

---

### 📊 플레이스 이미지 품질 점수 측정기
`Knowledge Distillation` `MLOps` | **2022**

> **"모델 경량화를 통한 리소스 절감 사례 PyTorch 공식 블로그 기재"**

**주요 성과**
* **Infrastructure Optimization:** 모델 사이즈 **94% 축소 (170M → 10M)** 및 추론 리소스 대폭 절감
* **Model Performance:** 0-5점 척도 Regression 기준 **MAE 0.3 후반**, Classification 기준 **Acc. 99%** 달성

**Technical Deep-dive**
* **Knowledge Distillation:** CLIP Vision Encoder의 지식을 EfficientNet으로 증류하여 모바일/에지 환경 최적화
* **Collaboration:** Intel과의 협업을 통해 [PyTorch 공식 블로그](https://pytorch.org/blog/ml-model-server-resource-saving/)에 리소스 절감 사례 기재
* **Internal Tools:** Streamlit 기반 데모 페이지를 구축하여 기획자 및 유관 부서의 즉각적인 모델 검증 지원