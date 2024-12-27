---
title: "[Paper Review] Real-IAD: A Real-World Multi-View Dataset for Benchmarking Versatile Industrial Anomaly Detection"
date: 2024-12-27
categories: [Paper Review]
tags: [markdown, yaml, typora]
layout: post
typora-copy-images-to: ../../../sosum22.github.io/_media
---

[toc]

# [Paper Review] Real-IAD: A Real-World Multi-View Dataset for Benchmarking Versatile Industrial Anomaly Detection



## Abstract

**Industrial anomaly detection (IAD)**

<span style="color: red;">dataset limitations</span>으로 연구에 어려움이 존재

- MVTec과 같은 mainstream dataset은 많은 방법론이 AUROC 99% 이상을 찍는 성능 포화 발생
  - → 방법의 차이를 구분하기 어려워 public datasets과 actual application scenarios 간의 격차가 초래됨
- Various new practical anomaly detection settings은 scale of dataset에 제한 존재



**Real-IAD**

- Large-scale, real-word, and multi-view Industrial Anomaly Detection dataset
- 150K high-resolution images of 30 different objects
- larger range of defect area and ratio proportions
- multi-view shooting method
- proposed sample-level evaluation metrics
- propose a new setting for Fully Unsupervised Industrial Anomaly Detection (FUIAD)
  - yield rate in industrial production이 일반적으로 60% 이상이므로 위와 같은 새로운 설정 제안 → more practical application value





## 1. Introduction

**High-quality datasets**

- pivotal role in the development of computer vision technology
- technical research와 practical application 간의 격차를 해소
- Ex) ImageNet, MVTec AD



**Industrial production**

- Product quality inspection은 매우 중요함
- 초기 적용은 주로 supervised learning (detection, segmentation)
  - Precise defect location anotation이 필수적임
  - training set에 거의 없거나 새로운 defect에 대해서는 성능이 크게 감소함

- MVTec AD, VisA와 같은 대규모 dataset의 등장으로 **unsupervised learning**이 발전하게 됨
  - training set: anomaly-free
  - defect locations and pixel areas를 탐지
  - manual annotation costs를 줄임
  - unknown defects까지 탐지 가능



**MVTec AD, VisA와 같은 datasets의 Limitation**

- MVTec AD의 defect range는 작고, actual application scenarios는 간단함
- 최근 방법론들은 I-AUROC, P-AUROC에서 99%를 초과함
  - 새로운 방법론의 장점을 구별하기 어려워짐
- object diversity가 부족함
  - MVTec AD: 15 types, VisA: 12 types
- single-view images로 구성됨
  - 실제 산업 현장의 데이터는 부품 구조가 복잡함
  - single-view는 모든 defect를 표현할 수 없음
  - MVTec AD-3D와 같은 dataset가 있지만 3D 센서는 높은 비용으로 실제 적용에 제한적임



**IAD의 Limitation**

- unsupervised learning이지만, training set에는 manual annotation이 필요함
  - *Soft Patch*에서 제안된 BTAD training set에 nosie sample을 추가하는 방식 등을 도입할 수 있음



**Real-IAD** (**Real**-world **I**ndustrial **A**nomaly **D**etection dataset)

- object categories, images 수의 측면에서 기존 dataset을 능가함
- 2D IAD task에서 처음으로 multi-view를 제공함
- Fully Unsupervised IAD 제안
  - 대부분의 production line의 수율이 60%이므로, 0~40%의 anomaly images를 anomaly-free train set에 추가하여 학습



> Contribution

- 기존의 mainstream datasets보다 10배 이상 큰 새로운 Real-IAD dataset 제안
  - The number of objects: 30 classes
  - The number of shooting angles: 5
  - 150K high-resolution
- Fully Unsupervised IAD setting
  - NO manual annotation
- Real-IAD dataset에서 성능이 좋은 IAD methods 탐색





## 2. Related Work

### 2.1. Anomaly Detection

**MVTec AD**

- 15 industrial products in 2 types
- total 5,354 images

**VisA**

- 12 objects in 3 types
- total 10,821 images



**좋은 퀄리티의 dataset을 얻기 위한 다양한 시도들**

- synthetic PAD
  - Pose-agnostic anomaly detection에 대한 연구
  - synthetic data와 real samples 사이에는 격차가 있어 평가가 어려움
- MVTec AD-3D, Eyecandies, Real3D
  - defect를 더 잘 탐지하기 위해 3D information 사용



- 그럼에도 여전히 scale, category가 작음
- classification field의 ImageNet-1K 정도의 dataset은 여전히 부재



### 2.2. Standard Anomaly Detection

**The standard IAD task**

<span style="color: red;">Aims</span>: 1) 주어진 이미지가 anomalous or not. 2) 주어진 이미지가 anomalous라면, localize the anomaly region.

Anomalous data가 부족하기 때문에, unsupervised learning이 주로 사용됨

- Data augmentation-based methods, reconstruction-based methods, embedding-based methods (memory bank, normalizing flow, knowledge distillation, classification)



### 2.3. Other Settings in Anomaly Detection

**Zero-/few-shot IAD**

- IAD에 대한 few normal samples을 사용하여 데이터의 수요를 줄임

**SoftPatch**

- anomaly-free training data에 noise data (less than 10%)를 추가

**Semi-supervised IAD**

- Training 과정에 anomalous data를 사용

**Unified IAD**

- accomplished IAD for multiple classes by a unified framework





## 3. Real-IAD Dataset Description

### 3.1. Data Collection and Construction Manner

<div style="text-align: center;">
    <img src="/Users/soyulim/sosum22.github.io/_media/스크린샷 2024-12-26 오후 5.05.26.png" alt="이미지 설명" style="width: 800px; height: auto;">
    <div style="display: inline-block; text-align: left; margin-top: 10px; max-width: 800px; font-size: 14px;">
    </div>
</div>



procedure includes *three parts*:

1) Material Preparation.

2) Prototype Construction.
3) Data Collection, Annotation, and Cleaning.



1) **Material Preparation.** (Fig. 1-(a))

30 objects

- Metal, plastic, wood, ceramics, mixed materials

Fig. 1-(a).

- manually created various types of defects
  - missing parts, dirt, deformation, pits, damage, holes, cracks, scratches



2. **Prototype Construction.** (Fig. 1-(b))

acquisition equipment

- five cameras capturing the obejct from different angles
- 1대: top shots/ 4대: 45도의 symmetric angles



3. **Data Collection, Annotation, and Cleaning.** (Fig. 1-(c))

​	1\) normal images, anomaly images를 수동으로 확인

​	2\) LabelME를 사용하여 pixel-level masks를 수동으로 label 지정

​	3\) 데이터를 세 그룹으로 나눈 후, 각 그룹은 RCNN을 사용하여 supervised trained 진행

​	4\) 모델의 예측 결과와 수동 annoation 간의 불일치를 찾음

​	5\) 수정이 필요한 annotation이 있는 이미지의 수가 특정 threshold보다 적을 때까지 수정



### 3.2. Comparison with Popular 2D Datasets

<div style="text-align: center;">
    <img src="/Users/soyulim/sosum22.github.io/_media/스크린샷 2024-12-26 오후 5.04.47.png" alt="이미지 설명" style="width: 800px; height: auto;">
    <div style="display: inline-block; text-align: left; margin-top: 10px; max-width: 800px; font-size: 14px;">
    </div>
</div>

<div style="text-align: center;">
    <img src="/Users/soyulim/sosum22.github.io/_media/스크린샷 2024-12-26 오후 5.14.24.png" alt="이미지 설명" style="width: 800px; height: auto;">
    <div style="display: inline-block; text-align: left; margin-top: 10px; max-width: 800px; font-size: 14px;">
    </div>
</div>

<div style="text-align: center;">
    <img src="/Users/soyulim/sosum22.github.io/_media/스크린샷 2024-12-26 오후 5.15.11.png" alt="이미지 설명" style="width: 400px; height: auto;">
    <div style="display: inline-block; text-align: left; margin-top: 10px; max-width: 400px; font-size: 14px;">
    </div>
</div>



**Comparison with Mainstream Datasets.**

- Real-IAD는 mainstream dataset에 비해 class 수는 2배 이상, Image 수도 150K로 증가함 (Tab. 1)

- 각 object는 multi-view setting에서 segmentation labeling이 달림 (Fig. 3)



<div style="text-align: center;">
    <img src="/Users/soyulim/sosum22.github.io/_media/스크린샷 2024-12-26 오후 5.21.22.png" alt="이미지 설명" style="width: 800px; height: auto;">
    <div style="display: inline-block; text-align: left; margin-top: 10px; max-width: 800px; font-size: 14px;">
    </div>
</div>


**Data Statistics.**

- Normal, abnormal data 모두 mainstream dataset보다 많은 양 수집 (Fig. 2-a)
- proportion of defect area, range of defect ratio가 모두 커짐 (Fig. 2-b, Fig. 2-c)

*\* proportion of defect area vs range of defect ratio ?*

|    | proportion of defect area | range of defect ratio |
| ---- | ---------------------- | ------------------ |
| 정의 | 전체 이미지에서 defect의 비율 |defect와 정상 영역의 비율|
|측정|절대적인 크기 (0~1)|상대적인 비율|
|예시|(20px/100px) → 0.2|(20px/100px) → 0.25|



**Advantage Analyses.**

1\) Diversity

- wider range of categories
- provides richer scenarios

→ helps to train more <span style="color: blue;">robust</span> anomaly detection models and conduct <span style="color: blue;">fair evaluations</span>.

2\) Large-scale

- 150K images
- multi-view images with pixel-level annotations

3\) Challenge

- 기존 dataset과 비교하여, Real-IAD는 higher level of difficulty를 가짐



### 3.3. Real-IAD Visualization

- 30 sample data types (Fig. 1)
  - 다양한 material types (such as metal, plastic, wood, ceramics, and mixed material.)
  - 다양한 defects (such as pit, deformation, abrasion, scratches, damage, missing parts, foreign objects, and contamination.)

- The defect area: 0.1 to 0.5

- A ratio range: 0.1 to 10.0





## 4. Benchmark

### 4.1. Evalution settings

**Unsupervised IAD (UIAD)**

- normal samples만 사용하여 학습
- <span style="color: blue;">training set</span>: only normal samples
- <span style="color: blue;">test set</span>: normal samples + anomalous samples



**Fully Unsupervised IAD (FUIAD)**

- MVTec AD, VisA 등과 같은 dataset에서는 anomalous sample의 수가 적기 때문에 FUIAD setting이 어려워 거의 연구되지 않음
- Real-IAD는 FUIAD를 고려하여 수집된 첫 dataset
  - large, diverse samples of anomalies 덕분에 FUIAD setting을 구성할 수 있음



**구성 방식**

- testing set
  - normal, anomaly samples은 모두 100 samples (500 images)
  - 남아있는 normal, anomaly samples은 noisy training set의 candidate set
- training set
  - total number of samples은 고정
  - normal, abnormals 수는 noise ratio에 따라 adaptively하게 조정됨



- benchmarks 생성 과정

1. candidate set의 규모와 noise ratio의 범위에 따라 training sample의 수를 추론

2. 특정 noise ratio를 가진 training set을 구성하기 위해 candidate set에서 normal, abnormal sample을 randomly sampling
3. 이런 방식으로, 서로 다른 noise ratio ($\alpha \in [0, 1]$) 를 가진 몇가지 새로운 fully unsupervised benchmarks를 생성함

<span style="color: gray;">Ex) candidate set: 2000, training set: 1000, $\alpha$ : 0.4</span>

<span style="color: gray;">→ anomaly sample = 1000 $\times$ 0.4 $=$ 400</span>

<span style="color: gray;">→ normal sample = 1000 $-$ 400 $=$ 600</span>



### 4.2. Evaluation Metric

**AUROC**

- Image-level and pixel-level anomaly detection에 가장 널리 쓰이는 metric

**AUPRO** (**A**rea **U**nder **P**er-**R**egion **O**verlap)

- pixel-level anomaly detection metric



Real-IAD에서는 multiple views의 결과를 통합하여 평가

→ sample-level performance를 평가하기 위함





## 5. Comparisons with IAD Benchmarks

### 5.1. Results on Unsupervised IAD

**Dataset**

- MVTec, VisA, Real-IAD (single view: top view)

**Baseline**

- PatchCore, PaDim, CFlow, data-augmentation-based methods, SimpleNet, DeSTSeg, reconstruction-bsaed RD, UniAD

**Hyperparameter**

- Resize: 256 $\times$ 256
- center crop 224 $\times$ 224 (only PatchCore, PaDim)
- batch size, learning rate 등은 각 method의 official implementation과 동일



**Evaluation analysis**

1\) Tab. 2를 보면 MVTec (I-AUROC의 경우 97.9%)에서 single-/multi- view Real-IAD (I-AUROC의 경우 85%)로 상당한 성능 저하가 있음. 

→ 이는 Real-IAD가 기존 dataset보다 더 어려우며 더 복잡한 데이터 분포를 고려할 때 합리적.

2\) 기존 문제로, 기존 dataset에 대한 AUROC 값이 모두 높기 때문에 다른 방법을 평가하기가 어려움. (특히 MVTec에서, 대부분의 방법은 I-AUROC에서 약 98%-99%를 달성.) 

→ 반대로, Real-IAD dataset에서 대부분의 방법은 약 90%의 I-AUROC만 얻을 수 있으며, 이는 이상 감지 알고리즘의 효과를 평가하기에 더 적합

3\) Real-IAD의 pixel-level P-PRO는 기존 VisA와는 비슷하지만 MVTec AD보다 현저하게 낮음. 

→ Real-IAD dataset이 pixel-level P-PRO에서도 어려움을 보임



### 5.2. IAD Benchmarks Meet FUIAD

<div style="text-align: center;">
    <img src="/Users/soyulim/sosum22.github.io/_media/스크린샷 2024-12-26 오후 7.42.44.png" alt="이미지 설명" style="width: 400px; height: auto;">
    <div style="display: inline-block; text-align: left; margin-top: 10px; max-width: 400px; font-size: 14px;">
    </div>
</div>

**FUIAD** (**F**ully **U**nsupervised **I**ndustrial **A**nomaly **D**etection)

- 실제 적용에서 모든 training sample이 normal을 보장할 수는 없음
- 실제 생산 라인에서 수율은 60%
- Training set에 anomalous sample을 추가하는 방식으로 실제 dataset과 유사하게 구성함



**FUIAD 실험 setting 구성**

- 기존: test set에서 random으로 anomalous sample 일부를 sampling하고 training set에 noise sample로 추가
  - 문제점: test sample의 수를 크게 감소시키게 됨. 이상 감지 알고리즘의 성능을 평가하기에 test sample 수가 부족
- 제안 방법론: $\alpha$ 와 #TS를 조정하여 # training images를 구성
  - $\alpha$: noise ratio (anomalous sample ratio), #TS: anomalous sample의 개수



**Evaluation analysis** (Tab. 3)

- 기존 mainstream dataset (MVTec AD, ViSA)
  - $\alpha$나 #TS가 커지면 유효한 test category가 감소하여 FUIAD 수행이 어려워짐
- Real-IAD
  - $\alpha$나 #TS가 커져도 유효한 test category가 감소하지 않으며 FUIAD 수행이 가능함



## 6. Comparisons with Fully Unsupervised IAD

<div style="text-align: center;">
    <img src="/Users/soyulim/sosum22.github.io/_media/스크린샷 2024-12-27 오후 1.32.38.png" alt="이미지 설명" style="width: 800px; height: auto;">
    <div style="display: inline-block; text-align: left; margin-top: 10px; max-width: 800px; font-size: 14px;">
    </div>
</div>

FUIAD: $\alpha \in \{0.1, 0.2, 0.4\}$

UIAD: $\alpha = 0$

\* 다른 dataset에 대해 FUIAD setting을 할 수 없기 때문에 (Sec. 5.2.) Real-IAD로만 실험 진행



**Evaluation analysis**

- UIAD
  - I-AUROC는 모든 방법론의 성능이 거의 유사함
- FUIAD
  - 대부분의 방법론은 모든 metric에서 큰 성능 저하 발생
  - PatchCore는 patch level의 memory bank를 사용하여 robust
  - SoftPatch는 noise를 필터링한 후 memory bank를 사용하여 robust



→ FUIAD는 UIAD와 비교하여 성능이 안 좋음. noise에 robust할 수 있도록 model ensemble이나 VLM 등의 연구가 필요





## 7. Conclusion

**Motivation**

1. UIAD 방법론은 포화 상태이지만, 실제 산업에 적용되는 것은 여전히 어려움

2. 대부분의 알고리즘이 pure한 normal training sample만 사용하지만, 실제 데이터는 noise를 포함하며, 이는 UIAD와 실제 응용 사이에 격차를 유발함

3. FUIAD는 실제 적용에 더 적합하지만, 기존 mainstream dataset은 제한된 sample로 인해 연구에 사용되기 부적절



**Proposed method**

- Real-IAD 제안
  -  150K high-resolution images
  - 30 objects of metal, plastic, wood, ceramics, and mixed materials
  - Each obejct contains 5 different views with 8 common defects
- UIAD, FUIAD 두 가지 setting 모두 설계하여 성능 평가 진행



**Limitation and Future Works**

- Real-IAD를 사용하여 다양한 연구 진행
  - zero-shot, few-shot, semi-supervised setting
  - Large-scale, multi-view characteristics를 고려할 수 있는 알고리즘 연구
