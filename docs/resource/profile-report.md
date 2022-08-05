---
layout: default
title: Pandas Profiling
parent: 리소스
nav_order: 2
---

Profile Report는 탐색적 데이터 분석(EDA: Exploratory Data Analysis) 도구로 생성한 리포트에요.

CSV, 엑셀 파일 같은 표 형식 데이터의 EDA 리포트를 제공해요.

## 탐색적 데이터 분석

데이터 활용에 앞서 데이터에 대해 알아보는 과정이 필수라고 할 수 있죠.

탐색적 데이터 분석(EDA: Exploratory Data Analysis)은 데이터의 특징과 구조에 대해 알아보는 것을 말하는데요,
그래프나 통계적인 방법을 사용하면 직관적이고 빠르게 데이터를 이해하는데 도움이 되요.

기본적인 탐색은 항상 필요하고, 언제나 비슷한 작업이니까 EDA 리포트를 자동으로 만들어 주는 솔루션을 이용하면 편해요.

## Pandas Profiling

EDA 리포트를 자동으로 만들어 주는 솔루션이 몇 가지 있는데요, 
[Pandas Profiling](https://pandas-profiling.ydata.ai/docs/master/pages/getting_started/overview.html)은 pandas에서 프로필 리포트를 생성해 주는 오픈소스 패키지에요.

pandas만 가지고도 df.describe()나 head(), tail(), sample() 등으로 매우 기본적인 탐색을 할 수 있겠지만,
Pandas Profiling은 신세계에요. 

![Profile Report 예시1](/images/profile-report-1.png)

변수별 특성을 체계적으로 잘 보여줄 뿐 아니라 변수간의 상관관계를 분석한 그래프를 제공하거든요.

![Profile Report 예시2](/images/profile-report-2.png)

![Profile Report 예시3](/images/profile-report-3.png)

## gimi9의 EDA

기미나인은 CSV나 엑셀 파일의 Pandas Profiling 리포트를 제공하는데요,
버전은 v3.2.0 이며, 한글 깨지는 문제가 없어요.

> 한글 문제는 골치 아픈 문제였는데, [오늘코딩의 유튜브 영상](https://youtu.be/BhZvZpNF9jU)을 참고해서 해결했어요.


### 주의 사항

* 1만건 이상의 큰 파일의 리포트는 1만건 샘플로 만든거에요.
* 20건 이하의 작은 파일은 미리보기만 제공해요. 눈으로 확인할 수 있는 수준이잖아요.
* 경우에 따라 correlations 생략된 리포트도 있어요. 컬럼 수가 많아 리포트 생성 시간이 매우 오래 걸리거나, 서버의 심장에 무리가 갈 것 같거든요. 
