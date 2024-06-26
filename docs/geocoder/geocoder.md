---
layout: default
title: 텍스트 입력 지오코더
parent: 지오코더
nav_order: 2
---


지오코딩 온라인 서비스는 사용자가 주소를 입력하면 해당 주소의 지리적 좌표를 반환하는 서비스입니다. 

말 그대로 주소를 입력하면 지도를 반환합니다.

## 실행 방법

![슬라이드2](https://github.com/gisman/public-data/assets/310358/38e8ef9f-970e-45e2-9f1f-068461a4b171)

1. 웹 인터페이스에서 '주소 찾기' 버튼을 클릭합니다.
2. 나타나는 대화 상자에 찾고자 하는 주소를 입력합니다.
3. '제출' 버튼을 클릭하여 주소를 제출합니다.
4. 제출 후, 시스템은 입력한 주소의 지리적 좌표를 반환하며, 이를 목록과 지도 두 영역에서 확인할 수 있습니다.

## 결과 확인 방법

지오코딩 서비스를 사용하여 주소를 검색한 후, 결과를 확인하는 방법은 다음과 같습니다:

![슬라이드3](https://github.com/gisman/public-data/assets/310358/0a24e049-c560-4a8f-bd80-8b30b953e772)

1. 검색 결과 목록에서 원하는 주소를 클릭합니다. 클릭하면 지도에 해당 위치가 표시되며, 해당 위치의 속성 정보가 표시됩니다.
1. 지도에 표시된 아이콘을 클릭합니다. 해당 주소의 속성 정보가 표시됩니다.

## 파일 다운로드 방법

지오코딩 서비스에서 검색한 결과를 파일로 다운로드하는 방법은 다음과 같습니다:

![슬라이드4](https://github.com/gisman/public-data/assets/310358/22f085aa-85b2-4e0d-b661-41958411b987)


1. '다운로드' 버튼을 클릭합니다.
2. 나타나는 메뉴에서 원하는 파일 포맷을 선택합니다. 선택할 수 있는 파일 포맷은 다음과 같습니다:
    * CSV
    * JSON
    * GEOJSON
    * KML

### Python 코드 예시

``` python
# 필요한 라이브러리를 불러옵니다.
import geopandas as gpd

# GEOJSON 파일의 경로를 지정합니다.
file_path = 'path_to_your_file.geojson'

# geopandas의 read_file 함수를 이용하여 GEOJSON 파일을 불러옵니다.
gdf = gpd.read_file(file_path)

# 데이터프레임을 출력하여 확인합니다.
print(gdf)
```
