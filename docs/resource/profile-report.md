---
layout: default
title: Pandas Profiling
parent: 리소스
nav_order: 2
---

Profile Report는 탐색적 데이터 분석(EDA: Exploratory Data Analysis) 도구로 생성한 리포트입니다.

CSV, 엑셀 파일 등의 표 형식 데이터의 EDA 리포트를 제공합니다.

## 탐색적 데이터 분석

데이터 활용에 앞서 데이터에 대한 이해는 필수적입니다.

탐색적 데이터 분석(EDA: Exploratory Data Analysis)은 데이터의 특징과 구조를 파악하는 과정으로, 그래프나 통계적인 방법을 사용하여 데이터를 직관적이고 빠르게 이해하는데 도움이 됩니다.

기본적인 탐색은 항상 필요하며, 이를 위해 EDA 리포트를 자동으로 생성하는 솔루션을 사용하면 효율적입니다.

## Pandas Profiling

EDA 리포트를 자동으로 생성하는 솔루션 중 하나인 [Pandas Profiling](https://pandas-profiling.ydata.ai/docs/master/pages/getting_started/overview.html)은 pandas에서 프로필 리포트를 생성해 주는 오픈소스 패키지입니다.

pandas만을 사용하여 df.describe(), head(), tail(), sample() 등의 기본적인 탐색을 수행할 수 있지만, Pandas Profiling은 보다 체계적인 탐색을 가능하게 합니다. 

![Profile Report 예시1](/public-data/images/profile-report-1.png)

변수별 특성을 체계적으로 보여주며, 변수간의 상관관계를 분석한 그래프를 제공합니다.

![Profile Report 예시2](/public-data/images/profile-report-2.png)

![Profile Report 예시3](/public-data/images/profile-report-3.png)

## gimi9의 EDA

기미나인은 CSV나 엑셀 파일의 Pandas Profiling 리포트를 제공합니다. 사용하는 버전은 v3.2.0이며, 한글 깨짐 문제가 없습니다.

> 한글 문제는 [오늘코딩의 유튜브 영상](https://youtu.be/BhZvZpNF9jU)을 참고하여 해결하였습니다.

### 주의 사항

* 1만건 이상의 큰 파일의 리포트는 1만건 샘플로 생성됩니다.
* 20건 이하의 작은 파일은 미리보기만 제공됩니다.