## 데이터 분석을 위한 SQL 레시피, 가사키 나가토 외 1명 지음
### 2019.05.14 ~ 2019.07.14

## note
**SQL 문장 실행 순서**
1. FROM : 조회 테이블 확인
2. WHERE : 데이터 추출 조건 확인
3. GROUP BY : 그룹화
4. HAVING : 그룹화 중 조건에 맞는 데이터만
5. SELECT : 명시된 데이터만 화면에 출력
6. ORDER BY : 순서대로 정렬

**SQL표준 함수 분류**
1. (GROUP) AGGREGATE FUNCTION : 집계 함수
		+ count, sum, avg, max, min 등
2. GROUP FUNCTION : 결산 개념의 업무, 소계, 중계, 합계, 총 합계 등 보고서를 만드는 기능
		+ ROLLUP, CUBE, GROUPING SETS 등
3. WINDOW FUNCTION : 행과 행간의 관계를 쉽게 정의하기 위해 만든 함수(OVER 구문 필수)
		+ 순위(RANK), 집계(AGGREGATE), 행 순서(LAG, FIRST_VALUE), 비율, 통계 분석 관련 함수 등.

**윈도 프레임 지정**
- ROWS BETWEEN start AND end
- start, end : CURRENT ROW, n PRECEDING, n FOLLOWING, UNBOUNDED PRECEDING, UNBOUNDED FOLLOWING
- 프레임 지정 하지 않을 경우, 디폴트 프레임은
	+ ORDER BY 없는 경우 : 모든 행
	+ ORDER BY 있는 경우 : 첫 행 ~ 현재 행
