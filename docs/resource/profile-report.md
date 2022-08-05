---
layout: default
title: Buttons
parent: 리소스
nav_order: 2
---

## 탐색적 데이터 분석

데이터 활용에 앞서 데이터에 대해 알아보는 과정이 필요하다.

탐색적 데이터 분석(EDA: Exploratory Data Analysis)은 데이터의 특징과 구조에 대해 알아보는 행위를 뜻한다.

## Pandas Profiling

EDA 리포트를 자동으로 만들엉 주는 솔루션이 몇 가지 있다.

[Pandas Profiling](https://pandas-profiling.ydata.ai/docs/master/pages/getting_started/overview.html)은 pandas에서 프로필 리포트를 생성해 준다. df.describe() 탐색은 매우 기본적인 정보만 제공하는데 반해, 데이터 이해를 위한 변수별 특성 및 변수간의 상관관계 보고서를 제공한다. 

## gimi9의 EDA

기미나인은 CSV나 엑셀 파일의 Pandas Profiling 리포트를 제공한다.

pandas-profiling의 버전은 v3.2.0 이며, 한글이 깨지는 문제는 없다.
[오늘코딩의 유튜브 영상](https://youtu.be/BhZvZpNF9jU)을 참고하였는데 정말 감사드린다.

![Profile Report 예시1](images/profile-report-1.png)
![Profile Report 예시2](images/profile-report-2.png)
![Profile Report 예시3](images/profile-report-3.png)

### 제약 사항

* 1만건 이상의 큰 파일은 1만건 샘플을 추출하여 리포트를 제공한다.
* 20건 이하의 작은 파일은 눈으로 확인할 수 있는 수준이므로 미리보기만 제공한다.
* 경우에 따라 correlations 생략된 리포트도 있다. 컬럼 수가 많아 리포트 생성 시간이 매우 오래 걸리는 데이터인 경우. 


