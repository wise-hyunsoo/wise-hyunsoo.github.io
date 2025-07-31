# Note 1

- 지도 서비스 관련 프로젝트
- SDK 사용경험 위치기반 기능 개발 경험
- 사우디 지도/POI 데이터 확보 어려움
- 고덕지도 
- 실시간 교통번보 분석
- 다중경로 선택
- 경로 중간변경 감지
- 고정밀 지도 데이터 및 3디 시각화
- 스마트 음성 내비게이션
- 멀티스타일 음성안내/ 대화 인터페이스 / 운전중 조작 최소화
- AI기반 맞춤형 추천
	- 이동 습관 학습 개인화된 알림 상황 인지형 경로 제안
- 생활 밀착형 이동정보 서비스
- 바이두맵 고덕맵
- Raw data -> OSM schema data
- POI converter
- Pelias
- 지도 렌더링 
	- 벡터/래스터 지도 차이
	- 줌 이동 스타일링
	- 고속벡터 렌더링 구조
- POI/주소 검색 (Geocoding)
	- forward/reverse geocoding 구조
	- OSM과의 관계
- 공간데이터 처리 
	- PostGIS 구조 
	- GeoServer 활용법
	- SHP/GDB 파일구조
- 경로탐색
	- Turn-by-Turn 방식
	- OSM기반 루팅
	- GTFS와 대중교통계획
- 3D 앱/실사 지도
	- 3D tiles 이해
	- Cesium Native 호라용
	- 지도 내 실사 타일 구조
- POI 컨버팅 시 고려 포인트
	- 데이터 정규화
	- 줌 레벨 필터링 (레벨별 표시 POI 제한 및 우선순위 체계 구성)
	- 시각적 처리 (중첩 방지, 우선순위 기반 렌더링)
	- 품질 관리 (태그 누락, 오류데이터 필터링)
	- 로컬라이징 (지역별 POI 중요도 차이 반영 필요)
- 대중교통 데이터 원천 -> gtfs 변환프로그램
- polyline, polygon, aoi(area of interest), wps
Example: link to [Mermaid Diagrams](../Features/Mermaid%20Diagrams.md) under `Features`


$$
\begin{align*}
\tilde{\mu} &= \dfrac{\sqrt{\alpha_t}(1-\bar{\alpha}_{t-1})}{1-\bar{\alpha}_t} x_t + \dfrac{\sqrt{\bar{\alpha}_{t-1}}\beta_t}{1-\bar{\alpha}_t}\dfrac{1}{\sqrt{\bar{\alpha}_t}}(x_t-\sqrt{1-\bar{\alpha}_t}\epsilon_t) \\

&= \dfrac{(\alpha_t-\bar{\alpha}_{t})}{\sqrt{\alpha_t}(1-\bar{\alpha}_t)}x_t + \dfrac{\beta_t}{\sqrt{\alpha_t}(1-\bar{\alpha}_t)}(x_t-\sqrt{1-\bar{\alpha}_t}\epsilon_t) \\


&= \dfrac{(\alpha_t-\bar{\alpha}_{t})}{\sqrt{\alpha_t}(1-\bar{\alpha}_t)}x_t + \dfrac{1-\alpha_t}{\sqrt{\alpha_t}(1-\bar{\alpha}_t)}(x_t-\sqrt{1-\bar{\alpha}_t}\epsilon_t) \ (\because \beta_t = 1-\alpha_t)\\

&= \dfrac{1}{\sqrt{\alpha_t}}x_t-\dfrac{1-\alpha_t}{\sqrt{\alpha_t}\sqrt{1-\bar{\alpha}_t}}\epsilon_t \\

&= \dfrac{1}{\sqrt{\alpha_t}}\bigg(x_t-\dfrac{1-\alpha_t}{\sqrt{1-\bar{\alpha}_t}}\epsilon_t\bigg) \\

\end{align*}

$$