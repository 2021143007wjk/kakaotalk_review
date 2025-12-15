# KoElectra를 활용한 카카오톡 버전별 리뷰 감성 및 이슈 분석

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0%2B-orange)
![HuggingFace](https://img.shields.io/badge/Transformers-4.0%2B-yellow)
![KoElectra](https://img.shields.io/badge/Model-KoElectra--Small-green)

## 프로젝트 개요

국민 메신저 **카카오톡(KakaoTalk)**의 특정 업데이트(v25.8.0 등) 전후로 발생하는 사용자 반응의 변화를 심층 분석한 프로젝트이다.
한국어 특화 언어 모델인 **KoElectra**를 Fine-tuning하여 리뷰 텍스트에 담긴 **감성(Sentiment)**과 **구체적인 불만 원인(Pain Points)**을 정량적으로 도출하였다.<br>
최근 카카오톡의 업데이트에 어떤 의견들을 가지고 있는지를 분석하는 것을 목표로 한다.

### 분석 목표
1.  **감성 분석:** 업데이트 전/후, 논란이 된 버전의 긍정/부정 비율 변화 추적
2.  **이슈 탐지:** 부정 리뷰 내 핵심 키워드를 추출하여 구체적인 불만 원인 파악
3.  **문맥 파악:** 주요 키워드 뒤에 따르는 서술어를 분석하여 사용자 경험(UX)의 디테일한 문제점 도출

---

## 사용 기술 및 데이터

### 1. 데이터셋 (Dataset)
* **출처:** Google Play Store 리뷰 크롤링<br>
 google_play_scraper 라이브러리를 사용하여 크롤링 하였다.
* **대상 버전:**
    * `v25.7.3` (업데이트 전)
    * `v25.8.0` (이슈 발생 버전)
    * `v25.9.0` (최신/수정 버전)
* **데이터 규모:** 약 35000건의 Raw Data
* **전처리:** * 2000건의 샘플 데이터에 대해 **3-Class 라벨링 (긍정/중립/부정)** 수행
    * **클래스 불균형(Class Imbalance)** 해결을 위한 **오버샘플링(Oversampling)** 기법 적용

### 2. 모델링 (Modeling)
* **Base Model:** `monologg/koelectra-small-v3-discriminator`
* **Fine-tuning:**
    * Epochs: 4
    * Optimizer: AdamW (Learning Rate: 5e-5)
    * Loss: CrossEntropyLoss
* **Performance:**
    * **Validation Accuracy: 94.39%**

---

## 📊 분석 결과 (Analysis Results)

### 1. 버전별 감성 비율 변화
![감성분석](감성분석.png)

* **v25.7.3:** 부정 리뷰 비율이 높음. 업데이트를 일부러 안하는 사람들도 있기에 이전 버전에도 불만이 많다.
* **v25.8.0:** 부정 리뷰 비율이 높음. 업데이트에 대한 불만이 많다.
* **v25.9.0:** 부정 리뷰 비율이 높음. 여전히 부정 비율이 높지만, 긍정과 중립 비율이 증가하였다.

### 2. 주요 불만 키워드 (Word Extraction)
![주요키워드](주요키워드.png)

### 3. 키워드 문맥 분석 (Context Analysis)
> *[여기에 생성된 '키워드 후행 단어' 이미지를 캡처해서 넣으세요]*

* **'광고' + [서술어]:** "가리다", "크다" (화면 방해) vs "많다", "뜨다" (빈도수 불만)
* **'사진' + [서술어]:** "안보내지다", "깨지다" (전송 오류 및 화질 저하 지속)

---

## 🚀 실행 방법 (How to Run)

### 1. 환경 설정
```bash
# 필수 라이브러리 설치
pip install torch transformers pandas scikit-learn matplotlib seaborn konlpy tqdm
